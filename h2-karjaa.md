# H1 Karjaa ([Karvinen 2023](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa))

## x) Tiivistelmät

### What is the definition of "cattle not pets"? ([Slater 2017](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654))

- Aikaisemmin, jos palvelin kaatui kutsuttiin kaikki kykenevät kannelle auttamaan nostamaan palvelin takaisin pystyyn
  - Analogia lemmikkiin eli kaatunut serveri on rakas ja tärkeä tai korvaamaton
- Nykyään (parempi toimintamalli) on ajatella palvelimia karjana
  - Vialliset/kaatuneet palvelimet viedään ladon taakse lahdattavaksi -> toiminta jatkuu
- Palvelinten käsittely karjana edesauttaa varautumaan ongelmatilanteisiin ja monet ongelmatilanteet ovat automatisoinnin avulla korjattavissa ilman ihmistä

### Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds ([Karvinen 2017](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/))

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/de54506d-6d02-4704-a243-23ba83df8f10)

> [Kuva 1](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/). Ohjeet Vagrantin ja VirtualBoxin asentamiseen ja uuden virtuaalikoneen luominen.

### Salt Vagrant - automatically provision one master and two slaves ([Karvinen 2023](https://terokarvinen.com/2023/salt-vagrant/))

- Asenna virtualisointiympäristö (kuva 2)

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/475d2861-d325-47f8-b1a4-c9f76732e409)
> [Kuva 2](https://terokarvinen.com/2023/salt-vagrant/). Virtualisointiympäristön asentaminen.

- Kopioi Karvisen sivulta valmis kolmen koneen [konfiguroimistiedosto](https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers) Vagranfile -nimiseen tiedostoon
- Käynnistä koneet

		$ vagrant up

- Muodosta yhteys masteriin

		$ vagrant ssh tmaster

- Hyväksy orjien kutsut (salausavaimet)

  		$ sudo salt-key -A

- Testaa, että master pystyy komentamaan orjia

		sudo salt '*' test.ping

- Hallitset nyt orjakoneita ja pysty saltin avulla suorittamaan idempotentteja funktioita

## a) Asenna Vagrant.

Testausympäristö:

- Host OS: Ubuntu 22.04
- Prosessori arkkitehtuuri: AMD64
- Prosessorimalli: 11th Gen Intel(R) Core(TM) i3-1115G4 @ 3.00GHz

Testataan ensin löytyykö koneelta Vagrant.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/9b9df10f-1c29-48c9-9d17-dcadfbf807ce)

> Kuva 3. Vagrant-komentoa ei löydy -> Vagrantia ei ole asennettu.

Lähteet:

Karvinen, T. 2017. Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds Luettavissa: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ Luettu: 2.11.2023

Karvinen, T. 2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa Luettu: 2.11.2023

Slater, R. 2017. What is the definition of "cattle not pets"?. Luettavissa: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654 Luettu: 2.11.2023
