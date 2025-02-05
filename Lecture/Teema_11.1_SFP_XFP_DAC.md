# 🎯 **Teema 11.1: SFP, SFP+, XFP, DAC moodulite näited**  

🤔 **Miks ei saa lihtsalt kasutada Etherneti kaablit? Miks on vaja SFP, SFP+, XFP ja optilisi mooduleid?**  

### ⚡ **Peamised põhjused, miks Etherneti kaabel ei ole alati parim lahendus**  

| 🔥 **Probleem** | 📡 **Ethernet (RJ45) kaabel** | 🌐 **SFP / Optiline moodul** |
|-----------------|------------------------------|------------------------------|
| **🚀 Kiirus** | Tavaliselt kuni **1G või 10G** (uusimate kaablitega **25G**) | Toetab **10G, 25G, 40G, 100G, 400G+** ⚡ |
| **📏 Kaugus** | Maksimaalselt **100m** (CAT6A, CAT7, CAT8) | **Multimood** (500m–2km) 📡 **Ühemood** (kuni 80km+) 🌍 |
| **⚡ Häired (EMI - elektromagnetiline interferents)** | Suured häired, eriti tööstuskeskkonnas ⚠️ | Optiline kiud **ei ole elektromagnetiliste häirete suhtes tundlik** 🛡️ |
| **⏳ Latentsus (viivitus)** | **Kõrgem latentsus** kui optilisel ühendusel ⏳ | **Madalam latentsus**, eriti kõrgemate kiiruste puhul ⚡ |
| **🛠️ Kaabli paindlikkus** | **Paksud ja jäigad kaablid** võivad olla probleemiks 🏋️ | **Õhukesed optilised kaablid** on kergemini juhitavad ja paindlikud 🧵 |
| **🏢 Andmekeskuse kasutus** | Tihedalt pakitud serveririiulites **keeruline kasutada** 😰 | **Optilised ühendused ja DAC** on andmekeskustes eelistatud 🏬 |
| **🔋 Võimsuskulu** | Ethernet (RJ45) võib **tarbida rohkem energiat** suurematel kiirustel ⚡🔌 | **SFP+ ja optilised lahendused võivad olla energiasäästlikumad** 🌱 |

---

## ✅ **Millal kasutada Etherneti kaablit?**  
✔️ Kui **kaugus on ≤100m** ja kiirus **1G või 10G**.  
✔️ Kui **kaablite paigaldus on lihtne** ja **pole vaja väga kiireid või kaugeid ühendusi**.  
✔️ Kui elektromagnetilised häired **ei ole probleemiks** ⚡.  

## ✅ **Millal kasutada SFP/SFP+ mooduleid või optilisi lahendusi?**  
🔹 Kui **kaugus on üle 100m** ja **Etherneti kaabel ei ulatu**.  
🔹 Kui on vaja **väiksemat latentsust** ja **suuremat kiirust (25G, 40G, 100G+)**.  
🔹 Kui keskkond tekitab palju **elektromagnetilisi häireid** (tehased 🏭, lennujaamad ✈️, andmekeskused 🏢).  
🔹 Kui **tulevikus on vaja võrgu kiirust suurendada**, sest **optika on tulevikukindlam** 🚀.  

---

💡 **Lühidalt:** Etherneti kaabel on **lihtne ja odav lühikeste ühenduste jaoks** 🏠, kuid **kõrgema kiiruse ja pikemate vahemaade puhul on optilised moodulid ja SFP lahendused paremad** 🌍⚡.  

---

### **Vaskmoodulid (Copper SFP)**

See on SFP vaskmoodul, mis on mõeldud gigabitisele ühendusele. Samasugune on ka Juniperi vastav moodul – gigabitine, samuti vaskühenduseks.  

Moodulite eemaldamisel tuleb olla ettevaatlik – mõnikord need jäävad pesasse kinni, eriti vaskmoodulid. Optilised moodulid kipuvad vähem kinni jääma. Kui moodul kinni jääb, tuleb seda ettevaatlikult eemaldada, kasutades klambrit, mitte tangidega jõuga välja tõmmates, sest see võib kahjustada luku mehhanismi.  

Milleks on üldse vaja vaskmooduleid? Kui teil on switch, mille kõik pordid on SFP või SFP+ tüüpi, siis võib vaskmoodul olla kasulik. Kui sellist switchi pole, ei pruugi need teie jaoks nii olulised olla.  

