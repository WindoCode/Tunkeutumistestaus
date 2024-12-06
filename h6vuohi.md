## Palautus myöhässä sairastelun takia. 
## x) Lue/katso ja tiivistä.
### Try Web Hacking on New Webgoat 2023.4 

- Java jdk ja jre asennus
- palomuurin asennus
- jar-tiedoston lataus githubista

## a) Asenna Webgoat 2023.4.

    $ sudo apt update
    $ sudo apt install ufw
    $ sudo ufw enable
- Komentojen jälkeen avasin verkkoselaimessa 127.0.0.1:8888/WebGoat ja sain palvelimen auki.

![image](https://github.com/user-attachments/assets/b4111efb-215b-4dfb-88de-07d47e03c726)

## Ratkaise WebGoat 2023.4

- Tarkistin että internettiin ei ole yhteyttä, ping 8.8.8.8. ZAP toimii normaalisti.

## Insecure Direct Object References
- Aluksi syötetään ZAP:ssa tarvittavat käyttäjätiedot omasta käyttäjästä.
- Tarkistellaan serverin vastausta ja löydetään lisää tietoa, kuten "role" ja "userid" tägit.
  ![image](https://github.com/user-attachments/assets/a9418da0-3874-4c40-99e3-7a145c8052ac)
- Kokeilin osoitetta "WebGoat/IDOR/profile/2342384", tällä kuitenkin tulee samat tiedot.

- Webgoat palauttaa: "internal server error".

- - Jäin jumiin, sillä pelkkä "userid" numeron muuttaminen ei riittänyt. 
- - Vihjeissä luki että tässä kohtaa pitäisi fuzzata jotta käyttäjän ID löytyisi. Testasin numeroita ylöspäin ja oikea käyttäjäID löytyi, joka oli "2342388".
- 
## Missing Function Level Access Control (ei kohtaa 4)

![image](https://github.com/user-attachments/assets/7c50f927-87e4-4b23-8132-0718e945817d)

- Tehtävänannosta päättelin, että tehtävässä käytetään dev toolseja selaimessa. Tutkin Inspectorissa elementit auki tuloksetta. 

- Vastaus löytyi kuitenkin lomaketta tutkimalla, jossa haavoittuvuus on.

![image](https://github.com/user-attachments/assets/b5bbd873-727a-4676-a02b-623c6ff7c98d)

- Lomakkeeseen "User"s ja "Config". Tehtävä suoritettu!

## Gathering User Info
-  Tarkoituksena on saada Jerry - käyttäjän tiiviste esille hyödyntäen aiemman osan tietoja. Tiedämme, että /access-control/users-polussa voisi olla näitä tietoja.
-  En kuitenkaan tiennyt tästä eteenpäin, joten jouduin käyttämään vinkkejä. Pyyntöön tuli lisätä "Content-Type: application/json".
-  Lopulta käyttäjätiedot tulivat tällä näkyviin:

![image](https://github.com/user-attachments/assets/a401416b-5a7c-42ed-8b36-7b4c3245de90)

## c) (A7) Identity & Auth Failure

### Authentication Bypasses

- Tarkoituksena kiertää 2FA. Ohjeiden esimerkissä oli tapa, jolla turvakysymysparametrit poistettiin pyynnöstä, jolloin autentikointi murrettiin.
- Kokeilin eri parametrejä, muuttaa tai poistaa niitä, tehtävän läpäisemiseksi kuitenkin piti muuttaa scQuestion0 ja 1 parametrejä. Netistä vinkin avulla muutin niitä secQuestionA:ksi ja B:ksi.
![image](https://github.com/user-attachments/assets/f10bad87-9a8a-4cfd-9f47-d1cc7bde706c)

### Insecure Login
- Tässä kohdassa nappasimme ZAP:illa paketin kiinni ja saimme käyttöön käyttäjätiedot "CaptainJack" sekä "BlackPearl".

![image](https://github.com/user-attachments/assets/832ce4bd-16c8-416c-954d-c9b9c6c456a0)

![image](https://github.com/user-attachments/assets/393248bd-677a-425e-bd95-90abe8185a92)

## Server Side Request Forgery
- Muutamme "tom.png" "jerry.png":ksi ohjeiden mukaisesti.

![image](https://github.com/user-attachments/assets/1a24c1ed-1a19-4fbc-8041-8279594fed90)

- Toisessa kohdassa muutetaan URL tiedostosijainnin sijasta. Vaihdetaan kissakuvan URL, tehtävänannossa pyydettyyn "http://ifconfig.pro" urliin.

![image](https://github.com/user-attachments/assets/890ef065-95da-440d-9867-a1eab497567a)




