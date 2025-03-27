# TCP Dump Lab Exercise

## Objective
Learn how to use tcpdump to capture, filter, and analyze network traffic in a Linux environment.

## Lab Setup

### 1. Create a Linux Virtual Machine in VirtualBox

1. Download and install VirtualBox from [virtualbox.org](https://www.virtualbox.org/wiki/Downloads)
2. Download Ubuntu Server ISO from [ubuntu.com](https://ubuntu.com/download/server)
3. Create a new VM in VirtualBox:
   - Name: TcpdumpLab
   - Type: Linux
   - Version: Ubuntu (64-bit)
   - Memory: 2048 MB
   - Create a virtual hard disk (20 GB)
4. Configure network settings:
   - Adapter 1: NAT (for internet access)
   - Adapter 2: Host-only adapter (for isolated lab environment)
5. Install Ubuntu Server following the on-screen instructions
   - Choose minimal installation
   - Install OpenSSH server when prompted

### 2. Set Up tcpdump

1. Log in to your VM
2. Update the package lists:
   ```
   sudo apt update
   ```
3. Install tcpdump:
   ```
   sudo apt install tcpdump -y
   ```
4. Verify installation:
   ```
   tcpdump --version
   ```

## Lab Exercises

### Exercise 1: Basic Packet Capture

1. List available network interfaces:
   ```
   sudo tcpdump -D
   ```

2. Capture packets on the primary interface (usually eth0):
   ```
   sudo tcpdump -i eth0
   ```

3. Capture a specific number of packets:
   ```
   sudo tcpdump -i eth0 -c 10
   ```

4. Save captured packets to a file:
   ```
   sudo tcpdump -i eth0 -c 100 -w capture.pcap
   ```

5. Read packets from a saved file:
   ```
   sudo tcpdump -r capture.pcap
   ```

### Exercise 2: Filtering Traffic

1. Filter by host:
   ```
   sudo tcpdump -i eth0 host 8.8.8.8
   ```

2. Filter by source or destination:
   ```
   sudo tcpdump -i eth0 src 10.0.2.15
   sudo tcpdump -i eth0 dst 8.8.8.8
   ```

3. Filter by port:
   ```
   sudo tcpdump -i eth0 port 80
   sudo tcpdump -i eth0 port 443
   ```

4. Filter by protocol:
   ```
   sudo tcpdump -i eth0 icmp
   sudo tcpdump -i eth0 tcp
   ```

5. Combine filters with logical operators:
   ```
   sudo tcpdump -i eth0 'tcp and port 80'
   sudo tcpdump -i eth0 'host 8.8.8.8 and icmp'
   sudo tcpdump -i eth0 'tcp and (port 80 or port 443)'
   ```

### Exercise 3: Advanced Capture and Analysis

1. Display verbose output:
   ```
   sudo tcpdump -i eth0 -v
   sudo tcpdump -i eth0 -vv
   sudo tcpdump -i eth0 -vvv
   ```

2. View packet contents (ASCII and hex):
   ```
   sudo tcpdump -i eth0 -X -c 5
   ```

3. Capture HTTP headers:
   ```
   sudo tcpdump -i eth0 -A 'tcp port 80'
   ```

4. Filter by TCP flags (SYN packets):
   ```
   sudo tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn) != 0'
   ```

5. Filter by packet size:
   ```
   sudo tcpdump -i eth0 'greater 1000'
   sudo tcpdump -i eth0 'less 128'
   ```

### Exercise 4: Real-World Scenarios

1. Capture DNS traffic:
   ```
   sudo tcpdump -i eth0 udp port 53
   ```

2. Monitor SSH connections:
   ```
   sudo tcpdump -i eth0 port 22
   ```

3. Detect network scanning:
   ```
   sudo tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0'
   ```

4. Capture HTTP GET requests:
   ```
   sudo tcpdump -i eth0 -A 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
   ```

5. Generate traffic for analysis:
   ```
   # In a separate terminal
   ping 8.8.8.8
   curl http://example.com
   ssh username@hostname
   ```

## Submission Requirements

1. Screenshots of 1 exercise showing the command and its output

## Resources
- [Daniel Miessler's tcpdump Tutorial](https://danielmiessler.com/blog/tcpdump)
- [tcpdump Man Page](https://www.tcpdump.org/manpages/tcpdump.1.html)
- [Wireshark Display Filters](https://wiki.wireshark.org/DisplayFilters)