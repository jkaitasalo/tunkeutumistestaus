## x) Lue ja tiivistä

### **Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)**
Mikä on Fuff?
- verkko fuzzer, jolla voi etsiä piilotettuja hakemistoja (tai muita esim. POST parametrejä, hearereita) verkkosivuilta
- sanalistaa (esim. [SecLists common.txt](https://github.com/danielmiessler/SecLists)) hyväksikäyttäen voi hakea useita eri endpointeja erittäin nopeasti
- `./ffuf -w <wordlist> -u <url/FUZZ>` fuff alkaa hakemaan wordlistin rivi riviltä FUZZ avainsanan kohdalle kohdeosoitteesta 
- Filttereitä käyttämällä voidaan sulkea pois tuloksista yleisimmät vastaukset ja näin hakea poikkeamia
- esimerkkejä filttereistä:
  - `-fs`  size
  - `-fc`  status
  - `-fw`  words
  - `-fl`  lines
  - `-ft`  duration (ms)

### **Karvinen 2022: [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)**
- Hashing on yksisuuntaista. Hashattua salasanaa ei voi kääntää takaisin, mutta suuri sanakirja voidaan sana sanalta hashata ja katsoa saadaanko sama hash
- Hashcat on työkalu, joka on tehty tätä varten

**Hashcat asennus:**
> $ sudo apt-get update
> 
> $ sudo apt-get -y install hashid hashcat wget

Työkansion luonti:
> $ mkdir hashed
> 
> $ cd hashed

Ison sanakirjan haku (esim. Rockyou, joka sisältää 14 miljoonaa salasanaa)
> $ wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz
> 
> $ tar xf rockyou.txt.tar.gz
> 
> $ rm rockyou.txt.tar.gz

Hash tyypin tunnistus
- Hashcat haluaa tietää murrettavan hashin tyypin
- voi manuaalisesti verrata hashia ja `hashcat --example-hashes`
- helpompi tapa käyttää `hashid -m <hash>`

Hashin murtaminen

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/bda81735-3472-4b0b-afdc-0c5870e727b6)

- jos hash on jo murrettu, mutta ei tallennettu tiedostoon, voi tuloksen nähdä lisäämällä komennon loppuun `--show`

Mitä enemmän prosessointitehoa, sitä nopeampaa on Hashcatin käyttö

### **Karvinen 2023: [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)**
- Edellytyksien lataaminen ja asennus
  - `sudo apt-get update`
  - `sudo apt-get -y install micro bash-completion git build-essential libssl-dev zlib1g zlib1g-dev zlib-gst libbz2-1.0 libbz2-dev atool zip wget`

    ![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/540ff3a1-eb39-4042-9473-8db27c7ac760)
    
- John the Ripper, Jumbo version lataaminen ja kokoaminen
  - `git clone --depth=1 https://github.com/openwall/john.git` Ladataan lähdekoodi kloonaamalla repo gitillä
  - > $ cd john/src/
    >
    > $ ./configure
    >
    > $ make -s clean && make -sj4
    - Configure tunnistaa ympäristön ja luo Makefile 'make' komennolle.
    - Tässä kohtaa saattaa tulla varoituksia puuttuvista riippuvuuksista, ne voidaan käydä virheilmoitus virheilmoitukselta läpi `apt-cache search <lib>` Tämän jälkeen pitää aina muistaa ajaa `./configure` uudestaan
    - Lopuksi itse kokoaminen `make -s clean && make -sj4` komennolla
  - uudet skriptit löytyvät run/ kansiosta `cd ../run/`
  - `$ $HOME/john/run/john ` John the Ripper ajo
- salasanasuojatun zip tiedoston murtaminen `$ $HOME/john/run/zip2john tero.zip >tero.zip.hash` tämä printtaa muutaman rivin zip tiedostosta
  - tämän jälkeen John:n sanakirja hyökkäys tiedoston hashia vastaan
  - `$ $HOME/john/run/john tero.zip.hash `
    - tärkeä rivi tuloksesta `butterfly        (tero.zip/secretFiles/SECRET.md)  `
  - zipin purku juuri saadulla salasanalla `unzip tero.zip` ja salasanaa pyydettäessä, kirjoitetaan se
  - `cat secretFiles/SECRET.md `

John the Ripper:llä pystyy murtamaan todella useita [formaatteja](https://terokarvinen.com/2023/crack-file-password-with-john/#:~:text=So%20many%20files,wpapcap%20zed%20zip) mm. 
  > 1password 7z DPAPImk adxcsouf aem aix aix andotp androidbackup androidfde ansible apex apop applenotes aruba atmail axcrypt bestcrypt bestcryptve bitcoin bitlocker bitshares bitwarden bks blockchain cardano ccache cisco cracf dashlane deepsound diskcryptor dmg dmg ecryptfs ejabberd electrum encdatavault encfs enpass ethereum filezilla geli gpg hccap hccapx htdigest ibmiscanner ikescan itunes_backup iwork kdcdump keepass keychain keyring keystore kirbi known_hosts krb kwallet lastpass ldif libreoffice lion lion lotus luks mac mac mcafee_epo monero money mongodb mosquitto mozilla multibit neo network office openbsd_softraid openssl padlock pcap pdf pem pfx pgpdisk pgpsda pgpwde prosody ps_token pse putty pwsafe racf radius radius rar restic sap sense signal sipdump ssh sspr staroffice strip telegram test_tezos tezos truecrypt uaf vdi vmx wpapcap zed zip

***
## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

- Asensin Hashcatin ilman ongelmia ja loin hashed kansion `mkdir hashed` komennolla
- Latasin itselleni SecLists dictionaryn ja testasin tutoriaalissa olevat `head` ja `wc -l` komennot
- Laitoin `hashid -m 6b1628b016dff46e6fa35684be6acc96` tunnistamaan hashin

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/19c0a90c-42e7-4c9a-9d7b-f4ad33812ad6)

Tämän jälkeen syötin `hashcat -m 0 '6b1628b016dff46e6fa35684be6acc96' rockyou.txt -o solved`, joka siis lähtee murtamaan hashia `rockyou.txt`:n sanakirjalla ja tallentaa `solved` tiedostoon onnistuneen murron tuloksen

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0318cdac-b918-4601-9c48-1cf6b5661f65)

