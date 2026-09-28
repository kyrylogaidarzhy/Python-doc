Q01. [Descriptor protocol та precedence]

Питання Поясніть механіку теми «Descriptor protocol та precedence». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Descriptor — class attribute, що визначає `__get__`, `__set__` або `__delete__`. Data descriptor (із `__set__`/`__delete__`) має пріоритет над instance dictionary; non-data descriptor може бути shadowed instance attribute. Descriptor працює, коли збережений у class, а не просто в instance. Це фундамент для methods, properties, classmethod/staticmethod та багатьох framework fields.

Senior Reasoning Senior-рівень — знати precedence, бо саме вона пояснює managed attributes і те, чому assignment не завжди просто записує value в `obj.__dict__`.

Key Points
- descriptor живе в class
- data descriptor > instance attribute
- non-data descriptor може бути shadowed
- lookup запускає protocol

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Descriptor protocol та precedence»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[descriptor] [__get__] [__set__] [data descriptor] [precedence]

Q02. [Descriptor protocol та precedence]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Descriptor protocol та precedence», і як їх правильно пояснити?

Відповідь Descriptor працює, коли збережений у class, а не просто в instance. Це фундамент для methods, properties, classmethod/staticmethod та багатьох framework fields. Базова причина цих ефектів така: Descriptor — class attribute, що визначає `__get__`, `__set__` або `__delete__`. Data descriptor (із `__set__`/`__delete__`) має пріоритет над instance dictionary; non-data descriptor може бути shadowed instance attribute.

Senior Reasoning Senior-рівень — знати precedence, бо саме вона пояснює managed attributes і те, чому assignment не завжди просто записує value в `obj.__dict__`. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- descriptor живе в class
- data descriptor > instance attribute
- non-data descriptor може бути shadowed
- lookup запускає protocol

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[descriptor] [__get__] [__set__] [data descriptor] [precedence]

Q03. [Descriptor protocol та precedence]

Питання Як би ви застосували знання про «Descriptor protocol та precedence» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Descriptor — class attribute, що визначає `__get__`, `__set__` або `__delete__`. Data descriptor (із `__set__`/`__delete__`) має пріоритет над instance dictionary; non-data descriptor може бути shadowed instance attribute. У design/debugging сценарії важливо зробити contract явним. Descriptor працює, коли збережений у class, а не просто в instance. Це фундамент для methods, properties, classmethod/staticmethod та багатьох framework fields.

Senior Reasoning Senior-рівень — знати precedence, бо саме вона пояснює managed attributes і те, чому assignment не завжди просто записує value в `obj.__dict__`. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- descriptor живе в class
- data descriptor > instance attribute
- non-data descriptor може бути shadowed
- lookup запускає protocol

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[descriptor] [__get__] [__set__] [data descriptor] [precedence]

Q04. [property та managed attributes]

Питання Поясніть механіку теми «property та managed attributes». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `property` дає attribute-style API із getter/setter/deleter logic. Це дозволяє зберігати зовнішній interface `obj.value`, але додати validation, lazy calculation або compatibility logic. Property корисний для local invariant enforcement, але важкий I/O або непередбачувано дорогі операції в getter роблять attribute access оманливим. Setter також не повинен приховувати несподівані side effects без вагомої причини.

Senior Reasoning Senior-кандидат має балансувати encapsulation та predictability. Property — хороший evolution tool, але не спосіб замаскувати remote call під простий field read.

Key Points
- property зберігає attribute syntax
- може enforce invariant
- дорогий getter — design smell
- setter side effects мають бути очевидними

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «property та managed attributes»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[property] [getter] [setter] [managed attribute] [encapsulation]

Q05. [property та managed attributes]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «property та managed attributes», і як їх правильно пояснити?

Відповідь Property корисний для local invariant enforcement, але важкий I/O або непередбачувано дорогі операції в getter роблять attribute access оманливим. Setter також не повинен приховувати несподівані side effects без вагомої причини. Базова причина цих ефектів така: `property` дає attribute-style API із getter/setter/deleter logic. Це дозволяє зберігати зовнішній interface `obj.value`, але додати validation, lazy calculation або compatibility logic.

