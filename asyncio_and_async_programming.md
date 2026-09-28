Q01. [Event loop, coroutine, Task та Future]

Питання Поясніть механіку теми «Event loop, coroutine, Task та Future». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Виклик `async def` створює coroutine object. Coroutine виконується, коли її await-ять або schedule-ять. `Task` планує coroutine в event loop і представляє її lifecycle/result; `Future` є нижчорівневим awaitable-представленням майбутнього результату. Створити coroutine object недостатньо — без await/scheduling вона не виконає роботу. Fire-and-forget tasks без ownership можуть загубити exceptions і ускладнити shutdown.

Senior Reasoning Senior-рівень — завжди відповісти, хто володіє task, хто чекає completion, хто обробляє failure і хто скасовує її при shutdown.

Key Points
- coroutine object ≠ running task
- Task schedule-ить coroutine
- Future представляє future result
- event loop координує cooperative concurrency

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Event loop, coroutine, Task та Future»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[event loop] [coroutine] [Task] [Future] [await]

Q02. [Event loop, coroutine, Task та Future]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Event loop, coroutine, Task та Future», і як їх правильно пояснити?

Відповідь Створити coroutine object недостатньо — без await/scheduling вона не виконає роботу. Fire-and-forget tasks без ownership можуть загубити exceptions і ускладнити shutdown. Базова причина цих ефектів така: Виклик `async def` створює coroutine object. Coroutine виконується, коли її await-ять або schedule-ять. `Task` планує coroutine в event loop і представляє її lifecycle/result; `Future` є нижчорівневим awaitable-представленням майбутнього результату.

Senior Reasoning Senior-рівень — завжди відповісти, хто володіє task, хто чекає completion, хто обробляє failure і хто скасовує її при shutdown. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- coroutine object ≠ running task
- Task schedule-ить coroutine
- Future представляє future result
- event loop координує cooperative concurrency

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[event loop] [coroutine] [Task] [Future] [await]

Q03. [Event loop, coroutine, Task та Future]

Питання Як би ви застосували знання про «Event loop, coroutine, Task та Future» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Виклик `async def` створює coroutine object. Coroutine виконується, коли її await-ять або schedule-ять. `Task` планує coroutine в event loop і представляє її lifecycle/result; `Future` є нижчорівневим awaitable-представленням майбутнього результату. У design/debugging сценарії важливо зробити contract явним. Створити coroutine object недостатньо — без await/scheduling вона не виконає роботу. Fire-and-forget tasks без ownership можуть загубити exceptions і ускладнити shutdown.

Senior Reasoning Senior-рівень — завжди відповісти, хто володіє task, хто чекає completion, хто обробляє failure і хто скасовує її при shutdown. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- coroutine object ≠ running task
- Task schedule-ить coroutine
- Future представляє future result
- event loop координує cooperative concurrency

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[event loop] [coroutine] [Task] [Future] [await]

Q04. [Cancellation, timeout та TaskGroup]

Питання Поясніть механіку теми «Cancellation, timeout та TaskGroup». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Cancellation у asyncio є cooperative: `Task.cancel()` організовує доставку `CancelledError` у coroutine на await boundary. `asyncio.timeout()` використовує cancellation і перетворює її на `TimeoutError` на boundary context manager-а. `TaskGroup` дає structured concurrency та координує lifecycle дочірніх tasks. `CancelledError` зазвичай треба пропустити далі після cleanup; бездумне swallowing може зламати structured concurrency. Timeout — це не просто «повернути false», а контроль lifecycle виконуваної роботи.

Senior Reasoning Senior-кандидат проектує cancellation-safe cleanup, не залишає orphan tasks і розуміє failure propagation у group scope.

Key Points
- cancellation cooperative
- cleanup перед propagation
- timeout використовує cancellation internally
- TaskGroup структурує lifecycle

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Cancellation, timeout та TaskGroup»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[CancelledError] [timeout] [TaskGroup] [structured concurrency] [cancellation]

Q05. [Cancellation, timeout та TaskGroup]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Cancellation, timeout та TaskGroup», і як їх правильно пояснити?

Відповідь `CancelledError` зазвичай треба пропустити далі після cleanup; бездумне swallowing може зламати structured concurrency. Timeout — це не просто «повернути false», а контроль lifecycle виконуваної роботи. Базова причина цих ефектів така: Cancellation у asyncio є cooperative: `Task.cancel()` організовує доставку `CancelledError` у coroutine на await boundary. `asyncio.timeout()` використовує cancellation і перетворює її на `TimeoutError` на boundary context manager-а. `TaskGroup` дає structured concurrency та координує lifecycle дочірніх tasks.

