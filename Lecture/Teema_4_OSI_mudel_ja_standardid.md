# **Teema 4: Standardid ja organisatsioonid. OSI mudel. Kus seda kasutatakse?**

## **Sisu**

1. [Standardid ja organisatsioonid](#standardid-ja-organisatsioonid)  
   - [ISO](#iso)  
   - [IEEE](#ieee)  
   - [EIA](#eia)  
   - [Interneti standardid](#interneti-standardid)  
2. [OSI mudel](#osi-mudel)  
   - [OSI kihid ja nende funktsioonid](#osi-kihid-ja-nende-funktsioonid)  
   - [OSI mudeli analoogia arvutiprogrammidega](#osi-mudeli-analoogia-arvutiprogrammidega)  
3. [OSI mudeli kihid](#osi-mudeli-kihid)  
   - [Kihi 0 – Füüsiline keskkond](#kihi-0--füüsiline-keskkond)  
   - [Kihi 1 – Füüsiiline kiht](#kihi-1--füüsiiline-kiht)  
   - [Kihi 2 – Kanalikiht (MAC ja LLC)](#kihi-2--kanalikiht-mac-ja-llc)  
   - [Kihi 3 – Võrgukiht](#kihi-3--võrgukiht)  
   - [Kihi 4 – Transpordikiht](#kihi-4--transpordikiht)  
   - [Kihi 5–7](#kihi-57)  
4. [Populaarsed protokollide virnad](#populaarsed-protokollide-virnad)  
   - [OSI vs TCP/IP (DoD) mudel](#osi-vs-tcpip-dod-mudel)  
5. [Kokkuvõte ja praktiline tähtsus](#kokkuvõte-ja-praktiline-tähtsus)  
   - [Kus seda kasutatakse?](#kus-seda-kasutatakse)  
6. [Selgitavad näited ja täiendavad seletused](#selgitavad-näited-ja-täiendavad-seletused)  
   - [Miks OSI mudel loodi hiljem kui TCP/IP](#miks-osi-mudel-loodi-hiljem-kui-tcpip)  
   - [Praktilised näited L2 ja L3 seadmetest](#praktilised-näited-l2-ja-l3-seadmetest)  
   - [Rakenduskiht ja selle erinevused TLS/HTTPS puhul](#rakenduskiht-ja-selle-erinevused-tlshttps-puhul)  

---

## 1. Standardid ja organisatsioonid

### ISO (Rahvusvaheline Standardiorganisatsioon)
ISO on rahvusvaheline standardiorganisatsioon, mis hõlmab erinevaid rahvuslikke standardiorganisatsioone. Selle suurim panus on OSI (Open Systems Interconnection) mudeli loomine, mis standardiseerib avatud süsteemide koostoimimist.

### IEEE (Elektri- ja elektroonikainseneride instituut)
IEEE on USA päritolu standardiorganisatsioon, mis kehtestab peamiselt võrgu ja elektrotehnika standardeid. Näiteks:

- IEEE 802.3 – Ethernet
- IEEE 802.11 – Wi-Fi

### EIA (Elektroonikatööstuse Assotsiatsioon)
EIA on USA kaubandusorganisatsioon, mis arendab standardeid juhtmete ja ühenduste jaoks, nt RS-232C ja EIA-568.

### Interneti standardid
- **ISOC** – Internet Society tegeleb interneti infrastruktuuri arenguga.
- **IETF** – Internet Engineering Task Force arendab interneti protokolle ja tehnilisi lahendusi.
- **RFC** (Request for Comments) – tehnilised dokumendid, mis kirjeldavad võrgu ja interneti standardeid.

![Network Standards](https://www.signalintegrityjournal.com/ext/resources/article-images-2019/Why-Are-There-So-Many-Standards/T5.jpg)

Network standards ensure compatibility and interoperability between devices and systems in a networked environment. They are crucial for seamless communication and technology integration.  
Source: [Why Are There So Many Standards?](https://www.signalintegrityjournal.com/articles/1339-why-are-there-so-many-standards)


### Question:
What is an RFC (Request for Comments), and why is it important in the context of networking and internet standards?
[April Fools' Day Request for Comments](https://en.wikipedia.org/wiki/April_Fools%27_Day_Request_for_Comments)

---

![OSI Model](https://www.networkworld.com/wp-content/uploads/2024/07/964816-0-60870500-1721321266-OSI_7_layers_shutterstock_2483259351.jpg?resize=1024%2C576&quality=50&strip=all)

The OSI (Open Systems Interconnection) model is a conceptual framework used to understand and implement network communication. It divides the process into seven layers, each with specific functions to ensure efficient and interoperable data exchange.  
Source: [Understanding the OSI Model](https://www.networkworld.com/article/964816-understanding-the-osi-model.html)

## 2. OSI mudel

### OSI kihid ja nende funktsioonid
OSI mudel loodi 1984. aastal ja see jagab võrgusuhtluse 7 eraldi kihiks. Peamine eesmärk on tagada kihiti abstraktsioon, et üleminevad kihid ei peaks teadma, kuidas madalamad kihid töötavad.

### OSI mudeli analoogia arvutiprogrammidega
Operatsioonisüsteem (OS) ja programmid on hea analoogia OSI mudelile:

- Programm (rakenduskiht) suhtleb OS-iga, mitte otse riistvaraga.
- OSi kihid (draiverid) peidavad riistvara keerukuse.
- Sama loogika kehtib OSI mudeli puhul: andmete saatmine ja vastuvõtt on kihtidena eraldatud.

---

![OSI Model](https://media.geeksforgeeks.org/wp-content/uploads/20241111182857579134/OSI-Model.gif)

This animated GIF illustrates the OSI (Open Systems Interconnection) model, showcasing the functions of its seven layers in a dynamic and visually engaging way.  
Source: [GeeksforGeeks - OSI Model](https://www.geeksforgeeks.org/osi-model/).

## 3. OSI mudeli kihid
(
    <div className="w-full max-w-4xl mx-auto p-4">
      <table className="w-full border-collapse">
        <thead>
          <tr className="bg-gray-100">
            <th className="border p-2 text-left w-16">Kiht</th>
            <th className="border p-2 text-left w-32">Nimi</th>
            <th className="border p-2 text-left">Põhifunktsioonid</th>
          </tr>
        </thead>
        <tbody>
          <tr className="bg-gray-50">
            <td className="border p-2">0*</td>
            <td className="border p-2">Füüsiline keskkond</td>
            <td className="border p-2">Kaablid, pistikud ja raadiosignaalid. (Mitteametlik kiht)</td>
          </tr>
          <tr>
            <td className="border p-2">1</td>
            <td className="border p-2">Füüsiline kiht</td>
            <td className="border p-2">Elektrisignaalide, pingete ja sageduste edastamine</td>
          </tr>
          <tr className="bg-gray-50">
            <td className="border p-2">2</td>
            <td className="border p-2">Kanalikiht</td>
            <td className="border p-2">Usaldusväärne andmevahetus (MAC - meediumile juurdepääsu kontroll, LLC - loogilise lingi haldus)</td>
          </tr>
          <tr>
            <td className="border p-2">3</td>
            <td className="border p-2">Võrgukiht</td>
            <td className="border p-2">Pakettide marsruutimine ja suunamine (IP protokoll)</td>
          </tr>
          <tr className="bg-gray-50">
            <td className="border p-2">4</td>
            <td className="border p-2">Transpordikiht</td>
            <td className="border p-2">Andmeedastuse kontroll (UDP, TCP protokollid)</td>
          </tr>
          <tr>
            <td className="border p-2">5</td>
            <td className="border p-2">Seansikiht</td>
            <td className="border p-2">Seansside loomine ja lõpetamine</td>
          </tr>
          <tr className="bg-gray-50">
            <td className="border p-2">6</td>
            <td className="border p-2">Esitluskiht</td>
            <td className="border p-2">Andmete süntaks ja semantika</td>
          </tr>
          <tr>
            <td className="border p-2">7</td>
            <td className="border p-2">Rakenduskiht</td>
            <td className="border p-2">Kasutajale nähtavad teenused (failiedastus, e-post jne)</td>
          </tr>
        </tbody>
      </table>
      <div className="text-sm text-gray-600 mt-2">* Kiht 0 ei kuulu ametlikku OSI mudelisse</div>
    </div>
  )

---

## 4. Populaarsed protokollide virnad

### OSI vs TCP/IP (DoD) mudel
TCP/IP mudel (tuntud ka kui DoD mudel) on lihtsustatud 4-kihiline mudel, mis pärineb Ameerika Ühendriikide Kaitseministeeriumi (Department of Defense) algatustest:

1. **Võrgule ligipääsu kiht** – OSI kihid 1–2.
2. **Interneti kiht** – OSI kiht 3.
3. **Transpordikiht** – OSI kiht 4.
4. **Rakenduse kiht** – OSI kihid 5–7.

![Comparison of DoD Model and OSI Model](https://www.oreilly.com/api/v2/epubs/9781118435250/files/images/ch002-f001.jpg)

The DoD model and the OSI reference model. While both models are similar in concept, they differ in the number of layers and the names assigned to those layers. This comparison helps in understanding how the two frameworks align and differ in networking principles.  
Source: [O'Reilly - Networking for Dummies](https://www.oreilly.com/library/view/networking-for-dummies/9781118435250/).

![OSI Protocols Stack](https://ipwithease.com/wp-content/uploads/2023/10/What-are-OSI-Protocols-7-Network-Layer-Protocols-Explained.jpg.webp)

OSI Protocols Stack  
Source: [IP With Ease - OSI Protocols](https://ipwithease.com/what-are-osi-protocols-7-network-layer-protocols-explained/).

---

### Questions:

1. **OSI mudeli ja TCP/IP standardite kohta**:  
   Miks loodi OSI mudel pärast TCP/IP protokolli ja milliseid piiranguid see kaasa tõi?

2. **Praktilised näited L2 ja L3 seadmetest**:  
   Milline on peamine erinevus L2 kommutaatorite ja L3 ruuterite vahel, ning kuidas need seadmed suunavad liiklust võrgus?


![OSI Model (7 Layers)](https://cf-assets.www.cloudflare.com/slt3lc6tev37/6ZH2Etm3LlFHTgmkjLmkxp/59ff240fb3ebdc7794ffaa6e1d69b7c2/osi_model_7_layers.png)

Each layer of the OSI Model handles a specific job and communicates with the layers above and below itself.
Source: [Cloudflare - OSI Model](https://www.cloudflare.com/learning/network-layer/what-is-the-osi-model/).
