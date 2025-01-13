# Teema 6: Ethernet - Kuidas see töötab ja miks see on võrguadministraatorile oluline

## 1. Võrgu põhimõisted

### 1.1 Võrgu operatsioonisüsteem (Network Operating System, NOS)

![Network Operating System Model](https://www.techtarget.com/rms/onlineimages/network_operating_system_model-f.png)

*Image source: [TechTarget](https://www.techtarget.com/)*

For more information, refer to the [TechTarget article](https://www.techtarget.com/).

Võrgu operatsioonisüsteem on tänapäeva arvutivõrkude keskne komponent. See ei ole pelgalt tavaline operatsioonisüsteem, vaid spetsiaalselt võrgukeskkonna jaoks optimeeritud tarkvara, mis võimaldab arvutitel ja muudel seadmetel suhelda.

#### Põhiülesanded:
1. Ressursside jagamine võrgus olevate seadmete vahel.
2. Võrguteenuste haldamine ja koordineerimine.
3. Turvalisuse tagamine ja kasutajate autentimine.
4. Võrguprotokollide tugi ja haldamine.

Tänapäeval on kõik populaarsemad operatsioonisüsteemid (Windows, Linux, macOS, Android, iOS) võrguvõimelised. See tähendab, et nad suudavad hästi hallata nii sissetulevat kui väljaminevat võrguliiklust.

### 1.2 Host ehk võrguseade

Host on iga seade, mis on arvutivõrku ühendatud. See võib olla personaalarvuti, server, nutitelefon, printer või isegi nutikas koduseade.

#### Olulisemad omadused:
1. **Identifitseerimine**: Igal hostil on unikaalne identifikaator võrgus, nagu MAC-aadress ja IP-aadress.
2. **Võrguliides**: Host peab omama vähemalt ühte võrguliidest (NIC), et võrguga ühenduda.
3. **Protokollitugi**: Host peab toetama vajalikke võrguprotokolle, et teiste seadmetega edukalt suhelda.

Lisateabe saamiseks võrgus olevate hostide rolli kohta vaadake [Vikipeedia artiklit võrguhostide kohta](https://en.wikipedia.org/wiki/Host_%28network%29).

### 1.3 Võrguliides (NIC)

Võrguliides ehk võrgukaart on riistvaraline komponent, mis võimaldab seadmel võrguga ühenduda. See täidab olulist rolli andmevahetuses.

#### Funktsioonid:
1. **Andmete puhverdamine**: Puhverdab saadetavaid ja vastuvõetavaid andmeid, et tagada sujuv andmevahetus.
2. **Protokollide tugi**: Toetab erinevaid võrguprotokolle ja suudab osaliselt töödelda neid riistvaraliselt.
3. **MAC-aadressi haldamine**: Sisaldab tehases määratud unikaalset MAC-aadressi.

![Network Interface Card](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9e/Network_card.jpg/1024px-Network_card.jpg)

*Image source: [Wikipedia Commons](https://commons.wikimedia.org/wiki/File:Network_card.jpg)*

### Ethernet kaadri formaat

![Ethernet Frame Header Types](https://ipcisco.com/wp-content/uploads/2018/10/Ethernet-Frame-Header-Types-ipcisco.png)

Ethernet kaader on andmeedastuse põhiühik Etherneti võrgus. Selle struktuur on rangelt määratletud standarditega ja sisaldab järgmisi elemente:

1. **Preambul** (7 baiti): Sisaldab järjestust `10101010`, mida kasutatakse vastuvõtja sünkroonimiseks.

2. **Kaadri alguse tähis (SFD, Start Frame Delimiter)** (1 bait): Järjestus `10101011`, mis märgib kasuliku informatsiooni algust kaadris.

3. **Sihtaadress (DA, Destination Address)** (6 baiti): Näitab seadme MAC-aadressi, millele kaader on suunatud.

4. **Lähteaadress (SA, Source Address)** (6 baiti): Näitab seadme MAC-aadressi, mis saatis kaadri.

5. **Kaadri tüübi või pikkuse väli (T või L, Type or Length)** (2 baiti):
   - **Ethernet II** puhul näitab see väli järgmise taseme protokolli tüüpi (nt `0x0800` IP jaoks, `0x0806` ARP jaoks, `0x8137` IPX/SPX jaoks).
   - **IEEE 802.3** standardis näitab see väli edastatavate andmete pikkust baitides (LLC Data).

6. **Andmed (LLC Data)**:
   - Sisaldavad edastatavat informatsiooni. **Ethernet 802.3** puhul võivad LLC andmete esimesed kaks baiti olla `0xFFFF` (aegunud formaat).
   - **Ethernet SNAP** puhul on LLC andmete esimene bait `0xAA`, mis vastab IEEE 802.2 SNAP standardile.

7. **Kaadri kontrollsõnum (FCS, Frame Check Sequence)** (4 baiti): Kasutatakse kaadri terviklikkuse kontrollimiseks.

#### DSAP ja SSAP selgitus
- **DSAP (Destination Service Access Point)**: Määrab sihtteenuse ligipääsupunkti, millele kaader on suunatud.
- **SSAP (Source Service Access Point)**: Määrab allikateenuse, mis kaadri saatis.

[![Ethernet Frame Structure](https://img.youtube.com/vi/gApiTj4PBq4/0.jpg)](https://www.youtube.com/watch?v=gApiTj4PBq4)

---

## 2. Ethernet Protokoll

### 2.1 MAC-Aadressid

![MAC Address Format](https://data-flair.training/blogs/wp-content/uploads/sites/2/2022/01/mac-address-format.webp)

**MAC-aadress (Media Access Control address)** on unikaalne füüsiline identifikaator, mille määrab seadme tootja ja mida kasutatakse võrguliidese tuvastamiseks. MAC-aadressid on vajalikud andmete edastamiseks Etherneti kaudu, kuna need tagavad igale võrguseadmele unikaalse aadressi.

Näiteks kontorivõrgus kasutab võrguadministraator MAC-aadresse seadmete tuvastamiseks, IP-aadresside sidumiseks või juurdepääsulubade määramiseks. Kui printeri MAC-aadress on teada, saab administraator seadistada võrgu nii, et ainult lubatud seadmed saavad selle printeriga ühendust võtta.

#### MAC-aadressi struktuur:

1. **Esimesed 24 bitti (3 baiti)**: OUI (Organizationally Unique Identifier), mis identifitseerib seadme tootjat. Näited:
   - 00:00:0C - Cisco Systems
   - 00:05:69 - VMware
   - 00:14:22 - Dell

2. **Viimased 24 bitti (3 baiti)**: Tootja määratud unikaalne identifikaator konkreetsele seadmele.

#### Esimese baidi kaks olulist bitti:
- **Esimene bit**: Määrab, kas tegemist on unicast (0) või multicast (1) aadressiga.
- **Teine bit**: Näitab, kas aadress on globaalselt unikaalne (0) või lokaalselt hallatav (1).

### MAC-aadressi formaat
- Esitatud kujul 48-bitise numbrina (6 baiti), näiteks:
  - 00:1A:2B:3C:4D:5E
  - 00-1A-2B-3C-4D-5E
  - 001A.2B3C.4D5E

![MAC Address in CMD](https://smarthelpguides.com/wp-content/uploads/2019/11/ipconfigall-mac-address-cmd.png)

---

### 3. Süvakäsitlus ja andmeedastuse tüübid

#### 3.1 Andmeedastuse tüübid

![Tüübid](https://benisnous.com/wp-content/uploads/2023/11/Types-of-the-MAC-Addresses-Unicast-Multicast-and-Broadcast.jpg)

1. **Unicast**: 
   - Üks-ühele suhtlus kahe seadme vahel võrgus.
   - Kasutatakse enamiku tavapäraste võrgusessioonide jaoks, nagu failide edastamine ja veebis sirvimine.
   - **Näide**: Arvuti saadab andmeid printerile konkreetse MAC-aadressi alusel.

2. **Multicast**:
   - Üks-paljude suhtlus, kus andmed saadetakse ainult huvitunud seadmetele, mitte kogu võrgule.
   - Kasutab spetsiaalseid multicast MAC-aadresse.
   - Levinud aadressid:
     - 01-00-0C-CC-CC-CC (Cisco protokollid nagu CDP ja VTP)
     - 01-80-C2-00-00-00 (Spanning Tree Protocol)

3. **Broadcast**:
   - Üks-kõik suhtlus, kus andmed saadetakse kõikidele võrgu seadmetele.
   - Kasutab aadressi FF-FF-FF-FF-FF-FF.
   - **Näide**: Kasutatakse ARP (Address Resolution Protocol) päringutes.

![Unicast, Multicast, Broadcast Destination Addresses](https://image.slideserve.com/280399/unicast-multicast-broadcast-destination-addresses-l.jpg)
---

#### 3.2 Süvakäsitlus: MAC-aadresside struktuur ja funktsioonid

MAC-aadressi **esimene oktett** sisaldab kahte kriitilist bitti:
1. **Unicast/Multicast eristamine**:
   - 0 = Unicast
   - 1 = Multicast

2. **Globaalne/lokaalne unikaalsus**:
   - 0 = Globaalselt unikaalne (IEEE määratud OUI)
   - 1 = Kohapeal administreeritav aadress.

---

### 4. Multicast ja Broadcast aadresside tabel

Multicast aadresside näited ja nende kasutusprotokollid:

| **Ethernet multicast aadress** | **Kasutatav protokoll**                      |
|---------------------------------|----------------------------------------------|
| 01-00-0C-CC-CC-CC               | Cisco Discovery Protocol (CDP), VTP         |
| 01-80-C2-00-00-00               | Spanning Tree Protocol (STP)                |
| 01-80-C2-00-00-03               | Link Layer Discovery Protocol (LLDP)        |
| 01-80-C2-00-00-0E               | Spanning Tree Protocol (for provider bridges) |
| 01-00-5E-00-00-00 kuni 01-00-5E-7F-FF-FF | IPv4 Multicast (RFC 1112)           |

---

### 5. MTU ja fragmentatsioon

**MTU (Maximum Transmission Unit)** on võrgus kasutatava andmeploki maksimaalne suurus, mida saab edastada ilma fragmentatsioonita. Etherneti MTU on tavaliselt 1500 baiti.

#### MTU ja päised:
- Ethernet-päis: 14 baiti
- IP-päis: 20 baiti
- TCP-päis: 20 baiti
- Maksimaalne kasulik andmesisu (Payload): 1460 baiti

Fragmentatsioon toimub siis, kui andmepaketi suurus ületab määratud MTU. See võib põhjustada jõudluse langust, kuna suurem hulk väiksemaid pakette peab olema uuesti kokkupandud.

![Maximum Transmission Unit (MTU)](https://image2.slideserve.com/4379376/maximum-transmission-unit-mtu-n.jpg)

---

## 4. CSMA/CD Protokoll ja Andmeedastus

CSMA/CD (Carrier Sense Multiple Access with Collision Detection) on Etherneti võrkude aluspõhimõte, mis võimaldab mitmel seadmel jagada sama füüsilist meediumit tõhusalt ja korrastatult. See protokoll aitab vältida kokkupõrkeid ja tagab võrgu tõrgeteta toimimise.

### Toimimise Põhimõtted

1. **Kandesageduse Kuulamine (Carrier Sense)**: Seade kontrollib enne andmete edastamist, kas võrguliin on vaba.
   - Kui liin on hõivatud, ootab seade, kuni see vabaneb.
2. **Mitmikjuurdepääs (Multiple Access)**: Mitmed seadmed jagavad sama füüsilist edastuskanalit kokkulepitud järjekorras.
3. **Kokkupõrgete Tuvastamine (Collision Detection)**: Kui kaks seadet alustavad saatmist samaaegselt, tekib kokkupõrge, mida tuvastatakse signaalide interferentsi abil.

![Collision of first bits in CSMA](https://www.softwaretestinghelp.com/wp-content/qa/uploads/2020/12/Collision-of-first-bits-in-CSMA.png)

### Kokkupõrke Lahendamine

1. Kui seade tuvastab kokkupõrke, katkestab see edastamise.
2. Saadetakse **JAM-signaal**, mis teavitab teisi seadmeid kokkupõrkest.
3. Seade ootab juhusliku aja (Binary Exponential Backoff) enne uuesti proovimist, vähendades korduvate kokkupõrgete ohtu.

---

## Klassikalise Etherneti Piirangud

### Peamised Piirangud

- **Minimaalne Andmevälja Pikkus**:
  - Raami minimaalne andmeväli on 46 baiti (koos juhtväljaga 64 baiti; koos preambulaga 72 baiti).
- **Vahemaapiirangud**:
  - Ethernet 10 Mbit/s standardis on minimaalne raami edastamise aeg 57,5 μs (575 bitintervalle). Selle ajaga levib signaal kaablis kuni 13 280 meetrit. Arvestades signaali edasi-tagasi liikumist, on kaabli maksimaalne pikkus piiratud 2 500 meetriga.
- **Miinimumpikkuse Mõju**:
  - Kui minimaalne raami pikkus oleks 0 baiti, väheneks võrgu maksimaalne ulatus märgatavalt.
- **Kiiruse Mõju Ulatusele**:
  - Suurem protokolli kiirus vähendab võrgu ulatust:
    - 100 Mbit/s korral ulatus 250 meetrit.
    - 1 Gbit/s korral ulatus ainult 25 meetrit.

### Kokkuvõte Klassikalisest Ethernetist

1. Toetab ringhäälingut (WiFi-s samuti).
2. Kokkuvõrked on võimalikud (WiFi-s samuti).
3. Madal andmeedastuskiirus.
4. Kaabli hooldamine on keerukas.

---

## 10BASE-T (IEEE 802.3i)

### Üleminek Koaksiaalilt Keerdpaarkaablitele

- **Koaksiaalkaabli Probleemid**:
  - Kaablirikete tuvastamine oli keeruline ja hooldamine vaevaline.
- **Keerdpaarkaablite Kasutuselevõtt**:
  - 1980ndatel hakati kasutama keerdpaarkaableid ja võrgurepeaatoreid, mis lihtsustasid hooldust.
  - Idee põhines telefonifirmade varasemal kogemusel ja kohandati edukalt kohalike võrkude jaoks.

### Ajaloolised Etapid

- **1988**: AT&T tutvustas StarLAN 10 standardit kiirusega 10 Mbit/s.
- **1990**: IEEE võttis vastu 10BASE-T standardi (IEEE 802.3i-1990, CL14).

![Netgear EN108 Image](https://www.hardwarejet.com/image/cache/netgear/en108-900x900.jpg)

![Adaca Product Image](https://cdn11.bigcommerce.com/s-2nchx8is/images/stencil/original/products/16487/68249/adaca2a7-a7a1-4fe6-a02b-b63d58c2d4d4__74062.1668011790.jpg)

---

## CSMA/CD Rakendamine

Näiteks kontorivõrgus, kus mitmed arvutid jagavad sama kaablit, aitab CSMA/CD vältida andmepakettide kokkupõrkeid:

1. Kui arvuti A soovib andmeid saata, kuulab see esmalt liini.
2. Kui liin on vaba, alustab arvuti A andmete saatmist.
3. Kui samal ajal alustab ka arvuti B, tekib kokkupõrge.
4. Mõlemad arvutid tuvastavad kokkupõrke, saadavad JAM-signaali ja ootavad juhuslikult määratud aja.

### Tänapäevane Kontekst

CSMA/CD on olnud ülioluline Etherneti võrkude arengus, kuid selle tähtsus on vähenenud suuremahuliste võrkude ja switchide kasutuselevõtuga. Tänapäeval kasutatakse sageli **full-duplex** ühendusi, kus kokkupõrkeid ei esine.

---

## 10Base-T Hub ja Võrgusõlmede Ühendamine

10Base-T hub imiteerib koaksiaalkaablivõrku, kus keerdpaarkaablite füüsiliselt eraldiseisvad lõigud moodustavad loogiliselt ühe jagatud keskkonna. Kõik CSMA/CD algoritmi reeglid jäävad kehtima.

### Andmeedastus ja Portide Töö

Iga võrgukaart ühendatakse hubiga kahe keerdpaari kaudu:

- **TX (saatepaar)**: Kasutatakse andmete saatmiseks ning ühendab arvuti hulgimajale (hub).  
- **RX (vastuvõtupaar)**: Kasutatakse andmete vastuvõtmiseks hulgimajalt.  

Repeater (kordaja) võtab signaale vastu ühest sisendpordist, taastab need ja edastab kõikidesse väljunditesse, välja arvatud porti, kust signaal algselt tuli. **Loopback (tagasiside)** ei ole lubatud, kuna see põhjustaks võrgu ummistumise.

### Link Integrity Test (LIT)

10Base-T standard nõuab füüsilise ühenduse testimiseks **Link Integrity Test (LIT)** impulsside kasutamist. Kui võrguseade tuvastab, et LIT impulssid edastatakse ja vastuvõetakse korrektselt, loetakse ühendus toimivaks. Tavaliselt tähistab töötavat ühendust roheline LED-tuli seadme pordi juures.

---

## FastEthernet 100BASE-TX IEEE 802.3u

### FastEtherneti Standardi Vastuvõtmine

1995. aasta mais võttis IEEE vastu FastEtherneti spetsifikatsiooni. See oli oluline samm võrguarhitektuuri arengus, võimaldades andmeedastuse kiirust 100 Mbps ning pakkudes tagurpidi ühilduvust 10 Mbps Ethernetiga.

### Keerukam Füüsilise Taseme Arhitektuur

FastEtherneti füüsiline kiht jagati mitmeks alamkihiks, et toetada erinevaid kaablitüüpe:

- **Optiline kiudkaabel**  
- **Kahe paari keerdpaar (Cat5)**  
- **Nelja paari keerdpaar (Cat3)**  

![100BASE-TX Network](https://networkencyclopedia.com/wp-content/uploads/2019/08/100base-tx-network.jpg)

#### Füüsilise Taseme Kihid

1. **Physical Coding Sublayer (PCS):** Kodeerib andmed efektiivseks edastamiseks (nt MLT-3 kodeering 100BASE-TX puhul).  
2. **Physical Medium Attachment (PMA):** Teisendab digitaalset andmevoogu elektrilisteks või optilisteks signaalideks.  
3. **Medium Dependent Interface (MDI):** Füüsiline liides, mis ühendab võrguadapteri kaabliga (näiteks RJ-45 pistik).

---

## FastEtherneti Arhitektuuri Võrdlus

| **Füüsiline Liides**      | **100Base-FX**           | **100Base-TX**              | **100Base-T4**              |
|---------------------------|--------------------------|-----------------------------|-----------------------------|
| **Liides**                | Duplex SC               | RJ-45                      | RJ-45                      |
| **Edastuskeskkond**       | Optiline kiud           | Keerdpaar UTP Cat5 (5e)    | Keerdpaar UTP Cat3, 4, 5   |
| **Signaali Skeem**        | 4B/5B                   | 4B/5B                      | 8B/6T                      |
| **Kodeerimine**           | NRZI                    | MLT-3                      | 8B/6T                      |
| **Keerdpaare/kiude**      | 2 optilist kiudu        | 2 keerdpaari               | 4 keerdpaari               |
| **Maksimaalne Pikkus**    | Kuni 2 km (MMF), 10 km (SMF) | Kuni 100 m               | Kuni 100 m                |

---

### Režiimide Võrdlus

| **Režiim**                | **Kaabli Kategooria**    | **Keerdpaaride Arv**        |
|---------------------------|--------------------------|-----------------------------|
| 10Base-T                 | Cat3                    | 2                           |
| 10Base-T (full-duplex)   | Cat3                    | 2                           |
| 100Base-TX               | Cat5                    | 2                           |
| 100Base-TX (full-duplex) | Cat5                    | 2                           |
| 100Base-T4               | Cat3                    | 4                           |

---

### Auto-Negotiation

**Auto-Negotiation** on tehnoloogia, mis võimaldab võrguseadmetel automaatselt valida parima töörežiimi (kiirus ja duplex-režiim).

- **Funktsioon:** Võimaldab seadmetel vahetada teavet oma võimekuste kohta.  
- **Tehnoloogia:** Kasutatakse **FastLinkPulse (FLP)** signaale.  
- **Tähtsus:** Minimeerib konfiguratsioonivigu ja optimeerib võrguühendust, võimaldades seadmetel koos tõrgeteta töötada.  

---

### Täisdupleks Režiim

Võrgusõlmed, mis toetavad 100BASE-FX ja 100BASE-TX spetsifikatsioone, võivad töötada **täisdupleks režiimis** (*full-duplex mode*). Selles režiimis ei kasutata CSMA/CD meetodit ning puudub kokkupõrgete mõiste. Iga võrgusõlm saab samaaegselt andmeid edastada ja vastu võtta läbi eraldi kanalite **Tₓ** ja **Rₓ**.

- **Kasutuspiirang**: Täisdupleks režiimi saab kasutada ainult siis, kui võrguadapter on ühendatud kommutaatoriga või otse kommutaatorite vahel.
- **Kiirus**: Täisdupleks ühenduses tagab standard 100Base-FX andmeedastuskiiruse kuni **200 Mb/s**.
- **Kokkupõrke vältimine**: Silmuseid (loop) ei ole lubatud.

### Gigabit Ethernet IEEE 802.3z (1998) ja IEEE 802.3ab (1999)

Gigabit Ethernet tõi kaasa märkimisväärse edasimineku andmeedastuse kiirustes. **802.3z (1998)** spetsifikatsioon tutvustas varjestatud keerdpaari (STP) kaableid, mille laineimpedants oli 150 oomi. Maksimaalne segmendi pikkus oli vaid **25 meetrit**, mis sobis peamiselt lühikeste ühenduste jaoks samas ruumis.

**802.3ab (1999)** spetsifikatsioon laiendas Gigabit Etherneti kasutusvõimalusi, tutvustades uut kaabeldustüüpi, sealhulgas UTP (Unshielded Twisted Pair), mis võimaldas ulatuslikumat kasutust nii kontori- kui ka suuremates võrkudes.

### Cabling Specifications

| **GE Type**      | **Wiring Type**                                   | **Pairs** | **Cable Length** |
|-------------------|--------------------------------------------------|-----------|-------------------|
| 1000BASE-CX      | Shielded twisted pair (STP)                      | 1         | 25 m             |
| 1000BASE-T       | EIA/TIA Category 5 UTP                           | 4         | 100 m            |
| 1000BASE-SX      | Multimode fiber (MMF) with 62.5-micron core, 850-nm laser | 1       | 275 m            |
|                   | MMF with 50-micron core, 850-nm laser            | 1         | 550 m            |
| 1000BASE-LX/LH   | MMF with 62.5-micron core, 1300-nm laser          | 1         | 550 m            |
|                   | MMF with 50-micron core, 1300-nm laser           | 1         | 550 m            |
|                   | SMF with 9-micron core, 1300-nm laser            | 1         | 10 km            |
| 1000BASE-ZX      | SMF with 9-micron core, 1550-nm laser             | 1         | 70 km            |
|                   | SMF with 8-micron core, 1550-nm laser            | 1         | 100 km           |


[Gigabit Ethernet - Wikipedia](https://en.wikipedia.org/wiki/Gigabit_Ethernet)

[1000BASE-T: Gigabit Ethernet over category 5 copper cabling](http://pdf.cloud.opensystemsmedia.com/embedded-computing.com/CDT.Fall00.pdf)

### Gigabit Ethernet IEEE 802.3ab (1999)

Gigabit Ethernet IEEE 802.3ab standard tõi kaasa märkimisväärseid uuendusi kaabeldusvõimalustes ja andmeedastuse efektiivsuses, muutes võrgud kiiremaks ja usaldusväärsemaks. 

Kategooria 5 kaabel suudab pakkuda sagedusriba kuni **100 MHz**, võimaldades selle kaudu edastada andmeid kiirusega **1000 Mbit/s**. Selle saavutamiseks jaotatakse andmevoog **neljaks paralleelseks andmesuunaks**, igaüks kiirusega **250 Mbit/s**. Kõik neli paari töötavad samaaegselt, et saavutada vajalik Gigabit Etherneti kiirus.

---

## Täisdupleks Režiim

Võrgusõlmed, mis toetavad **100BASE-FX** ja **100BASE-TX** spetsifikatsioone, võivad töötada **täisdupleks režiimis** (*full-duplex mode*). Selles režiimis:

- **CSMA/CD meetodit ei kasutata** ning kokkupõrkeid ei esine.  
- Iga võrgusõlm saab samaaegselt andmeid edastada ja vastu võtta läbi eraldi kanalite **Tₓ** ja **Rₓ**.

### Täisdupleksi Eelised

- **Kiirus**: Täisdupleks režiim võimaldab **100Base-FX** ühendusel saavutada andmeedastuskiirust kuni **200 Mb/s** (100 Mb/s edastuseks ja 100 Mb/s vastuvõtuks).  
- **Kasutuspiirangud**: Täisdupleksi saab kasutada ainult siis, kui võrguadapter on ühendatud kas:
  - Kommutaatoriga (*switch*)  
  - Otse kahe kommutaatori vahel.  
- **Kokkupõrke vältimine**: Võrgus ei tohi olla silmuseid (*loop*), kuna need põhjustavad liikluse ummistumist.

---

## Gigabit Ethernet IEEE 802.3z (1998) ja IEEE 802.3ab (1999)

Gigabit Ethernet tõi kaasa märkimisväärse edasimineku andmeedastuse kiirustes.  

### IEEE 802.3z (1998)
- Esmakordselt tutvustati **varjestatud keerdpaari (STP)** kaableid, mille laineimpedants oli 150 oomi.  
- Maksimaalne segmendi pikkus oli vaid **25 meetrit**, sobides peamiselt lühikesteks ühendusteks samas ruumis.  

### IEEE 802.3ab (1999)
- Laiendas Gigabit Etherneti kasutusvõimalusi, lisades tugi **UTP (Unshielded Twisted Pair)** kaablitele, võimaldades suuremat paindlikkust nii kontori- kui ka suuremates võrkudes.  
- Maksimaalne kaabli pikkus ulatus **100 meetrini**.  

### Gigabit Etherneti Kaabeldusspetsifikatsioonid

| **GE Tüüp**      | **Kaablitüüp**                                       | **Paarid** | **Kaabli Pikkus** |
|-------------------|-----------------------------------------------------|------------|--------------------|
| 1000BASE-CX      | Varjestatud keerdpaar (STP)                          | 1          | 25 m              |
| 1000BASE-T       | EIA/TIA Cat5 UTP                                     | 4          | 100 m             |
| 1000BASE-SX      | Multimode Fiber (MMF), 62,5-mikron tuum, 850-nm laser | 1          | 275 m             |
|                  | MMF, 50-mikron tuum, 850-nm laser                    | 1          | 550 m             |
| 1000BASE-LX/LH   | MMF, 62,5-mikron tuum, 1300-nm laser                 | 1          | 550 m             |
|                  | SMF, 9-mikron tuum, 1300-nm laser                    | 1          | 10 km             |
| 1000BASE-ZX      | SMF, 8- või 9-mikron tuum, 1550-nm laser             | 1          | 70–100 km         |

---

## Gigabit Ethernet Automaatläbirääkimised ja Auto-MDI(X)

Gigabit Ethernet laiendas **automaatläbirääkimiste** võimalusi, toetades täis- ja pooldupleksrežiime kiirusel kuni **1000 Mbit/s**.  

### Auto-MDI(X)
- **Funktsioon**: Liides tuvastab automaatselt, kas ühendus vajab sirget või ristkaablit, ning töötab mõlemaga.  
- **Eelis**: Auto-MDI(X) tugi lihtsustab seadistamist, kuna ainult üks liides peab seda toetama.  
- **Standardid**: Sisaldub 1000Base-T ja uuemates standardites.

---

## 10G Ethernet IEEE 802.3ae (2003)

**10G Ethernet** on oluline samm kõrge jõudlusega võrguarhitektuurides, pakkudes erinevaid ühendusstandardeid, mis sobivad nii lühikesteks kui ka pika vahemaaga ühendusteks.

### 10G Ethernet Standardid

| **Ühendus**      | **Meedium**           | **Lainepikkus** | **Maksimaalne Kaugus**     |
|------------------|-----------------------|-----------------|----------------------------|
| 10GBASE-SR       | Multimode Fiber       | 850 nm          | OM3: 300 m, OM4: 400 m    |
| 10GBASE-LR       | Single-mode Fiber     | 1310 nm         | 10 km                     |
| 10GBASE-LRM      | Multimode Fiber       | 1310 nm         | 220 m                     |
| 10GBASE-ER       | Single-mode Fiber     | 1550 nm         | 40 km                     |
| 10GBASE-ZR       | Single-mode Fiber     | 1550 nm         | 80 km                     |

---

### 10G Ethernet 10GBASE-T IEEE 802.3an (2006)

**10GBASE-T** laiendas Etherneti kaablivõimalusi, kasutades keerdpaarikaableid. See standard võimaldab Etherneti jõudlust kuni 10 Gbps, pakkudes paindlikkust ja suhteliselt madalaid kaabelduskulusid.

| **Omadus**           | **100BASE-TX**       | **1000BASE-T**        | **10GBASE-T**            |
|----------------------|----------------------|-----------------------|--------------------------|
| Liini Kiirus         | 100 Mbps            | 1000 Mbps            | 10 Gbps                  |
| Modulatsioon         | PAM-3               | PAM-5                | PAM-16                  |
| Kasutatavad Paarid   | 2                   | 4                    | 4                        |
| Modulatsiooni Kiirus | 125 Mbaud           | 125 Mbaud            | 800 Mbaud               |
| Maksimaalne Segment  | 100 m               | 100 m                | 55 m (Cat6), 100 m (Cat6a) |

### Täiendav Selgitus

- **Modulatsioon**:  
  - PAM (*Pulse Amplitude Modulation*) võimaldab kodeerida rohkem andmeid ühe signaali kohta. Suurem PAM väärtus nõuab keerukamat signaalitöötlust.  
- **Kaabeldus**:  
  - Cat6 ja Cat6a kaablid on vajalikud 10GBASE-T jaoks, et vähendada **võõrristkaja (Alien Crosstalk)** mõju.  
- **Kodeerimine**:  
  - Täiustatud kodeeringute, nagu **LDPC (Low-Density Parity-Check)** ja **Tomlinson-Harashima eelkodeerimine**, kasutamine tagab usaldusväärse andmeedastuse suurematel kiirustel.

---

## 40-Gigabit Ethernet (40GbE) ja 100-Gigabit Ethernet (100GbE)

### Põhistandardid ja Rakendused

Allolev tabel võtab kokku 40GbE ja 100GbE peamised PHY (füüsilise kihi) standardid, keskendudes toetatud meediumitele, kaugustele ja kasutusjuhtudele. Need tehnoloogiad on laialdaselt kasutusel andmekeskustes ja suurte võrkude operaatorite juures.

| **Meedium / Kaugus**                 | **40-Gigabit Ethernet** | **100-Gigabit Ethernet** |
|--------------------------------------|-------------------------|--------------------------|
| Vähemalt 1 m üle tagaplaadi          | 40GBASE-KR4            | -                        |
| Vähemalt 10 m vasekaabliga           | 40GBASE-CR4            | 100GBASE-CR10            |
| Vähemalt 100 m OM3 mitmerežiimkiuga  | 40GBASE-SR4            | 100GBASE-SR10            |
| Vähemalt 125 m OM4 mitmerežiimkiuga  | 40GBASE-SR4            | 100GBASE-SR10            |
| Vähemalt 10 km ühe režiimiga kiuga   | 40GBASE-LR4            | 100GBASE-LR4             |
| Vähemalt 40 km ühe režiimiga kiuga   | 40GBASE-ER4            | 100GBASE-ER4             |

### Standardite Selgitus

1. **Tagaplaat (40GBASE-KR4)**:
   - Mõeldud lühikesteks ühendusteks (vähemalt 1 m) šassii sees, tavaliselt serveririiulites.  
2. **Vasekaabel (40GBASE-CR4, 100GBASE-CR10)**:
   - Sobib lühikesteks ühendusteks (kuni 10 m) andmekeskuste kaablitega.  
3. **OM3/OM4 Mitmerežiimkiud (40GBASE-SR4, 100GBASE-SR10)**:
   - Mitmerežiimkiud toetab keskmise pikkusega ühendusi. OM4 pakub OM3-ga võrreldes pikemat ulatust.  
4. **Üherežiimkiud (40GBASE-LR4, 40GBASE-ER4)**:
   - Üherežiimkiud sobib pikamaaühendusteks, näiteks andmekeskustevahelisteks ja metroovõrkudeks. LR4 ulatus on kuni 10 km, ER4 kuni 40 km.  

---

## 2.5GBASE-T ja 5GBASE-T Ethernet

### Vahekiirusega Etherneti Tutvustus

2014. aastal tutvustasid **NBASE-T (Cisco)** ja **MGBASE-T (Broadcom)** standardeid, mis võimaldavad andmeedastust kiirustel **2,5 Gbps** ja **5 Gbps**, täites tühimiku 1 Gbps ja 10 Gbps Etherneti vahel.

### Põhijooned

- **Tagasiühilduvus**: Ühildub olemasolevate Cat 5e ja Cat 6 kaablitega, mis vähendab uuenduskulusid.  
- **Kiirused**: Pakub vahekiirusi 2,5 Gbps ja 5 Gbps kuni 100 meetri kaugusel.  
- **Rakendused**:
  - Kõrge kiirusega Wi-Fi tugijaamad (802.11ac/ax).  
  - Kaasaegsed ettevõttevõrgud, kus 10 Gbps pole vajalik või liiga kallis.  

### Standardiseerimine

2016. aastal võeti need standardid vastu kui **IEEE 802.3bz**, pakkudes:  
- **2.5GBASE-T**: 2,5 Gbps Cat 5e või parema kaabliga.  
- **5GBASE-T**: 5 Gbps Cat 6 kaabliga.

---

## Power-over-Ethernet (PoE)

### Ülevaade

**Power-over-Ethernet (PoE)** võimaldab elektritoidet edastada samaaegselt andmetega üle standardsete Etherneti kaablite, kaotades vajaduse eraldi toitekaablite järele. PoE on populaarne IP-telefonide, traadita juurdepääsupunktide, IP-kaamerate ja teiste seadmete juures.

### Põhistandardid

| **Standard**      | **Maksimaalne Võimsus (Pordi Kohta)** | **Rakendused**                        |
|--------------------|---------------------------------------|---------------------------------------|
| IEEE 802.3af      | 15,4 W                               | IP-telefonid, lihtsad Wi-Fi tugijaamad |
| IEEE 802.3at (PoE+)| 25,5 W                               | Edasijõudnud IP-kaamerad, kaksikribaga Wi-Fi |

### Eelised

- **Lihtsustatud paigaldus**: Ei ole vaja eraldi toitekaableid.  
- **Tsentraliseeritud haldus**: Toetab UPS-seadmeid kriitiliste seadmete jaoks.  
- **Kuluefektiivsus**: Kasutab olemasolevat Etherneti kaabeldust.  

---

### PoE Pinout ja Energiaklassid

| **Pordiklemmid**   | **10/100 DC (Meetod B)** | **10/100 Segatud DC & Andmed (Meetod A)** | **1 Gbps Biandmed & DC (Meetod A)** |
|--------------------|--------------------------|------------------------------------------|-------------------------------------|
| Klemm 1           | Rx +                    | Rx + DC+                                | TxRx A + DC+                      |
| Klemm 2           | Rx -                    | Rx - DC+                                | TxRx A - DC+                      |
| Klemm 3           | Tx +                    | Tx + DC-                                | TxRx B + DC+                      |
| Klemm 4           | DC +                    | Mitte kasutusel                         | TxRx C                            |
| Klemm 5           | DC +                    | Mitte kasutusel                         | TxRx C                            |
| Klemm 6           | Tx -                    | Tx - DC-                                | TxRx B - DC+                      |
| Klemm 7           | DC -                    | Mitte kasutusel                         | TxRx D                            |
| Klemm 8           | DC -                    | Mitte kasutusel                         | TxRx D                            |

| **Klass** | **Maksimaalne Võimsus Seadmes (W)** | **Rakendused**                      |
|-----------|-------------------------------------|-------------------------------------|
| Klass 0   | 0,44–12,95                         | Madala võimsusega üldseadmed       |
| Klass 1   | 0,44–3,84                          | VoIP telefonid, andurid            |
| Klass 2   | 3,84–6,49                          | Lihtsad IP-kaamerad                |
| Klass 3   | 6,49–12,95                         | Wi-Fi tugijaamad                   |
| Klass 4   | 12,95–25,5                         | Edasijõudnud IP-kaamerad, Wi-Fi    |

---

### PoE Tööpõhimõte

1. **Toiteallikas (PSE)** edastab toidet Etherneti kaabli kaudu.  
2. **Toideseade (PD)** tuvastab ja lepib kokku vajaliku toitevõimsuse.  
3. Andmed ja elekter liiguvad samaaegselt, ilma et see mõjutaks jõudlust.  

![PoE Süsteem](https://kintronics.com/wp-content/uploads/2016/05/poe-explained.png)

[PoE Splitter Image](http://www.fiber-optic-solutions.com/wp-content/uploads/2017/02/PoE-splitter.jpg)

---

## Võrdlus: PoE IEEE 802.3af vs PoEPlus IEEE 802.3at

### Peamised Eriomadused

| **Kategooria**                 | **PoE IEEE 802.3af**                                       | **PoEPlus IEEE 802.3at**                                   |
|--------------------------------|-----------------------------------------------------------|-----------------------------------------------------------|
| **Kaabli Nõuded**              | Kategooria 3 (UTP CAT3) või parem                         | Type 1: Kategooria 3 (UTP CAT3) või parem<br>Type 2: Kategooria 5 (UTP CAT5) või parem |
| **Voolutugevus**               | 0.35 A                                                   | Type 1: 0.35 A<br>Type 2: 0.6 A                          |
| **Injektori Väljundpinge**     | 44–57 V                                                  | Type 1: 44–57 V<br>Type 2: 50–57 V                       |
| **Toiteseadme Sisendpinge**    | 37–57 V                                                  | Type 1: 37–57 V<br>Type 2: 42.5–57 V                     |
| **Maksimaalne Energiatarve**   | Klass 0, 3: 12.95 W<br>Klass 1: 3.84 W<br>Klass 2: 6.49 W | Type 1: Samad väärtused kui 802.3af<br>Type 2: Klass 4: 25.5 W |
| **Toetatavad Seadmed**         | IP-kaamerad, IP-telefonid, traadita pääsupunktid          | Kõik PoE seadmed, välitingimustes PTZ-kaamerad, WiMAXi tugipunktid, LED-ekraanid, mõned arvutid |

---

### Tabeli Tähtsamad Punktid

1. **Kaabli nõuded**:
   - **PoE 802.3af** töötab kategooria 3 (CAT3) kaablitega.
   - **PoEPlus 802.3at** nõuab kõrgema võimsusega seadmetele kategooria 5 (CAT5) kaableid.

2. **Voolutugevus ja pinge**:
   - PoEPlus (802.3at) toetab suuremat voolutugevust ja kõrgemat pinget, et võimaldada suurema energiavajadusega seadmete toetamist.

3. **Maksimaalne energiatarve**:
   - **PoE (802.3af)** piirab tarbimist 12.95 W.  
   - **PoEPlus (802.3at)** toetab kuni 25.5 W, pakkudes tuge võimsamate seadmete jaoks nagu välitingimustes kasutatavad kaamerad ja LED-ekraanid.

4. **Toetatavad seadmed**:
   - PoE sobib väiksema energiatarbega seadmetele, näiteks IP-telefonidele ja kaameratele.
   - PoEPlus võimaldab toita võimsamaid seadmeid, sealhulgas PTZ-kaameraid ja teatud tüüpi arvuteid.

---

## Passiivne PoE

### Olulised Punktid:

1. **Standardse PoE (802.3af) puudused**:
   - PoE funktsionaalsusega seadmete kõrgem lisakulu.  
   - PoE kommutaatorite suurem energiatarve võrreldes tavaliste kommutaatoritega.

2. **Passiivse PoE kasutuselevõtt**:
   - Alternatiivsed lahendused, mida tuntakse kui **Passiivne PoE**.  
   - Passiivne PoE koosneb adapterikomplektist, mis toetab ainult elektrilisi omadusi (mitte protokollipõhiseid), mis vastavad standardile 802.3af.  

3. **Kuidas Passiivne PoE töötab**:
   - Passiivse PoE injektor edastab ükskõik millist pinget, mida toiteplokk pakub (ei pea olema tingimata 48V).  
   - See ei rakenda 802.3af protokollistandardeid, mistõttu on see **IEEE 802.3af standardiga täielikult ühildumatu**.

### Kasutusala ja tähelepanekud:
Passiivne PoE on lihtne ja kuluefektiivne lahendus madala energiatarbega seadmetele, mis ei vaja täielikku vastavust PoE standarditele, kuid vajavad toite edastamist Ethernet-kaabli kaudu.

## Passiivse PoE Võrguskeem

Allolev diagramm illustreerib, kuidas **Passiivne PoE** töötab. See näitab Passiivse PoE injektori ja jaoturi kasutamist seadmete, näiteks IP-kaamerate või juurdepääsupunktide toiteks.

![Passiivse PoE injektori ja jaoturi võrguskeem](http://www.cctvcamerapros.com/v/vspfiles/assets/images/passive-poe-injector-splitter-network-diagram.jpg)

### Diagrammi Komponendid:
1. **Passiivne PoE Injektor**: Kombineerib elektritoite ja andmed ühele Ethernet-kaablile.  
2. **Ethernet-kaabel**: Edastab samaaegselt nii andmed kui ka elektri, vähendades eraldi toitekaablite vajadust.  
3. **Passiivne PoE Jaotur (Splitter)**: Eraldab toite ja andmed seadmete jaoks, mis ei toeta natiivset PoE-d.  
4. **Toiteallikaga Seade (PD)**: Seade (nt IP-kaamera või juurdepääsupunkt), mis vajab andmeid ja elektrivoolu.

### Olulised Märkused:
- Passiivne PoE ei vasta IEEE 802.3af/at standarditele.  
- Injektor edastab fikseeritud pinget (tavaliselt toiteadapterist).  
- Sobib madalama maksumuse ja energiatarbega seadmetele, kus täielik PoE ühilduvus ei ole vajalik.
