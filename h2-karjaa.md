# H1 Karjaa ([Karvinen 2023](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa))

## x) Tiivistelmät

### What is the definition of "cattle not pets"? ([Slater 2017](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654))

- Aikaisemmin, jos palvelin kaatui kutsuttiin kaikki kykenevät kannelle auttamaan nostamaan palvelin takaisin pystyyn
  - Analogia lemmikkiin eli serveri on rakas ja tärkeä tai korvaamaton
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

Ylläolevista ohjeista poiketen en asenna VirtualBoxia (on jo asennettu) tai microa (olen tottunut käyttämään nanoa tekstieditorina).

	$ sudo apt install vagrant

Komennon suoritettuani muistin, että paketinhallintaohjelma olisi hyvä päivittää. Ajoin siis komennot:

	$ sudo apt update
ja

  	$ sudo apt upgrade
    
sekä uudestaan
		
	$ sudo apt install vagrant

Lopputulos (kuva 4). Kuvasta näkee, että ensimmäinen ajo ennen päivitystä käytti viimeisimpiä Vagrantin pakettien lähteitä (*already the newest version*).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7c55832d-afae-4757-a23a-7303581311d2)

> Kuva 4. Onnistunut Vagrantin asennus Ubuntulle.

## b) Yksi maankiertäjä. Asenna yksi kone Vagrantilla, ota siihen SSH-yhteys, osoita että netti toimii.

Mukailen Karvisen (2017) [ohjeita](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/).

Aloitetaan asentamalla uusi kone käyttäen Vagrantin init-funktiota (kuva 5).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f811b826-a50f-47ea-b090-b2bf5775e551)

> Kuva 5. Debian Bullseyen asennus vagrantilla.

Seuraavaksi voidaan käynnistää Vagrant ja yrittää ottaa yhteys juuri luotuun koneeseen.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/d4627582-9905-48da-9e6e-75d7a85eec72)

> Kuva 6. Vagrantin käynnistys.

Vagrantin käynnistys näemmä aloitti libvirt:n asennuksen. Antaa mennä vaan!

Olisiko pitänyt määrittää, mitä virtualisoimisalustaa käytetään? Default näyttäisi kuvan 6 mukaan olevan libvirt ja se asennetaan ennemmin kuin etsittäisiin paikallisesti toista virtualisoimisalustaa kuten jo asennettua VirtualBoxia.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/82d57a84-5134-41d3-a9e9-7075ab1cbd1d)

> Kuva 7. `$ vagrant up` jäätyminen ja komennon uudelleen suorittaminen.

`$ vagrant up` suorittaminen pysähtyi kohtaan **Mounting NFS shared folders** useaksi minuutksi. Joten päätin lopettaa suorituksen näppäinyhdistelmällä Ctrl + C. Hellä pysäytys clean upin kanssa myöskin jäätyi, joten Ctrl + C uusiksi ja prosessi tapettiin.

Suoritin `$ vagrant up` -komennon uudestaan oletuksella, että sama virhe toistuu. Yllätyksekseni se onnistuikin, joten yritin muodostaa ssh-yhteyden uuteen koneeseen (`$ vagrant ssh`).

Se toimikin! Kuvasta 7 näkee, että käyttäjä on vaihtunut hostkoneen käyttäjästä ja laitteesta Vagrantilla luotuun koneeseen.

Viimeiseksi testataan, että netti toimii pingaamalla jotain verkkosivua (kuva 8).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/f73e1ea9-9b82-46ee-ac22-5fd87508687f)

> Kuva 8. Onnistunut icmp-paketin lähetys -> netti toimii!

## c) Oma orjansa. Asenna Salt herra ja orja samalle koneelle.



Lähteet:

Karvinen, T. 2017. Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds Luettavissa: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ Luettu: 2.11.2023

Karvinen, T. 2023. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa Luettu: 2.11.2023

Slater, R. 2017. What is the definition of "cattle not pets"?. Luettavissa: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654 Luettu: 2.11.2023
