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

- Kirjauduin käyttäjätunnuksilla sisään. Kuva paketista.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/151b7937-66fb-46fc-96ec-fd461cbc2d9d)

- Yritin laittaa templateen suoraan `{{secret_key}}` sormet ristissä toivoen, että se palauttaa avaimen. Ei palauttanut muuta kuin tyhjää.
- Seuraavaksi yritin artikkelista apinoiden `$_GET['secret.key']`, mutta sain erroria.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/af7e7ce2-547d-49ec-892e-818cde3e2a8b)

  - Virheilmoitus päästi minut itse varsinaisen templatetyökalun jäljille

- Djangon dokumentaatiosta löytyi auto-escape funktionaalisuuden ohitus safe filterillä, mutta siitä ei ollut hyötyä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/40b0f2c8-e937-492e-a928-8f1802504da2)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/4ed6519f-20c7-4a89-a548-7abc7a4cc67c)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/f3dde360-d09d-4d83-8fe9-95a16357c032)

- Printtaa pelkkää tyhjää.
- Löysin debug funktionaalisuutta, jos se on päällä.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/19aaf149-8002-441a-920f-5242ad547ff9)

- Vaikuttaa, että sivusto on julkaistu ilman varotoimintojen käyttöönottoa.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/177c7b58-e571-4c6e-9417-04998f220bda)

- Debug antoi vinon pinon erilaisia objekteja ja moduuleja. Ensimmäisenä silmään osui kuitening settings. Yritin kuitenkin debugista hakea ctrl+f 'secret' ja 'key', mutta tuloksetta.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/4dbff7f1-7912-4b41-b1a8-20d7d79e2ca1)

- Settings objekti palauttaa UserSettingsHolder nimisen palikan. Takaisin djangon dokumentaation pariin, tällä kertaa suoraan hakuun tuo palikka.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/6899c6a9-4f0f-4e43-8150-a8a942bb58ad)

- Täältähän löytyi hyvinkin raskauttavaa tavaraa. Tällä voi lähteä toteuttamaan tuosta settings.py ja SECRET_KEY yhdistelmällä

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/c5ca066b-fc47-48d6-9b45-a839e334d544)

- Epämääräinen merkkijono sieltä löytyi ja kun sitä kokeili antaa vastaukseksi olikin lopputulos ilahduttava.


### h) [Basic SSRF against the local server](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)


- lisäämällä /admin sivuston osoitteen loppuun tervehtii meitä tämänlainen sivu:

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/14af05a4-d7c7-4576-bf31-f5182c0d4bdb)

- Navigoin etsimään haavoittuvuutta tuotetietojen Stock ominaisuudesta

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0a51f4c1-853c-49b8-ab72-0930a934d24f)

- Hetken aikaa ZAP:n nappaamia paketteja tutkittuani sain kiinni paketista, jolla oli Api avain.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/e8a44485-7c63-4479-bec2-f346fbaf832f)

- Muutin api avaimen tilalle `http://localhost/admin` ja sain vastauksena admin käyttöliittymän HTML koodin, johon minulla ei kuitenkaan ollut pääsyä.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/9d121809-1166-45f5-adcc-e9b9a66bf9f8)

- Seuraavaksi jatkoin kyseistä litaniaa muotoon `http://localhost/admin/delete?username=carlos`, joka palautti minulle labran "Solved" paketin

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/2806c4dd-405c-47ef-b45d-f37bbaf98be5)

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/83c0e6f2-c7ca-4c8a-8e31-17aa5be9d0bf)


### i) [Reflected XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded)


- Kirjoitin sivun hakukenttään jotain, jotta saan napattua sivulta paketin kyseisestä toiminnasta

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/fce73cd9-5e25-4fab-b08f-7155e81f5c5c)

- Bongasin artikkelista vinkin, että kenttään voisi sujauttaa skriptiä muodossa: `<script></script>`. Tehtävänannossa myös vinkattiin `alert()` käyttöä, joten käytin sitä.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/fae10651-b904-41e6-843c-45c932e1dc96)

- Seuraavasta paketista huomasin, että pyyntö muuttaa tuon skript litanian merkit erilaiseen muotoon, jotta palvelin pystyy tunnistamaan ja ajamaan sisällön.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/ff4984e9-1868-4d9f-9950-4319dc4ff409)

- Hetken päästä sivusto palautti paketin onnistuneesta tehtävän suorituksesta.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/9af9ea1b-4106-417e-b5b4-f9a942244b40)


### j) [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)


- Tehtävänanto oli suoraviivainen ja käytin edellisestä tehtävästä tuttua `alert()` funktiota

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/b99d6439-3fff-4e53-ae6d-4cdeb91539c9)

- ZAP nappasi paketin, jossa näkyykin POST metodin paketti, joka sisältää tuon meidän skriptin, jossa jälleen nuo epämääräiset merkit muunnettu luettavaan muotoon.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/1d85928d-4282-40ca-bc33-4bdd241f29d8)

- Kommentti postattua palauttikin sivu heti tuon LAB Solved tekstin.

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/1f7ad49d-218c-4c2e-b1ea-7e2ccdcbb360)


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
- https://docs.djangoproject.com/en/5.0/topics/templates/
- https://docs.djangoproject.com/en/5.0/ref/settings/
