# Palvelimien hallinta harjoitus 3 GitHub ja git
Tehtävä piti tehdä MarkDownina GitHubissa. Aluksi pitää luoda GitHubiin varasto, johon tehtävä tehdään. Ohjeet alkutoimenpiteille löytyy Tero Karvisen sivuilta:
http://terokarvinen.com/2016/publish-your-project-with-github
Koneena minulla oli Fujitsu Lifebook  AH531, i3-2328M, Windows 7. Käyttämäni livetikun olin päivittänyt ensin uudempaan versioon Xubuntu 18.04.1 LST, koska olin huomannut jotain ongelmia työskennellessäni vanhalla tikulla, jossa oli Xubuntu 16.4.

# Varaston kopioiminen koneelle
Kopioin omalta tililtäni luomani varaston osoitteen ja kopioin sen koneelle

    git clone https://github.com/SeppanenP/spegit.git

Koneelle tuli spegit-kansio polkuun /home/xubuntu/spegit. Kansiosta löytyi README.md tiedosto.

Seuraavaksi aloin muokkaamaan tiedostoa komentoriviltä. Kun avasin sen siitä puuttui viimeisimmät GitHubissa tekemäni päivitykset. En ollut päivittänyt koneella olevaa versiota, joten palasin ilman muutoksia ja annoin komennon:

    git pull

Jolloin muutokset olivat päivittyneet tiedostoon. Kun kirjoitin muutokset komentoriviltä ja tallensin ne annoin seuraavaksi komennot:

    git add . (valmistelee tiedoston siirtoon)
 
    git commit (voit kommentoida muutosta)
 
    git pull (haetaan mahdolliset muutokset GitHubista ensin)
 
    git push (julistetaan muutokset GitHubiin)

Kaikki tekemäni muutokset näkyivät hienosti GitHubin puolella. Seuraavaksi testataan lisää julkaisemista.

<<<<<<< HEAD
Tämä teksti on kirjoitettu sen jälkeen komentorivillä, kun olin ensin kirjoittanut ensin otsikon aiheesta samanaikainen muokkaus.

=======
# Mitä tapahtuu jos samaa tiedostoa muokataan samanaikaisesti?

Seuraavaksi testaan ihan mielenkiinnosta, mitä tapahtuu jos muokkaan samaa tiedostoa samaan aikaan GitHubissa. Tämä teksti tallennetaan ensin.
>>>>>>> 4fdef09e2c604ca8b794aec911a442612ea357f3

Tuloksena oli ristiriita:

    Auto-merging README.md
    CONFLICT (content): Merge conflict in README.md
    Automatic merge failed; fix conflicts and then commit the result.

eli en saanut julkaistua tekstiä komentoriviltä ja dokumenttiin tuli ohjelmallisesti ylläolevat ilmoitukset ennen otsikkoa (<<<<<HEAD) ja rivi otsikon kappaleen alle, jossa SHA-1 luku. Kun annoin uudelleen komennot add., git commit, git pull, git push muutokset onnituivat. Jätin tekstiin tulleet ilmoitukset näkyviin dokumenttiin.

# Git log

Testataan komentoa

    git log

Tuloksena useampi rivi seuraavanlaisia ilmoituksia:

    commit a36a79e5641c43589e76c80492626678a87a3f80 (HEAD -> master, origin/master, origin/HEAD)
    Merge: da44e62 4fdef09
    Author: Seppanen <spe@trainee.train>
    Date:   Sat Nov 10 18:46:16 2018 +0000

    Ilmoitukset mitä tuli samanaikaisesta päivityksestä
    Merge branch 'master' of https://github.com/SeppanenP/spegit

Ensimmäisellä rivillä on SHA-1 tarkastusrivi, jonka avulla voidaan jäljitää muutoksia myöhemmin testattavalla git diff komennolla. Seuraavana on muutoksen tekijä ja aikaleima. Viimeisenä kommentti/lisätieto muutoksesta, minkä olen itse kirjoittanut git commitin jälkeen.

# Git diff

Seuraavaksi kokeillaan mitä muutoksia kahdessa README.md tiedostojen välillä on tapahtunut. Komennon jälkeen laitetaan molempien tiedostojen SHA-1 luku, joka saadaan git logilla:

    git diff d931913a92f30ffd1c75cb13278564a4f810eb66 c57b75b8e0fae4952205327ef4e44c30756dd6f2

Tuloksena tuli:
 
    diff --git a/README.md b/README.md
    index 8521be4..035803f 100644
    --- a/README.md
    +++ b/README.md
    @@ -62,8 +62,6 @@ Ensimmäisellä rivillä on SHA-1 tarkastusrivi, jonka avulla voidaan jäljitä
 
    Seuraavaksi kokeillaan mitä muutoksia kahdessa README.md tiedostojen välillä on tapahtunut. Komennon jälkeen laitetaan    molempien tiedostojen SHA-1 luku, joka saadaan git logilla:
 
    -git diff 

Ylläolevassa tuloksessa ei näy värejä, mutta viimeinen rivi "-git diff" on punaisella ja se on ainoa muutos kahden viimeisimmän version välissä.

Seuraavaksi testataan samaa komentoa niin että tehdään muutoksia README.md tiedostoon komentorivillä, mutta ei julkaista niitä vielä push-komennolla GitHubiin.

Tuloksena seuraavaa:

    diff --git a/README.md b/README.md
    index 61718fb..72b93c6 100644
    --- a/README.md
    +++ b/README.md
    @@ -76,6 +76,8 @@ Seuraavaksi kokeillaan mitä muutoksia kahdessa README.md tiedostojen välillä
 
    Ylläolevassa tuloksessa ei näy värejä, mutta viimeinen rivi "-git diff" on punaisella ja se on ainoa muutos kahden viimeisimmän version välissä.
 
    +Seuraavaksi testataan samaa komentoa niin että tehdään muutoksia README.md tiedostoon komentorivillä, mutta ei julkaista  niitä vielä push-komennolla GitHubiin.
    +
 
Viimeinen rivi (+ merkkien välissä+) on vihreänä, eli olen tehnyt muutoksia README.md tiedostoon. Komennon avulla voidaan katsoa mitä muutoksia olet tehnyt ennen kuin julkaiset ne. Jos ehdit antaa komennon

    git add .

Muutoksia ei näe enää pelkällä git diff komennolla.
