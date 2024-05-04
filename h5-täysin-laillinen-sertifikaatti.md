## x) Lue/katso ja tiivistä


- OWASP 2021: OWASP Top 10:2021
  - A01:2021 – Broken Access Control (IDOR ja path traversal ovat osa tätä)
  - A10:2021 – Server-Side Request Forgery (SSRF)
- PortSwigget Academy:
  - Insecure direct object references (IDOR)
  - Path traversal
  - Server-side template injection
  - Server-side request forgery (SSRF)
  - Cross-site scripting
- Karvinen 2020: Using New Webgoat 2023.4 to Try Web Hacking


## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi. Laita ZAP proxyksi selaimeesi. Laita ZAP sieppaamaan myös kuvat, niitä tarvitaan tämän kerran kotitehtävissä. Osoita, että hakupyynnöt ilmestyvät ZAP:n käyttöliittymään. (Ei toimi localhost:lla ilman Foxyproxya)


- Loin CA-sertifikaatin ZAP:n `Tools>Options>Network>Server Certificates` valikossa Generate ja Save helppoon tiedostosijaintiin.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/17f406a7-e50c-47b3-b9b6-aceea1cc4442)

- FoxyProxy lisäosa asennettuna lisäsin 'Proxies' välilehdelle uuden proxyn 'ZAP Proxy' ja annoin sille asetuksilla tyypin HTTP, hostname localhost ja portti 8080

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/ca2cd839-35b1-4ac6-b997-3e5e71e42394)

- Firefoxin asetuksista käytiin laittamassa luotu sertifikaatti päälle Privacy&Security välilehden 'View Certificates...' valikosta: Authorities->Import->tuo luotu sertifikaatti->OK molempiin

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/9c84260a-dd21-4891-ab23-e0c6a8822967)

- siirryin google.com:iin ja huomaan ZAP:n nappaavan liikennettä ja yhteyden olevan `Secure "Verified by ZAP Root CA"`

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/c9edc593-2b24-493a-8741-68deb2a69f28)


## b) Kettumaista. Asenna FoxyProxy Standard Firefox Addon, ja lisää ZAP proxyksi siihen. Käytä FoxyProxyn "Patterns" -toimintoa, niin että vain valitsemasi weppisivut ohjataan Proxyyn. (Läksyssä ohjataan varmaankin PortSwigger Labs ja localhost.)


- Edellisessä tehtävässä asennettiinkin jo FoxyProxy ja lisättiin ZAP proxyksi
- Lisätään Whitelist (include) toiminnolla localhost ja portswigger proxyttavien listalle, jolloin muuta ei välitetä proxylle

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f2d066f6-2848-4752-a6e3-00b07abd523a)

- Testasin vielä mennä googleen ja totesinkin ZAP:sta, ettei ohjelmaan tule merkintöjä


## PortSwigger Labs. Ratkaise tehtävät.


### c) [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)


- Tehtävänannossa vihjattiin, että labra tallentaa käyttäjien chättilogit suoraan serverin tiedostojärjestelmään. Lähdin siis tutkimaan sivuston chättiä.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/400bdf99-9586-47ca-8f3d-3abaf4823e11)

- Chätistä pystyi lataamaan itselleen talteen login käydystä keskustelusta. ZAP:n puolella ilmestyi GET-pyyntö, jossa osana pyyntöä oli tosiaan tuo tekstitiedosto, joka omalle koneelle latautui. Huomasin myös, että ladattu tiedosto oli nimeltää `2.txt` ja mietinkin, ettei olisi niinkin helppo tehtävä, kuin selvittää onko `1.txt` logitiedosto olemassa.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6792b0ee-a5a8-4ac6-aa01-5689d595b9d6)

- Korvasin ZAP:n Requester toiminnallisuudella tuon GET pyyntöön haettavan tiedoston nimeen '2.txt' sijaan '1.txt' ja sieltähän saatiin vastauksena chättilogit toisen käyttäjän käymistä keskusteluista aspan kanssa, joka paljasti meille tehtävään vaadittavan salasanan.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/77d99e06-d376-49ea-b24a-71dcd619dc09)


### d) [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)


Labran avattuani ZAP nappasi heti kaikki sivulle latautuneet kuvat. File Path Traversal ohjeesta suoraan lähdin korvaamaan kuvan 'filename=66' tilalle 'filename=../../../etc/passwd' Requesterin avulla.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0b6f95a6-6477-4d3e-83a2-7a7bc4a70027)

- Requester palautti Content-Length 2316, eli jotain sieltä löytyi. Mitään ei kuitenkaan tapahtunut välittömästi, mutta sivuston päivitettyäni ruudulle pomppasi "Onnittelut!" ruutu, eli kai se siis meni läpi?

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/2639b5d3-374c-4df1-aebc-e8381849a960)


### e) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)


- Samalla tavalla, kuin edellisessä kohdassa, ZAP avulla nappasin sivuston kuvat. Etenin yksinkertaisesti artikkelin ohjeita noudattamalla ja esittämillä menetelmillä. Lisäsin 'filename=' perään taas polun `/etc/passwd/`, mutta sain Handlerilta vain .json muodossa vastauksen "No such file"
- Tämän jälkeen yritin "nested traversal sequences" `....//....//....//etc/passwd/`, mutta sekään ei tuottanut tulosta
- Pidemmän pähkäilyn ja testailun lopuksi testasin uudemman kerran tuota `/etc/passwd/` ja tällä kertaa se meni läpi (!?)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f969e9ef-8224-4582-916f-6af42056740c)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/328a1d2e-bddd-4735-81f5-9616fe734174)

- Sivusto palautti "Onnittelut!" -viestin
  - Olinkohan ensiyrittämällä kirjoittanut virheellisesti osoitteen ilman tuota ensimmäistä ` / ` -merkkiä, eli siis `etc/passwd/`, kun olisi pitänyt kirjoittaa `/etc/passwd/`


### f) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)

- Kaappasin ZAP:lla sivustolta jälleen kuvan.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/266cba64-2756-4a3f-8caf-874f9addb4c4)

- Lähdin toteuttamaan ei-rekursiivista etenemistä kohdesijaintiin `....//....//....//etc/passwd/`. Kyseinen menetelmä ohittaa järjestelmään asetettuja varotoimia, jotka yrittävät estää perinteisemmän `../../../` siirtymisen.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/70798f41-348a-4429-afad-f4889046ddc1)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/b96b25b3-c0a6-4039-a093-3ff2c5f60471)

- Tuollahan tuo saatiin maaliin.


### g) [Server-side template injection with information disclosure via user-supplied objects](https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects)


### h) [Basic SSRF against the local server](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)


### i) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)


### j) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)




## Lähteet


- https://terokarvinen.com/2024/eettinen-hakkerointi-2024/
- https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references
- https://portswigger.net/web-security/file-path-traversal/lab-simple
- https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass
- https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively
- https://portswigger.net/web-security/server-side-template-injection/exploiting/lab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects
- https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost
- https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded
- https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded
