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
- pkg.installed = tarkista onko järjestelmä tilassa, johon on asennettu paketti (Karvinen 2023c)
- tree = paketti, jota tarkistetaan

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/5f071c97-97e4-4d63-9dc3-c67c05e0cfe5)

> Kuva 3. Esimerkkiajo: sudo salt-call --local -l info state.single pkg.installed tree

### file

File-funktiolla voidaan tarkistaa tiedoston olemassaolo

### Lähteet

BSD System Manager's Manual, 2023. sudo Documentation. Manual Page sudo.

Hatch, T.S. et al. 2023. salt-call Documentation. Manual Page salt-call.

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/ Luettu: 26.10.2023

Karvinen, T. 2023b. Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/ Luettu: 26.10.2023

Karvinen, T. 2023c. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/ Luettu: 26.10.2023

Oxford University Press, 2023. pkg abbreviation. Luettavissa: https://www.oxfordlearnersdictionaries.com/definition/english/pkg Luettu: 26.10.2023

VMware, Inc., 2023a. salt.states.pkg. Salt Project. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.pkg.html Luettu: 27.10.2023
