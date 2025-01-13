# **Teema 1: Pakettkommuteeritud ja kanalipõhised võrgud**

## **Sisu**

1. [Sissejuhatus](#sissejuhatus)  
2. [Elementaarkanal](#elementaarkanal)  
3. [Kanalipõhine kommuteerimine](#kanalipõhine-kommuteerimine)  
   - [Näide: Telefonivõrgud](#näide-telefonivõrgud)  
   - [Kanalipõhiste võrkude omadused](#kanalipõhiste-võrkude-omadused)  
4. [Pakettkommuteeritud võrgud](#pakettkommuteeritud-võrgud)  
   - [Pakettide edastamise meetodid](#pakettide-edastamise-meetodid)  
     - [Datagrammi edastus](#datagrammi-edastus)  
     - [Loogilise ühenduse loomisega edastus](#loogilise-ühenduse-loomisega-edastus)  
     - [Virtuaalse kanali loomisega edastus](#virtuaalse-kanali-loomisega-edastus)  
5. [Kommuteerimise meetodite võrdlus](#kommuteerimise-meetodite-võrdlus)

## **Sissejuhatus**

Andmesidevõrgud jagunevad kaheks peamiseks tüübiks:
- **Kanalipõhised võrgud**, kus igale ühendusele eraldatakse püsiv andmekanal.  
- **Pakettkommuteeritud võrgud**, kus andmed jagatakse väiksemateks pakettideks ja edastatakse võrgu kaudu.

Mõlemal meetodil on omad eelised ja puudused, sõltuvalt kasutusjuhtudest.

```mermaid
graph TB
    A[Andmesidevõrgud] --> B[Kanalipõhised võrgud]
    A --> C[Pakettkommuteeritud võrgud]
    B --> D[Püsiv kanal]
    B --> E[Garanteeritud läbilaskevõime]
    C --> F[Paketipõhine edastus]
    C --> G[Dünaamiline ressursikasutus]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bbf,stroke:#333,stroke-width:2px
```

## **Elementaarkanal**

**Elementaarkanal** on kanalipõhise võrgu põhiline tehniline omadus. See esindab kindlaksmääratud läbilaskevõime väärtust, mida kasutatakse abonentide vahelise ühenduse loomiseks.

```mermaid
graph LR
    subgraph Elementaarkanal
    A[Saatja] -->|Fikseeritud läbilaskevõime| B[Vastuvõtja]
    end
    
    style A fill:#98FB98,stroke:#333,stroke-width:2px
    style B fill:#98FB98,stroke:#333,stroke-width:2px
```

**Omadused:**
1. Elementaarkanali läbilaskevõime on fikseeritud.  
2. Komposiitkanal koosneb alati elementaarkanalite täisarvust.  
3. Kasutajale eraldatud elementaarkanaleid ei saa kasutada teised kasutajad kuni ühenduse lõppemiseni.  

## **Kanalipõhine kommuteerimine**

Kanalipõhine kommuteerimine hõlmab spetsiaalse kanali eraldamist kahe abonendi vaheliseks suhtluseks. See kanal jääb reserveerituks kogu seansi kestel, tagades püsiva ja häireteta ühenduse.

```mermaid
sequenceDiagram
    participant A as Kasutaja A
    participant S as Kommutaator
    participant B as Kasutaja B
    
    A->>S: 1. Ühenduse päring
    S->>B: 2. Kutsung
    B->>S: 3. Vastus
    S->>A: 4. Ühenduse kinnitamine
    Note over A,B: 5. Püsiv kanal on loodud
    A->>B: 6. Andmevahetus
```

### **Näide: Telefonivõrgud**  
Telefonivõrk on kanalipõhise kommuteerimise klassikaline näide. Kui kasutaja valib numbri, loob telefonivõrk spetsiaalse kanali, mis jääb kõne ajaks reserveerituks.

### **Kanalipõhiste võrkude omadused**

```mermaid
graph TD
    A[Kanalipõhiste võrkude omadused] --> B[Ressursside eksklusiivsus]
    A --> C[Stabiilsus]
    A --> D[Ressursikasutus]
    A --> E[Ühenduse kontroll]
    
    B --> B1[Üks kanal = üks ühendus]
    C --> C1[Garanteeritud ribalaius]
    C --> C2[Madal latentsus]
    D --> D1[Ebaefektiivne kasutus]
    E --> E1[Võimalik ühenduse keeldumine]
```

1. **Ressursside eksklusiivsus:** Üks kanal on reserveeritud ainult ühe ühenduse jaoks.  
2. **Stabiilsus:** Tagatud on kindel ribalaius ja madal latentsus.  
3. **Ebatõhus ressursikasutus:** Kanalid jäävad kasutamata, kui andmeedastus ei ole pidev.  
4. **Ühenduse loomine:** Võrk võib ühenduse loomisest keelduda, kui kõik kanalid on hõivatud.

## **Pakettkommuteeritud võrgud**

Pakettkommuteeritud võrgus jagatakse andmed väiksemateks osadeks ehk **pakettideks**, mis liiguvad sõltumatult läbi võrgu. Iga pakett sisaldab:
- **Andmeid**  
- **Pealkirja (header)**, kus on allika ja sihtkoha aadress.

### **Paketi struktuur:**

```mermaid
graph LR
    subgraph Pakett
    A[Päis] --> B[Andmed] --> C[Lõpp]
    end
    
    A --> D[Sihtkoha aadress<br>Allika aadress<br>Kontrollandmed]
    
    style A fill:#FFA07A,stroke:#333,stroke-width:2px
    style B fill:#98FB98,stroke:#333,stroke-width:2px
    style C fill:#FFA07A,stroke:#333,stroke-width:2px
```

**Eelised:**  
- Võrguressursse kasutatakse efektiivsemalt.  
- Võimalik edastada mitmeid andmevooge samaaegselt.  
- Suurem tõrketaluvus.

## **Pakettide edastamise meetodid**

### **1. Datagrammi edastus**

- Andmed jagatakse pakettideks, mida töödeldakse iseseisvalt.  
- Ei ole vaja eelnevalt ühendust luua.  
- **Näited:** IP, UDP.  

```mermaid
graph LR
    A[Saatja] --> B((R1))
    B --> C((R2))
    B --> D((R3))
    C --> E[Vastuvõtja]
    D --> E
    
    style A fill:#FFB6C1,stroke:#333,stroke-width:2px
    style E fill:#FFB6C1,stroke:#333,stroke-width:2px
```

**Omadused:**  
- Sobib kiireks ja lihtsaks edastuseks.  
- Ei garanteerita pakettide järjekorda ega kohaletoimetamist.

### **2. Loogilise ühenduse loomisega edastus**

- Loogiline ühendus luuakse enne andmete edastamist.  
- Garanteeritakse andmete järjekord ja terviklikkus.  
- **Näited:** TCP.  

```mermaid
sequenceDiagram
    participant A as Saatja
    participant N as Võrk
    participant B as Vastuvõtja
    
    A->>N: 1. Ühenduse päring
    N->>B: 2. Ühenduse päring
    B->>N: 3. Kinnitus
    N->>A: 4. Ühenduse kinnitus
    A->>B: 5. Andmete edastus
    A->>B: 6. Ühenduse lõpetamine
```

**Protsess:**  
1. Ühenduse loomise päring.  
2. Ühenduse loomise kinnitus.  
3. Andmete edastus.  
4. Ühenduse lõpetamise päring ja kinnitus.

### **3. Virtuaalse kanali loomisega edastus**

- Pakettide liikumiseks määratakse kindel marsruut läbi võrgu.  
- Sobib suure stabiilsusega rakendustele.  
- **Näited:** MPLS, ATM.  

```mermaid
graph LR
    A[Saatja] --> B((R1))
    B --> C((R2))
    C --> D((R3))
    D --> E[Vastuvõtja]
    
    style A fill:#FFB6C1,stroke:#333,stroke-width:2px
    style E fill:#FFB6C1,stroke:#333,stroke-width:2px
    linkStyle 0,1,2,3 stroke:#ff0000,stroke-width:2px
```

**Eelised:**  
- Püsiv marsruut tagab usaldusväärsuse.  
- Efektiivsem võrguressursside haldamine.

## **Kommuteerimise meetodite võrdlus**

| **Omadus** | **Kanalipõhine kommuteerimine** | **Pakettkommuteerimine** |
|------------|--------------------------------|-----------------------|
| Ribalaiuse garantii | ✓ Tagatud | ✗ Ei ole tagatud |
| Ühenduse loomine | ✓ Vajalik | ✗ Pole vajalik |
| Ressursikasutus | ✗ Vähem efektiivne | ✓ Efektiivne pulsseeriva liiklusega |
| Aadressi kasutamine | Ainult ühenduse loomisel | Iga paketiga kaasas |
| Latentsus | ✓ Madal | ✗ Varieeruv |
| Tõrketaluvus | ✗ Madal | ✓ Kõrge |

### **Rakenduste näited**

```mermaid
graph TB
    A[Võrgutehnoloogiad] --> B[Kanalipõhine]
    A --> C[Pakettkommuteerimine]
    
    B --> D[Telefonivõrk]
    B --> E[ISDN]
    B --> F[Raadioside]
    
    C --> G[Internet]
    C --> H[LAN]
    C --> I[VoIP]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bbf,stroke:#333,stroke-width:2px
```