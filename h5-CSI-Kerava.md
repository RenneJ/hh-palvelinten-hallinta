# H5 CSI Kerava ([Karvinen 2023](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava))

## x) Lue ja tiivistä

### Apache User Homepages Automatically – Salt Package-File-Service Example ([Karvinen 2018](https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/))

- Ennen automatisointia asenna käsin
- Tiedätkö mitä tiedostoa muutit, jos käytit graafista käyttöliittymää tai komentoa?

      $ find -printf "%T+ %p\n"|sort
      # ...
      2018-04-03+13:38:25.3595187980 ./mods-enabled/userdir.conf
      2018-04-03+13:38:25.3595187980 ./mods-enabled/userdir.load
      (Karvinen 2018)

- Yllä olevassa esimerkissä on komentoriville syötetyn konfigurointikomennon tuloste
- Konfigurointikomennon luomat tiedostot löydetään `find`-komennolla
  - `-printf` -toiminto ja sen parametri määrittelevät tulosteen muotoilun
    - `%T+` muokkausaika
    - `%p` tiedostonimi ja polku
    - `\n` uusi rivi
    - `sort`-komento järjestää tulosteen aakkosjärjestykseen tai numeroiden kohdalla pienimmästä suurimpaan

- Kun käytät komentoa salt-tilassa, tee siitä idempotentti
- Kannattaa automatisoidessa saltin `service.running` -funktio kohdistaa tiedostoon, ei komentoon

## a) CSI Kerava. Näytä 'find' avulla viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostasi.

Käytetään x) -kohdan esimerkkiä `find -printf '%T+ %p \n' | sort`.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/820f86e6-0e96-422c-9e4d-cf9cd31449ec)

> Kuva 1. Ote `find`-komennon tulosteesta kotihakemistossani.

Kuvassa 1 näkyy alimmilla riveillä uusimmat muokkaukset tiedostoihin kotihakemistossani. Näyttää siltä, että Teams keräilee koneelleni jatkuvasti jotain tietoja.

Siirrytään seuraavaksi `/etc/`-hakemistoon ja tarkastetaan samalla komennolla konfigurointihakemistoon tehdyt muutokset.

      $ cd /etc
      $ find -printf '%T+ %p \n' | sort

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/65bdc95b-e5d1-491c-9219-01eea52a6b2f)

> Kuva 2. Ote `find`-komennon tulosteesta konfigurointihakemistossani.

## b) Gui2fs. Muokkaa asetuksia jostain graafisen käyttöliittymän (GUI) ohjelmasta käyttäen ohjelman valikoita ja dialogeja. Etsi tämä asetus tiedostojärjestelmästä.

Muokataan VScoden asetuksia graafisesta käyttöliittymästä ja etsitään konfigurointitiedosto. Kokeillaan myös muuttaa asetus takaisin tällä kertaa tekstitiedostoa muuttamalla ja todennetaan muutos graafisessa käyttöliittymässä.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a7d60f33-dcac-43d0-989b-32787febdc53)

> Kuva 3. VScoden näkymä asetusten muuttamiseen.

      File -> Preferences -> Settings
            tai
      Ctrl + ,

Muutetaan tabin arvoa. Kuvasta 3 voi nähdä, että se on oletusarvoisesti 4.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/b5fbafbf-6e16-44bb-95e6-e117a07c2444)

> Kuva 4. Muutos tehty asetuksiin.

Todensin asetuksen muutoksen astuneen voimaan uudelleenkäynnistämällä VScoden.

Nyt yritetään paikallistaa asetustiedosto.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/98664bb1-e2e9-4eda-8911-409ec535f88c)

> Kuva 5. `find`-komento taas kotihakemistossa.

En äkkiseltään löytänyt mitään sopivaa osumaa kotihakemistosta (kuva 5) tai `/etc`-hakemistossa. Kokeilin myös lisätä komentoon `| grep vscode`. Mutta tämäkään muutos ei liiemmin helpottanut, joten turvauduin googlaamaan mistä löytyy VScoden konfigurointitiedosto (hakusanoilla *vscode config file*). Hakutulokset osoittivat, että asetustiedosto löytyy käyttäjän kotihakemistosta `.config/Code/User/settings.json`. Olisinpa tajunnut grepata Code enkä vscode...

      ~$ sudo find -printf '%T+ %p \n' | sort | grep Code

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/89c77649-27e2-4788-af70-44f909bf8aed)

