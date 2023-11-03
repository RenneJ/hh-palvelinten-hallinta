# H1 Karjaa ([Karvinen 2023a](https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa))

## x) Tiivistelmät

### What is the definition of "cattle not pets"? ([Slater 2017](https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654))

- Aikaisemmin, jos palvelin kaatui kutsuttiin kaikki kykenevät kannelle auttamaan nostamaan palvelin takaisin pystyyn
  - Analogia lemmikkiin eli serveri on rakas ja tärkeä tai korvaamaton
- Nykyään (parempi toimintamalli) on ajatella palvelimia karjana
  - Vialliset/kaatuneet palvelimet viedään ladon taakse lahdattavaksi -> toiminta jatkuu
- Palvelinten käsittely karjana edesauttaa varautumaan ongelmatilanteisiin ja monet ongelmatilanteet ovat automatisoinnin avulla korjattavissa ilman ihmistä

### Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds ([Karvinen 2017](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/))

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/de54506d-6d02-4704-a243-23ba83df8f10)

> Kuva 1. Ohjeet Vagrantin ja VirtualBoxin asentamiseen ja uuden virtuaalikoneen luominen. [Kuvan lähde](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/)

### Salt Vagrant - automatically provision one master and two slaves ([Karvinen 2023b](https://terokarvinen.com/2023/salt-vagrant/))

- Asenna virtualisointiympäristö (kuva 2)

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/475d2861-d325-47f8-b1a4-c9f76732e409)
> Kuva 2. Virtualisointiympäristön asentaminen. [Kuvan lähde](https://terokarvinen.com/2023/salt-vagrant/)

- Kopioi Karvisen sivulta valmis kolmen koneen [konfiguroimistiedosto](https://terokarvinen.com/2023/salt-vagrant/#ready-made-vagrantfile-for-three-computers) Vagranfile -nimiseen tiedostoon
- Käynnistä koneet

		$ vagrant up

- Muodosta yhteys masteriin

		$ vagrant ssh tmaster

- Hyväksy orjien kutsut (salausavaimet)

  		$ sudo salt-key -A

- Testaa, että master pystyy komentamaan orjia

		sudo salt '*' test.ping

- Hallitset nyt orjakoneita ja pystyt saltin avulla suorittamaan idempotentteja funktioita

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

Pyrin asentamaan salt-masterin ja salt-minionin edellisessä osatehtävässä Vagrantilla alustamaani virtuaalikoneeseen.

Jälleen kerran Vagrantin käynnistys jäätyy samaan kohtaan kuin aikaisemmin (kuva 9). Googlasin hakusanoilla "vagrant up stops at mounting nfs", mikä johti hashicorpin GitHub-sivuille, jossa käyttäjä lordgordon oli kohdannut saman ongelman. [Keskustelusta](https://github.com/hashicorp/vagrant/issues/5802) päätellen ongelma johtuu palomuurista.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/591a6a8a-2056-43af-ba4b-f9a48d05ace4)

> Kuva 9. `$ vagrant up` jäätyy taas. Kuvassa koko stack trace.

Kuvan 9 rivillä `default: SSH address: 192.168.121.241:22` viimeinen numero kuvastaa tcp/ip tietoliikenne porttia. Sallitaan palomuurin hyväksyvän kaikki liikenne ko. portista. Ufw:llä portin avaus ja palomuurin status (kuva 10).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/7747f9a5-12c6-4e92-8431-eafc3d8551d5)

> Kuva 10. Palomuuriin uusi sääntö ja tilannekatsaus.

Palomuuriin reiän porattuani voidaan kokeilla uudestaan. Aikaisemman "virheellisen" onnistumisen (kuva 7) välttämiseksi sammutetaan vagrant-kone (kuva 11).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/67a49fcb-e6c3-4047-bf43-bf0392dcf5c0)

> Kuva 11. Koneen sammutus Vagrantilla.

Taas kun kokeilin Vagrantin käynnistystä sain saman virheen kuin aikaisemmin (kuva 9). Luulen, että muitakin portteja pitäisi avata.

Seuraavaksi kokeillaan palomuurin kytkemistä pois päältä ja toistetaan uudestaan vagrantin käynnistys.

	$ sudo ufw disable
 	$ vagrant halt
  	$ vagrant up

Vihdoin sain vagrantkoneen käynnistettyä ongelmitta (kuva 12).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/df8c4288-9d5c-4dec-9c7f-8008b2680e6a)

> Kuva 12. Onnistunut vagrantkoneen käynnistys!
 
En äkkiseltään löytänyt portteja, jotka pitäisi avata. Se ei myöskään ole tätä tehtävää varten oleellista, joten jatkan palomuuri kytkettynä pois päältä.

Seuraavaksi on tarkoitus muodostaa ssh-yhteys ja asentaa kohdekoneeseen master ja minion.

	$ vagrant ss
 	$ sudo apt update
  	$ sudo apt install salt-minion
   	$ sudo apt install salt-master

