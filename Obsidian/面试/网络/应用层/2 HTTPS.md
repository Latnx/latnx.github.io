## HTTPS RSA

![[Pasted image 20260314192241.png|400]]
四次握手
1. Client Hello：TLS版本号，随机数，支持的密码套件列表
2. Server阶段
	1. Server Hello：TLS版本，随机数2，选择的密码套件
	2. Server Certificate：证书
	3. Server Hello Done
3. Client阶段
	1. Client Key Exchange：生成一个随机数（pre-master），用服务器公钥加密
	2. Change Cipher Spec：使用加密方式发送消息
	3. Finished：前面握手的摘要
4. 
	1. Change Cipher Spec：使用加密方式发送消息
	2. Finished：前面握手的摘要

数字证书
- 证书签名过程：
	1. 公钥，用途，颁发者，有效时间，进行HASH
	2. 使用CA私钥对HASH进行签名
	3. 合并签名和信息
- 客户端校验证书：
	1. 对信息HASH
	2. 使用CA公钥对签名进行解密
	3. 对比

## HTTPS ECDHE

DH算法：离散对数
DHE算法：临时生成离散对数，保证前向安全
ECDHE算法：椭圆曲线

握手过程
1. 第一次握手
	1. Client Hello：TLS版本，客户端随机数，密码套件
2. 第二次握手
	1. Server Hello：TLS版本，客户端随机数，密码套件
	2. Server Certificate：证书
	3. Server Key Exchange：交换公钥，用RSA签名
	4. Server Hello Done
3. 第三次握手
	1. Client Key Exchange
	2. Change Cipher Spec：使用加密方式发送消息
	3. Finished：前面握手的摘要
4. 
	1. Change Cipher Spec：使用加密方式发送消息
	2. Finished：前面握手的摘要

## HTTPS 优化

### 协议优化

1. 密钥交换算法优化
	1. 使用ECDHE：2RTT->1RTT
	2. 选择x25519曲线
	3. AES_128_GCM
2. TLS升级：TLS1.2->1.3减少两个RTT
### 证书优化

1. 使用ECDSA证书代替RSA证书
2. CRL证书吊销列表->OCSP在线证书状态协议->OCSP Staping服务器提前缓存证书

### 会话复用

1. Session ID：缓存会话密钥
2. Session Ticket：客户端缓存票据