# **Teema 5: Aegunud võrgu protokollid ja Etherneti evolutsioon**

## **Sisu**

1. [Sissejuhatus: miks õppida vanu tehnoloogiaid?](#sissejuhatus-miks-õppida-vanu-tehnoloogiaid)  
2. [Etherneti loomise ajalugu](#etherneti-loomise-ajalugu)  
   - [Eetri kontseptsioon](#eetri-kontseptsioon)  
   - [Esimesed Etherneti standardid](#esimesed-etherneti-standardid)  
   - [Kokkupõrked ja varajane topoloogia](#kokkupõrked-ja-varajane-topoloogia)  
3. [Esimesed Etherneti standardid](#esimesed-etherneti-standardid-1)  
   - [10BASE5 ("Paks Ethernet")](#10base5-paks-ethernet)  
   - [10BASE2 ("Õhuke Ethernet")](#10base2-õhuke-ethernet)  
   - [Peamised probleemid](#peamised-probleemid)  
4. [Alternatiivsed lähenemised: Token Ring](#alternatiivsed-lähenemised-token-ring)  
   - [Token Ringi kontseptsioon ("Markerirõngas")](#token-ringi-kontseptsioon-markerirõngas)  
   - [Eelised ja puudused](#eelised-ja-puudused)  
   - [Evolutsioon](#evolutsioon)  
5. [Tärntopoloogia: StarLAN](#tärntopoloogia-starlan)  
   - [StarLAN-i ilmumine](#starlan-i-ilmumine)  
   - [Tärntopoloogia eelised](#tärntopoloogia-eelised)  
   - [Etherneti evolutsioon](#etherneti-evolutsioon)  
6. [Kokkuvõte](#kokkuvõte)  

---

## **Sissejuhatus: miks õppida vanu tehnoloogiaid?**
Aegunud võrgu protokollide teema võib esmapilgul tunduda kummaline ja ebaoluline. Kuid selle õppimine on kasulik kahel peamisel põhjusel:

1. **Ajalooline arusaam:** Et mõista, kuidas Ethernet tekkis ja mõista selle arhailisi jooni, mis mõjutavad endiselt kaasaegseid võrke. Need omadused kujunesid 1970. aastatel ja on säilinud protokolli põhikontseptsioonis.

2. **Alternatiivsed lähenemised:** Inseneridel on oluline mõista, kuidas võrgusuhtluse ülesandeid saaks lahendada erinevate meetoditega, sealhulgas enne Etherneti ilmumist kasutatud meetoditega.

![Ethernet is Born](https://thisdayintechhistory.com/wp-content/uploads/2012/12/ethernet_630px.jpg)

Ethernet is Born
Source: [This Day in Tech History - Ethernet is Born](https://thisdayintechhistory.com/).

### Questions:

1. **Ethernet Development**:  
   - Who developed Ethernet, and what other innovations is this company known for?

2. **Meaning of "Ether"**:  
   - What does "Ether" mean in the context of Ethernet, and how does it relate to the concept of communication in networks?

3. **Mouse and Personal Computers**:  
   - Which company first added the mouse to the personal computer, and how did this innovation shape modern computing?

---

## **Etherneti loomise ajalugu**

### **Eetri kontseptsioon**
- Nimi "Ethernet" pärineb mõistest "eeter", mida kasutati raadiosignaalide edastuskeskkonna kirjeldamiseks. Võrgus kasutati pakettide edastamiseks raadiolaineid.
- Andmeedastuse lihtsustamiseks olukordades, kus raadioseadmeid ei olnud, pakuti raadiolainete asendamiseks koaksiaalkaablit. See võimaldas "peita" raadiolaine kaabli sees.

### **Esimesed Etherneti standardid**
- Xerox’i labor eksperimenteeris võrguga, kus seadmed ühendati ühise koaksiaalkaabliga. See lahendas raadioseadmete puudumise probleemi, kuid tõi kaasa uusi tehnilisi väljakutseid.

### **Kokkupõrked ja varajane topoloogia**
- Üks peamisi probleeme olid kokkupõrked — olukorrad, kus kaks seadet üritasid andmeid samaaegselt edastada. Seda piirangut püüti lahendada enam kui 20 aastat.
- Esimene Etherneti võrk oli kaabelpuu. Signaalid edastati ühes ühises kanalis, mis tegi võrgu haavatavaks igasugustele füüsilistele kaablipurunemistele.

Etherneti loomisel saadi inspiratsiooni Aloha Networki katsest, mis viidi läbi Hawaii ülikoolis. Aloha Network loodi selleks, et testida, kuidas jagada ühte suhtluskanalit (raadioside) paljude seadmete vahel, kuna Hawaii saarte vahel oli vaja tõhusat sidevõrku.
[Ethernet Evolution in Computer Networks](https://www.geeksforgeeks.org/ethernet-evolution-in-computer-networks/)


---
![Thicknet vs Thinnet](https://network-byte.com/wp-content/uploads/2020/05/thicknet-10base5-vs-thinnet-10base2-min.png)

Both Thicknet (10Base5) and Thinnet (10Base2) coaxial cables use a bus topology
Source: [Network Byte - Thicknet vs Thinnet](https://network-byte.com/).


## **Esimesed Etherneti standardid**

### **10BASE5 ("Paks Ethernet")**
- Võeti vastu 1983. aastal.
- Kasutas paksu koaksiaalkaablit, mille läbimõõt oli 10 mm (RG-8).
- Edastuskiirus — 10 Mbit/s.
- Võrgu seadmete ühendamiseks kasutati 15-kontaktilisi transiivereid.
- Peamine puudus — mahukas konstruktsioon ja keeruline seadistamine.

### **10BASE2 ("Õhuke Ethernet")**
- Võeti vastu 1985. aastal.
- Kasutas õhemat koaksiaalkaablit (RG-58), mis lihtsustas paigaldust.
- Säilitas edastuskiiruse 10 Mbit/s.
- Seadmete ühendamine toimus BNC-pistikutega ja T-kujuliste ühendustega.

### **Peamised probleemid**
- Kaabli purunemine tõi kaasa võrgu täieliku töökatkestuse.
- Signaalide peegeldumine purunemiskohtadelt tekitas tõsiseid häireid.
- Purunemiskoha leidmine oli väga aeganõudev, eriti suurtes võrkudes.

![BNC Connector](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhgBZwyxqhDd1F1ezUWDLUppmOoQ20hfhecQz99tJ-nTUgoPVS39ix80-0O3T8LRXp9p7wtXdNj2mGevDAnvRKZmvtQ69yJrJf8-u53TTDMvrXt-KMnrGS6jr5u4A1DC4-SAv330H6_Vm6n/w640-h399/BNC-T.jpg)

The BNC connector (Bayonet Neill-Concelman) is a quick-connect RF connector used in coaxial cable systems, such as Thinnet (10Base2) Ethernet networks. It provides a secure connection for data transmission.  
Source: [Blogger - BNC Connector](https://blogger.googleusercontent.com).

---

![Thinnet Coaxial Cable System](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj5uiHR1hE605nOI9aa8Kq7wPxRNNCv2-I7YOH7ie0e4nmGCu2hHJ1d4VarDWZvohJ1DP7miM6C5Tv7C-4bHKY00PDCb6ozl9cREzeuS2X5Hoe2ADwMOuQJ7SmeHRkcVAtPfzxttSHAhO-b/w640-h272/Untitled.png)

Typical Thinnet (10Base2) coaxial cable setup, illustrating how devices are connected in a bus topology using BNC connectors and terminators.  
Source: [Blogger - Thinnet Coaxial Cable System](https://blogger.googleusercontent.com).

---

## **Alternatiivsed lähenemised: Token Ring**

### **Token Ringi kontseptsioon ("Markerirõngas")**
- IBM-i tehnoloogia, mis standardiseeriti 1985. aastal (IEEE 802.5).
- Võrgus edastati spetsiaalne marker ("token"), mis andis seadmetele õiguse andmeid edastada.
- Kui seadmel ei olnud andmeid edastada, anti marker järgmisele sõlmele rõngas.

### **Eelised ja puudused**
- **Eelised:** Täielik kokkupõrgete välistamine tänu võrgujuurdepääsu kontrollile.
- **Puudused:**
  - Rõnga purunemine tõi kaasa kogu võrgu töö seiskumise.
  - Ühe seadme väljalülitamine võis samuti võrgu töö häirida.
  - Piiratud edastuskiirus: 4 ja 16 Mbit/s algversioonides.

![Token Ring Topology](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Token_ring.svg/1920px-Token_ring.svg.png)

Source: [Wikipedia - Token Ring](https://en.wikipedia.org/wiki/Token_ring).


![Physical Token Ring Wiring](https://upload.wikimedia.org/wikipedia/commons/4/48/Physical_Token_Ring_Wiring.jpg)
  
Source: [Wikipedia - Physical Token Ring Wiring](https://en.wikipedia.org/wiki/Token_ring).


### **Evolutsioon**
- 1990. aastatel täiustati Token Ring’i tehnoloogiat optilise kiu tehnoloogia FDDI (Fiber Distributed Data Interface) abil, mis tagas kiiruse kuni 100 Mbit/s ja omas topeltrõngast töökindluse suurendamiseks.


---

## **Tärntopoloogia: StarLAN**

### **StarLAN-i ilmumine**
- 1988. aastal tõi AT&T turule StarLAN 10 standardi, mis kasutas tärntopoloogiat.
- See kontseptsioon sai aluseks keerupaari kasutavale Ethernetile (10BASE-T), kus füüsiline struktuur oli täht ja loogiline — siin.

### **Tärntopoloogia eelised**
- Suurenenud töökindlus: ühe ühenduse rikke korral jätkas ülejäänud võrk tööd.
- Lihtne diagnostika: lingi oleku näitajad habidel võimaldasid kiiresti leida vead.

### **Etherneti evolutsioon**
- Tärntopoloogia koos kommuteerimisseadmete (lülitite) arenguga tõrjus täielikult välja koaksiaalkaabli võrgud.
- Üleminek keerupaarile ja hiljem optilisele kiule võimaldas kiirust tõsta gigabititasemele ja kõrgemale.


![Networking Topology Example](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSVDB6ISPRgyk7dXTEJ-bNjmhC3zNpzrXJ22g&s)

Source: [Google Images](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSVDB6ISPRgyk7dXTEJ-bNjmhC3zNpzrXJ22g&s).

![Star Network LAN](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdIMk_L6-gHHmQ8_sS-xSR129_MKOLEl05iNb8_wrtimorVircVGLg8Ch_Abu3k6c5kjc1lD0xXHbkPvab4KaQg06iypybWBo7uaHik1TqRdZGphlu3bJbpbjEk5r7UFZLARSe9N_-x9Np/s1600/StarNetworkLan+%25281%2529.jpg)


Source: [Blogger - Star Network LAN](https://blogger.googleusercontent.com).

### Source: StarLAN Reference
[StarLAN in Networking Essentials](https://books.google.ee/books?id=jE2OlZ9PkrkC&pg=PA238&dq=starlan&hl=et&sa=X&ved=2ahUKEwj0q6m8jOeKAxXwPhAIHfiLHDgQ6AF6BAgKEAI#v=onepage&q=starlan&f=false)

### Source: TWISTED: The dramatic history of twisted-pair Ethernet
[![StarLAN Networking Overview](https://img.youtube.com/vi/f8PP5IHsL8Y/0.jpg)](https://www.youtube.com/watch?v=f8PP5IHsL8Y)

This YouTube video provides an overview of StarLAN networking, explaining its historical context, key features, and how it contributed to the evolution of Ethernet-compatible LANs. It's a great visual resource for understanding the basics of StarLAN technology.
