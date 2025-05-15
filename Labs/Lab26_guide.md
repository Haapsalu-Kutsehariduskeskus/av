# Lab 26: student guide

## Quick Reference Information

### IP Addressing Scheme

| Network Purpose | Subnet | Gateway | VLAN | DHCP Range |
|----------------|--------|---------|------|------------|
| Student Network | 10.10.10.0/24 | 10.10.10.1 | 10 | 10.10.10.11 - 10.10.10.254 |
| Teacher Network | 10.20.20.0/24 | 10.20.20.1 | 20 | 10.20.20.11 - 10.20.20.254 |
| Future External | 192.168.100.0/24 | 192.168.100.1 | N/A | N/A |

### Port Assignments

| Device | Port | Connected To | Purpose |
|--------|------|-------------|---------|
| Router | Console | Terminal PC (via patch panel) | Configuration |
| Router | GE0/0 | Switch GE0/1 | Trunk Connection |
| Router | GE0/1 | Unconnected (future) | Future External |
| Switch | Console | Terminal PC (via patch panel) | Configuration |
| Switch | GE0/1 | Router GE0/0 | Trunk Connection |
| Switch | Ports 1-12 | Client PC (for testing) | VLAN 10 Access |
| Switch | Ports 13-24 | Client PC (for testing) | VLAN 20 Access |

### Required Screenshots (18 total)

1. Router: `show version`
2. Router: `show ip interface brief`
3. Router: `ip nat ?`
4. Router: `show running-config interface GigabitEthernet0/0.10` and `show running-config interface GigabitEthernet0/0.20`
5. Router: `show ip dhcp pool`
6. Router: `show ip nat statistics`
7. Router: `copy running-config startup-config` confirmation
8. Switch: `show version`
9. Switch: `show vlan brief`
10. Switch: `show interfaces gigabitethernet0/1 trunk`
11. Switch: `show interfaces status`
12. Switch: `copy running-config startup-config` confirmation
13. Client PC: `ipconfig /all` on VLAN 10
14. Client PC: `ping 10.10.10.1` from VLAN 10
15. Client PC: `ipconfig /all` on VLAN 20
16. Client PC: `ping 10.20.20.1` from VLAN 20
17. Router: `show ip dhcp binding`
18. Router: `show ip nat statistics` (after client testing)

## Step-by-Step Procedure

### Part 1: Physical Setup

1. **Router Setup:**
   - Connect power to router
   - Connect Terminal PC to router console via patch panel
   - Connect router GE0/0 to switch GE0/1 via patch panel

2. **Switch Setup:**
   - Connect power to switch
   - Connect Client PC to a port in VLAN 10 (ports 1-12) via patch panel

### Part 2: Router Configuration

1. **Access Router Console:**
   - Open terminal emulation software
   - Configure: 9600 baud, 8-N-1, no flow control
   - Connect to router console port via patch panel

2. **Basic Configuration:**
   ```
   enable
   configure terminal
   hostname [YourName]-Router
   enable secret cisco
   line console 0
   password cisco
   login
   exit
   ```

3. **Interface Configuration:**
   ```
   interface GigabitEthernet0/0
   description Connection to Switch
   no shutdown
   exit

   interface GigabitEthernet0/0.10
   encapsulation dot1Q 10
   ip address 10.10.10.1 255.255.255.0
   ip nat inside
   exit

   interface GigabitEthernet0/0.20
   encapsulation dot1Q 20
   ip address 10.20.20.1 255.255.255.0
   ip nat inside
   exit

   interface GigabitEthernet0/1
   description Future External
   ip address 192.168.100.1 255.255.255.0
   ip nat outside
   no shutdown
   exit
   ```

4. **NAT Configuration:**
   ```
   access-list 1 permit 10.10.10.0 0.0.0.255
   access-list 1 permit 10.20.20.0 0.0.0.255
   ip nat inside source list 1 interface GigabitEthernet0/1 overload
   ```

