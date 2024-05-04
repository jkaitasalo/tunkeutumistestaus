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

![image](https://github.com/jkaitasalo/tunkeutumistestaus/assets/117358885/0f1df049-4241-4a9e-9a92-7f3bac6d8de2)

- Testasin vielä mennä googleen ja totesinkin ZAP:sta, ettei ohjelmaan tule merkintöjä

## PortSwigger Labs. Ratkaise tehtävät.

### c) [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)
### d) [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)
### e) [File path traversal, traversal sequences blocked with absolute path bypass](https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass)
### f) [File path traversal, traversal sequences stripped non-recursively](https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively)
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
