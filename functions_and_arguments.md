Q01. [Object sharing при виклику функцій]

Питання Поясніть механіку теми «Object sharing при виклику функцій». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Під час виклику аргументи зв’язуються з parameter names; функція отримує references на передані objects, а не автоматичні copies. Тому mutation mutable argument може бути видима caller-у, тоді як local rebinding параметра на інший object не змінює binding caller-а. Функція, яка отримує list і виконує `append`, змінює shared object. Функція, яка робить `items = items + [x]`, створює/отримує новий object і змінює лише локальне ім’я. Контракт API має явно визначати, чи функція мутує аргумент.

Senior Reasoning Фрази «pass by reference» або «pass by value» без уточнення часто вводять в оману. Найточніша mental model для Python — object sharing плюс name binding.

Key Points
- parameters bind to objects
- implicit copy немає
- mutation може бути видима caller-у
- rebinding локальний

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Object sharing при виклику функцій»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[arguments] [parameters] [object sharing] [mutation] [rebinding]

Q02. [Object sharing при виклику функцій]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Object sharing при виклику функцій», і як їх правильно пояснити?

Відповідь Функція, яка отримує list і виконує `append`, змінює shared object. Функція, яка робить `items = items + [x]`, створює/отримує новий object і змінює лише локальне ім’я. Контракт API має явно визначати, чи функція мутує аргумент. Базова причина цих ефектів така: Під час виклику аргументи зв’язуються з parameter names; функція отримує references на передані objects, а не автоматичні copies. Тому mutation mutable argument може бути видима caller-у, тоді як local rebinding параметра на інший object не змінює binding caller-а.

Senior Reasoning Фрази «pass by reference» або «pass by value» без уточнення часто вводять в оману. Найточніша mental model для Python — object sharing плюс name binding. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- parameters bind to objects
- implicit copy немає
- mutation може бути видима caller-у
- rebinding локальний

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[arguments] [parameters] [object sharing] [mutation] [rebinding]

Q03. [Object sharing при виклику функцій]

Питання Як би ви застосували знання про «Object sharing при виклику функцій» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Під час виклику аргументи зв’язуються з parameter names; функція отримує references на передані objects, а не автоматичні copies. Тому mutation mutable argument може бути видима caller-у, тоді як local rebinding параметра на інший object не змінює binding caller-а. У design/debugging сценарії важливо зробити contract явним. Функція, яка отримує list і виконує `append`, змінює shared object. Функція, яка робить `items = items + [x]`, створює/отримує новий object і змінює лише локальне ім’я. Контракт API має явно визначати, чи функція мутує аргумент.

Senior Reasoning Фрази «pass by reference» або «pass by value» без уточнення часто вводять в оману. Найточніша mental model для Python — object sharing плюс name binding. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- parameters bind to objects
- implicit copy немає
- mutation може бути видима caller-у
- rebinding локальний

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[arguments] [parameters] [object sharing] [mutation] [rebinding]

Q04. [Parameter kinds, *args та **kwargs]

Питання Поясніть механіку теми «Parameter kinds, *args та **kwargs». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Python підтримує positional-only, positional-or-keyword, var-positional `*args`, keyword-only та var-keyword `**kwargs` parameters. Маркери `/` і `*` дозволяють зробити API-contract точнішим і контролювати допустимі форми виклику. `*args` збирає додаткові positional arguments, `**kwargs` — додаткові keyword arguments. Keyword-only parameters корисні, коли позиційний виклик погіршує читабельність; positional-only дозволяє не робити ім’я параметра частиною публічного API.

Senior Reasoning Senior-кандидат має думати про evolution API. Надмірний `**kwargs` може приховувати typo і розмивати контракт; надто багато positional parameters погіршують читабельність. Parameter kinds — інструмент дизайну, а не лише синтаксис.

Key Points
- `/` задає positional-only
- `*` вводить keyword-only
- `*args` збирає positional
- `**kwargs` збирає keyword

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Parameter kinds, *args та **kwargs»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[positional-only] [keyword-only] [*args] [**kwargs] [API contract]

Q05. [Parameter kinds, *args та **kwargs]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Parameter kinds, *args та **kwargs», і як їх правильно пояснити?

Відповідь `*args` збирає додаткові positional arguments, `**kwargs` — додаткові keyword arguments. Keyword-only parameters корисні, коли позиційний виклик погіршує читабельність; positional-only дозволяє не робити ім’я параметра частиною публічного API. Базова причина цих ефектів така: Python підтримує positional-only, positional-or-keyword, var-positional `*args`, keyword-only та var-keyword `**kwargs` parameters. Маркери `/` і `*` дозволяють зробити API-contract точнішим і контролювати допустимі форми виклику.

