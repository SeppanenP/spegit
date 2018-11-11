# Palvelimien hallinta harjoitus 3 GitHub ja git
Tehtävä piti tehdä MarkDownina GitHubissa. Aluksi pitää luoda GitHubiin varasto, johon tehtävä tehdään. Ohjeet alkutoimenpiteille löytyy Tero Karvisen sivuilta:
http://terokarvinen.com/2016/publish-your-project-with-github
Koneena minulla oli Fujitsu Lifebook  AH531, i3-2328M, Windows 7. Käyttämäni livetikun olin päivittänyt ensin uudempaan versioon Xubuntu 18.04.1 LST, koska olin huomannut jotain ongelmia työskennellessäni vanhalla tikulla, jossa oli Xubuntu 16.4.

## Varaston kopioiminen koneelle
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
Tämä teksti on kirjoitettu komentorivillä sen jälkeen, kun olin ensin kirjoittanut ensin otsikon ja ensimmäisen kappaleen aiheesta "Mitä tapahtuu jos samaa tiedostoa muokataan samanaikaisesti?" GitHubissa.

=======
## Mitä tapahtuu jos samaa tiedostoa muokataan samanaikaisesti?

Seuraavaksi testaan ihan mielenkiinnosta, mitä tapahtuu jos muokkaan samaa tiedostoa samaan aikaan GitHubissa. Tämä teksti tallennetaan ensin.
>>>>>>> 4fdef09e2c604ca8b794aec911a442612ea357f3

Tuloksena oli ristiriita:

    Auto-merging README.md
    CONFLICT (content): Merge conflict in README.md
    Automatic merge failed; fix conflicts and then commit the result.

eli en saanut julkaistua tekstiä komentoriviltä ja dokumenttiin tuli ohjelmallisesti ylläolevat ilmoitukset ennen otsikkoa (<<<<<HEAD) ja rivi otsikon kappaleen alle, jossa SHA-1 luku. Kun annoin uudelleen komennot add., git commit, git pull, git push muutokset onnituivat. Jätin tekstiin tulleet ilmoitukset näkyviin dokumenttiin.

## Git log

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

## Git diff

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

Seuraavaksi testataan samaa komentoa niin että tehdään muutoksia README.md tiedostoon komentorivillä, mutta ei julkaista niitä vielä push-komennolla GitHubiin ja annetaan muutosten jälkeen pelkkä:

    git diff

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

## Git Blame

Seuraavaksi testataan komentoa

    git blame

Komento palauttaa nykyisen tekstin kaikki rivit jossa näkyy viimeisen muutoksen aikaleima ja muutoksen tekijä:

    0c7bfaa (SeppanenP 2018-11-10 19:58:38 +0200   5) 
    e1506551 (SeppanenP 2018-11-10 20:04:46 +0200   6) # Varaston kopioiminen koneelle
    e1506551 (SeppanenP 2018-11-10 20:04:46 +0200   7) Kopioin omalta tililtäni luomani varaston osoitteen ja kopioin sen koneelle
    be4fc1d5 (SeppanenP 2018-11-10 20:19:38 +0200   8) 
    46c46344 (SeppanenP 2018-11-10 20:10:58 +0000   9)     git clone https://github.com/SeppanenP/spegit.git
    be4fc1d5 (SeppanenP 2018-11-10 20:19:38 +0200  10) 
    c01c9c08 (Seppanen  2018-11-10 18:25:39 +0000  11) Koneelle tuli spegit-kansio polkuun /home/xubuntu/spegit. Kansiosta löytyi README.md tiedosto.
    c01c9c08 (Seppanen  2018-11-10 18:25:39 +0000  12) 
    c01c9c08 (Seppanen  2018-11-10 18:25:39 +0000  13) Seuraavaksi aloin muokkaamaan tiedostoa komentoriviltä. Kun avasin sen siitä puuttui viimeisimmät GitHubissa tekemäni päivitykset. En ollut päivittänyt koneella olevaa versiota, joten palasin ilman muutoksia ja annoin komennon:
    c01c9c08 (Seppanen  2018-11-10 18:25:39 +0000  14) 
    46c46344 (SeppanenP 2018-11-10 20:10:58 +0000  15)     git pull

Tehtyjä muutoksia ei voi nähdä. Rivin alussa on vain SHA-1 luvun alku. Jos haluaa nähdä koko luvun esimerkiksi git diff-komentoa varten on annettava komento:

    git blame -l README.md

Tällöin rivin akuun tulee koko SHA-1 luku joka voidaan kopioida vaikkapa git diff vertailuun:

    0c7bfaadedc29c1d2374585e31eda81a4f858b7 (SeppanenP 2018-11-10 19:58:38 +0200   5) 
    e150655107721e4693017df8369830a93ed318f2 (SeppanenP 2018-11-10 20:04:46 +0200   6) # Varaston kopioiminen koneelle
    e150655107721e4693017df8369830a93ed318f2 (SeppanenP 2018-11-10 20:04:46 +0200   7) Kopioin omalta tililtäni luomani varaston    osoitteen ja kopioin sen koneelle
    be4fc1d54b0399589460446d4def566b40621520 (SeppanenP 2018-11-10 20:19:38 +0200   8) 
    46c4634406f4bf16199302c2ce4e2a436ab07b2b (SeppanenP 2018-11-10 20:10:58 +0000   9)     git clone https://github.com/SeppanenP/spegit.git
    be4fc1d54b0399589460446d4def566b40621520 (SeppanenP 2018-11-10 20:19:38 +0200  10) 
    c01c9c089804ad5b2bca81e84c77a0a418be3232 (Seppanen  2018-11-10 18:25:39 +0000  11) Koneelle tuli spegit-kansio polkuun      /home/xubuntu/spegit. Kansiosta löytyi README.md tiedosto.

## Oma moduli /srv/salt Gittiin

Tein ensimmäiseksi spegit-varastoon kansiot srv/salt. Salt kansioon tein top.sls-tiedoston:

    base:
      '*':
        - tools
        
Tein salt kansioon kansion tools, johon tein init.sls-tiedoston:

    install_tools:
      pkg.installed:
        - pkgs:
        - gimp
        - vlc
        
Tämän jälkeen kloonasin paketit livetikku-koneelle

    git pull
   
Seurravaksi jouduin etsimään monestakin paikasta miten saan ajettua salt-minionilla modulin. Parhaimmat neuvot löysin aikaisemman kurssin Katri Laulajaisen oppilastyöstä:

https://katrilaulajainen.wordpress.com/2018/05/02/palvelinten-hallinta-h5-29-4-2018-git-github-ja-markdown/

En tehnyt high.sh-tiedostoa vaan menin spegit kansioon ja annoin komennon:

    sudo salt-call --local state.highstate --file-root srv/salt
        
Hetkeen ei tapahtunut mitään, mutta lopulta molemmat ohjelmat asentuivat koneelle.

## Lähteet

http://terokarvinen.com/

https://git-scm.com/book/fi/v1/Gitin-perusteet-Muutosten-tallennus-tietol%C3%A4hteeseen

https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History

https://git-scm.com/docs/git-blame

https://katrilaulajainen.wordpress.com/2018/05/02/palvelinten-hallinta-h5-29-4-2018-git-github-ja-markdown/

https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet


