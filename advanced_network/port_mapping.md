## 映射容器連接埠到宿主主機的實作

預設情況下，容器可以主動存取到外部網路的連接，但是外部網路無法存取到容器。
### 容器存取外部實作
容器所有到外部網路的連接，源位址都會被NAT成本地系統的IP位址。這是使用 `iptables` 的源位址偽裝操作實作的。

查看主機的 NAT 規則。
```
$ sudo iptables -t nat -nL
...
Chain POSTROUTING (policy ACCEPT)
target     prot opt source               destination
MASQUERADE  all  --  172.17.0.0/16       !172.17.0.0/16
...
```
其中，上述規則將所有源位址在 `172.17.0.0/16` 網段，目標位址為其他網段（外部網路）的流量動態偽裝為從系統網卡發出。MASQUERADE 跟傳統 SNAT 的好處是它能動態從網卡取得位址。

### 外部存取容器實作

容器允許外部存取，可以在 `docker run` 時候透過 `-p` 或 `-P` 參數來啟用。

不管用那種辦法，其實也是在本地的 `iptable` 的 nat 表中新增相應的規則。

使用 `-P` 時：
```
$ iptables -t nat -nL
...
Chain DOCKER (2 references)
target     prot opt source               destination
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:49153 to:172.17.0.2:80
```

使用 `-p 80:80` 時：
```
$ iptables -t nat -nL
Chain DOCKER (2 references)
target     prot opt source               destination
DNAT       tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80 to:172.17.0.2:80
```
注意：
* 這裡的規則映射了 0.0.0.0，意味著將接受主機來自所有界面的流量。使用者可以透過 `-p IP:host_port:container_port` 或 `-p
IP::port` 來指定允許存取容器的主機上的 IP、界面等，以制定更嚴格的規則。
* 如果希望永久綁定到某個固定的 IP 位址，可以在 Docker 設定檔案 `/etc/default/docker` 中指定 `DOCKER_OPTS="--ip=IP_ADDRESS"`，之後重啟 Docker 服務即可生效。
