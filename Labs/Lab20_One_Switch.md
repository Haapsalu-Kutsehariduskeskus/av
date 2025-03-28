# Lab_20: VLAN-i Seadistamise Klass 310
## Üks Kommutaator Kahe Arvutiga

Selles praktikumis vaatleme, kuidas seadistada ja kasutada VLAN-e (virtuaalsed kohtvõrgud) lihtsustatud võrgukeskkonnas.

### Võrgu Topoloogia

```mermaid
graph LR
    PCA[PC-A] --- |F0/6| S1[Switch S1]
    S1 --- |F0/7| PCB[PC-B]
    
    style PCA fill:#f9f,stroke:#333,stroke-width:2px
    style PCB fill:#f9f,stroke:#333,stroke-width:2px
    style S1 fill:#bbf,stroke:#333,stroke-width:2px
```

### IP-aadresside Konfiguratsiooni Tabel

| Seade  | Liides    | IP-aadress    | Võrgumask       | Vaikelüüs      |
|--------|-----------|---------------|-----------------|-----------------|
| S1_*   | VLAN99    | 192.168.1.11  | 255.255.255.0   | -               |
| PC-A   | NIC       | 192.168.10.3  | 255.255.255.0   | 192.168.10.1    |
| PC-B   | NIC       | 192.168.20.3  | 255.255.255.0   | 192.168.20.1    |

_* - Kasuta oma eesnime (ilma täpitähtedeta, nt "Tõnu" asemel "Tonu" või "T6nu", liitnimede korral jäta tühik vahelt ära)._

## Töö Käik

### 1. Kaablite Ühendamine ja Esialgne Juurdepääs

1. Ühenda kaablid vastavalt topoloogiajoonisele.
   - Ühenda PC-A kommutaatori S1 porti F0/6
   - Ühenda PC-B kommutaatori S1 porti F0/7
   - Ühenda konsoolikaabel oma arvutist kommutaatorisse

2. Ühendu kommutaatoriga üle konsoolikaabli:
   - Ava PuTTY programm ja ühendu üle "Serial" ühendustüübi
   - Seadista parameetrid: Speed = 9600; Data bits = 8; Stop bits = 1; Parity = None; Flow control = None

### 2. Kommutaatori Põhiseadistused

1. Seadista kommutaatori nimi vastavalt oma nimele:
   ```
   switch(config)# hostname S1_SinuNimi
   ```

2. Lülita välja kasutamata pordid turvalisuse eesmärgil:
   ```
   S1_SinuNimi(config)# interface range f0/1-5, f0/8-24, g0/1-2
   S1_SinuNimi(config-if-range)# shutdown
   ```

3. Lülita välja DNS-päringu teostamine, et vältida viivitusi valesti sisestatud käskude puhul:
   ```
   S1_SinuNimi(config)# no ip domain lookup
   ```

4. Seadista priviligeeritud režiimi parool:
   ```
   S1_SinuNimi(config)# enable secret class
   ```

5. Seadista konsooliliini parool:
   ```
   S1_SinuNimi(config)# line console 0
   S1_SinuNimi(config-line)# password cisco
   S1_SinuNimi(config-line)# login
   ```

6. Seadista VTY-liinide parool Telnet-ühenduse jaoks:
   ```
   S1_SinuNimi(config)# line vty 0 4
   S1_SinuNimi(config-line)# password cisco
   S1_SinuNimi(config-line)# login
   ```

7. Krüpteeri kõik paroolid seadistuses:
   ```
   S1_SinuNimi(config)# service password-encryption
   ```

8. Seadista sisselogimise tervitussõnum:
   ```
   S1_SinuNimi(config)# banner motd $ S1_SinuNimi: Ainult volitatud kasutajad! $
   ```

9. Seadista õige kellaaeg:
   ```
   S1_SinuNimi# clock set TT:MM:SS PÄEV KUU AASTA
   ```

### 3. Põhilised Ühenduvuse Testid

1. Proovi pingida PC-A-lt PC-B-sse.
2. Proovi pingida arvutitest kommutaatorisse.

_Küsimus: Kas pingid õnnestuvad? Miks või miks mitte?_

### 4. Vaikimisi VLAN-i Seadistuste Uurimine

1. Vaata praegust VLAN-ide infot:
   ```
   S1_SinuNimi# show vlan
   S1_SinuNimi# show vlan brief
   ```

_Küsimus: Mis on vaikimisi VLAN? Millised pordid on määratud sellesse?_

### 5. VLAN-ide Loomine

