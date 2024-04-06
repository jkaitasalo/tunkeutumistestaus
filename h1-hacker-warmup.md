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


### d) Ratkaise ja selitä PortSwigger Labs: Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data. (Edellyttää ilmaista rekisteröitymistä. Tehtävän voi ratkaista pelkästään selaimen osoitekenttää muokkaamalla.) Selitä, mitä tekniikoita kokeilit, ja mitä hyökkäyksesi eri osat tekevät.

