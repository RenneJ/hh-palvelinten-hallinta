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

Teron vinkeistä päätellen yllä olevat esimerkkimääritykset ovat puutteeliset. Tilan ID:ksi (identifier) olisi hyvä asettaa itse tilafunktio. Jos masterilla muutetaan määrityksiä `httpd.conf`-tiedostossa, ne eivät päivity orjille automaattisesti. Määrityksiin olisi hyvä lisätä service.watch -funktio. Perustan muutokseni `watch`-kohdan osalta Saltin dokumentaatioon (VMware, Inc. 2023c).

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

### Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port [(Karvinen 2018)](https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh)

Package-file-service on yleinen mallinnus useiden daemoneiden hallintaan.

- package viittaa asennettavaan daemoniin
- file muutettaavaan konffaustiedostoon
- service uudelleenkäynnistykseen uusien määritysten käyttöönottamiseksi

## a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/d1ccfd7a-37be-4796-9583-d8f16680b295)

> Kuva 1. Alkuasetelma tehtävään ryhdyttäessä.

Kuvasta 1 voi nähdä, että masterkoneellani on jo tiedosto `init.sls` tehtävänannon mukaisessa polussa `/srv/salt/hello`. Muokataan ko. tiedostoa tätä tehtävän vaihetta varten niin, että tilafunktio tarkistaa `helloworld.txt` -tiedoston olemassaolon ja tarvittaessa lataa sen masterilta.

Master-koneella navigoidaan `/srv/salt/hello` -hakemistoon ja muokataan `init.sls` -tiedostoa (kuva 2). Käytetään aiemmasta tehtävästä analysoitua argumenttia tässä tehtävssä `- source: salt://<hakemisto/tiedosto.pääte>`. En ole varma minne `helloworld.txt` pitäisi sijoittaa masterilla, että orjat sen lataisivat. Tehdään tiedosto nykyiseen hakemistoon, jossa `init.sls` on.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a37ba736-ba62-4a67-b7c6-368e38e65ce6)

> Kuva 2. Kuvakaappaus init.sls -tiedoston muokkauksesta.

Seuraavaksi suoritetaan tilafunktio kaikilla orjilla.

    $ sudo salt '*' state.apply hello

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/bf9140f7-9bc9-4043-b20c-49b092ee8e94)

> Kuva 3. Tilan suoritus toimii!

Tekstitiedosto oli oikeassa paikassa suoritettavan sls-tiedoston kanssa samassa hakemistossa.

## b) Top. Tee top.sls niin, että tilat ajetaan automaattisesti, esim komennolla "sudo salt '*' state.apply".

Tähän osavaiheeseen riittänee kopioida suoraan tämän tehtävän ensimmäisen osan kohtaa **top.sls - What Slave Runs What States**.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/eb14ede4-4da0-4678-a10a-31b8235025e4)

> Kuva 4. Tilan suoritus onnistuu top.sls -tiedoston määritysten mukaisesti jokaiselle orjalle!

Kuvasta 4 voi nähdä top.sls tiedoston luomisen ja tilan onnistuneen suorituksen. Muutoksia ei tehty, koska `helloworld.txt` on jo määritellyssä polussa.

## c) Apache. Asenna Apache, korvaa sen testisivu ja varmista, että demoni käynnistyy.

Tätä osiota varten loin uuden testiympäristön [Teron sivuilta](https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers) tutulla Vagrantfile-pohjalla. Muutin ainoastaan virtuaalikoneiden nimet.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7116e364-9545-42f9-9f70-2ed412eee265)

> Kuva 5. Apachea ei löydy juuri luodulta masterilta.

Apache2:n asennus:

    $ sudo apt-get update
    $ sudo apt-get install apache2
    $ apache2 -v

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/aabc2d03-275d-4502-8e96-9c79eec4b6d1)

> Kuva 6. Apache2:n asennuksen onnistumisen varmistuminen.

Alkuasetelmaa havainnoidessani käytin erheellisesti komentoa `apache2 -v`. Tällainen tarkistus toiminee vain sovelluksille, jotka eivät ole daemoneita. Kuvassa 6 näkyy myös yksi toimiva tapa tarkistaa apache2:n asennus.

Seuraavaksi on tarkoitus muuttaa apachen oletusaloitussivu. Aikaisemmalta oppitunnilta muistan, että oletussivu löytyy `/var/www` loput polusta täyttyy tabulaattoria reippaasti näpyttelemällä:

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/28ba122d-488d-427e-a87d-83ca60480c21)

> Kuva 6. Apache2:n index.html ja sen sijainti.

Muokataan sitä!

    $ nano index.html

Testataan sitä selaimella. Ensiksi pitää tosin asentaa linuxin terminaalissa toimiva selain. Muistan Teron maininneen Lynxin, joten asennetaan se.

    $ sudo apt-get install lynx
    $ lynx localhost

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/a2ee3025-8f87-4c66-8a1c-2e62e9cfe05a)

