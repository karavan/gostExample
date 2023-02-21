
# gost Example
Help you to use gost.  
gost is a very good tunnel tool.  
But it's document is not very clear.  
And gost itself is very complicated, because it's powerful.  

Here, I want to show some examples to help ordinary users to use gost.  


## Introduce
version 2 is here https://github.com/ginuerzh/gost  
version 3 is here https://github.com/go-gost/gost  
**version 3 is now under developing. Not for publishing. **

Offical doc site for v2: https://v2.gost.run/  
Offical doc site for v3: https://gost.run/


## Download and run
Open release page to down the binaries for your platform.  
v2: https://github.com/ginuerzh/gost/releases  
v3: https://github.com/go-gost/gost/releases  

On windows, if you don't want to see the black terminal, you can use [gostGUI](https://github.com/woodlyer/gostGUI) to run gost.exe in the background. 
On Android, May be you can use [ShadowsocksGostPlugin](https://github.com/segfault-bilibili/ShadowsocksGostPlugin) .  
On IOS, May be you can use [shadowrocket](https://www.applevis.com/apps/ios/utilities/shadowrocket) .  


## General Net Knowlege

gost is named from "GO Simple Tunnel", and it's always used as a tunnel.  
Although gost can works as a proxy.  

When gost works as tunnel, the network is like this.  
Gost client and gost server set up a tunnel to serve for proxy server run on.
![net](./tunnel.png)


## gost Tunnel Example

The first line is for gost server, running on VPS.  
The second line is for gost client, running on your PC.

Suppose you are running SS(shadowsocks) or v2ray on 8388, on the client side, the gost tunnel works on 127.0.0.1:8083 links to SS or V2ray on your server.  

You should modified the server_ip to your own domain name or ip address. 

- kcp tunnel (I recommend you use this to speed up and secure)   
kcp protocal is based on udp.  
kcp can speed up your connection and keep your connection secure.

```
# server,  ss or v2ray listen on 8083 
./gost -L kcp://:9000/:8083 
./gost -L tcp://127.0.0.1:8083  -F forward+kcp://server_ip:9000
```
If you want to change some parameter of kcp. you can write a kcp.json and append it into cmd.  like this:  
```
./gost -L kcp://:9000/:8083?c=./kcp.json 
./gost -L tcp://127.0.0.1:8083  -F forward+kcp://server_ip:9000?c=./kcp.json
```

I recommend you to change the kcp.json. Not to use default parameter.  
More info about kcp parameter. see: https://github.com/xtaci/kcptun
``` json
{
    "key": "it's a secrect",
    "crypt": "aes",
    "mode": "fast",
    "mtu" : 1350,
    "sndwnd": 1024,
    "rcvwnd": 1024,
    "datashard": 10,
    "parityshard": 3,
    "dscp": 0,
    "nocomp": false,
    "acknodelay": false,
    "nodelay": 0,
    "interval": 40,
    "resend": 0,
    "nc": 0,
    "sockbuf": 4194304,
    "keepalive": 10,
    "snmplog": "",
    "snmpperiod": 60,
    "tcp": false
}
```




- tls tunnel

```
./gost -L tls://:443/:8083
./gost -L=tcp://:8083 -F forward+tls://server_ip:443
```


- quic tunnel
```
./gost -L quic://:1443/:8083
./gost -L tcp://127.0.0.1:8083  -F "forward+quic://server_ip:1443"
```

- dtls tunnel.  
dtls in only available in v3. Not tested.
```
./gost -L dtls://:1443/:8083
./gost -L tcp://127.0.0.1:8083  -F "forward+dtls://server_ip:1443"
```

- icmp tunnel.   only for v3
```
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
./gost -L icmp://:0
./gost -L :8080 -F "relay+icmp://server_ip:12345?keepAlive=true&ttl=10s"


```

## gost Proxy Examples

gost act as a socks5 proxy.  
you can connect socks5://127.0.0.1:8080 to connect the internet.
- tls proxy
```
./gost -L tls://:443
./gost -L :8080 -F tls://server_ip:443
```

- mtls proxy
```
./gost -L mtls://:443
./gost -L :8080 -F mtls://server_ip:443
```

- kcp proxy
```
./gost -L=kcp://:8388
./gost -L=:8080 -F=kcp://server_ip:8388
```

- kcp proxy with fake tcp
```
./gost -L=kcp://:8388?tcp=true
./gost -L=:8080 -F=kcp://server_ip:8388?tcp=true
```


## Some tips

- run gost at background in Linux
use nohup to run gost in background and the log redirect to gost.log  
``` 
  nohup ./gost -L mtls://:443  >> gost.log  2>&1 &
```














## Don't have a VPS?
Oh, It's very easy. Buy one.

- [bandwagonhost](https://bandwagonhost.com/aff.php?aff=56257)   $49.9 for 1 year.
- [vultr.com](https://www.vultr.com/?ref=7621285)  Easy to use.
- [DMIT](https://www.dmit.io/)   Many data center.
- [racknerd.com](https://my.racknerd.com/aff.php?aff=3278) It's very cheap. Click this link to buy  is cheap [BlackFriday](https://www.racknerd.com/BlackFriday/).  $10.28 for 1 year.







## Don't know how to do?
If you have read this document and don't know how to use gost, maybe  you don't need to waste some more time on it.  
Please use some commercial mature VPN service.   
Such as:
- 1.[justMysocks.net](https://justmysocks.net/members/aff.php?aff=24386)   
- 2.[ExpressVPN](https://www.expressvpn.com/) 
- 3.[StrongVPN](https://www.strongvpn.com) 





## Welcome Pull Requests