- Sehän lähti raksuttamaan. Huomasin, että murron käynnistyksen yhteydessä ohjelma asettaa tiettyjä toimintoja esimerkiksi ylikuumentumista varten varotoimenpiteeksi keskeyttämisen, jos lämmöt nousevat yli 90 C


Sieltähän se tulos saatiin onnistuneesti. Salasana on tuolle hashille siis `summer`. Koneen rautaa hyödynnettiin 51% kapasiteetista

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/200ad101-b68c-4d2b-9228-5f3ed931acd4)

***
## b) Salainen, mutta ei multa. Ratkaise dirfuzt-1 artikkelista Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)

Latasin tehtävää varten [dirfuzt-1](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/#:~:text=Challenge%20binary-,dirfuzt%2D1,-.) kohteen

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/cb2a2f1a-5f1a-40d6-a5bd-b9348389b3b9)

Käynnistin sen ja totesin, että pelittää!

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/5f2bc4c8-a329-46cd-b1b5-70bd799a8e96)

Latasin FFUF version 2.1.0 ja tarkistin, että se tosiaan tuli ladattua

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/54c823f2-0673-4418-9a35-966066209d6d)

Seuraavaksi ladattiin Daniel Miesslerin SecLists:stä sanalista yleisimmistä verkkopoluista

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/78f7cf64-1b5b-427a-8a43-0cd1d4c813e9)

Ja sitten lähdettiin matkaan

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f0789d12-d074-4e11-a79d-96765a2dc84e)


- ffuf tarkisti 4727 endpointtia ja aikaahan tuohon tosiaan meni se 8 sekuntia, 511 pyyntöä/sekunti. Oletin, että se olisi ollut hitaampi, mutta muistinkin joohoin presentaatiosta saman ja jopa kovemman nopeuden

Seuraavaksi parhaimmaksi askeleeksi näin, että filteröidään koon 154 mukaan parametrillä `-fs 154`, jotta saadaan karsittua tuloksia

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f933a52f-2103-4d1f-b218-5e7fea8c2afa)

