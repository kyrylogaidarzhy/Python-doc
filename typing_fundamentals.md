Q01. [Type hints, Any, object та Optional/Union]

Питання Поясніть механіку теми «Type hints, Any, object та Optional/Union». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Type hints у Python насамперед описують contract для static analysis, IDE та tooling; runtime не enforce-ить annotations автоматично. `Any` дозволяє type checker-у пропускати більшість перевірок, тоді як `object` означає довільний object, але вимагає narrowing перед конкретними операціями. Optionality виражається як `T | None`. Неконтрольоване поширення `Any` із external boundary послаблює весь type graph. Краще validate/narrow dynamic input близько до boundary і далі працювати з конкретними types.

Senior Reasoning Senior-кандидат використовує types як design/documentation tool, але не підміняє runtime validation там, де приходять untrusted JSON, DB rows або network payloads.

Key Points
- annotations не enforce-яться автоматично
- Any вимикає значну частину checking
- object потребує narrowing
- T | None робить optionality явною

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Type hints, Any, object та Optional/Union»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[type hints] [Any] [object] [Union] [Optional]

Q02. [Type hints, Any, object та Optional/Union]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Type hints, Any, object та Optional/Union», і як їх правильно пояснити?

Відповідь Неконтрольоване поширення `Any` із external boundary послаблює весь type graph. Краще validate/narrow dynamic input близько до boundary і далі працювати з конкретними types. Базова причина цих ефектів така: Type hints у Python насамперед описують contract для static analysis, IDE та tooling; runtime не enforce-ить annotations автоматично. `Any` дозволяє type checker-у пропускати більшість перевірок, тоді як `object` означає довільний object, але вимагає narrowing перед конкретними операціями. Optionality виражається як `T | None`.

Senior Reasoning Senior-кандидат використовує types як design/documentation tool, але не підміняє runtime validation там, де приходять untrusted JSON, DB rows або network payloads. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- annotations не enforce-яться автоматично
- Any вимикає значну частину checking
- object потребує narrowing
- T | None робить optionality явною

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[type hints] [Any] [object] [Union] [Optional]

Q03. [Type hints, Any, object та Optional/Union]

Питання Як би ви застосували знання про «Type hints, Any, object та Optional/Union» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Type hints у Python насамперед описують contract для static analysis, IDE та tooling; runtime не enforce-ить annotations автоматично. `Any` дозволяє type checker-у пропускати більшість перевірок, тоді як `object` означає довільний object, але вимагає narrowing перед конкретними операціями. Optionality виражається як `T | None`. У design/debugging сценарії важливо зробити contract явним. Неконтрольоване поширення `Any` із external boundary послаблює весь type graph. Краще validate/narrow dynamic input близько до boundary і далі працювати з конкретними types.

Senior Reasoning Senior-кандидат використовує types як design/documentation tool, але не підміняє runtime validation там, де приходять untrusted JSON, DB rows або network payloads. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- annotations не enforce-яться автоматично
- Any вимикає значну частину checking
- object потребує narrowing
- T | None робить optionality явною

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[type hints] [Any] [object] [Union] [Optional]

Q04. [Generics та collection abstractions]

Питання Поясніть механіку теми «Generics та collection abstractions». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Generics дозволяють виразити зв’язок між input/output types і parameterized containers, наприклад `list[T]`. Для API часто краще приймати abstraction (`Iterable`, `Sequence`, `Mapping`) замість конкретного `list`/`dict`, якщо реалізація не залежить від mutability або конкретних operations. Занадто конкретний parameter type створює unnecessary coupling. Водночас надто широкий interface може приховати фактичні вимоги, наприклад потребу в random access або повторній iteration.

Senior Reasoning Senior-рівень — вибирати найвужчий meaningful behavioral contract: достатньо абстрактний для callers, але достатньо точний для реалізації.

Key Points
- generic зберігає type relationship
- приймай abstraction за потребою
- interface має відображати required operations
- не розширювати contract без причини

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Generics та collection abstractions»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[generics] [TypeVar] [Iterable] [Sequence] [Mapping]

Q05. [Generics та collection abstractions]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Generics та collection abstractions», і як їх правильно пояснити?