[![Copper SFP](https://www.sopto.com.cn/upload/201905/31/201905311714353271.png)](https://www.sopto.com.cn/upload/201905/31/201905311714353271.png)

### **Lühimaa optilised moodulid**
Järgmiseks vaatame optilisi mooduleid, alustades lühimaa variandist. Siin on Cisco SX MM moodul, mis on multimood-ühenduseks, suudab töötada umbes 400–500 meetri peal ja hea kaabli puhul isegi kuni 600 meetrit.  

SX moodulid on tavaliselt multimoodilised ja mõeldud lühikesele vahemaale. Need võivad olla erinevatelt tootjatelt, näiteks Hiina tootjad pakuvad palju SX mooduleid odavama hinnaga.  

[![Cisco SX MM Module](https://www.almiriatechstore.co.ke/wp-content/uploads/2020/06/Cisco-GLC-SX-MM-SFP-Module-scaled.jpg)](https://www.almiriatechstore.co.ke/wp-content/uploads/2020/06/Cisco-GLC-SX-MM-SFP-Module-scaled.jpg)

### **Pikamaa optilised moodulid**
Kui vaatame pikema vahemaa mooduleid, siis siin on Cisco LX moodul, mis töötab ühemodaalse kiuga kuni 10 km. Tavaliselt tähistab LX 10 km ulatust, kuid on olemas ka 20 km variandid. Veel pikemate vahemaade jaoks on olemas ZX moodulid, mis võivad ulatuda kuni 80 km kaugusele.  

Mõned moodulid on universaalsed – näiteks see LX moodul toetab nii ühe- kui multimood-ühendust (10 km SMF või 550 m MMF). See on kasulik juhul, kui teil on mõnel objektil multimoodkaabel, sest sellist moodulit saab kasutada mõlemal juhul.  

[![Cisco LX Module](https://m.media-amazon.com/images/I/71NEm9hYqHL._AC_SL1500_.jpg)](https://m.media-amazon.com/images/I/71NEm9hYqHL._AC_SL1500_.jpg)

### **Ühilduvusprobleemid**
Kui kasutada erinevate tootjate mooduleid (nt Cisco ja Juniper), tuleb tihti mängida ühilduvusseadistustega. Huawei kipub eriti sageli kaebama mitteametlike moodulite peale, kuid sageli saab neid siiski tööle. Tihti on Huawei moodulid ühilduvad H3C moodulitega.  

Kui moodul ei tööta, tuleb kasutada spetsiaalseid käske, et seade aktsepteeriks mitteametlikku moodulit.  

[![Huawei H3C Module](https://www.etulinktechnology.com/js/htmledit/kindeditor-en/attached/20200527/20200527101320_11953.png)](https://www.etulinktechnology.com/js/htmledit/kindeditor-en/attached/20200527/20200527101320_11953.png)

### **10G SFP+ moodulid**
SFP+ moodulid näevad välja sarnaselt SFP moodulitele, kuid SFP moodulid ei tööta SFP+ pesades. Vastupidine ühilduvus on olemas – SFP moodulit saab kasutada SFP+ pesas, kuid ainult gigabitikiirusel.  

Näiteks Intel 10G ühemodaalse kiuga moodul, mis töötab lühikestel vahemaadel, umbes 200 meetrit. Samasugune on ka SFP-10G-SR moodul, mis on mõeldud multimoodkiule ja toetab kuni 500–600 meetrit. Originaalsetel Cisco moodulitel on tavaliselt ka turvakattega hologramm, mis aitab originaalsust määrata.  

[![SFP-10G-SR Module](https://www.tonitrus.com/media/image/97/8d/f4/cisco-sfp-10g-sr-10gbase-sr-sfp-module-10103878-Vm5n.jpg)](https://www.tonitrus.com/media/image/97/8d/f4/cisco-sfp-10g-sr-10gbase-sr-sfp-module-10103878-Vm5n.jpg)

### **DAC kaablid**
10G ühenduste puhul kasutatakse ka DAC (Direct Attach Copper) kaableid. Näiteks SFP-H10GB-CU3M kaabel, mis on 3 meetrit pikk. "Q" tähistab, et see on vaskkaabel. DAC kaableid kasutatakse peamiselt serverite ühendamiseks ja need on saadaval erinevates pikkustes, näiteks 3, 5 või 7 meetrit.  

On olemas ka aktiivsed DAC kaablid, mis sisaldavad optilist signaalitöötlust ja suudavad töötada paremini pikema aja jooksul. Passiivsed DAC kaablid võivad mõnikord halvasti töötada, eriti suuremates andmekeskustes.  

[![SFP-H10GB-CU3M Cable](https://m.media-amazon.com/images/I/81jx2UBzBXL._AC_SL1500_.jpg)](https://m.media-amazon.com/images/I/81jx2UBzBXL._AC_SL1500_.jpg)

### **Breakout kaablid**
Breakout kaablid jagavad ühe suurema ühenduse mitmeks väiksemaks. Näiteks 40G ühendus saab jagada neljaks 10G ühenduseks või 100G ühendus neljaks 25G ühenduseks. Neid kasutatakse peamiselt andmekeskustes, kuid minu enda töökohtades neid ei kohta.  

![Breakout Cable](https://media.startech.com/cms/products/gallery_large/qsfp4x10ao15.main.jpg)

### **XFP moodulid**
Lõpetuseks vaatame XFP mooduleid. Need on haruldasemad ja neid leidub enamasti teenusepakkujate seadmetes, näiteks Juniperi ja mitmesugustes multiplekserites. XFP moodul on 850 nm lainepikkusega ja mõeldud 10G ühenduseks multimoodkiuga.  

[![XFP Modules](https://upload.wikimedia.org/wikipedia/commons/d/d8/Intel_XFP.jpg)](https://upload.wikimedia.org/wikipedia/commons/d/d8/Intel_XFP.jpg)


🔗 **[Watch on YouTube](https://www.youtube.com/watch?v=o8jL1lxdN4M)**

[![YouTube Video](https://img.youtube.com/vi/o8jL1lxdN4M/0.jpg)](https://www.youtube.com/watch?v=o8jL1lxdN4M)