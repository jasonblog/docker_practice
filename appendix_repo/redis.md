## [Redis](https://registry.hub.docker.com/_/redis/)

### 基本訊息
[Redis](https://en.wikipedia.org/wiki/Redis) 是開源的記憶體 Key-Value 資料庫實作。

### 使用方法
預設會在 `6379` 連接埠啟動資料庫。
```
$ sudo docker run --name some-redis -d redis
```
另外還可以啟用 [持久儲存](http://redis.io/topics/persistence)。
```
$ sudo docker run --name some-redis -d redis redis-server --appendonly yes
```
預設資料儲存位置在 `VOLUME/data`。可以使用 `--volumes-from some-volume-container` 或 `-v /docker/host/dir:/data` 將資料存放到本地。

使用其他應用連接到容器，可以用
```
$ sudo docker run --name some-app --link some-redis:redis -d application-that-uses-redis
```
或者透過 `redis-cli`
```
$ sudo docker run -it --link some-redis:redis --rm redis sh -c 'exec redis-cli -h "$REDIS_PORT_6379_TCP_ADDR" -p "$REDIS_PORT_6379_TCP_PORT"'
```

### Dockerfile
* [2.6 版本](https://github.com/docker-library/redis/blob/02d9cd887a4e0d50db4bb085eab7235115a6fe4a/2.6.17/Dockerfile)
* [最新 2.8 版本](https://github.com/docker-library/redis/blob/d0665bb1bbddd4cc035dbc1fc774695fa534d648/2.8.13/Dockerfile)
