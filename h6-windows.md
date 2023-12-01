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

### Installing Windows 10 on a virtual machine ([Halonen, Ollikainen, Rajala 2023](https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md))

- Lataa iso-tiedosto https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise 
- Avaa VirtualBox -> New -> nimeä kone -> aseta os windows 10:ksi -> aseta RAM 8GB (windows tarvitsee paljon resursseja) -> Create
- Valitse polku asennusta varten ja aseta kiintolevyn koko; allokoi tarpeeksi asennustilaa ettei se lopu kesken
- Aseta prosessoreiden määrä (väh. 4)
- Käynnistä virtuaalikone
    - VirtualBox promptaa asennusmedian valinnan -> valitse lataamasi iso-tiedosto
    - Windowsin asennus alkaa. Valitse itsellesi sopivat paikallistiedot (aika ja pvm). Valitse oikea näppäimistötyyppi.
    - Seuraa prompteja (lisenssin hyväksyntä, maatiedot toistamiseen...)
    - Kirjautumistietojen antamisen kohdalla valitse "Domain join instead" (vasen alakulma)
        - Syötä käyttäjätiedot, älä käytä huonoja salasanoja edes haroituskäytössä!

## a) Asenna Windows virtuaalikoneeseen.

Host: Ubuntu 22.04LTS
Prosessori: 11th Gen Intel(R) Core(TM) i3-1115G4 @ 3.00GHz
Prosessori arkkitehtuuri: x86_64 (AMD64)
Muisti (total): 8GB
Virtualisointi: libvirt

Tässä tehtävässä mukailen yllä olevan tehtävän ohjeita (Halonen, Ollikainen, Rajala 2023). Virtualisointiin käytän avoimen lähdekoodin ohjelmaa: [libvirt](https://libvirt.org/index.html).

Aiemmalla oppitunnilla tehtävänämme oli ladata Windows 10 iso-tiedosto. Tiedoston voi ladata [täältä](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise).

Suoritetaan Windowsin asennus käyttäen libvirtin graafista käyttöliittymää.

    $ virt-manager

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/ca0011b6-d0ce-4acc-a2f6-5f65fc646d8d)

> Kuva 2. Libvirtin Virtual Machine Manager aloitusnäkymä.

Alustetaan uusi virtuaalikone painamalla  VMM:n (Virtual Machine Manager) kuvaketta *File*-valikon alla (Create a new virtual machine).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/21537259-4634-4a45-9366-6857517c6905)

> Kuva 3. Uuden virtuaalikoneen luominen.

Iso image löytyy jo koneelta joten valitaan se ja edetään virtuaalikoneen luomisessa.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/5d9a842c-b314-470f-bba3-1b3973c68310)

> Kuva 4. Asennusmedian ja asennettavan käyttöjärjestelmän valinta.

Browse -> valitse oikea iso image -> Choose volume -> valitse oikea käyttöjärjestelmä; kirjoita windows 10 tekstikenttään -> Forward

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/c09cbbcc-9331-4bae-8c27-2af2daf8b35b)

> Kuva 5. Prosessorin ytimien määrän valinta ja muistin allokointi.

Kuvassa 5 (yllä) näkyy oletusvalinnat. Lisätään muistin määrää hieman valinta-boxin alla näkyy myös vapaana oleva muistin määrä. Lisäsin muistin määrää 1GB:llä (5120MB) ja prosessorin ytimien määrän 4:ään.

-> Forward.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/205ee09b-8212-4e4d-912e-f14f32412ad3)

> Kuva 6. Tallenustilan asetukset.

Jatkoin oletusasetuksilla (Enable storage.. ja 40GB) -> Forward.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/cc12a513-72c6-4639-8007-3dc88d5b2599)

> Kuva 7. Koneen nimeäminen ja määritysten vahvistaminen.

Nimesin koneen lab02 mukaan. Aion lisätä tämän virtuaalimasiinan lab02_wsmasterin alaiseksi.

-> Finish.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/ebc94770-1fc3-423e-9365-cddcef597867)

> Kuva 8. Jotain meni pieleen...

Kokeilin vaihtaa boot-järjestystä, jolloin windows käynnistyi mutta sammui ilman virheilmoitusta. Tämän jälkeen kokeilin lisätä koneeseen muistin määrää mutta sekään ei auttanut. Näiden [ohjeiden](https://raphtlw.medium.com/how-to-set-up-a-kvm-qemu-windows-10-vm-ca1789411760) mukaan onnistuin luomaan uuden koneen. Erona aiempaan:

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/dbb153db-c9b7-41a9-93d6-1b26b5b509e0)

> Kuva 9. Tallenusmedian luominen oikein.

Eipäs käytetäkään defaultia vaan: Select or create custom storage -> Manage -> valitse oikea iso image -> Forward


# Lähteet

Halonen, Ollikainen, Rajala. 2023. Installing Windows 10 on a virtual machine. Luettavissa: https://github.com/therealhalonen/PhishSticks/blob/master/notes/ollikainen/windows.md Luettu: 2023/12/01

Hautala, S. 2018. Ph h5. Luettavissa: https://sampohautala.wordpress.com/2018/11/24/ph-h5/ Luettu: 2023/12/01

Karvinen, T. 2023. Infra as Code 2023. H6 Windows. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h6-windows Luettu: 2023/12/01
