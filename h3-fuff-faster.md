## x) Lue ja tiivistä

### **Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)**
- Mikä on Fuff?
  - verkko fuzzer, jolla voi etsiä piilotettuja hakemistoja (tai muita esim. POST parametrejä, hearereita) verkkosivuilta
  - sanalistaa (esim. [SecLists common.txt](https://github.com/danielmiessler/SecLists)) hyväksikäyttäen voi hakea useita eri endpointeja erittäin nopeasti
- `$ ./ffuf -w <wordlist> -u <url/FUZZ>` fuff alkaa hakemaan wordlistin rivi riviltä FUZZ avainsanan kohdalle kohdeosoitteesta 
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
- asd

***
## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.


***
## Lähteet:

- Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- Daniel Miessler: [SecLists](https://github.com/danielmiessler/SecLists)
- 
- Karvinen 2022: [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)
- 
- Karvinen 2023: [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)
