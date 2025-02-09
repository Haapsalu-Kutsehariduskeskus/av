
# Network Interfaces in Linux
### Võrguliidesed Linuxis: Võrgu seadistamine  

Linuxi süsteemi haldamisel on oluline mõista ja konfigureerida võrguliideseid. Olenemata sellest, kas seadistad lihtsat koduserverit või haldad keerulisi korporatiivvõrke, annab võrguliideste haldamise oskus sulle kontrolli selle üle, kuidas süsteem teiste võrguseadmetega suhtleb.

See juhend katab Linuxi võrguliideste põhialuseid – mis need on, kuidas neid vaadata ja konfigureerida ning milliseid tööriistu kasutada, näiteks **ifconfig**, **ip** ja **nmcli**, et võrguühendusi hallata.

---

## Mis on võrguliides?

Võrguliides on tark- või riistvaraline liides, mis ühendab arvuti võrguga, võimaldades süsteemil andmepakette saata ja vastu võtta. Igal võrguliidesel on seos võrguliidese kontrolleriga (NIC), mis võib olla füüsiline (nt Etherneti kaart või WiFi-adapter) või virtuaalne (kasutatakse virtuaalmasinates või konteinerites).

Linuxis nimetatakse võrguliideseid tavaliselt kindlas formaadis. Levinumad näited:

- **eth0**: Esimene Etherneti liides süsteemis.  
- **wlan0**: Esimene traadita võrguliides.  
- **lo**: Loopback-liides, mida kasutatakse sisemiseks suhtluseks.  
- **enp0s3**: Ennustatavad võrguliidese nimed, mida näeb sagedamini uuemates Linuxi distributsioonides.

---

## Võrguliideste vaatamine Linuxis

Võrguliideste vaatamiseks ja nende staatuse ning konfiguratsiooni kontrollimiseks on Linuxis mitu tööriista, sealhulgas **ifconfig**, **ip** ja **nmcli**.

### ifconfig kasutamine  
`ifconfig` on üks vanematest tööriistadest võrguliideste haldamiseks. Kuigi see on paljudes uuemates distributsioonides asendatud käsuga **ip**, on see endiselt laialdaselt kasutusel.  
Käsk kõigi aktiivsete võrguliideste ja nende konfiguratsiooni vaatamiseks:  

```
ifconfig
```

### ip addr kasutamine  
`ip` käsk on **ifconfig** kaasaegne asendus. Kõigi võrguliideste ja nende detailide vaatamiseks kasuta:  

```
ip addr
```

### nmcli kasutamine (NetworkManager)  
Kui kasutad Linuxi distributsiooni, mis haldab võrke **NetworkManageriga**, siis saad **nmcli** abil võrguliideseid vaadata ja hallata:  

```
nmcli device status
```

---

## Võrguliideste seadistamine Linuxis

Võrguliidese seadistamine hõlmab IP-aadresside määramist, liideste lubamist või keelamist ning täiendavate võrguseadete, nagu marsruudid ja DNS, konfigureerimist.

### IP-aadressi määramine ifconfig abil  
```
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up
```

### IP-aadressi määramine ip käsuga  
```
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set dev eth0 up
```

### NetworkManageri seadistamine nmcli abil  
```
nmcli con add type ethernet ifname eth0 con-name static-eth0 ip4 192.168.1.100/24 gw4 192.168.1.1
```

---

## Võrguliideste lubamine ja keelamine

### ifconfig abil  
- Liidese lubamiseks:  
  ```
  sudo ifconfig eth0 up
  ```
- Liidese keelamiseks:  
  ```
  sudo ifconfig eth0 down
  ```

### ip link abil  
```
sudo ip link set eth0 up  
sudo ip link set eth0 down
```

### nmcli abil  
```
nmcli device connect eth0  
nmcli device disconnect eth0  
```

---

## Marsruutide haldamine Linuxis

Lisaks IP-aadressidele vajavad võrguliidesed marsruudiinfot, et teada, kuidas liiklust teistele võrkudele suunata.

- Praeguse marsruuditabeli vaatamiseks:  
  ```
  ip route
  ```
- Statiivilise marsruudi lisamiseks:  
  ```
  sudo ip route add 192.168.2.0/24 via 192.168.1.1 dev eth0
  ```

---

## DNS-i konfigureerimine Linuxis

DNS (Domain Name System) tõlgib domeeninimed (nt www.example.com) IP-aadressideks. DNS-servereid saab konfigureerida staatiliselt või DHCP kaudu.

### Käsitsi DNS-serverite määramine
Faili **/etc/resolv.conf** redigeerimine:  
```
sudo nano /etc/resolv.conf
```
Lisa DNS-serverid:  
```
nameserver 8.8.8.8  
nameserver 8.8.4.4  
```

---

## Võrguliidese konfiguratsioonifailid

Lisaks käskudele saab võrguliideseid konfigureerida ka konfiguratsioonifailide kaudu. Need failid asuvad enamasti:  
- **Debian/Ubuntu**: `/etc/network/interfaces`  
- **Red Hat/CentOS**: `/etc/sysconfig/network-scripts/ifcfg-*`  