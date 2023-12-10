# Salt-moduuli

## Kuvaus projektista

Tämä projekti on lopputyö Tero Karvisen opintojaksolle Palvelinten hallinta ([Karvinen 2023](https://terokarvinen.com/2023/configuration-management-2023-autumn/))

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

Käytän virtuaalimasiinoiden luomiseen Vagrantia (2.2.19). Kuvassa 1 näkyvää Vagrantfile on muokattu Tero

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/9570686e-dcf8-4c2b-b056-03ecce923105)

> Kuva 1. Vagrantfile virtuaalikoneiden luomiseen.
