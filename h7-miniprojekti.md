# Salt-moduuli

## Kuvaus projektista

Tämä projekti on lopputyö Tero Karvisen opintojaksolle Palvelinten hallinta ([Karvinen 2023a](https://terokarvinen.com/2023/configuration-management-2023-autumn/))

Tavoitteena on saada luotua moduuli, jolla voidaan asentaa määrätylle orjalle postgresql-tietokanta ja pääkäyttäjä.
Muille orjille luodaan oma käyttäjä tietokantaan eli jokaiselle orjalle ei ole tarkoitus alustaa omaa tietokantaa.

Jokaiselle orjalle tulee asentaa myös neovim, java ja git. Neovimiin tulee lisätä java-devaukseen sopiva lisäosa [nvim-jdtls](https://github.com/mfussenegger/nvim-jdtls/tree/master).

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

## Lähteet

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/ Luettu: 2023/12/10

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers Luettu: 2023/12/10
