# Teema 12: Marsruutimine. Kuidas marsruuter töötab?

## Mis on marsruutimine?
Marsruutimine on protsess, mille käigus andmepaketid suunatakse võrgu kaudu sihtkohta, kasutades marsruutimistabeleid ja -protokolle. See toimub IP-aadresside põhjal ning võimaldab liiklust suunata erinevate alavõrkude vahel.

## Mis on marsruutimistabel ja miks seda vaja on?
Marsruutimistabel sisaldab informatsiooni selle kohta, kuidas paketid peaksid liikuma sihtkohta. See sisaldab:
- Sihtvõrgu aadressi ja maski
- Väljundliidest või järgmist hüppepunkti (next hop)
- Marsruudi metrikat ja eelistust

## Marsruutide võrdlemine ja longest match reegel
Marsruuter võrdleb marsruutimistabelis olevaid kirjeid ja valib selle marsruudi, millel on kõige pikem mask (longest match reegel). See tähendab, et kitsam sihtvõrk (rohkem maskibitte) on eelistatum kui laiem sihtvõrk.

## Marsruutide allikate võrdlemine ja eelistus erinevatel tootjatel
Erinevad seadmed kasutavad marsruutide eelistamiseks administratiivset distantsi (administrative distance). Tavaliselt on parimad marsruudid järgmises eelistusjärjestuses:
1. Otseselt ühendatud võrgud (AD=0)
2. Staatilised marsruudid (AD=1 või 5, sõltuvalt tootjast)
3. Dünaamilised marsruudid (OSPF, EIGRP, BGP jne, millel on erinevad AD-väärtused)

Erinevad tootjad (Cisco, Juniper, Windows, Linux) kasutavad erinevat süntaksit marsruutimistabelite haldamiseks.

## Kas hostil on ka marsruutimistabel?
Jah, igal hostil on marsruutimistabel, mis suunab liikluse kas lokaalsesse võrku või väljapoole (default gateway kaudu). Seda saab vaadata käsuga:
- Windows: `route print` või `netstat -rn`
- Linux: `ip route` või `route -n`

## Mis on default gateway?
Default gateway on vaikimisi väljapääsuvõrk, kuhu saadetakse paketid, mis ei kuulu hosti lokaalsesse võrku. See on tavaliselt ruuteri IP-aadress, mis on hostiga samas alamvõrgus.

## Mis juhtub, kui hostil on rohkem marsruute kui ainult default gateway?
Kui hostil on mitu marsruuti, siis kasutatakse longest match reeglit. Kui saadaval on mitu marsruuti sama sihtkoha jaoks, siis valitakse madalaima metrikaga tee.

## Marsruutimistabeli näited:
### Cisco
```
R1# show ip route
C 192.168.1.0/24 is directly connected, GigabitEthernet0/0
S 10.10.10.0/24 [1/0] via 192.168.1.1
```

### Juniper
```
user@router> show route
inet.0: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
10.10.10.0/24 * [Static/5] via 192.168.1.1
```

### Windows
```
C:\> route print
Network Destination        Netmask          Gateway
0.0.0.0                    0.0.0.0         192.168.1.1
192.168.1.0                255.255.255.0   On-link
```

### Linux
```
$ ip route
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.2
```

## Layer 3 kommutatsioon ja miks on vahel parem kasutada L3 kommutaatorit
L3 kommutaator on kommutaator, millel on ka marsruutimisfunktsioonid. Tavaliselt on see kiire alternatiiv traditsioonilisele ruuterile sisevõrkudes, kuna see töötab riistvarakiirendusega ja vähendab liikluse latentsust.

## Kokkuvõte
Marsruutimine on oluline osa võrgu tööst, mis võimaldab andmepakettide suunamist erinevate võrkude vahel. Marsruutimistabel määrab, kuhu paketid saadetakse, kasutades longest match reeglit, administratiivset distantsi ja metrikat. Erinevatel seadmetel (Cisco, Juniper, Windows, Linux) on omad viisid marsruutimistabelite haldamiseks, kuid põhiline loogika on kõigil sama. Lisaks kasutatakse suuremates sisevõrkudes tihti L3 kommutaatorit kiireks marsruutimiseks ilma eraldi ruuterita.
