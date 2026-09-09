#### HTTP/1.1优化
##### 1. 避免HTTP请求
##### 缓存
###### 强制缓存

`Cache-Control`
`Expires` 
###### 协商缓存
第二次请求，服务器返回304
响应头中 `Last-Modified`
请求头中 `If-Modified-Since`

ETag
If-None-Match
##### 减少请求次数
1. 减少重定向请求次数：重定向的任务交给代理服务器
2. 和并请求：小图片合成大图片，webpack合并打包js，图片base64嵌入html
3. 按需请求
##### 减少响应大小
1. 无损压缩
2. 有损压缩