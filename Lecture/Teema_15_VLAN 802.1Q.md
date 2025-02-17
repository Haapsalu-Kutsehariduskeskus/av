# Teema 15: Mis on VLAN 802.1Q ?

### **1. Miks on VLAN vajalik ja mis on selle ideoloogia?**  
VLAN (Virtual Local Area Network) on tehnoloogia, mis võimaldab jagada füüsilise võrgu loogilisteks segmentideks, pakkudes järgmist:  
✅ **Liikluse isoleerimine** – seadmed eri VLAN-ides ei näe üksteist ilma marsruutimiseta.  
✅ **Turvalisus** – saab piirata juurdepääsu seadmete vahel.  
✅ **Liikluse optimeerimine** – vähendab laialdaste edastuste (broadcast) koormust.  
✅ **Paindlikkus** – seadmed võivad olla füüsiliselt ühes võrgus, kuid loogiliselt eraldatud.

---

### **2. Kuidas kommutaatorid töötlevad 802.1Q siltidega kaadreid?**  
Kui Etherneti kaader liigub läbi 802.1Q-toega võrgu, võib see olla kas:  
🔹 **Sildistatud (Tagged)** – kaader sisaldab 802.1Q sildi (VLAN ID), mis määrab, millisesse VLAN-i see kuulub.  
🔹 **Sildistamata (Untagged)** – kaader ei sisalda VLAN ID-d ja edastatakse **Native VLAN-i**.  

- **Tavaline port (Access port)** – aktsepteerib ainult sildistamata kaadreid ja määrab need kindlale VLAN-ile.  
- **Trunk port** – aktsepteerib mitme VLAN-i liiklust, sildistades ja eemaldades VLAN ID vastavalt vajadusele.  

---

### **3. Mitu VLAN-i ühes kaablis ja miks ühel kutsutakse seda trunkiks, teisel taggediks?**  
- **Cisco kasutab terminit "Trunk"**, mis tähendab, et üks port suudab edastada liiklust mitmest VLAN-ist.  
- **Teised tootjad (näiteks HP, Juniper) kasutavad "Tagged" ja "Untagged"**, kus "Tagged" kaadrid kuuluvad kindlasse VLAN-i, "Untagged" aga vaikimisi VLAN-i.

Trunk (või tagged) võimaldab mitme VLAN-i liiklust edastada ühe füüsilise lingi kaudu, näiteks switchide vahel.

---

### **4. VLAN-i seadistamine Cisco seadmetes**  
Siin on mõned põhikäsud VLAN-i loomiseks ja määramiseks:

📌 **VLAN-i loomine ja määramine pordile**  
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config)# interface FastEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
```

📌 **Trunk-pordi seadistamine**  
```bash
Switch(config)# interface GigabitEthernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
```

📌 **VLAN-de nägemine ja trunk-pordi kontrollimine**  
```bash
Switch# show vlan brief
Switch# show interfaces trunk
```

---

### **5. Teenusepakkujate spetsiifika: Private VLAN, Q-in-Q**  
- **Private VLAN (PVLAN)** – võimaldab piirata hostide omavahelist suhtlust isegi samas VLAN-is.  
- **Q-in-Q (VLAN stacking)** – lisab **topelt VLAN-sildi**, et võimaldada mitme VLAN-i ülekanne läbi teenusepakkuja võrgu.

**Näide Q-in-Q seadistamisest:**
```bash
Switch(config)# interface GigabitEthernet 0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport trunk allowed vlan add 100
Switch(config-if)# switchport vlan stacking enable
```

---

### **6. Kodutöö: Packet Tracer - VLAN**  
Soovitan harjutada Cisco Packet Traceris järgmisi teemasid:  
✅ VLAN loomine ja määramine  
✅ Trunk-pordi seadistamine  
✅ Marsruutimine VLAN-ide vahel (ROAS - Router on a Stick)  
✅ Native VLAN ja VLAN ID sildistamine  

Loodan, et see aitab! 🚀 Kui on täpsemaid küsimusi, anna teada!