# Lab10: MAC-aadresside õppimine ja kommuteerimise kontroll

## **1. Loo topoloogia:**
- Ava Packet Tracer ja loo järgmine topoloogia:


**PC0 → SW1 → SW2 → PC2**  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;↓  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**PC1**

  - Lisa seadmetele oma nimi. 
  - Kasuta **otsekaableid (straight-through cables)** nii arvutite ja kommutaatorite vahel kui ka kommutaatorite omavahelisteks ühendusteks.

## **2. Määra seadmete IP-aadressid:**
| Seade          | IP-aadress   | Alamvõrgu mask |
|-----------------|--------------|----------------|
| PC0-(Sinu nimi) | 192.168.1.10 | 255.255.255.0  |
| PC1-(Sinu nimi) | 192.168.1.20 | 255.255.255.0  |
| PC2-(Sinu nimi) | 192.168.1.30 | 255.255.255.0  |
| SW1-(Sinu nimi) | Ei vaja IP-d  | -              |
| SW2-(Sinu nimi) | Ei vaja IP-d  | -              |

## **3. Testi ühendusi:**
- Ping PC0 ja PC1 vahel:
  ```bash
  ping 192.168.1.20
  ```
- Ping PC0 ja PC2 vahel:
  ```bash
  ping 192.168.1.30
  ```

## **4. Kontrolli MAC-aadresside tabelit:**
- Ava SW1 ja SW2 CLI-d ning kasuta käsku:
  ```bash
  show mac address-table
  ```
- Vaata, kuidas kommutaatorid MAC-aadresse õpivad ja salvestavad.

---

### **Kohustuslikud ülesanded:**

#### **2 kuvapilti:**
1. **Topoloogia Packet Traceris** (kus on näha seadmete nimed ja ühendused).  
2. **MAC-aadresside tabel** (SW1 ja SW2 puhul, kasutades käsku `show mac address-table`).

---

#### **Vasta küsimustele:**
1. Mida teeb kommutaator, kui MAC-aadressi tabel on tühi?  
2. Mis toimub, kui MAC-aadress on juba tabelis olemas?  
3. Mida näitab MAC-aadresside tabeli käsk `show mac address-table`?  
4. Miks on oluline, et kommutaatorid õpivad MAC-aadresse?
5. 

---

#### **Salvesta ja esita:**
- Salvesta kõik tulemused (sh ekraanipildid).
- Kui sa jõudsid siia lõppu, kirjuta oma lehe lõppu vastus järgmisele küsimusele:
"Mis on Mario lemmik toit ja miks?"
- Esita ülesanne Google Classroomis.