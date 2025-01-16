# Teema 7.1: Ethernet-kaablite tüübid ja kategooriad. Cat3, Cat5, Cat6, UTP, FTP, S/FTP

## 1. Keerdpaarkaabli põhimõtted

Keerdpaarkaabel (Twisted Pair) on võrgukaabel, mis koosneb neljast keerdpaarist juhtmetest. Iga paar on erineva keerdesammuga, mis aitab vähendada elektromagnetilist interferentsi.

### Kaabli põhikomponendid:
- Väliskest (PVC)
- HDPE isolatsioon
- Rebimiskindel niit
- Vaskjuhtmed

## 2. Kaablite tüübid varjestuse järgi

### UTP (Unshielded Twisted Pair)
- Varjestuseta keerdpaarkaabel
- Kõige levinum tüüp
- Sobilik tavakasutuseks siseruumides

![UTP vs STP comparison](https://images.javatpoint.com/tutorial/computer-network/images/utp-vs-stp2.png)

### FTP (Foiled Twisted Pair)
- Fooliumvarjestusega keerdpaarkaabel
- Parem kaitse väliste häirete eest
- Soovitatav keskkondades, kus esineb elektromagnetilist müra

![FTP Cable](https://www.tdk.co.ke/wp-content/uploads/2019/09/giganet-cat6a-Ethernet-Cable-prices-in-kenya.jpg)

### STP (Shielded Twisted Pair)
- Põimitud varjestusega keerdpaarkaabel
- Väga hea kaitse häirete eest
- Kasutatakse tööstuskeskkondades

![STP Cable](https://images.javatpoint.com/tutorial/computer-network/images/utp-vs-stp3.png)

### S/FTP (Screened Foiled Twisted Pair)
- Kombineeritud varjestusega (foolium + põimitud varjestus)
- Maksimaalne kaitse häirete eest
- Kasutatakse kriitilistes keskkondades

![S/FTP Cable](https://cdn.shopify.com/s/files/1/0613/4041/8306/files/understandand_copper_cabling.jpg?v=1648798820)

## 3. Kaablite kategooriad

![Cable Categories Overview](https://howevision.com/wp-content/uploads/2021/07/What-Cable-Should-I-Choose-for-10GB-Ethernet_-600x450.png)

### Cat3
- Sagedus: 16 MHz
- Kasutus: Peamiselt telefoniliinideks
- Andmeedastuskiirus: kuni 10 Mbit/s

![Cat3Cable](https://networkencyclopedia.com/wp-content/uploads/2019/09/category-3-cabling.jpg)

### Cat5
- Sagedus: 100 MHz
- Kasutus: Fast Ethernet (100BASE-TX)
- Andmeedastuskiirus: kuni 100 Mbit/s


### Cat5e
- Sagedus: 100 MHz
- Kasutus: Gigabit Ethernet (1000BASE-T)
- Täiustatud spetsifikatsioonid võrreldes Cat5-ga
- Kõige levinum kategooria tänapäeval

![Cat5e Cable](https://res.cloudinary.com/hdtsjhzsw/image/upload/s--XCqqODQP--/c_fit%2Cw_750%2Ch_500/w1cmor1rkixq7tkkpgx1.webp)

### Cat6
- Sagedus: 250 MHz
- Kasutus: 10 Gigabit Ethernet (10GBASE-T)
- Sisaldab eraldajat (spline) paremaks signaaliedastuseks
- Maksimaalne pikkus 10 Gbit/s korral: 55m

![Cat6 Cable](https://res.cloudinary.com/hdtsjhzsw/image/upload/s--V-2chbJm--/c_fit%2Cw_750%2Ch_500/hcp1uvxvbq7dbjadubgd.webp)

### Cat6a
- Sagedus: 500 MHz
- Kasutus: 10 Gigabit Ethernet täispikkuses
- Maksimaalne pikkus 10 Gbit/s korral: 100m

![Cat6a Cable](https://res.cloudinary.com/hdtsjhzsw/image/upload/s--8Oc_BXTw--/c_fit%2Cw_750%2Ch_500/jqs1ebth5u7zg0lhw5c2.webp)

## 4. Praktilised soovitused

### Kaabli valimine
- Siseruumidesse tavaliselt UTP Cat5e või Cat6
- Välistingimustesse varjestatud kaablid (FTP/STP)
- Tööstuskeskkonda S/FTP Cat6a või kõrgem

![Cable Selection Guide](https://planetechusa.com/wp-content/uploads/2020/07/image4.jpg)

### Paigaldamine
- Maksimaalne soovitatav pikkus: 100m
- Vältige teravaid paindeid
- Hoidke eemal elektrikaablitest
- Kasutage kvaliteetseid konnektoreid

### Testimine ja veaotsing
- Kasutage kaablitesterit
- Kontrollige konnektorite ühendusi
- Veenduge õiges pingepaaris (pinout)
- Jälgige maksimaalset lubatud pikkust

## 5. Kaabli märgistused ja standardid

- ISO/IEC 11801 rahvusvaheline standard
- ANSI/TIA/EIA Ameerika standard
- Kaablil peab olema märgitud:
  - Kategooria
  - Varjestuse tüüp
  - Tootja
  - Meetritähised

### Patch Panel
Patch panel on efektiivne ja paindlik võrgu seade, mis aitab andmekeskuse või serveriruumi korrastada ning muudab kaabelduse tõstmist, lisamist või muutmist tulevikus palju lihtsamaks. Iga port ühendub patch-kaabli kaudu teise pordiga hoone sees. Ärikasutuses võimaldab patch panel kiirelt suhelda ühest kontorist teise.

![Patch Panel Overview](https://cdn.shopify.com/s/files/1/0613/4041/8306/files/patch_panel.jpg?v=1650447348)
![Patch Panel Wiring](https://www.firefold.com/cdn/shop/articles/How_to_Wire_a_Patch_Panel_1024x1024.jpg?v=1554997145)

### Full Rack ja Half Rack
![Rack Comparison](https://www.cablematters.com/blog/image.axd?picture=/ArticlePhotos/PatchPanel/Patch-Panels-Guide_3.jpg)

### Fibers
![10G Fiber](https://media.fs.com/images/community/upload/kindEditor/202102/27/breakout-patch-panel-1614396831-vGZPK27w7D.jpg)

### Labeling
![MTU Structure](./media/labeling.png)
