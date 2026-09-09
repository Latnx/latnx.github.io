## **本地生成密钥**

```bash
ssh-keygen
```
## **上传公钥到服务器**

```bash
ssh-copy-id user@server_ip
```

没有 `ssh-copy-id` 就手动复制 `~/.ssh/id_ed25519.pub` 到服务器的  
`~/.ssh/authorized_keys`

## **VSCode 直接连接**

VSCode → **Remote-SSH** → **Connect to Host** → 输入：

```
ssh user@server_ip
```
