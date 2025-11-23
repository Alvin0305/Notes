We can see the ip address of the lap using the command

```bash
ip addr
```

```bash
alvin@fedora:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eno1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 48:ea:62:93:58:c8 brd ff:ff:ff:ff:ff:ff
    altname enp2s0
    altname enx48ea629358c8
3: wlo1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether fe:84:01:6a:9c:ad brd ff:ff:ff:ff:ff:ff permaddr 38:8d:3d:14:02:c5
    altname wlp4s0
    altname wlx388d3d1402c5
    inet 172.21.13.90/20 brd 172.21.15.255 scope global dynamic noprefixroute wlo1
       valid_lft 1519sec preferred_lft 1519sec
    inet6 fe80::8fa:bde:819b:caec/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default 
    link/ether a6:42:14:8f:e3:62 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever

```

- The above output means:
#### lo - loopback interface
- used for internal communication within the machine
- localhost => 127.0.0.1
- When a program is connected to 127.0.0.1, it's talking to itself(no data goes out of the NIC)
- [[Units#MTU|MTU]] will be very high since it is virtual
- Used if we want to check our own website (frontend and backend)

#### eno1 - Wired Ethernet (LAN) Interface
- Purpose: This is the Ethernet (wired) NIC
- Status: NO-CARRIER means no cable is plugged in (or no link detected)
- MAC Address: 48:ea:62:93:58:c8
- altname (enp2s0, enx48ea629358c8) are just system assigned aliases
- If we plugin a LAN cable, this interface would come up

#### wlo1 - Wireless Wi-Fi Interface
- Purpose: Your wifi card
- Status: UP and LOWER_UP means its connected to a network
- IP Address: 172.21.13.90 (assigned by campus router)
- Subnet Mask: /20 means the network range is 172.21.0.0 - 127.21.15.255
- MAC Address: fe:84:01:6a:9c:ad (the device ID)
- [[Units#Link Local IPv6 address|Link Local IPv6 address]]: fe80::8fa:bde:819b:caec/64
- This is the active internet interface -> all traffic goes through this

#### docker0 - Docker Virtual Bridge
- Purpose: A virtual network interface created by docker
- IP Address: 127.17.0.1 acts as a virtual gateway for Docker containers
- Role: Connects your container to each other and to the host
- NO-CARRIER because no containers are currently attached
- Used when Docker is running containers that need networking










