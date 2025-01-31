# Lab8: Configuring and Troubleshooting a Switched Network


![Topology_Lab7](/Lecture/media/switched-network-topology.svg)

### Topology Diagram

### Objectives
* Establish console connection to the switch
* Configure the host name and VLAN1
* Use the help feature to configure the clock
* Configure passwords and console/Telnet access
* Configure login banners
* Configure the router
* Solve duplex and speed mismatch problems
* Configure port security
* Secure unused ports
* Manage the switch configuration file

### Background/Preparation
In this Packet Tracer Skills Integration Challenge activity, you will configure basic switch management, including general maintenance commands, passwords, and port security. This activity provides you an opportunity to review previously acquired skills.

### Addressing Table
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| R1 | Fa0/0 | 172.17.99.1 | 255.255.255.0 |
| S1 | Fa0/1 | 172.17.99.11 | 255.255.255.0 |
| PC1 | NIC | 172.17.99.21 | 255.255.255.0 |
| PC2 | NIC | 172.17.99.22 | 255.255.255.0 |
| Server | NIC | 172.17.99.31 | 255.255.255.0 |

### Step 1: Establish a console connection to a switch
For this activity, direct access to the S1 Config and CLI tabs is disabled. You must establish a console session through PC1.

a. Connect a console cable from PC1 to S1.

b. From PC1, open a terminal window and use the default terminal configuration. You should now have access to the CLI for S1.

c. Check results.

### Step 2: Configure the host name and VLAN 1
a. Configure the switch host name as S1.

b. Configure port Fa0/1. Set the mode on Fast Ethernet 0/1 to access mode.
```
i. S1(config)#interface fastethernet 0/1
ii. S1(config-if)#switchport mode access
```

c. Configure IP connectivity on S1 using VLAN 1.
```
i. S1(config)#interface vlan 1
ii. S1(config-if)#ip address 172.17.99.11 255.255.255.0
iii. S1(config-if)#no shutdown
```

d. Configure the default gateway for S1 and then test connectivity. S1 should be able to ping R1.

e. Check results.

### Step 3: Configure the current time using Help
a. Configure the clock to the current time. At the privileged EXEC prompt, enter clock ?.

b. Use Help to discover the steps required to set the current time.

c. Use the show clock command to verify that the clock is now set to the current time. Packet Tracer may not correctly simulate the time you entered.

### Step 4: Configure passwords
a. Use the encrypted form of the privileged EXEC mode password and set the password to class.

b. Configure the passwords for console and Telnet. Set both the console and vty password to cisco and require users to log in.

c. View the current configuration on S1. Notice that the line passwords are shown in clear text. Enter the command to encrypt these passwords.

d. Check results.

### Step 5: Configure the login banner
If you do not enter the banner text exactly as specified, Packet Tracer does not grade your command correctly. These commands are case-sensitive. Also make sure that you do not include any spaces before or after the text.

a. Configure the message-of-the-day banner on S1 to display as Authorized Access Only. (Do not include the period.)

b. Check results.

### Step 6: Configure the router
Routers and switches share many of the same commands. Configure the router with the same basic commands you used on S1.

a. Access the CLI for R1 by clicking the device.

b. Do the following on R1:
* Configure the hostname of the router as R1
* Configure the encrypted form of the privileged EXEC mode password and set the password to class
* Set the console and vty password to cisco and require users to log in
* Encrypt the console and vty passwords
* Configure the message-of-the-day as Authorized Access Only. (Do not include the period.)

c. Check results.

### Step 7: Solve a mismatch between duplex and speed
a. PC1 and Server currently do not have access through S1 because the duplex and speed are mismatched. Enter commands on S1 to solve this problem.

b. Verify connectivity.

c. Both PC1 and Server should now be able to ping S1, R1, and each other.

d. Check results.

### Step 8: Configure port security
a. Use the following policy to establish port security on the port used by PC1:
* Enable port security
* Allow only one MAC address
* Configure the first learned MAC address to "stick" to the configuration

Note: Only enabling port security is graded by Packet Tracer and counted toward the completion percentage. However, all the port security tasks listed above are required to complete this activity successfully.

b. Verify that port security is enabled for Fa0/18. Your output should look like the following output. Notice that S1 has not yet learned a MAC address for this interface. What command generated this output?

```
S1#_________________________________

Port Security              : Enabled
Port Status               : Secure-up
Violation Mode            : Shutdown
Aging Time                : 0 mins
Aging Type               : Absolute
SecureStatic Address Aging: Disabled
Maximum MAC Addresses     : 1
Total MAC Addresses       : 0
Configured MAC Addresses  : 0
Sticky MAC Addresses      : 0
Last Source Address:Vlan  : 0000.0000.0000:0
Security Violation Count  : 0
```

c. Force S1 to learn the MAC address for PC1. Send a ping from PC1 to S1. Then verify that S1 added the MAC address for PC1 to the running configuration.

```
!
interface FastEthernet0/18
<output omitted>
switchport port-security mac-address sticky 0060.3EE6.1659
<output omitted>
!
```

d. Test port security. Remove the FastEthernet connection between S1 and PC1. Connect PC2 to Fa0/18. Wait for the link lights to turn green. If necessary, send a ping from PC2 to S1 to cause the port to shut down. Port security should show the following results: (the Last Source Address may be different)

```
Port Security              : Enabled
Port Status               : Secure-shutdown
Violation Mode            : Shutdown
Aging Time                : 0 mins
Aging Type               : Absolute
SecureStatic Address Aging: Disabled
Maximum MAC Addresses     : 1
Total MAC Addresses       : 1
Configured MAC Addresses  : 1
Sticky MAC Addresses      : 0
Last Source Address:Vlan  : 00D0.BAD6.5193:99
Security Violation Count  : 1
```

e. Viewing the Fa0/18 interface shows that line protocol is down (err-disabled), which also indicates a security violation.
```
S1#show interface fa0/18
FastEthernet0/18 is down, line protocol is down (err-disabled)
<output omitted>
```

f. Reconnect PC1 and re-enable the port. To re-enable the port, disconnect PC2 from Fa0/18 and reconnect PC1. Interface Fa0/18 must be manually reenabled with the no shutdown command before returning to the active state.

g. Check results.

### Step 9: Secure unused ports
a. Disable all ports that are currently not used on S1. Packet Tracer grades the status of the following ports: Fa0/2, Fa0/3, Fa0/4, Gig 1/1, and Gig 1/2.

b. Check results.

### Step 10: Manage the switch configuration file
a. Save the current configuration for S1 and R1 to NVRAM.

b. Back up the startup configuration file on S1 and R1 by uploading them to Server. Verify that Server has the R1-config and S1-config files.

c. Check results.

# Lab Report Requirements

### Required Format
1. Cover Page:
   - Your Name
   - Lab8: Configuring and Troubleshooting a Switched Network

2. Topology Documentation:
   - Screenshot of your completed network topology in Packet Tracer
   - All device names must show your name prefix (e.g., JohnSmith_R1)

3. Implementation Process:
   For each configuration step, provide:
   - Screenshots of command outputs

4. Verification Screenshot:
   - Final configuration backups

5. Port Security Testing Evidence:
   - Final working state

6. Configuration Backup:
   - Screenshot showing successful backup to server

7. Files to Submit:
   - Lab report
   - Packet Tracer file named [YourName]_Lab8.pkt

Note: Ensure all screenshots are clear and readable. Include command inputs and outputs to demonstrate your understanding of the configuration process.
