Q01. [Вибір list/tuple/dict/set]

Питання Поясніть механіку теми «Вибір list/tuple/dict/set». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `list` моделює mutable ordered sequence, `tuple` — фіксовану послідовність references, `dict` — mapping від hashable keys до values, `set` — множину унікальних hashable elements. Вибір структури має випливати з інваріантів і домінантних операцій, а не із синтаксичної зручності. Для membership checks по великій колекції `set` зазвичай кращий за linear scan list; для key→value lookup природний `dict`; для ordered mutable sequence — `list`. `dict` зберігає insertion order, але це не робить його заміною sequence, якщо семантично потрібен mapping.

Senior Reasoning Senior-кандидат має говорити не лише про Big-O, а й про memory overhead, ordering guarantees, hashability, duplicate semantics, readability і workload distribution. Правильна структура даних спрощує код і часто дає більший performance win, ніж локальні micro-optimizations.

Key Points
- структура обирається за інваріантами
- set — uniqueness/membership
- dict — mapping
- list/tuple — sequence semantics

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Вибір list/tuple/dict/set»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[list] [tuple] [dict] [set] [complexity]

Q02. [Вибір list/tuple/dict/set]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Вибір list/tuple/dict/set», і як їх правильно пояснити?

Відповідь Для membership checks по великій колекції `set` зазвичай кращий за linear scan list; для key→value lookup природний `dict`; для ordered mutable sequence — `list`. `dict` зберігає insertion order, але це не робить його заміною sequence, якщо семантично потрібен mapping. Базова причина цих ефектів така: `list` моделює mutable ordered sequence, `tuple` — фіксовану послідовність references, `dict` — mapping від hashable keys до values, `set` — множину унікальних hashable elements. Вибір структури має випливати з інваріантів і домінантних операцій, а не із синтаксичної зручності.

Senior Reasoning Senior-кандидат має говорити не лише про Big-O, а й про memory overhead, ordering guarantees, hashability, duplicate semantics, readability і workload distribution. Правильна структура даних спрощує код і часто дає більший performance win, ніж локальні micro-optimizations. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- структура обирається за інваріантами
- set — uniqueness/membership
- dict — mapping
- list/tuple — sequence semantics

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[list] [tuple] [dict] [set] [complexity]

Q03. [Вибір list/tuple/dict/set]

Питання Як би ви застосували знання про «Вибір list/tuple/dict/set» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `list` моделює mutable ordered sequence, `tuple` — фіксовану послідовність references, `dict` — mapping від hashable keys до values, `set` — множину унікальних hashable elements. Вибір структури має випливати з інваріантів і домінантних операцій, а не із синтаксичної зручності. У design/debugging сценарії важливо зробити contract явним. Для membership checks по великій колекції `set` зазвичай кращий за linear scan list; для key→value lookup природний `dict`; для ordered mutable sequence — `list`. `dict` зберігає insertion order, але це не робить його заміною sequence, якщо семантично потрібен mapping.

Senior Reasoning Senior-кандидат має говорити не лише про Big-O, а й про memory overhead, ordering guarantees, hashability, duplicate semantics, readability і workload distribution. Правильна структура даних спрощує код і часто дає більший performance win, ніж локальні micro-optimizations. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- структура обирається за інваріантами
- set — uniqueness/membership
- dict — mapping
- list/tuple — sequence semantics

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[list] [tuple] [dict] [set] [complexity]

Q04. [Hashability, __eq__ та __hash__]

Питання Поясніть механіку теми «Hashability, __eq__ та __hash__». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Hashable object має hash value, який залишається стабільним протягом його життя, і може брати участь у `dict`/`set`. Якщо два об’єкти рівні за `==`, вони повинні мати однаковий hash. Порушення цього contract робить lookup некоректним або непередбачуваним. Mutable value-based object небезпечно використовувати як key: якщо поля, що впливають на equality/hash, зміняться після вставки, object може опинитися не в тому bucket. Користувацькі класи мають узгоджувати `__eq__` та `__hash__`.

Senior Reasoning Senior-рівень — розуміти, що hash не є унікальним ID і collisions нормальні. Hash table завжди повинна перевіряти equality після збігу/пошуку кандидатів. Також важливо відрізняти semantic identity від hash-based indexing.

Key Points
- equal objects → same hash
- hash може мати collisions
- hash має бути стабільним
- mutable value-key — ризик

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Hashability, __eq__ та __hash__»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[hashable] [__hash__] [__eq__] [collision] [dict key]

Q05. [Hashability, __eq__ та __hash__]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Hashability, __eq__ та __hash__», і як їх правильно пояснити?

Відповідь Mutable value-based object небезпечно використовувати як key: якщо поля, що впливають на equality/hash, зміняться після вставки, object може опинитися не в тому bucket. Користувацькі класи мають узгоджувати `__eq__` та `__hash__`. Базова причина цих ефектів така: Hashable object має hash value, який залишається стабільним протягом його життя, і може брати участь у `dict`/`set`. Якщо два об’єкти рівні за `==`, вони повинні мати однаковий hash. Порушення цього contract робить lookup некоректним або непередбачуваним.

