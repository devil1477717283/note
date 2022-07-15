# Go并发编程

## Go的并发方案:goroutine

* Go 并没有使用操作系统线程作为承载分解后的代码片段（模块）的基本执行单元，而是实现了goroutine这一由 Go 运行时（runtime）负责调度的、轻量的用户级线程，为并发程序设计提供原生支持。

* goroutine的优势

  * 资源占用小，每个 goroutine 的初始栈大小仅为 **2k**；
  * 由 Go 运行时而不是操作系统调度，goroutine 上下文切换在用户层完成，开销更小
  * 在语言层面而不是通过标准库提供。goroutine 由go关键字创建，一退出就会被回收或销毁，开发体验更佳；
  * 语言内置 channel 作为 goroutine 间通信原语，为并发设计提供了强大支撑。

* goroutine的基本使用

  * go+函数/方法的方式创建一个goroutine，创建后新goroutine将拥有独立的代码执行流，并与创建它的goroutine一起背Go运行时调度
  * goroutine 的执行函数的返回，就意味着 goroutine 退出。

* goroutine间的通信

  * 使用CSP（Communicating Sequential Processes，通信顺序进程）并发模型。

    > P 并不一定与操作系统的进程或线程划等号。在 Go 中，与“Process”对应的是 goroutine。为了实现 CSP 并发模型中的输入和输出原语，Go 还引入了 goroutine（P）之间的通信原语channel

* 获取goroutine的退出状态

  ```go
  
  func spawn(f func() error) <-chan error {
      c := make(chan error)
  
      go func() {
          c <- f()
      }()
  
      return c
  }
  
  func main() {
      c := spawn(func() error {
          time.Sleep(2 * time.Second)
          return errors.New("timeout")
      })
      fmt.Println(<-c)
  }
  ```

## Goroutine调度器

* Goroutine 们要竞争的“CPU”资源就是操作系统线程
* Goroutine 调度器的任务也就明确了：将 Goroutine 按照一定算法放到不同的操作系统线程中去执行。

## channel

* 对nil channel读写都会阻塞
* 从一个已关闭的channel接收数据将永远不会被阻塞