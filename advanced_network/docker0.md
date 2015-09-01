## 設定 docker0 橋接器
Docker 服務預設會建立一個 `docker0` 橋接器（其上有一個 `docker0` 內部界面），它在核心層連通了其他的物理或虛擬網卡，這就將所有容器和本地主機都放到同一個物理網路。

Docker 預設指定了 `docker0` 界面 的 IP 位址和子網遮罩，讓主機和容器之間可以透過橋接器相互通信，它還給出了 MTU（界面允許接收的最大傳輸單元），通常是 1500 Bytes，或宿主主機網路路由上支援的預設值。這些值都可以在服務啟動的時候進行設定。
* `--bip=CIDR` -- IP 位址加遮罩格式，例如 192.168.1.5/24
* `--mtu=BYTES` -- 覆蓋預設的 Docker mtu 設定

也可以在設定檔案中設定 DOCKER_OPTS，然後重啟服務。
由於目前 Docker 橋接器是 Linux 橋接器，使用者可以使用 `brctl show` 來查看橋接器和連接埠連接訊息。
```
$ sudo brctl show
bridge name     bridge id               STP enabled     interfaces
docker0         8000.3a1d7362b4ee       no              veth65f9
                                             vethdda6
```
*註：`brctl` 命令在 Debian、Ubuntu 中可以使用 `sudo apt-get install bridge-utils` 來安裝。


每次建立一個新容器的時候，Docker 從可用的位址段中選擇一個未使用的 IP 位址分配給容器的 eth0 連接埠。使用本地主機上 `docker0` 界面的 IP 作為所有容器的預設網關。
```
$ sudo docker run -i -t --rm base /bin/bash
$ ip addr show eth0
24: eth0: <BROADCAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 32:6f:e0:35:57:91 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::306f:e0ff:fe35:5791/64 scope link
       valid_lft forever preferred_lft forever
$ ip route
default via 172.17.42.1 dev eth0
172.17.0.0/16 dev eth0  proto kernel  scope link  src 172.17.0.3
$ exit
```
