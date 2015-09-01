## 儲存和載入映像檔

### 儲存映像檔
如果要建立映像檔到本地檔案，可以使用 `docker save` 命令。
```
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              14.04               c4ff7513909d        5 weeks ago         225.4 MB
...
$sudo docker save -o ubuntu_14.04.tar ubuntu:14.04
```

### 載入映像檔
可以使用 `docker load` 從建立的本地檔案中再匯入到本地映像檔庫，例如
```
$ sudo docker load --input ubuntu_14.04.tar
```
或
```
$ sudo docker load < ubuntu_14.04.tar
```
這將匯入映像檔以及其相關的元資料信息（包括標籤等）。
