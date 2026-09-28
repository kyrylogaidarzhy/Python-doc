Q01. [try/except/else/finally та exception boundaries]

Питання Поясніть механіку теми «try/except/else/finally та exception boundaries». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `except` обробляє відповідні exception types; `else` виконується, якщо `try` завершився без exception; `finally` виконує cleanup незалежно від normal/exceptional path. Межа `try` має бути настільки вузькою, наскільки це практично. Широкий `except Exception` навколо великої функції може прихопити failure, який handler не вміє коректно обробити. `else` допомагає не включати зайву логіку до protected region.

Senior Reasoning Senior-рівень — проектувати exception boundaries за відповідальністю: recover там, де є достатній context; інакше translate або propagate. Не можна перетворювати всі failures на success-like return value без явної семантики.

Key Points
- ловити конкретні exceptions
- `else` звужує protected logic
- `finally` для cleanup
- boundary має мати context для recovery

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «try/except/else/finally та exception boundaries»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[try] [except] [else] [finally] [exception boundary]

Q02. [try/except/else/finally та exception boundaries]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «try/except/else/finally та exception boundaries», і як їх правильно пояснити?

Відповідь Широкий `except Exception` навколо великої функції може прихопити failure, який handler не вміє коректно обробити. `else` допомагає не включати зайву логіку до protected region. Базова причина цих ефектів така: `except` обробляє відповідні exception types; `else` виконується, якщо `try` завершився без exception; `finally` виконує cleanup незалежно від normal/exceptional path. Межа `try` має бути настільки вузькою, наскільки це практично.

Senior Reasoning Senior-рівень — проектувати exception boundaries за відповідальністю: recover там, де є достатній context; інакше translate або propagate. Не можна перетворювати всі failures на success-like return value без явної семантики. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- ловити конкретні exceptions
- `else` звужує protected logic
- `finally` для cleanup
- boundary має мати context для recovery

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[try] [except] [else] [finally] [exception boundary]

Q03. [try/except/else/finally та exception boundaries]

Питання Як би ви застосували знання про «try/except/else/finally та exception boundaries» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `except` обробляє відповідні exception types; `else` виконується, якщо `try` завершився без exception; `finally` виконує cleanup незалежно від normal/exceptional path. Межа `try` має бути настільки вузькою, наскільки це практично. У design/debugging сценарії важливо зробити contract явним. Широкий `except Exception` навколо великої функції може прихопити failure, який handler не вміє коректно обробити. `else` допомагає не включати зайву логіку до protected region.

Senior Reasoning Senior-рівень — проектувати exception boundaries за відповідальністю: recover там, де є достатній context; інакше translate або propagate. Не можна перетворювати всі failures на success-like return value без явної семантики. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- ловити конкретні exceptions
- `else` звужує protected logic
- `finally` для cleanup
- boundary має мати context для recovery

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[try] [except] [else] [finally] [exception boundary]

Q04. [Custom exceptions та chaining]

Питання Поясніть механіку теми «Custom exceptions та chaining». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Custom exception types дозволяють виразити domain failure semantics. `raise ... from ...` зберігає причинний зв’язок між низькорівневим exception і вищорівневим domain exception. При translation database/network error у domain error важливо не втратити original cause, інакше debugging стає складнішим. Exception hierarchy має бути достатньо стабільною, щоб callers могли ловити meaningful categories.

Senior Reasoning Senior-кандидат має відрізняти message text від machine-readable exception contract. Consumers не повинні парсити human-readable message, якщо можна мати окремий type/attribute.

Key Points
- exception type — частина contract
- chaining зберігає cause
- не парсити message як protocol
- translation має бути на boundary

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Custom exceptions та chaining»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[custom exception] [raise from] [cause] [context] [domain error]

Q05. [Custom exceptions та chaining]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Custom exceptions та chaining», і як їх правильно пояснити?

Відповідь При translation database/network error у domain error важливо не втратити original cause, інакше debugging стає складнішим. Exception hierarchy має бути достатньо стабільною, щоб callers могли ловити meaningful categories. Базова причина цих ефектів така: Custom exception types дозволяють виразити domain failure semantics. `raise ... from ...` зберігає причинний зв’язок між низькорівневим exception і вищорівневим domain exception.

