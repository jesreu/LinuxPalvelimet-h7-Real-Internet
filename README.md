# LinuxPalvelimet-h7-Real-Internet
    Kirjoittanut: Jesse Reunamo
    Kurssi:       Linux-palvelimet
    Linkki:       https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/

## x) Ensimmäiset askeleet palvelimella
 - Aseta palomuuri päälle, ja muista jättää ssh:n kokoinen aukko
            sudo apt-get update
            sudo apt-get install ufw
            sudo ufw allow 22/tcp
 - Lisää joku ei root käyttäjä.
        sudo adduser user
 - Sulje root käyttäjä ja poista root käyttäjän ssh login vaihtoehto.
        sudo usermod --lock root
        sudoedit /etc/ssh/sshd_config
        # ...
        PermitRootLogin no
        # ...
        sudo service ssh restart
 - Päivitä ohjelmisto, jotta järjestelmän haavoittuvaisuudet korjaantuvat.
        sudo apt-get update
        sudo apt-get upgrade
 - Apachen käyttöä varten palomuuri tarvitsee portin auki.
        sudo ufw allow 80/tcp
 - Lisää palvelimelle nimi, jonkin nimipalvelun avulla. Esimerkiksi NameCheap.
## a)
Oman virtuaalipalvelimen vuokraaminen digitalocean palvelusta. 
Tässä raportissa katsotaan digitalocean palvelun kautta palvelimen vuokraaminen, mutta prosessi on samankaltainen myös muilla virtuaalipalvelimen tarjoajilla. Ensimmäisenä sinun tulee luoda käyttäjä haluamallesi palveluntarjoajalle, rekisteröityminen yleensä vaatii luottokortin autentikointi metodina. Mikäli olet opiskelija, voit olla oikeutettu Github Educations pakettiin, jolla saat 200 $ krediittejä digitalocean palveluun käytettäväksi niin sinun ei tarvitse maksaa erikseen palvelun käytöstä.

Nyt, kun sinulla on käyttäjä voit aloittaa palvelimen luomisen painamalla isoa create nappia. Nappia painaessa aukeaa drop-down menu, josta haluat valita droplets vaihtoehdon.

