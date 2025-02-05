# 🔍 Network Detective Academy: Traffic Investigation Lab

## Your Mission
As a trainee Network Detective, you'll investigate how computers talk to each other on a network. You'll set up your own detective lab, capture network conversations, and solve networking mysteries!

## Lab Setup Requirements
- Windows computer
- Multipass installed
- Administrator access
- At least 8GB RAM available
- About 1 hour of time

## Part 1: Setting Up Your Detective Lab 🚀

### Step 1: Prepare Your Evidence Folder
1. Open File Explorer
2. Create a new folder: `C:\network-evidence`

### Step 2: Create Your Virtual Detective Office
Open PowerShell as Administrator and run:
```powershell
# Mount your evidence folder
multipass mount C:\network-evidence detective-hq

# Create your monitoring station
multipass launch -n detective-hq -m 2G -c 2 20.04 --network name=mystery-net,mode=manual

# Create two test stations
multipass launch -n station1 -m 1G -c 1 20.04 --network name=mystery-net,mode=manual
multipass launch -n station2 -m 1G -c 1 20.04 --network name=mystery-net,mode=manual
```

### Step 3: Configure Your Network
Set up each station with its own address:

```bash
# Detective HQ setup
multipass shell detective-hq
sudo ip addr add 192.168.100.10/24 dev enp0s3
sudo apt update
sudo apt install -y tcpdump wireshark iperf3 net-tools

# Station 1 setup
multipass shell station1
sudo ip addr add 192.168.100.11/24 dev enp0s3
sudo apt update
sudo apt install -y iperf3 net-tools

# Station 2 setup
multipass shell station2
sudo ip addr add 192.168.100.12/24 dev enp0s3
sudo apt update
sudo apt install -y iperf3 net-tools
```

## Part 2: Learning to Use Your Detective Tools 🔧

### Using tcpdump
tcpdump is your special network detective glasses. Here's how to use it:

```bash
# On Detective-HQ, watch all network traffic:
sudo tcpdump -i enp0s3

# Watch only specific traffic:
sudo tcpdump -i enp0s3 icmp          # Only ping messages
sudo tcpdump -i enp0s3 broadcast     # Only broadcast messages
```

### Saving Evidence
Save network traffic to your evidence folder:
```bash
# On Detective-HQ:
sudo tcpdump -i enp0s3 -w /mnt/network-evidence/case1.pcap
```
The file will appear in your Windows `C:\network-evidence` folder!

## Part 3: Detective Cases to Solve! 🕵️‍♂️

### Case #1: The Secret Messages
**Mission**: Find out how computers discover each other's addresses!

Steps:
1. Start capturing on Detective-HQ:
```bash
sudo tcpdump -i enp0s3 -w /mnt/network-evidence/case1.pcap
```

2. From Station1, ping Station2:
```bash
ping 192.168.100.12
```

3. Stop the capture (Ctrl+C) and investigate:
```bash
sudo tcpdump -r /mnt/network-evidence/case1.pcap
```

Research Questions:
1. What happened before the first ping?
2. Why did this happen?
3. What is ARP and why is it needed?

### Case #2: The Broadcast Mystery
**Mission**: Investigate messages that go to everyone!

Steps:
1. Start a new capture:
```bash
sudo tcpdump -i enp0s3 broadcast -w /mnt/network-evidence/case2.pcap
```

2. Send a broadcast ping:
```bash
ping -b 192.168.100.255
```

Research Questions:
1. How is broadcast traffic different from normal traffic?
2. Why do networks need broadcast messages?
3. What could happen if there were too many broadcasts?

### Case #3: The Speed Test
**Mission**: Measure how fast your network can send messages!

Steps:
1. On Station1:
```bash
iperf3 -s
```

2. On Station2:
```bash
iperf3 -c 192.168.100.11
```

Research Questions:
1. What was the network speed?
2. What could affect network speed?
3. How does this compare to real networks?

## Part 4: Your Detective Report 📝

Create a report including:

1. Network Map
   - Draw your network
   - Show all stations and their IP addresses
   - Mark the connections between them

2. Evidence Analysis
   - What types of traffic did you see?
   - How do computers find each other?
   - What was the most interesting discovery?

3. Research Answers
   - Answer all research questions
   - Explain your findings
   - Include interesting observations

## Part 5: Cleanup (Close the Case) 🧹

When you're done:
```powershell
# In Windows PowerShell (Administrator):
multipass stop detective-hq station1 station2
multipass delete detective-hq station1 station2
multipass purge
```

## Help & Troubleshooting 🆘

If things go wrong:

1. Network Issues
   - Check IP addresses
   - Make sure all VMs are running
   - Try restarting VMs

2. File Access Issues
   - Check folder permissions
   - Make sure mount is working
   - Try remounting the folder

3. Command Issues
   - Don't forget 'sudo'
   - Check interface names
   - Verify VM names

Remember: A good detective never gives up! 💪