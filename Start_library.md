Для начала надо скачать все пакеты и двоичные файлы браузеров по данной команде:
‘’’bash
  uv self update
  uv add playwright
  playwright install
‘’’

**Использоване**
после установки вы можете спокойно использовать библиотеку на любом из 3 браузеров  (chromium, firefox и webkit).

пример кода:
‘’’python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    page.goto("https://playwright.dev")
    print(page.title())
     browser.close()
    
‘’’

playwriter поддерживает 2 типа API: Синхронный и асинхронный. в зависимости от вашего проекта надо использовать соответственный API. 

пример кода на асинхронном:

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

