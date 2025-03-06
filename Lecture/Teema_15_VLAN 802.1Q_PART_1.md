# Teema 15: Mis on VLAN 802.1Q ? Part 1

## Sissejuhatus VLAN-idesse

[![Divide VLAN based on ports](https://www.fibermall.com/blog/wp-content/uploads/2023/01/VLAN-based-on-ports-e1673424252499.png)](https://www.fibermall.com/blog/wp-content/uploads/2023/01/VLAN-based-on-ports-e1673424252499.png)

Divide VLAN based on ports

Source: [FiberMall](https://www.fibermall.com)


VLAN (Virtual Local Area Network ehk virtuaalne kohtvõrk) on tehnoloogia, mis võimaldab võrguadministraatoritel luua füüsilise kommutaatori siseselt loogilisi jaotusi. See loob efekti, nagu kasutataks mitut eraldiseisvat kommutaatorit, kuigi tegelikult kasutatakse ainult ühte füüsilist seadet.

**Peamised põhjused VLAN-ide kasutamiseks:**

1. **Haldamise mugavus** - Seadmete loogiline organiseerimine sõltumata nende füüsilisest asukohast
2. **Turvalisus** - Tundlike võrkude eraldamine (nt raamatupidamisosakond) teistest võrkudest
3. **Leviedastusliikluse vähendamine** - Väiksemad leviedastusdomäänid vähendavad ebavajalikku võrguliiklust

Näiteks selle asemel, et vajada eraldi füüsilisi kommutaatoreid raamatupidamis-, turundus- ja IT-osakondade jaoks samas kontoris, võimaldavad VLAN-id kasutada ühte kommutaatorit, kuid liiklus on täielikult eraldatud, justkui oleksid seadmed ühendatud erinevate füüsiliste kommutaatoritega.

[![Divide VLANs based on network layer protocols](https://www.fibermall.com/blog/wp-content/uploads/2023/01/Divide-VLANs-based-on-MAC-address.png)](https://www.fibermall.com/blog/wp-content/uploads/2023/01/Divide-VLANs-based-on-MAC-address.png)

Divide VLANs based on network layer protocols

Source: [FiberMall](https://www.fibermall.com)


## Pordipõhised VLAN-id

Varaseimad VLAN-i implementatsioonid põhinesid portidel. Selles lähenemises:

- Kommutaatori administraator määrab iga füüsilise pordi konkreetsele VLAN-ile
- Kaadrid saab edastada ainult portide vahel, mis kuuluvad samasse VLAN-i
- Leviedastuskaadrid saadetakse ainult sama VLAN-i portidele

See loob loogilise eraldatuse, kus eri VLAN-idesse ühendatud seadmed ei saa omavahel otse suhelda, kuigi nad on ühendatud sama füüsilise kommutaatoriga.

Pordipõhises konfiguratsioonis, kui pordid 1-2 on VLAN 1-s, pordid 3-4 on VLAN 2-s ja pordid 5-8 on VLAN 3-s, siis:
- Leviedastus pordist 1 jõuab ainult pordini 2
- Leviedastus pordist 3 jõuab ainult pordini 4
- Leviedastus pordist 5 jõuab portideni 6, 7 ja 8

See vähendab oluliselt leviedastusliiklust. Näiteks 24 pordiga ja kahe arvutiga, millest kumbki saadab 10 leviedastust:
- Ilma VLAN-ideta: 24 porti × 20 leviedastust = 480 kaadrit
- VLAN-idega (jagatuna kaheks 12-pordiseks VLAN-iks): (12 porti × 10 leviedastust) × 2 = 240 kaadrit

[![Understanding VLANs and Network Layers](https://images.squarespace-cdn.com/content/v1/5d3c73c906ac510001158d1b/1618528223782-MLF9Q4FKTBOQPWQKR93O/Understanding+VLANs+and+Network+Layers.png?format=2500w)](https://images.squarespace-cdn.com/content/v1/5d3c73c906ac510001158d1b/1618528223782-MLF9Q4FKTBOQPWQKR93O/Understanding+VLANs+and+Network+Layers.png?format=2500w)

Understanding VLANs and Network Layers

Source: [Squarespace](https://squarespace.com)


## ROAS (Router on a Stick)

Kujutame ette, et meil on marsruuter, millel on kaks liidest:
- Esimene liides: 10.10.10.1/24
- Teine liides: 10.10.20.1/24

Traditsioonilises konfiguratsioonis on need liidesed ühendatud eraldi võrkudesse, kuid meie näites on need ühendatud ühte kommutaatorisse.

Mõlemad IP-aadressid töötavad ühel füüsilisel liidesel. Selle tulemusel on kommutaatorisse ühendatud kasutajad, kuid L3 vaatepunktist näeb see välja nii, nagu marsruuter oleks ühendatud kahte eraldi võrku:
- Võrk 1: 192.168.0.0/24
- Võrk 2: 10.10.0.0/24

## Ühenduse loogika

Põhiprintsiip on lihtne - kaks eraldi võrku töötavad ühe füüsilise liidese kaudu. See tähendab, et marsruuteri üks füüsiline port suudab teenindada mitu loogilist võrku.

## Alternatiivsed lahendused

On kaks põhilist alternatiivset lahendust:

1. **Eraldi füüsilised liidesed:** Võime luua teise liidese vastavate aadressidega ja ühendada need kommutaatoriga.

2. **Kommutaatori loogiline jagamine:** Võime jagada kommutaatori loogiliselt osadeks, kus iga osa on täielikult eraldatud teistest.

## VLAN-ide roll

[Presentation Slide Deck on Native VLAN](https://slideplayer.com/slide/13554705/)


Kuigi see näide pole ideaalne, illustreerib see ideed, kuidas üht füüsilist seadet (kommutaatorit) saab jagada mitmeks loogiliseks üksuseks, mis töötavad täiesti eraldi, justkui oleks tegemist mitme eraldi kommutaatoriga.

## Pordipõhiste VLAN-ide jätkame

## Loogika ja toimimine

Kui rakendame sellist lahendust, siis tegelikult näeb see välja nagu kolm kommutaatorit, kuid loogiliselt on see üks kommutaator. Pordipõhised VLAN-id kannavad seda nimetust, sest me toetume pordipõhisele loogikale. Me ütleme, et meie pordid (teatud numbritega) kuuluvad ühte loogilisse gruppi, kus igaühel neist on oma MAC-tabel.

Kaadrid ühest segmendist ei lahku sellest segmendist, isegi mitte leviedastuskaadrid (broadcast). See tähendab, et leviedastus ühest segmendist ei jõua teise segmenti. Need segmendid ei saa omavahel mingil moel suhelda. See on väga kasulik funktsioon.

## Leviedastusliikluse vähenemine

[![Multiswitch broadcast domains](https://www.networkacademy.io/sites/default/files/inline-images/LAN%20on%20multiple%20switches.gif)](https://www.networkacademy.io/sites/default/files/inline-images/LAN%20on%20multiple%20switches.gif)

Having that in mind, if we connect two default setting switches, as shown in figure 1, any broadcast frame received by either switch is forwarded to the other one and then out all its ports.

Source: [Network Academy](https://www.networkacademy.io)

Siin on struktureeritud ja teksti lisatud versioon, mis selgitab arvutusi ja VLAN-i kasutamise eeliseid:

---

### Leviedastuste arvutamise näide

Vaatleme olukorda, kus meil on 24-pordiga kommutaator ja kaks arvutit, mis saadavad kumbki 10 leviedastust. Ühes võrgus tähendab see, et igas pordis liigub kokku 20 leviedastust, mis annab 24 porti korral 480 kaadrit. Kui aga jagame võrgu kaheks loogiliseks osaks (näiteks VLAN-ide abil), jaguneb 24 porti kaheks 12-pordi segmentiks. Iga segment on iseseisev ja ühes segmentis saadab üks arvuti 10 leviedastust, mis annab 12 porti korral 120 kaadrit. Kogu võrgu liiklus jagatud segmentides on seega 120 + 120 = 240 kaadrit – kaks korda vähem kui algses ühes võrgus.

| Kirjeldus                                | Pordide arv | Leviedastused (arvuti kohta) | Arvutatud kaadrite arv         |
|------------------------------------------|-------------|------------------------------|--------------------------------|
| **Ühes võrgus (24 porti)**               | 24          | 2 × 10 = 20                  | 24 × 20 = **480 kaadrit**       |
| **Jagatud võrk (2 segmenti, 12 porti igaüks)** | 12 (iga segment) | 1 arvuti × 10 = 10          | 12 × 10 = **120 kaadrit** (iga segment) |
| **Kokku jagatud võrgus**                 | 24          | –                            | 120 + 120 = **240 kaadrit**     |

---

### VLAN-i kasutamise eelised

VLAN-i kasutamine võimaldab võrke loogiliselt jaotada, mis toob kaasa mitmeid eeliseid:

| Eelis                         | Selgitus                                                                 |
|-------------------------------|--------------------------------------------------------------------------|
| **1. Mugav võrkude jaotamine** | Võimaldab lihtsasti jagada füüsilised seadmed loogilisteks segmentideks.  |
| **2. Leviedastusliikluse vähendamine** | Väiksemad segmentid vähendavad leviedastuste arvu, mis parandab võrgu jõudlust. |
| **3. Suurem turvalisus**      | Loogiline eraldamine tagab parema turvakontrolli ja piirab volitamata juurdepääsu. |

---

Kokkuvõttes näitab see näide, kuidas väiksemateks segmentideks jagades saab märkimisväärselt vähendada leviedastusliiklust ning pakkuda samal ajal paremat võrgu struktuuri ja turvalisust. VLAN-id võimaldavad seega võrguressursside paremat juhtimist ja optimeerimist, mis võib oluliselt kaasa aidata üldise võrgutõhususe parandamisele.


# Kuidas ühendada kommutaatoreid? Trunk-kontseptsioon

Järgnev diagramm illustreerib VLAN-ide liikumist trunk-pordi kaudu kommutaatorite vahel ja 802.1Q märgistuse töötamist:
```mermaid
graph TD
    subgraph "Kommutaator A"
        A_PC1[PC1] --> A_P1[Port 1\nAccess VLAN10]
        A_PC2[PC2] --> A_P2[Port 2\nAccess VLAN20]
        A_PC3[PC3] --> A_P3[Port 3\nAccess VLAN30]
        A_P1 --> A_Internal[Sisemine töötlus\nKõik kaadrid märgistatud]
        A_P2 --> A_Internal
        A_P3 --> A_Internal
        A_Internal --> A_Trunk[Port 24\nTrunk]
    end
    
    subgraph "Kommutaator B"
        B_PC1[PC4] --> B_P1[Port 1\nAccess VLAN10]
        B_PC2[PC5] --> B_P2[Port 2\nAccess VLAN20]
        B_PC3[PC6] --> B_P3[Port 3\nAccess VLAN30]
        B_P1 --> B_Internal[Sisemine töötlus\nKõik kaadrid märgistatud]
        B_P2 --> B_Internal
        B_P3 --> B_Internal
        B_Internal --> B_Trunk[Port 24\nTrunk]
    end
    
    A_Trunk <--> B_Trunk
    
    subgraph "802.1Q Kaader"
        Frame["MAC siht | MAC lähte | 802.1Q märgistus | Tüüp | Andmed | CRC"]
        Tag["TPID\n(0x8100) | Prioriteet\n(PCP) | VLAN ID\n(12 bitti)"]
        Frame -.- Tag
    end
    
    style A_PC1 fill:#f9f,stroke:#333
    style A_PC2 fill:#bbf,stroke:#333
    style A_PC3 fill:#bfb,stroke:#333
    style B_PC1 fill:#f9f,stroke:#333
    style B_PC2 fill:#bbf,stroke:#333
    style B_PC3 fill:#bfb,stroke:#333
    style A_Trunk fill:#ff9,stroke:#333,stroke-width:2px
    style B_Trunk fill:#ff9,stroke:#333,stroke-width:2px
    style Frame fill:#ddf,stroke:#333
    style Tag fill:#fdd,stroke:#333
```
## Kommutaatorite ühendamise probleem

Meie näites on kaks gruppi (VLAN 1 ja VLAN 2). Järelikult peame eraldama iga grupi jaoks eraldi pordi ja ühendama kommutaatorid nende portidega. Siis saab grupp 1 suhelda teise kommutaatori grupiga 1 ja grupp 2 saab suhelda teise kommutaatori grupiga 2.

Seega, kui meil on VLAN 1, ühendame selle ühe pordiga, ja VLAN 2 ühendame teise pordiga. Kuid siit tekib küsimus - mis juhtub, kui meil on 24 VLAN-i? On selge, et sellise kontseptsiooniga me kaugele ei jõua.

Alguses nii see oligi - kuni VLAN-e oli kaks või kolm, sai niimoodi töötada. Kuid kui nende arv suureneb, ei saa te sel viisil kommutaatoreid ühendada.

[![VLAN on multiple switches](https://www.networkacademy.io/sites/default/files/inline-images/VLANs%20on%20multiple%20switches%20without%20trunk.gif)](https://www.networkacademy.io/sites/default/files/inline-images/VLANs%20on%20multiple%20switches%20without%20trunk.gif)

Using VLANs, a switch topology can be segmented into multiple broadcast domains—such as assigning ports 1–4 to VLAN 10 and ports 5–9 to VLAN 20—but this design doesn’t scale well because each VLAN requires a dedicated physical link between switches.

Source: [Network Academy](https://www.networkacademy.io)



## Magistraalide kontseptsioon (Trunk)
```mermaid
graph TD
    subgraph "SWITCH_A"
        SA[SWITCH A]
    end
    
    subgraph "SWITCH_B"
        SB[SWITCH B]
    end
    
    SA -- "TRUNK" --- SB
    
    subgraph "VLAN_1A"
        style VLAN_1A fill:#f99,stroke:#333,stroke-width:1px
        VLAN1A[VLAN 1]
        PC1A[PC] --- VLAN1A
    end
    
    subgraph "VLAN_2A"
        style VLAN_2A fill:#99f,stroke:#333,stroke-width:1px
        VLAN2A[VLAN 2]
        PC2A[PC] --- VLAN2A
    end
    
    subgraph "VLAN_3A"
        style VLAN_3A fill:#9f9,stroke:#333,stroke-width:1px
        VLAN3A[VLAN 3]
        PC3A[PC] --- VLAN3A
    end
    
    subgraph "VLAN_1B"
        style VLAN_1B fill:#f99,stroke:#333,stroke-width:1px
        VLAN1B[VLAN 1]
        PC1B[PC] --- VLAN1B
    end
    
    subgraph "VLAN_2B"
        style VLAN_2B fill:#99f,stroke:#333,stroke-width:1px
        VLAN2B[VLAN 2]
        PC2B[PC] --- VLAN2B
    end
    
    subgraph "VLAN_3B"
        style VLAN_3B fill:#9f9,stroke:#333,stroke-width:1px
        VLAN3B[VLAN 3]
        PC3B[PC] --- VLAN3B
    end
    
    VLAN1A --- SA
    VLAN2A --- SA
    VLAN3A --- SA
    
    VLAN1B --- SB
    VLAN2B --- SB
    VLAN3B --- SB
    
    style SA fill:#9cf,stroke:#333,stroke-width:2px
    style SB fill:#9cf,stroke:#333,stroke-width:2px
```
Sõna "trunk" võis teile varasemalt tuttav olla raadiosidest või mujalt. Sealt on näha, et tegemist on sidespetsialistide terminiga. Idee on sarnane - kasutada üht ühendust (link) kommutaatorite vahel paljude erinevate VLAN-ide edastamiseks.

Niipea kui hakkame neid ühes ühenduses edastama, tekib küsimus: kuidas teha nii, et need ei segaks üksteist? Kui edastate kaadri lihtsalt ühes ühenduses, pole teisel pool selge, millisesse gruppi see kuulub.

[![VLAN Trunking](https://www.networkacademy.io/sites/default/files/inline-images/VLANs%20on%20multiple%20switches_0.gif)](https://www.networkacademy.io/sites/default/files/inline-images/VLANs%20on%20multiple%20switches_0.gif)

The link between switch 1 and switch 2 is a trunk link and you can see that both VLAN 10 and VLAN 20 pass through the link.

Source: [Network Academy](https://www.networkacademy.io)

## 802.1Q standard (Trunking protocols)

802.1Q kontseptsioon - see on tegelikult üldnimetuseks muutunud (inglise keeles kõlab see sageli kui "dot1Q" ehk ".1Q"). Sidesüsteemide spetsialistid räägivad sageli just sel viisil. Võite öelda, et "mis port see on? Andke meile dot1Q port" - nii öeldaksegi ja mõeldakse sellega trunk-porti.

Mõnikord juhtub seadmete seadistamisel, kui valite kapseldusrežiimi, et seal on kirjas "802.1Q". See viitab alati trunk-portidele, ka mõõteseadmetes. Seetõttu peab mõiste "802.1Q" olema tihedalt seotud sõnaga "trunk".

## Kuidas see töötab?

[![Ethernet frame tagged with 802.1q header](https://www.networkacademy.io/sites/default/files/inline-images/802.1q%20header.png)](https://www.networkacademy.io/sites/default/files/inline-images/802.1q%20header.png)

Ethernet frame tagged with 802.1q header

Source: [Network Academy](https://www.networkacademy.io)


Tuleme selle juurde, kuidas see kõik tegelikult töötab. Otsustati standardiseerida ja öeldi: "Lisame siia, meie MAC-aadresside ja tüübivälja (Type field) vahele paketi Ethernet-kaadris ühe lisavälja."

Meie kaks päist - esimene on MAC ja teine on tüüp. Tüübis on tavaliselt kirjas 0x0800, mis tähendab, et seal asub IP-pakett.

Otsustati lisada veel üks väli, mida nimetatakse Tag'iks (sildiks või märgiseks). Millest see koosneb?
- Tag Protocol Identifier (TPID) - protokolli identifikaator
- Tag Control Information (TCI) - kus kõige olulisem on VLAN ID bittide väli, milleks on eraldatud 12 bitti

Siin edastataksegi meie VLAN-i number. Kui neid on 12 bitti, siis VLAN-id nummerdatakse 1 kuni 4096.

[![Frame formats for 802.1q, 802.1ad (PB), 802.1ah (PBB)](https://www.researchgate.net/profile/Rute-Sofia/publication/224392603/figure/fig7/AS:668490671325192@1536392004611/Frame-formats-for-8021q-8021ad-PB-8021ah-PBB.png)](https://www.researchgate.net/profile/Rute-Sofia/publication/224392603/figure/fig7/AS:668490671325192@1536392004611/Frame-formats-for-8021q-8021ad-PB-8021ah-PBB.png)

Frame formats for 802.1q, 802.1ad (PB), 802.1ah (PBB).

Source: [ResearchGate](https://www.researchgate.net)

Samuti on 802.1Q märgistuses prioriteedibidid (PCP), mis näitavad, et kui see on mingi kriitilise tähtsusega liiklus (tavaliselt hääl ja telefoniside), siis tuleks seda edastada esimeses järjekorras. See on sisuliselt Quality of Service'i (QoS) abimehhanism. Me seda loengutes pikemalt ei käsitle, kuid teadke, et need bitid võimaldavad liiklust prioritiseerida isegi kommutaatorites.

## Kuidas märgistus töötab?

[![Example of VLANs tagging](https://www.networkacademy.io/sites/default/files/inline-images/VLAN-Tagging.gif)](https://www.networkacademy.io/sites/default/files/inline-images/VLAN-Tagging.gif)

Example of VLANs tagging

Source: [Network Academy](https://www.networkacademy.io)


Vaadake, TPI ja TCI väljad paiknevad täpselt samas kohas, kus tavaliselt asub Type väli. Need praktiliselt dubleerivad seda. Siin on TPID, mis ütleb, et "jah, see on märgistatud kaader" ja et peale seda välja tuleb konkreetne VLAN number, mitte mõni protokoll.

Põhimõtteliselt asendatakse Type väli spetsiaalse väljaga, mis ütleb: "Vaata, edasi ma annan sulle veel VLAN numbri ja alles seejärel algab tavapärane kaader." Muide, siin on kirjas 0x8100, mis tähistab märgistatud Ethernet kaadrit.

Sellise kaadritüübi saamisel mõistab kommutaator, et seal on VLAN märgistus, ja saab lugeda selle numbri, et teada, millisesse gruppi või millisesse VLAN numbrisse antud kaader kuulub.

## 802.1Q märgistuse komponendid

| Komponent | Ingliskeelne nimetus | Kirjeldus |
|-----------|----------------------|-----------|
| **TPID** | Tag Protocol Identifier | Märgistamise protokolli identifikaator.<br>802.1q puhul kasutatakse väärtust 0x8100. |
| **TCI** | Tag Control Information | Väli, mis kapseldab prioriteedi, kanoonilise formaadi ja VLAN identifikaatori välju. |

## TCI välja komponendid

| Komponent | Ingliskeelne nimetus | Kirjeldus |
|-----------|----------------------|-----------|
| **Prioriteet** | Priority | Prioriteet. Kasutatakse IEEE 802.1p standardit edastatava liikluse prioriteedi määramiseks. |
| **CFI** | Canonical Format Indicator | Kanoonilise formaadi indikaator.<br>Näitab MAC-aadressi formaati:<br>0 — kanooniline (Ethernet kaader)<br>1 — mitte-kanooniline (Token Ring, FDDI kaader). |
| **VID** | VLAN Identifier | VLAN identifikaator.<br>Võimalike väärtuste vahemik on 0 kuni 4094. |

## Erinevused tootjate vahel

Erinevad tootjad nimetavad 802.1Q märgistust erinevalt:
- Cisco, Juniper, Huawei: nimetavad seda "trunk"
- D-Link, ZyXEL, HP: nimetavad seda "tagged" või "T-tagged"

Märgistamata porte kutsutakse Cisco, Juniper, Huawei terminoloogias "access" ja D-Link, ZyXEL, HP terminoloogias "untagged" või "untag".

Mõnikord juhtub, nagu HP kommutaatorite puhul, et üks on sarnane D-Link'ile ja teine Huawei'le. Seega olenevalt sellest, millega tegemist on.

Kui seadistate porte, siis lõppseadmetega ühendatud pordid seadistatakse kui "untag" või "access". See tähendab, et kommutaator võtab vastu ilma märgistuseta kaadri - teie seadmed ei saada märgistusega kaadrit.

[![Cisco SLM2024 vlan management](https://filedb.experts-exchange.com/incoming/2020/01_w03/1440769/vlan5.PNG)](https://filedb.experts-exchange.com/incoming/2020/01_w03/1440769/vlan5.PNG)

Cisco SLM2024 vlan management

Source: [Experts Exchange](https://www.experts-exchange.com)


Kui need kaadrid saabuvad kommutaatorisse, lisatakse neile märgistus ja need liiguvad edasi koos selle märgistusega kommutaatori sees. Kui nad jõuavad trunk-magistraali, liiguvad need kõik koos üksteise järel selles järjekorras, milles need sinna sattusid.

Kuna kaadrites on märgistus (tag), on võimalik need tagasi õigetesse pordigruppidesse suunata. Kui kaader väljub access-pordist, eemaldatakse märgistus, sest tavaline arvuti ei suuda märgistusega kaadrit lugeda. Seetõttu peab märgistuse eemaldama.

## Kommutaatori konfiguratsioon

Oluline on teada, et kommutaator ei võta niisama kõiki VLAN-e vastu. Tal on oma põhiandmebaas, ja kui loote VLAN-id 1, 2 ja 3, siis ta võtab vastu märgistusi 1, 2 ja 3. Kui te ei loo sellist kirjet, st kui te ei ütle talle, et ta peaks töötlema märgistusi numbritega 1, 2, 3, siis ta seda ei tee.

Kommutaatoritel võivad olla piirangud. Näiteks mõni "light" seeria võib öelda, et saate määrata suvalised VLAN-numbrid, kuid neid ei tohi olla rohkem kui 16. Need võivad olla ükskõik millised numbrid, näiteks 100, 200, 1050, kuid ainult 16 tükki - 17ndat ta teile lisada ei lase. See sõltub litsentseerimisest ja versioonidest.

## Vaikimisi VLAN (VLAN 1)

[![The Default VLAN – VLAN 1](https://www.flackbox.com/wp-content/uploads/2017/09/word-image-20.png)](https://www.flackbox.com/wp-content/uploads/2017/09/word-image-20.png)

The Default VLAN – VLAN 1

Source: [Flackbox](https://www.flackbox.com)


Pöörake tähelepanu, et VLAN 1 on karbist välja võetud kommutaatoril kõikides portides vaikimisi aktiveeritud, olenemata tootjast. See tähendab, et kõik pordid on access-režiimis esimeses VLAN-is. See on loodud vaikimisi, ja kui mingi kaader sinna saabub, elab see kommutaatori sees juba alati märgistusega.

Kogu seda infot on vaja seedida. Kommutaator töötab seesmiselt märgistustega. Kui võtate ta karbist välja, on kõik pordid VLAN 1-s. Teile tundub, et see on üks tervik, et kommutaator ja pordid pole seadistatud, et need on kõik omavahel ühenduses. See on õige, kuna vaikimisi seadistus on tavaliselt kõik pordid esimeses VLAN-is, kuid sisemiselt liiguvad need kõik märgistustega ja kõik on märgitud number ühega.

## ISL (Inter-Switch Link) - Cisco protokoll

IEEE 802.1Q ei ole ainus võimalus VLAN-ide kapseldamiseks. Cisco lõi omal ajal tehnoloogia nimega ISL (Inter-Switch Link), mis eksisteeris enne 802.1Q standardi tekkimist.

## Praktiline tähtsus konfigureerimisel

Kui te tulevikus seadistate oma laborites ja kodutöödes VLAN-e, peate sageli määrama kapselduse tüübi. Selleks on käsk `encapsulation dot1q`, mille järel võib tekkida küsimus: miks peab täpsustama, kuidas VLAN-e kapseldada, kui standard on juba olemas?

Põhjus seisneb selles, et Cisco-l oli oma ISL kapseldamise tehnoloogia. Kuigi tänapäeval seda enam laialdaselt ei kasutata, küsib seade pordi seadistamisel ikkagi teilt, mida kasutada - vana ISL-i või kaasaegsemat dot1Q-d. Peate seda iga kord täpsustama, sest ajalooliselt eksisteerisid erinevad variandid.

## Ajaloolised märkused

ISL oli Cisco proprietaarne protokoll, mis täitis sama funktsiooni mida tänapäeval täidab 802.1Q standard. Kuigi seda enam aktiivselt ei kasutata, on teadlikkus sellest oluline, eriti kui töötate vanemate Cisco seadmetega või kohtate viiteid sellele konfiguratsiooni dokumentides.

Tänapäeval on 802.1Q universaalne standard, mida toetavad kõik võrguseadmete tootjad, samas kui ISL jääb Cisco ajaloo osaks.

## Põhi-VLAN (Native VLAN) - Kommutaatori sisene töötlemine
```mermaid
graph TD
    subgraph "Kommutaator A"
        A_PC1[PC1<br>VLAN 10] --> A_P1[Access Port<br>VLAN 10]
        A_PC2[PC2<br>VLAN 20] --> A_P2[Access Port<br>VLAN 20]
        A_PC3[PC3<br>Native VLAN 1] --> A_P3[Access Port<br>VLAN 1]
        
        A_P1 --> A_Internal[Sisemine töötlus<br>Kõik kaadrid märgistatud]
        A_P2 --> A_Internal
        A_P3 --> A_Internal
        
        A_Internal --> A_Trunk[Trunk Port]
    end
    
    subgraph "Kommutaator B"
        B_PC1[PC4<br>VLAN 10] --> B_P1[Access Port<br>VLAN 10]
        B_PC2[PC5<br>VLAN 20] --> B_P2[Access Port<br>VLAN 20]
        B_PC3[PC6<br>Native VLAN 1] --> B_P3[Access Port<br>VLAN 1]
        
        B_P1 --> B_Internal[Sisemine töötlus<br>Kõik kaadrid märgistatud]
        B_P2 --> B_Internal
        B_P3 --> B_Internal
        
        B_Internal --> B_Trunk[Trunk Port]
    end
    
    A_Trunk <--> T_Link[Trunk Link] <--> B_Trunk
    
    subgraph "Trunk Link - Kaadrite liikumine"
        T_Frame1[Kaader VLAN 10<br>802.1Q Tag: 10]
        T_Frame2[Kaader VLAN 20<br>802.1Q Tag: 20]
        T_Frame3[Kaader Native VLAN<br>Ilma tagita]
    end
    
    style A_PC1 fill:#f9f,stroke:#333
    style A_PC2 fill:#bbf,stroke:#333
    style A_PC3 fill:#bfb,stroke:#333
    style B_PC1 fill:#f9f,stroke:#333
    style B_PC2 fill:#bbf,stroke:#333
    style B_PC3 fill:#bfb,stroke:#333
    
    style T_Frame1 fill:#f9f,stroke:#333
    style T_Frame2 fill:#bbf,stroke:#333
    style T_Frame3 fill:#bfb,stroke:#333
    
    style A_Trunk fill:#ff9,stroke:#333,stroke-width:2px
    style B_Trunk fill:#ff9,stroke:#333,stroke-width:2px
    style T_Link fill:#ff9,stroke:#333,stroke-width:2px
```
Kommutaatori sees edastatakse kõiki VLAN-e märgistustega (tag). See tähendab, et kommutaator töötleb sisemiselt kõiki kaadreid koos märgistustega.

Kui te teete laboritööd, näitasin teile, et tegelikult on MAC-tabelis külgedel näidatud VLAN-ide numbrid. Kui vaatame siit, näeme, et see on VLAN 1 ja see MAC-aadress tuleb VLAN 1-st. Või näiteks näete, et siin on MAC-aadress VLAN 10 või 11 jaoks.

Kommutaator töötleb kõiki kaadreid alati märgistustega. Isegi kui võtate täiesti uue, seadistamata kommutaatori, millel on kõik pordid VLAN 1-s, ja kui kõik pordid on access-režiimis ning kaader satub kommutaatorisse, lisab kommutaator sellele märgistuse. See tähendab, et sisemiselt töödeldakse kõike ikkagi märgistustega.

Lihtsalt kui kommutaator on karbist välja võetud, on VLAN 1 seal alati olemas ja hiljem hakkate lisama oma VLAN-e.

### Native VLAN kontseptsioon
```mermaid
graph LR
    subgraph "Kommutaator A"
        A_VLAN1[VLAN 1 - Native]
        A_VLAN10[VLAN 10]
        A_VLAN20[VLAN 20]
        A_VLAN30[VLAN 30]
        A_PORT[Trunk Port]
    end
    
    subgraph "Kommutaator B"
        B_PORT[Trunk Port]
        B_VLAN1[VLAN 1 - Native]
        B_VLAN10[VLAN 10]
        B_VLAN20[VLAN 20]
        B_VLAN30[VLAN 30]
    end
    
    A_PORT -- "VLAN 10 (tagged)" --> B_PORT
    A_PORT -- "VLAN 20 (tagged)" --> B_PORT
    A_PORT -- "VLAN 30 (tagged)" --> B_PORT
    A_PORT -- "VLAN 1 (untagged)" --> B_PORT
    
    style A_VLAN1 fill:#f9f,stroke:#333,stroke-width:2px
    style B_VLAN1 fill:#f9f,stroke:#333,stroke-width:2px
```
Näitel on kujutatud VLAN-id 30, 20, 10 ja 1, ning siin tahetakse arutada sellist mõistet nagu "Native VLAN" (põhi-VLAN).

VLAN-ide kaadrid saabuvad ilma märgistuseta access-portidesse ja edastatakse trunk-ühenduses märgistustega. Kuid on võimalik määrata selline VLAN, mida edastatakse trunk-ühenduses ilma märgistuseta. Sellele VLAN-ile ei lisata märgistust ja selline VLAN saab olla ainult üks.

Me ütleme, et "seda VLAN-i võib edastada ilma märgistuseta". Vaikimisi seadistustes on see tavaliselt VLAN 1, kuid mõnikord saab määrata ka mingi muu numbri, mis on teile sobiv.

Mõnikord püütakse asendada VLAN-i numbrit, mis pole alati hea idee, eriti kui jutt käib spanning-tree protokollidest, mis lahendavad silmuste probleeme. Kuid mõnikord seda tehakse.

### Näide

Näiteks siin väljub see kui VLAN 1, aga siin seadistame kommutaatoril ja ütleme, et põhi-VLAN on näiteks VLAN 204. Vastavalt sellele:
- Siit väljub kaader ilma märgistuseta
- Kui see saabub teise kommutaatorisse, lisatakse sellele teise VLAN-i märgistus

Sellist lähenemist kasutatakse mõnikord, kui on vaja muuta VLAN-i numbrit trunk-ühenduses. Ülejäänud VLAN-id edastatakse oma numbrite all ja töödeldakse kommutaatori sisenemisel.

## Olulised tähelepanekud

Oluline moment on see, et selleks, et kommutaator saaks trunk-ühenduse kaudu VLAN-e vastu võtta, peab see VLAN olema kommutaatori VLAN-ide andmebaasis olemas. See tähendab, et kui valite kommutaatori, millel on ainult VLAN 1, siis seni, kuni te ei lisa VLAN-ide andmebaasi numbreid, millega töötate, ei saa seade nendega töötada.

Kommutaatorile tuleb "selgitada", milliseid VLAN-e ta peaks töötlema. Lisaks räägime hiljem, et trunk-ühenduse seadistamisel saate keelata/lubada teatud VLAN-ide vastuvõtmist. See on konfigureeritav - saate otse määrata, milliseid VLAN-e vastu võtate. Nii me ka seadistame oma labori- ja kodutöödes.

## VLAN-ide piirangud

Veel üks asi, mida võib märkida - on kommutaatoreid, millel on piiratud VLAN-ide arv. On teatud piiratud versioonid või lihtsustatud litsentsid, kus näiteks öeldakse, et võite kasutada ükskõik millist VLAN-i numbrit, kuid mitte rohkem kui 16 VLAN-i. Võib tunduda, et see on palju, kuid mõnikord on see vähe. Näiteks ettevõtetes on tavaline olukord, kus ühes kommutaatoris võib olla 30-40 VLAN-i.


## Marsruutimine VLAN-ide vahel. ROAS.
```mermaid
graph TD
    subgraph "L3 Tase"
        R[Ruuter]
        R_IF1[Sub-IF 1 - VLAN 10]
        R_IF2[Sub-IF 2 - VLAN 20]
        R_IF3[Sub-IF 3 - VLAN 30]
        
        R --- R_IF1
        R --- R_IF2
        R --- R_IF3
    end
    
    subgraph "L2 Tase"
        SW[Kommutaator]
        VLAN10[VLAN 10]
        VLAN20[VLAN 20]
        VLAN30[VLAN 30]
        TRUNK[Trunk Port]
        
        SW --- VLAN10
        SW --- VLAN20
        SW --- VLAN30
        SW --- TRUNK
    end
    
    PC1[Arvuti 1] --- VLAN10
    PC2[Arvuti 2] --- VLAN20
    PC3[Arvuti 3] --- VLAN30
    
    TRUNK -- "Üks füüsiline kaabel" --- R
    
    R_IF1 -.- VLAN10
    R_IF2 -.- VLAN20
    R_IF3 -.- VLAN30
    
    style R fill:#f96,stroke:#333,stroke-width:2px
    style SW fill:#69f,stroke:#333,stroke-width:2px
    style VLAN10 fill:#9f9,stroke:#333,stroke-width:1px
    style VLAN20 fill:#ff9,stroke:#333,stroke-width:1px
    style VLAN30 fill:#f9f,stroke:#333,stroke-width:1px
```
Võib öelda, et need on eraldi võrgud; võiksin isegi öelda, et need on kolm eraldi kommutaatorit, kuhu on ühendatud meie eraldi kasutajad.

Need kasutajad saavad omavahel töötada ühe võrgu piires, kuid nad ei saa suhelda üksteisega, sest ilmselgelt on nad ühendatud võrgu erinevatesse segmentidesse laialt leviedastuse mõttes.

Tehniliselt on see üks seade, kuid loogiliselt näeb see välja nagu erinevad võrgud, mis ei saa kunagi üksteisega ühendust.

Ühenduse loomise küsimus on alati marsruutimine. Pidage meeles, et ühest võrgust väljumiseks ja teise sisenemiseks on vaja marsruutimist.

### Router on a Stick (ROAS)

![HSRP](https://media.geeksforgeeks.org/wp-content/uploads/hsrp1.png)

Image Source: [GeeksforGeeks](https://www.geeksforgeeks.org/)

Selleks on vaja kas ruuterit või L3 kommutaatorit. Meil on siin ruuter, ma ei räägi L3 kommutaatoritest, räägime neist hiljem. Siin on kujutatud skeem, mida sageli nimetatakse "ruuter kepil" või "router on a stick".

Pange tähele, et siin on L2 skeem ja siin on L3 skeem. Nagu ma juba mainisin, et võrgu toimimise mõistmiseks peab alati olema kaks vaadet sellele. L2 näitab, kuidas me oleme füüsiliselt ühendatud, L3 näitab, kuidas see toimib loogiliselt.

Ruuterisse läheb trunk-kaabel. See on üks kaabel, füüsiliselt mitte kolm, vaid üks. Need on lihtsalt kujutatud erinevate VLAN-idena ja erinevate ühendusliinidena, et teil oleks arusaadav, et need on kaadrid erinevate VLAN-i numbritega, erinevalt värvitud ruuteril.

Need ühendusliinid on koondatud kolme liideseks, iga VLAN-i jaoks eraldi. On selge, et ruuteril on need otseselt ühendatud võrgud.

Kui ruuter teab kõigist kolmest võrgust, tähendab see, et paketid marsruuditakse nende vahel ühes ja teises suunas, sest ta teab, et on otseselt ühendatud kõigi kolmega.

Seega saab toimuda marsruutimine. Seetõttu suunatakse ühest VLAN-ist pärit paketid, mis jõuavad vaikimisi lüüsile ruuteris, teise VLAN-i ja vastupidi.

Märgin veel kord, et skeemi nimetatakse "router on a stick", kus ruuter ühendub mitme VLAN-iga ja teostab nende vahel marsruutimist.

### Alternatiivid ja kaalutlused

See pole alati mugav, sest vaadake - teie link hakkab koormatud olema voogudega ühes ja teises suunas. Hakkate koormama lihtsalt üht linki voogudega edasi-tagasi. Lahkute kommutaatorist ja marsruudite, see pole alati hea, kui teil on läbilaskevõime piirangud. Tavaliselt püütakse vältida selliseid kõrge koormusega linke.

Miski ei takista meil võtmast ruutereid ja neid ühendamast lihtsalt kolme füüsiliselt erineva lingiga, igaühes oma VLAN. See on selge, et võtame ruuteri ja teeme sellised eraldi ühendused - access pordid, ilma trunk-ita - määrame neile kolmele IP-aadressile igaühele eraldi pordile.

Aga me teame kulusid, mäletate, kui näitasin, et ruuteritel pole nii palju liideseid ja neid on üsna vähe, seega püütakse alati kokku hoida. Hea viis on koguda kõik trunk-i ja anda see üle.

## Alamliidesed (Subinterfaces)
```mermaid
flowchart LR
    PC1[Arvuti VLAN 10] --> SW1
    PC2[Arvuti VLAN 20] --> SW1
    
    subgraph SW1[Kommutaator]
        V10[VLAN 10]
        V20[VLAN 20]
        TP[Trunk Port]
    end
    
    TP -- "Trunk-ühendus" --> RT
    
    subgraph RT[Ruuter]
        SIF1[Sub-IF VLAN 10\n192.168.10.1/24]
        SIF2[Sub-IF VLAN 20\n192.168.20.1/24]
        RSW[Marsruutimise\nProtsess]
        
        SIF1 --> RSW
        SIF2 --> RSW
    end
    
    RSW -- "Paketi marsruutimine" --> SIF2
    SIF2 -- "Liiklus VLAN 20-sse" --> TP
    TP -- "VLAN 20 liiklus" --> V20
    V20 --> PC2
    
    style V10 fill:#9f9,stroke:#333,stroke-width:1px
    style V20 fill:#ff9,stroke:#333,stroke-width:1px
    style SIF1 fill:#9f9,stroke:#333,stroke-width:1px
    style SIF2 fill:#ff9,stroke:#333,stroke-width:1px
    style RSW fill:#f96,stroke:#333,stroke-width:2px
```
Te loote niinimetatud subinterface'i ja sellel on mitu liidest ühes füüsilises - ütlete, et sellises VLAN-is on selline liides, sellises VLAN-is on selline liides ja sellises on selline. Seda nimetatakse subinterface'iks.

On võimalik lisada märgiseid erinevatele liidestele - teil on üks füüsiline liides, kuid sellel on mitu alamliidest, mida nimetatakse subinterface'iks. See sõltub jälle tootjast - Cisco puhul on see subinterface ja kui see on Juniper, siis on see unit-interface, mida nimetatakse unit-interface'iks. VLAN-i konfigureerimine on erinev.

# VLAN-ide Konfigureerimine

## VLAN-ide Loomine ja Haldamine

![Crear VLAN Cisco](https://netcloudengineering.com/wp-content/uploads/2018/07/crear-vlan-cisco.jpg.webp)

### VLAN-i Loomine
VLAN-i loomiseks sisesta järgmised käsud konfiguratsiooni režiimis:
```
vlan 2
name test
```

**Selgitus:**
- Esimene rida loob VLAN-i numbriga 2
- Teine rida annab sellele nime "test"
- VLAN-ile nime andmine pole kohustuslik, kuid see aitab võrgu halduril hiljem paremini mõista, milleks konkreetne VLAN loodi (näiteks "külalisvõrk", "õpilaste võrk" või "administratiivne võrk")

### VLAN-ide Kuvamine

![Word Image 24](https://www.flackbox.com/wp-content/uploads/2017/09/word-image-24.png)

VLAN-ide ja nendesse kuuluvate portide vaatamiseks kasuta järgmisi käske:
```
show vlan
show vlan brief
```

**Selgitus:**
- `show vlan` näitab detailset informatsiooni kõigi VLAN-ide kohta
- `show vlan brief` annab kokkuvõtlikuma ülevaate
- Need käsud näitavad, milliseid VLAN-e on loodud ja millised pordid neisse kuuluvad
- VLAN võib andmebaasis eksisteerida ka siis, kui sellesse pole veel ühtegi porti määratud

### VLAN-i Kustutamine
VLAN-i kustutamiseks andmebaasist:
```
no vlan X
```

**Selgitus:**
- X on VLAN-i number, mida soovid kustutada
- Enne kustutamist veendu, et ükski aktiivne port pole selle VLAN-iga seotud

## Portide Konfigureerimine

![Word Image](https://www.flackbox.com/wp-content/uploads/2017/09/word-image-22.png)

### Access-režiimi Port
Access-režiimi port kuulub ainult ühte VLAN-i ja on tavaliselt mõeldud lõppseadmete (arvutid, printerid jne) ühendamiseks.

```
interface fastEthernet 0/1
switchport mode access
switchport access vlan 2
```

**Selgitus:**
- Esimene rida valib pordi, mida soovid konfigureerida
- Teine rida määrab pordi töörežiimiks "access"
- Kolmas rida määrab, millisesse VLAN-i (antud juhul VLAN 2) see port kuulub
- Access-režiimis port eemaldab saabuvalt raamilt VLAN-i sildi ja lisab selle väljuvale raamile
- Oluline: tavaliselt saab üks port kuuluda ainult ühte VLAN-i

![Word Image 25](https://www.flackbox.com/wp-content/uploads/2017/09/word-image-25.png)

### Trunk-režiimi Port
Trunk-režiimi port võimaldab edastada mitme VLAN-i liiklust ja on tavaliselt kasutusel kommutaatorite või ruuterite vaheliste ühenduste jaoks.

```
interface fastEthernet 0/22
switchport mode trunk
switchport trunk allowed vlan 1,2
```

**Selgitus:**
- Esimene rida valib pordi, mida soovid konfigureerida
- Teine rida määrab pordi töörežiimiks "trunk"
- Kolmas rida määrab, milliste VLAN-ide (antud juhul VLAN 1 ja 2) liiklust see port lubab
- Mõnedel vanematel Cisco 3750 seeria lülititel võib olla vaja lisada ka käsk `encapsulation dot1q`

![Word Image 8](https://www.flackbox.com/wp-content/uploads/2017/10/word-image-8.png)

### Lubatud VLAN-id Trunk-pordis
```
switchport trunk allowed vlan 1,2
```

**Selgitus:**
- See käsk määrab, milliste VLAN-ide liiklus on trunk-pordil lubatud
- Antud näites on lubatud VLAN 1 ja 2
- TÄHTIS: See käsk asendab kõik eelnevad seadistused - kui soovid lisada uut VLAN-i olemasolevate kõrvale, kasuta järgmist käsku

### VLAN-ide Lisamine Trunk-porti
```
switchport trunk allowed vlan add 5
```

**Selgitus:**
- See käsk lisab VLAN 5 lubatud VLAN-ide nimekirja, säilitades samal ajal olemasolevad seadistused
- See on turvalisem meetod, kuna ei kustuta eelnevaid seadistusi

![Word Image 9](https://www.flackbox.com/wp-content/uploads/2017/10/word-image-9.png)

![Word Image 10](https://www.flackbox.com/wp-content/uploads/2017/10/word-image-10.png)

## Native VLAN

![Native VLAN Image](https://slideplayer.com/slide/13554705/82/images/59/Native+VLAN+Native+VLAN+For+devices+that+do+not+support+tagging..jpg)

Native VLAN on eriline VLAN, mille liiklus saadetakse trunk-pordis ilma VLAN-sildita (untagged).

```
switchport trunk native vlan 5
```

**Selgitus:**
- See käsk määrab VLAN 5 native VLAN-iks antud trunk-pordil
- TÄHTIS: Native VLAN peab olema eelnevalt trunk-pordil lubatud
- Erinevate native VLAN-ide kasutamine trunk-ühenduse eri otstes võib põhjustada probleeme

### Milleks Native VLAN?

**1. Seadmete Haldamine:**
- Native VLAN on kasulik seadmete puhul (nt DSL-modemid), mis ei töötle VLAN-silte
- Need seadmed saavad vastu võtta ainult märgistamata liiklust
- Kui määrad haldusvõrgu VLAN-i native VLAN-iks, saad hallata ka neid seadmeid, mis ei toeta VLAN-tehnoloogiat

**2. Hädaolukorra Juurdepääs:**
- Kui haldusvõrgu lüliti lakkab töötamast, saad ühendada haldusarvuti otse trunk-porti
- Kui haldusvõrgu VLAN on määratud native VLAN-iks, saad juurdepääsu ilma täiendava konfiguratsioonita
- See on nagu tagavaraväljapääs võrguadministraatoritele

## Ohud ja Vältimissoovitused

**VLAN-ide Numbrite Muutmine Ühenduste Vahel:**
- Väldi erinevate VLAN-i numbrite kasutamist ühenduse eri otstes, näiteks:
  ```
  (Switch 1) access vlan 10 --- access vlan 100 (Switch 2)
  ```
- See võib põhjustada probleeme, kui sama seade hiljem teise porti ühendatakse
- Lüliti võib näha sama MAC-aadressi tulemas erinevatest VLAN-idest, mis tekitab segadust

**CDP ja Spanning-Tree Hoiatused:**
- Erinevad native VLAN-id trunk-ühenduse otstes võivad põhjustada "CDP VLAN mismatch" hoiatusi
- Need protokollid jagavad infot VLAN-ide kohta ja võivad tuvastada mittevastavusi
- Jälgi alati süsteemi hoiatusi, kuna need võivad osutada konfiguratsioonivigadele

## Kasulikud Käsud VLAN-ide Kontrollimiseks

**Trunk-portide Kontrollimine:**
```
show interface trunk
```

See käsk näitab:
- Millised pordid on konfigureeritud trunk-režiimis
- Millised pordid on aktiivsed trunk-pordid
- Mis on native VLAN iga trunk-pordi jaoks
- Millised VLAN-id on lubatud üle trunk-pordi

![Show Interfaces Trunk](https://geek-university.com/wp-content/uploads/2015/10/show_interfaces_trunk.jpg)

Image Source: [Geek University](https://geek-university.com)


**VLAN-ide Kontrollimine:**
```
show vlan
```

See käsk näitab:
- Millised VLAN-id on loodud
- Millised pordid kuuluvad millisesse VLAN-i
- VLAN-ide olekut ja muid parameetreid

![Show VLANs Command](https://geek-university.com/wp-content/uploads/2015/10/show_interfaces_trunk.jpg)

Image Source: [Geek University](https://geek-university.com)
