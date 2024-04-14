## x) Lue/katso ja tiivistä.

### Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
***
[Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
- Nämä olomuodot kuvailevat miten nmap näkee skannatut portit
  - **open**
    - Tätä porttia käyttävä applikaatio aktiivisesti hyväksyy TCP-yhteyksiä, UDP-datagrammeja tai SCTP-yhteyksiä. Porttiskannauksen pääasiallinen tavoite löytää portti tällä olomuodolla.
  - **closed**
    - Vastaanottaa nmap paketteja ja vastaa niille. Applikaatio ei aktiivisesti kuitenkaan käytä tätä porttia. Hyödyllistä skannata myöhemmin, jos portti olisikin käytössä.
  - **filtered**
    - nmap ei onnistu määrittämään onko portti `open`, koska paketin filteröinti estää portin saavuttamisen palomuurin tai muun ominaisuuden takia. Hidastaa porttiskannausta huomattavasti.
  - **unfiltered**
    - Portti on saavutettavissa, mutta nmap ei onnistu määrittämään onko portti `open` vai `closed`.
  - **open|filtered**
    - nmap ei kykene määrittämään onko portti `closed` vai `filtered`

[Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)
- **-sS**
  - TCP SYN scan. Oletus TCP skannaustyyppi kun SYN skanni ei ole vaihtoehto. Ei suorita TCP yhteyksiä, joten vaikeampi havaita.
- **-sT**
  - TCP connect scan. Oletus skannaustyyppi, jos SYN ei ole vaihtoehto. Nmap käyttää Berkeley Sockets APIa yhteyden muodostamiseen.
- **-sU**
  - UDP scan. Suurin osa palveluista käyttää TCP protokollaa, DNS, SNMP ja DHCP ovat yleisiä UDP käyttäviä. UDP skannaus on hitaampaa ja vaikeampaa, kuin TCP. UDP skannauksen voi lisätä SYN skannauksen kanssa `-sS` tarkistaakseen molemmat samalla skannauksella.

***
### KKO 2003:36. Alaikäinen tuomittiin Osuuspankkikeskuksen porttiskannaamisesta, korkeimman oikeuden ratkaisu.

- 17-vuotias "osaava" nuori porttiskannasi pankin sivut.
- Syytetty ei ollut tehnyt muuta, mutta piilevät kustannukset pankille olivat niin suuret, että oikeudesta lähdettiin hakemaan korvauksia
- Tuomion lievennystä oli haettu, mutta oikeus katsoi syytetyn tietotaidon olevan sillä tasolla, että alennusta ei tehty.

***
## a) Asenna Kali virtuaalikoneeseen

- Asensin Kalin valmiin vboxin sivulta
[https://www.kali.org/get-kali/#kali-virtual-machines](https://www.kali.org/get-kali/#kali-virtual-machines)
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/aa932635-cd1b-4c40-ad7d-ac4bef088ddc)


## b) Asenna Metasploitable 2 virtuaalikoneeseen

- Asensin Metasploitable 2:n lataamalla [https://sourceforge.net/projects/metasploitable/files/Metasploitable2/](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/) Metasploitable 2 zipin ja seuraamalla [https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/](https://www.geeksforgeeks.org/how-to-install-metasploitable-2-in-virtualbox/) ohjeita.

## c) Tee koneille virtuaaliverkko, jossa Kalin ja Metasploitablen välillä on host-only network

Loin koneiden välille virtuaalisen verkon Network asetuksissa.
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/649ae896-0fb4-4d89-8d4f-70b8219c5a31)

Testasin molemmissa ping ja curl
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/4dd8e7f8-133d-4ef2-b086-4ed8fc4fc629)

Yhteys Kalista metasploitableen onnistuu:
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/199a62b2-ced2-4b74-9cad-a1afe3bb2877)

***
## d) Etsi Metasploitable porttiskannaamalla (db_nmap -sn). Katso, ettei skannauspaketteja vuoda Internetiin - kannattaa irrottaa koneet netistä skannatessa. Seuraa liikennettä snifferillä.

- Käynnistin Metasploitin Kalilla komennolla `sudo msfdb init && msfconsole`
- Käynnistin Wiresharkin
![Screenshot 2024-04-14 201255](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/7f35a351-db29-430d-9ec7-b9ae9469185f)

- `db_nmap -sn <ip>`
![Screenshot 2024-04-14 201716](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/bb30025e-c513-4c19-ba40-0aa6a1eb4060)

- Wireshark havaitsi TCP protokollan pakettien vaihtoa koneiden välillä.
![Screenshot 2024-04-14 201744](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/02acef08-29aa-43e3-ae46-0c465d7c62ee)


## e) Porttiskannaa Metasploitable huolellisesti (db_nmap -A -p0-). Analysoi tulos. Kerro myös ammatillinen mielipiteesi (uusi, vanha, tavallinen, erikoinen), jos jokin herättää ajatuksia. Seuraa liikennettä snifferillä.

- Wireshark skannasi 117722 pakettia
![Screenshot 2024-04-14 203844](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/fe3937ff-1e08-48f1-a8f5-d4d7955526e9)

- Silmään erityisesti pisti portti `513/tcp open login`, `514/tcp open shell Netkit rshd` ja `3306/tcp open mysql MySQL 5.0.51a-3ubuntu5`. Nämä ainakin omaan korvaan kuulostavat sellaisilta, jotka olisivat mahdollisesti todella haavoittuvaisia hyökkäykselle.
![Screenshot 2024-04-14 203959](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6f9bc4f4-c3d2-4992-b7d0-d2987b70cad6)


## f) Tallenna portiskannauksen tulos tiedostoon käyttäen nmap:n omaa tallennusta (nmap -oA foo).'

- Tallensin tulokset tiedostoon komennolla `nmap -oA foo <ip>`
![Screenshot 2024-04-14 204750](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/63507b0b-a06c-40c0-923a-03331da36327)

- Tulokset tallentuivat tiedostomuotoihin .gnmap .nmap ja .xml

## Tallenna shell-sessio tekstitiedostoon script-työkalulla (script -fa log001.txt)

- Tallensin shell-session edellisestä tehtävästä tiedostoon log001.txt
![Screenshot 2024-04-14 205747](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/497cf685-daf3-4f2b-b0e3-7f1c76536adb)


## h) Etsi kaikki maininnat jostain osoitteesta, palvelusta tai vastaavasta kaikista tallennetuista tuloksista ja lokeista (grep -ir tero).

- Tulokset `grep -ir tero` ajon jälkeen.
![Screenshot 2024-04-14 205906](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/5966148c-e5ea-4936-914e-96573c6c5471)



## i) Anna esimerkit nmap ajonaikaisista toiminnosta. (man nmap: runtime interaction)

- `v / V` Lisää/Vähennä monisanaisuustasoa
- `d / D` Lisää/Vähennä debuggauksen tasoa
- `p / P` Päälle/Pois pakettien jäljitys
- `?` Printtaa runtime interaction help ruudun
- Mikä vaan muu: printtaa terminaaliin status viestin

***
#### Tehtävät palautettiin deadlinesta myöhässä Sunnuntai-illalla.

***
## Lähteet


