Q01. [LEGB та resolution імен]

Питання Поясніть механіку теми «LEGB та resolution імен». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Name lookup у функції концептуально проходить local, enclosing function scopes, module global і builtins. Якщо binding operation для імені є в function block, це ім’я зазвичай вважається local у всьому блоці, якщо не оголошено `global` або `nonlocal`. Через це `print(x); x = 1` усередині функції може дати `UnboundLocalError`, навіть якщо global `x` існує. Python визначає локальність імені зі структури блоку, а не лише з порядку runtime execution.

Senior Reasoning Senior-кандидат має розрізняти lexical scope та runtime value. Це критично для debugging closures, decorators і nested functions.

Key Points
- LEGB описує lookup
- binding у function робить name local
- UnboundLocalError ≠ NameError у сенсі причини
- scope lexical

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «LEGB та resolution імен»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[LEGB] [scope] [UnboundLocalError] [binding] [builtins]

Q02. [LEGB та resolution імен]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «LEGB та resolution імен», і як їх правильно пояснити?

Відповідь Через це `print(x); x = 1` усередині функції може дати `UnboundLocalError`, навіть якщо global `x` існує. Python визначає локальність імені зі структури блоку, а не лише з порядку runtime execution. Базова причина цих ефектів така: Name lookup у функції концептуально проходить local, enclosing function scopes, module global і builtins. Якщо binding operation для імені є в function block, це ім’я зазвичай вважається local у всьому блоці, якщо не оголошено `global` або `nonlocal`.

Senior Reasoning Senior-кандидат має розрізняти lexical scope та runtime value. Це критично для debugging closures, decorators і nested functions. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- LEGB описує lookup
- binding у function робить name local
- UnboundLocalError ≠ NameError у сенсі причини
- scope lexical

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[LEGB] [scope] [UnboundLocalError] [binding] [builtins]

Q03. [LEGB та resolution імен]

Питання Як би ви застосували знання про «LEGB та resolution імен» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Name lookup у функції концептуально проходить local, enclosing function scopes, module global і builtins. Якщо binding operation для імені є в function block, це ім’я зазвичай вважається local у всьому блоці, якщо не оголошено `global` або `nonlocal`. У design/debugging сценарії важливо зробити contract явним. Через це `print(x); x = 1` усередині функції може дати `UnboundLocalError`, навіть якщо global `x` існує. Python визначає локальність імені зі структури блоку, а не лише з порядку runtime execution.

Senior Reasoning Senior-кандидат має розрізняти lexical scope та runtime value. Це критично для debugging closures, decorators і nested functions. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- LEGB описує lookup
- binding у function робить name local
- UnboundLocalError ≠ NameError у сенсі причини
- scope lexical

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[LEGB] [scope] [UnboundLocalError] [binding] [builtins]

Q04. [global та nonlocal]

Питання Поясніть механіку теми «global та nonlocal». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `global` спрямовує binding до module namespace; `nonlocal` — до вже існуючого binding у найближчому enclosing function scope. `nonlocal` не створює довільну глобальну змінну і вимагає відповідного binding у зовнішній функції. Ці оператори потрібні, коли nested function має не лише читати, а й переприв’язувати name із зовнішнього scope. Часте використання `global` зазвичай збільшує coupling і ускладнює тестування.

Senior Reasoning Senior-рівень — розуміти, коли closure state доречний, а коли краще використати object зі станом або explicit dependency. `global/nonlocal` вирішують binding semantics, але не concurrency або ownership проблеми.

Key Points
- global → module binding
- nonlocal → enclosing function binding
- nonlocal потребує існуюче ім’я
- state ownership лишається design concern

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «global та nonlocal»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[global] [nonlocal] [enclosing scope] [module namespace] [state]

Q05. [global та nonlocal]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «global та nonlocal», і як їх правильно пояснити?

Відповідь Ці оператори потрібні, коли nested function має не лише читати, а й переприв’язувати name із зовнішнього scope. Часте використання `global` зазвичай збільшує coupling і ускладнює тестування. Базова причина цих ефектів така: `global` спрямовує binding до module namespace; `nonlocal` — до вже існуючого binding у найближчому enclosing function scope. `nonlocal` не створює довільну глобальну змінну і вимагає відповідного binding у зовнішній функції.

