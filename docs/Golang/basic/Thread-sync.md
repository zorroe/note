在Go语言中经常遇到并发的问题，我们会优先考虑使用通道，同时Go语言也给出了传统的解决方式**Mutex（互斥锁）**和**RWMutex（读写锁）**来处理竞争条件

## 临界区

首先需要理解临界区的概念。当程序并发地运行时，多个Go协程不应该同时访问那些修改共享资源的代码。这些修改共享资源的代码称为临界区

```go
type Bank struct {
	balance int
}

func (b *Bank) Deposit(amount int) {
	b.balance += amount  // 临界区代码
}

func main() {
	var wg sync.WaitGroup
	b := &Bank{}

	n := 10000
	wg.Add(n)
	for i := 0; i < n; i++ {
		go func() {
			b.Deposit(1000)
			wg.Done()
		}()
	}
	wg.Wait()
	fmt.Println(b.balance)  // 9673000  9843000 ...
}
```

对于只有一个协程的程序来说，上面的代码没有问题，但是如果有多个写成并发运行，就会发生错误

## 互斥锁 Mutex

**互斥锁(Mutex，mutual exclusion)** 用于提供一种 **加锁机制(Locking Mechanism)** ，可确保在某时刻只有一个协程在临界区运行，以防止出现竞争。也是为了来保护一个资源不会因为并发操作而引起冲突导致数据不准确。

`Mutex` 有两个方法，分别是 `Lock()` 和 `Unlock()` ，即对应的加锁和解锁。在 `Lock()` 和 `Unlock()` 之间的代码，都只能由一个协程执行，就能避免竞争条件。

如果有一个协程已经持有了**锁(Lock)**，当其他协程试图获得该锁时，这些协程会被阻塞，直到`Mutex`解除锁定。

```go
type Bank struct {
   balance int
   m       sync.Mutex
}

func (b *Bank) Deposit(amount int) {
   b.m.Lock()
   b.balance += amount
   b.m.Unlock()
}

func main() {
   var wg sync.WaitGroup
   b := &Bank{}

   n := 10000
   wg.Add(n)
   for i := 0; i < n; i++ {
      go func() {
         b.Deposit(1000)
         wg.Done()
      }()
   }
   wg.Wait()
   fmt.Println(b.balance)  // 10000000
}
```

在临界区代码前上锁，后解锁。保证同一时间只能有一个协程对`balance`进行操作

## 读写锁

`sync.RWMutex` 类型实现读写互斥锁，适用于读多写少的场景，它规定了当有人还在读取数据（即读锁占用）时，不允许有人更新这个数据（即写锁会阻塞）；为了保证程序的效率，多个人（协程）读取数据（拥有读锁）时，互不影响不会造成阻塞，它不会像 `Mutex` 那样只允许有一个人（协程）读取同一个数据。读锁与读锁兼容，读锁与写锁互斥，写锁与写锁互斥。

* 可同时申请多个读锁
* 有读锁时申请写锁将阻塞，有写锁时申请读锁将阻塞
* 只要有写锁，后续申请读锁和写锁都将阻塞

`RWMutex` 里提供了两种锁，每种锁分别对应两个方法，为了避免死锁，两个方法应成对出现，必要时请使用 `defer` 。

- 读锁：调用 `RLock` 方法开启锁，调用 `RUnlock` 释放锁；
- 写锁：调用 `Lock` 方法开启锁，调用 `Unlock` 释放锁。

```go
type BankV2 struct {
   balance int
   reMutex sync.RWMutex
}

func (b *BankV2) Deposit(amount int) {
   b.reMutex.Lock()
   b.balance += amount
   b.reMutex.Unlock()
}

func (b *BankV2) Balance() (balance int) {
   b.reMutex.RLock()
   balance = b.balance
   b.reMutex.RUnlock()
   return
}

func main() {
   var wg sync.WaitGroup
   b := &BankV2{}

   n := 10000
   wg.Add(n)
   for i := 0; i < n; i++ {
      go func() {
         b.Deposit(1000)
         wg.Done()
      }()
   }
   wg.Wait()
   fmt.Println(b.balance)
}
```

## 条件变量

`Cond` 实现了一个条件变量，在 `Locker` 的基础上增加的一个消息通知的功能，保存了一个通知列表，用来唤醒一个或所有因等待条件变量而阻塞的 Go 程，以此来实现多个 Go 程间的同步。

```go
func listen(name string, s []string, c *sync.Cond) {
   c.L.Lock()
   c.Wait()
   fmt.Println(name, "报名：", s)
   c.L.Unlock()
}

func broadcast(event string, c *sync.Cond) {
   time.Sleep(time.Second)
   c.L.Lock()
   fmt.Println(event)
   c.Broadcast()
   c.L.Unlock()
}

func main() {
   s1 := []string{"张三"}
   s2 := []string{"赵四"}
   s3 := []string{"王五"}
   var m sync.Mutex
   cond := sync.NewCond(&m)

   go listen("Java", s1, cond)
   go listen("Python", s2, cond)
   go listen("C++", s3, cond)

   go broadcast("秒杀开始:", cond)

   ch := make(chan os.Signal, 1)
   signal.Notify(ch, os.Interrupt)
   <-ch
}
```