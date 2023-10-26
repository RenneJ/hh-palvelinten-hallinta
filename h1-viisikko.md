# H1 Viisikko (Karvinen 2023a)

## Tiivistelmät

### Create a Web Page Using Github (Karvinen 2023b)

- Luo tili Githubiin
- Navigoi "Repositories" -näkymään
  - Klikkaa "New" ja täytä kentät/valinnat (nimi, README.md, public/private, lisenssi)
- Klikattuasi "Create Repository" omaat uuden Github-sivun
  - Sivulla ei tosin ole README.md:n ja lisenssin lisäksi muuta sisältöä
- Uuden tiedoston lisääminen onnistuu sivulla näkyvän "Add file" -painikkeen kautta
  - Muista tieodostoa nimetessäsi tiedostopääte (.md eli markdown-tiedosto on hyvä)

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

 ## Saltin asennus Debian 12:lle

 Noudatan tässä osatehtävässä Tero Karvisen (2023a) vinkkejä. Olen aikaisemmalla opintojaksolla asentanut Debian 12:n (Bookworm), joten käytän sitä tämän ja tulevien tehtävien tekemiseen. 

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a850dbc7-ee3f-49ed-8d59-09b751b3b82a)

> Kuva 1. Teron ohjeet salt-minionin asentamiseen Debian 12:lle.

**HUOM!** Näitä ohjeita ei suositella käytettävän tuotantokoneelle.

Ensimmäinen rivi, komennolla mkdir on omassa tapauksessani turha. Tarkistin kyseisen hakemiston olevan olemassa listauskomennolla:

    ls /etc/apt/keyrings

Komentoriville tulostuisi "no such file or directory", jos hakemistoa ei löytyisi.

Suoritin loput komennot ohjeiden mukaisesti (kuva 1). Tämän jälkeen testasin, että asennus onnistui (kuva 2).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7ed290f4-915d-4e74-a018-c75859828b57)

### Lähteet

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/ Luettu: 26.10.2023

Karvinen, T. 2023b. Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/ Luettu: 26.10.2023

Karvinen, T. 2023c. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/ Luettu: 26.10.2023
