```go
type slice struct {
   array unsafe.Pointer
   len   int
   cap   int
}
```
拷贝复制指针，使用同一块内存空间
![[image-4-BIMp1lQz.png]]
`newcap = oldcap+(oldcap+3*256)/4`
- **检查所需容量**：如果所需容量大于当前容量的两倍，则直接扩容到所需容量。
- **容量小于阈值时**：如果当前容量小于阈值（默认 256），则新容量为当前容量的两倍。
- **容量大于阈值时**：当容量超过阈值，扩容系数逐渐从 2 倍过渡到 1.25 倍，具体公式为：