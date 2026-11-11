**Введение**

Playwright может взаимодействовать с элементами ввода HTML, такими как текстовые поля, флажки, радиокнопки, выпадающие списки, а также с кликами мыши, вводом символов, клавишами и сочетаниями клавиш, а также с загрузкой файлов и фокусировкой элементов.

**Ввод текста**

Самый простой способ заполнить поля формы — использовать locator.fill(). При этом элемент фокусируется, и запускается событие input с введенным текстом. Это работает для элементов <input>, <textarea> и [contenteditable] .

примеры кода для синхронного и асинхронного выполнения:

синхронно:

# Text input
page.get_by_role("textbox").fill("Peter")

# Date input
page.get_by_label("Birth date").fill("2020-02-02")

# Time input
page.get_by_label("Appointment time").fill("13:15")

# Local datetime input
page.get_by_label("Local time").fill("2020-03-02T05:15")

Асинхронно:

# Text input
await page.get_by_role("textbox").fill("Peter")

# Date input
await page.get_by_label("Birth date").fill("2020-02-02")

# Time input
await page.get_by_label("Appointment time").fill("13:15")

# Local datetime input
await page.get_by_label("Local time").fill("2020-03-02T05:15")

**Флажки и переключатели**

Использование locator.set_checked() — самый простой способ установить или снять флажок или переключатель. Этот метод можно использовать с элементами input[type=checkbox], input[type=radio] и [role=checkbox].

синхронно:

# Check the checkbox
page.get_by_label('I agree to the terms above').check()

# Assert the checked state
expect(page.get_by_label('Subscribe to newsletter')).to_be_checked()

# Select the radio button
page.get_by_label('XL').check()

асинхронно:

# Check the checkbox
page.get_by_label('I agree to the terms above').check()

# Assert the checked state
expect(page.get_by_label('Subscribe to newsletter')).to_be_checked()

# Select the radio button
page.get_by_label('XL').check()

**Выбор опций**

Выбирает одну или несколько опций в элементе <select> с помощью locator.select_option(). Можно выбрать опцию value, или label. Можно выбрать несколько опций.

синхронно:

# Single selection matching the value or label
page.get_by_label('Choose a color').select_option('blue')

# Single selection matching the label
page.get_by_label('Choose a color').select_option(label='Blue')

# Multiple selected items
page.get_by_label('Choose multiple colors').select_option(['red', 'green', 'blue'])

асинхронно:

# Single selection matching the value or label
await page.get_by_label('Choose a color').select_option('blue')

# Single selection matching the label
await page.get_by_label('Choose a color').select_option(label='Blue')

# Multiple selected items
await page.get_by_label('Choose multiple colors').select_option(['red', 'green', 'blue'])

**Щелчок мышью**

синхронно:
# Generic click
page.get_by_role("button").click()

# Double click
page.get_by_text("Item").dblclick()

# Right click
page.get_by_text("Item").click(button="right")

# Shift + click
page.get_by_text("Item").click(modifiers=["Shift"])

# Hover over element
page.get_by_text("Item").hover()

# Click the top left corner
page.get_by_text("Item").click(position={ "x": 0, "y": 0})

асинхронно:

# Generic click
await page.get_by_role("button").click()

# Double click
await page.get_by_text("Item").dblclick()

# Right click
await page.get_by_text("Item").click(button="right")

# Shift + click
await page.get_by_text("Item").click(modifiers=["Shift"])

# Ctrl + click on Windows and Linux
# Meta + click on macOS
await page.get_by_text("Item").click(modifiers=["ControlOrMeta"])

# Hover over element
await page.get_by_text("Item").hover()

# Click the top left corner
await page.get_by_text("Item").click(position={ "x": 0, "y": 0})

**принудительный щелчок**

