# Nutikodu võrgutopoloogia planeerimine
## 1. Võrgutopoloogia alused

### Mis on võrgutopoloogia ja miks see on oluline?

Võrgutopoloogia kirjeldab, kuidas võrgu seadmed on omavahel ühendatud. 
Nutikodudes on õige topoloogia valimine eriti oluline, sest seadmete arv on suur ja nende paigutus varieeruv.
Peamised topoloogiatüübid on:

**Täht-topoloogia**: 
- Kõige tavalisem koduvõrkudes
- Üks keskne seade (ruuter või switch), millega kõik teised seadmed otse ühenduvad
- **Eelised**: lihtne ülesehitus ja haldamine
- **Puudused**: kui keskne seade rikneb, kaotavad kõik seadmed ühenduse

**Mesh-võrk**:
- Mitu võrdset sõlmpunkti, mis suhtlevad omavahel otse
- Kui üks sõlm rikneb, leiab võrk automaatselt uue marsruudi
- Eriti kasulik suurtes kodudes või mitmekorruselistes majades, kus üks ruuter ei suuda kõike katta
- Näiteks Google Nest või Amazon Eero mesh-süsteemid kasutavad seda topoloogiat

**Hübriid-topoloogia**:
- Kombineerib erinevate topoloogiate elemente
- Näiteks võib põhivõrk olla täht-kujuline, aga kaugemad punktid on ühendatud mesh-süsteemiga

## 2. WiFi levi planeerimine

### Kuidas tagada optimaalne WiFi levi kogu kodus?

WiFi levi sõltub mitmest tegurist:

**Levikaugus ja takistused**:
- Tavaline kodune ruuter katab umbes 80-100m² avatud ruumi
- Betoonist seinad võivad levikaugust vähendada kuni 50%
- Puidust või kipsist seinad vähendavad levikaugust umbes 10-20%
- Metallpinnad (sh peeglid) ja mikrolaineahju tüüpi seadmed võivad signaali oluliselt blokeerida

**Ruuteri paigutus**:
Ideaalne asukoht on:
- Kodu keskosas
- Võimalikult kõrgel (nt riiulil)
- Eemal suurtest metallpindadest ja elektriseadmetest
- Vertikaalselt paigutatud antennidega (kui need on välised)

**Suurema pinna katmine**:
Kui üks ruuter ei suuda kogu kodu katta, on järgmised lahendused:
- **WiFi võimendi (range extender)** - lihtne, aga tihti vähendab kiirust
- **Access point** - vajalik kaabliga ühendus ruuterini
- **Mesh-süsteem** - mitu seadet, mis loovad ühtse võrgu ilma kiiruskadudeta

## 3. IP-aadresside planeerimine

### Kuidas luua loogiline ja turvaline aadressiskeem?

IP-aadresside korrektne planeerimine on nutikodu võrgu alus:

**Aadressiruum**:
Enamik koduvõrkudes kasutatakse privaatseid IP-aadresse:
- 192.168.0.0/24 või 192.168.1.0/24 (256 aadressi)
- 10.0.0.0/8 (väga suur aadressiruum)
- 172.16.0.0/12 (keskmine aadressiruum)

**DHCP seadistus**:
Dünaamiline IP-aadresside jagamine seadmetele. Tüüpiline skeem:
- Gateway (ruuter): 192.168.1.1
- DHCP vahemik: 192.168.1.100 - 192.168.1.200 (muutuvad aadressid)
- Staatiliste IP-de vahemik: 192.168.1.2 - 192.168.1.99 (käsitsi määratud)

**Staatilised IP-aadressid**:
Kasutatakse seadmetel, millele on vaja stabiilset ligipääsu:
- Võrguprinteri jaoks (nt 192.168.1.20)
- Meediaserveri jaoks (nt 192.168.1.30)
- Turvakaamerate jaoks (nt 192.168.1.50-59)

## 4. Võrgu segmenteerimine ja turvalisus

### Miks ja kuidas võrku segmenteerida?

