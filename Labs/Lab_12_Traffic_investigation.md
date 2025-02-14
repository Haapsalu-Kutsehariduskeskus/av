# 🔍 Lab12: Network Traffic Investigation

## 📋 Overview
This lab provides hands-on experience in analyzing network traffic using virtualized environments. You will set up a simulated network using Multipass, capture traffic using **tcpdump**, and analyze key networking behaviors.

## ⚡ Requirements
- 💻 Windows computer
- 🚀 **Multipass** installed
- 🔑 Administrator privileges
- 💾 At least **8GB RAM** available
- ⏰ Approximately **1 hour** to complete the lab
- 🌐 **Wireshark installed on Windows** (for analyzing captured network traffic)

## 🏗️ Part 1: Setting Up the Lab Environment

### 📁 Step 1: Prepare the Evidence Folder
Before proceeding, create a folder in Windows to store captured network traffic:

1. Open **File Explorer**.
2. Create a new folder at **C:\network-evidence**.

This folder will be used to store captured traffic logs.

### 🔌 Step 2: Verify Network Adapters
Before launching the virtual machines, students must check their available network adapters:

#### **Checking Network Adapters in Windows**
Run the following command in **PowerShell**:
```powershell
Get-NetAdapter
```
This will list all available network interfaces. Ensure that an active interface is available before proceeding.

### 🖥️ Step 3: Create Virtual Machines
Each student should personalize their VM names using their own name to ensure authenticity.

OR YOU CAN DO THIS PART MANUALLY IN GUI

Open **PowerShell as Administrator** and execute the following commands to set up the virtual environment (replace `<student_name>` with your own name):

```powershell
# Mount the evidence folder into the virtual environment
multipass mount C:\network-evidence detective-hq-<student_name>

# Create a monitoring station (Detective HQ)
multipass launch -n detective-hq-<student_name> -m 2G -c 2 20.04 --network name=mystery-net,mode=manual

# Create two test stations
multipass launch -n station1-<student_name> -m 1G -c 1 20.04 --network name=mystery-net,mode=manual
multipass launch -n station2-<student_name> -m 1G -c 1 20.04 --network name=mystery-net,mode=manual
```

### 🌐 Step 4: Configure Networking
Once the VMs are running, configure the network settings for each machine.

#### **Checking Network Interfaces in Linux**
Inside each VM, verify the network interface name using:
```bash
ip link show
```
Some systems may not use `enp0s3`, so adjust commands accordingly.
If you are doing manually, it will get already IP, you have to give it again!!!!

#### **Detective HQ (Monitoring Station)** 🕵️
```bash
multipass shell detective-hq-<student_name>
sudo ip addr add 192.168.100.10/24 dev enp0s3
sudo apt update
sudo apt install -y tcpdump iperf3 net-tools
exit
```

#### **Station 1** 📱
```bash
multipass shell station1-<student_name>
sudo ip addr add 192.168.100.11/24 dev enp0s3
sudo apt update
sudo apt install -y iperf3 net-tools
exit
```

#### **Station 2** 💻
```bash
multipass shell station2-<student_name>
sudo ip addr add 192.168.100.12/24 dev enp0s3
sudo apt update
sudo apt install -y iperf3 net-tools
exit
```

### 🔄 Step 5: Verify VM Connectivity
To ensure the VMs are properly networked, students should test connectivity between them:
```bash
ping 192.168.100.11   # From Detective HQ to Station 1
ping 192.168.100.12   # From Detective HQ to Station 2
```

If the pings fail, recheck the network interfaces using `ip addr show`.

## 📊 Part 2: Capturing and Analyzing Traffic

### 🎣 Using tcpdump
**tcpdump** is a powerful tool for monitoring network traffic.

#### Capture all traffic for 30 seconds:
```bash
sudo timeout 30 tcpdump -i enp0s3 -w /mnt/network-evidence/capture.pcap
```
This will capture traffic for **30 seconds** before stopping automatically.

#### Capture specific traffic types:
```bash
sudo tcpdump -i enp0s3 icmp          # Capture ping requests
sudo tcpdump -i enp0s3 broadcast     # Capture broadcast messages
```

## 🔍 Part 3: Analyzing Captured Traffic with Wireshark

1. **Ensure Wireshark is installed on Windows** from [https://www.wireshark.org/](https://www.wireshark.org/).
2. Open **Wireshark** on Windows.
3. Navigate to **File > Open** and select the `.pcap` file from `C:\network-evidence`.

### **Using Filters in Wireshark** 🔎
Wireshark allows you to filter traffic to focus on specific packets. Below are useful filters for this lab:

- **To filter ICMP traffic (ping requests)**:
  ```
  icmp
  ```
- **To filter ARP traffic**:
  ```
  arp
  ```
- **To filter traffic between two specific IPs (e.g., Station 1 and Station 2)**:
  ```
  ip.addr == 192.168.100.11 && ip.addr == 192.168.100.12
  ```

### **Key Questions to Answer** 🤔
- What traffic types did you observe in Wireshark?
- How do ARP requests function in network communication?
- What is the role of ICMP, and how does it work in network troubleshooting?

### **Hints for Answering Questions** 💡
- **What traffic types did you observe?**
  - Use different Wireshark filters (`icmp`, `arp`, `tcp`, etc.) to see which protocols are captured.
  - Look at the **protocol column** in Wireshark.

- **How do ARP requests function?**
  - Use the `arp` filter to see how ARP requests are sent before a ping.
  - Check which machine is **asking for a MAC address** and which one is replying.

- **What is the role of ICMP?**
  - Use the `icmp` filter to view **ping requests and replies**.
  - Observe the time between an **Echo Request** and an **Echo Reply**.

## 📝 Part 4: Submitting Your Work

### **Screenshots and Documentation** 📸
Students are required to take **screenshots** at key steps of the lab, including:
- **Wireshark open with captured `.pcap` files**
- **Filters applied in Wireshark**
- **Key packets observed and analyzed**

### **Google Classroom Submission** 📤
1. Compile your screenshots and a short **summary document** with answers to key questions.
2. Upload the document and images to **Google Classroom** under the assigned task.

## ⚠️ Part 5: Troubleshooting and Common Issues

### **1. Wireshark Not Showing Any Packets** 🚫
- Ensure the `.pcap` file was captured correctly using `tcpdump`.
- Confirm the `.pcap` file is located in `C:\network-evidence` and is not empty.
- Restart Wireshark and try reopening the file.

### **2. Filters Not Working** ❌
- Ensure there are relevant packets in the capture.
- Double-check IP addresses or protocol names in the filter syntax.
- Try broadening the filter criteria and then narrowing it down.

### **3. `.pcap` File Not Found in Windows** 🔍
- Verify that the Multipass mount is working correctly.
- Try manually copying the file from the VM using:
  ```bash
  multipass transfer detective-hq-<student_name>:/mnt/network-evidence/capture.pcap C:\network-evidence\capture.pcap
  ```
