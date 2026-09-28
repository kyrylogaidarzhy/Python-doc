Q01. [Reference counting та object lifetime у CPython]

Питання Поясніть механіку теми «Reference counting та object lifetime у CPython». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь На рівні мови точний момент collection не є універсальною гарантією. CPython традиційно використовує reference counting як основний механізм lifetime і доповнює його cyclic garbage collector для unreachable cycles. Object може жити довше очікуваного через cache, global registry, closure, traceback або інше strong reference. Зовнішні resources не слід закривати «коли GC прибере object» — потрібен explicit/context-managed cleanup.

Senior Reasoning Senior-кандидат має відділяти memory lifetime від resource lifetime і не переносити CPython implementation details у portable language contract без потреби.

Key Points
- CPython використовує refcounting
- цикли потребують cyclic GC
- strong reference продовжує lifetime
- resource cleanup має бути explicit

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Reference counting та object lifetime у CPython»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[reference counting] [lifetime] [CPython] [strong reference] [cleanup]

Q02. [Reference counting та object lifetime у CPython]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Reference counting та object lifetime у CPython», і як їх правильно пояснити?

Відповідь Object може жити довше очікуваного через cache, global registry, closure, traceback або інше strong reference. Зовнішні resources не слід закривати «коли GC прибере object» — потрібен explicit/context-managed cleanup. Базова причина цих ефектів така: На рівні мови точний момент collection не є універсальною гарантією. CPython традиційно використовує reference counting як основний механізм lifetime і доповнює його cyclic garbage collector для unreachable cycles.

Senior Reasoning Senior-кандидат має відділяти memory lifetime від resource lifetime і не переносити CPython implementation details у portable language contract без потреби. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- CPython використовує refcounting
- цикли потребують cyclic GC
- strong reference продовжує lifetime
- resource cleanup має бути explicit

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[reference counting] [lifetime] [CPython] [strong reference] [cleanup]

Q03. [Reference counting та object lifetime у CPython]

Питання Як би ви застосували знання про «Reference counting та object lifetime у CPython» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь На рівні мови точний момент collection не є універсальною гарантією. CPython традиційно використовує reference counting як основний механізм lifetime і доповнює його cyclic garbage collector для unreachable cycles. У design/debugging сценарії важливо зробити contract явним. Object може жити довше очікуваного через cache, global registry, closure, traceback або інше strong reference. Зовнішні resources не слід закривати «коли GC прибере object» — потрібен explicit/context-managed cleanup.

Senior Reasoning Senior-кандидат має відділяти memory lifetime від resource lifetime і не переносити CPython implementation details у portable language contract без потреби. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- CPython використовує refcounting
- цикли потребують cyclic GC
- strong reference продовжує lifetime
- resource cleanup має бути explicit

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[reference counting] [lifetime] [CPython] [strong reference] [cleanup]

Q04. [Cyclic GC, reachability та finalization]

Питання Поясніть механіку теми «Cyclic GC, reachability та finalization». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Reference counting сам по собі не звільняє cycle, якщо objects посилаються один на одного. Cyclic GC шукає unreachable cycles і може їх collect. Reachability, а не «кількість об’єктів у циклі», визначає, чи graph ще потрібен програмі. Cycles не завжди є memory leak: якщо вони unreachable, GC може їх прибрати. Реальний leak часто означає, що існує unintended live reference із root-like object, cache або registry.

Senior Reasoning Senior-рівень — діагностувати retention graph, а не автоматично звинувачувати cycles. Finalization logic у складних object graphs потребує обережності і не повинна замінювати explicit lifecycle management.

Key Points
- cycle ≠ leak автоматично
- unreachable cycles можуть collect-итися
- live root reference утримує graph
- finalization не замінює lifecycle

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Cyclic GC, reachability та finalization»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[cyclic GC] [reachability] [cycle] [finalization] [retention]

Q05. [Cyclic GC, reachability та finalization]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Cyclic GC, reachability та finalization», і як їх правильно пояснити?

Відповідь Cycles не завжди є memory leak: якщо вони unreachable, GC може їх прибрати. Реальний leak часто означає, що існує unintended live reference із root-like object, cache або registry. Базова причина цих ефектів така: Reference counting сам по собі не звільняє cycle, якщо objects посилаються один на одного. Cyclic GC шукає unreachable cycles і може їх collect. Reachability, а не «кількість об’єктів у циклі», визначає, чи graph ще потрібен програмі.