Senior Reasoning Senior-кандидат має балансувати encapsulation та predictability. Property — хороший evolution tool, але не спосіб замаскувати remote call під простий field read. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- property зберігає attribute syntax
- може enforce invariant
- дорогий getter — design smell
- setter side effects мають бути очевидними

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[property] [getter] [setter] [managed attribute] [encapsulation]

Q06. [property та managed attributes]

Питання Як би ви застосували знання про «property та managed attributes» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `property` дає attribute-style API із getter/setter/deleter logic. Це дозволяє зберігати зовнішній interface `obj.value`, але додати validation, lazy calculation або compatibility logic. У design/debugging сценарії важливо зробити contract явним. Property корисний для local invariant enforcement, але важкий I/O або непередбачувано дорогі операції в getter роблять attribute access оманливим. Setter також не повинен приховувати несподівані side effects без вагомої причини.

Senior Reasoning Senior-кандидат має балансувати encapsulation та predictability. Property — хороший evolution tool, але не спосіб замаскувати remote call під простий field read. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- property зберігає attribute syntax
- може enforce invariant
- дорогий getter — design smell
- setter side effects мають бути очевидними

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[property] [getter] [setter] [managed attribute] [encapsulation]

Q07. [Bound methods, __getattr__ та __getattribute__]

Питання Поясніть механіку теми «Bound methods, __getattr__ та __getattribute__». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Functions у class є non-data descriptors: access через instance створює bound method, що зберігає function і instance. `__getattribute__` бере участь у кожному attribute access, тоді як `__getattr__` — fallback після невдалого normal lookup. Неправильний override `__getattribute__` легко створює recursion або ламає стандартну lookup machinery. `__getattr__` безпечніший для dynamic fallback, але також може приховувати typo, якщо повертає value для будь-якого імені.

Senior Reasoning Senior-рівень — використовувати hooks локально й обґрунтовано. Framework magic має зберігати debuggability та не руйнувати інваріанти стандартного lookup.

Key Points
- function descriptor створює bound method
- __getattribute__ викликається завжди
- __getattr__ — fallback
- dynamic lookup може приховати помилки

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Bound methods, __getattr__ та __getattribute__»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[bound method] [__getattribute__] [__getattr__] [function descriptor] [lookup hook]

Q08. [Bound methods, __getattr__ та __getattribute__]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Bound methods, __getattr__ та __getattribute__», і як їх правильно пояснити?

Відповідь Неправильний override `__getattribute__` легко створює recursion або ламає стандартну lookup machinery. `__getattr__` безпечніший для dynamic fallback, але також може приховувати typo, якщо повертає value для будь-якого імені. Базова причина цих ефектів така: Functions у class є non-data descriptors: access через instance створює bound method, що зберігає function і instance. `__getattribute__` бере участь у кожному attribute access, тоді як `__getattr__` — fallback після невдалого normal lookup.

Senior Reasoning Senior-рівень — використовувати hooks локально й обґрунтовано. Framework magic має зберігати debuggability та не руйнувати інваріанти стандартного lookup. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- function descriptor створює bound method
- __getattribute__ викликається завжди
- __getattr__ — fallback
- dynamic lookup може приховати помилки

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[bound method] [__getattribute__] [__getattr__] [function descriptor] [lookup hook]

Q09. [Bound methods, __getattr__ та __getattribute__]

Питання Як би ви застосували знання про «Bound methods, __getattr__ та __getattribute__» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Functions у class є non-data descriptors: access через instance створює bound method, що зберігає function і instance. `__getattribute__` бере участь у кожному attribute access, тоді як `__getattr__` — fallback після невдалого normal lookup. У design/debugging сценарії важливо зробити contract явним. Неправильний override `__getattribute__` легко створює recursion або ламає стандартну lookup machinery. `__getattr__` безпечніший для dynamic fallback, але також може приховувати typo, якщо повертає value для будь-якого імені.

Senior Reasoning Senior-рівень — використовувати hooks локально й обґрунтовано. Framework magic має зберігати debuggability та не руйнувати інваріанти стандартного lookup. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- function descriptor створює bound method
- __getattribute__ викликається завжди
- __getattr__ — fallback
- dynamic lookup може приховати помилки

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[bound method] [__getattribute__] [__getattr__] [function descriptor] [lookup hook]
