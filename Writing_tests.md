## Написание тестов

**Автоматическое ожидание действий**  
   Playwriter сам смотрит на то можно ли потыкать на предмет на его доступность и видимость перед каждым действием

**Умные проверки**
   проверки проходят самостоятельно пока не выполняться главное условие


**Первый тест**
   как мы поняли из интро все тестовые файлы должны начинаться с test_….py
   
   вот пример того как может выглядеть тестовый файл
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

**Действия**

большинство тестов могут начинаться с перехода по ссылкам после этого тест может начать взаимодействовать со страницей


наша библиотека будет ждать загрузки страницы прежде чем начать работать 

**Взаимодействия**

все начинается с поиска объектов для этого Playwriter использует Locators API это способ найти элементы на экране в любой момент времени. 

Playwriter сам будет ждать пока предмет станет доступным так что нет необходимости прописывать разные условия

пример кода как на Playwriter-ре можно взаимодействовать с элементами 

```python
   # создать локатор.
   get_started = page.get_by_role("link", name="Get started")

   # Клик.
   get_started.click()
```

**Основные действия**


| Действие | Описание |
| :--- | :---: |
| Текст слева | По центру |
| Еще текст | Еще текст |