- Tällä kertaa saatiin 7 osumaa

Seuraava vaihe olisi tutkia itse kyseisiä endpointeja, joten päätin lähteä `curl` avuin liikenteeseen. Noista osumista "hyökkääjänä" ehkä kiinnostavin olisi tuo `wp-admin`, joten aloitetaan sillä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/94695ac9-9d83-48dc-aee3-d056fe9095af)

- Sieltähän tuo heti löytyikin. `curl <url>` palauttaa HTML sisällön ja sieltähän löytyy "You found it!"

Päätin vielä tarkastella noita muita endpointteja ja sieltähän löytyy `.git/index`, `.git/config`, `.git/logs` ja `.git/HEAD` takaa tuo sama "You found it!". Tuo `.git` palauttaa vain "Moved permanently" ja oletan `render/https://www.google.com`:n lataavan näytölle vain googlen etusivun, toki ei toimi kokeiltuna, sillä kone on kytketty irti verkosta. Huomasin myös `.git/*` endpointin tuloksilla olevan kaikilla sama lippu, mutta `wp-admin` endpointilla on oma noista muista eriävä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/71b78476-707e-48e6-aacd-791437cbd6a8)

***
## c) Asenna John the Ripper ja testaa sen toiminta murtamalla jonkin esimerkkitiedoston salasana

Aloitin John the Ripper asennuksen esivaatimuksien asentamisella

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6f6c05e7-2241-4ce2-b050-ccba66d48da0)

Seuraavaksi kloonasin John the Ripper, Jumbo version

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f7b9ea64-799c-40a4-bf68-27df144f47b7)

Ja sitten konfiguraatioon

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/aac4b2a5-66d5-4cb0-a244-da4942d6160e)

libssl-dev antoi virheilmoitusta, joten latasin sen uudestaan ja tämän jälkeen `./configure` toimi. Tämän jälkeen `make -s clean && make -sj4` komennolla päästiin kasailemaan

- Hetken odottelun jälkeen saatiin ohjelma kasailtua

Seuraavaksi `cd ../run` ja `ls -1` komennoilla tarkastelemaan skriptejä ym

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/d28abe99-360a-43d1-9664-69fbb6008d2b)

- Näitähän löytyy vino pino..

Latasin tero.zip ja yritin `unzip tero.zip` ihan vain nähdäkseni, että tiedosto on kuin onkin lukossa. Sen jälkeen lähdin toteuttamaan ohjeen mukaisesti:

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6cedb47a-e535-48e9-bbf9-1e18d8bdf0ea)

- Saatiin ongittua `tero.zip`:n salasanan hash

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/71b24b66-2ee2-418c-a5d4-80c61e8f1927)

- Hyökättiin tuota hashia vastaan ja saatiin john:n avulla murrettua tiedoston salasana `butterfly`

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6eb2a3ab-8e55-421d-8bce-49b3f483d87d)

- Navigoitiin vielä purettuun kansioon ja luettiin mitä salaisuuksia `SECRET.md` piti sisällään

## d) Fuffme. Asenna [Ffufme](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/) harjoitusmaali paikallisesti omalle koneellesi. Ratkaise tehtävät (kaikki paitsi ei "Content Discovery - Pipes")

Alotin tehtävän perinteisesti esivaatimuksien asennuksella. Tässä kohtaa käytin paljon enemmän aikaa, kuin tekisi mieli myöntää, mutta mitä tästä opittiin oli se, että kannattaa katsoa tarkkaan oikeinkirjoitus.. etenkin `ffuf` kaltaisten sanojen kanssa.. ongelmana oli väärinkirjoitettu `fuff` ..

Tästä kompastuskivestä eteenpäin päästyä, saatiin asennukset hyvin pulkkaan ja kontti pyörimään

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/4b1b99c7-e22f-48f3-84ef-8b4e0ee70a76)

Seuraavaksi latailtiin sanalistoja ilman ongelmia

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/2bcff330-05b5-48bf-a1cf-64aa79d3bfa1)


### Basic Content Discovery

