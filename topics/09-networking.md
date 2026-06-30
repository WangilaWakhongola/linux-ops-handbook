# 9. Networking

## Checking network configuration

```bash
ip addr                  # show IP addresses (modern)
ip a                        # short form
ifconfig                       # older equivalent (may need net-tools)
ip route                          # show routing table
hostname -I                          # show this machine's IP(s)
```

## Connectivity testing

```bash
ping google.com               # test reachability
ping -c 4 google.com             # send only 4 pings
traceroute google.com               # trace the path to a host
mtr google.com                         # combined ping + traceroute, live
```

## DNS

```bash
nslookup google.com         # basic DNS lookup
dig google.com                 # detailed DNS lookup
dig +short google.com            # just the IP
cat /etc/resolv.conf                # current DNS servers in use
```

## Ports and connections

```bash
ss -tulpn               # list listening ports + owning process
netstat -tulpn             # older equivalent
sudo lsof -i :8080            # what's using port 8080
curl -I https://example.com      # check HTTP headers/response
telnet host 22                      # test if a TCP port is open
nc -zv host 22                         # faster port check with netcat
```

## Firewall (ufw - Ubuntu / firewalld - RHEL)

```bash
sudo ufw status              # check firewall status (Ubuntu)
sudo ufw allow 22                  # allow SSH
sudo ufw enable                       # turn on firewall

sudo firewall-cmd --list-all         # check firewall status (RHEL)
sudo firewall-cmd --add-port=80/tcp --permanent   # open a port (RHEL)
sudo firewall-cmd --reload             # apply RHEL firewall changes
```

## Transferring files over network

```bash
scp file.txt user@host:/path/        # secure copy to remote host
scp user@host:/path/file.txt .            # secure copy from remote host
rsync -avz dir/ user@host:/path/             # efficient sync, preserves attrs
wget https://example.com/file.zip                # download a file
curl -O https://example.com/file.zip                # download, alternative
```

## Common gotchas

- `ping` working doesn't mean a service is reachable — ICMP and the actual service port are separate; check with `nc` or `curl`.
- Firewall changes that aren't `--permanent` (firewalld) or saved (ufw) vanish on reboot.
- `ss`/`netstat` showing a port as listening doesn't mean it's reachable externally — local firewall or cloud security groups can still block it.