Senior Reasoning Senior-рівень — розуміти, що hash не є унікальним ID і collisions нормальні. Hash table завжди повинна перевіряти equality після збігу/пошуку кандидатів. Також важливо відрізняти semantic identity від hash-based indexing. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- equal objects → same hash
- hash може мати collisions
- hash має бути стабільним
- mutable value-key — ризик

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[hashable] [__hash__] [__eq__] [collision] [dict key]

Q06. [Hashability, __eq__ та __hash__]

Питання Як би ви застосували знання про «Hashability, __eq__ та __hash__» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Hashable object має hash value, який залишається стабільним протягом його життя, і може брати участь у `dict`/`set`. Якщо два об’єкти рівні за `==`, вони повинні мати однаковий hash. Порушення цього contract робить lookup некоректним або непередбачуваним. У design/debugging сценарії важливо зробити contract явним. Mutable value-based object небезпечно використовувати як key: якщо поля, що впливають на equality/hash, зміняться після вставки, object може опинитися не в тому bucket. Користувацькі класи мають узгоджувати `__eq__` та `__hash__`.

Senior Reasoning Senior-рівень — розуміти, що hash не є унікальним ID і collisions нормальні. Hash table завжди повинна перевіряти equality після збігу/пошуку кандидатів. Також важливо відрізняти semantic identity від hash-based indexing. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- equal objects → same hash
- hash може мати collisions
- hash має бути стабільним
- mutable value-key — ризик

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[hashable] [__hash__] [__eq__] [collision] [dict key]

Q07. [Hash table behavior, complexity та ordering]

Питання Поясніть механіку теми «Hash table behavior, complexity та ordering». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `dict` і `set` реалізують hash-based lookup із середньою близькою до O(1) вартістю membership/get/insert, але це не математична гарантія для будь-якого input. Collisions і resize впливають на реальну поведінку. `dict` гарантує insertion order на рівні мови. Небезпечно робити performance-висновки лише з asymptotic complexity: великий hash object, поганий `__hash__`, часті allocations або adversarial workload можуть змінити картину. Для ordered operations треба явно розуміти, що порядок dict — insertion order, не sorted order.

Senior Reasoning Senior-кандидат має пояснювати amortized behavior і workload sensitivity. Якщо потрібні range queries або sorted traversal, hash table може бути неправильною структурою, навіть якщо точковий lookup дуже швидкий.

Key Points
- average O(1) не означає worst-case O(1)
- collisions обробляються таблицею
- dict order = insertion order
- resize має amortized cost

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Hash table behavior, complexity та ordering»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[hash table] [collision] [amortized] [insertion order] [resize]

Q08. [Hash table behavior, complexity та ordering]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Hash table behavior, complexity та ordering», і як їх правильно пояснити?

Відповідь Небезпечно робити performance-висновки лише з asymptotic complexity: великий hash object, поганий `__hash__`, часті allocations або adversarial workload можуть змінити картину. Для ordered operations треба явно розуміти, що порядок dict — insertion order, не sorted order. Базова причина цих ефектів така: `dict` і `set` реалізують hash-based lookup із середньою близькою до O(1) вартістю membership/get/insert, але це не математична гарантія для будь-якого input. Collisions і resize впливають на реальну поведінку. `dict` гарантує insertion order на рівні мови.

Senior Reasoning Senior-кандидат має пояснювати amortized behavior і workload sensitivity. Якщо потрібні range queries або sorted traversal, hash table може бути неправильною структурою, навіть якщо точковий lookup дуже швидкий. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- average O(1) не означає worst-case O(1)
- collisions обробляються таблицею
- dict order = insertion order
- resize має amortized cost

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[hash table] [collision] [amortized] [insertion order] [resize]

Q09. [Hash table behavior, complexity та ordering]

Питання Як би ви застосували знання про «Hash table behavior, complexity та ordering» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `dict` і `set` реалізують hash-based lookup із середньою близькою до O(1) вартістю membership/get/insert, але це не математична гарантія для будь-якого input. Collisions і resize впливають на реальну поведінку. `dict` гарантує insertion order на рівні мови. У design/debugging сценарії важливо зробити contract явним. Небезпечно робити performance-висновки лише з asymptotic complexity: великий hash object, поганий `__hash__`, часті allocations або adversarial workload можуть змінити картину. Для ordered operations треба явно розуміти, що порядок dict — insertion order, не sorted order.

Senior Reasoning Senior-кандидат має пояснювати amortized behavior і workload sensitivity. Якщо потрібні range queries або sorted traversal, hash table може бути неправильною структурою, навіть якщо точковий lookup дуже швидкий. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- average O(1) не означає worst-case O(1)
- collisions обробляються таблицею
- dict order = insertion order
- resize має amortized cost

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[hash table] [collision] [amortized] [insertion order] [resize]
