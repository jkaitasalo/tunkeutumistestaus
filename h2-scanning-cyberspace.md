## x) Lue/katso ja tiivistä.

### Lyon 2009: Nmap Network Scanning: Chapter 15. Nmap Reference Guide:
#### [Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html) (opettele, mitä tarkoittavat: open, closed, filtered; muuten vain silmäily)
  - Nämä olomuodot kuvailevat miten nmap näkee skannatut portit
    - open
      - Tätä porttia käyttävä applikaatio aktiivisesti hyväksyy TCP-yhteyksiä, UDP-datagrammeja tai SCTP-yhteyksiä. Porttiskannauksen pääasiallinen tavoite löytää portti tällä olomuodolla.
    - closed
      - Vastaanottaa nmap paketteja ja vastaa niille. Applikaatio ei aktiivisesti kuitenkaan käytä tätä porttia. Hyödyllistä skannata myöhemmin, jos portti olisikin käytössä.
    - filtered
      - nmap ei onnistu määrittämään onko portti `open`, koska paketin filteröinti estää portin saavuttamisen palomuurin tai muun ominaisuuden takia. Hidastaa porttiskannausta huomattavasti.
    - unfiltered
      - Portti on saavutettavissa, mutta nmap ei onnistu määrittämään onko portti `open` vai `closed`.
    - open|filtered
      - nmap ei kykene määrittämään onko portti `closed` vai `filtered`
- [Port Scanning Techniques](https://nmap.org/book/man-port-scanning-techniques.html) (opettele, mitä ovat: -sS -sT -sU; muuten vain silmäily)

### KKO 2003:36. Alaikäinen tuomittiin Osuuspankkikeskuksen porttiskannaamisesta, korkeimman oikeuden ratkaisu.

### Vapaavalintainen läpikävely 0xdf tai ippsec (Kannattaa valita helppo; esim "Base Points: Easy")