Senior Reasoning Senior-рівень — розуміти, коли closure state доречний, а коли краще використати object зі станом або explicit dependency. `global/nonlocal` вирішують binding semantics, але не concurrency або ownership проблеми. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- global → module binding
- nonlocal → enclosing function binding
- nonlocal потребує існуюче ім’я
- state ownership лишається design concern

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[global] [nonlocal] [enclosing scope] [module namespace] [state]

Q06. [global та nonlocal]

Питання Як би ви застосували знання про «global та nonlocal» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `global` спрямовує binding до module namespace; `nonlocal` — до вже існуючого binding у найближчому enclosing function scope. `nonlocal` не створює довільну глобальну змінну і вимагає відповідного binding у зовнішній функції. У design/debugging сценарії важливо зробити contract явним. Ці оператори потрібні, коли nested function має не лише читати, а й переприв’язувати name із зовнішнього scope. Часте використання `global` зазвичай збільшує coupling і ускладнює тестування.

Senior Reasoning Senior-рівень — розуміти, коли closure state доречний, а коли краще використати object зі станом або explicit dependency. `global/nonlocal` вирішують binding semantics, але не concurrency або ownership проблеми. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- global → module binding
- nonlocal → enclosing function binding
- nonlocal потребує існуюче ім’я
- state ownership лишається design concern

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[global] [nonlocal] [enclosing scope] [module namespace] [state]

Q07. [Closures, late binding та lifetime]

Питання Поясніть механіку теми «Closures, late binding та lifetime». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Closure — function, що зберігає доступ до free variables із lexical environment. Важливо, що closure зазвичай посилається на binding/cell, а не «заморожує» значення автоматично в момент створення function object. Через late binding callbacks, створені в loop, можуть усі бачити останнє значення loop variable. Типове рішення — зафіксувати потрібне значення через default argument або створити окремий factory scope.

Senior Reasoning Closure продовжує lifetime captured objects. Це корисно для stateful factories, але може ненавмисно утримувати великі object graphs і впливати на memory usage.

Key Points
- closure захоплює free variable
- late binding може дивувати
- factory/default може зафіксувати значення
- closure подовжує lifetime references

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Closures, late binding та lifetime»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[closure] [late binding] [free variable] [cell] [lifetime]

Q08. [Closures, late binding та lifetime]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Closures, late binding та lifetime», і як їх правильно пояснити?

Відповідь Через late binding callbacks, створені в loop, можуть усі бачити останнє значення loop variable. Типове рішення — зафіксувати потрібне значення через default argument або створити окремий factory scope. Базова причина цих ефектів така: Closure — function, що зберігає доступ до free variables із lexical environment. Важливо, що closure зазвичай посилається на binding/cell, а не «заморожує» значення автоматично в момент створення function object.

Senior Reasoning Closure продовжує lifetime captured objects. Це корисно для stateful factories, але може ненавмисно утримувати великі object graphs і впливати на memory usage. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- closure захоплює free variable
- late binding може дивувати
- factory/default може зафіксувати значення
- closure подовжує lifetime references

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[closure] [late binding] [free variable] [cell] [lifetime]

Q09. [Closures, late binding та lifetime]

Питання Як би ви застосували знання про «Closures, late binding та lifetime» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Closure — function, що зберігає доступ до free variables із lexical environment. Важливо, що closure зазвичай посилається на binding/cell, а не «заморожує» значення автоматично в момент створення function object. У design/debugging сценарії важливо зробити contract явним. Через late binding callbacks, створені в loop, можуть усі бачити останнє значення loop variable. Типове рішення — зафіксувати потрібне значення через default argument або створити окремий factory scope.

Senior Reasoning Closure продовжує lifetime captured objects. Це корисно для stateful factories, але може ненавмисно утримувати великі object graphs і впливати на memory usage. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- closure захоплює free variable
- late binding може дивувати
- factory/default може зафіксувати значення
- closure подовжує lifetime references

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[closure] [late binding] [free variable] [cell] [lifetime]