Ensimmäisessä tehtävässä tehdään ihan perus fuzzaus, ei mitään erikoista

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f73a01fc-0144-48ab-8aff-b54ff077ccdc)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/c91fbc40-fced-49d1-9969-a642f93aa04c)

Tehtävänannon mukaiset polut löytyvät!

### Content Discovery With Recursion

Seuraavassa tehtävässä tehdään samankaltainen skannaus sillä poikkeuksella, että lisätään komentoon `-recursion` switch, joka hakemisto löydettyään siirtyy siihen hakemistoon aloittamaan saman skannauksen. Tämä looppi toistuu, kunnes ei enää löydy tuloksia

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0a363b70-a57a-4019-bbae-8ce062acc904)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/20afd8f3-fe28-4ddf-9878-fca1b9c213e6)

Tehtävänannon mukaiset polut löytyvät. Käytin vielä `curl <url>` komentoa tämän ja edellisen tehtävän polkuihin

### Content Discovery With File Extensions

Seuraavassa tehtävässä kurkataan `-e` switchillä file extensionin `.log` polkuja

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/057351e0-665a-46b7-86df-b5f5002a5189)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/fdd1a067-a362-4bc8-9ceb-9c645a6841cf)

Polku löytyy!

### No 404 Status

Testataan ensin perus fuzzauskomentoa ja sen jälkeen edetään sopivin keinoin

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0c551f77-a092-4807-a261-ed364503aa42)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6af9a190-0544-4574-ad1c-419a7e8e9891)

Läjä `Size: 669` polkuja löytyykin

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/ba041662-1940-4c25-ad6a-06b07a169927)

Fuzzaus `-fs` filter size parametrillä löytää meille `secret` polun. Tämän `curl` palauttaa oudon vastauksen muista poiketen. En tiedä pitikö näitä edes curlata

### Param Mining

Haetaan `localhost/cd/param/data`:lle puuttuvaa parametria fuzzaamalla `localhost/cd/param/data?FUZZ`

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/41148567-3237-45cc-93e1-073e67ace389)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/abf4dab6-6078-4003-b957-91351563188d)

Parametri löytyy!

### Rate Limited

Fuzzaus `-mc` match content parametrillä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/104afde0-24db-4374-9aea-fedadbcbc79c)

Läjä polkuja ja jäähyt. Eteneminen hidastuu todella paljon

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/944b0c50-75af-4933-b85e-a3d8bf112c2c)

Lisätään fuzziin `-t 5` threads 5 ja `-p 0.1` pause 0.1, jotka yhdessä saavat aikaan sen, että fuzzaus tapahtuu 50 pyyntöä sekunnissa

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/d3b9d166-0441-4894-9b67-a7650ae83167)

Polku `oracle` löytyy

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/ad9992bb-2287-47b7-891c-fe95a19fa169)


### Subdomains - Virtual Host Enumeration

Lopuksi fuzzataan urlista subdomainia virtuaalisten hostien avulla. `-H "Host: FUZZ.<url>`

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/4fe569de-4410-4364-b7a5-6899d0ec954a)

Fuzzaus palauttaa läjän `Size: 1495` polkuja

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/791efc99-b7ea-40d4-8634-78553a060dca)

Tämän jälkeen filteröidään tulokset `-fs 1495` parametrillä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/10c74365-684b-4e47-a495-8c0177e1485a)

Ja niin sieltä löydetäänkin yksi polku

***
### Palautettu tässä vaiheessa
***
## e) Tee msfvenom-työkalulla haittaohjelma, joka soittaa kotiin (reverse shell). Ota yhteys vastaan metasploitin multi/handler -työkalulla.

***
## f) Asenna Windows virtuaalikoneeseen. Voi olla esimerkiksi Metasploitable 3 tai Microsoftin sivuilta saatava ilmainen kokeiluversio.

***
## g) Ota Windowsiin graafinen etähallintayhteys Linuxista. Käytä RDP:tä eli Remote Desktop Protocol.

***
## Lähteet:

- Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- Daniel Miessler: [SecLists](https://github.com/danielmiessler/SecLists)
- Karvinen 2022: [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)
- Karvinen 2023: [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)
- Openwall/john (github): [John the Ripper](https://github.com/openwall/john)
- Karvinen 2023: [Fuffme](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/)