> Kuva 7. Näkymä lynxillä. Apache2 toimii!

Seuraavaksi kirjoitetaan määritykset apache2:n asentamiseen ja muokkaamani index.html:n muuttamiseen samaksi. Eikä unohdeta lynxin asentamistakaan!

Aloitan aikaisemmasta olettamuksestani, että jaettava tiedosto (muokattu index.html) täytyy olla salt statemoduulin juuressa. Ensimmäinen ajatukseni on luoda symbolinen linkki masterilla `/srv/salt/helloapache/index.html` -> `/var/www/html/index.html`. Samalla luodaan oikea hakemistorakenne.

Masterilla:

    $ mkdir /srv/salt/
    $ mkdir /srv/salt/helloapache
    $ cd /srv/salt/helloapache
    $ sudo ln -s /var/www/html/index.html
    $ sudo nano apacheinit.sls

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/642f22f6-fafa-4d48-92b2-645fed7839ce)

> Kuva 8. Apacheinit.sls sisältö.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/4b48e91e-0978-46ee-b5d6-8cc4e2661da7)

> Kuva 9. Tilan suorittaminen epäonnistuu.

Googlasin virheilmoituksen ja tulin siihen tulokseen, että 'base' on konfiguroitu väärin ([StackOverflow](https://stackoverflow.com/questions/60873775/no-matching-sls-found-for-httpd-in-env-base)).

Kurkataan mitä masterin `/etc/salt/master` konfiguroinnissa mättää. `$ less /etc/salt/master` komennon avulla huomaan, että konfigurointi on kommenttirivejä täynnä. Lisätään sinne yllä olevan linkin mukaan oikea 'base' määritys.

    $ sudo nano /etc/salt/master
    
    file_roots:
      base:
        - /srv/salt

Sama error tulee, kun yrittää uudelleen `sudo salt '*' state.apply helloapache`.

Seuraavaksi kokeilin muuttaa apacheinit.sls init.sls nimiseksi ja ajo käynnistyi. Taas opin uutta; sls-tiedostoja ei parane nimetä vapaasti.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f57c2203-68ac-4c3f-a50e-95ff55361626)

> Kuva 10. Ajo käynnistyy.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/66bb7702-555b-4361-ab34-71512014e620)

> Kuva 11. Jotain mättää määrityksissäni...

Kuvassa 11 on onnistunut käynnistys, mutta jotain vikaa määrityksissäni on. Ensinnäkin unohdin laittaa lynxin asennettavaksi. Selasin Saltin dokumentaatiota (VMware, Inc 2023d) daemonien käynnistyksen ja uudelleenkäynnistyksen osalta. Tämän pohjalta tein muutoksia init.sls -tiedostoon.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f386a527-9e91-4799-a391-4f203034b6a4)

> Kuva 12. Muutokset sls-tiedostoon.

Muutosten jälkeen `sudo salt '*' state.apply helloapache` ajaminen onnistui virheittä.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/dc44ce95-e7ee-4095-91a0-212f1aca4043)

> Kuva 13. Virheetön ajo.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0306b44b-a303-40c0-b000-523583a43be9)

> Kuva 14. Näkymä `lynx localhost` ajosta orjalla.

## d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee.

Tämä osa suositellaan tehtävän tavallisella virtuaalikoneella, jota Vagrant ei ohjaa. Alustin uuden Debian 12 koneen libvirtin virt-managerilla.

Aloitetaan siitä, että asennetaan koneelle salt-master ja -minion sekä openssh.

**[HUOM! Olen tässä vaiheessa jo myöhässä palautuksesta. En myöskään ymmärrä tämän osan tehtävänantoa, joten tämä tehtävä on palautettu puutteellisena. 20.11.2023 klo 15:15]**

Kokeillaan jatkaa nyt, kun pääsin arvioimaan muutaman kurssikaverin erinomaiset vastaukset. Heidän vastausten pohjalta jatkan tämän osion vastausta ([hautadata](https://github.com/hautadata/palvelintenhallinta-jh/blob/main/h4-demonit.md) ja [J-Huttununen](https://github.com/J-Huttunen/palvelinten-hallinta/blob/main/h4.md)).

Käytin salt-masterin ja -minionin asentamiseen Debian 12:lle Teron ohjeita (Karvinen 2023a). Ssh-palvelimen asennus: `sudo apt-get install openssh-server`.



## Lähteet:

Karvinen, T. 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh Luettu: 19.11.2023

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h4-demonit Luettu: 17.11.2023

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file Luettu: 17.11.2023

VMware, Inc. 2023a. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml Luettu: 17.11.2023

VMware, Inc. 2023b. Salt states. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html#state-modules Luettu: 17.11.2023

VMware, Inc. 2023c. Requisites and other global state arguments. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/requisites.html#watch Luettu: 19.11.2023

VMware, Inc. 2023d. Starting or restarting of services and daemons. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/all/salt.states.service.html#starting-or-restarting-of-services-and-daemons Luettu: 20.11.2023
