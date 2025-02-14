# Teema 12: Marsruutimine. Kuidas marsruuter töötab?

## Sissejuhatus marsruutimisse

![Router](./media/router.png)

Marsruutimine on võrgutehnoloogia põhiprotsesse, mis võimaldab andmepakettide liikumist erinevate võrkude vahel. Marsruutimine toimub võrgu kolmandal kihil (Layer 3) ja põhineb IP-aadressidel. Marsruutimise peamine eesmärk on tagada pakettide efektiivne ja usaldusväärne kohaletoimetamine lähtepunktist sihtpunkti.

![Router Network device](https://1.bp.blogspot.com/-oDx0rt6_hBI/XXUBN3d4jpI/AAAAAAAAGow/x2btZmgm3WY4AEEIaAB3QIybx6OPZfGzwCLcBGAs/s1600/Router%2Binterfaces.JPG)

## ❓ Küsimus  
Kas iga mitme interfaisiga arvuti on ruuter või mitte? 🤔

## 🚀 Oluline Teave!  

Router peab marsruutimiseks omama järgmisi andmeid:  

- 📍 **Sihtaadress (Address of destination)**  
- 🌐 **Kättesaadavad teed kõikidesse kaugvõrkudesse (Available paths to all remote networks)**  
- 🛤️ **Parim tee iga kaugvõrgu jaoks (The best path to each remote network)**  

![Router Diagram](https://cdn.educba.com/academy/wp-content/uploads/2019/11/What-is-Router-1.jpg)

### Marsruuter vs Kommutaator

Oluline on mõista erinevust marsruuteri ja kommutaatori vahel:

![Switch vs Router Chart](https://planetechusa.com/wp-content/uploads/2021/04/switch-vs-router-chart.png)

**Kommutaator (Switch):**
- Töötab OSI mudeli teisel kihil (Layer 2)
- Kasutab MAC-aadresse
- Ühendab seadmeid samas võrgus
- Ei saa ühendada erinevaid võrke

**Marsruuter (Router):**
- Töötab OSI mudeli kolmandal kihil (Layer 3)
- Kasutab IP-aadresse
- Ühendab erinevaid võrke
- Võimaldab marsruutimist ja filtreerimist

![Router Packet Forwarding](https://ptgmedia.pearsoncmg.com/images/chap1_9781587134289/elementLinks/01fig24_alt.jpg)

### Levinumad marsruutimistabeli märgistused:
- **C** - Connected (Otseühendatud võrgud)
- **S** - Static (Staatilised marsruudid)
- **O** - OSPF routes
- **D** - EIGRP routes
- **R** - RIP routes
- **B** - BGP routes

## Võrgu Marsruutimise Näide 🌐

### Skeemide Tüübid

Võrgutopoloogia visualiseerimiseks kasutame kahte erinevat skeemi, mis aitavad mõista võrgu struktuuri nii füüsilisel kui ka loogilisel tasandil. 

L2 füüsiline skeem 🔌 kujutab seadmete tegelikku füüsilist paigutust ja ühendusi võrgus. See näitab, kuidas hostid, kommutaatorid ja ruuterid on omavahel füüsiliselt ühendatud kaablitega. 

L3 loogiline skeem 🧠 illustreerib võrgu abstraktsemat, loogilist struktuuri IP-võrkude tasandil. See näitab, kuidas erinevad võrgusegmendid on omavahel ühendatud.

```mermaid
graph TB
    subgraph Füüsiline_Skeem
        PC1[PC1<br>150.9.1.10/24] --- S1{Kommutaator 1}
        PC11[PC1.1<br>150.9.1.11/24] --- S1
        S1 --- R1((Router 1<br>150.9.2.253/30))
        R1 --- R2((Router 2<br>150.9.4.33/27))
        R2 --- S2{Kommutaator 2}
        S2 --- PC2[PC2<br>150.9.4.34/27]
    end

    subgraph Loogiline_Skeem
        subgraph Võrk_1[150.9.1.0/24]
            Host1[PC1<br>150.9.1.10] -.- Host11[PC1.1<br>150.9.1.11]
            Host1 & Host11 -.- GW1[Default GW:<br>150.9.1.1]
        end
        
        subgraph Võrk_2[150.9.2.252/30]
            R1_L[R1<br>150.9.2.253] -..- R2_L[R2<br>150.9.2.254]
        end
        
        subgraph Võrk_3[150.9.4.32/27]
            Host2[PC2<br>150.9.4.34] -.- GW2[Default GW:<br>150.9.4.33]
        end
        
        Võrk_1 === Võrk_2
        Võrk_2 === Võrk_3
    end

    classDef pc fill:#E1ECF4,stroke:#2B579A,stroke-width:2px;
    classDef switch fill:#4472C4,stroke:#2B579A,stroke-width:2px,color:white;
    classDef router fill:#70AD47,stroke:#507E32,stroke-width:2px,color:white;
    classDef network fill:none,stroke:#2B579A,stroke-width:2px,stroke-dasharray:5 5;
    classDef gateway fill:#FFA500,stroke:#FF8C00,stroke-width:2px;

    class PC1,PC11,PC2,Host1,Host11,Host2 pc;
    class S1,S2 switch;
    class R1,R2,R1_L,R2_L router;
    class GW1,GW2 gateway;
    class Võrk_1,Võrk_2,Võrk_3 network;
```

## /30 (/31) Võrgumask Ruuterite Vahelises Ühenduses 🔗

Ruuterite vahelistes ühendustes kasutatakse /30 võrgumaski just seetõttu, et see võimaldab ainult 2 kasutatavat IP-aadressi, mis on täpselt see, mida ruuterite omavaheliseks ühendamiseks vaja on. See on väga efektiivne lahendus, kuna punkt-punkt ühenduses ruuterite vahel pole kunagi vaja rohkem kui kahte IP-aadressi. 

## TÄHTIS MÄRKUS VAIKIMISI LÜÜSI KOHTA! 🚨

**Kuldreegel**: Vaikimisi lüüs PEAB alati kuuluma samasse võrku, kus asub host! 

Selgitus praktilise näite põhjal:
Kui host asub võrgus 192.168.1.0/24, siis tema vaikimisi lüüs PEAB omama IP-aadressi formaadis 192.168.1.X.

🔴 Vale konfiguratsioon:
```
Host IP: 192.168.1.10/24
Default Gateway: 192.168.2.1  // EI TÖÖTA! Lüüs on teises võrgus!
```

🟢 Õige konfiguratsioon:
```
Host IP: 192.168.1.10/24
Default Gateway: 192.168.1.1  // TÖÖTAB! Lüüs on samas võrgus
```

## Marsruutimise Protsess

### 1. Paketi kontroll ja päise töötlemine
- FCS (Frame Check Sequence) kontroll vigade tuvastamiseks
- L2 (Ethernet) päise eemaldamine
- IP-paketi säilitamine edasiseks töötlemiseks

### 2. Marsruudi valimine
- IP-aadressi võrdlemine marsruutimistabeliga
- Parima marsruudi valimine longest match reegli alusel
- Järgmise punkti (next hop) määramine

### 3. Paketi töötlemine ja edastamine
- TTL väärtuse vähendamine
- Vajadusel paketi fragmenteerimine
- Uue L2 päise lisamine
- Paketi edastamine järgmisele seadmele

## Marsruutimise Tüübid 🌐

### 1. Staatiline Marsruutimine (Lihtne) 🔨

Staatilise marsruutimise puhul seadistatakse marsruudid käsitsi:
- Administraator määrab ise kõik marsruudid
- Iga võrgu jaoks määratakse kindel tee
- Marsruuter "teab" ainult seda, mis talle on öeldud
- Sobib väikestele võrkudele
- Lihtne seadistada ja hallata

Näide:
```
Router(config)# ip route 150.9.4.0/27 Serial0/0
```

### 2. Dünaamiline Marsruutimine (Keeruline) 🔄

Dünaamilise marsruutimise puhul vahetavad ruuterid omavahel marsruutimisinformatsiooni:

### Mastaabid:
- **Internet**: ~850,000 marsruuti
  - Uue võrgu info levib 10-15 minutiga üle maailma
  - Täisvaade (full view) tabelid ruuterite vahel
  
- **Ettevõtte võrgud**:
  - Muudatused levivad sekundite või millisekunditega
  - Väiksem marsruutide arv
  - Kiirem konvergents

### Levinumad Protokollid:
- OSPF (Open Shortest Path First)
- BGP (Border Gateway Protocol)
- EIGRP (Enhanced Interior Gateway Routing Protocol)
- RIP (Routing Information Protocol)

### Kokkuvõte ⭐

| Omadus | Staatiline | Dünaamiline |
|---------|------------|-------------|
| Seadistamine | Käsitsi | Automaatne |
| Sobivus | Väikesed võrgud | Suured võrgud |
| Haldus | Lihtne | Keerukam |
| Kohanemisvõime | Madal | Kõrge |
| Ressursikasutus | Madal | Kõrgem |
| Muudatuste kiirus | Manuaalne | Automaatne (sek-min) |

## MAC-aadresside käsitlemine marsruutimisel

MAC-aadresside muutmine on oluline osa marsruutimisprotsessist. Iga kord, kui pakett läbib marsruuteri:
- Lähtekoha MAC muutub marsruuteri liideseks
- Sihtkoha MAC muutub järgmise seadme aadressiks
- IP-aadressid jäävad samaks

### Näide MAC-aadresside muutumisest:
```
Lähe: PC1 -> Core
MAC: PC1_MAC -> CORE_MAC1
IP: 150.9.1.10 -> 150.9.4.33

Core -> B1
MAC: CORE_MAC2 -> B1_MAC1
IP: 150.9.1.10 -> 150.9.4.33

B1 -> PC2
MAC: B1_MAC2 -> PC2_MAC
IP: 150.9.1.10 -> 150.9.4.33
```

## Marsruutimistabeli struktuur

### 📋 Windows
Kasutage käsku **`route print`**, et vaadata marsruutimistabelit Windowsis.

```bash
C:\> route print
```

![Routing Table](https://www.learncisco.net/wp-content/themes/learncisco/assets/images/icnd1/42-routing-tables.jpg)

![Routing Example](https://lh3.googleusercontent.com/proxy/Ejil1j8cUw5TPPq5SeoNFRR-XEOh--c7MKuWz9rGIYClYwCSqxh1OWZE6EiT2QBzv_-3hMbk9JZOso9PfMBOCs-tP9gvP7Z6pmvR6TIMGtnGyWZ-c3BHBBXA=w1200-h630-p-k-no-nu)

![Junos Routing Overview](https://image.slidesharecdn.com/junosroutingoverviewall-in-one-140912210812-phpapp01/95/junos-routing-overview-from-juniper-53-638.jpg?cb=1410556602)


Marsruutimistabel sisaldab:
1. **Sihtkoha Võrguaadress** (destination) 🎯
   - Siht-võrgu aadress, kuhu paketid tuleb saata

2. **Võrgumask** (netmask) 🔍
   - Määratleb siht-võrgu alamvõrgu osa

3. **Gateway Aadress** (gateway) 🌉
   - Järgmise hüppe marsruuteri aadress
   - Pole vajalik otseühendatud võrkude puhul

4. **Marsruudi Meetrika** (metric) ⚖️
   - Väärtus, mida kasutatakse marsruudi eelistuse määramiseks

5. **On-link** ühendused 🔗
   - Tähistab otseühendatud võrke
   - Seade saab suhelda otse, ilma marsruuterita
   - Näide:
     ```
     192.168.1.0/24    on-link    192.168.1.100
     ```
   - Linux süsteemides näidatakse kui "scope link"

6. **Staatilised marsruudid** (S) 🔒
   - Tähistatakse 'S' tähega marsruutimistabelis
   - Näide Cisco tabelis:
     ```
     S  192.168.2.0/24 [1/0] via 10.0.0.1
     ```
   - [1/0] näitab administratiivset distantsi (1) ja meetrikat (0)
   - Käsitsi lisatud marsruudid
   - Muudetavad ainult administraatori poolt

## Administratiivne Distans (AD)

![Image Description](https://www.flackbox.com/wp-content/uploads/2020/03/word-image-6.png)

AD väärtused on vahemikus 0-255:
- **0**: Connected (otseühendatud)
- **1**: Static (staatilised marsruudid)
- **20**: External BGP
- **90**: EIGRP
- **110**: OSPF
- **115**: IS-IS
- **120**: RIP

## Longest Match reegel

![Best Longest Match](https://image.slideserve.com/765527/best-longest-match20-l.jpg)

Longest match reegel on fundamentaalne marsruutimise printsiip:
- Valitakse kõige pikema maskiga sobiv marsruut
- Pikem mask tähendab täpsemat sihtkoha määratlust

![Routing Table Lookup - Longest Prefix Match](https://image1.slideserve.com/3546085/routing-table-lookup-longest-prefix-match-l.jpg)

## L3 kommutaator marsruutimislahendusena

### Eelised:
- ⚡ **Madalam latentsus**: Kiirem andmeedastus
- 🚀 **Suurem läbilaskevõime**: Ideaalne andmekeskustele
- 🌐 **Efektiivne VLANide vaheline suhtlus**: Kiire sisevõrgu liiklus
- 🔒 **Parem jõudlus ja turvalisus sisevõrgus**: Sisseehitatud kaitsemehhanismid

### Liidesed:
1. **Physical L2 Interface (🌐)** – tavalised kommutatsiooniportid, mis töötavad **Ethernet**-tasandil
2. **SVI (Switch Virtual Interface) (🛡️)** – VLANide L3 liidesed
3. **Physical L3 Interface (🔗)** – Pordid, mis töötavad otse **L3 režiimis**

## Marsruutimiskäsud erinevatel platvormidel

### Cisco IOS
```cisco
ROUTER_CORE#show ip route
C 150.9.1.0/24 is directly connected, Ethernet0
C 150.9.2.252/30 is directly connected, Ethernet1
S 150.9.4.32/27 [1/0] via 150.9.2.254
```

### Juniper JunOS
```juniper
user@ROUTER_B> show route table
150.9.4.32/27     *[Direct/0] 00:08:42
                    > via ge-0/0/0.0
```

### Linux
```bash
$ ip route
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.2
```

## Marsruuterite tüübid ja sümbolid

Kaasaegsed marsruuterid võivad täita erinevaid rolle:
- Tavaline marsruuter
- Tulemüüriga marsruuter
- Traadita marsruuter
- VoIP marsruuter
- Content Service marsruuter
- Multiteenuse marsruuter

![CISCO Routers Design Elements](https://www.conceptdraw.com/How-To-Guide/picture/CISCO-routers-design-elements.png)

## Video: Introduction to Networking  
[![Introduction to Networking](https://img.youtube.com/vi/qIIRSwnIcaA/0.jpg)](https://www.youtube.com/watch?v=qIIRSwnIcaA)  
**Source:** [YouTube - Introduction to Networking](https://www.youtube.com/watch?v=qIIRSwnIcaA)

## Praktiline Näide 💡

Vaatame lähemalt, kuidas toimub tegelik andmevahetus meie võrgus. Kui PC1 soovib saata andmeid teises võrgus asuvale PC2-le:

1. PC1 kontrollib sihtkoha IP-aadressi (150.9.4.34)
2. Kuna aadress ei asu PC1 enda võrgus (150.9.1.0/24), saadab PC1 paketi oma vaikimisi lüüsi (150.9.1.1)
3. Router 1 võtab paketi vastu ja edastab selle Router 2-le
4. Router 2 edastab paketi lõpuks PC2-le läbi kommutaatori

See protsess toimub iga paketi puhul, tagades andmete korrektse liikumise erinevate võrkude vahel. 🎯