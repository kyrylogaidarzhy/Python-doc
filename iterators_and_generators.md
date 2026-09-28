Q01. [Iterable, iterator та protocol]

Питання Поясніть механіку теми «Iterable, iterator та protocol». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Iterable — object, з якого можна отримати iterator. Iterator має traversal state, повертає наступні значення через iterator protocol і сигналізує завершення `StopIteration`. `for` працює з protocol, а не вимагає list або indexing. Container на кшталт list зазвичай може створювати новий iterator для кожного проходу. Сам iterator є stateful і після exhaustion зазвичай не перемотується. Приховане споживання iterator під час logging або validation може залишити downstream-код без даних.

Senior Reasoning API має чітко визначати, що він повертає: reusable collection, iterable factory чи one-shot iterator. Це впливає на повторне читання, memory і debugging.

Key Points
- iterable створює iterator
- iterator має state
- StopIteration завершує iteration
- iterator часто one-shot

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Iterable, iterator та protocol»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[iterable] [iterator] [iter] [next] [StopIteration]

Q02. [Iterable, iterator та protocol]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Iterable, iterator та protocol», і як їх правильно пояснити?

Відповідь Container на кшталт list зазвичай може створювати новий iterator для кожного проходу. Сам iterator є stateful і після exhaustion зазвичай не перемотується. Приховане споживання iterator під час logging або validation може залишити downstream-код без даних. Базова причина цих ефектів така: Iterable — object, з якого можна отримати iterator. Iterator має traversal state, повертає наступні значення через iterator protocol і сигналізує завершення `StopIteration`. `for` працює з protocol, а не вимагає list або indexing.

Senior Reasoning API має чітко визначати, що він повертає: reusable collection, iterable factory чи one-shot iterator. Це впливає на повторне читання, memory і debugging. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- iterable створює iterator
- iterator має state
- StopIteration завершує iteration
- iterator часто one-shot

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[iterable] [iterator] [iter] [next] [StopIteration]

Q03. [Iterable, iterator та protocol]

Питання Як би ви застосували знання про «Iterable, iterator та protocol» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Iterable — object, з якого можна отримати iterator. Iterator має traversal state, повертає наступні значення через iterator protocol і сигналізує завершення `StopIteration`. `for` працює з protocol, а не вимагає list або indexing. У design/debugging сценарії важливо зробити contract явним. Container на кшталт list зазвичай може створювати новий iterator для кожного проходу. Сам iterator є stateful і після exhaustion зазвичай не перемотується. Приховане споживання iterator під час logging або validation може залишити downstream-код без даних.

Senior Reasoning API має чітко визначати, що він повертає: reusable collection, iterable factory чи one-shot iterator. Це впливає на повторне читання, memory і debugging. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- iterable створює iterator
- iterator має state
- StopIteration завершує iteration
- iterator часто one-shot

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[iterable] [iterator] [iter] [next] [StopIteration]

Q04. [Generators, yield та suspension]

Питання Поясніть механіку теми «Generators, yield та suspension». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Function із `yield` створює generator object. Виконання тіла призупиняється на `yield` і продовжується з того самого execution state при наступному `next()`/iteration. Це дозволяє lazy вироблення значень. Generator може обробляти великий stream без materialization всієї колекції. Але resources, exceptions і cleanup треба проектувати з урахуванням того, що execution розтягнута в часі і generator може бути спожитий не до кінця.

Senior Reasoning Senior-рівень — розуміти lifecycle generator-а, exception propagation і cleanup boundary. Lazy pipeline зменшує peak memory, але може перенести помилку на пізніший момент і ускладнити observability.

Key Points
- `yield` suspend/resume
- generator є iterator
- lazy evaluation економить memory
- execution errors можуть бути deferred

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Generators, yield та suspension»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[generator] [yield] [lazy] [suspension] [generator state]

Q05. [Generators, yield та suspension]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Generators, yield та suspension», і як їх правильно пояснити?

Відповідь Generator може обробляти великий stream без materialization всієї колекції. Але resources, exceptions і cleanup треба проектувати з урахуванням того, що execution розтягнута в часі і generator може бути спожитий не до кінця. Базова причина цих ефектів така: Function із `yield` створює generator object. Виконання тіла призупиняється на `yield` і продовжується з того самого execution state при наступному `next()`/iteration. Це дозволяє lazy вироблення значень.

