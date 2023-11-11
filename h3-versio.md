# H3 Versio [(Karvinen 2023)](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h3-versio)

## a) Online. Tee uusi varasto GitHubiin. Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "winter".

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/261be2d1-620d-479a-a1da-cb72ef079950)

> Kuva 1. Hakusanalla *winter* ei löydy säilöä. 

Uuden varaston (suomeksi myös säilö/säilytyspaikka) luonti GitHubiin onnistuu navigoimalla omalle **Repositories**-sivulle (kuva 1). Tämä toiminto tosin edellyttää käyttäjätilin luomista GitHubiin. Uutta säilöä pääsee luomaan 
**New**-painiketta klikkaamalla (kuvassa 1 oikeassa reunassa vihreä painike).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/c1f13d4c-6dbe-4731-8873-d4aae8ccbf48)

> Kuva 2. Repositories-sivun näkymä.

Teron vinkeissä uuden säilön luomiseksi (Karvinen 2023) suositellaan tehtävän README.md ja lisenssi heti säilön luomisvaiheessa. Olen aiemmin tehnyt muiden ohjeiden mukaan ja luonut tyhjän säilön. Noudatan tässä tehtävässä Teron vinkkejä.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/58754021-6211-4076-b5d0-8b97edb4f3b8)

> Kuva 3. Näkymä uuden säilön luomisesta GitHubiin.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f3267452-59f1-46d8-834a-d4b05a705786)

> Kuva 4. Uusi säilö löytyy!

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.

Kloonaamisella tarkoitetaan tarkan kopion luomista säilöstä paikalliseen hakemistoon (Git 2023). Ennen kloonaamista on siis hyvä navigoida siihen hakemistoon, jonne haluaa kloonata säilön GitHubista. 

    $ cd <my/relative/path/to/directory>

Olen gitiä käyttänyt jo jonkin verran, joten tiedän sen olevan asennettuna. Sen voi tarkastaa komennolla `git --version`. Jos git ei ole asennettu, se käy helposti Debian/Ubuntu -järjestelmillä komennoilla `sudo apt-get update` ja `sudo apt-get install git`.

Käytetään kloonamiseen Teron suosituksesta ssh-yhteyttä. Ssh-yhteyden käyttämiseksi on luotava avainpari; julkinen ja yksityinen (kuva 5). Julkinen avain täytyy myös jakaa GitHubille. Julkisen avaimen jakaminen GitHubissa onnistuu omista asetuksista (klikkaa pyöreää ikonia oikeassa yläkulmassa).

    Settings > SSH and GPG keys > New SSH key

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/8f5f50ea-338b-4aaa-b75b-07cd4247942f)

> Kuva 5. Ssh-avainparin luominen.

Kopioidaan avain (joka päättyy .pub) sisältö **New SSH key** -painikkeesta avautuvaan promptiin ja nimeä avaimesi.

Seuraavaksi voidaan kokeilla kloonausta ssh-yhteydellä. Ssh-urlin voi kätevästi kopioida leikepöydälle säilön sivulta **Code**-painike (kuva 6). Kloonaus (kuva 7) suoritetaan alla olevalla komennolla. Jos annoit ssh-avainaparia luodessasi 
salasanan, kysyy järjestelmäsi antamaan sen.

    $ git clone <ssh url>

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/b5f730b8-ecfa-47eb-90bf-e3c63175d7bf)

> Kuva 6. Säilön main branch. Urlin kopiointi.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/883becb6-1a23-477f-b523-2753b9c46a87)

> Kuva 7. Säilön kloonaus.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/e3935a9b-984f-422e-ab04-921e6bfa88c3)

> Kuva 8. Onnistunut kloonaus. Uusi hakemisto nimeltä *palvelinten-hallinta-winter* löytyy paikallisesti. 

## Lähteet

Git 2023. Git manual. `man git clone`. Git v. 2.34.1.

Karvinen, T. 2023. Infra as Code 2023 - H3 Versio. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h3-versio Luettu: 11.11.2023 
