## Q1
- Prediction / 预测:总耗时1000，两个进程交替调度执行。
- Reasoning / 理由:两个进程都是纯CPU任务，没有IO操作，每条指令耗时100，合计10条CPU指令。每当一个进程执行完毕，操作系统切换另一个进程运行。
- Verified result / 验证结果:Total time: 10
- Analysis / 分析:两个纯CPU进程，进程执行结束才切换，无IO，总时间等于所有CPU指令时间之和。

## Q2
- Prediction / 预测:PID0发起IO之后让出CPU，PID1利用IO等待时间运行；总运行时间会小于800。
- Reasoning / 理由:PID0全部是IO操作，发起IO触发上下文切换，CPU此时调度PID1执行，实现IO等待和CPU计算并行。
- Verified result / 验证结果:Total time: 19
- Analysis / 分析:进程发起IO触发切换，另一个进程可以在IO等待期间运行，CPU‑IO实现重叠，总耗时比串行小

## Q3
- Prediction / 预测:进程运行2条CPU后发起IO，CPU空闲等待IO完成，之后继续执行剩余指令；总耗时大于600。
- Reasoning / 理由:系统只有单个进程，进程发起IO之后没有其他进程可以调度，CPU无事可做，必须等待IO结束，无法并行。
- Verified result / 验证结果:Total time: 16
- Analysis / 分析:三个进程交替执行，CPU在IO阻塞时调度就绪进程，IO与CPU计算并行执行，缩短整体完成时间。

## Q4
- Prediction / 预测:进程交替IO与CPU，每次发起IO，CPU空闲等待IO结束，不能并行，总耗时为各段时间相加。
- Reasoning / 理由:只有一个进程，每次发起IO，没有别的进程占用CPU，CPU只能等待IO完成，CPU和IO无法重叠。
- Verified result / 验证结果:Total time: 14
- Analysis / 分析:PID0执行IO时进入阻塞，CPU调度PID1运行；IO操作与CPU计算可以并行，整体耗时小于两个进程串行执行的时间。

## Q5
- Prediction / 预测:切换为SWITCH_ON_END策略后，IO阻塞期间CPU不会调度其他进程，CPU空闲，总运行时间相比默认策略会大幅增加。
- Reasoning / 理由:SWITCH_ON_END仅当进程全部执行完毕才发生上下文切换。进程发起IO阻塞时不会让出CPU，其他就绪进程得不到运行机会，无法利用IO等待时间。
- Analysis / 分析:PID0和PID2会交替发起IO请求；当一个进程IO阻塞时CPU调度另一个进程运行，IO设备可同时处理多个IO请求，实现CPU‑IO并发执行。

## Q6
- Prediction / 预测:调度策略修改为进程结束才切换，Process1会完整跑完，IO期间CPU空闲；无法和Process2并行，总时间变长。
- Reasoning / 理由:SWITCH_ON_END模式，就算发起IO也不切换，必须等整个进程结束；IO等待时CPU无事可做，不能跑另一个进程。
- Verified result / 验证结果:Total time: 39
- Analysis / 分析:三个进程轮流执行IO操作，大量时间进程处于BLOCKED阻塞状态；CPU在IO完成后调度就绪进程运行，IO设备并行处理多路IO请求。

## Q7
- Prediction / 预测:Process1执行完3条CPU发起IO，触发切换，Process2在IO等待期间运行，实现IO‑CPU重叠，总耗时比串行更短。
- Reasoning / 理由:恢复默认调度，进程发起IO立刻切换；IO等待时CPU调度另一个进程执行，充分利用CPU资源。
- Verified result / 验证结果:Total time: 63
- Analysis / 分析:SWITCH_ON_END模式下，进程发起IO阻塞时CPU不会切换到其他就绪进程，CPU空闲等待当前进程IO完成，无法利用IO等待时间运行别的任务；对比默认SWITCH_ON_IO（IO发生就切换），总运行时间显著变长，CPU利用率下降。

## Q8
- Prediction / 预测:SWITCH_ON_END模式，Process1从头到尾执行完毕才切换；IO期间CPU空闲，不能运行Process2，总耗时最大。
- Reasoning / 理由:该模式只有进程执行完毕才发生切换，即便发起IO也不会让出CPU；IO等待阶段CPU无事可做，无法与其他进程并行。
- Verified result / 验证结果:Total time: 28
- Analysis / 分析:`IO_RUN_LATER`：IO完成后进程进入就绪队列，等待调度器分配CPU，不会抢占当前正在运行的进程。`IO_RUN_IMMEDIATE`：IO一完成就立刻抢占CPU，马上恢复该进程运行。本实验两组总时间均为28。在该测试用例下，CPU‑bound进程（PID1）只运行1条指令就结束，抢占没有改变整体耗时。当CPU‑bound进程执行时间很长时，`IO_RUN_IMMEDIATE`会频繁抢占CPU，带来更多上下文切换开销；`IO_RUN_LATER`会减少抢占，对CPU密集型任务更友好。

