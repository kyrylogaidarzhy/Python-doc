Q01. [Class/instance state та attribute lookup]

Питання Поясніть механіку теми «Class/instance state та attribute lookup». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Instance і class мають різні namespaces. Attribute lookup через `obj.attr` враховує class hierarchy, MRO, descriptor protocol, instance dictionary і fallback hooks. Спрощена модель «спочатку instance dict, потім class dict» неповна. Class mutable attribute, який спільний для всіх instances, часто стає accidental shared state. Methods, properties та ORM-like fields працюють через lookup machinery, а не як звичайні dictionary entries.

Senior Reasoning Senior-кандидат має розуміти lookup precedence, бо від нього залежать debugging properties/descriptors, mocking і inheritance behavior.

Key Points
- instance і class namespaces різні
- lookup включає MRO/descriptors
- class mutable state може бути shared
- method binding — частина data model

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Class/instance state та attribute lookup»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[class] [instance] [attribute lookup] [MRO] [namespace]

Q02. [Class/instance state та attribute lookup]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Class/instance state та attribute lookup», і як їх правильно пояснити?

Відповідь Class mutable attribute, який спільний для всіх instances, часто стає accidental shared state. Methods, properties та ORM-like fields працюють через lookup machinery, а не як звичайні dictionary entries. Базова причина цих ефектів така: Instance і class мають різні namespaces. Attribute lookup через `obj.attr` враховує class hierarchy, MRO, descriptor protocol, instance dictionary і fallback hooks. Спрощена модель «спочатку instance dict, потім class dict» неповна.

Senior Reasoning Senior-кандидат має розуміти lookup precedence, бо від нього залежать debugging properties/descriptors, mocking і inheritance behavior. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- instance і class namespaces різні
- lookup включає MRO/descriptors
- class mutable state може бути shared
- method binding — частина data model

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[class] [instance] [attribute lookup] [MRO] [namespace]

Q03. [Class/instance state та attribute lookup]

Питання Як би ви застосували знання про «Class/instance state та attribute lookup» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Instance і class мають різні namespaces. Attribute lookup через `obj.attr` враховує class hierarchy, MRO, descriptor protocol, instance dictionary і fallback hooks. Спрощена модель «спочатку instance dict, потім class dict» неповна. У design/debugging сценарії важливо зробити contract явним. Class mutable attribute, який спільний для всіх instances, часто стає accidental shared state. Methods, properties та ORM-like fields працюють через lookup machinery, а не як звичайні dictionary entries.

Senior Reasoning Senior-кандидат має розуміти lookup precedence, бо від нього залежать debugging properties/descriptors, mocking і inheritance behavior. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- instance і class namespaces різні
- lookup включає MRO/descriptors
- class mutable state може бути shared
- method binding — частина data model

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[class] [instance] [attribute lookup] [MRO] [namespace]

Q04. [Inheritance, polymorphism, MRO та super()]

Питання Поясніть механіку теми «Inheritance, polymorphism, MRO та super()». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Python використовує MRO для визначення порядку пошуку атрибутів у hierarchy. `super()` працює відносно MRO і дозволяє cooperative multiple inheritance, якщо класи дотримуються сумісних method contracts. Hard-coded виклик конкретного base class обходить cooperative chain і може ламати mixin/multiple inheritance. У diamond hierarchy важливо, щоб кожен cooperative method викликав `super()` сумісним способом.

Senior Reasoning Senior-рівень — оцінювати, чи inheritance дійсно моделює substitutability, чи composition простіша. MRO — механізм; правильний object design усе одно потребує ясних contracts.

Key Points
- MRO визначає lookup order
- super() слідує MRO
- cooperative inheritance вимагає сумісних signatures
- composition часто простіша

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Inheritance, polymorphism, MRO та super()»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[inheritance] [MRO] [super] [polymorphism] [diamond]

Q05. [Inheritance, polymorphism, MRO та super()]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Inheritance, polymorphism, MRO та super()», і як їх правильно пояснити?

Відповідь Hard-coded виклик конкретного base class обходить cooperative chain і може ламати mixin/multiple inheritance. У diamond hierarchy важливо, щоб кожен cooperative method викликав `super()` сумісним способом. Базова причина цих ефектів така: Python використовує MRO для визначення порядку пошуку атрибутів у hierarchy. `super()` працює відносно MRO і дозволяє cooperative multiple inheritance, якщо класи дотримуються сумісних method contracts.