Ylläolevien komentojen suoritus onnistui ongelmitta. Tarkistetaan sovellusten versiot (kuva 13). Saltin dokumentaation mukaan on suositeltavaa, että master ja minion käyttävät samaa versiota ([VMware, Inc. 2023a](https://docs.saltproject.io/en/latest/faq.html#can-i-run-different-versions-of-salt-on-my-master-and-minion)).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/2d2648b2-014f-4fc8-9945-fac9830c8b97)

> Kuva 13. Herran ja orjan versiot.

Versiot tarkastettuani muistin, että viime viikon tehtävissä asennettiin uusin versio, jonka asentamiseen oli hieman monimutkaisemmat [ohjeet](https://github.com/RenneJ/hh-palvelinten-hallinta/blob/main/h1-viisikko.md#a-saltin-asennus-debian-12lle). Jatkan tämän viikon tehtävien tekemistä toistaiseksi ohjelmistoversiolla, jonka asensin vagrantkoneelle.

## d) Asenna Saltin herra-orja arkkitehtuuri toimimaan verkon yli.

Asetelma johon pyrin on, että herra ja orja ovat eri koneilla; niillä on oltava eri ip-osoitteet. Herran on kyettävä ajamaan komentoja verkon yli ja orjan on vastattava. Tässä vaiheessa tehtäviä on herra ja orja asennettu samalle laitteelle. Rakennetaan testausympäristö Karvisen (2023b) [ohjeiden](https://terokarvinen.com/2023/salt-vagrant/) mukaan.

Loin ensimmäisen vagrantfilen kotihakemistooni, joten kasataan uusi testausympäristö omaan hakemistoon:

	$ mkdir lab01; cd lab01

Ohjeista poiketen käytän tekstieditorina nanoa. Kopioin ohjeista konfiguroimistiedoston Vagrantfile-nimiseen tiedostoon. En ole varma mitä tapahtuu, kun vagrant käynnistetään. Pohdin onko ip-määrityksissä jotain, mitä minun pitäisi muuttaa. Mutta testausympäristö on testausta varten joten:

	$ vagrant up

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0e92f656-a4e0-42e3-b37e-1ccef4709aa7)

> Kuva 14. Testausympäristön käynnistys. Stack tracen viimeiset rivit.

Käynnistys sujui ongelmitta (kuva 14). Kokeillaan ensin manuaalisesti kerätä tietoja luoduista koneista. Selvitetään jokaisen koneen Salt-versio (minion vai master) sekä ip-osoitetiedot.

Yhteyden muodostus:

	$ vagrant ssh <hostname>

Jos <hostname> jätetään tyhjäksi, vagrant muodostaa yhteyden masteriin.

Komennot joilla tarkistin alla olevaan taulukkoon kirjatut tiedot:

	$ ip address 			# kirjasin sen ip:n mikä on kofigurointitiedoissa määritetty
 	$ ls /usr/bin/ | grep salt 	# binääreiden hakemistosta listattu tulokset, jotka sisältävät merkkijonon "salt"
  	$ systemctl status salt-<rooli>	# tarkistetaan onko daemon päällä
   	$ salt -V			# saltin versio

|   |tmaster|t001|t002|
|---|-------|----|----|
|ip|192.168.12.3/24|192.168.12.100/24|192.168.12.102/24|
|rooli|master|minion|minion|
|versio|3002.6|3002.6|3002.6|

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/897368c9-a927-4f02-861f-217c3a60fc9f)

> Kuva 15. Esimerkki roolin tarkistuksesta ja daemonin ajosta.

Tässä vaiheessa ollaan tilanteessa, jossa on mahdollista verkon välityksellä ohjata orjakoneita masterilla. MEn aivan tarkalleen muista, mitä Tero edellisellä oppitunnilla demonstroi tähän vaiheeseen päästyään. Se liitty avaimiin. Kuvasta 15 näkee, että binääreihin tmasterille on asennettu `salt-key`, joten kokeillaan ajaa se.

	$ sudo salt-key

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/b27eef9d-039f-41bf-90e1-f5e11cb347e2)

> Kuva 16. Orjat ovat yrittäneet ottaa yhteyttä.

Tarkistan ohjelman dokumentaatiosta, `man salt-key` (Hatch et al 2021), kuinka hyväksytään vastauspyynnöt.

	$ sudo salt-key -A			# -A = accept all

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/78d3ae94-d2b7-4020-8be6-92f6dc4d66a2)

> Kuva 17. Avainten hyväksyminen masterilla.

Nyt pitäisi herra-orja -arkkitehtuurin toimia verkon yli, ainakin pingaaminen onnistuu (kuva 17). Seuraavissa osissa testataan tarkemmin.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/2c18ceed-b87b-4518-8ee3-d78037b2ac1c)

