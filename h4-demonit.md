# H4 Demonit [(Karvinen 2023a)](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit)

## x) Lue ja tiivistä

### Salt Vagrant - automatically provision one master and two slaves [(Karvinen 2023b)](https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file)

**Infra as Code - Your wishes as a text file**

Oman salt-moduulin luomisvaiheet:

- Hakemiston luonti polkuun `/srv/salt/<oma moduuli>`
- Oman määritystiedoston luonti juuri luotuun hakemistoon `<tiedostonimi>.sls`
- Määritykset YAMLilla; sisennys kaksi välilyöntiä
- Oman moduulin ajo: `$ sudo salt '*' state.apply <oma moduuli>`

**top.sls - What Slave Runs What States**
    
Top-tiedosto määrittelee mikä orja suorittaa minkäkin määrityksen. Ts. koodia, jossa määritellään orja/orjat sekä moduulit, jotka niiden on suoritettava.

- Uusi tiedosto polkuun `/srv/salt/top.sls`
- Määritykset top.sls -tiedostoon
    - Esim. (Karvinen 2023b)

            base:
              '*':
                - hello



## Lähteet:

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit Luettu: 17.11.2023

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file Luettu: 17.11.2023
