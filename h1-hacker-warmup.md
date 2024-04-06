# h1 Hacker Warmup


### x) Lue/katso ja tiivistä

#### Porttiskannaus

- Active Recon, voi jättää jäljen ja mahdollista jäädä kiinni.
- Menetelmiä: porttiskannaus, verkkopalvelun arviointi, haavoittuvuusskannaus
- haavoittuvuusskannaus erittäin "äänekästä"
- Nmap, "monipuolisin ja vakain porttiskanneri"
- Masscan, nopein porttiskanneri
- Udpprotoscanner, nopea UDP porttiskanneri
- EyeWitness kätevä työkalu webbisivujen skannaukseen

#### Intrusion Kill Chain

- Reconnaissance | Tiedustelu ja taustatutkimus
- Weaponization | Etäyhteys troijalaisen yhdistäminen exploitin kanssa yleisesti automaattisen työkalun kanssa. Yleisesti PDF tai Office dokumentit hyviä
- Delivery | "Aseen" toimitus esim. sähköpostiliitteet, verkkosivut ja USB-tikut
- Exploitation | Toimitettu ase alkaa toteuttamaan tunkeutujan koodia.
- Installation | Takaoven tai etäyhteys troijalaisen asennus, jotta saadaan "jalansija" järjestelmään
- Command and Control (C2) | C2 kanavan auettua tunkeutujalla on "manuaalinen" yhteys kohteeseen
- Actions on Objectives | Vasta tässä vaiheessa tunkeutuja voi alkaa toteuttamaan alkuperäisen motiivin ja tavoitteen toimia


### a) Ratkaise Over The Wire: Bandit kolme ensimmäistä tasoa (0-2)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/219eef90-39ea-4f93-abab-a81339008a37)


### b) Asenna WebGoat ja kokeile, että pääset kirjautumaan sisään

- Loin käyttäjän ja kirjauduin sisään onnistuneesti


### c) Ratkaise WebGoatista tehtävät "HTTP Basics" ja "Developer tools"

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/48fcad18-75c5-4d38-808e-a2cb21d17b6c)


### d) Ratkaise ja selitä PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. Selitä, mitä tekniikoita kokeilit, ja mitä hyökkäyksesi eri osat tekevät.

- Kirjoitin sivuston osoitekenttän perälle '-- joka mahdollisti sen, että sivusto lähetti tietokantaan pyynnön, joka normaalisti sisällyttäisi "näkymättömän" ehdon, mutta '-- luo tuolle pyynnölle tekstikentän kohtaan loppuosan kommentiksi muuttavan "komennon", jolloin ruudulle saadaan näkymään enemmän asioita, kuin on tarkoitus sillä pyyntöä rajaava ehto jää kommentiksi.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/07675710-c6f7-48b7-ae76-c3a15010a1eb)
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/7a455614-819f-4a58-be69-0d54d58a0795)

- Alla muokattu pyyntö palauttaa tuotteet joiden kategoria on Gifts tai 1 on yhtäsuurikuin 1. Kun 1=1 on aina tosi, kysely palauttaa kaikki tuotteet
- 
![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/af46111c-8033-4ef0-be46-75497975dee0)


### e) Asenna Linux virtuaalikoneeseen. Suosittelen joko Kali (viimeisin versio) tai Debian 12-Bookworm.

- Asensin Kali Linuxin VirtualBoxiin.


### f) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi (localhost). Analysoi tulokset.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0c5a83e3-e424-48c1-bf66-b3a61913b0a0)

Portit skannattuani, näyttäisi siltä, että localhostissa olevat 1000 yleisintä porttia ovat kiinni.
- Ports: ignored state: closed (1000)


### h) Tee laaja porttiskanaus (nmap -A) omalle koneellesi (localhost), kaikki portit. Selitä, mitä -A tekee. Analysoi tulokset.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6be45585-b92e-4c18-b4f2-9e94d4ad92e9)

`nmap -A localhost` -komento suoritti porttiskannauksen lisäksi käyttöjärjestelmän ja palvelun(?) tunnistuksen. Komennolla ei onnistuttu tunnistamaan käyttöjärjestelmää. Kalin sisäänrakennettu ominaisuus?


### i) Asenna demoni tai pari (esim Apache ja SSH). Vertaile, miten localhost:n laajan porttiskannauksen tulos eroaa.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/1e10d10c-738b-40a8-9ec0-aa96b3432b46)

Apache demoni käynnistettyä on porttiin 80 auennut Linux pohjainen (Debian) palvelu. Palvelu palauttaa viestin "It works". Tällä kertaa OS skannauksen tulos on täyttä hepreaa itselleni.


### j) Kokeile ja esittele jokin avointen lähteiden tiedusteluun sopiva weppisivu tai työkalu.

Käytin [Bazzel: IntelTechniques: Tools](https://inteltechniques.com/tools/index.html) sivustolta löytyvää Documents Search Tool:ia. Hain työkalulla osumia itsestäni ja löysinkin sieltä mm. vanhan yläasteella tehdyn kuvataiteen "kilpailuun" lähettämäni työn, vanhan facebook profiilikuvan ja muutaman eri tapahtuman ilmoituksen, joihin olen osallistunut.
