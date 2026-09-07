# Forwarding, NAT, proxies, and time synchronization

## IP forwarding and NAT

```bash
# Enable IPv4 forwarding immediately, until reboot
sudo sysctl -w net.ipv4.ip_forward=1

# Create a persistent forwarding configuration
sudoedit /etc/sysctl.d/99-forwarding.conf

# Apply all persistent sysctl settings
sudo sysctl --system

# Verify forwarding parameters
sysctl net.ipv4.ip_forward
sysctl net.ipv6.conf.all.forwarding

# Forward an incoming TCP port to an internal server
sudo iptables -t nat -A PREROUTING -i <external-interface> \
  -p tcp --dport <public-port> \
  -j DNAT --to-destination <internal-ip>:<internal-port>

# Rewrite source addresses for traffic leaving through the external interface
sudo iptables -t nat -A POSTROUTING -s <internal-cidr> \
  -o <external-interface> -j MASQUERADE

# Persist compatible iptables rules on Ubuntu after reviewing them
sudo netfilter-persistent save
```

DNAT changes the destination of incoming traffic; masquerading performs source NAT so return traffic follows the forwarding host. Packet forwarding, firewall permission, routing, and persistent rules are all required for a durable setup.

## Nginx reverse proxy and load balancer

```nginx
# /etc/nginx/sites-available/<name>.conf
server {
    listen 80;

    location / {
        proxy_pass http://<backend-host>:<backend-port>;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```nginx
# Load-balance across primary servers and use a third only as backup
upstream app_servers {
    server <backend-1>:<port> weight=3;
    server <backend-2>:<port>;
    server <backend-3>:<port> backup;
}

server {
    listen 80;

    location / {
        proxy_pass http://app_servers;
    }
}
```

```bash
# Enable a site with a symbolic link
sudo ln -s /etc/nginx/sites-available/<name>.conf \
  /etc/nginx/sites-enabled/<name>.conf

# Validate all Nginx configuration before applying it
sudo nginx -t

# Reload without dropping established connections
sudo systemctl reload nginx.service
```

The reverse proxy is the client-facing endpoint; upstream servers remain behind it. Preserve forwarding headers so the application can reconstruct the original host, scheme, and client chain, and configure which proxies it trusts.

## Time zones and NTP

```bash
# List known time zones
timedatectl list-timezones

# Set the system time zone
sudo timedatectl set-timezone <Area/City>

# Show local/UTC time, time zone, and synchronization state
timedatectl

# Enable network time synchronization
sudo timedatectl set-ntp true

# Inspect the systemd time-sync daemon
systemctl status systemd-timesyncd.service

# Configure preferred and fallback NTP servers
sudoedit /etc/systemd/timesyncd.conf

# Apply changed time-server settings
sudo systemctl restart systemd-timesyncd.service

# Show the selected server and synchronization properties
timedatectl show-timesync

# Show a human-readable synchronization status
timedatectl timesync-status
```

Consistent time is essential for log correlation, certificate validation, distributed systems, and incident timelines. UTC is usually the least surprising server time zone across regions.
