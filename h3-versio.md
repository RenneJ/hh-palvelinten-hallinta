# H3 Versio [(Karvinen 2023)](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h3-versio)

## a) Online. Tee uusi varasto GitHubiin. Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "winter".

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/261be2d1-620d-479a-a1da-cb72ef079950)

> Kuva 1. Haku omasta GitHub-varastolistauksesta. Hakusanalla *winter* ei ole osumia. 

Uuden varaston (suomeksi myös säilö/säilytyspaikka) luonti GitHubiin onnistuu navigoimalla omalle **Repositories**-sivulle (kuva 2). Tämä toiminto tosin edellyttää käyttäjätilin luomista GitHubiin. Uutta säilöä pääsee luomaan 
**New**-painiketta klikkaamalla (kuvassa 2 oikeassa reunassa vihreä painike).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/c1f13d4c-6dbe-4731-8873-d4aae8ccbf48)

> Kuva 2. Repositories-sivun näkymä.

Teron vinkeissä uuden säilön luomiseksi (Karvinen 2023) suositellaan tehtävän README.md ja lisenssi heti säilön luomisvaiheessa. Olen aiemmin tehnyt muiden ohjeiden mukaan ja luonut tyhjän säilön. Noudatan tässä tehtävässä Teron vinkkejä.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/58754021-6211-4076-b5d0-8b97edb4f3b8)

> Kuva 3. Näkymä uuden säilön luomisesta GitHubiin.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f3267452-59f1-46d8-834a-d4b05a705786)

> Kuva 4. Uusi säilö löytyy!

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.

Kloonaamisella tarkoitetaan tarkan kopion luomista säilöstä paikalliseen hakemistoon (Git 2023, `man git clone`). Ennen kloonaamista on siis hyvä navigoida siihen hakemistoon, jonne haluaa kloonata säilön GitHubista. 

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

Kokeillaan seuraavaksi tehdä muutoksia ja lisäyksiä säilöön niin, että ne näkyvät myös web-liittymässä.

Mielestäni muutosten tekeminen lisenssiin ei ole missään tapauksessa järkevää, joten muutetaan README.md -tiedostoa. Lisätään myös uusi tiedosto säilöön.

    $ nano README.md
    $ nano <filename.extension>

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/937887e8-a2f2-4d21-9f0f-2e7d654b042a)

> Kuva 9. README.md ennen muutoksia.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/1b1c57ae-ef68-4322-b8ca-b8489ba4367e)

> Kuva 10. Uuden tiedoston sisältö.

Muutosten ja lisäysten jälkeen, kerrotaan gitille muutoksista, jotka halutaan tallentaa.

    $ git add .         # indeksoidaan tehdyt muutokset säilön kaikille tiedostoille (Git 2023, `man git add`)
    $ git commit        # tallennetaan tehdyt muutokset, tallennetaan myös muutoksen 
                        # tekijä ja tekijän commit-viesti (Git 2023, `man git commit`)
    $ git pull          # yhdistää remoten (GitHubissa olevan säilön) nykyiseen paikalliseen versioon,
                        # jos nykyinen paikallinen versio on jäljessä remotea (Git 2023, `man git pull`)
    $ git push          # päivittää remoten paikallisesti tehtyjen muutosten perusteella (Git 2023, `man git push`)

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/2b31f5fe-0e2c-4f50-a981-9c29936ff2bf)

> Kuva 11. Gitin peruskäyttö terminaalista. -m optio commit-komennossa mahdollistaa viestin kirjoittamisen suoraan komentorivillä.

Päivitetään GitHub-sivu ja tarkistetaan näkyvätkö muutokset (kuva 12).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/3e871ea3-1461-4b5a-a31f-692950f4dae4)

> Kuva 12. Näkymä GitHubissa muutosten pushaamisen jälkeen.

## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

Tehdään tyhmä muutos.

    $ nano self_destruct.py

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/46a0278b-2071-42fa-aa9f-dfbdc15ee41c)

> Kuva 13. Itsetuhosekvenssi lisätty hakemistoon.

Lisätään muutos gitiin.

    $ git add self_destruct.py    # lisätään vain juuri luotu tiedosto
    $ git status                  # näytetään nykyisen työskentelytilan vaihe (Git 2023, `man git status`)

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f5dd34d6-5f78-4b71-b7e8-94527c5dc5f1)

> Kuva 14. Tiedosto lisätty ja git status.

Suoritetaan tehtävänannon komento ja tarkistetaan muutos gitissä sekä paikallisesti.

    $ git reset --hard
    $ git status
    $ ls

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/9ac52f34-b968-4406-b5f5-f4bad72f526f)

> Kuva 15. Resetointi, git status ja hakemiston paikallinen sisältö.

## d) Tukki. Tarkastele ja selitä varastosi lokia.

Gitin valvomat tiedostot (commit-suoritukset) saadaan terminaalissa näkyviin komennolla `git log` (Git 2023, `man git log`). Lokeista voidaan tarkastella commitin tunniste, tekijä, ajankohta ja viesti (kuva 16).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/3ab7935d-f072-49fd-bf11-dc88e4a4af44)

> Kuva 16. Säilön lokitus.

Tarkempaa tietoa commiteista voidaan hankkia komennolla `git diff` (Git 2023, `man git diff`). Ko. komennolla voidaan tarkastella kahden eri commitin eroja (rivimuutoksia, tiedostojen lisäyksiä/poistoja). Kuvassa 17 näkyy terminaaliin tulostuva output `git diff <commit1>..<commit2>` komennosta.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/438d55fc-8aae-4359-bcba-814be267a81f)

> Kuva 17. `git diff` terminaalissa.

Mielestäni GitHubin vastaava työkalu on selkolukuisempi. Painamalla säilön GitHub-sivulla olevaa **X commits** -linkkiä (vihreän **New**-painikkeen alla, kuva 18) pääsee katselemaan commit-historiaa, josta näkee tehdyt commitit. Listanäkymästä voi valita haluamansa commitin ja tarkastella tehtyjä muutoksia (kuva 19).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/12a16179-3c67-4653-a111-fd55aa26db48)

> Kuva 18. Linkki commit-historiaan. Kuvassa sininen teksti oikeassa reunassa.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/6dcfb7d6-cf4a-414e-81b0-af46210fc788)

> Kuva 19. Yksittäisen commitin muutokset sen vanhempi-versioon (parent). Tässä tapauksessa parent on "Initial commit".


## Lähteet

Git 2023. Git manual. Git v. 2.34.1.

Karvinen, T. 2023. Infra as Code 2023 - H3 Versio. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h3-versio Luettu: 2023/11/11
