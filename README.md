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
    $ sudoedit /etc/ssh/sshd_config
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
Oman virtuaalipalvelimen vuokraaminen digitalocean palvelusta. Tässä raportissa katsotaan digitalocean palvelun kautta palvelimen vuokraaminen, mutta prosessi on samankaltainen myös muilla virtuaalipalvelimen tarjoajilla. Ensimmäisenä sinun tulee luoda käyttäjä haluamallesi palveluntarjoajalle, rekisteröityminen yleensä vaatii luottokortin autentikointi metodina. Mikäli olet opiskelija, voit olla oikeutettu Github Educations pakettiin, jolla saat 200 $ krediittejä digitalocean palveluun käytettäväksi niin sinun ei tarvitse maksaa erikseen palvelun käytöstä.

Nyt, kun sinulla on käyttäjä voit 


## b)
Suoritetaan kappalleen x) toimenpiteet palvelimella.

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
    
Apachen lokeissa näkyy todella paljon get pyyntöjä itse osoitteeseen, mutta muutamana todennäköisenä murtautumis yrityksen poimintana get pyynnöt osoitteisiin alampana. Voidaan vielä rajata lokia `cat access.log | grep 404` komenolla jotta nähdään vain ne pyynnöt joissa ei ole oikea osoite. 

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
    