> Kuva 18. Pingaaminen herralta orjalle ja orjalta herralle onnistuu.

## e) Aja useita idempotentteja (state.single) komentoja verkon yli.

Testataan idempotenssia pakettien asentamisella ja poistamisella; kokeillaan tapahtuuko yhtedenotoissa muutoksia, jos orjiin asennetaan ja käynnistetään palomuuri (ufw). Tehdään myös molemmille kohdekoneille käyttäjät kotihakemistoineen. Komennot mukailtu Karvisen (2023b) esimerkeistä.

Masterilla:

	$ sudo salt '*' state.single pkg.installed ufw
 	$ sudo salt '*' state.single service.running ufw

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/913945e2-c92c-41c1-89d2-037793d81b04)

> Kuva 19. Orjakoneiden idempotenssia muutettu. Ufw asennettu.

Alempi komento onnistuneesti käynnistää molemmissa orjissa daemonin. Tulostus on samankaltainen kuin yllä kuvassa 19.

Kokeillaan paketin asentamista nyt kun palomuurit ovat käynnissä kohdekoneissa.

	$ sudo salt '*' state.single pkg.installed tree
 	$ sudo salt '*' state.single user.present user01

Yllä olevat komennot suorituivat virheittä. Tämän testin perusteella palomuuri ei häiritse salt-masterin kykyä käskyttää salt-minioneita.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/0aaaad1c-a52f-4533-b6a2-d0f6b094d7cf)

> Kuva 20. Käyttäjän luominen onnistui.

## f) Kerää teknistä tietoa orjista verkon yli (grains.item)

Katsotaan orjien `cwd` current working directory, `os` operating system.

	$ sudo salt '*' grains.item cwd os

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/5295dd37-2dee-469f-8baa-c7cb002b3bf8)

> Kuva 21. Tietoja orjista.

Kaikki tiedot saadaan komennolla:

	$ sudo salt '*' grains.items

## g) Aja shell-komento orjalla verkon yli.

Luodaan shell-komennolla tiedosto käyttäjän kotihakemistoon:

	$ sudo salt '*' state.single cmd.run 'touch /home/user01/testfile'

Testataan onnistuiko saltin file.managed -funktion ajaminen. Oletetaan, että tiedosto löytyy, seuraavan komennon ajo onnistuu ja tilaa ei muuteta.

	$ sudo salt '*' state.single file.managed /home/user01/testfile

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/133aecdb-ccbe-4fd9-8f12-fd84881b4985)

> Kuva 22. Testfile oli luotu `touch` komennolla, joten file-funktio ei tehnyt muutoksia koneeseen.

## h) Hello, IaC. Tee infraa koodina kirjoittamalla /srv/salt/hello/init.sls. Aja tila jollekin orjalle.

Seurasin Karvisen (2023b) [ohjeita](ttps://terokarvinen.com/2023/salt-vagrant/) tavoitteenani luoda oma moduuli, jolla luodaan käyttäjän kotihakemistoon tiedosto nimeltä `fancy-example`.

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/addeb2bc-d70a-428d-b3c2-5e4830912635)

> Kuva 23. Oman moduulin ajo suoritui ja näyttäisi luoneen uuden tiedoston.

Tarkistetaan se paikallisesti ottamalla ssh-yhteys t001-koneeseen (kuva 24).

### ![image](https://github.com/RenneJ/hh-palvelinten-hallinta/assets/97522117/5ba84f2c-0e72-48dd-ba8a-bab2e4f12123)

> Kuva 24. Tiedosto löytyy paikallisesti tarkistettuna.

## Lähteet:

Hatch, T.S., et al. 2021. Salt-key manual pages. `man salt-key`.

Karvinen, T. 2017. Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds Luettavissa: https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ Luettu: 2.11.2023

Karvinen, T. 2021. Run Salt Commands Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/ Luettu. 3.11.2023

Karvinen, T. 2023a. Infra as Code 2023. Luettavissa: https://terokarvinen.com/2023/configuration-management-2023-autumn/#h2-karjaa Luettu: 3.11.2023

Karvinen, T. 2023b. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/ Luettu: 3.11.2023

Slater, R. 2017. What is the definition of "cattle not pets"?. Luettavissa: https://devops.stackexchange.com/questions/653/what-is-the-definition-of-cattle-not-pets#654 Luettu: 2.11.2023

VMware, Inc. 2023a. Frequently Asked Questions. Can I run different versions of Salt on my master and minion?. Luettavissa: https://docs.saltproject.io/en/latest/faq.html#can-i-run-different-versions-of-salt-on-my-master-and-minion Luettu: 2.11.2023

VMware, Inc. 2023b. Configuring the Salt Minion. Minion Primary Configuration. Luettavissa: https://docs.saltproject.io/en/latest/ref/configuration/minion.html Luettu: 3.11.2023
