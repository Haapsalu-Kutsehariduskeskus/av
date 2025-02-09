# Teema 13: Marsruuterite arhitektuur ja teostus

# Marsruuterite arhitektuur ja teostus

![Router Models Overview](https://www.bleepstatic.com/content/hl-images/2023/04/23/Routers.jpg)  
**Source:** [Bleeping Computer](https://www.bleepstatic.com/content/hl-images/2023/04/23/Routers.jpg)

![Router Architecture](https://assets.omscs.io/notes/0104.png)  
**Source:** [OMCS Notes](https://assets.omscs.io/notes/0104.png)

Arhitektuur on üles ehitatud nii, et meil on juhtimiskontuur (Control Plane) ja protokollide marsruutimine. See arhitektuur ei kehti ainult marsruuteritele, vaid ka võimsatele L3 kommutaatoritele. Marsruutimine toimub protokollide RIP, OSPF, IS-IS abil.

![Network Management Overview](https://mgmt.rstforum.net/uploads/media-1701418352497.png)  
**Source:** [RST Forum](https://mgmt.rstforum.net/uploads/media-1701418352497.png)

```mermaid
flowchart TB
    subgraph "Management Plane (CPU + IOS)"
        MGMT[Management Functions<br>Haldus funktsioonid]
    end

    subgraph "Control Plane (CPU + IOS)"
        RIB[RIB<br>Routing Information Base<br>Marsruutimise andmebaas]
        ACL[ACL<br>Access Control Lists<br>Juurdepääsu kontrollnimekiri]
        FWNAT[FW/NAT<br>Firewall/Network Address Translation<br>Tulemüür/Võrguaadresside tõlkimine]
    end

    subgraph "Data Plane (Network Processor)"
        FIB[FIB<br>Forwarding Information Base<br>Edastamise andmebaas]
        PKT[Packet Processing<br>Pakettide töötlemine]
    end

    CPU[CPU + IOS] --> MGMT
    CPU --> RIB
    CPU --> ACL
    CPU --> FWNAT
    
    NP[Network Processor<br>Võrguprotsessor] --> FIB
    NP --> PKT

    RIB --> FIB
    ACL --> PKT
    FWNAT --> PKT

    style CPU fill:#f96,stroke:#333
    style NP fill:#96f,stroke:#333
    style MGMT fill:#ff9,stroke:#333
    style RIB fill:#9f9,stroke:#333
    style ACL fill:#9f9,stroke:#333
    style FWNAT fill:#9f9,stroke:#333
    style FIB fill:#99f,stroke:#333
    style PKT fill:#99f,stroke:#333
```

**Marsruuteri põhikomponendid** (Router Main Components):

1. **Kontrollkiht** (Control Plane):
   - RIB (Routing Information Base) - marsruutimise andmebaas
   - ACL (Access Control Lists) - juurdepääsu kontrollnimekiri
   - FW/NAT (Firewall/Network Address Translation) - tulemüür/võrguaadresside tõlkimine

2. **Andmekiht** (Data Plane):
   - FIB (Forwarding Information Base) - edastamise andmebaas
   - Teostab pakettide füüsilist edastamist (Physical packet forwarding)

3. **Juhtimine** (Management):
   - CPU koos IOS tarkvaraga juhib Control Plane'i tööd
   - NP (Network Processor) teostab Data Plane'i funktsioone
   - CPU tegeleb ka seadme haldamisega (Management Plane)

```mermaid
flowchart TD
    %% Backbone Router at top level
    MB[Backbone Router]
    MB1[Speed: Very high]
    MB2[Example: Cisco ASR 9000]
    MB3[Usage: ISP core network]
    
    %% Border Router in middle
    PB[Border Router]
    PB1[Filtering: ACL, Firewall]
    PB2[Example: Cisco ASR 1000]
    PB3[Usage: Enterprise edge]
    
    %% Branch Router at bottom
    KB[Branch Router]
    KB1[Functions: Basic routing, VPN]
    KB2[Example: Cisco ISR 1100]
    KB3[Usage: Small offices]
    
    %% Show hierarchical connections
    MB --> MB1
    MB --> MB2
    MB --> MB3
    
    MB --> PB
    
    PB --> PB1
    PB --> PB2
    PB --> PB3
    
    PB --> KB
    
    KB --> KB1
    KB --> KB2
    KB --> KB3
    
    style MB fill:#f96,stroke:#333
    style PB fill:#99f,stroke:#333
    style KB fill:#9f9,stroke:#333
```

**Marsruuterite liigid** (Router Types):

1. **Magistraalmarsruuter** (Backbone/Core Router):
   - Põhiülesanne: pakettide edastamine kõrge kiirusega
   - Kasutusel: suurtes võrkudes ja andmekeskustes

2. **Piirilevalgumarsruuter** (Border/Edge Router):
   - Põhiülesanne: maksimaalne paindlikkus filtreerimisel ja liikluse profileerimisel
   - Kasutusel: võrkude piiril, ühendab sisevõrku välisega

3. **Kaugkontorite ja kodumarsruuterid** (Branch/Home Routers):
   - Põhiülesanne: põhilised võrgufunktsioonid
   - Kasutusel: väiksemates kontorites ja kodudes

**Täiendav jaotus** (Additional Classification):
- Marsruutereid saab jagada vastavalt nende asukohale võrgus (sisevõrk vs välisvõrk)
- Samuti jaotatakse neid kasutuse järgi:
  - Sideoperaatorite marsruuterid (Service Provider Routers)
  - Ettevõtte marsruuterid (Corporate Routers)

## Võrgu näide Teliaga

```mermaid
graph TB
    subgraph INTERNET
        I((Internet))
    end
    
    subgraph "TELIA NETWORK / TELIA VÕRK"
        TBR[Telia Backbone Router<br>Telia Magistraalmarsruuter<br>Cisco ASR 9000]
        TER[Telia Edge Router<br>Telia Piirilevalgumarsruuter<br>Cisco ASR 1000]
    end
    
    subgraph "OFFICE BUILDING / KONTOR"
        direction TB
        subgraph "WAN SIDE / WAN POOL"
            ER[Edge Router<br>Piirilevalgumarsruuter<br>Cisco ISR 4000]
            FW[Firewall<br>Tulemüür<br>Palo Alto]
        end
        
        subgraph "LAN SIDE / LAN POOL"
            CS[Core Switch<br>Põhikommutaator<br>Cisco Catalyst 9300]
            
            subgraph "FLOOR 1 / 1. KORRUS"
                AS1[Access Switch 1<br>Ligipääsukommutaator 1]
                W1[WiFi AP 1<br>WiFi seade 1]
                PC1[Workstations<br>Tööjaamad]
                PR1[Printer<br>Printer]
            end
            
            subgraph "FLOOR 2 / 2. KORRUS"
                AS2[Access Switch 2<br>Ligipääsukommutaator 2]
                W2[WiFi AP 2<br>WiFi seade 2]
                PC2[Workstations<br>Tööjaamad]
                PR2[Printer<br>Printer]
            end
            
            subgraph "SERVER ROOM / SERVERIRUUM"
                SRV1[Server 1<br>Email & Files]
                SRV2[Server 2<br>Database]
            end
        end
    end
    
    %% Connections
    I --- TBR
    TBR --- TER
    TER --- ER
    ER --- FW
    FW --- CS
    CS --- AS1 & AS2
    CS --- SRV1 & SRV2
    AS1 --- W1 & PC1 & PR1
    AS2 --- W2 & PC2 & PR2
    
    %% Styles
    classDef wan fill:#f96,stroke:#333,stroke-width:2px
    classDef lan fill:#99f,stroke:#333
    classDef endpoint fill:#9f9,stroke:#333
    
    class TBR,TER,ER,FW wan
    class CS,AS1,AS2 lan
    class PC1,PC2,PR1,PR2,W1,W2,SRV1,SRV2 endpoint
```
---

## Korporatiivne tootevalik:

**Huawei:**  
- AR 150, AR 200  
- AR 1200, AR 2200, AR 3200  
[Huawei Fixed Network Routers](https://carrier.huawei.com/en/products/fixed-network/data-communication/router)  

**Cisco:**  
- ISR 800, SRP 500  
- ISR 1900, ISR 2900, ISR 3900  
[Cisco 800 Series Routers](https://www.cisco.com/c/en/us/support/routers/800-series-routers/series.html)  

**HP:**  
- MSR 900, MSR 20  
- MSR 30, MSR 50  
![HP Router Series Overview](https://image5.slideserve.com/9711152/mid-low-end-routers-family-l.jpg)  

```mermaid
flowchart TB
    subgraph KORPORATIIVNE_TOOTEVALIK
        subgraph Huawei
            H1[AR 150, AR 200]
            H2[AR 1200, AR 2200, AR 3200]
        end

        subgraph Cisco
            C1[ISR 800, SRP 500]
            C2[ISR 1900, ISR 2900, ISR 3900]
        end

        subgraph HP
            HP1[MSR 900, MSR 20]
            HP2[MSR 30, MSR 50]
        end
    end

    I1[Integreeritud lahendused video, hääle,<br>andmete ja turvalisuse jaoks]
    I2[Integreeritud lahendused andmete, WiFi<br>ja põhilise turvalisuse funktsionaalsusega]

    H2 --> I1
    C2 --> I1
    HP2 --> I1

    H1 --> I2
    C1 --> I2
    HP1 --> I2

    style H1 fill:#f9f,stroke:#333
    style H2 fill:#f9f,stroke:#333
    style C1 fill:#9ff,stroke:#333
    style C2 fill:#9ff,stroke:#333
    style HP1 fill:#ff9,stroke:#333
    style HP2 fill:#ff9,stroke:#333
```

**![External Connections on a 2600 Router](https://image2.slideserve.com/4185508/external-connections-on-a-2600-router-l.jpg)**

2600 seeria marsruuteri välised komponendid:
- Jadapordid (Serial Ports)
- FastEthernet pordid
- Konsooliport (Console Port)
- Lisaport (Auxiliary Port)
- Toitelüliti (Power Switch)
- Toitejuhtme ühendus (Power Cord Connection)

**[Cisco Interfaces and Modules Support](https://www.cisco.com/c/en/us/support/interfaces-modules/index.html)**

**![Cisco Network Interface Module](https://www.networktigers.com/cdn/shop/files/cisco-NIM-4E-M_f71ba07c-1795-4ea6-992c-e22a54188b69_large.jpg?v=1729620828)**

Moodulid marsruuterite jaoks:

| **Module Type**                              | **Description**                               |
|-----------------------------------------------|----------------------------------------------|
| **Moodulid marsruutitavate portidega**        | Modules with routable ports for WAN or LAN connectivity. |
| **Moodulid kommutaatoritega**                 | Modules with switching capability, typically for Layer 2 networking. |
| **ADSL moodulid**                             | Asymmetric Digital Subscriber Line (ADSL) modules for broadband access over telephone lines. |
| **3G, 4G, LTE moodulid**                      | Cellular network modules for mobile broadband connectivity. |
| **WiFi moodulid**                             | Modules for wireless LAN (WLAN) connectivity using WiFi standards. |
| **Moodulid sünkroonsete portidega V.35**      | Modules with synchronous ports supporting V.35 interface for high-speed data transmission. |
| **Moodulid PDH (E1, G.703, G703.1)**          | Plesiochronous Digital Hierarchy (PDH) modules supporting E1 and G.703 interfaces for digital voice and data transmission. |
| **Moodulid häälportidega FXS/FXO/E1**         | Modules with voice ports for analog (FXS/FXO) and digital (E1) voice communication. |

Here's the YouTube videos:

[![Video Preview](https://img.youtube.com/vi/Nnh_z8fWJuY/0.jpg)](https://www.youtube.com/watch?v=Nnh_z8fWJuY&t=137s)

[![Video Preview](https://img.youtube.com/vi/lTpMZlUNdCs/0.jpg)](https://www.youtube.com/watch?v=lTpMZlUNdCs)
