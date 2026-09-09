## 重传机制

### 超时重传

超时重传RTO应略大于RTT
超时间隔加倍
### 快重传

收到三个相同的ACK
### SACK

选择性确认
![[Pasted image 20260315193001.png]]
### D-SACK

重复确认 ACK > SACK
## 滑动窗口


## 流量控制

接收方控制接收窗口大小
发送方：零窗口计时器超时，零窗口探测报文段
小报文传输：Nagle，延迟确认
## 拥塞控制

拥塞窗口（cwnd）
慢启动：当发送方收到一个ack，cwnd+1（指数增加）
拥塞避免：cwnd >= ssthresh，每次收到一个ACK，cwnd+=1/cwnd（线性增加）
重传发生：
1. 超时重传：满开始门限 = cwnd/2，cwnd = 1

快重传：接收到三个ACK就重传

快恢复：
ssthresh = cwnd/2
cwnd = ssthresh+3

ssthresh = cwnd/2
cwnd = ssthresh+3
