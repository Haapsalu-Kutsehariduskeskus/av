# Lab_21: VLAN ja Trunk-ühenduste Seadistamine Klass 310
## Kahe Kommutaatoriga Laboratoorne Töö

Selles praktikumis vaatleme kus ja miks on VLAN-e (virtuaalne kohtvõrk) vaja ja kuidas neid rakendada. Samuti õpime seadistama trunk-ühendusi kommutaatorite vahel.

### Võrgu Topoloogia

```mermaid
graph LR
    PCA[PC-A] --- |F0/6| S1
    S1[Switch S1] === |F0/1 Trunk| S2[Switch S2]
    S2 --- |F0/6| PCB[PC-B]
    
    style PCA fill:#f9f,stroke:#333,stroke-width:2px
    style PCB fill:#f9f,stroke:#333,stroke-width:2px
    style S1 fill:#bbf,stroke:#333,stroke-width:2px
    style S2 fill:#bbf,stroke:#333,stroke-width:2px
```

### IP-aadresside Konfiguratsiooni Tabel

| Seade  | Liides    | IP-aadress    | Võrgumask       | Vaikelüüs      |
|--------|-----------|---------------|-----------------|-----------------|
| S1_*   | VLAN1     | 192.168.1.11  | 255.255.255.0   | -               |
| S2_*   | VLAN1     | 192.168.1.12  | 255.255.255.0   | -               |
| PC-A   | NIC       | 192.168.10.3  | 255.255.255.0   | 192.168.10.1    |
| PC-B   | NIC       | 192.168.10.4  | 255.255.255.0   | 192.168.10.1    |

_* - Kasuta oma eesnime (ilma täpitähtedeta, nt "Tõnu" asemel "Tonu" või "T6nu", liitnimede korral jäta tühik vahelt ära)._

## Töö Käik

### 1. Kaablite Ühendamine ja Esialgne Juurdepääs

1. Ühenda kaablid vastavalt topoloogiajoonisele.
   - Ühenda PC-A kommutaatori S1 porti F0/6
   - Ühenda PC-B kommutaatori S2 porti F0/6
   - Ühenda kommutaatorid S1 ja S2 omavahel (port F0/1 mõlemal kommutaatoril)
   - Ühenda konsoolikaabel oma arvutist kommutaatorisse

2. Ühendu kommutaatoriga üle konsoolikaabli:
   - Ava Putty programm ja ühendu üle "Serial" ühendustüübi
   - Seadista parameetrid: Speed = 9600; Data bits = 8; Stop bits = 1; Parity = None; Flow control = None

### 2. Kommutaatorite Põhiseadistused

Tee järgmised seadistused mõlemas kommutaatoris (S1 ja S2).

1. Seadista kommutaatori nimi vastavalt seadmele ja oma nimele:
   ```
   switch(config)# hostname S1_SinuNimi
   ```

2. Lülita välja kasutamata pordid turvalisuse eesmärgil:
   ```
   S1_SinuNimi(config)# interface range f0/2-5, f0/7-24, g0/1-2
   S1_SinuNimi(config-if-range)# shutdown
   ```

3. Lülita välja DNS-päringu teostamine:
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

### 3. Arvutite Seadistamine

1. Seadista PC-A ja PC-B IP-aadressid vastavalt tabelile:
   - PC-A: IP: 192.168.10.3, mask: 255.255.255.0, vaikelüüs: 192.168.10.1
   - PC-B: IP: 192.168.10.4, mask: 255.255.255.0, vaikelüüs: 192.168.10.1

### 4. Põhilised Ühenduvuse Testid

1. Proovi pingida arvutist arvutisse.
2. Proovi pingida arvutist kommutaatoreid.
3. Proovi kommutaatorist pingida arvuteid ja teist kommutaatorit.

_Küsimus: Kas pingid peaksid õnnestuma? Miks?_

### 5. VLAN-ide Uurimine ja Loomine

1. Vaata VLAN-ide infot:
   ```
   S1_SinuNimi# show vlan
   S1_SinuNimi# show vlan brief
   ```

