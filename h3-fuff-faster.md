## x) Lue ja tiivistä

**Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)**
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

**Karvinen 2022: [Cracking Passwords with Hashcat](https://terokarvinen.com/2022/cracking-passwords-with-hashcat/)**
- asd

**Karvinen 2023: [Crack File Password With John](https://terokarvinen.com/2023/crack-file-password-with-john/)**
- asd

***
## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

***
## Lähteet:

- Karvinen 2023: [Find Hidden Web Directories - Fuzz URLs with ffuf](https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/)
- Daniel Miessler, [SecLists](https://github.com/danielmiessler/SecLists)