5. **DHCP Configuration:**
   ```
   ip dhcp excluded-address 10.10.10.1 10.10.10.10
   ip dhcp excluded-address 10.20.20.1 10.20.20.10

   ip dhcp pool StudentNet
   network 10.10.10.0 255.255.255.0
   default-router 10.10.10.1
   dns-server 8.8.8.8 8.8.4.4
   domain-name lab.local
   exit

   ip dhcp pool TeacherNet
   network 10.20.20.0 255.255.255.0
   default-router 10.20.20.1
   dns-server 8.8.8.8 8.8.4.4
   domain-name lab.local
   exit
   ```

6. **Router Verification (Take Screenshots):**
   ```
   show version                                 # Screenshot #1
   show ip interface brief                      # Screenshot #2
   ip nat ?                                     # Screenshot #3
   show running-config interface GigabitEthernet0/0.10  # Screenshot #4 (part 1)
   show running-config interface GigabitEthernet0/0.20  # Screenshot #4 (part 2)
   show ip dhcp pool                            # Screenshot #5
   show ip nat statistics                       # Screenshot #6
   copy running-config startup-config           # Screenshot #7
   ```

### Part 3: Switch Configuration

1. **Access Switch Console:**
   - Disconnect console from router
   - Connect to switch console via patch panel
   - Open terminal software and connect

2. **Basic Configuration:**
   ```
   enable
   configure terminal
   hostname [YourName]-Switch
   enable secret cisco
   line console 0
   password cisco
   login
   exit
   ```

3. **VLAN Configuration:**
   ```
   vlan 10
   name Students
   exit

   vlan 20
   name Teachers
   exit

   interface range fastethernet0/1-12
   switchport mode access
   switchport access vlan 10
   exit

   interface range fastethernet0/13-24
   switchport mode access
   switchport access vlan 20
   exit

   interface gigabitethernet0/1
   switchport mode trunk
   switchport trunk allowed vlan 10,20
   exit
   ```

4. **Switch Verification (Take Screenshots):**
   ```
   show version                        # Screenshot #8
   show vlan brief                     # Screenshot #9
   show interfaces gigabitethernet0/1 trunk  # Screenshot #10
   show interfaces status              # Screenshot #11
   copy running-config startup-config  # Screenshot #12
   ```

### Part 4: Client Testing

1. **VLAN 10 Testing:**
   - Ensure Client PC is connected to a port in VLAN 10 (1-12)
   - Open Command Prompt and run:
     ```
     ipconfig /release
     ipconfig /renew
     ipconfig /all                    # Screenshot #13
     ping 10.10.10.1                  # Screenshot #14
     ```

2. **VLAN 20 Testing:**
   - Move Client PC to a port in VLAN 20 (13-24) via patch panel
   - Open Command Prompt and run:
     ```
     ipconfig /release
     ipconfig /renew
     ipconfig /all                    # Screenshot #15
     ping 10.20.20.1                  # Screenshot #16
     ```

3. **Final Router Verification:**
   - Reconnect console to router via patch panel
   - Run:
     ```
     show ip dhcp binding            # Screenshot #17
     show ip nat statistics          # Screenshot #18
     ```

## Troubleshooting Tips

### No Console Access
- Check console cable connection at patch panel
- Verify terminal settings (9600 baud, 8-N-1)
- Try restarting terminal program

### Interface Not Coming Up
- Check cable connections at patch panel
- Verify interface isn't shut down (`no shutdown`)
- Check interface status with `show interfaces`

### DHCP Not Working
- Verify interface is up (`show ip interface brief`)
- Check DHCP pool configuration (`show ip dhcp pool`)
- Verify client is set to obtain IP automatically

### VLAN Issues
- Verify VLANs exist (`show vlan brief`)
- Check port assignments (`show interfaces status`)
- Verify trunk configuration (`show interfaces trunk`)

### Can't Ping Between VLANs
- Verify subinterfaces are up (`show ip interface brief`)
- Check routing table (`show ip route`)
- Verify client has correct IP and gateway