![luontinappi](https://user-images.githubusercontent.com/112503770/217781334-cc176052-c883-4000-a6ec-e59c8729324d.jpg)

Sinun tulee valita palvelimen asetukset. Palvelimen sijainti siis Region kannattaa valita itsesi tai käyttäjiesi läheltä, jotta palvelimen käyttö olisi tehokasta. Valitsen siis itse Frankfurt vaihtoehdon.

![Region](https://user-images.githubusercontent.com/112503770/217782391-44a27e5d-92ee-4588-a657-5a2c0f8cff39.jpg)

Palvelu pyytää valitsemaan datakeskuksen, mutta Frankfurtin tapauksessa on vain yksi valittavissa oleva datakeskus, muuten kannattaa valita itseään lähinnä oleva.

![Datakeskus](https://user-images.githubusercontent.com/112503770/217782809-c89d065a-7336-4594-ad92-3cca53308ac4.jpg)

Levynkuvalla valitset sen käyttöjärjestelmän, joka tulee toimimaan pilvipalvelimellasi. Voit, joko käyttää valmiita, maksullisia tai jopa ladata oman levykuvan. Valitsen Debian käyttöjärjestelmän sillä siitä minulla on eniten kokemusta.

![levynkuva](https://user-images.githubusercontent.com/112503770/217783582-a3710291-6266-47b2-a97d-64983271a165.jpg)

Nyt sinun tulee valita kuinka tehokkaan palvelimen haluat, hyvin todennäköisesti pärjäät halvimalla mahdollisella 1gt ram muistia sisältävällä palvelimella. Palvelu yrittää heti tarjota kallista vaihtoehtoa, mutta kun siirryt CPU-options välilehdellä regular osioon niin halvat vaihtoehdot tulevat esiin. Huomiona vielä, että kaikista halvin 4€/kk vaihtoehto on piilotettu, kun siirryt regular välilehdelle. Valitsen 6€/kk vaihtoehdon, koska siinä on tarvittava 1gt ram muistia.

![Hintavaihtoehdot](https://user-images.githubusercontent.com/112503770/217784745-02ecd22e-cf32-430c-97b0-42617735df35.jpg)

Digitalocean tässä välissä yrittää kaupitella lisää muistia, mutta se on tuskin tarpeellista.

Autentikointi asetuksissa voit, joko valita ssh-avaimen tai salasanan. Mikäli valitset salasanan tulee sen olla vahva sillä palvelimelle tullaan yrittämään murtautumaan katso osio d. Asetin itse salasanan koska se on hieman helpompi.

Ohittamalla Digitaloceanin lisäpalvelujen kaupittelu yritykset, voit siirtyä Finalize osioon, jossa asetat palvelimen nimen. Nimen kannattaa olla neutraali sillä se voi näkyä ulospäin. 

![Finalize](https://user-images.githubusercontent.com/112503770/217787723-035c428b-eee1-41d0-a05e-d954a41aa0bf.jpg)

Nyt kun painat Create Droplet nappulaa käynnistyy palvelin noin minuutin sisällä ja voit aloittaa palvelimen käytön. Voit katsoa palvelimen tarkemmat tiedot 'first-project' välilehdeltä, johon digitalocean luo palvelimesi, mikäli sinulla ei ole muita projekteja.
## b)
Suoritetaan kappalleen x) toimenpiteet palvelimella.

Otetaan ssh yhteys palvelimeen komennolla:
    
    ssh root@(palvelimen ip)

Palvelimen ip osoite löytyy palveluntarjoajan sivuilta. Kun ajat komennon ensimmäistä kertaa varoittaa järjestelmä yhteydestä ja kysyy Yes/no kysymyksen haluatko ottaa yhteyden. Vastatessasi yes, kysyy järjestelmä salasanaa, jonka asetimme a) kohdassa.

 Asennetaan palomuuri komennoilla:
 
        sudo apt-get update
        sudo apt-get install ufw
        sudo ufw allow 22/tcp
        sudo ufw enable
        
Haetaan tiedot uusimmista paketeista update komennolla. Asennetaan palomuurin paketti install komennolla. Jätetään palomuuriin reitti ssh yhteydelle, jotta emme sulje itseämme ulos palvelimelta. Enable komennolla käynnistetään palomuuri.

Lisätään ei root käyttäjä ja poistetaan root käyttäjän ssh kirjautuminen. Muista edelleen asettaa vahva salasana käyttäjälle.

        sudo adduser jesse
        ssh jesse@(palvelimen ip)

Kannattaa kokeilla luotua käyttäjää erillisessä komentoriviikkunnassa ssh komennolla ja sitten poistaa root-käyttäjän kirjautumisen. Kannatta myös tarkistaa, että luotu käyttäjä voi käyttää sudo komentoja. Mikäli ei ole oikeutta sudo komentoihin kannattaa root käyttäjällä käydä editoimassa `/etc/sudoers` tiedostoa. Muokkaamalla osioon User privilege specification: `käyttäjänimi   ALL=(ALL:ALL) ALL`. Tiedostoa voi muokata komennolla `sudo visudo sudoers`.

        sudo usermod --lock root
        sudoedit /etc/ssh/sshd_config
        # ...
        PermitRootLogin no
        # ...
        sudo service ssh restart
        
Päivitetään palvelimen ohjelmisto komennoilla 

        sudo apt-get update
        sudo apt-get upgrade
        
Luodaan reitti apachen käyttöä varten.

        sudo ufw allow 80/tcp
        
## c)
Asennetaan apache ja muutetaan testisivu.

## d)
Nyt kokkiohjelman tyyliin näytän muutaman päivän päällä olleella palvelimella, miten etsiä merkkejä tunkeutumisyrityksistä.

    ssh root@(palvelimen ip)

Yhteyden ottaessa järjestelmä mukavasti kertoo viimeisimmän kirjautumisen, minkä perusteella palvelimelle ei ole päästy sisään, mutta tutkitaan tilannetta vielä tarkemmin.

    cd /var/log/
    cat auth.log
    
Logi on varsin pitkä, vaikka palvelin on ollut muutaman päivän päällä. Eroitellaan vielä hieman tietoa grep komennolla `cat auth.log | grep password`. Joka näyttää onnistuneet/epäonnistuneet salasana yritykset.

kuve

Hakemalla logeista komennolla `cat auth.log | grep Accepted`. Löytyy kaikki onnistuneet yritykset, joilla palvelimelle on päästy sisään. Logit vastaavat omaa käyttöäni, joten kukaan ulkopuolinen ei ole päässyt sisään. Palvelimen kello on hieman eri ajassa kuin paikallinen aika, joten `date` komento on hyvä apu.

Tutkitaan vielä apachen lokeja mahdollisista yrityksistä.

    cd apache2/
    cat access.log
    
Apachen lokeissa näkyy todella paljon get pyyntöjä itse osoitteeseen, mutta muutamana poimintana get pyynnöt osoitteisiin alampana. Voidaan vielä rajata lokia `cat access.log | grep 404` komenolla, jotta nähdään vain ne pyynnöt joissa ei ole oikea osoite. 

    "/admin/config.php"
    "/aaa9"
    "/aab8"
    "/wp-includes/css/buttons.css"
    "/boaform/admin/login.asp" 
    "/boaform/admin/formLogin?username=admin&psd=admin"
    "/.env"

Ainakin edellä olevista pyynnöistä päätellen palvelimen tietoturvallisuus on kokoajan koetuksella. 
## Lähteet

    https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/
    https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/
    
