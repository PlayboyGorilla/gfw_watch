服务器在运行过程中收到疑似来自GFW的端口扫描。具有如下的特征

## 端口扫描特征  

### 服务器在短时间内（通常在几秒内）收到来自多个ip的tcp连接请求   
服务器日志片段：
```
...
2022/10/08 02:07:58: WARNING: 144.202.112.137:43246 (US) handshake failed
...
2022/10/08 02:07:59: WARNING: 185.230.210.252:51454 (SA) handshake failed
2022/10/08 02:07:59: WARNING: 61.164.253.69:55018 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 185.93.245.159:44980 (AE) handshake failed
2022/10/08 02:07:59: WARNING: 82.157.45.227:37490 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 82.157.24.196:49978 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 82.157.22.250:48942 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 101.42.134.30:40314 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 82.157.15.212:56688 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 43.140.210.123:53654 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 82.157.16.56:45978 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 43.138.6.40:54046 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 185.241.124.155:53882 (AE) handshake failed
2022/10/08 02:07:59: WARNING: 43.140.211.241:41478 (CN) handshake failed
2022/10/08 02:07:59: WARNING: 47.100.164.32:47372 (CN) handshake failed
...
```

### 对端在完成tcp三次握手后立刻进行四次握手断开（非rst断开），服务器没有收到任何来自对端的数据
tcpdump抓包
![tcpdump sniffer trace](img/gfw_port_scan.png "TCP port scan from 144.202.112.137")

### 目前仅在bandwagon的vps上收到过此类tcp端口扫描，其他vps供应商没有看到

### 扫描的周期大约为五天，但不固定，偶尔会六天甚至七天
来自同一ip的不同时间段的端口扫描
```
...
2022/09/12 02:37:31: WARNING: 144.202.112.137:32952 (US), handshake failed
2022/09/17 02:30:52: WARNING: 144.202.112.137:36918 (US), handshake failed
2022/09/22 03:56:24: WARNING: 144.202.112.137:44610 (US), handshake failed
2022/09/27 02:37:03: WARNING: 144.202.112.137:35822 (US), handshake failed
2022/10/02 03:21:37: WARNING: 144.202.112.137:44022 (US), handshake failed
2022/10/08 02:07:58: WARNING: 144.202.112.137:43246 (US), handshake failed
2022/10/14 07:28:23: WARNING: 144.202.112.137:50494 (US), handshake failed
...
```
可以看到间隔时间约为五天，后两次隔间为六天。   
由此推测每次端口扫描都由人工下命令发起。用于扫描的各机器接受指令后同步执行

## 扫描发起ip特征
总共收集到62个ip地址:
|IP|国家|提供商|
|---|---|---|
|104.36.184.122|US|IT7 Networks|
|144.202.112.137|US|Vultr|
|174.137.48.255|US|IT7 Networks|
|165.227.48.82|US|Digital Ocean|
|173.255.209.253|US|Linode|
|35.197.10.99|US|Google|
|169.54.194.61|US|Dmytro Postryhan|
|23.237.26.69|US|FDCservers.net LLC|
|24.86.248.28|CA|Shaw Comms|
|168.235.67.174|US|InMotion Hosting|
|50.7.8.99|US|FDCservers.net LLC|
|192.95.19.164|CA|OVH Hosting|
|147.135.22.28|US|OVH Hosting|
|45.77.19.32|JP|Vultr|
|35.221.248.87|TW|Google|
|81.4.125.158|NL|RouteLabel V.O.F.|
|50.7.114.87|US|FDCservers.net LLC|
|116.203.131.24|DE|Hetzner Online AG|
|51.158.147.91|NL|Scaleway|
|51.15.179.93|FR|Scaleway|
|185.243.217.223|NO|TerraHost AS|
|192.165.67.112|SE|Resilans AB|
|196.46.190.56|EG|City Net Telecom|
|185.123.101.33|TR|BrainStorm Network|
|147.78.2.180|IL|BrainStorm Network|
|185.106.103.26|CY|CloudLayer8 Limited|
|195.16.73.33|NG|TerraHost AS|
|1.116.218.46|CN|Tencent Cloud|
|128.199.126.228|SG|Digital Ocean|
|61.164.253.69|CN|Yunqikeji CoLtd|
|65.20.71.66|IN|Vultr|
|185.230.210.252|SA|Al-Kam Telecomm|
|185.93.245.159|AE|Bamboozle|
|43.138.120.23|CN|Tencent Cloud|
|82.157.16.56|CN|Tencent Cloud|
|82.157.22.250|CN|Tencent Cloud|
|43.138.6.40|CN|Tencent Cloud|
|47.105.221.174|CN|Aliyun|
|43.140.210.123|CN|Tencent Cloud|
|101.42.134.30|CN|Tencent Cloud|
|43.140.211.241|CN|Tencent Cloud|
|82.157.24.196|CN|Tencent Cloud|
|82.157.15.212|CN|Tencent Cloud|
|120.79.217.127|CN|Aliyun|
|117.28.245.245|CN|ChinaNet|
|134.175.60.96|CN|Tencent Cloud|
|82.157.45.227|CN|Tencent Cloud|
|47.100.201.195|CN|Aliyun|
|45.76.118.224|AU|Vultr|
|47.100.109.145|CN|Aliyun|
|47.100.212.169|CN|Aliyun|
|185.241.124.155|AE|Buzinessware FZCO|
|47.101.65.208|CN|Aliyun|
|47.100.161.30|CN|Aliyun|
|47.96.79.76|CN|Aliyun|
|47.100.164.32|CN|Aliyun|
|47.100.223.153|CN|Aliyun|
|39.106.209.134|CN|Aliyun|
|47.100.212.183|CN|Aliyun|
|185.140.251.177|SA|Buzinessware FZCO|
|101.132.235.87|CN|Aliyun|
|185.170.8.163|IR|Sefroyek Pardaz|
