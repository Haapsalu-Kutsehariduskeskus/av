# Lab7: Initial Router Configuration

**Performing an Initial Router Configuration**

![Topology_Lab7](/Lecture/media/router-topology.svg)

## Objectives
* Configure the router host name
* Configure passwords
* Configure banner messages
* Verify the router configuration

## Background/Preparation
In this activity, you will use the Cisco IOS CLI to apply an initial configuration to a router, including host name, passwords, a message-of-the-day (MOTD) banner, and other basic settings.

### Required Equipment
* 1 Cisco 1841 ISR Router (Customer Router)
* 1 PC with terminal emulation software (Customer PC)
* Console cable
* Network cables as needed
* Packet Tracer software

### Naming Convention
All device names should include your name in the following format:
* Router name: [YourName]Router (e.g., JohnSmithRouter)
* PC name: [YourName]PC
* Switch name: [YourName]Switch

### Required Submissions
1. Completed Packet Tracer file (.pkt) with your name in the filename (e.g., JohnSmith_Lab7.pkt)
2. Screenshots of configuration verification
3. Written answers to reflection questions

## Configuration Steps

### Step 1: Configure the router host name
a. On Customer PC, use the terminal emulation software to connect to the console of the customer Cisco 1841 ISR.

b. Set the host name on the router using these commands:
```
Router>enable
Router#configure terminal
Router(config)#hostname CustomerRouter
```

### Step 2: Configure the privileged mode and secret passwords
a. Set the password to cisco:
```
CustomerRouter(config)#enable password cisco
```

b. Set an encrypted privileged password to cisco123:
```
CustomerRouter(config)#enable secret cisco123
```

### Step 3: Configure the console password
a. Configure console line and set password:
```
CustomerRouter(config)#line console 0
CustomerRouter(config-line)#password cisco123
CustomerRouter(config-line)#login
CustomerRouter(config-line)#exit
```

### Step 4: Configure the VTY password for Telnet access
a. Configure VTY lines:
```
CustomerRouter(config)#line vty 0 4
CustomerRouter(config-line)#password cisco123
CustomerRouter(config-line)#login
CustomerRouter(config-line)#exit
```

### Step 5: Configure additional security settings
a. Enable password encryption:
```
CustomerRouter(config)#service password-encryption
```

b. Configure MOTD banner:
```
CustomerRouter(config)#banner motd $Authorized Access Only!$
```

c. Disable domain lookup:
```
CustomerRouter(config)#no ip domain-lookup
```

d. Save the configuration:
```
CustomerRouter(config)#end
CustomerRouter#copy run start
```

### Step 6: Verify the configuration
a. Log out of your terminal session
b. Log back in to verify password prompts
c. Navigate to privileged EXEC mode to verify privileged password
d. Check the running configuration to verify all settings

## Reflection Questions
1. Which Cisco IOS CLI commands did you use most frequently in this lab?

2. How can you make the customer router passwords more secure?

Note: Include your answers to these questions in your lab report submission.

## Expected Results
* All passwords should be properly configured and working
* MOTD banner should display when accessing the router
* Password encryption should be enabled
* Domain lookup should be disabled
* Configuration should be saved to startup-config