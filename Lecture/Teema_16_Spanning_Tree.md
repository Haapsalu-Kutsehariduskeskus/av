# Teema 16: Spanning Tree protokollide perekond. STP, RSTP, PVST/RPVST, MSTP.

## 📜 **Mis on Spanning Tree ja miks see on oluline?**  

- **Ethernet-võrkudes võib tekkida SILMUS (LOOP)**, kui **kommutaatorid (switchid) on valesti ühendatud**.
- **Spanning Tree Protocol (STP)** aitab **silmuseid vältida**, blokeerides üleliigsed ühendused.  
- Ilma STP-ta võib võrk **aeglustuda või täielikult lakata töötamast**.  

---

## ⚠️ **Kuidas silmus (loop) tekib?**  

**1️⃣ Liiga palju ühendusi**  
- Kujuta ette, et meil on kaks kommutaatorit (A ja B).  
- Mõlemad on ühendatud omavahel **mitme kaabliga** (nt kaks Ethernet-kaablit).  

**2️⃣ Broadcast-kaadrid hakkavad korduma**  
- Kui arvuti saadab **broadcast-kaadri (teate, mis läheb kõigile)**, saadavad kommutaatorid selle edasi **kõikidele portidele**.  
- See tähendab, et sama kaader võib **tulla tagasi algsesse kommutaatorisse**, luues lõputu tsükli (silmuse).  

**3️⃣ Võrk jookseb kinni**  
- Kuna Etherneti protokollis **pole TTL-i (Time-To-Live)**, ei tea kommutaatorid, **millal lõpetada kaadri edastamine**.  
- **Kaadrid hakkavad võrgus lõputult liikuma**, ummistades kõik lingid ja aeglustades võrku.  

🔍 **Võtame lihtsa näite:**  

```mermaid
sequenceDiagram
    participant A as Kommutaator A
    participant B as Kommutaator B
    participant C as Kommutaator C
    
    A->>B: 📡 Broadcast kaader
    B->>C: 📡 Broadcast kaader
    C->>A: 📡 Broadcast kaader (tagasi algusse!)
    A->>B: 📡 Broadcast kaader (uuesti!)
```

**📌 Probleem:** Kaader ei peatu kunagi → **võrk on ummistunud!**  

✅ **STP lahendus:** **Blokeerib ühe lingi**, et vältida lõputut ringlust.  

---

## 📌 **STP Olulised Mõisted**  

| 🔍 **Mõiste** | 📝 **Lihtne Selgitus** |
|--------------|------------------|
| **🛠️ Root Bridge** | **Võrgu keskne kommutaator** (valitakse automaatselt). |
| **🔗 Root Port** | **Parim port Root Bridge’ini jõudmiseks**. |
| **🚦 Blocking Port** | **Blokeeritud port, et vältida silmuseid**. |

👨‍💻 **Näide:** Kui kolm switchi on ühendatud ringina, valitakse üks neist **Root Bridge’iks** ja üleliigsed ühendused **blokeeritakse**.

