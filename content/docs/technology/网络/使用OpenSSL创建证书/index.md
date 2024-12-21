+++
title = '使用OpenSSL创建证书'
date = 2024-12-21T20:54:13+08:00
draft = false
featureimage = 'https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203542229.png'
tags = ["证书", "SSL", "公钥"]
+++

描述使用什么工具、如何创建的数字证书,
描述用什么工具对数字证书进行了查看，各个字段具体的值是什么以及具体台义，包括扩展项。

## 1. 通过什么工具生成的SM2证书，用到哪些参数，代表什么含义

使用工具：openssl
命令 1 ：`openssl ecparam -genkey -name SM2 -out sm2.key`
![image-20241209203217481](https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203217481.png)

- **`ecparam`**
	用于生成椭圆曲线参数或私钥
	
- 	**`-genkey`**
	指定生成一个私钥
	
-	**`-name SM2`**
	使用 `SM2` 椭圆曲线算法
	
-	**`-out sm2.key`**
	将生成的私钥保存到文件 `sm2.key` 中

命令 2 ：`openssl req -new -key sm2.key -out sm2.csr -sm3 -config sm2.cnf`
- **`req`**
	表示创建或处理证书签名请求 (CSR)。
-	**`-new`**
	生成新的 CSR。
-	**`-key sm2.key`**
	指定使用的私钥文件 `sm2.key`，用于签署 CSR。
-	**`-out sm2.csr`**
	指定输出文件名，生成的 CSR 文件将保存为 `sm2.csr`。
-	**`-sm3`**
	指定使用 SM3 哈希算法

![image-20241209203231611](https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203231611.png)

命令 3 ：`openssl x509 -req -in sm2.csr -signkey sm2.key -sm3 -out sm2.crt -days 365`

- **`x509`**
	表示生成或处理 X.509 格式的证书。
-	**`-req`**
	指定输入为 CSR 文件（`sm2.csr`），用于签发证书。
-	**`-in sm2.csr`**
	指定输入的 CSR 文件路径。
-	**`-signkey sm2.key`**
	使用私钥 `sm2.key` 为证书签名，生成自签名证书。
-	**`-sm3`**
	指定使用 SM3 哈希算法。
-	**`-out sm2.crt`**
	指定输出文件名，生成的证书将保存为 `sm2.crt`。
-	**`-days 365`**
	设置证书有效期为 365 天。

![image-20241209203306323](https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203306323.png)

![image-20241209203417551](https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203417551.png)

## 2. 通过什么工具打开证书，证书里面有哪些字段

### **1. 标准字段**

- **版本（Version）**
  指明 X.509 证书的版本（常见为 v3）。
- **序列号（Serial Number）**
  证书颁发机构（CA）分配的唯一标识符。
- **签名算法（Signature Algorithm）**
  证书签名使用的算法（如 `SM2`, `SHA256-RSA`）。
- **颁发者（Issuer）**
  签发证书的 CA 的信息（如组织名、国家等）。
- **有效期（Validity）**
  - **Not Before**: 证书的生效时间。
  - **Not After**: 证书的到期时间。
- **主题（Subject）**
  被签发证书的实体信息，如域名、公司名等。
- **公钥信息（Public Key Info）**
  包含公钥算法及公钥值。

### **2. 可选扩展字段**

- **基本约束（Basic Constraints）**
  表示是否为 CA 证书，以及证书链的路径长度限制。
- **主题密钥标识符（Subject Key Identifier, SKI）**
  唯一标识证书的公钥。
- **密钥用途（Key Usage）**
  指明证书的用途，如签名、加密、密钥交换等。
- **扩展密钥用途（Extended Key Usage, EKU）**
  进一步限制证书用途，例如仅用于服务器身份验证或代码签名。
- **主题备用名称（Subject Alternative Name, SAN）**
  指定证书绑定的备用名称（如多个域名或 IP 地址）。
- **CRL 分发点（CRL Distribution Points）**
  提供吊销证书列表（CRL）的 URL。
- **OCSP URL（Authority Information Access, AIA）**
  提供 OCSP（在线证书状态协议）服务地址。

![image-20241209203542229](https://mygitea.higeorgehu.top:666/shy/shy_IMG/raw/branch/main/img/image-20241209203542229.png)

## 3. 邮件安全加密和签名的过程和原理

### **1. 邮件加密**

加密用于保护邮件内容免受未经授权的访问，常见方式是使用 **公钥加密**。

**过程**：

- 发送者使用收件人的公钥加密邮件内容。
- 收件人用自己的私钥解密邮件，只有收件人能够解密读取邮件。

**原理**：

- 公钥加密：加密用公钥，解密用私钥，保证机密性。

### **2. 邮件签名**

邮件签名用于验证邮件的发送者身份并确保邮件内容未被篡改。

**过程**：

- 发送者用私钥对邮件内容生成数字签名。
- 收件人用发送者的公钥验证签名，确保邮件内容完整且来自合法发送者。

**原理**：

- 数字签名：使用私钥生成签名，公钥验证签名，保证邮件的真实性和完整性。