Senior Reasoning Senior-рівень — оцінювати, чи inheritance дійсно моделює substitutability, чи composition простіша. MRO — механізм; правильний object design усе одно потребує ясних contracts. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- MRO визначає lookup order
- super() слідує MRO
- cooperative inheritance вимагає сумісних signatures
- composition часто простіша

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[inheritance] [MRO] [super] [polymorphism] [diamond]

Q06. [Inheritance, polymorphism, MRO та super()]

Питання Як би ви застосували знання про «Inheritance, polymorphism, MRO та super()» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Python використовує MRO для визначення порядку пошуку атрибутів у hierarchy. `super()` працює відносно MRO і дозволяє cooperative multiple inheritance, якщо класи дотримуються сумісних method contracts. У design/debugging сценарії важливо зробити contract явним. Hard-coded виклик конкретного base class обходить cooperative chain і може ламати mixin/multiple inheritance. У diamond hierarchy важливо, щоб кожен cooperative method викликав `super()` сумісним способом.

Senior Reasoning Senior-рівень — оцінювати, чи inheritance дійсно моделює substitutability, чи composition простіша. MRO — механізм; правильний object design усе одно потребує ясних contracts. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- MRO визначає lookup order
- super() слідує MRO
- cooperative inheritance вимагає сумісних signatures
- composition часто простіша

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[inheritance] [MRO] [super] [polymorphism] [diamond]

Q07. [Special methods та Python protocols]

Питання Поясніть механіку теми «Special methods та Python protocols». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Special methods (`__len__`, `__iter__`, `__eq__`, `__enter__`, arithmetic methods тощо) інтегрують class із мовними protocols. Багато операцій Python виконують implicit lookup special methods на type, а не просто викликають однойменний instance attribute. Реалізація protocol повинна зберігати очікувані algebraic/behavioral invariants. Наприклад, equality має бути узгоджена з hashability, context manager — із resource lifecycle, iterator — із exhaustion semantics.

Senior Reasoning Senior-кандидат має проектувати «pythonic» API через established protocols, але не перевантажувати operators семантикою, яка дивує користувача.

Key Points
- special methods визначають protocol behavior
- оператори делегують data model
- contracts важливіші за синтаксис
- неочікуване operator overloading шкодить API

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Special methods та Python protocols»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[dunder] [protocol] [__iter__] [__eq__] [operator overloading]

Q08. [Special methods та Python protocols]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Special methods та Python protocols», і як їх правильно пояснити?

Відповідь Реалізація protocol повинна зберігати очікувані algebraic/behavioral invariants. Наприклад, equality має бути узгоджена з hashability, context manager — із resource lifecycle, iterator — із exhaustion semantics. Базова причина цих ефектів така: Special methods (`__len__`, `__iter__`, `__eq__`, `__enter__`, arithmetic methods тощо) інтегрують class із мовними protocols. Багато операцій Python виконують implicit lookup special methods на type, а не просто викликають однойменний instance attribute.

Senior Reasoning Senior-кандидат має проектувати «pythonic» API через established protocols, але не перевантажувати operators семантикою, яка дивує користувача. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- special methods визначають protocol behavior
- оператори делегують data model
- contracts важливіші за синтаксис
- неочікуване operator overloading шкодить API

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[dunder] [protocol] [__iter__] [__eq__] [operator overloading]

Q09. [Special methods та Python protocols]

Питання Як би ви застосували знання про «Special methods та Python protocols» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Special methods (`__len__`, `__iter__`, `__eq__`, `__enter__`, arithmetic methods тощо) інтегрують class із мовними protocols. Багато операцій Python виконують implicit lookup special methods на type, а не просто викликають однойменний instance attribute. У design/debugging сценарії важливо зробити contract явним. Реалізація protocol повинна зберігати очікувані algebraic/behavioral invariants. Наприклад, equality має бути узгоджена з hashability, context manager — із resource lifecycle, iterator — із exhaustion semantics.

Senior Reasoning Senior-кандидат має проектувати «pythonic» API через established protocols, але не перевантажувати operators семантикою, яка дивує користувача. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- special methods визначають protocol behavior
- оператори делегують data model
- contracts важливіші за синтаксис
- неочікуване operator overloading шкодить API

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[dunder] [protocol] [__iter__] [__eq__] [operator overloading]
