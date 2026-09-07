# Network addressing, services, and firewalls

## Addressing and routes

IPv4 addresses contain 32 bits and IPv6 addresses contain 128. In CIDR notation, `/24` means the first 24 bits identify the network; the remaining bits identify addresses within it.

```bash
# Show interfaces and assigned IPv4 and IPv6 addresses
ip address show

# Show link state and interface names
ip link show

# Show the kernel routing table and default gateway
ip route show

# Show the route the kernel would use for one destination
ip route get <destination-ip>

# Show DNS resolver status on a systemd-resolved host
resolvectl status

# Test basic IP reachability with a bounded packet count
ping -c 4 <host-or-ip>

# Trace the network path without requiring root
tracepath <host-or-ip>

# Query DNS directly
dig <hostname>
```

Addresses on the same subnet communicate directly; traffic for other subnets follows a route, commonly through the default gateway. A bridge joins network segments at layer 2, while a bond presents multiple links as one logical interface for redundancy, distribution, or both.

## Listening services and connections

```bash
# List listening TCP and UDP sockets numerically with owning processes
sudo ss -ltunp

# List established TCP connections
ss -tn

# Show socket details and timers
ss -tno

# Inspect a process discovered through a socket's PID
ps -fp <pid>

# Show files and sockets opened by that process
sudo lsof -p <pid>

# Check, stop, and start the service behind a listening socket
systemctl status <name>.service
sudo systemctl stop <name>.service
sudo systemctl start <name>.service
```

`127.0.0.1:<port>` accepts local IPv4 connections only. `0.0.0.0:<port>` listens on all IPv4 addresses, while `[::]:<port>` is the corresponding IPv6 wildcard.

## Host firewall with UFW

```bash
# Show UFW state and numbered rules
sudo ufw status numbered

# Permit SSH before enabling the firewall on a remote server
sudo ufw allow 22/tcp

# Enable the firewall
sudo ufw enable

# Allow one TCP port only from a trusted subnet
sudo ufw allow from <source-cidr> to any port <port> proto tcp

# Remove a rule by its displayed number
sudo ufw delete <rule-number>

# Apply a default-deny policy for unsolicited incoming traffic
sudo ufw default deny incoming

# Permit routed traffic between specified networks or hosts
sudo ufw route allow from <source-cidr> to <destination-ip>
```

When administering remotely, validate the SSH allow rule and keep an existing session open before enabling or changing firewall policy.

## Inspect packet-filter rules

```bash
# Display the active nftables ruleset
sudo nft list ruleset

# Display iptables filter rules with counters and numeric addresses
sudo iptables -L -n -v

# Display rules in the NAT table
sudo iptables -t nat -L -n -v

# Display replayable NAT-table commands
sudo iptables --list-rules --table nat
```

UFW, iptables, and nftables may be different interfaces to overlapping kernel packet-filtering state. Determine which tool owns persistence before modifying rules through another interface.
