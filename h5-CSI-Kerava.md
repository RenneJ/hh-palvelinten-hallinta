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

# Lähteet

Karvinen, T. 2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/ Luettu: 2023/11/24

Karvinen, T. 2023. Infra as Code 2023. H5 CSI Kerava. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava) Luettu: 2023/11/24