Senior Reasoning Senior-рівень — розуміти lifecycle generator-а, exception propagation і cleanup boundary. Lazy pipeline зменшує peak memory, але може перенести помилку на пізніший момент і ускладнити observability. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- `yield` suspend/resume
- generator є iterator
- lazy evaluation економить memory
- execution errors можуть бути deferred

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[generator] [yield] [lazy] [suspension] [generator state]

Q06. [Generators, yield та suspension]

Питання Як би ви застосували знання про «Generators, yield та suspension» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Function із `yield` створює generator object. Виконання тіла призупиняється на `yield` і продовжується з того самого execution state при наступному `next()`/iteration. Це дозволяє lazy вироблення значень. У design/debugging сценарії важливо зробити contract явним. Generator може обробляти великий stream без materialization всієї колекції. Але resources, exceptions і cleanup треба проектувати з урахуванням того, що execution розтягнута в часі і generator може бути спожитий не до кінця.

Senior Reasoning Senior-рівень — розуміти lifecycle generator-а, exception propagation і cleanup boundary. Lazy pipeline зменшує peak memory, але може перенести помилку на пізніший момент і ускладнити observability. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- `yield` suspend/resume
- generator є iterator
- lazy evaluation економить memory
- execution errors можуть бути deferred

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[generator] [yield] [lazy] [suspension] [generator state]

Q07. [yield from, delegation та lazy pipelines]

Питання Поясніть механіку теми «yield from, delegation та lazy pipelines». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь `yield from` делегує iteration іншому iterable/generator і передає значення через зовнішній generator. Це спрощує композицію generator-ів і частину handling `send/throw/close` semantics. У data pipeline delegation допомагає уникати ручного nested loop boilerplate. Водночас chain із багатьох lazy stages може приховувати, де фактично виконується робота і де виникає exception.

Senior Reasoning Senior-кандидат має оцінювати backpressure-like behavior, consumption rate і boundary materialization. Іноді `list()` у контрольованій точці є правильним рішенням, якщо потрібні retry, random access або багато проходів.

Key Points
- yield from делегує iteration
- lazy stages виконуються при consumption
- materialization має memory cost
- one-shot semantics впливають на retry

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «yield from, delegation та lazy pipelines»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[yield from] [delegation] [pipeline] [materialization] [consumption]

Q08. [yield from, delegation та lazy pipelines]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «yield from, delegation та lazy pipelines», і як їх правильно пояснити?

Відповідь У data pipeline delegation допомагає уникати ручного nested loop boilerplate. Водночас chain із багатьох lazy stages може приховувати, де фактично виконується робота і де виникає exception. Базова причина цих ефектів така: `yield from` делегує iteration іншому iterable/generator і передає значення через зовнішній generator. Це спрощує композицію generator-ів і частину handling `send/throw/close` semantics.

Senior Reasoning Senior-кандидат має оцінювати backpressure-like behavior, consumption rate і boundary materialization. Іноді `list()` у контрольованій точці є правильним рішенням, якщо потрібні retry, random access або багато проходів. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- yield from делегує iteration
- lazy stages виконуються при consumption
- materialization має memory cost
- one-shot semantics впливають на retry

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[yield from] [delegation] [pipeline] [materialization] [consumption]

Q09. [yield from, delegation та lazy pipelines]

Питання Як би ви застосували знання про «yield from, delegation та lazy pipelines» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь `yield from` делегує iteration іншому iterable/generator і передає значення через зовнішній generator. Це спрощує композицію generator-ів і частину handling `send/throw/close` semantics. У design/debugging сценарії важливо зробити contract явним. У data pipeline delegation допомагає уникати ручного nested loop boilerplate. Водночас chain із багатьох lazy stages може приховувати, де фактично виконується робота і де виникає exception.

Senior Reasoning Senior-кандидат має оцінювати backpressure-like behavior, consumption rate і boundary materialization. Іноді `list()` у контрольованій точці є правильним рішенням, якщо потрібні retry, random access або багато проходів. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- yield from делегує iteration
- lazy stages виконуються при consumption
- materialization має memory cost
- one-shot semantics впливають на retry

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[yield from] [delegation] [pipeline] [materialization] [consumption]