2. Loo järgmised VLAN-id mõlemas kommutaatoris:
   ```
   S1_SinuNimi(config)# vlan 10
   S1_SinuNimi(config-vlan)# name Operations
   S1_SinuNimi(config-vlan)# vlan 20
   S1_SinuNimi(config-vlan)# name Parking_Lot
   S1_SinuNimi(config-vlan)# vlan 99
   S1_SinuNimi(config-vlan)# name Management
   S1_SinuNimi(config-vlan)# vlan 1000
   S1_SinuNimi(config-vlan)# name Native
   S1_SinuNimi(config-vlan)# end
   ```

3. Vaata uuesti VLAN-ide infot:
   ```
   S1_SinuNimi# show vlan brief
   ```

_Küsimus: Mis on vaikimisi VLAN? Millised pordid on määratud vaike-VLAN-i?_

### 6. Portide Määramine VLAN-idesse

1. Määra PC-A ühendusport VLAN 10-sse (S1):
   ```
   S1_SinuNimi(config)# interface f0/6
   S1_SinuNimi(config-if)# switchport mode access
   S1_SinuNimi(config-if)# switchport access vlan 10
   ```

2. Määra PC-B ühendusport VLAN 10-sse (S2):
   ```
   S2_SinuNimi(config)# interface f0/6
   S2_SinuNimi(config-if)# switchport mode access
   S2_SinuNimi(config-if)# switchport access vlan 10
   ```

### 7. Haldus-VLAN-i Seadistamine

1. S1 kommutaatoris:
   ```
   S1_SinuNimi(config)# interface vlan 1
   S1_SinuNimi(config-if)# no ip address
   S1_SinuNimi(config-if)# interface vlan 99
   S1_SinuNimi(config-if)# ip address 192.168.1.11 255.255.255.0
   S1_SinuNimi(config-if)# end
   ```

2. S2 kommutaatoris:
   ```
   S2_SinuNimi(config)# interface vlan 1
   S2_SinuNimi(config-if)# no ip address
   S2_SinuNimi(config-if)# interface vlan 99
   S2_SinuNimi(config-if)# ip address 192.168.1.12 255.255.255.0
   S2_SinuNimi(config-if)# end
   ```

3. Kontrolli liideste staatust:
   ```
   S1_SinuNimi# show ip interface brief
   ```

_Küsimus: Mis seisus on VLAN 99?_

### 8. Ühenduvuse Testimine

1. Kas S1 saab pingida S2 (ja vastupidi)? Selgita põhjuseid.
2. Kas PC-A saab pingida PC-B (ja vastupidi)? Selgita põhjuseid.
3. Kas S1 ja S2 saavad pingida PC-A ja PC-B (ja vastupidi)? Selgita põhjuseid.

### 9. VLAN-ide Portide Haldamine

1. Tõsta pordid 11-24 ümber VLAN 99 alla (mõlemas kommutaatoris):
   ```
   S1_SinuNimi(config)# interface range f0/11-24
   S1_SinuNimi(config-if-range)# switchport mode access
   S1_SinuNimi(config-if-range)# switchport access vlan 99
   S1_SinuNimi(config-if-range)# end
   ```

2. Kontrolli, kas käsud rakendusid:
   ```
   S1_SinuNimi# show vlan brief
   ```

3. Tõsta pordid F0/11 ja F0/21 ümber VLAN 10 alla:
   ```
   S1_SinuNimi(config)# interface range f0/11, f0/21
   S1_SinuNimi(config-if-range)# switchport access vlan 10
   S1_SinuNimi(config-if-range)# end
   ```

4. Eemalda pordilt F0/24 VLAN 99-i määramine:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# no switchport access vlan
   S1_SinuNimi(config-if)# end
   ```

5. Kontrolli VLAN-i muudatusi:
   ```
   S1_SinuNimi# show vlan brief
   ```

_Küsimus: Kus VLAN-i all F0/24 on nüüd määratud?_

6. Proovi määrata port VLAN-ile, mida ei eksisteeri:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# switchport access vlan 30
   ```

