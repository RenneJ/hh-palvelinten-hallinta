# H1 Viisikko (Karvinen 2023a)

## x) Tiivistelmät

### Create a Web Page Using Github (Karvinen 2023b)

- Luo tili Githubiin
- Navigoi "Repositories" -näkymään
  - Klikkaa "New" ja täytä kentät/valinnat (nimi, README.md, public/private, lisenssi)
- Klikattuasi "Create Repository" omaat uuden Github-sivun
  - Sivulla ei tosin ole README.md:n ja lisenssin lisäksi muuta sisältöä
- Uuden tiedoston lisääminen onnistuu sivulla näkyvän "Add file" -painikkeen kautta
  - Muista tieodostoa nimetessäsi tiedostopääte (.md eli markdown-tiedosto on hyvä tekstitiedostoille)

### Run Salt Command Locally (Karvinen 2023c)

- Salt-komentojen käyttäminen paikallisesti on hyvää harjoitusta ja sopii testaamiseen
  - Tuloksen näkee välittömästi
- Samat komennot toimivat sekä Linuxilla että Windowsilla
- Saltia käytetään tavanomaisesti useiden koneiden hallintaan verkon välityksellä
- Tärkeimmät komennot:
  - pkg
  - file
  - serivce
  - user
  - cmd

## a) Saltin asennus Debian 12:lle

Noudatan tässä osatehtävässä Tero Karvisen (2023a) vinkkejä. Olen aikaisemmalla opintojaksolla asentanut Debian 12:n (Bookworm), joten käytän sitä tämän ja tulevien tehtävien tekemiseen. 

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a850dbc7-ee3f-49ed-8d59-09b751b3b82a)

> Kuva 1. Teron ohjeet salt-minionin asentamiseen Debian 12:lle.

**HUOM!** Näitä ohjeita ei suositella käytettävän tuotantokoneelle.

Ensimmäinen rivi, komennolla mkdir on omassa tapauksessani turha. Tarkistin kyseisen hakemiston olevan olemassa listauskomennolla:

    ls /etc/apt/keyrings

Komentoriville tulostuisi "no such file or directory", jos hakemistoa ei löytyisi.

Suoritin loput komennot ohjeiden mukaisesti (kuva 1). Tämän jälkeen testasin, että asennus onnistui (kuva 2). Tarkistin komennon Teron ohjeista (Karvinen 2023c).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7ed290f4-915d-4e74-a018-c75859828b57)

> Kuva 2. Onnistunut asennus!

## b & c) Viisi tärkeintä & idempotentti

Karvisen (2023b) mukaan viisi tärkeintä Saltin toimintoa ovat: pkg, file, service, user ja cmd. 

### pkg

Pkg on lyhenne sanasta package (Oxford University Press 2023). Pkg-moduulilla voidaan tarkistaa, onko jokin sovellus asennettu tai onko asennetusta ohjelmassa käytössä viimeisin versio (VMware, Inc 2023a).

    sudo salt-call --local -l info state.single pkg.installed tree

Yllä oleva komento tarkoittaa:

- sudo = suorita seuraava komento/ohjelma superuserina (vaatii sudo-salasanan) (BSD System Manager's Manual 2023)
- salt-call = suoritettava ohjelma
- --local = saltin vaihtoehtoinen suoritustapa eli suorita paikallisesti (Hatch et al 2023)
- -l info = lokitus eli lisää suorituksen tulostukseen myös välivaiheet (muita vaihtoehtoja -l option kanssa esim. all tai error) (Hatch et al 2023)
- state.single = suorita yksi tilafunktio (VMware, Inc. 2023b)
- pkg.installed = suoritettava funktio; tarkista onko järjestelmä tilassa, johon on asennettu paketti (Karvinen 2023c)
- tree = paketti, jota tarkistetaan

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/5f071c97-97e4-4d63-9dc3-c67c05e0cfe5)

> Kuva 3. Ensimmäisen ajon tulos (sudo salt-call --local -l info state.single pkg.installed tree)

Pkg.installed on idempotentti funktio. Sen ajaminen useamman kerran palauttaa saman lopputuloksen (Wigmore 2016). Ts. funktio ei asenna toista tree-ohjelmaa seuraavilla funktion suorituskerroilla. Ko. funktio myös tulostaa terminaaliin lisätietoja ajosta. Kuvasta 3 voi nähdä, että yksi muutos on tapahtunut
    
    Succeeded: 1 (Changed=1)

Saman funktion suorittaminen uudestaan ei luo muutoksia järjestelmään (kuva 4).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/9c491937-f672-4d19-b5ef-3da85ec7b363)

> Kuva 4. Aikaisemman idempotenttifunktion suoritus toisen kerran.

### file

File-moduulilla voidaan tarkistaa mm. tiedoston olemassaolo, sisältö ja käyttöoikeudet (VMware 2023c).

    sudo salt-call --local -l info state.single file.managed /tmp/hellotero

Yllä oleva komento tarkoittaa:

- file.managed = hallinnoi tiedostoa; tässä tapauksessa ei ole määritelty mistä ladataan, joten luodaan tyhjä tiedosto polkuun
- /tmp/hellotero = polku; tiedosto luodaan tänne

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/188ce3ad-e1fc-44f3-b847-03ed38853ccf)

