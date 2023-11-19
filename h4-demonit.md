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

- Datarakenne avain-arvo -pareina
- Kaksoispiste ja välilyönti erottimena `: `
- Arvo voi esiintyä useamman kerran eri rakenteissa
- Merkkikoolla on väliä `a != A`
- Ei tabeja vaan välilyöntejä
- Kommentirivit alkavat risuaidalla `#`

#### YAML simple structure

Kolme peruselementtiä:

- Scalars:
    - Avain, jonka arvo on merkkijono, luku tai totuusarvo
- Listat:
    - Avain, jonka arvo on lista
    - Listan alkion merkkaus: uusi rivi, kaksi välilyöntiä, väliviiva, välilyönti, alkion arvo
- Sanakirjat:
    - Kokoelma skalaareita ja listoja

#### Lists and dictionaries - YAML block structures

- YAML on jäsennelty blokkeihin
- Sisennys määrittelee asiayhteyden; samaan lohkoon liittyvät argumentit

### Salt states [(VMware, Inc. 2023b)](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules)

#### State modules

- Yksittäisistä tiloista määritetään `module.function` (tilamoduuleista)
- Tilamoduulit kutsuvat toteutusmoduulivastineitaan (`exectuion module`)
- Paketin asennuserot; terminaali vs salt:
  - Terminaalikomentoja käytettäessä pitää ensin tarkistaa onko paketti jo asennettu ja sitten syöttää komento, jolla se asennetaan
  - Saltilla ei tarvitse erikseen tarkistaa, onko paketti asennettu

#### The state SLS data structure

- Identifier, tunniste tilablokille
- State moduuli, jossa on funktio esim. `pkg`
- Function, esim. `installed`
- Name, tilakutsun nimi, yleensä hallinnoitava tiedosto tai asennettava paketti
- Arguments, funktion hyväksymät argumentit

#### Organizing states

- State-moduuleiden järjestäminen tulee tehdä niin, että muutkin ymmärtävät jo nimestä niiden tarkoituksen
- Rajoita tilamoduuleiden alihakemistojen määrää

#### The top.sls file

- Kun orjakoneita on paljon, ei ole järkevää suorittaa tilamoduuleita yksitellen.
- Top.sls -tiedostoon voidaan kartoittaa orjakoneet, minkä lisäksi voidaan määritellä orjien toimintaympäristöt.

#### Create the SSH state

Alla olevat esimerkkimääritykset on kopioitu [täältä](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#create-the-ssh-state).

**SSH:**
    
    install_openssh:                        # identifier; asenna openssh
      pkg.installed:                        # statemoduuli ja funktio
        - name: openssh                     # tilakutsun nimi
    
    push_ssh_conf:
      file.managed:
        - name: /etc/ssh/ssh_config         # managed-funktion tilakutsun nimi
        - source: salt://ssh/ssh_config     # argumentti managed-funktiolle; orja lataa tämän tiedoston itselleen
    
    push_sshd_conf:
      file.managed:
        - name: /etc/ssh/sshd_config
        - source: salt://ssh/sshd_config
    
    start_sshd:
      service.running:
        - name: sshd
        - enable: True                      # argumentti running-funktiolle

**Apache:**

    implement_httpd:
      pkg.installed:
        - name: httpd
    
    http_conf:
      file.managed:
        - name: /etc/httpd/conf/httpd.conf
        - source: salt://apache/httpd.conf
    
    start_httpd:
      service.running:
        - name: httpd
        - enable: True

Teron vinkeistä päätellen yllä olevat esimerkkimääritykset ovat puutteeliset. Tilan ID:ksi (identifier) olisi hyvä asettaa itse tilafunktio. Jos masterilla muutetaan määrityksiä `httpd.conf`-tiedostossa, ne ei vät päivity orjille automaattisesti. Määrityksiin olisi hyvä lisätä service.watch -funktio. Perustan muutokseni `watch`-kohdan osalta Saltin dokumentaatioon (VMware, Inc. 2023c).

**Apache muutettuna:**

    httpd:                                    # id
      pkg.installed                           # funktion kutsu saa nimekseen id:n

    /etc/httpd/conf/httpd.conf:
      file.managed:
        - source: salt://apache/httpd.conf
    
    httpd:
      service.running:
        - enable: True
        - watch: /etc/httpd/conf/httpd.conf   # watchin arvoksi tulee konffaustiedoston funktion id

## Lähteet:

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit Luettu: 17.11.2023

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file Luettu: 17.11.2023

VMware, Inc. 2023a. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml Luettu: 17.11.2023

VMware, Inc. 2023b. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules Luettu: 17.11.2023

VMware, Inc. 2023c. Requisites and other global state arguments. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/requisites.html#watch Luettu: 19.11.2023