_Küsimus: Kas oli mingeid teateid? Kontrolli VLAN-de määramisi, mida märkad F0/24 ja VLAN 30 kohta?_

7. Eemalda VLAN 30 VLAN-de nimistust:
   ```
   S1_SinuNimi(config)# no vlan 30
   S1_SinuNimi(config)# end
   ```

8. Kontrolli VLAN-de määranguid:
   ```
   S1_SinuNimi# show vlan brief
   S1_SinuNimi# show interfaces f0/24 switchport
   ```

9. Eemalda VLAN määrang pordilt F0/24:
   ```
   S1_SinuNimi(config)# interface f0/24
   S1_SinuNimi(config-if)# no switchport access vlan
   ```

_Küsimus: Kuhu määratakse port pärast VLAN määrangu eemaldamist?_

### 10. 802.1Q Trunk Seadistamine

1. Seadista port F0/1 kommutaatoris S1 kasutama DTP-d läbirääkimiseks:
   ```
   S1_SinuNimi(config)# interface f0/1
   S1_SinuNimi(config-if)# switchport mode dynamic desirable
   ```

2. Kontrolli VLAN-de määramisi mõlemas kommutaatoris:
   ```
   S1_SinuNimi# show vlan brief
   ```

3. Kontrolli trunk-porte mõlemas kommutaatoris:
   ```
   S1_SinuNimi# show interfaces trunk
   ```

4. Testi ühenduvust kõikide osapoolte vahel:
   - S1 <=> S2
   - PC-A <=> PC-B
   - PC-A <=> S1
   - PC-B <=> S2

5. Seadista trunk laad käsitsi mõlemas kommutaatoris:
   ```
   S1_SinuNimi(config)# interface f0/1
   S1_SinuNimi(config-if)# switchport mode trunk
   ```
   
   ```
   S2_SinuNimi(config)# interface f0/1
   S2_SinuNimi(config-if)# switchport mode trunk
   ```

6. Kontrolli trunk-porte:
   ```
   S1_SinuNimi# show interfaces trunk
   ```

7. Seadista native VLAN trunkil mõlemas kommutaatoris:
   ```
   S1_SinuNimi(config)# interface f0/1
   S1_SinuNimi(config-if)# switchport trunk native vlan 1000
   ```
   
   ```
   S2_SinuNimi(config)# interface f0/1
   S2_SinuNimi(config-if)# switchport trunk native vlan 1000
   ```

8. Vaata uuesti trunk-portide infot:
   ```
   S1_SinuNimi# show interfaces trunk
   ```

_Küsimus: Miks peaks käsitsi sättima pordi trunk laadi, mitte kasutama DTP? Miks peaks tahtma muuta trunki native VLAN-i?_

### 11. Laboratoorse Töö Lõpetamine

1. Kontrolli, kas "vlan.dat" fail eksisteerib flash: kettal:
   ```
   S1_SinuNimi# show flash:
   ```

2. Kustuta "vlan.dat" fail, kui see eksisteerib:
   ```
   S1_SinuNimi# delete vlan.dat
   ```

3. Kontrolli, et "vlan.dat" fail ei eksisteeri enam flash: kettal:
   ```
   S1_SinuNimi# show flash:
   ```

4. Kontrolli, kas seadistus NVRAM-s eksisteerib:
   ```
   S1_SinuNimi# show startup-config
   ```

5. Kustuta NVRAM-s olev seadistus, kui see eksisteerib:
   ```
   S1_SinuNimi# erase startup-config
   ```

6. Kontrolli NVRAM-s olevat seadistust:
   ```
   S1_SinuNimi# show startup-config
   ```

### Aruteluküsimused

1. Mida on vaja, et saaks näiteks VLAN 10-s ja VLAN 99-s olevad hostid omavahel ühenduda?
2. Mis on peamised kasud, mida saadakse VLAN-ide rakendamisega asutuse võrgus?
3. Millistel juhtudel pole mõtet VLAN-e rakendada?
4. Mis eesmärgil kasutatakse erinevaid VLAN-e organisatsiooni võrgus?
5. Miks on oluline seadistada erinevaid VLAN-e kommutaatori portidele?