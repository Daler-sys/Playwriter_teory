Playwright выполняет ряд проверок работоспособности элементов перед выполнением действий, чтобы убедиться, что эти действия ведут себя так, как ожидалось. Он автоматически ожидает прохождения всех соответствующих проверок и только после этого выполняет запрошенное действие. Если требуемые проверки не проходят в пределах заданного timeout, действие завершается ошибкой TimeoutError.

Например, для locator.click() Playwright будет проверять, что:

1. locator указывает ровно на один элемент
2. элемент Visible
3. элемент Stable, то есть не анимируется и анимация не завершена
4. элемент Receives Events, то есть не перекрывается другими элементами
5. элемент Enabled
6. элемент Not Inert


| Действия | Visible | Stable | Receives | Events | Enabled | Editable | Not inert |
| --- | --- | --- | | --- | | --- | | --- | | --- | | --- | ——- |
| locator_check() | YES | YES | YES | YES | YES | - | YES |
| locator_clicl() | YES | YES | YES | YES | YES | - | YES |
| locator_dbclicl() | YES | YES | YES | YES | YES | - | YES |
| locator.set_checked() | YES | YES | YES | - | YES |
| locator.uncheck() | YES | YES | YES | YES | - | YES |
| locator.tap() | YES | YES | YES | YES | - | YES |
| locator.hover() | YES | YES | YES | - | - | - |
| locator.drag_to() | YES | YES | YES | - | - | - |
| locator.screenshot() | YES | YES | - | - | - | - |
| locator.fill() | YES | - | - |YES | YES | YES |
| locator.clear() | YES | - | - | YES |YES | YES |
| locator.select_option() | YES | - | - | YES | - | YES |
| locator.select_text() | YES | - | - | - |- | - |
| locator.scroll_into_view_if_needed() | - | YES | - | - |- | - |
| locator.blur() | - | - | - | - |- | - |
| locator.dispatch_event() | - | - | - | - |- | - |
| locator.focus() | - | - | - | - |- | - |
| locator.press() | - | - | - | - |- | - |
| locator.press_sequentially() | - | - | - | - |- | - |
| locator.set_input_files() | - | - | - | - |- | - |

**Принулительные действия**

Некоторые действия, такие как locator.click(), поддерживают force опцию, которая отключает необязательные проверки возможности выполнения действия. Например, если передать значение true force методу locator.click(), он не будет проверять, действительно ли целевой элемент получает события клика.

**Утверждения**

Playwright включает в себя утверждения с автоматической повторной попыткой, которые устраняют нестабильность, ожидая выполнения условия, аналогично автоматическому ожиданию перед выполнением действий.




| Утверждение | Описание |
| --- | --- |
| expect(locator).to_be_attached() | элемент прикреплен |
| expect(locator).to_be_checked() | флажок установлен |
| expect(locator).to_be_disabled() | элемент отключен |
| expect(locator).to_be_editable() | элемент доступен для редактирования |
| expect(locator).to_be_empty() | контейнер пуст |
| expect(locator).to_be_enabled()  | Элемент включен |
| expect(locator).to_be_focused() | Элемент сфокусирован |
| expect(locator).to_be_hidden() | Элемент не виден |
| expect(locator).to_be_in_viewport() | Элемент пересекает область просмотра |
| expect(locator).to_be_visible() | Элемент виден |
| expect(locator).to_contain_text() | Элемент содержит текст |
| expect(locator).to_have_attribute() | Элемент имеет атрибут DÓM |
| expect(locator).to_have_class() | Элемент имеет свойства класса |
| expect(locator).to_have_count() | Список содержит точное количество дочерних элементов |
| expect(locator).to_have_css() | Элемент имеет свойство CSS |
| expect(locator).to_have_id() | Элемент имеет индификатор |
| expect(locator).to_have_js_property() | Элемент имеет свойства JS |
| expect(locator).to_have_text() | Элемент соответствует тексту |
| expect(locator).to_have_value() | Выходные данные имеют значение |
| expect(locator).to_have_values() | Для выбора выбраны параметры |
| expect(page).to_have_title() | У страницы есть заголовок |
| expect(page).to_have_url() | страница имеет URL |
| expect(response).to_be_ok() | ответ имеет статус ОК |

