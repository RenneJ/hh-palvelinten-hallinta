# Salt-moduuli

## Kuvaus projektista

Tämä projekti on lopputyö Tero Karvisen opintojaksolle Palvelinten hallinta ([Karvinen 2023a](https://terokarvinen.com/2023/configuration-management-2023-autumn/))

Tavoitteena on saada luotua moduuli, jolla voidaan asentaa määrätylle orjalle postgresql-tietokanta. Tietokantaan lisätään kaksi käyttäjää - admin ja developer.
Tietokanta konfiguroidaan niin, että sinne voi ottaa yhteyden lähiverkon sisällä.

Jokaiselle orjalle tulee asentaa postgresin lisäksi neovim, java ja git.

## Työskentely-ympäristön luominen

Tähän projektiin alustan 4 virtuaalikonetta. 

- 1 salt-master
- 1 tietokantapalvelin (orja)
- 2 devaajan konetta (orja)

Hostin tiedot:

- OS: Ubuntu 22.04LTS
- Prosessori: 11th Gen Intel(R) Core(TM) i3-1115G4 @ 3.00GHz
- Prosessori arkkitehtuuri: x86_64 (AMD64)
- Muisti (total): 8GB
- Virtualisointi: libvirt

Käytän virtuaalimasiinoiden luomiseen Vagrantia (2.2.19). Kuvassa 1 näkyvä Vagrantfile on muokattu Teron esimerkistä ([Karvinen 2023b](https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers)). Tiedostoon on lisätty alustettavaksi yksi orja lisää sekä koneiden nimet ja ip-osoitteet ovat muutettu.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/285c9529-276d-4519-90f4-f13596b39627)

> Kuva 1. Vagrantfile virtuaalikoneiden luomiseen.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/3bbf564c-a86a-4852-8a99-7c3f6e747d5b)

> Kuva 2. Orjien avainten hyväksyminen master-koneella.

On mahdollista, että kestää hetken ennen kuin orjat ilmoittavat herralle. Tässä tapauksessa välittömästi koneiden pystyttämisen jälkeen, kun kokeilin `sudo salt-key` eli avainten listausta herralla, ei tulosteeseen printtaantunut mitään. Tarkistin yksitellen orjilta, salt-minion daemonin statuksen `sudo systemctl status salt-minion.service`. Status oli jokaisella **Active(running)**, minkä jälkeen kokeilin `sudo salt-key` komentoa herralla uudestaan. Nyt listaukseen ilmestyi orjien avainten lähetykset. Kaikki avaimet hyväksytään kuvan 2 osoittamalla tavalla.

Testataan vielä, että arkkitehtuuri toimii "Hello World!" -tyyppisellä tilalla.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/fca88a4a-72ab-4f59-8dc3-6b8f37a7f25d)

> Kuva 3. Tilan ajo ja tilamääritystiedoston sisältö.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/c6d34f8e-d18a-4b8a-b946-83730918a6f7)

> Kuva 4. Testitilan tarkistus paikallisesti.

## Moduulin rakentaminen

### Kaikille orjille asennettavat paketit

Pakettien asennus on tullut tutuksi opintojakson aikana. Tilatiedoston polku `/srv/salt/dev_tools/init.sls`. Tilatiedoston sisältö:

    postgresql:
      pkg.installed
    default-jdk:
      pkg.installed
    neovim:
      pkg.installed
    git:
      pkg.installed

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/e3173bbb-7816-4757-8c23-c12caad287d9)

> Kuva 5. Ensimmäinen ajo. Näemmä kestää liian kauan...

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/6a4da224-fef5-4b1c-bf98-3c11fbb4fa47)

> Kuva 6. Toinen ajo. Huomioitavaa! Ei "Changed" -merkintää.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0738146d-8ee5-497b-8811-266d9b3a63c4)

> Kuva 7. Pakettien asentumisen tarkistus paikallisesti.

### Palvelimen konfigurointi

Aloitin tietokantapalvelimen konfiguroimsen selvittämllä, miten voidaan ottaa yhteys ei-paikalliseen postgresql-tietokantaan. 

[Tämän](https://blog.devart.com/configure-postgresql-to-allow-remote-connection.html) ohjeen perusteella tein muutoksen `/etc/postgresql/13/main/postgresql.conf`-tiedostoon (Alexander 2023).

        - # listen_addresses = 'localhost*
        + listen_addresses = '*'

Alexanderin (2023) artikkelissa neuvotaan myös muuttamaan `/etc/postgresql/<postgres versio>/main/pg_hba.conf`-tiedostoa. Artikkelissa kehotetaan muuttamaan IPv4 yhteys sallimaan kaikki yhteydet (0.0.0.0/0). Itse kuitenkin muokkasin tiedostoa niin, että ainoastaan samasta verkosta tulevat yhteyspyynnöt hyväksytään.

|![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/bbd3845e-44f6-45f9-bf88-e29a146a69bb)|![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/1dd2622e-d4d8-46b6-b7b6-2c903de4047d)|
|---|---|

> Kuvat 8 & 9. `pg_hba.conf`. Oletusarvot (vas.) ja tekemäni muutokset.

Luodaan salt-tilaan määritystiedosto `/srv/salt/pg_config/init.sls`.

Tällä määritystiedostolla käsketään palvelin-orjaa:

- lataamaan herralta konfigurointitiedostot `pg_hba.conf` ja `postgresql.conf`.
- uudelleenkäynnistämään postgres-daemon, kun mikä tahansa .conf-tiedosto muuttuu postgresin konffaushakemistossa
- luomaan tietokantaan admin-käyttäjän sekä tavallisen käyttäjän

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/b821b2b4-fe40-4113-b8cc-361f2d2221ee)

> Kuva 10. `/srv/salt/pg_config/init.sls` sisältö. Salasanat sensuroitu.

Määritystiedostoa kirjoittaessa minua huolestutti salasanojen näkyvyys. En kuitenkaan keksinyt mitään keinoa, jolla ne saataisiin pois näkyvistä `init.sls`-tiedostosta.

Tarkistin kuitenkin tietokannasta, että eiväthän salasanat ole sinne tallennettu plaintextinä (kuva 11). Ei ole onneksi. Samalla tuli varmistetuksi, että käyttäjien luominen saltilla onnistui.

Kirjautuminen tietokantaan server-koneella.

        $ psql -U admin -d postgres        # -U = user ja -d = database
                                           # jos -d optiota ei käytä, yrittää psql hakea admin-nimistä tietokantaa

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/042749d0-d7ba-4a3e-b10b-85378b181be5)

> Kuva 11. Salasanat tallennettuna hash-arvoina tietokannassa.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/ea9bee26-d438-4121-8f10-3e149a5bcc9f)

> Kuva 12. Kirjautuminen tietokantaan toiselta koneelta. -h = host

## Lähteet

Alexander, H. 2023. How to Configure PostgreSQL for Remote Connections: A Beginner’s Guide. Luettavissa: https://blog.devart.com/configure-postgresql-to-allow-remote-connection.html Luettu: 2023/12/10

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/ Luettu: 2023/12/10

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers Luettu: 2023/12/10
