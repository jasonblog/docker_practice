## [MongoDB](https://registry.hub.docker.com/_/mongo/)

### 基本訊息
[MongoDB](https://en.wikipedia.org/wiki/MongoDB) 是開源的 NoSQL 資料庫實作。
該倉庫提供了 MongoDB 2.2 ~ 2.7 各個版本的映像檔。

### 使用方法
預設會在 `27017` 連接埠啟動資料庫。
```
$ sudo docker run --name some-mongo -d mongo
```

使用其他應用連接到容器，可以用
```
$ sudo docker run --name some-app --link some-mongo:mongo -d application-that-uses-mongo
```
或者透過 `mongo`
```
$ sudo docker run -it --link some-mongo:mongo --rm mongo sh -c 'exec mongo "$MONGO_PORT_27017_TCP_ADDR:$MONGO_PORT_27017_TCP_PORT/test"'
```

### Dockerfile
* [2.2 版本](https://github.com/docker-library/mongo/blob/77c841472ccb6cc87fea1218269d097405edc6cb/2.2/Dockerfile)
* [2.4 版本](https://github.com/docker-library/mongo/blob/807078cb7b5f0289f6dabf9f6875d5318122bc30/2.4/Dockerfile)
* [2.6 版本](https://github.com/docker-library/mongo/blob/77c841472ccb6cc87fea1218269d097405edc6cb/2.6/Dockerfile)
* [2.7 版本](https://github.com/docker-library/mongo/blob/807078cb7b5f0289f6dabf9f6875d5318122bc30/2.7/Dockerfile)