> Kuva 6. `find`-komennon ajo kotihakemistossa. Tulosteessa vain ne rivit, joissa on sana 'Code'.

Kuvassa 6 ensimmäisellä rivillä näkyy VScoden asetustiedosto.

Kokeillaan tehdä muutos siihen.

      $ sudo nano .config/Code/User/settings.json

Tiedostossa ei ollut kuin 4 riviä (kuva 7). Palautetaan tabikoko takaisin 4:ään (kuva 8).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/958df9fd-f385-400f-b31e-1eff4e23dcbb)

> Kuva 7. VScoden settings.json.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/af05d38b-1e42-45e4-8a1d-db8e7a078542)

> Kuva 8. Muutos tehty!

Tarkistetaan vielä tuliko muutos voimaan graafiseen käyttöliittymään.

Tulihan se (kuva 9)!

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/865f4f8c-8b17-482a-8e8f-81dde7abb4b6)

> Kuva 9. Muutos näkyy myös graafisessa käyttöliittymässä.

## c) Komennus. Tee Salt-tila, joka asentaa järjestelmään uuden komennon.

Käytetään edellistä viikkotehtävää (h4 Demonit) varten alustamaani Vagrant-labraa (master ja kaksi orjaa).

Ensin komennon asentaminen käsin yhdelle koneelle (masterilla):

      $ nano output                         # nimesin esimerkkini outputiksi
      $ chmod +x output                     # lisätään kaikille ajo-oikeus tiedostoon
      $ ./output                            # testataan toimiiko komennon ajo
      $ sudo mv output /usr/local/bin/      # siirretään tiedosto, jotta komennon voi ajaa mistä hakemistosta tahansa
      $ output                              # testataan toimiiko

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0cc61fa2-5f89-4967-9005-6c665997e1b0)

> Kuva 10. Oma komento toimii!

Lähdetään määrittelemään salt-moduulia.

      $ sudo mkdir /srv/salt/commands; cd /srv/salt/commands
      $ sudo ln -s /usr/local/bin/output                        # luodaan symlinkki nykyiseen hakemistoon
      $ sudo nano init.sls                                      # YAML-tiedosto salt-moduulin ajoa varten (kuva 11)
      $ sudo salt '*' state.apply commands                      # komennetaan orjat ajamaan commands-hakemiston sisältö

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/914d12ea-9242-4201-8fff-8044c8bd4b11)

> Kuva 11. Salt-konfiguraatiot oman komennon asennusta varten orjille.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/52998ce9-2434-42b9-aade-b35116a4e886)

> Kuva 12. Komennon `sudo salt '*' state.apply commands` tuloste.

Uusi tiedosto näyttäisi olevan luotuna molemmille orjille. Testataan sitä vielä ottamalla ssh-yhteys toiseen orjaan ja kokeillaan ajaa komento `output`.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a79a05b2-1e24-43c9-967f-747ee72dfe96)

> Kuva 13. Access denied! Komento tosin löytyy, joten jotain onnistui!

Oikeudet ovat väärin! Saltin default-määritykset file.managed -funktiossa ovat väärät tähän tapaukseen. Muutetaan commands-moduulin määrityksiä (kuva 14). Tässä kohtaa tehtävien tekemistä muistinkin edelliseltä oppitunnilta, että tämähän käytiinkin läpi. Tero muistini mukaan suosittele antamaan oikeusmääritykset saltissa merkkijonona ei lukuina.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7a517c9d-b322-4fa4-9f9e-8481a16457a6)

> Kuva 14. Muutokset init.sls -tiedostoon.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/03755a49-0af9-440f-b31d-5add59602282)

> Kuva 15. Uusi state.apply ajo. Muutoksia tapahtui!

