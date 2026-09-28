Q01. [Module objects, sys.modules та import pipeline]

Питання Поясніть механіку теми «Module objects, sys.modules та import pipeline». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Module — runtime object із namespace. Import system перевіряє `sys.modules`, знаходить module spec/loader, створює module object і виконує initialization code. Module може з’явитися в `sys.modules` до повного завершення initialization. Import виконує top-level code. Network calls, heavy initialization або registration із side effects на import-time погіршують startup, test isolation і можуть ускладнювати circular imports.

Senior Reasoning Senior-кандидат розрізняє loading module object, execution top-level code та binding imported names у caller namespace.

Key Points
- module — runtime object
- sys.modules — import cache
- import виконує top-level code
- partial initialization можливий

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Module objects, sys.modules та import pipeline»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[module] [sys.modules] [import] [loader] [initialization]

Q02. [Module objects, sys.modules та import pipeline]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Module objects, sys.modules та import pipeline», і як їх правильно пояснити?

Відповідь Import виконує top-level code. Network calls, heavy initialization або registration із side effects на import-time погіршують startup, test isolation і можуть ускладнювати circular imports. Базова причина цих ефектів така: Module — runtime object із namespace. Import system перевіряє `sys.modules`, знаходить module spec/loader, створює module object і виконує initialization code. Module може з’явитися в `sys.modules` до повного завершення initialization.

Senior Reasoning Senior-кандидат розрізняє loading module object, execution top-level code та binding imported names у caller namespace. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- module — runtime object
- sys.modules — import cache
- import виконує top-level code
- partial initialization можливий

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[module] [sys.modules] [import] [loader] [initialization]

Q03. [Module objects, sys.modules та import pipeline]

Питання Як би ви застосували знання про «Module objects, sys.modules та import pipeline» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Module — runtime object із namespace. Import system перевіряє `sys.modules`, знаходить module spec/loader, створює module object і виконує initialization code. Module може з’явитися в `sys.modules` до повного завершення initialization. У design/debugging сценарії важливо зробити contract явним. Import виконує top-level code. Network calls, heavy initialization або registration із side effects на import-time погіршують startup, test isolation і можуть ускладнювати circular imports.

Senior Reasoning Senior-кандидат розрізняє loading module object, execution top-level code та binding imported names у caller namespace. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- module — runtime object
- sys.modules — import cache
- import виконує top-level code
- partial initialization можливий

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[module] [sys.modules] [import] [loader] [initialization]

Q04. [Packages, import forms та bindings]

Питання Поясніть механіку теми «Packages, import forms та bindings». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `import package.module` і `from package.module import name` створюють різні bindings у namespace caller-а, хоча використовують ту саму import machinery. Packages організують module namespace і можуть мати власний initialization. `from module import value` копіює reference у caller binding; подальший rebinding `module.value` не переприв’яже вже імпортоване локальне ім’я автоматично. Це часто плутають із «live alias».

Senior Reasoning Senior-рівень — проектувати package boundaries так, щоб dependency direction був очевидним, а public API не вимагав знати внутрішню layout структуру.

Key Points
- import form впливає на bindings
- from-import не є live rebinding link
- package має namespace
- public API може приховувати internal layout

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Packages, import forms та bindings»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[package] [from import] [binding] [namespace] [public API]

Q05. [Packages, import forms та bindings]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Packages, import forms та bindings», і як їх правильно пояснити?

Відповідь `from module import value` копіює reference у caller binding; подальший rebinding `module.value` не переприв’яже вже імпортоване локальне ім’я автоматично. Це часто плутають із «live alias». Базова причина цих ефектів така: `import package.module` і `from package.module import name` створюють різні bindings у namespace caller-а, хоча використовують ту саму import machinery. Packages організують module namespace і можуть мати власний initialization.

Senior Reasoning Senior-рівень — проектувати package boundaries так, щоб dependency direction був очевидним, а public API не вимагав знати внутрішню layout структуру. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- import form впливає на bindings
- from-import не є live rebinding link
- package має namespace
- public API може приховувати internal layout

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[package] [from import] [binding] [namespace] [public API]

Q06. [Packages, import forms та bindings]

Питання Як би ви застосували знання про «Packages, import forms та bindings» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `import package.module` і `from package.module import name` створюють різні bindings у namespace caller-а, хоча використовують ту саму import machinery. Packages організують module namespace і можуть мати власний initialization. У design/debugging сценарії важливо зробити contract явним. `from module import value` копіює reference у caller binding; подальший rebinding `module.value` не переприв’яже вже імпортоване локальне ім’я автоматично. Це часто плутають із «live alias».

Senior Reasoning Senior-рівень — проектувати package boundaries так, щоб dependency direction був очевидним, а public API не вимагав знати внутрішню layout структуру. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- import form впливає на bindings
- from-import не є live rebinding link
- package має namespace
- public API може приховувати internal layout

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[package] [from import] [binding] [namespace] [public API]

Q07. [Circular imports та reload]

Питання Поясніть механіку теми «Circular imports та reload». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Circular import виникає, коли module initialization залежить від іншого module, який у свою чергу звертається назад до першого до завершення його initialization. У результаті consumer може побачити partially initialized module. Локальний import усередині function інколи розриває timing cycle, але часто це лише symptom fix. Кращим рішенням може бути винесення shared abstraction або зміна dependency graph. `reload` також не перезв’язує автоматично всі references, імпортовані в інших namespaces.

Senior Reasoning Senior-кандидат сприймає circular import як architecture signal, а не лише syntax issue. Dependency graph має мати зрозумілий напрямок.

Key Points
- circular import дає partial initialization
- local import — не універсальний fix
- краще виправляти dependency graph
- reload не оновлює всі зовнішні bindings

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Circular imports та reload»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[circular import] [partially initialized] [dependency graph] [reload] [import cycle]

Q08. [Circular imports та reload]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Circular imports та reload», і як їх правильно пояснити?

Відповідь Локальний import усередині function інколи розриває timing cycle, але часто це лише symptom fix. Кращим рішенням може бути винесення shared abstraction або зміна dependency graph. `reload` також не перезв’язує автоматично всі references, імпортовані в інших namespaces. Базова причина цих ефектів така: Circular import виникає, коли module initialization залежить від іншого module, який у свою чергу звертається назад до першого до завершення його initialization. У результаті consumer може побачити partially initialized module.

Senior Reasoning Senior-кандидат сприймає circular import як architecture signal, а не лише syntax issue. Dependency graph має мати зрозумілий напрямок. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- circular import дає partial initialization
- local import — не універсальний fix
- краще виправляти dependency graph
- reload не оновлює всі зовнішні bindings

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[circular import] [partially initialized] [dependency graph] [reload] [import cycle]

Q09. [Circular imports та reload]

Питання Як би ви застосували знання про «Circular imports та reload» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Circular import виникає, коли module initialization залежить від іншого module, який у свою чергу звертається назад до першого до завершення його initialization. У результаті consumer може побачити partially initialized module. У design/debugging сценарії важливо зробити contract явним. Локальний import усередині function інколи розриває timing cycle, але часто це лише symptom fix. Кращим рішенням може бути винесення shared abstraction або зміна dependency graph. `reload` також не перезв’язує автоматично всі references, імпортовані в інших namespaces.

Senior Reasoning Senior-кандидат сприймає circular import як architecture signal, а не лише syntax issue. Dependency graph має мати зрозумілий напрямок. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- circular import дає partial initialization
- local import — не універсальний fix
- краще виправляти dependency graph
- reload не оновлює всі зовнішні bindings

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[circular import] [partially initialized] [dependency graph] [reload] [import cycle]
