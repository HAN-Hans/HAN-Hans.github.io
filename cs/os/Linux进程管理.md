## 查看电脑配置
```
cat /proc/cpuinfo  # 1. 查看cpu
cat /proc/meminfo  # 2. 查看内存到使用
cat /proc/partitions  # 3. 硬盘分区
cat /proc/version  # 4. 版本
df  # 5. 磁盘管理
```

## 界面控制台
```
top - 15:27:07 up 3 days, 22:58,  1 user,  load average: 0.53, 0.81, 0.88  
Tasks: 112 total,   1 running, 111 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.3 us,  0.3 sy,  0.0 ni, 97.3 id,  1.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1016052 total,   503656 free,   193964 used,   318432 buff/cache
KiB Swap:   998396 total,   809016 free,   189380 used.   658524 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
18996 ubuntu    20   0  331916  66748   6360 S  0.7  6.6   0:04.97 celery
18995 ubuntu    20   0  297396  55376   5928 S  0.3  5.5   0:02.15 celery
    1 root      20   0   37644   2036   1356 S  0.0  0.2   0:03.98 systemd
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.02 kthreadd
    3 root      20   0       0      0      0 S  0.0  0.0   0:03.84 ksoftirqd/0
    5 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H
    7 root      20   0       0      0      0 S  0.0  0.0   0:07.32 rcu_sched
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh
    9 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 migration/0
   10 root      rt   0       0      0      0 S  0.0  0.0   0:00.97 watchdog/0
   11 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kdevtmpfs
   12 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 netns
   13 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 perf
   14 root      20   0       0      0      0 S  0.0  0.0   0:00.17 khungtaskd
   15 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback
```
1. 第一行，任务队列信息，同 uptime 命令的执行结果，具体参数说明情况如下：
15:27:07 up 3 days, 22:58 — 当前时间和持续运行时间 
1 user — 当前在线用户数
load average: 1.15, 1.42, 1.44 — load average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。(load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了。)

2. 第二行，Tasks — 任务（进程），具体信息说明如下：
系统现在共有112个进程，其中处于运行中的有1个，111个在休眠（sleep），stoped状态的有0个，zombie状态（僵尸）的有0个。

3. 第三行，cpu状态信息，具体属性说明如下：
1.3%us — 用户空间占用CPU的百分比。
0.3% sy — 内核空间占用CPU的百分比。
0.0% ni — 改变过优先级的进程占用CPU的百分比
97.3% id — 空闲CPU百分比
1.0% wa — IO等待占用CPU的百分比
0.0% hi — 硬中断（Hardware IRQ）占用CPU的百分比
0.0% si — 软中断（Software Interrupts）占用CPU的百分比
备注：在这里CPU的使用比率和windows概念不同，需要理解linux系统用户空间和内核空间的相关知识！

4. 第四行,内存状态，具体信息如下：
1016052 total — 物理内存总量（1GB）
503656  free — 空闲内存总量（0.5GB）
193964 used — 使用中的内存总量（0.2GB）
318432 buffers — 缓存的内存量 （0.3GB）

5. 第五行，swap交换分区信息，具体信息说明如下：
998396 total — 交换区总量（1GB）
809016 free — 空闲交换区总量（0.8GB）
189380 used — 使用的交换区总量（0.2K）
658524 avail — 可用的交换区总量（3.6GB）

第七行以下：各进程（任务）的状态监控，项目列信息说明如下：
PID — 进程id
USER — 进程所有者
PR — 进程优先级
NI — nice值。负值表示高优先级，正值表示低优先级
VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
SHR — 共享内存大小，单位kb
S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
%CPU — 上次更新到现在的CPU时间占用百分比
%MEM — 进程使用的物理内存百分比
TIME+ — 进程使用的CPU时间总计，单位1/100秒
COMMAND — 进程名称（命令名/命令行）

## TOP程序中命令
1. 多U多核CPU监控，按键盘数字`1`，可监控每个逻辑CPU的状况
2. 高亮显示当前运行进程, 敲击键盘`b`（打开/关闭加亮效果），“top”进程被加亮了，可以通过敲击`y`键关闭或打开运行态进程的加亮效果。
3. 进程字段排序，敲击键盘“x”（打开/关闭排序列的加亮效果）
4. 通过”shift + >”或”shift + <”可以向右或左改变排序列
5, 显示完整命令`c`
6. 以批处理模式显示程序信息`b`
7. 以累积模式显示程序信息`-S`
8. 设置信息更新次数 `top -n 2`s
9. 设置信息更新时间 `top -d 3`, 表示更新周期为3秒
10. 显示指定的进程信息`top -p 574`

## 交互命令

在top 命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了s 选项， 其中一些命令可能会被屏蔽。
```
h 显示帮助画面，给出一些简短的命令总结说明
k 终止一个进程。
i 忽略闲置和僵死进程。这是一个开关式命令。
q 退出程序
r 重新安排一个进程的优先级别
S 切换到累计模式
s 改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成m s。输入0值则系统将不断刷新，默认值是5 s
f或者F 从当前显示中添加或者删除项目
o或者O 改变显示项目的顺序
l 切换显示平均负载和启动时间信息
m 切换显示内存信息
t 切换显示进程和CPU状态信息
c 切换显示命令名称和完整命令行
M 根据驻留内存大小进行排序
P 根据CPU使用百分比大小进行排序
T 根据时间/累计时间进行排序
W 将当前设置写入~/.toprc文件中
```

## supercisor
