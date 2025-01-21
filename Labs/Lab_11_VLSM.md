# **Lab10: VLSM**

## **Part 1: Network Planning**

### **Instructions**
1. Use the provided main network: **192.168.45.0/24**.
2. Divide the network into subnets using **VLSM** to meet the user requirements for each location.
3. Start with the **largest group first** and work your way down.
4. For each subnet, calculate:
   - **Network Address**
   - **Usable IP Range** (First to Last)
   - **Broadcast Address**
   - **Subnet Mask (/x)**

### **Network Design**
| Location          | Number of People | Network Address | Available Addresses  (First - Last) | Broadcast Address | Subnet Mask (/x) |
|--------------------|------------------|-----------------|--------------------------------------|-------------------|------------------|
| Tallinn HQ         | 120              |                 |                                      |                   |                  |
| Tartu Office       | 62               |                 |                                      |                   |                  |
| Pärnu Branch       | 25               |                 |                                      |                   |                  |
| IT Room            | 10               |                 |                                      |                   |                  |
| Reception          | 5                |                 |                                      |                   |                  |

---

## **Part 2: Network Implementation**

### **Objective**
Build the planned network in **Cisco Packet Tracer**. Configure devices and connections to enable communication between all locations.

---
```mermaid
graph TD
    subgraph HQ[TALLINN HQ]
    R1[Router-HQ]
    SW1[Switch-HQ]
    SW4[Switch-IT]
    SW5[Switch-Reception]
    HQ1[Tallinn HQ 120 hosts]
    IT1[IT Room 10 hosts]
    RC1[Reception 5 hosts]
    
    R1---SW1
    R1---SW4
    R1---SW5
    SW1---HQ1
    SW4---IT1
    SW5---RC1
    end

    subgraph TARTU[TARTU OFFICE]
    R2[Router-Tartu]
    SW2[Switch-Tartu]
    TO1[Tartu Office 62 hosts]
    
    R2---SW2
    SW2---TO1
    end

    subgraph PARNU[PÄRNU BRANCH]
    R3[Router-Parnu]
    SW3[Switch-Parnu]
    PB1[Parnu Branch 25 hosts]
    
    R3---SW3
    SW3---PB1
    end

    I((Internet))---R1
    R1---R2
    R1---R3

    classDef router fill:#f96,stroke:#333,stroke-width:2px
    classDef switch fill:#9cf,stroke:#333,stroke-width:2px
    classDef endpoint fill:#9f9,stroke:#333,stroke-width:2px
    classDef tallinn fill:#e6ffe6,stroke:#333,stroke-width:2px
    classDef tartu fill:#fff2e6,stroke:#333,stroke-width:2px
    classDef parnu fill:#e6f3ff,stroke:#333,stroke-width:2px
    
    class R1,R2,R3 router
    class SW1,SW2,SW3,SW4,SW5 switch
    class HQ1,TO1,PB1,IT1,RC1 endpoint
    class HQ tallinn
    class TARTU tartu
    class PARNU parnu
```
---

TO BE CONTINUED