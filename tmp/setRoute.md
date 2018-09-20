
### windows
```
route add 172.21.5.0 mask 255.255.255.0 172.21.12.254 -p
route add 172.21.12.0 mask 255.255.255.0 172.21.12.254 -p
route add 172.21.4.0 mask 255.255.255.0 172.21.12.254 -p
route add 172.21.8.0 mask 255.255.255.0 172.21.12.254 -p

route add 192.168.0.0 mask 255.255.255.0 172.21.12.254 -p
route add 192.168.1.0 mask 255.255.255.0 172.21.12.254 -p
route add 192.168.2.0 mask 255.255.255.0 172.21.12.254 -p


route delete 172.21.5.0 mask 255.255.255.0


route add 172.21.5.0 mask 255.255.255.0 192.168.31.1 -p
route add 172.21.12.0 mask 255.255.255.0 192.168.31.1 -p
route add 172.21.4.0 mask 255.255.255.0 192.168.31.1 -p
route add 172.21.8.0 mask 255.255.255.0 192.168.31.1 -p

route add 192.168.0.0 mask 255.255.255.0 192.168.31.1 -p
route add 192.168.1.0 mask 255.255.255.0 192.168.31.1 -p
route add 192.168.2.0 mask 255.255.255.0 192.168.31.1 -p


route delete 172.21.5.0 mask 255.255.255.0
route delete 172.21.12.0 mask 255.255.255.0
route delete 172.21.4.0 mask 255.255.255.0
route delete 172.21.9.0 mask 255.255.255.0

route delete 192.168.0.0 mask 255.255.255.0
route delete 192.168.1.0 mask 255.255.255.0
route delete 192.168.2.0 mask 255.255.255.0
```
