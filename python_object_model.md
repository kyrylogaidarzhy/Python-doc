Q01. [Об’єкти, identity та name binding]

Питання Поясніть механіку теми «Об’єкти, identity та name binding». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь У Python дані представлені об’єктами, а імена є прив’язками до цих об’єктів. Кожен об’єкт має identity, type і value. Операція присвоєння змінює binding імені, а не копіює об’єкт автоматично. Оператор `is` перевіряє identity, тоді як `==` перевіряє семантичну рівність і може бути перевизначений типом. Ключова практична пастка — плутати rebinding з мутацією. Після `b = a` два імена можуть вказувати на той самий mutable object, тому зміна через одне ім’я буде видима через інше. Але `b = []` лише переприв’яже `b` і не змінить об’єкт, на який вказує `a`.

Senior Reasoning Senior-рівень передбачає reasoning через граф посилань, а не через модель «змінна містить значення». Не можна покладатися на випадкове interning/cache immutable-об’єктів і використовувати `is` як заміну `==`; identity comparison доречний для singleton-сентинелів на кшталт `None`.

Key Points
- ім’я — це binding до object
- `is` перевіряє identity
- `==` перевіряє equality
- assignment не створює копію

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Об’єкти, identity та name binding»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[identity] [binding] [is] [==] [aliasing]

Q02. [Об’єкти, identity та name binding]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Об’єкти, identity та name binding», і як їх правильно пояснити?

Відповідь Ключова практична пастка — плутати rebinding з мутацією. Після `b = a` два імена можуть вказувати на той самий mutable object, тому зміна через одне ім’я буде видима через інше. Але `b = []` лише переприв’яже `b` і не змінить об’єкт, на який вказує `a`. Базова причина цих ефектів така: У Python дані представлені об’єктами, а імена є прив’язками до цих об’єктів. Кожен об’єкт має identity, type і value. Операція присвоєння змінює binding імені, а не копіює об’єкт автоматично. Оператор `is` перевіряє identity, тоді як `==` перевіряє семантичну рівність і може бути перевизначений типом.

Senior Reasoning Senior-рівень передбачає reasoning через граф посилань, а не через модель «змінна містить значення». Не можна покладатися на випадкове interning/cache immutable-об’єктів і використовувати `is` як заміну `==`; identity comparison доречний для singleton-сентинелів на кшталт `None`. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- ім’я — це binding до object
- `is` перевіряє identity
- `==` перевіряє equality
- assignment не створює копію

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[identity] [binding] [is] [==] [aliasing]

Q03. [Об’єкти, identity та name binding]

Питання Як би ви застосували знання про «Об’єкти, identity та name binding» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь У Python дані представлені об’єктами, а імена є прив’язками до цих об’єктів. Кожен об’єкт має identity, type і value. Операція присвоєння змінює binding імені, а не копіює об’єкт автоматично. Оператор `is` перевіряє identity, тоді як `==` перевіряє семантичну рівність і може бути перевизначений типом. У design/debugging сценарії важливо зробити contract явним. Ключова практична пастка — плутати rebinding з мутацією. Після `b = a` два імена можуть вказувати на той самий mutable object, тому зміна через одне ім’я буде видима через інше. Але `b = []` лише переприв’яже `b` і не змінить об’єкт, на який вказує `a`.

Senior Reasoning Senior-рівень передбачає reasoning через граф посилань, а не через модель «змінна містить значення». Не можна покладатися на випадкове interning/cache immutable-об’єктів і використовувати `is` як заміну `==`; identity comparison доречний для singleton-сентинелів на кшталт `None`. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- ім’я — це binding до object
- `is` перевіряє identity
- `==` перевіряє equality
- assignment не створює копію

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[identity] [binding] [is] [==] [aliasing]

Q04. [Mutable та immutable об’єкти]

Питання Поясніть механіку теми «Mutable та immutable об’єкти». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Mutability описує, чи може value конкретного object змінюватися без зміни його identity. `list`, `dict`, `set` зазвичай mutable; `int`, `str`, `bytes`, `tuple` — immutable як об’єкти свого типу. Immutable container може містити посилання на mutable objects, тому незмінність контейнера не означає глибоку незмінність усього графа. Операції на immutable-значеннях зазвичай створюють новий об’єкт і переприв’язують ім’я. Для mutable structures методи на кшталт `append`, `update` або item assignment змінюють існуючий object. Це критично для API-контрактів, shared state і кешування.

Senior Reasoning Senior-кандидат має розрізняти shallow immutability та deep immutability, а також пояснювати вплив mutability на hashability, defensive copying і thread-safety. Сам факт, що reference оголошений у tuple, не робить вкладений list immutable.

Key Points
- mutability належить object/type semantics
- immutable operation часто створює новий object
- tuple може містити mutable element
- мутація і rebinding — різні речі

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Mutable та immutable об’єкти»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[mutable] [immutable] [rebinding] [tuple] [shared state]

Q05. [Mutable та immutable об’єкти]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Mutable та immutable об’єкти», і як їх правильно пояснити?

Відповідь Операції на immutable-значеннях зазвичай створюють новий об’єкт і переприв’язують ім’я. Для mutable structures методи на кшталт `append`, `update` або item assignment змінюють існуючий object. Це критично для API-контрактів, shared state і кешування. Базова причина цих ефектів така: Mutability описує, чи може value конкретного object змінюватися без зміни його identity. `list`, `dict`, `set` зазвичай mutable; `int`, `str`, `bytes`, `tuple` — immutable як об’єкти свого типу. Immutable container може містити посилання на mutable objects, тому незмінність контейнера не означає глибоку незмінність усього графа.

