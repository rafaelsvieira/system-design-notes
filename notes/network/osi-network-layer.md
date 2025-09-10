  # OSI Network Layer (Layer 3)

## Overview
The Network Layer is responsible for **routing packets** between different networks. It determines the best path for data to travel from source to destination across multiple network segments.

## Key Functions
- **Logical Addressing**: Uses IP addresses to identify devices globally
- **Routing**: Determines optimal path through interconnected networks  
- **Packet Forwarding**: Moves packets from one network interface to another
- **Fragmentation**: Breaks large packets into smaller ones when needed
- **Path Determination**: Selects best route based on metrics (distance, cost, etc.)

## Main Protocols

### Internet Protocol (IP)
- **IPv4**: 32-bit addresses (192.168.1.1)
- **IPv6**: 128-bit addresses (2001:db8::1)
- **Functions**: Addressing, fragmentation, TTL management

### Routing Protocols
- **Static Routing**: Manually configured routes
- **Dynamic Routing**:
  - **RIP** (Routing Information Protocol): Distance-vector, hop count
  - **OSPF** (Open Shortest Path First): Link-state, cost-based
  - **BGP** (Border Gateway Protocol): Path-vector, inter-AS routing

### Support Protocols
- **ICMP** (Internet Control Message Protocol): Error reporting, diagnostics
- **ARP** (Address Resolution Protocol): IP-to-MAC address mapping
- **DHCP** (Dynamic Host Configuration Protocol): Automatic IP assignment

## Key Concepts

### Routing Tables
```
Destination     Gateway         Interface    Metric
192.168.1.0/24  0.0.0.0        eth0         1
10.0.0.0/8      192.168.1.1    eth0         2
0.0.0.0/0       192.168.1.1    eth0         1
```

### Packet Structure (IPv4)
- **Header**: 20-60 bytes (version, length, TTL, checksum, addresses)
- **Payload**: Variable length data from upper layers
- **TTL**: Prevents infinite loops by decrementing at each hop

### Subnetting
- **Purpose**: Divide networks into smaller segments
- **CIDR Notation**: 192.168.1.0/24 (24-bit network mask)
- **Benefits**: Efficient addressing, improved security, reduced broadcast domains

## Routing Process
1. **Destination Analysis**: Check if target is on local network
2. **Route Lookup**: Search routing table for best match
3. **Next Hop**: Forward to appropriate gateway/interface
4. **TTL Decrement**: Reduce TTL, drop if zero
5. **Forward Packet**: Send to next router or final destination

## Network Address Translation (NAT)
- **Purpose**: Map private IPs to public IPs
- **Types**: Static NAT, Dynamic NAT, PAT (Port Address Translation)
- **Benefits**: IP conservation, security through obscurity

## Dual Stack
- **Purpose**: Simultaneous support for both IPv4 and IPv6 protocols
- **Implementation**: Devices maintain separate IPv4 and IPv6 routing tables and addresses
- **Benefits**: Smooth transition from IPv4 to IPv6, backward compatibility
- **Challenges**: Increased complexity, potential for configuration errors
- **Examples**: Modern routers, web servers, and operating systems supporting both protocol stacks

## Common Issues
- **Routing Loops**: Packets circulate indefinitely
- **Black Holes**: Traffic dropped without notification  
- **Asymmetric Routing**: Different paths for forward/return traffic
- **MTU Mismatch**: Fragmentation issues across networks

## Layer 3 Devices
- **Routers**: Primary Layer 3 devices, make routing decisions
- **Layer 3 Switches**: Combine switching and routing capabilities
- **Firewalls**: Filter traffic based on IP addresses and protocols

## Command Examples

### Viewing Network Configuration
```bash
# Display IP configuration
ip addr show                    # Linux
ifconfig                       # Linux/macOS
ipconfig /all                  # Windows

# Show routing table
ip route show                  # Linux
route -n                       # Linux/macOS
route print                    # Windows
netstat -rn                    # All platforms
netstat -rn -f inet            # macOS (IPv4 only)
```

### Testing Connectivity
```bash
# Basic connectivity test
ping 8.8.8.8                   # Test reachability
ping -c 4 google.com           # Linux/macOS (4 packets)
ping -n 4 google.com           # Windows (4 packets)

# Trace packet path
traceroute google.com          # Linux/macOS
tracert google.com             # Windows
mtr google.com                 # Linux (continuous trace)

# Test specific ports
telnet 8.8.8.8 53             # Test DNS port
nc -v google.com 80           # Linux/macOS netcat
```

### Analyzing Routes
```bash
# Add/remove static routes
ip route add 10.0.0.0/8 via 192.168.1.1    # Linux
route add -net 10.0.0.0/8 192.168.1.1      # macOS
route add 10.0.0.0 mask 255.0.0.0 192.168.1.1  # Windows

# Delete routes
ip route del 10.0.0.0/8                    # Linux
route del -net 10.0.0.0/8                  # macOS
route delete 10.0.0.0                      # Windows
```

### Troubleshooting Tools
```bash
# ARP table management
arp -a                         # Show ARP cache
ip neigh show                  # Linux ARP table
arp -d 192.168.1.100          # Delete ARP entry
arp -s 192.168.1.100 aa:bb:cc:dd:ee:ff  # Static ARP

# Network statistics
netstat -i                     # Interface statistics
ss -tuln                       # Linux socket statistics
netstat -an                    # All connections

# ICMP diagnostics
ping -R 8.8.8.8               # Record route (IPv4)
ping -f 8.8.8.8               # Flood ping (careful!)
ping -s 1472 8.8.8.8          # Test MTU size
```

### Advanced Diagnostics
```bash
# Packet capture for Layer 3 analysis
tcpdump -i eth0 icmp                    # Capture ICMP packets
tcpdump -i any host 8.8.8.8            # Capture traffic to/from host
wireshark                               # GUI packet analyzer

# DNS and routing
nslookup google.com                     # DNS lookup
dig google.com                          # Detailed DNS query
host google.com                         # Simple DNS lookup

# Network path analysis
pathping 8.8.8.8                       # Windows path analysis
hping3 -S -p 80 google.com             # Custom packet crafting
```

### Router/Switch Commands (Cisco)
```bash
# Cisco router/switch commands
show ip route                           # Display routing table
show ip interface brief                 # Interface status
show arp                               # ARP table
ping 8.8.8.8 source loopback0         # Ping from specific interface
traceroute 8.8.8.8                    # Trace route path

# Configuration
ip route 0.0.0.0 0.0.0.0 192.168.1.1  # Default route
no ip route 10.0.0.0 255.0.0.0        # Remove route
```

### Monitoring and Analysis
```bash
# Real-time monitoring
iftop                          # Interface traffic monitor
nethogs                       # Process network usage
iotop                         # I/O monitoring
watch -n 1 'ip route show'    # Monitor route changes

# Performance testing
iperf3 -s                     # Server mode
iperf3 -c 192.168.1.100       # Client test to server
speedtest-cli                 # Internet speed test
```