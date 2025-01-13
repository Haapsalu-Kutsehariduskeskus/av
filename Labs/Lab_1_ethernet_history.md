# Lab.1 Ethernet History and Practical Star Topology Design

## Eesmärk:

Labõppe käigus uurivad õpilased neile antud materjale, diagramme ja videot, et saada põhjalik ülevaade arvutivõrkude põhikontseptsioonidest, sealhulgas Etherneti ajaloost, OSI mudelist, võrgu protokollidest ja topoloogiatest. Õpilased vastavad küsimustele, et kinnistada oma teadmisi.

---

## Labõppe juhised:

1. **Uurige materjale**:
   - Lugege läbi kõik üleslaaditud dokumendid:
     - **Teema 1**: Pakettkommuteeritud võrgud.
     - **Teema 2**: Arvutivõrkude ajalugu ja klassifikatsioon.
     - **Teema 4**: OSI mudel ja standardid.
     - **Teema 5**: Aegunud võrgu protokollid.
   - Keskenduge olulistele kontseptsioonidele ja nende rakendustele.

2. **Vaadake videot**:
   - Vaadake videot: [TWISTED: The dramatic history of twisted-pair Ethernet](https://www.youtube.com/watch?v=f8PP5IHsL8Y).
   - Tehke märkmeid, kuidas Ethernet arenes koaksiaalkaablitest keerdpaarjuhtmeteni.

3. **Vastake labõppe küsimustele**:
   - Vastake kõigile allpool esitatud küsimustele, kasutades üleslaaditud materjale ja videot viidetena.

4. **Looge Star-topoloogia Packet Traceris**:
   - Looge kaasaegse Ethernet-põhise võrgu diagramm, kasutades **Star-topoloogiat**.
   - Lisage seadmed ja nimetage need tudengi nime järgi (vt allpool "Praktiline ülesanne").

---

## Küsimused

### Materjalide põhjal:

1. **Teema 1: Pakettkommuteeritud võrgud**:
   - Mis on pakettkommuteeritud võrgud ja kuidas need erinevad ahelkommuteeritud võrkudest?
   - Milline oli Aloha võrgu tähtsus varases võrgunduses?

2. **Teema 2: Ajalugu ja klassifikatsioon**:
   - Kuidas erinevad varased arvutivõrgud kaasaegsetest võrkudest?
   - Selgitage võrkude klassifikatsiooni suuruse (LAN, MAN, WAN) ja eesmärgi järgi.

3. **Teema 4: OSI mudel ja standardid**:
   - Mis on OSI mudeli eesmärk ja miks on see tänapäeval endiselt asjakohane?
   - Võrrelge OSI mudelit TCP/IP mudeliga. Tõstke esile nende sarnasused ja erinevused.

4. **Teema 5: Aegunud võrgu protokollid**:
   - Millised olid aegunud protokollidega, nagu Token Ring ja X.25, seotud väljakutsed?
   - Miks sai Ethernetist lõpuks domineeriv LAN-tehnoloogia?

### Video põhjal:

1. **Koaksiaalkaablite väljakutsed**:
   - Miks olid koaksiaalkaablid (Thicknet ja Thinnet) varajase Etherneti kasutuses keerulised?

2. **Üleminek keerdpaarile**:
   - Mis tegi keerdpaarjuhtmed Etherneti võrkude jaoks revolutsiooniliseks?

3. **Olulised isikud ja panused**:
   - Tuvastage videost olulised isikud (nt Bob Gallan, Pat Thor) ja nende panused Etherneti arengusse.

4. **StarLAN-i roll**:
   - Kuidas kasutas StarLAN olemasolevaid telefonikaableid võrgunduskulude vähendamiseks?

5. **IEEE 802.3 standardid**:
   - Mis oli IEEE 802.3 komitee tähtsus twisted-pair Etherneti standardite väljatöötamisel?

---

## Praktiline ülesanne

### Star-topoloogia loomine Packet Traceris

1. **Seadmete lisamine**:
   - Lisa tööalale järgmised seadmed:
     - **1 lüliti** (nt 2960 Switch).
     - **4 arvutit (PC)**.
     - Valikuline: **1 ruuter**, kui soovid lisada ruutimisühenduse.

2. **Seadmete ümbernimetamine**:
   - Näiteks, kui tudengi nimi on **Mari**:
     - Nimetage arvutid: **PC1_Mari**, **PC2_Mari**, **PC3_Mari**, **PC4_Mari**.
     - Lüliti: **Switch_Mari**.
     - Ruuter (kui lisatud): **Router_Mari**.

3. **Keerdpaarjuhtmete kasutamine**:
   - Kasuta **Copper Straight-Through** kaableid, et ühendada:
     - Kõik arvutid lülitiga.
     - (Valikuline) Lüliti ruuteriga.

4. **IP-aadresside seadistamine**:
   - Iga arvuti jaoks:
     - Ava **Desktop** > **IP Configuration** ja määra IP-aadress (nt 192.168.1.2, 192.168.1.3 jne).
     - Subnet mask: **255.255.255.0**.
   - (Valikuline) Kui lisasite ruuteri:
     - Määrake ruuteri liidesele IP-aadress (nt 192.168.1.1).
     - Seadistage arvutite **Gateway** ruuteri IP-aadressiks.

5. **Testimine**:
   - Kasuta Packet Traceris **ping**-käsku, et kontrollida, kas arvutid saavad omavahel suhelda.
     - Näiteks: Avage **Command Prompt** arvutis 1 ja sisestage: `ping 192.168.1.3`.

6. **Diagrammi salvestamine**:
   - Salvesta loodud fail formaadis **.pkt**.
   - Failinimi: `Star_Topoloogia_<Teie_Nimi>.pkt`.

7. **Faili esitamine**:
   - Lisage salvestatud fail labõppe lõppraportile.

---

## Tulemid:

1. **Vastused kõigile küsimustele**: Kirjutage selged ja detailsed vastused, mis on toetatud antud materjalide ja videoga.
2. **Võrgudiagramm**: Esitage sildistatud diagramm, mis näitab teie arusaamist Ethernet-põhistest LAN-idest.
3. **Packet Tracer fail**: Lisage loodud võrgudiagrammi fail oma esitluse juurde.
4. **Kokkuvõte**: Kirjutage lühike refleksioon Etherneti evolutsiooni mõjust kaasaegsele võrgundusele.

---

## Lisamaterjalid:

- [Networking Essentials - Google Books](https://books.google.ee/books?id=jE2OlZ9PkrkC&pg=PA238&dq=starlan)
- [StarLAN Overview - Archive.org](https://archive.org/details/starlanintro/page/n1/mode/2up)
- [Token Ring Explanation - Wikipedia](https://en.wikipedia.org/wiki/Token_ring)

Edukat õppimist ja head võrkude avastamist!