Відповідь Занадто конкретний parameter type створює unnecessary coupling. Водночас надто широкий interface може приховати фактичні вимоги, наприклад потребу в random access або повторній iteration. Базова причина цих ефектів така: Generics дозволяють виразити зв’язок між input/output types і parameterized containers, наприклад `list[T]`. Для API часто краще приймати abstraction (`Iterable`, `Sequence`, `Mapping`) замість конкретного `list`/`dict`, якщо реалізація не залежить від mutability або конкретних operations.

Senior Reasoning Senior-рівень — вибирати найвужчий meaningful behavioral contract: достатньо абстрактний для callers, але достатньо точний для реалізації. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- generic зберігає type relationship
- приймай abstraction за потребою
- interface має відображати required operations
- не розширювати contract без причини

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[generics] [TypeVar] [Iterable] [Sequence] [Mapping]

Q06. [Generics та collection abstractions]

Питання Як би ви застосували знання про «Generics та collection abstractions» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Generics дозволяють виразити зв’язок між input/output types і parameterized containers, наприклад `list[T]`. Для API часто краще приймати abstraction (`Iterable`, `Sequence`, `Mapping`) замість конкретного `list`/`dict`, якщо реалізація не залежить від mutability або конкретних operations. У design/debugging сценарії важливо зробити contract явним. Занадто конкретний parameter type створює unnecessary coupling. Водночас надто широкий interface може приховати фактичні вимоги, наприклад потребу в random access або повторній iteration.

Senior Reasoning Senior-рівень — вибирати найвужчий meaningful behavioral contract: достатньо абстрактний для callers, але достатньо точний для реалізації. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- generic зберігає type relationship
- приймай abstraction за потребою
- interface має відображати required operations
- не розширювати contract без причини

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[generics] [TypeVar] [Iterable] [Sequence] [Mapping]

Q07. [Protocol та type narrowing]

Питання Поясніть механіку теми «Protocol та type narrowing». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `Protocol` підтримує structural typing: object сумісний, якщо має потрібний interface, без обов’язкового nominal inheritance. Type narrowing уточнює union/object type після `isinstance`, перевірки `None` або інших guard conditions. Protocols зручні для dependency inversion і test doubles. Але великий Protocol, який копіює весь concrete class, перетворюється на приховане tight coupling.

Senior Reasoning Senior-кандидат проектує маленькі capability-oriented protocols і локалізує dynamic type uncertainty біля boundaries.

Key Points
- Protocol = structural contract
- inheritance не обов’язкове
- narrowing уточнює type
- малий protocol кращий за копію concrete API

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Protocol та type narrowing»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[Protocol] [structural typing] [narrowing] [isinstance] [dependency inversion]

Q08. [Protocol та type narrowing]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Protocol та type narrowing», і як їх правильно пояснити?

Відповідь Protocols зручні для dependency inversion і test doubles. Але великий Protocol, який копіює весь concrete class, перетворюється на приховане tight coupling. Базова причина цих ефектів така: `Protocol` підтримує structural typing: object сумісний, якщо має потрібний interface, без обов’язкового nominal inheritance. Type narrowing уточнює union/object type після `isinstance`, перевірки `None` або інших guard conditions.

Senior Reasoning Senior-кандидат проектує маленькі capability-oriented protocols і локалізує dynamic type uncertainty біля boundaries. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- Protocol = structural contract
- inheritance не обов’язкове
- narrowing уточнює type
- малий protocol кращий за копію concrete API

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[Protocol] [structural typing] [narrowing] [isinstance] [dependency inversion]

Q09. [Protocol та type narrowing]

Питання Як би ви застосували знання про «Protocol та type narrowing» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `Protocol` підтримує structural typing: object сумісний, якщо має потрібний interface, без обов’язкового nominal inheritance. Type narrowing уточнює union/object type після `isinstance`, перевірки `None` або інших guard conditions. У design/debugging сценарії важливо зробити contract явним. Protocols зручні для dependency inversion і test doubles. Але великий Protocol, який копіює весь concrete class, перетворюється на приховане tight coupling.

Senior Reasoning Senior-кандидат проектує маленькі capability-oriented protocols і локалізує dynamic type uncertainty біля boundaries. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- Protocol = structural contract
- inheritance не обов’язкове
- narrowing уточнює type
- малий protocol кращий за копію concrete API

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[Protocol] [structural typing] [narrowing] [isinstance] [dependency inversion]
