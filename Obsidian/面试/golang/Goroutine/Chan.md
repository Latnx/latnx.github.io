```go
ch := make(chan int)         // 创建一个管道ch
ch <- v                      // 向管道ch中发送数据v.
v := <-ch                    // 从管道中读取数据存储到变量v
close(ch)                    // 关闭管道ch
```
**双向channel和单向channel**
协程之间可以利用channel来传递数据
