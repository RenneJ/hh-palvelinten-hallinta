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

- Kun käytät komentoa salt-tilassa, tee siitä idempotentti
- Kannattaa automatisoidessa saltin `service.running` -funktio kohdistaa tiedostoon ei komentoon

# Lähteet

Karvinen, T. 2018. Apache User Homepages Automatically – Salt Package-File-Service Example. Luettavissa: https://terokarvinen.com/2018/04/03/apache-user-homepages-automatically-salt-package-file-service-example/ Luettu: 2023/11/24

Karvinen, T. 2023. Infra as Code 2023. H5 CSI Kerava. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h5-csi-kerava) Luettu: 2023/11/24