Võrgu segmenteerimine on nutikodudes eriti oluline, sest paljud IoT seadmed ei ole piisavalt turvalised:

**Eraldi võrgud erinevate seadmete jaoks**:
- **Põhivõrk**: Arvutid, telefonid, tahvlid (kõrgeim turvalisus)
- **IoT võrk**: Nutilambid, termostaadid, sensorid (keskmise taseme turvalisus)
- **Külalisvõrk**: Külaliste seadmed, piiratud ligipääsuga (madalaim turvalisus)

**Segmenteerimise meetodid**:
- Erinevad SSID-d (WiFi võrgu nimed) sama ruuteriga
- VLAN-id võimekama võrguvarustuse korral
- Täiesti eraldi ruuterid kriitiliste süsteemide jaoks

**Turvameetmed**:
- WPA2/WPA3 protokoll WiFi jaoks (mitte kunagi WEP või WPA1)
- MAC-aadresside filtreerimine (täiendav turbemeede)
- Ruuteri püsivara regulaarne uuendamine
- Vaikeparoolide muutmine KÕIGIL seadmetel

## 5. Praktilised näited ja arvutused

### WiFi access pointide arvu määramine

![AP](https://cdn.prod.website-files.com/622b70d8906c7ab0c03f77f8/66bccd60e14e9c1a9d7c88f6_unifi7.png)

Valem: `Vajalike AP-de arv = Kodu pindala (m²) / Ühe AP leviala (m²)`

Näide:
- Maja suurus: 180m²
- Ühe AP leviala: ~80m² (arvestades seinu)
- Vajalik: 180/80 ≈ 2.25 ehk 3 access pointi

### Ethernet kaablite pikkuse arvutamine

Valem: `Kaabli pikkus = Otsene vahemaa + 30% varu`

Näide:
- Vahemaa ruuterist arvutini: 5m
- Vajalik kaabli pikkus: 5m + 1.5m (30%) = 6.5m

### IP-aadresside jagamine alamvõrkude vahel

Näide kolme alamvõrgu loomisest:
- Põhivõrk: 192.168.1.0/24 (256 aadressi)
- IoT seadmete võrk: 192.168.2.0/24 (256 aadressi)
- Külaliste võrk: 192.168.3.0/24 (256 aadressi)

## 6. Korduma kippuvad küsimused

**K: Kas ma peaksin eelistama WiFi-t või kaabliga ühendust?**
V: Statsionaarsete ja suurt andmeedastust vajavate seadmete jaoks (arvuti, TV, mängukonsool) on parem kaabliga ühendus. Mobiilsete seadmete jaoks on WiFi mugavam.

**K: Kuidas tagada, et IoT seadmed ei ohusta minu arvutite turvalisust?**
V: Parim lahendus on IoT seadmed paigutada eraldi võrku (VLAN või eraldi WiFi SSID), millel pole ligipääsu sinu peamiste seadmete võrku.

**K: Kui tihti peaks uuendama võrguseadmete tarkvara?**
V: Kontrolli uuendusi vähemalt kord kvartalis või luba automaatsed uuendused, kui see on võimalik.

**K: Kas mul on vaja VPN-i oma koduvõrgus?**
V: VPN ei ole koduvõrgus hädavajalik, kuid see võib olla kasulik, kui soovid koduvõrku turvaliselt ühenduda väljastpoolt või kui soovid täiendavat privaatsuskihti.

## 7. Kokkuvõte

Nutikodu võrgu planeerimisel on olulised järgmised aspektid:
- Sobiva võrgutopoloogia valimine vastavalt kodu suurusele ja kujule
- WiFi levialade optimeerimine õige paigutuse ja vajadusel võimendajatega
- IP-aadresside loogiline ja tulevikukindel planeerimine
- Võrgu segmenteerimine erinevate seadmete jaoks
- Turvalisuse tagamine kõigil tasanditel

Hästi planeeritud võrk tagab, et kõik nutikodu seadmed töötavad sujuvalt ja turvaliselt nii praegu kui ka tulevikus.