Иногда приложения используют нетривиальную логику, когда при наведении курсора на элемент на него накладывается другой элемент, который перехватывает щелчок. Такое поведение неотличимо от ошибки, когда элемент перекрывается и клик обрабатывается в другом месте. Если вы знаете, что это происходит, вы можете обойти проверки возможности выполнения действия и принудительно выполнить клик:

синхронно:

page.get_by_role("button").click(force=True)

асинхронно:

await page.get_by_role("button").click(force=True)

**Програмный клик**

Если вы не заинтересованы в тестировании приложения в реальных условиях и хотите имитировать клик любым возможным способом, вы можете вызвать поведение HTMLElement.click() простым нажатием на элемент с помощью locator.dispatch_event():

синхронно:

page.get_by_role("button").dispatch_event('click')

асинхронно:

await page.get_by_role("button").dispatch_event('click')

**Сочетание клавишь**

синхронно:
# Hit Enter
page.get_by_text("Submit").press("Enter")

# Dispatch Control+Right
page.get_by_role("textbox").press("Control+ArrowRight")

# Press $ sign on keyboard
page.get_by_role("textbox").press("$")

асинхронно:

# Hit Enter
await page.get_by_text("Submit").press("Enter")

# Dispatch Control+Right
await page.get_by_role("textbox").press("Control+ArrowRight")

# Press $ sign on keyboard
await page.get_by_role("textbox").press("$")

Метод locator.press() фокусирует выбранный элемент и выполняет одно нажатие клавиши. Он принимает логические названия клавиш, которые передаются в свойстве keyboardEvent.key

**Загрузка файлов**

Вы можете выбрать входные файлы для загрузки с помощью метода locator.set_input_files(). Он ожидает, что первый аргумент будет указывать на элемент input с типом "file". В массиве можно указать несколько файлов. Если некоторые пути к файлам относительные, они будут разрешены относительно текущего рабочего каталога. Пустой массив очищает список выбранных файлов.

синхронно:

# Select one file
page.get_by_label("Upload file").set_input_files('myfile.pdf')

# Select multiple files
page.get_by_label("Upload files").set_input_files(['file1.txt', 'file2.txt'])

# Select a directory
page.get_by_label("Upload directory").set_input_files('mydir')

# Remove all the selected files
page.get_by_label("Upload file").set_input_files([])

# Upload buffer from memory
page.get_by_label("Upload file").set_input_files(
    files=[
        {"name": "test.txt", "mimeType": "text/plain", "buffer": b"this is a test"}
    ],
)

асинхронно:

# Select one file
await page.get_by_label("Upload file").set_input_files('myfile.pdf')

# Select multiple files
await page.get_by_label("Upload files").set_input_files(['file1.txt', 'file2.txt'])

# Select a directory
await page.get_by_label("Upload directory").set_input_files('mydir')

# Remove all the selected files
await page.get_by_label("Upload file").set_input_files([])

# Upload buffer from memory
await page.get_by_label("Upload file").set_input_files(
    files=[
        {"name": "test.txt", "mimeType": "text/plain", "buffer": b"this is a test"}
    ],
)

**Фокус**

На динамических страницах, обрабатывающих события фокуса, вы можете сфокусировать указанный элемент с помощью locator.focus().

синхронно:
page.get_by_label('password').focus()

асинхронно:
await page.get_by_label('password').focus()

**перетаскивание**

Вы можете выполнить операцию перетаскивания с помощью locator.drag_to(). Этот метод:

Наведите курсор на элемент, который нужно перетащить.
Нажмите левую кнопку мыши.
Подведите курсор к элементу, который получит перетаскиваемый элемент.
Отпустите левую кнопку мыши.

синхронная:


page.locator("#item-to-be-dragged").drag_to(page.locator("#item-to-drop-at"))

асинхронная:

await page.locator("#item-to-be-dragged").drag_to(page.locator("#item-to-drop-at"))

**прокрутка**

синхронно:

# Scrolls automatically so that button is visible
page.get_by_role("button").click()

асинхронно:

# Scrolls automatically so that button is visible
await page.get_by_role("button").click()

