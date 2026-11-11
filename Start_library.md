Для начала надо скачать все пакеты и двоичные файлы браузеров по данной команде:

```bash
uv self update
uv add playwright
playwright install
```

### Использование

После установки вы можете спокойно использовать библиотеку на любом из 3 браузеров (`chromium`, `firefox` и `webkit`).

Пример кода:

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto("https://playwright.dev")
    print(page.title())
    browser.close()
```

Playwright поддерживает 2 типа API: **Синхронный** и **асинхронный**. В зависимости от вашего проекта надо использовать соответственный API.

Пример кода на асинхронном:

```python
import asyncio
from playwright.async_api import async_playwright

async def main():
    async with async_playwright() as p:
        browser = await p.chromium.launch()
        page = await browser.new_page()
        await page.goto("https://playwright.dev")
        print(await page.title())
        await browser.close()

asyncio.run(main())
```

### Первый скрипт

В первом скрипте мы переедем на сайт `https://playwright.dev` и сделаем скриншот.

Пример кода:

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.webkit.launch()
    page = browser.new_page()
    page.goto("https://playwright.dev")
    page.screenshot(path="example.png")
    browser.close()
```

Мы упоминали то, что по умолчанию Playwright запускает браузер в безголовом режиме. Чтобы увидеть пользовательский интерфейс, надо прописать следующий код (для Firefox):

```python
firefox.launch(headless=False, slow_mo=50)  # slow_mo для замедления выполнения
```

### Интерактивный режим (REPL)

Для его активации надо написать в командную строку:

```bash
python
```

И в нем запустить:

```python
from playwright.sync_api import sync_playwright

playwright = sync_playwright().start()
# Use playwright.chromium, playwright.firefox or playwright.webkit
# Pass headless=False to launch() to see the browser UI
browser = playwright.chromium.launch()
page = browser.new_page()
page.goto("https://playwright.dev")
page.screenshot(path="example.png")
browser.close()
playwright.stop()
```
