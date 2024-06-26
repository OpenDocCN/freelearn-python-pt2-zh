# 第十二章：死锁

死锁是并发问题中最常见的问题之一。在本章中，我们将讨论并发编程中死锁的理论原因。我们将涵盖并发中的一个经典同步问题，称为哲学家就餐问题，作为死锁的现实例子。我们还将在 Python 中演示死锁的实际实现。我们将讨论解决该问题的几种方法。本章还将涵盖与死锁相关的活锁概念，这是并发编程中相对常见的问题。

本章将涵盖以下主题：

+   死锁的概念，以及如何在 Python 中模拟它

+   死锁的常见解决方案，以及如何在 Python 中实现它们

+   活锁的概念，以及它与死锁的关系

# 技术要求

以下是本章的先决条件列表：

+   确保您的计算机上安装了 Python 3

+   在[`github.com/PacktPublishing/Mastering-Concurrency-in-Python`](https://github.com/PacktPublishing/Mastering-Concurrency-in-Python)下载 GitHub 存储库

+   在本章中，我们将使用名为**`Chapter12`**的子文件夹进行工作

+   查看以下视频以查看代码的实际操作：[`bit.ly/2r2WKaU`](http://bit.ly/2r2WKaU)

# 死锁的概念

在计算机科学领域，死锁指的是并发编程中的一种特定情况，即程序无法取得进展并且陷入当前状态。在大多数情况下，这种现象是由于不同锁对象之间的协调不足或处理不当（用于线程同步目的）。在本节中，我们将讨论一个被称为哲学家就餐问题的思想实验，以阐明死锁及其原因的概念；从那里，您将学习如何在 Python 并发程序中模拟该问题。

# 哲学家就餐问题

哲学家就餐问题最初是由 Edgar Dijkstra（正如您在**第一章**中学到的那样，*并发和并行编程的高级介绍*是并发编程的领先先驱）在 1965 年首次提出的。该问题最初使用不同的技术术语（计算机系统中的资源争用）进行演示，并且后来由 Tony Hoare 重新表述，他是一位英国计算机科学家，也是快速排序算法的发明者。问题陈述如下。

五位哲学家围坐在一张桌子旁，每个人面前都有一碗食物。在这五碗食物之间放着五把叉子，所以每个哲学家左边和右边都有一把叉子。这个设置由以下图表演示：

![](img/4d3213aa-ee0f-4d22-a967-af295e8e34b5.png)

哲学家就餐问题的插图

每位沉默的哲学家都要在思考和进餐之间交替。每位哲学家需要周围的两把叉子才能够拿起自己碗里的食物，而且一把叉子不能被两个或更多不同的哲学家共享。当一个哲学家吃完一定量的食物后，他们需要把两把叉子放回原来的位置。在这一点上，那位哲学家周围的哲学家将能够使用那些叉子。

由于哲学家们是沉默的，无法相互交流，因此他们没有方法让彼此知道他们需要叉子来吃饭。换句话说，哲学家吃饭的唯一方法是已经有两把叉子可供他们使用。这个问题的问题是设计一组指令，使哲学家能够有效地在进餐和思考之间切换，以便每个哲学家都能得到足够的食物。

现在，解决这个问题的一个潜在方法可能是以下一组指令：

1.  哲学家必须思考，直到他们左边的叉子可用。当这种情况发生时，哲学家就要拿起它。

1.  哲学家必须思考，直到他们右边的叉子可用。当这种情况发生时，哲学家就要拿起它。

1.  如果一个哲学家手里拿着两个叉子，他们会从面前的碗里吃一定量的食物，然后以下情况将适用：

+   之后，哲学家必须把右边的叉子放回原来的位置

+   之后，哲学家必须把左边的叉子放回原来的位置。

1.  过程从第一个项目重复。

很明显，这一系列指令如何导致无法取得进展的情况；也就是说，如果一开始所有哲学家都同时开始执行他们的指令。由于一开始所有叉子都在桌子上，因此附近的哲学家可以拿起叉子执行第一个指令（拿起左边的叉子）。

现在，经过这一步，每个哲学家都会用左手拿着一个叉子，桌子上不会剩下叉子。由于没有哲学家手里同时拿着两个叉子，他们无法开始吃饭。此外，他们得到的指令集规定，只有在哲学家吃了一定量的食物后，才能把叉子放在桌子上。这意味着只要哲学家没有吃饭，他们就不会放下手里的叉子。

因此，每个哲学家只用左手拿着一个叉子，无法开始吃饭或放下手里的叉子。哲学家能吃饭的唯一时机是邻座的哲学家放下叉子，而这只有在他们自己能吃饭的情况下才可能发生；这造成了一个永无止境的条件循环，无法满足。这种情况本质上就是死锁的特性，系统中的所有元素都被困在原地，无法取得进展。

# 并发系统中的死锁

考虑到餐桌哲学家问题的例子，让我们考虑死锁的正式概念以及相关的理论。给定一个具有多个线程或进程的并发程序，如果一个进程（或线程）正在等待另一个进程持有并使用的资源，而另一个进程又在等待另一个进程持有的资源，那么执行流程就会陷入死锁。换句话说，进程在等待只有在执行完成后才能释放的资源时，无法继续执行其指令；因此，这些进程无法改变其执行状态。

死锁还由并发程序需要同时具备的条件来定义。这些条件最初由计算机科学家 Edward G. Coffman, Jr.提出，因此被称为 Coffman 条件。这些条件如下：

+   至少有一个资源必须处于不可共享的状态。这意味着资源被一个单独的进程（或线程）持有，其他人无法访问；资源只能被单个进程（或线程）在任何给定时间内访问和持有。这种情况也被称为互斥。

+   存在一个同时访问资源并等待其他进程（或线程）持有的进程（或线程）。换句话说，这个进程（或线程）需要访问两个资源才能执行其指令，其中一个已经持有，另一个则需要等待其他进程（或线程）释放。这种情况称为持有和等待。

+   资源只能由持有它们的进程（或线程）释放，如果有特定的指令要求进程（或线程）这样做。这就是说，除非进程（或线程）自愿主动释放资源，否则该资源将保持在不可共享的状态。这就是无抢占条件。

+   最终的条件称为循环等待。正如名称所示，该条件指定存在一组进程（或线程），使得该组中的第一个进程（或线程）处于等待状态，等待第二个进程（或线程）释放资源，而第二个进程（或线程）又需要等待第三个进程（或线程）；最后，该组中的最后一个进程（或线程）等待第一个进程。

让我们快速看一个死锁的基本例子。考虑一个并发程序，其中有两个不同的进程（进程**A**和进程**B**），以及两个不同的资源（资源**R1**和资源**R2**），如下所示：

![](img/e440b909-cfa2-4257-9c5c-6ab2a8eb71e2.png)

样本死锁图

这两个资源都不能在不同的进程之间共享，并且每个进程都需要访问这两个资源来执行其指令。以进程**A**为例。它已经持有资源**R1**，但它还需要**R2**来继续执行。然而，**R2**无法被进程**A**获取，因为它被进程**B**持有。因此，进程**A**无法继续。进程**B**也是一样，它持有**R2**，并且需要**R1**来继续。而**R1**又被进程**A**持有。

# Python 模拟

在本节中，我们将在一个实际的 Python 程序中实现前面的情况。具体来说，我们将有两个锁（我们将它们称为锁 A 和锁 B），以及两个分开的线程与锁交互（线程 A 和线程 B）。在我们的程序中，我们将设置这样一种情况：线程 A 已经获取了锁 A，并且正在等待获取锁 B，而锁 B 已经被线程 B 获取，并且正在等待锁 A 被释放。

如果您已经从 GitHub 页面下载了本书的代码，请转到`Chapter12`文件夹。让我们考虑`Chapter12/example1.py`文件，如下所示：

```py
# Chapter12/example1.py

import threading
import time

def thread_a():
    print('Thread A is starting...')

    print('Thread A waiting to acquire lock A.')
    lock_a.acquire()
    print('Thread A has acquired lock A, performing some calculation...')
    time.sleep(2)

    print('Thread A waiting to acquire lock B.')
    lock_b.acquire()
    print('Thread A has acquired lock B, performing some calculation...')
    time.sleep(2)

    print('Thread A releasing both locks.')
    lock_a.release()
    lock_b.release()

def thread_b():
    print('Thread B is starting...')

    print('Thread B waiting to acquire lock B.')
    lock_b.acquire()
    print('Thread B has acquired lock B, performing some calculation...')
    time.sleep(5)

    print('Thread B waiting to acquire lock A.')
    lock_a.acquire()
    print('Thread B has acquired lock A, performing some calculation...')
    time.sleep(5)

    print('Thread B releasing both locks.')
    lock_b.release()
    lock_a.release()

lock_a = threading.Lock()
lock_b = threading.Lock()

thread1 = threading.Thread(target=thread_a)
thread2 = threading.Thread(target=thread_b)

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print('Finished.')
```

在这个脚本中，`thread_a()`和`thread_b()`函数分别指定了我们的线程 A 和线程 B。在我们的主程序中，我们还有两个`threading.Lock`对象：锁 A 和锁 B。线程指令的一般结构如下：

1.  启动线程

1.  尝试获取与线程名称相同的锁（线程 A 将尝试获取锁 A，线程 B 将尝试获取锁 B）

1.  执行一些计算

1.  尝试获取另一个锁（线程 A 将尝试获取锁 B，线程 B 将尝试获取锁 A）

1.  执行一些其他计算

1.  释放两个锁

1.  结束线程

请注意，我们使用`time.sleep()`函数来模拟一些计算正在进行的动作。

首先，我们几乎同时启动线程 A 和线程 B，在主程序中。考虑到线程指令集的结构，我们可以看到此时两个线程将被启动；线程 A 将尝试获取锁 A，并且会成功，因为此时锁 A 仍然可用。线程 B 和锁 B 也是一样。然后两个线程将继续进行一些计算。

让我们考虑一下我们程序的当前状态：锁 A 已被线程 A 获取，锁 B 已被线程 B 获取。在它们各自的计算过程完成后，线程 A 将尝试获取锁 B，线程 B 将尝试获取锁 A。我们很容易看出这是我们死锁情况的开始：由于锁 B 已经被线程 B 持有，并且无法被线程 A 获取，出于同样的原因，线程 B 也无法获取锁 A。

现在，两个线程将无限等待，以获取它们各自的第二个锁。然而，锁能够被释放的唯一方式是线程继续执行指令并在最后释放它所持有的所有锁。因此，我们的程序将在这一点上被卡住，不会再有进展。

以下图表进一步说明了死锁是如何按顺序展开的。

![](img/bd657a2f-bc26-424b-a3ef-06c0bd441ffa.png)

死锁序列图

现在，让我们看看我们创建的死锁是如何发生的。运行脚本，你应该会得到以下输出：

```py
> python example1.py
Thread A is starting...
Thread A waiting to acquire lock A.
Thread B is starting...
Thread A has acquired lock A, performing some calculation...
Thread B waiting to acquire lock B.
Thread B has acquired lock B, performing some calculation...
Thread A waiting to acquire lock B.
Thread B waiting to acquire lock A.
```

正如我们讨论过的，由于每个线程都试图获取另一个线程当前持有的锁，而锁能够被释放的唯一方式是线程继续执行。这就是死锁，你的程序将无限挂起，永远无法到达程序最后一行的最终打印语句。

# 死锁情况的方法

正如我们所见，死锁会导致我们的并发程序陷入无限挂起，这在任何情况下都是不可取的。在本节中，我们将讨论预防死锁发生的潜在方法。直觉上，每种方法都旨在消除程序中的四个 Coffman 条件之一，以防止死锁发生。

# 实现资源之间的排名

从哲学家就餐问题和我们的 Python 示例中，我们可以看到四个 Coffman 条件中的最后一个条件，循环等待，是死锁问题的核心。它指定了并发程序中不同进程（或线程）等待其他进程（或线程）持有的资源的循环方式。仔细观察后，我们可以看到这种条件的根本原因是进程（或线程）访问资源的顺序（或缺乏顺序）。

在哲学家就餐问题中，每个哲学家都被指示首先拿起左边的叉子，而在我们的 Python 示例中，线程总是在执行任何计算之前尝试获取同名的锁。正如你所见，当哲学家们想同时开始就餐时，他们会拿起各自左边的叉子，并陷入无限等待；同样，当两个线程同时开始执行时，它们将获取各自的锁，然后再次无限等待另一个锁。

我们可以从中得出的结论是，如果进程（或线程）不是任意地访问资源，而是按照预定的静态顺序访问它们，那么它们获取和等待资源的循环性质将被消除。因此，对于我们的两把锁 Python 示例，我们将要求两个线程以相同的顺序尝试获取锁。例如，现在两个线程将首先尝试获取锁 A，进行一些计算，然后尝试获取锁 B，进行进一步的计算，最后释放两个线程。

这个改变是在`Chapter12/example2.py`文件中实现的，如下所示：

```py
# Chapter12/example2.py

import threading
import time

def thread_a():
    print('Thread A is starting...')

    print('Thread A waiting to acquire lock A.')
    lock_a.acquire()
    print('Thread A has acquired lock A, performing some calculation...')
    time.sleep(2)

    print('Thread A waiting to acquire lock B.')
    lock_b.acquire()
    print('Thread A has acquired lock B, performing some calculation...')
    time.sleep(2)

    print('Thread A releasing both locks.')
    lock_a.release()
    lock_b.release()

def thread_b():
    print('Thread B is starting...')

    print('Thread B waiting to acquire lock A.')
    lock_a.acquire()
    print('Thread B has acquired lock A, performing some calculation...')
    time.sleep(5)

    print('Thread B waiting to acquire lock B.')
    lock_b.acquire()
    print('Thread B has acquired lock B, performing some calculation...')
    time.sleep(5)

    print('Thread B releasing both locks.')
    lock_b.release()
    lock_a.release()

lock_a = threading.Lock()
lock_b = threading.Lock()

thread1 = threading.Thread(target=thread_a)
thread2 = threading.Thread(target=thread_b)

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print('Finished.')
```

这个版本的脚本现在能够完成执行，并应该产生以下输出：

```py
> python3 example2.py
Thread A is starting...
Thread A waiting to acquire lock A.
Thread A has acquired lock A, performing some calculation...
Thread B is starting...
Thread B waiting to acquire lock A.
Thread A waiting to acquire lock B.
Thread A has acquired lock B, performing some calculation...
Thread A releasing both locks.
Thread B has acquired lock A, performing some calculation...
Thread B waiting to acquire lock B.
Thread B has acquired lock B, performing some calculation...
Thread B releasing both locks.
Finished.
```

这种方法有效地消除了我们两把锁示例中的死锁问题，但它对哲学家就餐问题的解决方案有多大的影响呢？为了回答这个问题，让我们尝试自己在 Python 中模拟问题和解决方案。`Chapter12/example3.py`文件包含了 Python 中哲学家就餐问题的实现，如下所示：

```py
# Chapter12/example3.py

import threading

# The philosopher thread
def philosopher(left, right):
    while True:
        with left:
             with right:
                 print(f'Philosopher at {threading.currentThread()} 
                       is eating.')

# The chopsticks
N_FORKS = 5
forks = [threading.Lock() for n in range(N_FORKS)]

# Create all of the philosophers
phils = [threading.Thread(
    target=philosopher,
    args=(forks[n], forks[(n + 1) % N_FORKS])
) for n in range(N_FORKS)]

# Run all of the philosophers
for p in phils:
    p.start()
```

在这里，我们有`philospher()`函数作为我们单独线程的基本逻辑。它接受两个`Threading.Lock`对象，并模拟先前讨论的吃饭过程，使用两个上下文管理器。在我们的主程序中，我们创建了一个名为`forks`的五个锁对象的列表，以及一个名为`phils`的五个线程的列表，规定第一个线程将获取第一个和第二个锁，第二个线程将获取第二个和第三个锁，依此类推；第五个线程将按顺序获取第五个和第一个锁。最后，我们同时启动所有五个线程。

运行脚本，可以很容易地观察到死锁几乎立即发生。以下是我的输出，直到程序无限挂起：

```py
> python3 example3.py
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-1, started 123145445048320)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-5, started 123145466068992)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
Philosopher at <Thread(Thread-3, started 123145455558656)> is eating.
```

接下来自然而然的问题是：我们如何在`philosopher()`函数中实现获取锁的顺序？我们将使用 Python 中内置的`id()`函数，该函数返回参数的唯一常量标识作为排序锁对象的键。我们还将实现一个自定义上下文管理器，以便将这个排序逻辑分离到一个单独的类中。请转到`Chapter12/example4.py`查看具体实现。

```py
# Chapter12/example4.py

class acquire(object):
    def __init__(self, *locks):
        self.locks = sorted(locks, key=lambda x: id(x))

    def __enter__(self):
        for lock in self.locks:
            lock.acquire()

    def __exit__(self, ty, val, tb):
        for lock in reversed(self.locks):
            lock.release()
        return False

# The philosopher thread
def philosopher(left, right):
    while True:
        with acquire(left,right):
             print(f'Philosopher at {threading.currentThread()} 
                   is eating.')
```

在主程序保持不变的情况下，这个脚本将产生一个输出，显示排序的解决方案可以有效解决哲学家就餐问题。

然而，当这种方法应用于某些特定情况时，会出现问题。牢记并发的高级思想，我们知道在将并发应用于程序时的主要目标之一是提高速度。让我们回到我们的两锁示例，检查实现资源排序后程序的执行时间。看一下`Chapter12/example5.py`文件；它只是实现了排序（或有序）锁定的两锁程序，结合了一个计时器，用于跟踪两个线程完成执行所需的时间。

运行脚本后，你的输出应该类似于以下内容：

```py
> python3 example5.py
Thread A is starting...
Thread A waiting to acquire lock A.
Thread B is starting...
Thread A has acquired lock A, performing some calculation...
Thread B waiting to acquire lock A.
Thread A waiting to acquire lock B.
Thread A has acquired lock B, performing some calculation...
Thread A releasing both locks.
Thread B has acquired lock A, performing some calculation...
Thread B waiting to acquire lock B.
Thread B has acquired lock B, performing some calculation...
Thread B releasing both locks.
Took 14.01 seconds.
Finished.
```

你可以看到两个线程的组合执行大约需要 14 秒。然而，如果我们仔细看两个线程的具体指令，除了与锁交互外，线程 A 需要大约 4 秒来进行计算（通过两个`time.sleep(2)`命令模拟），而线程 B 需要大约 10 秒（两个`time.sleep(5)`命令）。

这是否意味着我们的程序花费的时间与我们按顺序执行两个线程时一样长？我们将用`Chapter12/example6.py`文件测试这个理论，在这个文件中，我们规定每个线程应该在主程序中依次执行它的指令：

```py
# Chapter12/example6.py

lock_a = threading.Lock()
lock_b = threading.Lock()

thread1 = threading.Thread(target=thread_a)
thread2 = threading.Thread(target=thread_b)

start = timer()

thread1.start()
thread1.join()

thread2.start()
thread2.join()

print('Took %.2f seconds.' % (timer() - start))
print('Finished.')
```

运行这个脚本，你会发现我们的两锁程序的顺序版本将花费与并发版本相同的时间。

```py
> python3 example6.py
Thread A is starting...
Thread A waiting to acquire lock A.
Thread A has acquired lock A, performing some calculation...
Thread A waiting to acquire lock B.
Thread A has acquired lock B, performing some calculation...
Thread A releasing both locks.
Thread B is starting...
Thread B waiting to acquire lock A.
Thread B has acquired lock A, performing some calculation...
Thread B waiting to acquire lock B.
Thread B has acquired lock B, performing some calculation...
Thread B releasing both locks.
Took 14.01 seconds.
Finished.
```

这个有趣的现象是我们在程序中对锁的严格要求的直接结果。换句话说，由于每个线程都必须获取两个锁才能完成执行，每个锁在任何给定时间内都不能被多个线程获取，最后，需要按特定顺序获取锁，并且单个线程的执行不能同时发生。如果我们回过头来检查`Chapter12/example5.py`文件产生的输出，很明显可以看到线程 B 在线程 A 在执行结束时释放两个锁后无法开始计算。

因此，很直观地得出结论，如果在并发程序的资源上放置了足够多的锁，它将在执行上变得完全顺序化，并且结合并发编程功能的开销，它的速度甚至会比程序的纯顺序版本更糟糕。然而，在餐桌哲学家问题中（在 Python 中模拟），我们没有看到锁所创建的这种顺序性。这是因为在两线程问题中，两个锁足以使程序执行顺序化，而五个锁不足以使餐桌哲学家问题执行顺序化。

我们将在《第十四章》*竞争条件*中探讨这种现象的另一个实例。

# 忽略锁并共享资源

锁无疑是同步任务中的重要工具，在并发编程中也是如此。然而，如果锁的使用导致不良情况，比如死锁，那么我们很自然地会探索在并发程序中简单地不使用锁的选项。通过忽略锁，我们程序的资源有效地在并发程序中的不同进程/线程之间可以共享，从而消除了 Coffman 条件中的第一个条件：互斥。

这种解决死锁问题的方法可能很容易实现；让我们尝试前面的两个例子。在两锁示例中，我们简单地删除了指定与线程函数和主程序中的锁对象的任何交互的代码。换句话说，我们不再使用锁定机制。`Chapter12/example7.py` 文件包含了这种方法的实现，如下所示：

```py
# Chapter12/example7.py

import threading
import time
from timeit import default_timer as timer

def thread_a():
    print('Thread A is starting...')

    print('Thread A is performing some calculation...')
    time.sleep(2)

    print('Thread A is performing some calculation...')
    time.sleep(2)

def thread_b():
    print('Thread B is starting...')

    print('Thread B is performing some calculation...')
    time.sleep(5)

    print('Thread B is performing some calculation...')
    time.sleep(5)

thread1 = threading.Thread(target=thread_a)
thread2 = threading.Thread(target=thread_b)

start = timer()

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print('Took %.2f seconds.' % (timer() - start))

print('Finished.')
```

运行脚本，你的输出应该类似于以下内容：

```py
> python3 example7.py
Thread A is starting...
Thread A is performing some calculation...
Thread B is starting...
Thread B is performing some calculation...
Thread A is performing some calculation...
Thread B is performing some calculation...
Took 10.00 seconds.
Finished.
```

很明显，由于我们不使用锁来限制对任何计算过程的访问，两个线程的执行现在已经完全独立于彼此，因此线程完全并行运行。因此，我们也获得了更好的速度：由于线程并行运行，整个程序所花费的总时间与两个线程中较长任务所花费的时间相同（换句话说，线程 B，10 秒）。

那么餐桌哲学家问题呢？似乎我们也可以得出结论，没有锁（叉子）的情况下，问题可以很容易地解决。由于资源（食物）对于每个哲学家都是独特的（换句话说，没有哲学家应该吃另一个哲学家的食物），因此每个哲学家都可以在不担心其他人的情况下继续执行。通过忽略锁，每个哲学家可以并行执行，类似于我们在两锁示例中看到的情况。

然而，这样做意味着我们完全误解了问题。我们知道锁被利用来让进程和线程可以以系统化、协调的方式访问程序中的共享资源，以避免对数据的错误处理。因此，在并发程序中移除任何锁定机制意味着共享资源的可能性，这些资源现在不受访问限制，被以不协调的方式操纵（因此，变得损坏）的可能性显著增加。

因此，通过忽略锁，我们很可能需要完全重新设计和重构我们的并发程序。如果共享资源仍然需要以有组织的方式访问和操作，就需要实现其他同步方法。我们的进程和线程的逻辑可能需要改变以适当地与这种新的同步方法进行交互，执行时间可能会受到程序结构变化的负面影响，还可能会出现其他潜在的同步问题。

# 关于锁的额外说明

虽然在我们的程序中取消锁定机制以消除死锁的方法可能会引发一些问题和关注，但它确实为我们揭示了 Python 中锁对象的一个新点：在访问给定资源时，一个并发程序的元素完全可以绕过锁。换句话说，锁对象只有在进程/线程实际获取锁对象时，才能阻止不同的进程/线程访问和操作共享资源。

因此，锁实际上并没有锁定任何东西。它们只是标志，帮助指示在给定时间是否应该访问资源；如果一个指令不清晰甚至恶意的进程/线程试图在没有检查锁对象存在的情况下访问该资源，它很可能可以轻松地做到这一点。换句话说，锁根本不与它们应该锁定的资源相关联，它们绝对不会阻止进程/线程访问这些资源。

因此，简单地使用锁来设计和实现安全的、动态的并发数据结构是低效的。为了实现这一点，我们需要在锁和它们对应的资源之间添加更多具体的链接，或者完全利用不同的同步工具（例如原子消息队列）。

# 关于死锁解决方案的结论

您已经看到了解决死锁问题的两种最常见方法。每种方法都解决了四个 Coffman 条件中的一个，虽然两种方法在我们的示例中都（在某种程度上）成功地防止了死锁的发生，但每种方法都引发了不同的额外问题和关注。因此，真正理解您的并发程序的性质非常重要，以便知道这两种方法中的哪一种是适用的，如果有的话。

也有可能，一些程序通过死锁向我们展示出不适合并发的特性；有些程序最好是按顺序执行，如果强制并发可能会变得更糟。正如我们所讨论的，虽然并发在我们应用程序的许多领域中提供了显著的改进，但有些领域本质上不适合并发编程的应用。在死锁的情况下，开发人员应该准备考虑设计并发程序的不同方法，并且在一个并发方法不起作用时不要犹豫地实现另一种方法。

# 活锁的概念

活锁的概念与死锁有关；有些人甚至认为它是死锁的另一种版本。在活锁的情况下，并发程序中的进程（或线程）能够切换它们的状态；事实上，它们不断地切换状态。然而，它们只是无限地来回切换，没有任何进展。现在我们将考虑一个实际的活锁场景。

假设一对夫妇在一起吃晚餐。他们只有一个叉子可以共用，所以在任何给定的时间只有一个人可以吃。此外，夫妇之间非常彬彬有礼，所以即使其中一位饥饿想吃饭，如果另一位也饥饿，他们会把叉子放在桌子上。这个规定是创建这个问题的活锁的核心：当夫妇两个都饥饿时，每个人都会等待另一个先吃饭，从而创建一个无限循环，每个人都在想要吃饭和等待另一位先吃饭之间切换。

让我们在 Python 中模拟这个问题。转到`Chapter12/example8.py`，看一下`Spouse`类：

```py
# Chapter12/example8.py

class Spouse(threading.Thread):

    def __init__(self, name, partner):
        threading.Thread.__init__(self)
        self.name = name
        self.partner = partner
        self.hungry = True

    def run(self):
        while self.hungry:
            print('%s is hungry and wants to eat.' % self.name)

            if self.partner.hungry:
                print('%s is waiting for their partner to eat first...' 
                      % self.name)
            else:
                with fork:
                    print('%s has stared eating.' % self.name)
                    time.sleep(5)

                    print('%s is now full.' % self.name)
                    self.hungry = False
```

这个类继承自`threading.Thread`类，并实现了我们之前讨论的逻辑。它接受一个`Spouse`实例的名称和另一个`Spouse`对象作为其伴侣；初始化时，`Spouse`对象也总是饥饿的（`hungry`属性始终设置为`True`）。类中的`run()`函数指定了线程启动时的逻辑：只要`Spouse`对象的`hungry`属性设置为`True`，对象将尝试使用叉子（一个锁对象）进食。但是，它总是检查其伴侣的`hungry`属性是否也设置为`True`，在这种情况下，它将不会继续获取锁，而是等待其伴侣这样做。

在我们的主程序中，首先将叉子创建为一个锁对象；然后，我们创建两个`Spouse`线程对象，它们分别是彼此的`partner`属性。最后，我们启动两个线程，并运行程序直到两个线程都执行完毕：

```py
# Chapter12/example8.py

fork = threading.Lock()

partner1 = Spouse('Wife', None)
partner2 = Spouse('Husband', partner1)
partner1.partner = partner2

partner1.start()
partner2.start()

partner1.join()
partner2.join()

print('Finished.')
```

运行脚本，您会看到，正如我们讨论的那样，每个线程都会进入一个无限循环，不断地在想要吃饭和等待伴侣吃饭之间切换；程序将永远运行，直到 Python 被中断。以下代码显示了我得到的输出的前几行：

```py
> python3 example8.py
Wife is hungry and wants to eat.
Wife is waiting for their partner to eat first...
Husband is hungry and wants to eat.
Wife is hungry and wants to eat.
Husband is waiting for their partner to eat first...
Wife is waiting for their partner to eat first...
Husband is hungry and wants to eat.
Wife is hungry and wants to eat.
Husband is waiting for their partner to eat first...
Wife is waiting for their partner to eat first...
Husband is hungry and wants to eat.
Wife is hungry and wants to eat.
Husband is waiting for their partner to eat first...
...
```

# 总结

在计算机科学领域，死锁是指并发编程中的一种特定情况，即没有任何进展并且程序被锁定在当前状态。在大多数情况下，这种现象是由于不同锁对象之间缺乏或处理不当的协调引起的，可以用餐厅哲学家问题来说明。

预防死锁发生的潜在方法包括对锁对象施加顺序和通过忽略锁对象共享不可共享的资源。每种解决方案都解决了四个 Coffman 条件中的一个，虽然这两种解决方案都可以成功地防止死锁，但每种解决方案都会引发不同的额外问题和关注点。

与死锁概念相关的是活锁。在活锁情况下，并发程序中的进程（或线程）能够切换它们的状态，但它们只是无休止地来回切换，没有任何进展。在下一章中，我们将讨论并发编程中的另一个常见问题：饥饿。

# 问题

+   什么会导致死锁情况，为什么这是不可取的？

+   餐厅哲学家问题与死锁问题有什么关系？

+   什么是四个 Coffman 条件？

+   资源排序如何解决死锁问题？在实施这一方法时可能会出现哪些其他问题？

+   忽略锁如何解决死锁问题？在实施这一方法时可能会出现哪些其他问题？

+   活锁与死锁有什么关系？

# 进一步阅读

有关更多信息，您可以参考以下链接：

+   *使用 Python 进行并行编程*，作者 Jan. Palach，Packt Publishing Ltd，2014

+   *Python 并行编程食谱*，作者 Giancarlo Zaccone，Packt Publishing Ltd，2015

+   *Python 线程死锁避免*（[dabeaz.blogspot.com/2009/11/python-thread-deadlock-avoidance_20](http://dabeaz.blogspot.com/2009/11/python-thread-deadlock-avoidance_20.html)）
