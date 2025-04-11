# Part 3: VLAN Configuration and Trunking

## Objective
To configure and implement Virtual LANs (VLANs) across multiple switches, establishing inter-switch trunking, and verifying segmented network connectivity.

## Topology Diagram

```mermaid
graph TD

%% === PCs ===
    MNG["🖥 PC-MNG<br/>10.20.0.10/24<br/>VLAN 209<br/>Fa0/1 (Access)"]:::pc
    PC0["🖥 PC-0<br/>10.20.1.10/24<br/>VLAN 201<br/>Fa0/1 (Access)"]:::pc
    PC2["🖥 PC-2<br/>10.20.2.10/24<br/>VLAN 202<br/>Fa0/2 (Access)"]:::pc
    PC1["🖥 PC-1<br/>10.20.1.11/24<br/>VLAN 201<br/>Fa0/1 (Access)"]:::pc
    PC3["🖥 PC-3<br/>10.20.2.11/24<br/>VLAN 202<br/>Fa0/2 (Access)"]:::pc

%% === Server ===
    SERVER["🖥 Server-0<br/>10.20.3.100/24<br/>VLAN 203<br/>Fa0/1 (Access)"]:::server

%% === Access Switches ===
    SW3["🔀 SW3<br/>2960-24TT<br/>Access Switch<br/>Fa0/1: PC0<br/>Fa0/2: PC2<br/>Fa0/24: Trunk"]:::accessSwitch
    SW4["🔀 SW4<br/>2960-24TT<br/>Access Switch<br/>Fa0/1: PC1<br/>Fa0/2: PC3<br/>Fa0/24: Trunk"]:::accessSwitch

%% === Distribution Switches ===
    SW1["🟩 SW1<br/>3560-24PS<br/>Core / Dist<br/>Root Bridge (STP)<br/>Fa0/1: PC-MNG<br/>Fa0/23: Trunk<br/>Fa0/24: Trunk"]:::coreSwitch
    SW2["🟩 SW2<br/>3560-24PS<br/>Core / Dist<br/>Fa0/1: Server-0<br/>Fa0/23: Trunk<br/>Fa0/24: Trunk"]:::coreSwitch

%% === Connections ===

    %% Access layer
    PC0 -->|Fa0/1<br/>VLAN 201| SW3
    PC2 -->|Fa0/2<br/>VLAN 202| SW3

    PC1 -->|Fa0/1<br/>VLAN 201| SW4
    PC3 -->|Fa0/2<br/>VLAN 202| SW4

    %% Trunks from access to core
    SW3 -->|Fa0/24<br/>Trunk| SW1
    SW4 -->|Fa0/24<br/>Trunk| SW2

    %% Trunk between distribution switches (redundant link)
    SW1 -->|Fa0/23<br/>Trunk| SW2

    %% Management + Server
    MNG -->|Fa0/1<br/>VLAN 209| SW1
    SERVER -->|Fa0/1<br/>VLAN 203| SW2

%% === Styling ===
    classDef pc fill:#d4f1f9,stroke:#333,stroke-width:1px;
    classDef server fill:#ffe599,stroke:#333,stroke-width:1px;
    classDef accessSwitch fill:#b6d7a8,stroke:#333,stroke-width:1px;
    classDef coreSwitch fill:#a2c4c9,stroke:#333,stroke-width:2px;

    class MNG,PC0,PC1,PC2,PC3 pc;
    class SERVER server;
    class SW3,SW4 accessSwitch;
    class SW1,SW2 coreSwitch;

```

## VLAN Configuration Table

### VLAN Definition

| VLAN ID | VLAN Name      | Network        | Purpose        |
|---------|----------------|----------------|----------------|
| X0      | Management     | 10.X.0.0/24    | Network Management |
| X1      | USERS1         | 10.X.1.0/24    | User Group 1   |
| X2      | USERS2         | 10.X.2.0/24    | User Group 2   |
| X3      | Servers        | 10.X.3.0/24    | Server Access  |

Note: X represents your variant number as specified in the assignment.

### Device IP Configuration

| Device        | Interface | IP Address   | Subnet Mask   | Default Gateway | VLAN   |
|---------------|-----------|--------------|---------------|-----------------|--------|
| Management PC | NIC       | 10.X.0.10/24 | 255.255.255.0 | N/A             | X0 (20)|
| PC-0          | NIC       | 10.X.1.10/24 | 255.255.255.0 | N/A             | X1     |
| PC-1          | NIC       | 10.X.1.11/24 | 255.255.255.0 | N/A             | X1     |
| PC-2          | NIC       | 10.X.2.10/24 | 255.255.255.0 | N/A             | X2     |
| PC-3          | NIC       | 10.X.2.11/24 | 255.255.255.0 | N/A             | X2     |
| Server-0      | NIC       | 10.X.3.10/24 | 255.255.255.0 | N/A             | X3 (30)|

### Switch Port Configuration

