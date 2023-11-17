# H4 Demonit [(Karvinen 2023a)](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit)

## x) Lue ja tiivistä

### Salt Vagrant - automatically provision one master and two slaves [(Karvinen 2023b)](https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

#### Infra as Code - Your wishes as a text file

Oman salt-moduulin luomisvaiheet:

- Hakemiston luonti polkuun `/srv/salt/<oma moduuli>`
- Oman määritystiedoston luonti juuri luotuun hakemistoon `<tiedostonimi>.sls`
- Määritykset YAMLilla; sisennys kaksi välilyöntiä
- Oman moduulin ajo: `$ sudo salt '*' state.apply <oma moduuli>`

#### top.sls - What Slave Runs What States
    
Top-tiedosto määrittelee mikä orja suorittaa minkäkin määrityksen. Ts. koodia, jossa määritellään orja/orjat sekä moduulit, jotka niiden on suoritettava.

- Uusi tiedosto polkuun `/srv/salt/top.sls`
- Määritykset top.sls -tiedostoon
    - Esim. (Karvinen 2023b)

            base:
              '*':
                - hello

Tämän jälkeen voi ajaa `$ sudo salt '*' state.apply`. Erona aikaisempaan on, että `<oma moduuli>` on jätetty pois. Se on määritetty nyt top.sls -tiedostossa.

### Salt overview ([VMware, Inc. 2023a)](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml)

#### Rules of YAML

YAML on merkintäkieli. Salt tarvitsee python-datarakenteet. YAML renderer suorittaa YAML datarakenteen muuttamisen pythonille sopivaksi.

YAML perussäännöt:

- datarakenne avain-arvo -pareina
- kaksoispiste ja välilyönti erottimena `": "`
- arvo voi esiintyä useamman kerran eri rakenteissa
- merkkikoolla on väliä; a != A
- ei tabeja vaan välilyöntejä
- kommentirivit alkavat risuaidalla `#`

#### YAML simple structure

Kolme peruselementtiä:

- Scalars:
    - avain, jonka arvo on merkkijono, luku tai totuusarvo
- Listat:
    - avain, jonka arvo on lista
    - listan alkion merkkaus: uusi rivi, kaksi välilyöntiä, väliviiva, välilyönti, alkion arvo
- Sanakirjat:
    - kokoelma skalaareita ja listoja

#### Lists and dictionaries - YAML block structures

- YAML on jäsennelty blokkeihin
- sisennys määrittelee asiayhteyden

### Salt states [(VMware, Inc. 2023b)](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules)



## Lähteet:

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit Luettu: 17.11.2023

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file Luettu: 17.11.2023

VMware, Inc. 2023a. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml Luettu: 17.11.2023

VMware, Inc. 2023b. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules Luettu: 17.11.2023
