## 自定義橋接器
除了預設的 `docker0` 橋接器，使用者也可以指定橋接器來連接各個容器。

在啟動 Docker 服務的時候，使用 `-b BRIDGE`或`--bridge=BRIDGE` 來指定使用的橋接器。

如果服務已經執行，那需要先停止服務，並刪除舊的橋接器。
```
$ sudo service docker stop
$ sudo ip link set dev docker0 down
$ sudo brctl delbr docker0
```
然後建立一個橋接器 `bridge0`。
```
$ sudo brctl addbr bridge0
$ sudo ip addr add 192.168.5.1/24 dev bridge0
$ sudo ip link set dev bridge0 up
```
查看確認橋接器建立並啟動。
```
$ ip addr show bridge0
4: bridge0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state UP group default
    link/ether 66:38:d0:0d:76:18 brd ff:ff:ff:ff:ff:ff
    inet 192.168.5.1/24 scope global bridge0
       valid_lft forever preferred_lft forever
```
設定 Docker 服務，預設橋接到建立的橋接器上。
```
$ echo 'DOCKER_OPTS="-b=bridge0"' >> /etc/default/docker
$ sudo service docker start
```
啟動 Docker 服務。
新建一個容器，可以看到它已經橋接到了 `bridge0` 上。

可以繼續用 `brctl show` 命令查看橋接的訊息。另外，在容器中可以使用 `ip addr` 和 `ip route` 命令來查看 IP 位址設定和路由訊息。