Senior Reasoning Senior-кандидат має відрізняти message text від machine-readable exception contract. Consumers не повинні парсити human-readable message, якщо можна мати окремий type/attribute. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- exception type — частина contract
- chaining зберігає cause
- не парсити message як protocol
- translation має бути на boundary

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[custom exception] [raise from] [cause] [context] [domain error]

Q06. [Custom exceptions та chaining]

Питання Як би ви застосували знання про «Custom exceptions та chaining» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Custom exception types дозволяють виразити domain failure semantics. `raise ... from ...` зберігає причинний зв’язок між низькорівневим exception і вищорівневим domain exception. У design/debugging сценарії важливо зробити contract явним. При translation database/network error у domain error важливо не втратити original cause, інакше debugging стає складнішим. Exception hierarchy має бути достатньо стабільною, щоб callers могли ловити meaningful categories.

Senior Reasoning Senior-кандидат має відрізняти message text від machine-readable exception contract. Consumers не повинні парсити human-readable message, якщо можна мати окремий type/attribute. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- exception type — частина contract
- chaining зберігає cause
- не парсити message як protocol
- translation має бути на boundary

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[custom exception] [raise from] [cause] [context] [domain error]

Q07. [Context managers та with]

Питання Поясніть механіку теми «Context managers та with». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Context manager пов’язує acquisition і release ресурсу з lexical scope через `__enter__`/`__exit__` або відповідні helper abstractions. `with` гарантує виконання exit logic після успішного входу, навіть коли всередині виникає exception. Файли, locks, database transactions і тимчасові ресурси краще оформляти context manager-ами, ніж покладатися на GC/finalizer. `__exit__` може suppress exception, але це має бути навмисна частина contract.

Senior Reasoning Senior-рівень — розуміти ownership і nested resource cleanup. Context manager — це не лише convenience syntax, а спосіб зробити lifecycle deterministic і локальним.

Key Points
- with задає lifecycle boundary
- cleanup deterministic
- `__exit__` бачить exception
- suppression має бути свідомим

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Context managers та with»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[context manager] [with] [__enter__] [__exit__] [resource cleanup]

Q08. [Context managers та with]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Context managers та with», і як їх правильно пояснити?

Відповідь Файли, locks, database transactions і тимчасові ресурси краще оформляти context manager-ами, ніж покладатися на GC/finalizer. `__exit__` може suppress exception, але це має бути навмисна частина contract. Базова причина цих ефектів така: Context manager пов’язує acquisition і release ресурсу з lexical scope через `__enter__`/`__exit__` або відповідні helper abstractions. `with` гарантує виконання exit logic після успішного входу, навіть коли всередині виникає exception.

Senior Reasoning Senior-рівень — розуміти ownership і nested resource cleanup. Context manager — це не лише convenience syntax, а спосіб зробити lifecycle deterministic і локальним. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- with задає lifecycle boundary
- cleanup deterministic
- `__exit__` бачить exception
- suppression має бути свідомим

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[context manager] [with] [__enter__] [__exit__] [resource cleanup]

Q09. [Context managers та with]

Питання Як би ви застосували знання про «Context managers та with» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Context manager пов’язує acquisition і release ресурсу з lexical scope через `__enter__`/`__exit__` або відповідні helper abstractions. `with` гарантує виконання exit logic після успішного входу, навіть коли всередині виникає exception. У design/debugging сценарії важливо зробити contract явним. Файли, locks, database transactions і тимчасові ресурси краще оформляти context manager-ами, ніж покладатися на GC/finalizer. `__exit__` може suppress exception, але це має бути навмисна частина contract.

Senior Reasoning Senior-рівень — розуміти ownership і nested resource cleanup. Context manager — це не лише convenience syntax, а спосіб зробити lifecycle deterministic і локальним. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- with задає lifecycle boundary
- cleanup deterministic
- `__exit__` бачить exception
- suppression має бути свідомим

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[context manager] [with] [__enter__] [__exit__] [resource cleanup]