> Kuva 5. Esimerkkiajon tulos (sudo salt-call --local -l info state.single file.managed /tmp/hellotero)

### service

Service-moduulilla voidaan hallinnoida daemoneiden käynnistystä ja lopetusta (VMware, Inc. 2023d).

    sudo salt-call --local -l info state.single service.dead apache2 enable=False

- service.dead = varmista, että palvelu (service) ei ole päällä ja lopeta sen ajaminen, jos se on päällä
- apache2 = palvelu/daemon/skripti (apache2 on webpalvelin)
- enable=False = enable viittaa palvelun käynnistykseen järjestelmän käynnistyessä; False -> älä käynnistä

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/76817a64-b0cf-4940-a587-445fdda51a5d)

> Kuva 6. Apache2 ei ole päällä eikä sitä yritetä käynnistää.

### user

User-moduulilla hallinnoidaan järjestelmän käyttäjiä. User-moduulissa on vain kaksi funktiota; present (läsnä) ja absent (poissa). (VMware, Inc. 2023e)

    sudo salt-call --local -l info state.single user.present terote08

- user.present = tarkistetaan onko, käyttäjä järjestelmässä; jos ei ole, sellainen luodaan
- terote08 = käyttäjänimi

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/725199f8-0d14-4978-be16-4bb3b71f4c3c)

> Kuva 7. Käyttäjä terote08, läsnäolo pakollinen!

### cmd

Cmd-moduulilla hallinnoidaan omavaltaisia (itse määritettyjä) komentoja (VMware, Inc. 2023f).

    sudo salt-call --local -l info state.single cmd.run 'touch /tmp/foo' creates="/tmp/foo"

- cmd.run = suorita komento
- 'touch /tmp/foo' = komento; touch luo uuden tyhjän tiedoston (Rubin et al 2022)
- creates="/tmp/foo" = ehto komennon suoritukselle; suoritetaan vain, jos /tmp/foo EI ole olemassa

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/1be2c2c5-a2f6-4083-bb82-5ef0bc6e8811)

> Kuva 8. Ehto toteutuu, itse määritelty komento suoritetaan.

## d) Tietoa koneesta

Saltin avulla voi kerätä tietoa järjestelmästä (Karvinen 2023a).

    sudo salt-call --local grains.items

Komento tulostaa terminaaliin tiedot käyttöjärjestelmästä, prosessorista, ip-tiedot ym.

Niitä voi erotella, jos ei halua kaikkia tietoja tulostuvan (Karvinen 2023a).

    sudo salt-call --local grains.item valinta1 valinta2 valinta3

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/71399293-c01f-4bd7-8320-86218d88d380)

> Kuva 9. Saltin avulla saadut tiedot.

Kuvassa 9 on käytetty grains.item -funktiota.

- cwd = current working directory; tämänhetkinen hakemistosi
  - cwd viittaa tilaan ja pwd (print working directory) on komento (Davies 2022)
- ip_interfaces = internetprotokollan liitännät
  - enp0s3 on viittaa ethernet-yhteyteen (ethernet network peripheral 0 serial 3)
    - koska tiedetään jo valmiiksi laitteen olevan virtuaalinen, on ethernet-yhteyskin virtuaalinen; hostkoneeni ei ole ethernet-kaapeliin yhdistettynä
  - lo on loopbackosoite eli osoite, jolla kone voi kutsua itseään
- os = operating system eli käyttöjärjestelmän nimi

## Lähteet

BSD System Manager's Manual, 2023. sudo Documentation. Manual Page sudo.

Davies, C., 2022. What is the difference between cwd and pwd?. StackExchange. Luettavissa: https://unix.stackexchange.com/questions/709896/what-is-the-difference-between-cwd-and-pwd Luettu: 2023/10/27

Hatch, T.S. et al., 2023. salt-call Documentation. Manual Page salt-call.

Karvinen, T., 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/ Luettu: 2023/10/26

Karvinen, T., 2023b. Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/ Luettu: 2023/10/26

Karvinen, T., 2023c. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/ Luettu: 2023/10/26

Oxford University Press, 2023. pkg abbreviation. Luettavissa: https://www.oxfordlearnersdictionaries.com/definition/english/pkg Luettu: 2023/10/26

Rubin, P. et al, 2022. User Commands. Touch manual pages. 

VMware, Inc., 2023a. salt.states.pkg. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.pkg.html Luettu: 2023/10/27

VMware, Inc., 2023b. salt.modules.state. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.state.html#salt.modules.state.single Luettu: 2023/10/27

VMware, Inc., 2023c. salt.modules.file. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/modules/all/salt.modules.file.html Luettu: 2023/10/27

VMware, Inc., 2023d. salt.states.service. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.service.html Luettu: 2023/10/27

VMware, Inc., 2023e. salt.states.user. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.user.html Luettu: 2023/10/27

VMware, Inc., 2023f. salt.states.cmd. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.cmd.html Luettu: 27.10.2023

Wigmore, I., 2016. What is idempotence?. TechTarget. Luettavissa: https://www.techtarget.com/whatis/definition/idempotence Luettu: 27.10.2023
