Playwright можно использовать для получения доступа к REST API вашего приложения.

Иногда вам может понадобиться отправлять запросы на сервер напрямую из Python, не загружая страницу и не запуская на ней JS-код. Вот несколько примеров, когда это может пригодиться:

* Тестирование серверного API.
* Подготовка серверного состояния перед тестированием веб-приложения.
* Проверка серверных пост-условий после выполнения некоторых действий в браузере.

Всего этого можно добиться с помощью методов `APIRequestContext`.

В следующих примерах используется пакет `pytest-playwright`, который добавляет фикстуры Playwright в средство запуска тестов Pytest.

---

### Написание тестов API

`APIRequestContext` может отправлять по сети все виды HTTP(S)-запросов.

В следующем примере показано, как использовать Playwright для тестирования создания задач с помощью API GitHub. Набор тестов выполняет следующие действия:

* Создает новый репозиторий перед запуском тестов.
* Создайте несколько задач и проверьте состояние сервера.
* После выполнения тестов удалите репозиторий.

#### Настройка

Для API GitHub требуется авторизация, поэтому мы настроим токен один раз для всех тестов. Кроме того, мы установим `baseURL` для упрощения тестов.

Долгожданный пример кода:

```python
import os
from typing import Generator

import pytest
from playwright.sync_api import Playwright, APIRequestContext

GITHUB_API_TOKEN = os.getenv("GITHUB_API_TOKEN")
assert GITHUB_API_TOKEN, "GITHUB_API_TOKEN is not set"


@pytest.fixture(scope="session")
def api_request_context(
    playwright: Playwright,
) -> Generator[APIRequestContext, None, None]:
    headers = {
        # We set this header per GitHub guidelines.
        "Accept": "application/vnd.github.v3+json",
        # Add authorization token to all requests.
        # Assuming personal access token available in the environment.
        "Authorization": f"token {GITHUB_API_TOKEN}",
    }
    request_context = playwright.request.new_context(
        base_url="https://api.github.com", extra_http_headers=headers
    )
    yield request_context
    request_context.dispose()
```

---

### Написание Тестов

Теперь, когда мы инициализировали объект запроса, мы можем добавить несколько тестов, которые будут создавать новые задачи в репозитории.

Пример кода:

```python
import os
from typing import Generator

import pytest
from playwright.sync_api import Playwright, APIRequestContext

GITHUB_API_TOKEN = os.getenv("GITHUB_API_TOKEN")
assert GITHUB_API_TOKEN, "GITHUB_API_TOKEN is not set"

GITHUB_USER = os.getenv("GITHUB_USER")
assert GITHUB_USER, "GITHUB_USER is not set"

GITHUB_REPO = "test"

# ...

def test_should_create_bug_report(api_request_context: APIRequestContext) -> None:
    data = {
        "title": "[Bug] report 1",
        "body": "Bug description",
    }
    new_issue = api_request_context.post(f"/repos/{GITHUB_USER}/{GITHUB_REPO}/issues", data=data)
    assert new_issue.ok

    issues = api_request_context.get(f"/repos/{GITHUB_USER}/{GITHUB_REPO}/issues")
    assert issues.ok
    issues_response = issues.json()
    issue = list(filter(lambda issue: issue["title"] == "[Bug] report 1", issues_response))[0]
    assert issue
    assert issue["body"] == "Bug description"

def test_should_create_feature_request(api_request_context: APIRequestContext) -> None:
    data = {
        "title": "[Feature] request 1",
        "body": "Feature description",
    }
    new_issue = api_request_context.post(f"/repos/{GITHUB_USER}/{GITHUB_REPO}/issues", data=data)
    assert new_issue.ok

    issues = api_request_context.get(f"/repos/{GITHUB_USER}/{GITHUB_REPO}/issues")
    assert issues.ok
    issues_response = issues.json()
    issue = list(filter(lambda issue: issue["title"] == "[Feature] request 1", issues_response))[0]
    assert issue
    assert issue["body"] == "Feature description"
```

