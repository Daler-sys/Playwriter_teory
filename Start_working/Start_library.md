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

### Pyinstaller

Вы можете использовать Playwright с Pyinstaller для создания автономных исполняемых файлов.

```python
    from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto("https://playwright.dev/")
    page.screenshot(path="example.png")
    browser.close()
```

Если вы хотите включить браузер в список исполняемых файлов:

```bash
PLAYWRIGHT_BROWSERS_PATH=0 playwright install chromium
pyinstaller -F main.py
```

### Известные проблемы

**time.sleep() приводят к устаревшему состоянию**

Скорее всего, вам не придется ждать вручную, поскольку в Playwright есть автоматическое ожидание. Если вы все еще полагаетесь на него, вам следует использовать page.wait_for_timeout(5000) вместо time.sleep(5) и лучше вообще не дожидаться истечения таймаута, но иногда это полезно для отладки. 

**несовместим с SelectorEventLoop из asyncio в Windows**

Playwright запускает драйвер в подпроцессе, поэтому для него требуется ProactorEventLoop из asyncio в Windows, поскольку SelectorEventLoop не поддерживает асинхронные подпроцессы.

В Windows на Python 3.7 Playwright устанавливает цикл обработки событий по умолчанию на ProactorEventLoop — так же, как в Python 3.8+.

**Многопоточность**

API Playwright не является потокобезопасным. Если вы используете Playwright в многопоточной среде, вам следует создавать отдельный экземпляр Playwright для каждого потока.

**отмена асинхронных задач**

Отмена задачи, в которой выполняется вызов Playwright, не поддерживается и приводит к неопределенному поведению. Если операция должна выполняться дольше, чем вызвавший ее код, запустите ее в отдельной задаче и защитите с помощью asyncio.shield().