| Switch   | Port   | Connection Type | VLAN Mode | VLAN Assignment |
|----------|--------|----------------|-----------|-----------------|
| Switch-1 | Fa0/1  | Management PC  | Access    | X0 (20)         |
| Switch-1 | Fa0/23 | Switch-2       | Trunk     | All VLANs       |
| Switch-1 | Fa0/24 | Switch-3       | Trunk     | All VLANs       |
| Switch-2 | Fa0/1  | Server-0       | Access    | X3 (30)         |
| Switch-2 | Fa0/23 | Switch-1       | Trunk     | All VLANs       |
| Switch-2 | Fa0/24 | Switch-4       | Trunk     | All VLANs       |
| Switch-3 | Fa0/1  | PC-0           | Access    | X1              |
| Switch-3 | Fa0/2  | PC-2           | Access    | X2              |
| Switch-3 | Fa0/24 | Switch-1       | Trunk     | All VLANs       |
| Switch-4 | Fa0/1  | PC-1           | Access    | X1              |
| Switch-4 | Fa0/2  | PC-3           | Access    | X2              |
| Switch-4 | Fa0/24 | Switch-2       | Trunk     | All VLANs       |

## Instructions

1. **Creating the Network Topology**:
   - Open Cisco Packet Tracer
   - Add network devices according to the topology diagram:
     - 4 PCs (PC-0, PC-1, PC-2, PC-3)
     - 1 Management PC
     - 1 Server
     - 4 Switches (Cisco 3560-24PS/3560-24TT)
   - Connect the devices using straight-through cables as shown in the diagram

2. **Configuring VLANs on Switches**:
   - For each switch, access the CLI through console connection
   - Enter privileged EXEC mode: `enable`
   - Enter global configuration mode: `configure terminal`
   - Create VLANs:
     ```
     vlan X0
     name Management
     exit
     vlan X1
     name USERS1
     exit
     vlan X2
     name USERS2
     exit
     vlan X3
     name Servers
     exit
     ```

3. **Configuring Access Ports**:
   - Configure access ports on Switch-1:
     ```
     interface fa0/1
     switchport mode access
     switchport access vlan X0
     no shutdown
     exit
     ```
   
   - Configure access ports on Switch-2:
     ```
     interface fa0/1
     switchport mode access
     switchport access vlan X3
     no shutdown
     exit
     ```
   
   - Configure access ports on Switch-3:
     ```
     interface fa0/1
     switchport mode access
     switchport access vlan X1
     no shutdown
     exit
     interface fa0/2
     switchport mode access
     switchport access vlan X2
     no shutdown
     exit
     ```
   
   - Configure access ports on Switch-4:
     ```
     interface fa0/1
     switchport mode access
     switchport access vlan X1
     no shutdown
     exit
     interface fa0/2
     switchport mode access
     switchport access vlan X2
     no shutdown
     exit
     ```

4. **Configuring Trunk Ports**:
   - Configure trunk ports on Switch-1:
     ```
     interface fa0/23
     switchport mode trunk
     no shutdown
     exit
     interface fa0/24
     switchport mode trunk
     no shutdown
     exit
     ```
   
   - Configure trunk ports on Switch-2:
     ```
     interface fa0/23
     switchport mode trunk
     no shutdown
     exit
     interface fa0/24
     switchport mode trunk
     no shutdown
     exit
     ```
   
   - Configure trunk ports on Switch-3:
     ```
     interface fa0/24
     switchport mode trunk
     no shutdown
     exit
     ```
   
   - Configure trunk ports on Switch-4:
     ```
     interface fa0/24
     switchport mode trunk
     no shutdown
     exit
     ```

5. **Configuring IP Addresses on End Devices**:
   - Configure each PC and server with the appropriate IP address, subnet mask, and default gateway (if needed) according to the Device IP Configuration table

6. **Verifying VLAN Configuration**:
   - On each switch, verify the VLAN configuration:
     ```
     show vlan brief
     show interfaces trunk
     ```
   - Verify connectivity between devices in the same VLAN:
     - PC-0 should be able to ping PC-1 (same VLAN X1)
     - PC-2 should be able to ping PC-3 (same VLAN X2)
     - Management PC should not be able to ping any PC except the Server
     - PCs in VLAN X1 should not be able to ping PCs in VLAN X2

## Expected Results
- Devices within the same VLAN can communicate with each other
- Devices in different VLANs cannot communicate with each other without routing
- Trunk links successfully carry traffic for all configured VLANs

## Explanation

This VLAN configuration setup demonstrates several important networking concepts:

1. **VLAN Segmentation**:
   - VLANs allow logical separation of networks on the same physical infrastructure
   - Network security is enhanced by isolating traffic between different user groups
   - Broadcast domains are reduced to only the devices within each VLAN

2. **Trunk Links**:
   - Trunk ports carry traffic for multiple VLANs
   - IEEE 802.1Q tagging is used to identify which VLAN each frame belongs to
   - Trunk links are essential for extending VLANs across multiple switches

3. **Access Ports**:
   - Access ports belong to a single VLAN
   - Frames entering an access port are assigned to the configured VLAN
   - Frames leaving an access port have any VLAN tags removed

4. **Network Management**:
   - A dedicated Management VLAN (X0) provides separation of management traffic
   - Server VLAN (X3) isolates server traffic from regular user traffic

5. **Network Topology Design**:
   - The hierarchical design with core switches (Switch-1, Switch-2) and access switches (Switch-3, Switch-4) demonstrates best practices
   - Redundant connections between core switches provide failover capability

This VLAN setup provides the foundation for more advanced network configurations including inter-VLAN routing, which would be necessary for communication between different VLANs in a production environment.