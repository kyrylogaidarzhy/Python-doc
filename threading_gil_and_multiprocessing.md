Q01. [Threads, GIL та workload selection]

Питання Поясніть механіку теми «Threads, GIL та workload selection». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь У звичайній GIL-enabled збірці CPython одночасне виконання Python bytecode в одному interpreter обмежене GIL. Threads все одно корисні для багатьох I/O-bound workloads, бо blocking I/O може звільняти GIL. Сучасний CPython також має free-threaded builds, тому GIL не можна вважати універсальною властивістю всіх збірок. Для CPU-bound pure Python workload threads часто не дають очікуваного parallel speedup у GIL-enabled build. Для I/O-bound requests threads можуть добре приховувати wait time. Рішення треба приймати за profile workload.

Senior Reasoning Senior-кандидат не використовує GIL як гарантію thread-safety application invariants. Навіть якщо окрема операція виглядає atomic у конкретній реалізації, multi-step check-then-act потребує synchronization.

Key Points
- GIL не скасовує користь threads для I/O
- CPU-bound pure Python має обмеження
- free-threaded builds існують
- GIL не замінює locks

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Threads, GIL та workload selection»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[threading] [GIL] [I/O-bound] [CPU-bound] [free-threaded]

Q02. [Threads, GIL та workload selection]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Threads, GIL та workload selection», і як їх правильно пояснити?

Відповідь Для CPU-bound pure Python workload threads часто не дають очікуваного parallel speedup у GIL-enabled build. Для I/O-bound requests threads можуть добре приховувати wait time. Рішення треба приймати за profile workload. Базова причина цих ефектів така: У звичайній GIL-enabled збірці CPython одночасне виконання Python bytecode в одному interpreter обмежене GIL. Threads все одно корисні для багатьох I/O-bound workloads, бо blocking I/O може звільняти GIL. Сучасний CPython також має free-threaded builds, тому GIL не можна вважати універсальною властивістю всіх збірок.

Senior Reasoning Senior-кандидат не використовує GIL як гарантію thread-safety application invariants. Навіть якщо окрема операція виглядає atomic у конкретній реалізації, multi-step check-then-act потребує synchronization. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- GIL не скасовує користь threads для I/O
- CPU-bound pure Python має обмеження
- free-threaded builds існують
- GIL не замінює locks

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[threading] [GIL] [I/O-bound] [CPU-bound] [free-threaded]

Q03. [Threads, GIL та workload selection]

Питання Як би ви застосували знання про «Threads, GIL та workload selection» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь У звичайній GIL-enabled збірці CPython одночасне виконання Python bytecode в одному interpreter обмежене GIL. Threads все одно корисні для багатьох I/O-bound workloads, бо blocking I/O може звільняти GIL. Сучасний CPython також має free-threaded builds, тому GIL не можна вважати універсальною властивістю всіх збірок. У design/debugging сценарії важливо зробити contract явним. Для CPU-bound pure Python workload threads часто не дають очікуваного parallel speedup у GIL-enabled build. Для I/O-bound requests threads можуть добре приховувати wait time. Рішення треба приймати за profile workload.

Senior Reasoning Senior-кандидат не використовує GIL як гарантію thread-safety application invariants. Навіть якщо окрема операція виглядає atomic у конкретній реалізації, multi-step check-then-act потребує synchronization. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- GIL не скасовує користь threads для I/O
- CPU-bound pure Python має обмеження
- free-threaded builds існують
- GIL не замінює locks

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[threading] [GIL] [I/O-bound] [CPU-bound] [free-threaded]

Q04. [Race conditions, locks та synchronization]

Питання Поясніть механіку теми «Race conditions, locks та synchronization». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Race condition виникає, коли результат залежить від interleaving concurrent operations над shared state. Locks, conditions, semaphores та thread-safe queues задають synchronization/coordination boundaries. Типовий баг — check-then-update без lock: два threads бачать однаковий old state і обидва виконують update. Надмірне locking може створити contention або deadlock.

Senior Reasoning Senior-рівень — мінімізувати shared mutable state, визначати lock ownership/order і розглядати message passing/queues там, де це спрощує reasoning.

Key Points
- race залежить від interleaving
- multi-step invariant потребує synchronization
- lock order важливий для deadlock
- менше shared state — простіше

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Race conditions, locks та synchronization»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[race condition] [lock] [deadlock] [synchronization] [shared state]

Q05. [Race conditions, locks та synchronization]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Race conditions, locks та synchronization», і як їх правильно пояснити?

Відповідь Типовий баг — check-then-update без lock: два threads бачать однаковий old state і обидва виконують update. Надмірне locking може створити contention або deadlock. Базова причина цих ефектів така: Race condition виникає, коли результат залежить від interleaving concurrent operations над shared state. Locks, conditions, semaphores та thread-safe queues задають synchronization/coordination boundaries.

