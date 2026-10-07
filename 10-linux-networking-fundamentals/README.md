# Linux Networking Fundamentals

## 1. Network Interfaces

```bash
ip link show    # List network interfaces and their state (UP/DOWN)
ip addr show    # Show interfaces with their IP addresses
```

## 2. Connectivity Testing

```bash
ping -c 4 127.0.0.1      # Ping localhost (tests your own network stack)
ping -c 4 8.8.8.8        # Ping Google DNS (tests internet access)
ip route | grep default  # Find the default gateway (router) IP
ping -c 4 <gateway-ip>   # Ping the gateway
```

### Troubleshooting Tips

- Ping gateway OK + ping Google fails = ISP/internet issue.
- Ping gateway fails = local network/Wi-Fi issue.

## 3. DNS

```bash
host google.com     # Look up the IP address of a domain
cat /etc/resolv.conf  # Show which DNS servers the system uses
dig -x 8.8.8.8      # Reverse lookup: find the domain name of an IP
```

## 4. Listening Ports (ss)

Flags: `-l` listening, `-t` TCP, `-u` UDP, `-n` numeric ports, `-p` show process (needs sudo).

```bash
ss -ltn                    # Listening TCP ports
ss -lun                    # Listening UDP ports
sudo ss -ltnp              # Listening TCP ports with process names
sudo ss -ltnp | grep :80   # Check what is listening on port 80
```

## 5. Downloading Files and HTTP Requests

```bash
wget <url>                       # Download a file
curl -I <url>                    # Show only the HTTP response headers
curl -o custom_name.txt http://localhost:8080/hello.txt  # Download and save with a custom name
```

## 6. Managing IP Addresses

```bash
ip addr show labex0                       # Show addresses on the labex0 interface
sudo ip addr add 10.10.10.10/24 dev labex0  # Add an IP address to labex0
sudo ip addr del 10.10.10.10/24 dev labex0  # Remove that IP address
```
