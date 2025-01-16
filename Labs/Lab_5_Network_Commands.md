# Lab5: Network Commands and Router Configuration

## Aim:
To configure a network with multiple devices connected to a single router, test connectivity using basic commands, and validate proper network operation.

---

## Materials Required:
1. **Devices:**
   - **1 Router** (e.g., Cisco 1941 or equivalent).
   - **4 PCs** (name them uniquely, e.g., `PC_Alice1`, `PC_Bob2`, `PC_Charlie3`, `PC_David4`).
   - **1 Switch**.
2. **Packet Tracer Software.**

---

## Objectives:
1. Practice configuring a router for multiple connected devices.
2. Test connectivity between devices using commands like `ping`, `tracert`, and `nslookup`.
3. Analyze packet paths and validate connectivity.
4. Use a single subnet to avoid complexity while creating a multi-device environment.

---

## Network Design:

### **Topology Table**
| **Device**       | **Interface**           | **IP Address**    | **Connection**            |
|-------------------|-------------------------|-------------------|---------------------------|
| **Router**       | GigabitEthernet0/0      | 192.168.1.1       | Connected to Switch Port 1|
| **PC_Alice1**    | Ethernet0               | 192.168.1.2       | Connected to Switch Port 2|
| **PC_Bob2**      | Ethernet0               | 192.168.1.3       | Connected to Switch Port 3|
| **PC_Charlie3**  | Ethernet0               | 192.168.1.4       | Connected to Switch Port 4|
| **PC_David4**    | Ethernet0               | 192.168.1.5       | Connected to Switch Port 5|

### **IP Addressing Plan**
- **Network:** `192.168.1.0/24`
- **Subnet Mask:** `255.255.255.0`
- **Default Gateway:** `192.168.1.1`

---

## Steps for Configuration

### **Step 1: Setting Up Topology in Packet Tracer**
1. **Add Devices:**
   - Drag and drop the following:
     - **1 Router.**
     - **1 Switch.**
     - **4 PCs (rename each uniquely, e.g., PC_Alice1, PC_Bob2).**

2. **Connect Devices:**
   - Determine the appropriate cables to use for each connection:
     - **Router to Switch:**
       - Use a **Straight-Through Ethernet Cable** in most cases.
       - Consider testing and analyzing the connection type.
     - **PCs to Switch:**
       - Use a **Straight-Through Ethernet Cable** to connect each PC to the switch.
       - Confirm if the device interfaces require a specific cable based on simulation results.

---

### **Step 2: Configure the Router**
1. **Access the Router CLI:**
   - Click the router and select **CLI**.

2. **Enter Configuration Mode:**
   ```
   enable
   configure terminal
   ```

3. **Configure the Router Interface:**
   ```
   interface GigabitEthernet0/0
   ip address 192.168.1.1 255.255.255.0
   no shutdown
   ```

4. **Exit Configuration Mode:**
   ```
   end
   ```

5. **Save the Configuration:**
   ```
   write memory
   ```

---

### **Step 3: Configure the PCs**
1. **Open IP Configuration on Each PC:**
   - Go to the **Desktop Tab** → **IP Configuration.**
   - Assign the following:
     - **PC_Alice1:** IP Address: `192.168.1.2`, Subnet Mask: `255.255.255.0`, Gateway: `192.168.1.1`
     - **PC_Bob2:** IP Address: `192.168.1.3`, Subnet Mask: `255.255.255.0`, Gateway: `192.168.1.1`
     - **PC_Charlie3:** IP Address: `192.168.1.4`, Subnet Mask: `255.255.255.0`, Gateway: `192.168.1.1`
     - **PC_David4:** IP Address: `192.168.1.5`, Subnet Mask: `255.255.255.0`, Gateway: `192.168.1.1`

---

### **Step 4: Testing Connectivity**

#### **1. Ping Test:**
- From each PC:
  - Open the **Command Prompt**.
  - Ping the router:
    ```
    ping 192.168.1.1
    ```
  - Ping other PCs:
    ```
    ping 192.168.1.2
    ping 192.168.1.3
    ping 192.168.1.4
    ```

#### **2. Traceroute Test:**
- From each PC:
  - Use `tracert` to trace the path to another PC or the router:
    ```
    tracert 192.168.1.1
    tracert 192.168.1.3
    ```

#### **3. nslookup Test:**
- Simulate DNS by resolving domain names:
  - Open the Command Prompt.
  - Type:
    ```
    nslookup google.com
    ```
  - Note the output and IP resolution.

---

## Deliverables

1. **Packet Tracer File:**
   - Save and submit the `.pkt` file with all configurations.

2. **Lab Report:**
   - Include:
     - **Test Results:** Provide **a single screenshot showing multiple terminal windows** for all PCs with the following visible:
       - **PC names** (e.g., PC_Alice1, PC_Bob2).
       - Commands executed (e.g., `ping`, `tracert`, `nslookup`).
       - Output of the commands.

3. **Optional Live Demo:**
   - Students may be asked to demonstrate their configurations and run a few commands live.

---

## Troubleshooting Tips
1. **Ping Fails:**
   - Check cable connections and ensure interfaces are active.
   - Verify IP configurations on all devices.

2. **No Response from Router:**
   - Ensure the router interface is configured and not in shutdown mode.

3. **Traceroute Stops:**
   - Ensure all devices in the path have proper IP addresses and connectivity.