![Root Bridge Election Process](https://www.networkacademy.io/sites/default/files/inline-images/root-bridge-election-process-step-1.svg)

*Image Source: [Network Academy](https://www.networkacademy.io/) - [Root Bridge Election Process](https://www.networkacademy.io/sites/default/files/inline-images/root-bridge-election-process-step-1.svg)*


---

## ⏳ **STP Pordiseisundid**  

| 🎯 **Seisund** | 📝 **Mida see tähendab?** |
|---------------|------------------|
| **Blocking** | ❌ Ei edasta liiklust, kuulab BPDU-sid. |
| **Listening** | 🔄 Valmistub edastamiseks, ei õpi MAC-aadresse. |
| **Learning** | 🏗️ Õpib MAC-aadresse, ei edasta veel andmeid. |
| **Forwarding** | ✅ Pordil **täielik töövõime**. |

![Spanning Tree Port States](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgYNNj7TtBV5SKCX6E7eclsvl43QTMCe-tCkC_9Wxvf3zbAMpgiij_yrEWoY9VLpN03ho83VOIp3-oDViTG_wNB3Q7vVZsV58W-k6Xvzi5qefzB8T3CcSMFP9boA2EnR6R2b8QECezVe7c9/s1600/www.ccnaccnplinux.com+Spanning+Tree+Port+States00005.jpg)

*Image Source: [www.ccnaccnplinux.com](https://www.ccnaccnplinux.com/)*

---

## 🚀 **RSTP (Rapid STP - IEEE 802.1w)**  

| 🚀 **Mida see parandab?** | 📝 **Lihtne selgitus** |
|--------------------------|------------------|
| **Kiirem koondumisaeg** ⏳ | ✅ Üleminek võtab **1-2 sekundit** (vs STP 30-50s). |
| **Uued porditüübid** 🔄 | ✅ **Alternate Port** – Varutee juurkommutaatorile. |
| **Backup Port** 🔁 | ✅ **Varutee sama segmendi sees**. |

👨‍💻 **Ehk:**  
Kui mõni **ühendus katkeb**, siis **STP arvutab kõik uuesti (~50 sek)**, aga **RSTP suudab kiiremini reageerida (1-2 sek)**.  

![Image](https://miro.medium.com/v2/resize:fit:476/0*8yJ7aCSufO-wlnEk.jpg)

*Image Source: [Medium](https://medium.com/)*


---

## 🔀 **PVST+ & RPVST+ (Cisco)**  

**📌 Cisco PVST+ ja RPVST+ eelised:**  

| 🔍 **Funktsioon** | 📝 **Mida see teeb?** |
|------------------|------------------|
| **Eraldi STP puu iga VLAN-i jaoks** | ✅ VLAN A võib kasutada ühte teed, VLAN B teist! |
| **Parem võrgu koormuse jagamine** ⚖️ | ✅ Võrk töötab **efektiivsemalt**, kui erinevad VLAN-id kasutavad erinevaid linke. |

🔴 **Puudused:**  
- ❌ Ainult **Cisco seadmetele**.  
- ❌ Rohkem protsessoriressursse vaja.  

![Cisco Community Image](https://community.cisco.com/t5/image/serverpage/image-id/160582i8DC8A3943B08DE0F/image-size/large?v=v2&px=999)

*Image Source: [Cisco Community](https://community.cisco.com/)*

---

## 🏆 **MSTP (Multiple Spanning Tree Protocol - IEEE 802.1s)**  

📌 **Mis see on?**  
- **MSTP on standardiseeritud protokoll** (erinevalt PVST-st, mis on Cisco oma).  
- **Koondab VLAN-id gruppidesse** ja teeb **neile eraldi spanning tree puu**.  

| ✅ **Eelised** |
|-----------------|
| **Töötab erinevate tootjate seadmetega** 🌍 |
| **Ressursside kokkuhoid** 🖥️ (mitte iga VLAN ei vaja oma STP-d) |
| **Tasakaalustab võrgu koormust** ⚖️ |

👨‍💻 **Lihtne näide:**  
Kui meil on **50 VLAN-i**, siis **PVST+ teeks 50 STP arvutust**, aga **MSTP teeb ainult 3-4 arvutust** (gruppide järgi).  

---

## 🔐 **STP Kaitsemehhanismid**  

| 🛡️ **Kaitse** | 📝 **Mida see teeb?** |
|----------|----------------|
| **BPDU Guard** 🚫 | ❌ **Keelab BPDU-de vastuvõtmise**, kui pordil pole lubatud kommutaatoreid. |
| **Root Guard** 🔒 | ❌ **Takistab Root Bridge'i muutmist** (hoiab võrgu stabiilsena). |
| **PortFast** ⚡ | ✅ **Kiire üleminek Forwarding-seisundisse** (kasutatakse PC-de portidel). |

**Näide:** Kui keegi ühendab uue switchi ilma teadmiseta, **Root Guard ja BPDU Guard aitavad vältida võrgu rikkeid**.  

---

## 🏁 **Spanning Tree Protokollide Võrdlus**  

| 🏷️ **Protokoll** | 🔎 **Standard** | ⏳ **Koondumisaeg** | 🔄 **Eraldi puu VLAN-ile?** |
|-----------|--------------|------------------|----------------------|
| **STP** | IEEE 802.1D | 🕒 **30-50 sek** | ❌ Ei |
| **RSTP** | IEEE 802.1w | ⚡ **1-2 sek** | ❌ Ei |
| **PVST+** | Cisco | 🕒 **30-50 sek** | ✅ Jah |
| **RPVST+** | Cisco | ⚡ **1-2 sek** | ✅ Jah |
| **MSTP** | IEEE 802.1s | ⚡ **1-2 sek** | 🏷️ **VLAN-i gruppide kaupa** |

👨‍💻 **Selgitus:**  
- Kui tahad **väga lihtsat võrku**, kasuta **STP**.  
- Kui tahad **kiiret reageerimist**, kasuta **RSTP**.  
- Kui sul on **palju VLAN-e**, kasuta **MSTP või PVST+**.  

[**Understand STP Loop Guard and UDLD Features**](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol-stp-8021d/218321-configure-stp-with-loop-guard-and-bpdu-s.html)  
*Source: [Cisco Official Documentation](https://www.cisco.com/)*

 Vabandust vääritimõistmise pärast! Täiendan sinu algset teksti, lisades selgitusi kaitsemehhanismide ja teiste protokollide kohta, kasutades emojisid.

## STP kokkuvõte
```mermaid
flowchart TD
    subgraph "The Problem: Network Loop"
        A1[Switch 1 gets a message] -->|"Forwards to"| B1[Switch 2]
        B1 -->|"Forwards to"| C1[Switch 3]
        C1 -->|"Forwards back to"| A1
        A1 -->|"OMG not again!"| B1
    end

    subgraph "The Fix: Spanning Tree Protocol"
        A2[Switch 1 becomes Root Bridge] -->|"I'm the boss!"| B2[Switch 2]
        A2 -->|"I'm the boss!"| C2[Switch 3]
        B2 -->|"Link between us? BLOCKED!"| C2
        C2 -->|"Can't send this way"| B2
    end

    subgraph "When Links Break"
        A3[Switch 1: Root Bridge] -->|"Link breaks!"| C3[Switch 3: Oh no!]
        C3 -->|"Emergency! Unblock my port!"| B3[Switch 2]
        B3 -->|"You can use me to reach the Root now"| C3
    end

    style A1 fill:#ff9999
    style B1 fill:#ff9999
    style C1 fill:#ff9999
    
    style A2 fill:#99ff99
    style B2 fill:#99ff99
    style C2 fill:#99ff99
    
    style A3 fill:#9999ff
    style B3 fill:#9999ff
    style C3 fill:#9999ff
```
## Probleem
- Lülitid saadavad sõnumeid ringiratast 🔄
- Tekib silmus, mis koormab võrku ⚠️
- Võrk muutub kasutuskõlbmatuks 💥

## Lahendus: STP
1. **Vali Root Bridge** - üks lüliti saab juurlülitiks 👑
2. **Blokeeri mõned teed** - silmuste vältimiseks 🚫
3. **Kiire taastamine** - kui ühendus katkeb, ava alternatiivne tee 🔄

STP on nagu liikluspolitseinik, kes blokeerib mõned teed, et keegi ei saaks lõputult ringi sõita! 🚦

## STP kaitsemehhanismid 🛡️
- **BPDU Guard** - kaitseb serverite porte, blokeerides need kui BPDU ilmub 🚫
- **Root Guard** - takistab võõraste lülitite juurlülitiks saamist 👮
- **Loop Guard** - tuvastab ühepoolseid ühendusi, mis võivad silmuseid tekitada 🔍
- **PortFast** - lubab lõppseadmetel kohe andmeid saata, jätmata STP ootamist ⚡

## Teised silmuste kaitse protokollid 🌐
- **RSTP** - Rapid STP, kiirem taastumine (~2-3 sekundit) 🏎️
- **MSTP** - võimaldab mitut STP instantsi erinevatele VLAN-idele 🧩
- **PVST+** - Cisco versioon, töötab iga VLAN-iga eraldi 🔌
- **Etherchannel/LACP** - mitu linki töötavad kui üks, suurendades kiirust ja pakkudes varundust 🔗

STP on elutähtis võrgusilmuste vastu, muidu võrk "uputaks ennast" broadcast-tormidega! 🌊💻