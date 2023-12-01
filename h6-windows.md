# H6 Windows ([Karvinen 2023](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h6-windows))

## x) Lue ja tiivistä

### Saltin käyttö Windowsilla.

### Ph h5 ([Hautala 2018](https://sampohautala.wordpress.com/2018/11/24/ph-h5/))

Windows orjana:

- Salt-masterin asennus herrakoneelle
    - ip-osoite muistiin, tarkista salt-versio `salt-master --version`
- Lataa Windowsille salt-minion (sama versio kuin masterilla)
    - https://repo.saltproject.io/windows/
- Aja ladattu asennustiedosto järjestelmänvalvojana
    - Seuraa prompteja, täytä masterin ip-osoite ja nimeä salt-minion
    - Käynnistä orja
- Herrakoneella hyväksy uusi avain `sudo salt-key -A`
- Testaa herra-orja -arkkitehtuuri esim. grains-kutsulla `sudo salt '*' grains.item cpu_model`

Windows-orjan konfigurointi:

- Määrittele windows-ohjelmien repot ***(Huom! tutustu saltin dokumentaatioon windowsohjelmien paketinhallinasta; [Windows Package Manager](https://sampohautala.wordpress.com/2018/11/24/ph-h5/))***

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/796e79e9-9fb1-40c9-bd2c-b3d96bdada47)

> Kuva 1. Kuvakaappaus Hautalan antamista komennoista repojen päivittämiseksi. Kuvan [lähde](https://sampohautala.wordpress.com/2018/11/24/ph-h5/).

- Ohjelmien paikallisen metadatan luominen `sudo salt -G 'os:windows' pkg.refresh_db`
- Paketinhallinta toimii nyt windows-orjalle
    - `sudo salt '*' pkg.install putty` komentoo asentaa puttyn orjille
- Tarkistus paikallisesti orjalla

### Installing Windows 10 on a virtual machine ([Halonen, Ollikainen Rajala 2023](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md))

# Lähteet

Hautala, S. 2018. Ph h5. Luettavissa: https://sampohautala.wordpress.com/2018/11/24/ph-h5/ Luettu: 2023/12/01

Karvinen, T. 2023. Infra as Code 2023. H6 Windows. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h6-windows Luettu: 2023/12/01