Senior Reasoning Senior-рівень — мінімізувати shared mutable state, визначати lock ownership/order і розглядати message passing/queues там, де це спрощує reasoning. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- race залежить від interleaving
- multi-step invariant потребує synchronization
- lock order важливий для deadlock
- менше shared state — простіше

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[race condition] [lock] [deadlock] [synchronization] [shared state]

Q06. [Race conditions, locks та synchronization]

Питання Як би ви застосували знання про «Race conditions, locks та synchronization» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Race condition виникає, коли результат залежить від interleaving concurrent operations над shared state. Locks, conditions, semaphores та thread-safe queues задають synchronization/coordination boundaries. У design/debugging сценарії важливо зробити contract явним. Типовий баг — check-then-update без lock: два threads бачать однаковий old state і обидва виконують update. Надмірне locking може створити contention або deadlock.

Senior Reasoning Senior-рівень — мінімізувати shared mutable state, визначати lock ownership/order і розглядати message passing/queues там, де це спрощує reasoning. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- race залежить від interleaving
- multi-step invariant потребує synchronization
- lock order важливий для deadlock
- менше shared state — простіше

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[race condition] [lock] [deadlock] [synchronization] [shared state]

Q07. [Multiprocessing, IPC та process isolation]

Питання Поясніть механіку теми «Multiprocessing, IPC та process isolation». Який mental model має бути у розробника і які властивості є принциповими?

Відповідь Multiprocessing використовує окремі processes із власним address space, що дозволяє CPU parallelism поза одним GIL-enabled interpreter. Обмін даними потребує IPC, serialization/shared memory або manager abstractions. Process startup, serialization великих objects і copy-on-write/OS semantics можуть зробити multiprocessing дорожчим за очікування. Start method також впливає на inherited state і safety.

Senior Reasoning Senior-кандидат порівнює compute granularity з IPC overhead. Короткі дрібні tasks можуть програти через dispatch/serialization; довгі CPU-heavy tasks частіше окупають process cost.

Key Points
- processes мають isolation
- IPC має cost
- serialization може домінувати
- granularity визначає окупність multiprocessing

Follow-ups
1. Яку типову помилку ви очікуєте побачити в production-коді, якщо розробник неправильно розуміє «Multiprocessing, IPC та process isolation»?
2. Яку невелику перевірку або приклад ви б використали на співбесіді, щоб відрізнити механічне знання від реального розуміння?

Hint Triggers
[multiprocessing] [process] [IPC] [serialization] [start method]

Q08. [Multiprocessing, IPC та process isolation]

Питання Які типові помилки та неочевидні наслідки виникають у production-коді навколо «Multiprocessing, IPC та process isolation», і як їх правильно пояснити?

Відповідь Process startup, serialization великих objects і copy-on-write/OS semantics можуть зробити multiprocessing дорожчим за очікування. Start method також впливає на inherited state і safety. Базова причина цих ефектів така: Multiprocessing використовує окремі processes із власним address space, що дозволяє CPU parallelism поза одним GIL-enabled interpreter. Обмін даними потребує IPC, serialization/shared memory або manager abstractions.

Senior Reasoning Senior-кандидат порівнює compute granularity з IPC overhead. Короткі дрібні tasks можуть програти через dispatch/serialization; довгі CPU-heavy tasks частіше окупають process cost. На практиці відповідь має пов’язувати observed behavior із конкретним мовним contract, а не з випадковою деталлю одного прикладу.

Key Points
- processes мають isolation
- IPC має cost
- serialization може домінувати
- granularity визначає окупність multiprocessing

Follow-ups
1. Як би ви локалізували проблему, якщо вона проявляється лише під навантаженням або на частині даних?
2. Який design change зменшив би ймовірність повторення цієї помилки?

Hint Triggers
[multiprocessing] [process] [IPC] [serialization] [start method]

Q09. [Multiprocessing, IPC та process isolation]

Питання Як би ви застосували знання про «Multiprocessing, IPC та process isolation» під час проєктування API або розбору складного багу? Які trade-offs перевіряли б?

Відповідь Multiprocessing використовує окремі processes із власним address space, що дозволяє CPU parallelism поза одним GIL-enabled interpreter. Обмін даними потребує IPC, serialization/shared memory або manager abstractions. У design/debugging сценарії важливо зробити contract явним. Process startup, serialization великих objects і copy-on-write/OS semantics можуть зробити multiprocessing дорожчим за очікування. Start method також впливає на inherited state і safety.

Senior Reasoning Senior-кандидат порівнює compute granularity з IPC overhead. Короткі дрібні tasks можуть програти через dispatch/serialization; довгі CPU-heavy tasks частіше окупають process cost. Senior-відповідь повинна містити не лише правильну поведінку мови, а й наслідки для ownership, API boundaries, observability, performance або maintainability — залежно від контексту.

Key Points
- processes мають isolation
- IPC має cost
- serialization може домінувати
- granularity визначає окупність multiprocessing

Follow-ups
1. Які припущення в такому рішенні ви зробили б явними для команди?
2. За яких умов ви б обрали інший підхід, навіть якщо поточний технічно коректний?

Hint Triggers
[multiprocessing] [process] [IPC] [serialization] [start method]