1. Loo järgmised VLAN-id:
   ```
   S1_SinuNimi(config)# vlan 10
   S1_SinuNimi(config-vlan)# name Operations
   S1_SinuNimi(config-vlan)# vlan 20
   S1_SinuNimi(config-vlan)# name Finance
   S1_SinuNimi(config-vlan)# vlan 99
   S1_SinuNimi(config-vlan)# name Management
   S1_SinuNimi(config-vlan)# end
   ```

2. Vaata uuendatud VLAN-ide infot:
   ```
   S1_SinuNimi# show vlan brief
   ```

### 6. Kommutaatori Portide Määramine VLAN-idesse

1. Määra PC-A VLAN 10-sse (Operations):
   ```
   S1_SinuNimi(config)# interface f0/6
   S1_SinuNimi(config-if)# switchport mode access
   S1_SinuNimi(config-if)# switchport access vlan 10
   ```

2. Määra PC-B VLAN 20-sse (Finance):
   ```
   S1_SinuNimi(config)# interface f0/7
   S1_SinuNimi(config-if)# switchport mode access
   S1_SinuNimi(config-if)# switchport access vlan 20
   ```

3. Vaata uuendatud VLAN-ide määranguid:
   ```
   S1_SinuNimi# show vlan brief
   ```

### 7. Haldus-VLAN-i Seadistamine

1. Eemalda IP-aadress VLAN 1-lt:
   ```
   S1_SinuNimi(config)# interface vlan 1
   S1_SinuNimi(config-if)# no ip address
   ```

2. Seadista IP-aadress haldus-VLAN-ile (VLAN 99):
   ```
   S1_SinuNimi(config)# interface vlan 99
   S1_SinuNimi(config-if)# ip address 192.168.1.11 255.255.255.0
   S1_SinuNimi(config-if)# no shutdown
   ```

3. Kontrolli liideste staatust:
   ```
   S1_SinuNimi# show ip interface brief
   ```

_Küsimus: Mis on VLAN 99 staatus? Miks?_

### 8. VLAN-ide Vahelise Ühenduvuse Testimine

1. Proovi uuesti pingida PC-A-lt PC-B-sse.

_Küsimus: Kas ping õnnestub? Miks või miks mitte?_

### 9. VLAN-i Portide Määrangute Haldamine

1. Määra kasutamata pordid "parkimisplatsi" VLAN-i turvalisuse eesmärgil:
   ```
   S1_SinuNimi(config)# interface range f0/8-24
   S1_SinuNimi(config-if-range)# switchport mode access
   S1_SinuNimi(config-if-range)# switchport access vlan 20
   ```

2. Kontrolli muudatusi:
   ```
   S1_SinuNimi# show vlan brief
   ```

3. Muuda konkreetse pordi VLAN-i määrangut:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# switchport access vlan 10
   ```

4. Eemalda VLAN-i määrang pordilt:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# no switchport access vlan
   ```

5. Proovi määrata port olematule VLAN-ile:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# switchport access vlan 30
   ```

_Küsimus: Mis juhtub, kui proovid määrata porti VLAN-ile, mida ei eksisteeri?_

6. Kontrolli pordi detailset konfiguratsiooni:
   ```
   S1_SinuNimi# show interfaces f0/24 switchport
   ```

### 10. VLAN-ide Vahelise Marsruutimise Arutelu

_Küsimus: Mida oleks vaja, et VLAN 10 ja VLAN 20 hostid saaksid omavahel suhelda?_

### 11. Laboratoorse Töö Lõpetamine

1. Kontrolli, kas vlan.dat fail eksisteerib:
   ```
   S1_SinuNimi# show flash:
   ```

2. Kustuta vlan.dat fail, kui see eksisteerib:
   ```
   S1_SinuNimi# delete vlan.dat
   ```

3. Kontrolli, et fail on kustutatud:
   ```
   S1_SinuNimi# show flash:
   ```

4. Kontrolli, kas startup-konfiguratsioon eksisteerib:
   ```
   S1_SinuNimi# show startup-config
   ```

5. Kui see eksisteerib, kustuta see:
   ```
   S1_SinuNimi# erase startup-config
   ```

6. Kontrolli, et startup-konfiguratsioon on kustutatud:
   ```
   S1_SinuNimi# show startup-config
   ```

### Aruteluküsimused

1. Mis on peamised kasud, mida saadakse VLAN-ide rakendamisega organisatsiooni võrgus?
2. Millistel juhtudel ei ole VLAN-ide kasutamine vajalik või kasulik?
3. Miks ei saa erinevates VLAN-ides olevad seadmed vaikimisi omavahel suhelda?
4. Millist võrguseadet oleks vaja, et võimaldada VLAN-ide vahelist suhtlust?