Senior Reasoning Senior-кандидат має розрізняти shallow immutability та deep immutability, а також пояснювати вплив mutability на hashability, defensive copying і thread-safety. Сам факт, що reference оголошений у tuple, не робить вкладений list immutable. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- mutability належить object/type semantics
- immutable operation часто створює новий object
- tuple може містити mutable element
- мутація і rebinding — різні речі

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[mutable] [immutable] [rebinding] [tuple] [shared state]

Q06. [Mutable та immutable об’єкти]

Питання Як би ви застосували знання про «Mutable та immutable об’єкти» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Mutability описує, чи може value конкретного object змінюватися без зміни його identity. `list`, `dict`, `set` зазвичай mutable; `int`, `str`, `bytes`, `tuple` — immutable як об’єкти свого типу. Immutable container може містити посилання на mutable objects, тому незмінність контейнера не означає глибоку незмінність усього графа. У design/debugging сценарії важливо зробити contract явним. Операції на immutable-значеннях зазвичай створюють новий об’єкт і переприв’язують ім’я. Для mutable structures методи на кшталт `append`, `update` або item assignment змінюють існуючий object. Це критично для API-контрактів, shared state і кешування.

Senior Reasoning Senior-кандидат має розрізняти shallow immutability та deep immutability, а також пояснювати вплив mutability на hashability, defensive copying і thread-safety. Сам факт, що reference оголошений у tuple, не робить вкладений list immutable. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- mutability належить object/type semantics
- immutable operation часто створює новий object
- tuple може містити mutable element
- мутація і rebinding — різні речі

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[mutable] [immutable] [rebinding] [tuple] [shared state]

Q07. [Aliasing, shallow/deep copy та object lifetime]

Питання Поясніть механіку теми «Aliasing, shallow/deep copy та object lifetime». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Aliasing виникає, коли кілька references вказують на той самий object. Shallow copy створює новий зовнішній контейнер, але повторно використовує references на вкладені objects; deep copy рекурсивно намагається копіювати вкладений граф. Жоден із підходів не є універсально правильним: важливий семантичний контракт object graph. Типова помилка — очікувати, що `list(x)` або `copy.copy()` ізолює вкладені dictionaries/lists. Інша помилка — бездумно застосовувати `deepcopy` до графів із shared references, ресурсами, locks або об’єктами зі спеціальною логікою копіювання.

Senior Reasoning На senior-рівні слід спочатку визначити ownership model: хто володіє state, чи допускається sharing, чи потрібна snapshot semantics. Копіювання — це не «виправлення aliasing», а один зі способів реалізувати boundary між owners.

Key Points
- aliasing = кілька references на один object
- shallow copy не копіює вкладені objects
- deep copy має semantic cost
- ownership важливіший за механічне копіювання

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Aliasing, shallow/deep copy та object lifetime»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[aliasing] [copy.copy] [copy.deepcopy] [ownership] [object graph]

Q08. [Aliasing, shallow/deep copy та object lifetime]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Aliasing, shallow/deep copy та object lifetime», і як їх правильно пояснити?

Відповідь Типова помилка — очікувати, що `list(x)` або `copy.copy()` ізолює вкладені dictionaries/lists. Інша помилка — бездумно застосовувати `deepcopy` до графів із shared references, ресурсами, locks або об’єктами зі спеціальною логікою копіювання. Базова причина цих ефектів така: Aliasing виникає, коли кілька references вказують на той самий object. Shallow copy створює новий зовнішній контейнер, але повторно використовує references на вкладені objects; deep copy рекурсивно намагається копіювати вкладений граф. Жоден із підходів не є універсально правильним: важливий семантичний контракт object graph.

Senior Reasoning На senior-рівні слід спочатку визначити ownership model: хто володіє state, чи допускається sharing, чи потрібна snapshot semantics. Копіювання — це не «виправлення aliasing», а один зі способів реалізувати boundary між owners. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- aliasing = кілька references на один object
- shallow copy не копіює вкладені objects
- deep copy має semantic cost
- ownership важливіший за механічне копіювання

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[aliasing] [copy.copy] [copy.deepcopy] [ownership] [object graph]

Q09. [Aliasing, shallow/deep copy та object lifetime]

Питання Як би ви застосували знання про «Aliasing, shallow/deep copy та object lifetime» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Aliasing виникає, коли кілька references вказують на той самий object. Shallow copy створює новий зовнішній контейнер, але повторно використовує references на вкладені objects; deep copy рекурсивно намагається копіювати вкладений граф. Жоден із підходів не є універсально правильним: важливий семантичний контракт object graph. У design/debugging сценарії важливо зробити contract явним. Типова помилка — очікувати, що `list(x)` або `copy.copy()` ізолює вкладені dictionaries/lists. Інша помилка — бездумно застосовувати `deepcopy` до графів із shared references, ресурсами, locks або об’єктами зі спеціальною логікою копіювання.

Senior Reasoning На senior-рівні слід спочатку визначити ownership model: хто володіє state, чи допускається sharing, чи потрібна snapshot semantics. Копіювання — це не «виправлення aliasing», а один зі способів реалізувати boundary між owners. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- aliasing = кілька references на один object
- shallow copy не копіює вкладені objects
- deep copy має semantic cost
- ownership важливіший за механічне копіювання

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[aliasing] [copy.copy] [copy.deepcopy] [ownership] [object graph]
