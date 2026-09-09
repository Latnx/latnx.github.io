定义一个镜像怎么构建
- `RUN`：**构建镜像时执行命令**
- `CMD`：**容器启动时执行命令**

```dockerfile
FROM python:3.11
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```
用于：docker build -t <镜像名> <镜像位置>


### docker compose

定义多个容器怎么一起运行
```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"

  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456
```