# Teema 19: DHCP protokoll: tööpõhimõte, komponendid ja kasutusjuhtum

## Ülevaade

DHCP (Dynamic Host Configuration Protocol) on võrguprotokoll, mis võimaldab seadmetel automaatselt saada IP-aadressi ja muid võrguseadeid. See protsess toimub kiiresti ja läbipaistvalt, mistõttu kasutaja tavaliselt ei märka DHCP tööd. Protokoll on vaikimisi enamikes seadmetes sisse lülitatud, võimaldades neil pidevalt aadressi saada.

DHCP on arenenud välja vanemast BOOTP (Bootstrap Protocol) protokollist. Mõnikord võib vanemates seadmetes kohata viiteid BOOTP-le, mis tegelikult viitavad DHCP-le.

![ManageEngine DHCP Server Diagram](https://www.manageengine.com/products/oputils/images/DHCP-Server.png)

---

## DHCP tööpõhimõte

Kui seade ühendub võrku ja tal pole veel IP-aadressi, toimub järgmine protsess:

1.  **DHCP Discover**

    Seade saadab võrgus laiali spetsiaalse UDP-paketi (kasutades broadcast aadressi), otsides DHCP-serverit. See sõnum saadab välja, et leida saadaolevad DHCP serverid võrgus.

2.  **DHCP Offer**

    DHCP-server vastab pakkumisega (DHCP Offer), milles sisaldub soovitatav IP-aadress, mask, lüüs, DNS-server ja liisingu aeg. Server teatab, millist IP-aadressi ta on valmis seadmele pakkuma.
3.  **DHCP Request**

    Seade valib sobiva pakkumise ja saadab kinnituse (DHCP Request), et soovib seda aadressi kasutada. Kui klient saab mitu DHCP pakkumist, võib ta valida ühe neist.

4.  **DHCP Ack**

    DHCP-server saadab omakorda kinnituse (DHCP Ack), millega IP-aadress antakse seadmele teatud ajaks (liisinguaeg).
5.  **APIPA**

    Kui seade ei saa DHCP-serverilt vastust, määrab ta endale automaatselt aadressi vahemikust 169.254.x.x (APIPA - Automatic Private IP Addressing). Need aadressid viitavad probleemile, kus DHCP server ei ole kättesaadav.

---

![DHCP Services Flow](https://www.researchgate.net/publication/371162381/figure/fig4/AS:11431281225505230@1708745171644/DHCP-services-flow-allocation-of-IP-Renewal-of-IP-and-IP-release.png)


---

## Aadresside jagamise viisid

-   **Dünaamiline:** IP-aadressid jagatakse automaatselt vabade aadresside hulgast.
-   **Staatiline (käsitsi):** DHCP-serverile saab määrata, et konkreetse MAC-aadressiga seadmele antakse alati kindel IP-aadress. Näiteks printerid või serverid, mille aadress peab olema püsiv.

    Seda on vaja olukordades, kus seadmel peab olema fikseeritud aadress. Näiteks printeri puhul, millele kõik kasutajad on seadistanud ühenduse, või serveri puhul, millele on tulemüüris seatud spetsiifilised reeglid.

---

## DHCP komponendid

-   **DHCP klient:** iga seade, mis küsib võrgust IP-aadressi (arvuti, telefon, ruuteri port jne).
-   **DHCP server:** jagab aadresse (koduvõrgus tavaliselt ruuter).
-   **DHCP relay agent:** vahendab DHCP liiklust erinevate võrkude vahel. Seda kasutatakse, kui DHCP server ja klient asuvad erinevates võrgusegmentides.

---

## DHCP sõnumid ja väljad

![DHCP Lease Life Cycle](https://lazyadmin.nl/wp-content/uploads/2019/01/DHCP-Lease-Life-Cycle.jpg)


-   **Peamised sõnumid:** Discover, Offer, Request, Ack, Decline, Release, Inform.
-   **Paketi väljad:**
    *   Operatsiooni kood (request või reply)
    *   Riistvara aadressi pikkus (MAC aadressi pikkus)
    *   Hüppe loendur (kasutatakse relay agentide puhul)
    *   Transaktsiooni ID
    *   Sekundid kliendi käivitamisest
    *   Flagid (nt broadcast)
    *   Kliendi IP aadress
    *   Sinu IP aadress (serveri poolt pakutav aadress)
    *   Serveri IP aadress
    *   Relay agent IP aadress
    *   Kliendi riistvara aadress (MAC aadress)
    *   Serveri nimi
    *   Boot faili nimi
    *   Valikulised parameetrid (mask, lüüs, DNS-server, staatilised marsruudid)

DHCP kasutab UDP pakette.

---

![DHCP Packet Diagram](https://miro.medium.com/v2/resize:fit:720/format:webp/1*aF5s7OQXsTzXTMnqpMdyNA.png)


---

## Liisinguaeg (lease time)

-   IP-aadress antakse seadmele teatud ajaks.
-   Kui liisinguaeg saab läbi, peab seade aadressi uuendama.
-   Liisinguaja valik sõltub võrgukeskkonnast:
    *   Väikestes võrkudes võib see olla pikk (päevad-nädalad).
    *   Avalikes WiFi-võrkudes pigem lühike (minutid-tunnid), et aadressid kiiresti vabanevad.

Ligikaudu poole liisinguaja möödudes proovib klient DHCP serveriga ühendust, et liisingut uuendada. Kui see ebaõnnestub, proovib klient uuesti 87.5% möödudes.

![DHCP Lease](https://www.digitalcitizen.life/wp-content/uploads/2020/10/dhcp_9.png)

---

## Kasutusjuhtum: Staatilise IP määramine printerile

**Olukord:**

Kontoris on võrguprinter, millele kõik töötajad saadavad väljatrükke. Printeri IP-aadress peab olema alati sama, et draiverid töötaksid ja ühendused ei katkeks.

**Lahendus:**

DHCP-serveris määratakse printeri MAC-aadressile alati kindel IP-aadress (st staatiline jagamine).

Kui printer taaskäivitub või ühendub uuesti võrku, saab ta alati sama IP-aadressi.

**Tulemus:**

Töötajate arvutid leiavad printeri alati samalt aadressilt.

Võrguadministraatoril on lihtne printerit hallata.

---

![Static IP Diagram](https://www.trendnet.com/images/news/2021/0810/05.png)


---

## Kasutusjuhtum: DHCP server ruuteril koduvõrgus

**Olukord:**

Kodune ruuter toimib DHCP serverina, jagades IP aadresse kõigile koduvõrgu seadmetele (arvutid, telefonid, tahvelarvutid).

**Lahendus:**

Ruuter on seadistatud jagama IP-aadresse vahemikust 192.168.1.100 kuni 192.168.1.200. Iga seade, mis ühendub koduvõrguga, saab automaatselt IP-aadressi sellest vahemikust.

**Tulemus:**

Kasutajad ei pea käsitsi seadistama IP-aadresse oma seadmetel.
Ruuter haldab aadresside jagamist ja tagab, et igal seadmel on unikaalne IP-aadress.

---

## Kasutusjuhtum: DHCP Serveri varundamine ja töökindluse tagamine (2 DHCP serverit)

![2 DHCP servers](https://i.sstatic.net/tVOAv.png)

**Olukord:**

Suuremas ettevõttes on kriitiline, et IP-aadresside jagamine toimiks tõrgeteta. Üksiku DHCP serveri rike võib põhjustada võrguühenduse katkemise paljudele kasutajatele. Selle vältimiseks kasutatakse kahte DHCP serverit.

**Lahendus:**

Seadistatakse kaks DHCP serverit failover-režiimis. Üks server on aktiivne ja jagab aadresse, teine on ooterežiimis ja valmis üle võtma.

**Tööpõhimõtted:**

1.  **Aktiivne server:** Jagab aktiivselt IP-aadresse ja uuendab teise serverit andmetega.
2.  **Oote server:** Jälgib aktiivset serverit ja on valmis üle võtma, kui aktiivne server peaks rivist välja minema.
3.  **Failover:** Kui aktiivne server ei vasta, võtab oote server automaatselt üle ja jätkab aadresside jagamist.

**Põhjused kahe DHCP serveri kasutamiseks:**

-   **Töökindlus:** Tagab, et võrk ei jää IP-aadressideta ka serveri rikke korral.
-   **Katkestusteta teenus:** Kasutajad ei märka, kui üks serveritest rivist välja langeb.
-   **Koormuse jaotus:** Mõnel juhul saab seadistada serverid jagama koormust, kus kumbki server haldab teatud osa aadressivahemikust.

**Täiendavad kaalutlused:**

-   Liisingu aeg tuleb seadistada vastavalt võrgu vajadustele. Suure kasutajate arvuga võrkudes on vaja lühemat liisingu aega, et IP-aadressid vabaneeksid kiiremini.
-   Staatilised IP-aadressid tuleb määrata seadmetele, mis vajavad püsivat aadressi (nt printerid, serverid).
-   Turvalisuse kaalutlustel tuleb DHCP serverid paigutada turvalisse võrgusegmenti ja jälgida nende tegevust.

---

## Näide: Staatilise IP määramine Cisco ruuteris

```
ip dhcp pool VÕRK10
   network 192.168.10.0 255.255.255.0
   default-router 192.168.10.1
   dns-server 8.8.8.8
!
ip dhcp excluded-address 192.168.10.1 192.168.10.99
!
ip dhcp pool PRINTER1
   host 192.168.10.200 255.255.255.0
   client-identifier 0100.1122.3344.5566
!
```

Selles näites:

*   `ip dhcp pool VÕRK10` määrab DHCP-aadressi vahemiku.
*   `network 192.168.10.0 255.255.255.0` määrab võrgu aadressi ja maski.
*   `default-router 192.168.10.1` määrab vaikimisi lüüsi.
*   `dns-server 8.8.8.8` määrab DNS-serveri.
*   `ip dhcp excluded-address` määrab aadressid, mida DHCP ei tohiks jagada (kasutatakse staatiliste aadresside jaoks).
*   `ip dhcp pool PRINTER1` määrab staatilise IP-aadressi printerile.
*   `host 192.168.10.200 255.255.255.0` määrab IP-aadressi ja maski.
*   `client-identifier 0100.1122.3344.5566` määrab MAC-aadressi, millele see aadress antakse.

![How to Configure DHCP Relay Agent on Cisco Routers – ComputerNetworkingNotes](https://www.computernetworkingnotes.com/wp-content/uploads/ccna-study-guide/images/csg74-15-set-dhcp-clients.png)


## Näide: Liisinguaja seadistamine

```
ip dhcp pool VÕRK10
   network 192.168.10.0 255.255.255.0
   lease 7
```

*   `lease 7` määrab liisinguaja 7 päevaks. Väiksem väärtus (nt 0) võib määrata selle tunnini.

## Näide: DHCP Relay Agendi seadistamine

Kui DHCP server ja klient asuvad erinevates võrgusegmentides, tuleb kasutada DHCP Relay agenti.

```
interface VLAN10
   ip address 192.168.10.1 255.255.255.0
   ip helper-address 192.168.1.10
!
```

*   `ip helper-address 192.168.1.10` määrab DHCP-serveri aadressi, kuhu DHCP Requestid saadetakse.

![Dell Network Diagram](https://supportkb.dell.com/img/ka06P000000srfxQAA/ka06P000000srfxQAA_en_US_1.png)

---

## Näide

https://www.alibaba.com/countrysearch/CN/dhcp-server.html

YouTube link:

[![Watch the video](https://img.youtube.com/vi/Dp2mFo3YSDY/hqdefault.jpg)](https://www.youtube.com/watch?v=Dp2mFo3YSDY)