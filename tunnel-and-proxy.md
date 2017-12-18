# Tunneling and Proxies

SSH Tunnels
Dynamic SOCKS Proxy

This can be used with proxychains to forward client traffic through the remote server.

```bash
ssh -D8080 [user]@[host]
```

## Local Port Forwarding

This will bind to [bindaddr]:[port] on the client and forward through the SSH server to the [dsthost]:[dstport]
```bash
ssh -L [bindaddr]:[port]:[dsthost]:[dstport] [user]@[host]
```
Example: Create an encrypted tunnel between our machine and 192.168.1.12 on local port 4444. Forward all trafic that exits
the other end to 192.168.1.15 port 3389 (RDP port)
```bash
ssh tom@192.168.1.12 -L 4444:192.168.1.15:3389
```

## Remote Port Forwarding

This will bind to [bindaddr]:[port] on the remote server and tunnel traffic through the ssh client side to [localhost]:[localport]

```bash
ssh -R [bindaddr]:[port]:[localhost]:[localport] [user]@[host]
```

Establish VPN over SSH The following options must be enabled on the server side.

PermitRootLogin yes
PermitTunnel yes
ssh [user]@[host] -w any:any

You can see the established tun interface by typing ifconfig -a

The interfaces and forwarding must still be configured. This assumes that we are going to forward 10.0.0.0/24 through the remote server. We are also assuming that the server’s main connection is through eth0, and both client/server stood up tun0. This may be different if you already have existing VPN connections.

### Client

ip addr add 192.168.5.2/32 peer 192.168.5.1 dev tun0
#### Once Server is setup, run the following to add routes
route add -net 10.0.0.0/24 gw 192.168.5.1

###Server

ip addr add 192.168.5.1/32 peer 192.168.5.2 dev tun0
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -A POSTROUTING -s 192.168.5.1 -o eth0 -j MASQUERADE


## Proxychains
The configuration file in /etc/proxychains.conf must be edited to point towards your SOCKS proxy. Typically this is done with 
an SSH or other type of tunnel. Make sure your ports match.

[ProxyList]
socks4 localhost 8080
Now, in order to run any type of network through the proxy just run it like so. Remember, you can’t run any raw socket scans through a SOCKS4 proxy. You need to setup an SSH VPN tunnel or something similar for that type of functionality.

proxychains nmap 192.168.5.6