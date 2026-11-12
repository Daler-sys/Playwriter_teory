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
| Строка 1, ячейка 1 | Строка 1, ячейка 2 |
| Строка 2, ячейка 1 | Строка 2, ячейка 2 |