Senior Reasoning Senior-кандидат має думати про evolution API. Надмірний `**kwargs` може приховувати typo і розмивати контракт; надто багато positional parameters погіршують читабельність. Parameter kinds — інструмент дизайну, а не лише синтаксис. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- `/` задає positional-only
- `*` вводить keyword-only
- `*args` збирає positional
- `**kwargs` збирає keyword

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[positional-only] [keyword-only] [*args] [**kwargs] [API contract]

Q06. [Parameter kinds, *args та **kwargs]

Питання Як би ви застосували знання про «Parameter kinds, *args та **kwargs» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Python підтримує positional-only, positional-or-keyword, var-positional `*args`, keyword-only та var-keyword `**kwargs` parameters. Маркери `/` і `*` дозволяють зробити API-contract точнішим і контролювати допустимі форми виклику. У design/debugging сценарії важливо зробити contract явним. `*args` збирає додаткові positional arguments, `**kwargs` — додаткові keyword arguments. Keyword-only parameters корисні, коли позиційний виклик погіршує читабельність; positional-only дозволяє не робити ім’я параметра частиною публічного API.

Senior Reasoning Senior-кандидат має думати про evolution API. Надмірний `**kwargs` може приховувати typo і розмивати контракт; надто багато positional parameters погіршують читабельність. Parameter kinds — інструмент дизайну, а не лише синтаксис. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- `/` задає positional-only
- `*` вводить keyword-only
- `*args` збирає positional
- `**kwargs` збирає keyword

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[positional-only] [keyword-only] [*args] [**kwargs] [API contract]

Q07. [Default arguments і mutable defaults]

Питання Поясніть механіку теми «Default arguments і mutable defaults». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Default argument expressions обчислюються під час визначення функції, а не при кожному виклику. Тому mutable default object може повторно використовуватися між викликами. Класичний баг: `def add(x, items=[]): items.append(x); return items`. Стан накопичується між викликами. Зазвичай використовують sentinel на кшталт `None`, а всередині створюють новий object.

Senior Reasoning Mutable default не завжди помилка: інколи shared cache створюється навмисно. Але це має бути явний design decision. Senior-рівень — відрізняти deliberate persistent state від accidental shared state і враховувати concurrency/testing implications.

Key Points
- defaults evaluated once
- mutable default може зберігати state
- `None` часто використовується як sentinel
- shared default має бути свідомим

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Default arguments і mutable defaults»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[default arguments] [mutable default] [None sentinel] [definition time] [shared state]

Q08. [Default arguments і mutable defaults]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Default arguments і mutable defaults», і як їх правильно пояснити?

Відповідь Класичний баг: `def add(x, items=[]): items.append(x); return items`. Стан накопичується між викликами. Зазвичай використовують sentinel на кшталт `None`, а всередині створюють новий object. Базова причина цих ефектів така: Default argument expressions обчислюються під час визначення функції, а не при кожному виклику. Тому mutable default object може повторно використовуватися між викликами.

Senior Reasoning Mutable default не завжди помилка: інколи shared cache створюється навмисно. Але це має бути явний design decision. Senior-рівень — відрізняти deliberate persistent state від accidental shared state і враховувати concurrency/testing implications. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- defaults evaluated once
- mutable default може зберігати state
- `None` часто використовується як sentinel
- shared default має бути свідомим

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[default arguments] [mutable default] [None sentinel] [definition time] [shared state]

Q09. [Default arguments і mutable defaults]

Питання Як би ви застосували знання про «Default arguments і mutable defaults» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Default argument expressions обчислюються під час визначення функції, а не при кожному виклику. Тому mutable default object може повторно використовуватися між викликами. У design/debugging сценарії важливо зробити contract явним. Класичний баг: `def add(x, items=[]): items.append(x); return items`. Стан накопичується між викликами. Зазвичай використовують sentinel на кшталт `None`, а всередині створюють новий object.

Senior Reasoning Mutable default не завжди помилка: інколи shared cache створюється навмисно. Але це має бути явний design decision. Senior-рівень — відрізняти deliberate persistent state від accidental shared state і враховувати concurrency/testing implications. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- defaults evaluated once
- mutable default може зберігати state
- `None` часто використовується як sentinel
- shared default має бути свідомим

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[default arguments] [mutable default] [None sentinel] [definition time] [shared state]
