# 1️⃣ Mutex（互斥锁）

用于保护共享资源。
常用方法：
- `Lock()`
- `Unlock()`
- `TryLock()`（Go 1.18+）
### 示例

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.Mutex
	count := 0

	mu.Lock()
	count++
	mu.Unlock()

	fmt.Println(count)
}
```

---

# 2️⃣ RWMutex（读写锁）

读写分离，提高并发性能。

常用方法：
- `Lock()` / `Unlock()` （写锁）
- `RLock()` / `RUnlock()` （读锁）
### ✅ 最小示例

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var mu sync.RWMutex
	data := 10

	mu.RLock()
	fmt.Println(data)
	mu.RUnlock()
}
```

---

# 3️⃣ WaitGroup（等待协程完成）

用于等待多个 goroutine 执行完成。
常用方法：
- `Add(n)`
- `Done()`
- `Wait()`
### ✅ 示例
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup

	wg.Add(1)
	go func() {
		defer wg.Done()
		fmt.Println("hello")
	}()

	wg.Wait()
}
```

---

# 4️⃣ Once（只执行一次）

确保某段代码只执行一次。
常用方法：
- `Do(func())`
### 示例
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var once sync.Once

	once.Do(func() {
		fmt.Println("only once")
	})
}
```

---

# 5️⃣ Cond（条件变量）

用于通知等待的 goroutine。

常用方法：

- `Wait()`    
- `Signal()`    
- `Broadcast()`    
### 示例

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	mu := sync.Mutex{}
	cond := sync.NewCond(&mu)

	go func() {
		mu.Lock()
		cond.Wait()
		fmt.Println("received signal")
		mu.Unlock()
	}()

	mu.Lock()
	cond.Signal()
	mu.Unlock()
}
```

---

# 6️⃣ Map（并发安全 map）

线程安全的 map。

常用方法：
- `Store(key, value)`
- `Load(key)`
- `Delete(key)`
- `Range(func)`

### 示例

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var m sync.Map

	m.Store("a", 1)
	v, _ := m.Load("a")
	fmt.Println(v)
}
```

---

# 7️⃣ Pool（对象池）

用于减少频繁分配对象带来的 GC 压力。
常用方法：
- `Get()`
- `Put(x)`

### 示例

```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	pool := sync.Pool{
		New: func() any {
			return "new object"
		},
	}

	obj := pool.Get()
	fmt.Println(obj)
	pool.Put(obj)
}
```

---

# 📌 总结

|类型|作用|
|---|---|
|Mutex|普通互斥锁|
|RWMutex|读写锁|
|WaitGroup|等待 goroutine|
|Once|只执行一次|
|Cond|条件变量|
|Map|并发安全 map|
|Pool|对象池|
