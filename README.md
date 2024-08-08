## MAX API部署教程：

#### 演示站：[maxapi.aiearth.dev](https://maxapi.aiearth.dev)

![image](https://github.com/user-attachments/assets/e3190252-39f8-415f-b1b2-7bf64a04ab0f)


```
docker镜像地址：registry.cn-hangzhou.aliyuncs.com/pochacco/max-api:latest
```

**部署方式：仅支持采用docker-compose.yml文件进行部署**

**系统免费授权：加入QQ群，联系运营管理员免费获取授权码！**

<figure class="half">     
    <img src="https://github.com/user-attachments/assets/340591ba-a94b-4059-a0e9-05c7fb68a7fa" width="200">     
    <img src="https://github.com/user-attachments/assets/8b46f489-118f-4a9d-a237-cd1d3a0c4827" width="200"> 
</figure>

**docker-compose.yml文件代码：**

```
version: '1.0.2'

services:
  max-api1:
    image: pochacco/max-api:latest
    container_name: Max-API
    restart: always
    command: --log-dir /app/logs
    ports:
      - "3000:3000"
    volumes:
      - ./data:/data
      - ./logs:/app/logs
    environment:
      - SQL_DSN=root:123456@tcp(host.docker.internal:3306)/max-api  # 修改此行，或注释掉以使用 SQLite 作为数据库
      - REDIS_CONN_STRING=redis://redis:6379
      - SESSION_SECRET=max-api  #请修改为随机字符
      - TZ=Asia/Shanghai
      - SYNC_FREQUENCY=60
      - MEMORY_CACHE_ENABLED=true
      - SQL_MAX_IDLE_CONNS=1000
      - SQL_MAX_OPEN_CONNS=8000
      - SQL_CONN_MAX_LIFETIME=60
      - CHANNEL_UPDATE_FREQUENCY=60
      - CHANNEL_TEST_FREQUENCY=60
      - BATCH_UPDATE_ENABLED=true
      - BATCH_UPDATE_INTERVAL=60
      - GLOBAL_API_RATE_LIMIT=200000
      - GLOBAL_WEB_RATE_LIMIT=200000
      - RELAY_TIMEOUT=1000
      - AUTH_CODE=授权码
    depends_on:
      - redis

  redis:
    image: redis:latest
    container_name: redis
    restart: always
    volumes:
      - ./redis-data:/data
  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    restart: always
    environment:
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_POLL_INTERVAL=43200  # 12小时（43200秒）
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

**部署步骤：**

```
新建docker-compose.yml文件将docker-compose.yml文件代码粘贴进去保存

修改数据库信息，并填入授权码

启动项目：在docker-compose.yml文件目录，打开终端输入：docker-compose up -d 即可启动项目

注意：由于增加ip限制功能和速率限制功能需要使用redis
```

