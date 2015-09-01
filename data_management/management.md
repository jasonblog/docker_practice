## 利用資料卷容器來備份、恢復、遷移資料卷
可以利用資料卷對其中的資料進行進行備份、恢復和遷移。

### 備份
首先使用 `--volumes-from` 標記來建立一個載入 dbdata 容器卷的容器，並從本地主機掛載當前到容器的 /backup 目錄。命令以下：
```
$ sudo docker run --volumes-from dbdata -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```
容器啟動後，使用了 `tar` 命令來將 dbdata 卷備份為本地的 `/backup/backup.tar`。


### 恢復
如果要恢復資料到一個容器，首先建立一個帶有資料卷的容器 dbdata2。
```
$ sudo docker run -v /dbdata --name dbdata2 ubuntu /bin/bash
```
然後建立另一個容器，掛載 dbdata2 的容器，並使用 `untar` 解壓備份檔案到掛載的容器卷中。
```
$ sudo docker run --volumes-from dbdata2 -v $(pwd):/backup busybox tar xvf
/backup/backup.tar
```
