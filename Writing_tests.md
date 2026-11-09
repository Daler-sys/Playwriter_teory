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
| locator.check() | создать флажок |
| locator.click() | кликнуть |
| locator.uncheck() | убрать флажок |
| locator.hover() | навести курсор на элемент |
| locator.fill() | ввод текста |
| locator.focus() | сфокусируйся на элементе |
| locator.press() | нажми на клавишу |
| locator.set_input_files() | выбрать файлы для загрузки |
| locator.select_option() | выбрать опцию из раскрытого списка |


**Утверждения**

Playwriter включает в себя утверждения которые будут ждать пока главное условие не будет выполнено. Нужны они для чистоты тестов они позволяют сделать их. более устойчивыми и стабильными. 

пример где код будет ждать пока страница не получит заголовок содержащий подстройку Playwriter 

```python
import re
from playwright.sync_api import expect

expect(page).to_have_title(re.compile("Playwright"))
```



**вот таблица с самыми популярными асинхронными утверждениями:**

| Действие | Описание |
| :--- | :---: |
| expect(locator).to_be_checked() | Флажок установлен |
| expect(locator).to_be_enabled() | Элемент управления включен |
| expect(locator).to_be_visible() | Элемент виден |
| expect(locator).to_contain_text() | 	Элемент содержит текст |
| expect(locator).to_have_attribute() | Элемент имеет атрибут |
| expect(locator).to_have_count() | Список элементов имеет заданную длину |
| expect(locator).to_have_text() | Элемент соответствует тексту |
| expect(locator).to_have_value() | Входной элемент имеет значение |
| expect(page).to_have_title() | Страница имеет заголовок |
| expect(page).to_have_url() | Страница имеет URL |