Senior Reasoning Senior-рівень — діагностувати retention graph, а не автоматично звинувачувати cycles. Finalization logic у складних object graphs потребує обережності і не повинна замінювати explicit lifecycle management. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- cycle ≠ leak автоматично
- unreachable cycles можуть collect-итися
- live root reference утримує graph
- finalization не замінює lifecycle

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[cyclic GC] [reachability] [cycle] [finalization] [retention]

Q06. [Cyclic GC, reachability та finalization]

Питання Як би ви застосували знання про «Cyclic GC, reachability та finalization» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Reference counting сам по собі не звільняє cycle, якщо objects посилаються один на одного. Cyclic GC шукає unreachable cycles і може їх collect. Reachability, а не «кількість об’єктів у циклі», визначає, чи graph ще потрібен програмі. У design/debugging сценарії важливо зробити contract явним. Cycles не завжди є memory leak: якщо вони unreachable, GC може їх прибрати. Реальний leak часто означає, що існує unintended live reference із root-like object, cache або registry.

Senior Reasoning Senior-рівень — діагностувати retention graph, а не автоматично звинувачувати cycles. Finalization logic у складних object graphs потребує обережності і не повинна замінювати explicit lifecycle management. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- cycle ≠ leak автоматично
- unreachable cycles можуть collect-итися
- live root reference утримує graph
- finalization не замінює lifecycle

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[cyclic GC] [reachability] [cycle] [finalization] [retention]

Q07. [weakref та пошук memory leaks]

Питання Поясніть механіку теми «weakref та пошук memory leaks». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Weak reference дозволяє посилатися на object, не утримуючи його alive. Це корисно для caches/registries, які не повинні бути owners. Для memory diagnostics у Python корисний `tracemalloc`, snapshots і порівняння allocation growth. Weakref не підходить як універсальна заміна ownership: object може зникнути, тому consumer має обробляти відсутність. Під час leak investigation важливо відрізняти високі allocations від реально retained memory.

Senior Reasoning Senior-кандидат формує гіпотезу, вимірює growth, знаходить retaining references і перевіряє fix повторним measurement, а не просто викликає `gc.collect()` у production loop.

Key Points
- weakref не володіє object
- cache може не продовжувати lifetime
- tracemalloc показує Python allocations
- leak debugging потребує before/after measurement

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «weakref та пошук memory leaks»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[weakref] [tracemalloc] [snapshot] [memory leak] [retention]

Q08. [weakref та пошук memory leaks]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «weakref та пошук memory leaks», і як їх правильно пояснити?

Відповідь Weakref не підходить як універсальна заміна ownership: object може зникнути, тому consumer має обробляти відсутність. Під час leak investigation важливо відрізняти високі allocations від реально retained memory. Базова причина цих ефектів така: Weak reference дозволяє посилатися на object, не утримуючи його alive. Це корисно для caches/registries, які не повинні бути owners. Для memory diagnostics у Python корисний `tracemalloc`, snapshots і порівняння allocation growth.

Senior Reasoning Senior-кандидат формує гіпотезу, вимірює growth, знаходить retaining references і перевіряє fix повторним measurement, а не просто викликає `gc.collect()` у production loop. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- weakref не володіє object
- cache може не продовжувати lifetime
- tracemalloc показує Python allocations
- leak debugging потребує before/after measurement

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[weakref] [tracemalloc] [snapshot] [memory leak] [retention]

Q09. [weakref та пошук memory leaks]

Питання Як би ви застосували знання про «weakref та пошук memory leaks» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Weak reference дозволяє посилатися на object, не утримуючи його alive. Це корисно для caches/registries, які не повинні бути owners. Для memory diagnostics у Python корисний `tracemalloc`, snapshots і порівняння allocation growth. У design/debugging сценарії важливо зробити contract явним. Weakref не підходить як універсальна заміна ownership: object може зникнути, тому consumer має обробляти відсутність. Під час leak investigation важливо відрізняти високі allocations від реально retained memory.

Senior Reasoning Senior-кандидат формує гіпотезу, вимірює growth, знаходить retaining references і перевіряє fix повторним measurement, а не просто викликає `gc.collect()` у production loop. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- weakref не володіє object
- cache може не продовжувати lifetime
- tracemalloc показує Python allocations
- leak debugging потребує before/after measurement

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[weakref] [tracemalloc] [snapshot] [memory leak] [retention]