Kokeillaan nyt ajaa paikallisesti uusi komento 'output'.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/fc139d05-6c51-4696-bf5e-3b34111f28f1)

> Kuva 16. Ajo onnistuu!

Jätin kuvaan 16 näkyviin samat rivit kuin kuvassa 13 osoittaakseni, että en tehnyt paikallisesti mitään muutoksia ws02-nimisessä koneessa.

## d) Apassi. Tee Salt-tila, joka asentaa Apachen näyttämään kotihakemistoja.

Ensin käsin masterilla, sitten automatisoimaan.

Kotihakemistojen käyttöönotto apache2:een tapahtuu komennolla `a2enmod userdir` (a2enmod 006).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/ba7ce995-8b4f-4041-b76d-3a5a24a8d76a)

> Kuva 17. Tuloste `a2enmod userdir` komennosta.

Daemon pitää vielä uudelleenkäynnistää. Sen jälkeen selvitetään mitä muuttui järjestelmässä.

Aloitetaan selvitystyö `/etc/apache2` hakemistosta. Ajetaan komento, jota käytimme aiemmin `find -printf '%T+ %p \n' | sort`.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/2ee50060-bf2a-4494-b992-3b5bf1c63db4)

> Kuva 18. Salapoliisityö kävi helposti tällä kertaa!

Kuvassa 18 näkyy `find` komennon tulosteen kolme viimeistä riviä (eli uusimmat muokkaukset hakemistossa `/etc/apache2`). Käyttöönotettujen modien hakemisto `mods-enabled` sisältää ainoastaan symlinkkejä (kuva 19).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0e3419dd-8f4d-42e2-9bfc-070e33c548d1)

> Kuva 19. `/etc/apache2/mods-enabled` sisältö.

Eli `sudo a2enmod userdir` teki samat asiat kuin:

      $ ln -s /etc/apache2/mods-available/userdir.conf /etc/apache2/mods-enabled/userdir.conf
      $ ln -s /etc/apache2/mods-available/userdir.load /etc/apache2/mods-enabled/userdir.load

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/411e4bc2-861e-4f7e-8e85-224691225590)

> Kuva 20. `userdir.conf` tiedoston sisältö.

`userdir.conf` tiedoston sisällöstä päätellen apache2-käyttäjän kotihakemisto on `~/public_html`. Tehdään sinne index.html.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/307093b0-db4c-4047-a32f-66f6f372a3dc)

> Kuva 21. Käyttäjän oman kotisivun luominen.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/9c37492a-8fe0-44d2-b94c-b138c33358c4)

> Kuva 22. Näkymä kotisivusta lynx-selaimessa, url = localhost/~vagrant

Kokeillaan automatisoida salt-tila, jossa orjat saavat omat kotisivut.

Muokataan hieman Teron (Karvinen 2018) esimerkkiä. Lisätään tilamäärityksiin käyttäjän kotihakemistoon tyhjä index.html -tiedosto polkuun `~/public_html`.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/45e553c0-797b-43ac-bfb3-96a6e8ef0740)

> Kuva 23. Tilatiedoston sisältö.

Tyhjän index-tiedoston luominen onnistuu helposti, koska kaikkien labrani käyttäjänimi on sama - vagrant.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/d3d35583-e427-406a-ad98-84e51e624ae5)

> Kuva 24. Orjalle on luotu oma kotisivu, jota käyttäjällä on oikeus muokata.

## e) Ämpärillinen. Tee Salt-tila, joka asentaa järjestelmään kansiollisen komentoja.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/964b621f-c08b-47fb-b8ec-2cad0e2d4ecd)

> Kuva 25. Komentojen luonti hakemistoon.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/8df1d965-9336-44ec-b704-a07124b57509)

> Kuva 26. Hakemiston kopiointi `/usr/local/bin` hakemistoon.

# Lähteet

a2enmod 2006. `man a2enmod`.

Karvinen, T. 2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/ Luettu: 2023/11/24

Karvinen, T. 2023. Infra as Code 2023. H5 CSI Kerava. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava) Luettu: 2023/11/24



