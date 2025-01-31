# # Lab6: Initial Switch Configuration

## Objectives
* Perform an initial configuration of a Cisco Catalyst 2960 switch
* Configure basic switch settings including hostname, passwords, and VLAN interfaces
* Assign an IP address to the VLAN1 interface for remote management
* Test connectivity using basic network commands

## Background/Preparation
In this activity, you will configure these settings on the customer Cisco Catalyst 2960 switch:
* Host name 
* Console password
* VTY password
* Privileged EXEC mode password
* Privileged EXEC mode secret
* IP address on VLAN1 interface
* Default gateway

### Required Equipment
* 1 Cisco Catalyst 2960-24TT Switch (Customer Switch)
* 1 PC with terminal emulation software (Customer PC)
* Console cable
* Network cables as needed
* Packet Tracer software

### Naming Convention
All device names should include your name in the following format:
* Switch name: [YourName]Switch (e.g., JohnSmithSwitch)
* PC name: [YourName]PC (e.g., JohnSmithPC)
* Router name: [YourName]Router (e.g., JohnSmithRouter)

### Required Submissions
1. Completed Packet Tracer file (.pkt) with your name in the filename (e.g., JohnSmith_Lab6.pkt)
2. Screenshots of successful ping tests
3. Written answers to reflection questions

### Topology Details
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| Customer Switch | VLAN1 | 192.168.1.5 | 255.255.255.0 |
| Customer Router | Ethernet | 192.168.1.1 | 255.255.255.0 |
| PC | Ethernet | 192.168.1.3 | 255.255.255.0 |

## Configuration Steps

### Step 1: Configure the switch hostname
a. From the Customer PC, use a console cable and terminal emulation software to connect to the console of the customer Cisco Catalyst 2960 switch.

b. Set the host name on the switch to CustomerSwitch using these commands:
```
Switch>enable
Switch#configure terminal
Switch(config)#hostname CustomerSwitch
```

### Step 2: Configure the privileged mode password and secret
a. From global configuration mode, configure the password as cisco:
```
CustomerSwitch(config)#enable password cisco
```

b. Configure the secret as cisco123:
```
CustomerSwitch(config)#enable secret cisco123
```

### Step 3: Configure the console password
a. Enter line configuration mode for the console:
```
CustomerSwitch(config)#line console 0
```

b. Set the password to cisco and enable login:
```
CustomerSwitch(config-line)#password cisco
CustomerSwitch(config-line)#login
CustomerSwitch(config-line)#exit
```

### Step 4: Configure the VTY password
a. Enter line configuration mode for VTY lines:
```
CustomerSwitch(config)#line vty 0 15
```

b. Set the password to cisco and enable login:
```
CustomerSwitch(config-line)#password cisco
CustomerSwitch(config-line)#login
CustomerSwitch(config-line)#exit
```

### Step 5: Configure an IP address on interface VLAN1
a. Enter interface configuration mode for VLAN1:
```
CustomerSwitch(config)#interface vlan 1
```

b. Assign the IP address and subnet mask:
```
CustomerSwitch(config-if)#ip address 192.168.1.5 255.255.255.0
CustomerSwitch(config-if)#no shutdown
CustomerSwitch(config-if)#exit
```

### Step 6: Configure the default gateway
```
CustomerSwitch(config)#ip default-gateway 192.168.1.1
```

### Step 7: Verify the configuration
a. Exit to privileged EXEC mode:
```
CustomerSwitch(config)#end
```

b. Test connectivity by pinging the PC:
```
CustomerSwitch#ping 192.168.1.3
```

Note: The first one or two pings may fail while ARP converges.

## Reflection Questions

1. What is the significance of assigning the IP address to the VLAN1 interface instead of any of the Fast Ethernet interfaces?

2. What command is necessary to enforce password authentication on the console and VTY lines?

3. How many gigabit ports are available on the Cisco Catalyst 2960 switch that you used in the activity?

Note: Include your answers to these questions in your lab report submission.

## Expected Results
* All configuration commands should be accepted without errors
* The switch should be accessible via console with the configured password
* Pings to the ISP Server should be successful
* Success rate should be approximately 60 percent (3/5 pings successful)

Note: Not all commands are graded by Packet Tracer.