Senior Reasoning Senior-кандидат проектує cancellation-safe cleanup, не залишає orphan tasks і розуміє failure propagation у group scope. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- cancellation cooperative
- cleanup перед propagation
- timeout використовує cancellation internally
- TaskGroup структурує lifecycle

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[CancelledError] [timeout] [TaskGroup] [structured concurrency] [cancellation]

Q06. [Cancellation, timeout та TaskGroup]

Питання Як би ви застосували знання про «Cancellation, timeout та TaskGroup» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Cancellation у asyncio є cooperative: `Task.cancel()` організовує доставку `CancelledError` у coroutine на await boundary. `asyncio.timeout()` використовує cancellation і перетворює її на `TimeoutError` на boundary context manager-а. `TaskGroup` дає structured concurrency та координує lifecycle дочірніх tasks. У design/debugging сценарії важливо зробити contract явним. `CancelledError` зазвичай треба пропустити далі після cleanup; бездумне swallowing може зламати structured concurrency. Timeout — це не просто «повернути false», а контроль lifecycle виконуваної роботи.

Senior Reasoning Senior-кандидат проектує cancellation-safe cleanup, не залишає orphan tasks і розуміє failure propagation у group scope. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- cancellation cooperative
- cleanup перед propagation
- timeout використовує cancellation internally
- TaskGroup структурує lifecycle

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[CancelledError] [timeout] [TaskGroup] [structured concurrency] [cancellation]

Q07. [Blocking calls, backpressure та concurrency limits]

Питання Поясніть механіку теми «Blocking calls, backpressure та concurrency limits». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Event loop працює ефективно, коли tasks регулярно віддають керування. Blocking CPU або synchronous I/O всередині event loop затримує всі інші tasks. Concurrency limit/backpressure потрібні, щоб producer не створював необмежену кількість роботи швидше, ніж downstream здатний обробити. Blocking роботу слід винести в thread/process/external service залежно від nature workload. Semaphores/queues/bounded worker pools допомагають обмежити parallel requests і memory growth.

Senior Reasoning Senior-рівень — мислити не «async = швидше», а latency/throughput/resource limits. Без backpressure async service може впасти від memory/connection exhaustion при високому load.

Key Points
- blocking call зупиняє event loop
- async не робить CPU work дешевшим
- backpressure обмежує inflight work
- bounded concurrency захищає dependencies

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Blocking calls, backpressure та concurrency limits»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[blocking] [backpressure] [Semaphore] [Queue] [concurrency limit]

Q08. [Blocking calls, backpressure та concurrency limits]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Blocking calls, backpressure та concurrency limits», і як їх правильно пояснити?

Відповідь Blocking роботу слід винести в thread/process/external service залежно від nature workload. Semaphores/queues/bounded worker pools допомагають обмежити parallel requests і memory growth. Базова причина цих ефектів така: Event loop працює ефективно, коли tasks регулярно віддають керування. Blocking CPU або synchronous I/O всередині event loop затримує всі інші tasks. Concurrency limit/backpressure потрібні, щоб producer не створював необмежену кількість роботи швидше, ніж downstream здатний обробити.

Senior Reasoning Senior-рівень — мислити не «async = швидше», а latency/throughput/resource limits. Без backpressure async service може впасти від memory/connection exhaustion при високому load. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- blocking call зупиняє event loop
- async не робить CPU work дешевшим
- backpressure обмежує inflight work
- bounded concurrency захищає dependencies

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[blocking] [backpressure] [Semaphore] [Queue] [concurrency limit]

Q09. [Blocking calls, backpressure та concurrency limits]

Питання Як би ви застосували знання про «Blocking calls, backpressure та concurrency limits» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Event loop працює ефективно, коли tasks регулярно віддають керування. Blocking CPU або synchronous I/O всередині event loop затримує всі інші tasks. Concurrency limit/backpressure потрібні, щоб producer не створював необмежену кількість роботи швидше, ніж downstream здатний обробити. У design/debugging сценарії важливо зробити contract явним. Blocking роботу слід винести в thread/process/external service залежно від nature workload. Semaphores/queues/bounded worker pools допомагають обмежити parallel requests і memory growth.

Senior Reasoning Senior-рівень — мислити не «async = швидше», а latency/throughput/resource limits. Без backpressure async service може впасти від memory/connection exhaustion при високому load. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- blocking call зупиняє event loop
- async не робить CPU work дешевшим
- backpressure обмежує inflight work
- bounded concurrency захищає dependencies

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[blocking] [backpressure] [Semaphore] [Queue] [concurrency limit]
