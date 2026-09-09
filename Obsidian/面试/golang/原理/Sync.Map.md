
使用两个map之间的互相配合(一个read map 和 一个dirty map)，来为我们提供一个拥有并发读写能力，并且权衡整体操作性能的hashTable结构

![[Pasted image 20260529205600.png]]

![[Pasted image 20260529212404.png]]
```go
type Map struct {​
	// 互斥锁，保证dirty map 和 misses的并发安全​
	mu sync.Mutex​
	
	// 无锁化的read map（小林服务器）​
	read atomic.Value // readOnly​
	
	// 读写需要加锁的map（诸葛青服务器）​
	dirty map[any]*entry​
	
	// 记录有多少次读、删请求 找了诸葛青​
	misses int​
}​

type readOnly struct {​
	// read map 存放 k-v的实体​
	m map[interface{}]*entry​
	
	// 标识是否有 数据缺失，即dirty map里面有 read map中没有的key，就是amended为true​
	amended bool​
}
```

### 双向数据流转机制
大量 key 在 read 中找不到，但在 dirty 中能找到
```go
read = dirty
dirty = nil
misses = 0
```
1. read map获得dirty map全量数据，将dirty map的值直接赋给read map
2. 如果有新的写请求到来，将read map里面的逻辑上存在的数据拷贝到dirty map

### entry 状态
```go
type entry struct {
    p unsafe.Pointer
}
```
- nil：我们将nil称之为软删除态，代表数据逻辑上不存在了（物理上仍存在，小林和诸葛青都有这一份数据)​

- expunged: 特殊的一个指针值，把它称之为硬删除态

- 正常值: 一个正常的指针值，比如下面图 "一定"这个字符串的地址

