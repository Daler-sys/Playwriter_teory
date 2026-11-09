# Playwright Theory

Theory about Playwright.

## БЛОК №1: УСТАНОВКА

Чтобы установить Playwright, нужно ввести в терминал следующие команды, в зависимости от вашего менеджера пакетов:

* **pip**:
  ```bash
  pip install pytest-playwright
  ```
* **poetry**:
  ```bash
  poetry add pytest-playwright
  ```
* **uv**:
  ```bash
  uv add pytest-playwright
  ```

После установки надо скачать все браузеры командой:
```bash
playwright install
```

Потом надо добавить пример кода **ОБЯЗАТЕЛЬНО С ПРИСТАВКОЙ `test_….py`** (например, `test_example.py`):

```python
import re
from playwright.sync_api import Page, expect

def test_has_title(page: Page):
    page.goto("https://playwright.dev/")

    # Expect a title "to contain" a substring.
    expect(page).to_have_title(re.compile("Playwright"))

def test_get_started_link(page: Page):
    page.goto("https://playwright.dev/")

    # Click the get started link.
    page.get_by_role("link", name="Get started").click()

    # Expects page to have a heading with the name of Installation.
    expect(page.get_by_role("heading", name="Installation")).to_be_visible()
```

> **Примечание:** По умолчанию все выполняется в Chromium. но это можно настроить через командную строку

обновление: обновить можно через команду: 
```bash
  uv add --upgrade pytest-playwright playwright
```


