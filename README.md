# AI-avusteinen ohjelmistokehitys: määrittelyvetoinen kehitys (Spec-Driven Development) ja GitHub Spec Kit -työkalu

## Johdanto

Tekoäly on viime vuosina muuttanut ohjelmistokehityksen työskentelytapoja merkittävästi. Kehittäjät voivat hyödyntää erilaisia tekoälypohjaisia työkaluja koodin kirjoittamiseen, virheiden etsimiseen sekä ohjelmistojen suunnitteluun. Tekoälyn hyödyntämisen alkuvaiheessa tyypillinen tapa käyttää tekoälyä ohjelmistokehityksessä on ollut niin sanottu **prompt-pohjainen työskentely**, jossa kehittäjä kuvaa halutun toiminnallisuuden tekoälylle ja pyytää sitä tuottamaan koodia. Tätä lähestymistapaa kutsutaan usein epävirallisesti *vibe-koodaukseksi*, jossa kehitys etenee nopeasti ideasta koodiin kokeilemalla ja iteratiivisesti tarkentamalla tekoälylle annettuja kehotteita.

Vaikka tällainen työskentelytapa voi olla tehokas erityisesti pienissä kokeiluissa, kuten pohjakoodin (boilerplate-koodin) tuottamisessa, se voi suuremmissa projekteissa johtaa haasteisiin. Jos kehitys etenee pelkästään yksittäisten promptien ohjaamana, projektin kokonaisrakenne ja suunnitteluperiaatteet voivat jäädä epäselviksi. Tällöin myös tekoälyn tuottaman koodin laatu ja yhtenäisyys riippuvat vahvasti siitä, kuinka hyvin kehittäjä onnistuu kuvaamaan projektin kontekstia yksittäisissä kehotteissa. Tämän vuoksi tekoälyavusteisessa ohjelmistokehityksessä on alettu kiinnittää enemmän huomiota siihen, millaista kontekstia tekoälylle annetaan. Tätä lähestymistapaa kutsutaan usein *context engineeringiksi*, jossa tekoälyn käyttöä tuetaan rakenteisella ja hyvin määritellyllä projektikontekstilla.

Yksi tapa toteuttaa tällaista lähestymistapaa on **määrittelyvetoinen kehitys** (*Spec-Driven Development*). Tässä kehitystavassa ohjelmiston toteutus perustuu selkeään määrittelyyn, joka kuvaa mitä järjestelmän tulee tehdä, miten se on suunniteltu ja millaisiin tehtäviin toteutus voidaan jakaa. Määrittely toimii sekä kehittäjän että tekoälytyökalujen yhteisenä lähtökohtana, jonka pohjalta varsinainen toteutus tehdään. Näin kehitysprosessiin saadaan enemmän rakennetta ja läpinäkyvyyttä, vaikka toteutus tapahtuisi osittain tai lähes kokonaankin tekoälyn avulla.

Tässä materiaalissa tarkastellaan ensin AI-avusteisen ohjelmistokehityksen yleisiä työskentelytapoja ja verrataan **vibe-koodausta** ja **määrittelyvetoista kehitystä**. Tämän jälkeen tutustutaan tarkemmin **GitHubin Spec Kit -työkaluun**, joka tukee määrittelyvetoista kehitysprosessia ja auttaa rakentamaan tekoälylle selkeän projektikontekstin. Materiaalin lopuksi käydään läpi pieni esimerkkiprojekti, jossa nähdään käytännössä, miten määrittely, suunnittelu ja toteutus voidaan toteuttaa Spec Kit -työkalun avulla.


## AI-avusteinen kehitys ja vibe-koodaus

Yksi yleinen tapa hyödyntää tekoälyä ohjelmistokehityksessä on edellä mainittu **vibe-koodaus** (*vibe coding*). Termillä viitataan epämuodolliseen työskentelytapaan, jossa kehittäjä etenee ideasta suoraan toteutukseen keskustelemalla tekoälyn kanssa. Kehittäjä kuvaa haluamansa toiminnallisuuden kehotteella (*prompt*), jonka perusteella tekoäly tuottaa koodia. Tämän jälkeen kehittäjä arvioi tulosta, pyytää muutoksia tai lisäominaisuuksia ja jatkaa keskustelua, kunnes ratkaisu vastaa haluttua lopputulosta.

Vibe-koodauksessa kehitys etenee tyypillisesti iteratiivisesti: jokainen uusi kehote tarkentaa tai muuttaa aiemmin tuotettua koodia. Prosessi muistuttaa jatkuvaa vuoropuhelua kehittäjän ja tekoälyn välillä, jossa ratkaisu rakentuu vähitellen useiden keskustelukierrosten aikana.

Tyypillinen vibe-koodauksen työskentelyprosessi voidaan kuvata seuraavasti: ![Vibe-koodaus](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/vibe_koodaus.PNG)

Tämä lähestymistapa voi olla hyvin tehokas erityisesti silloin, kun tavoitteena on nopeasti kokeilla uusia ideoita tai tuottaa yksinkertaista toiminnallisuutta. Tekoäly pystyy usein tuottamaan käyttökelpoista koodia jo muutaman kehotteen perusteella, mikä tekee vibe-koodauksesta hyödyllisen esimerkiksi prototyyppien (proof-of-concept) rakentamisessa tai pohjakoodin tuottamisessa.

Samalla työskentelytapa voi kuitenkin aiheuttaa haasteita suuremmissa projekteissa. Koska kehitys etenee pääasiassa yksittäisten kehotteiden kautta, projektin kokonaisrakenne ja suunnitteluperiaatteet eivät välttämättä muodostu systemaattisesti. Jos projektin tavoitteita, arkkitehtuuria tai keskeisiä ratkaisuja ei kuvata selkeästi, tekoälyn tuottama koodi voi muodostua epäyhtenäiseksi tai vaikeasti ylläpidettäväksi.

Näiden haasteiden vuoksi AI-avusteisessa ohjelmistokehityksessä on alettu kiinnittää enemmän huomiota siihen, miten projektin tavoitteet, rakenne ja suunnitteluperiaatteet voidaan kuvata tekoälylle selkeästi. Seuraavassa luvussa tarkastellaan yhtä tällaista lähestymistapaa, **määrittelyvetoista kehitystä (Spec-Driven Development)**, jossa kehitystä ohjaa ensin laadittu määrittely.


## Määrittelyvetoinen kehitys (Spec-Driven Development)

Määrittelyvetoinen kehitys (*Spec-Driven Development*) on ohjelmistokehityksen lähestymistapa, jossa ohjelmiston toteutus perustuu ensin laadittuun määrittelyyn. Sen sijaan että kehitys etenisi suoraan ideasta koodiin, projektin tavoitteet, toiminnallisuudet ja keskeiset suunnitteluratkaisut kuvataan ensin rakenteisessa muodossa. Tätä määrittelyä käytetään myöhemmin sekä kehittäjän että tekoälytyökalujen yhteisenä lähtökohtana toteutukselle.

Tässä lähestymistavassa määrittely ei ole pelkkä dokumentti, vaan keskeinen osa kehitysprosessia. Se kuvaa esimerkiksi, mitä järjestelmän tulee tehdä, millaisia käyttötapauksia sillä on ja millaisia teknisiä ratkaisuja projektissa aiotaan käyttää. Kun nämä asiat on kuvattu selkeästi, voidaan toteutus jakaa pienempiin tehtäviin ja edetä niiden toteuttamisessa järjestelmällisesti.

Määrittelyvetoinen kehitys voidaan nähdä myös yhtenä sovelluksena ohjelmistokehityksen yleisestä elinkaarimallista (*Software Development Life Cycle*, SDLC). SDLC kuvaa ohjelmistoprojektin keskeiset vaiheet, kuten vaatimusten määrittelyn, suunnittelun, toteutuksen ja testauksen. Määrittelyvetoisessa kehityksessä näitä samoja vaiheita hyödynnetään, mutta niitä sovelletaan erityisesti AI-avusteiseem kehitykseen liittyen siten, että projektin määrittely toimii lähtökohtana myös tekoälytyökalujen käytölle.

Tätä määrittelyä ei kuitenkaan pidä sekoittaa perinteiseen vesiputousmallin mukaiseen määrittelyyn. Vesiputousmallissa tavoitteena oli laatia mahdollisimman kattava ja yksityiskohtainen määrittely projektin alkuvaiheessa ennen varsinaisen toteutuksen aloittamista. Määrittelyvetoisessa kehityksessä määrittely toimii sen sijaan kehitystä ohjaavana lähtökohtana, jota voidaan tarkentaa ja täydentää projektin edetessä.

Määrittelyvetoista kehitystä voidaan kuvata seuraavanlaisena prosessina: ![Spec-koodaus](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/spec_koodaus.PNG)


Tässä mallissa määrittely toimii kehityksen lähtökohtana ja ohjaa seuraavia vaiheita. Teknisessä suunnitelmassa kuvataan esimerkiksi arkkitehtuuri, käytettävät teknologiat sekä keskeiset suunnitteluratkaisut. Tämän jälkeen projekti pilkotaan toteutettaviin tehtäviin, joiden pohjalta varsinainen toteutus voidaan tehdä vaiheittain.

AI-avusteisessa ohjelmistokehityksessä määrittelyvetoisen lähestymistavan etuna on se, että se tarjoaa tekoälylle selkeämmän ja laajemman kontekstin. Kun projektin tavoitteet, rakenne ja suunnitteluperiaatteet on kuvattu etukäteen, tekoäly pystyy tuottamaan koodia johdonmukaisemmin ja paremmin projektin kokonaisuuteen sopivalla tavalla.

Määrittelyvetoinen kehitys ei kuitenkaan tarkoita jäykkää tai täysin lineaarista prosessia. Määrittelyä voidaan päivittää projektin aikana, ja kehitys voi edelleen olla iteratiivista. Keskeinen ero vibe-koodaukseen on se, että projektin kokonaiskuva pyritään kuvaamaan ensin, ennen kuin toteutusta laajennetaan merkittävästi.

Seuraavassa luvussa käydään läpi tarkemmin **GitHub Spec Kit**, työkalun toimintaperiaatetta ja toiminnan eri vaiheita.


## GitHub Spec Kit

Määrittelyvetoista kehitystä voidaan toteuttaa myös ilman erityisiä työkaluja esimerkiksi kirjoittamalla määrittely- ja suunnitteludokumentteja perinteisiin dokumentteihin tai projektinhallintajärjestelmiin. AI-avusteisesta ohjelmistokehitystä varten on kuitenkin alettu myös kehittää työkaluja, jotka tukevat tätä lähestymistapaa ja auttavat rakentamaan tekoälylle selkeän ja rakenteisen projektikontekstin.

Yksi tällainen työkalu on **GitHub Spec Kit**, joka tarjoaa rakenteen määrittelyvetoisen kehitysprosessin toteuttamiseen. Spec Kit auttaa kehittäjää jäsentämään projektin keskeiset vaiheet – määrittelyn, teknisen suunnittelun ja toteutettavat tehtävät – ja käyttämään näitä vaiheita tekoälyavusteisen kehityksen tukena.

Spec Kit -lähestymistavassa projekti etenee tyypillisesti seuraavien vaiheiden kautta: ![Spec Kit](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/spec_kit.PNG)


- **Määrittely (Spec)** kuvaa mitä järjestelmän tulee tehdä ja millaisia käyttötapauksia sillä on  
- **Suunnitelma (Plan)** sisältää teknisen suunnitelman, kuten arkkitehtuurin ja keskeiset tekniset ratkaisut
- **Tehtävät (Tasks)** jakaa toteutuksen pienempiin kehitystehtäviin
- **Toteutus (Implementation)** on varsinainen toteutusvaihe, jossa koodi tuotetaan vaiheittain

Spec Kit tukee erityisesti AI-avusteista kehitystä, koska se auttaa rakentamaan projektista selkeän kontekstin, jota tekoäly voi hyödyntää. Sen sijaan että tekoälylle annettaisiin yksittäisiä irrallisia kehotteita, projektin tavoitteet, rakenne ja toteutettavat tehtävät kuvataan ensin rakenteisessa muodossa. Tämä auttaa tuottamaan johdonmukaisempaa koodia ja helpottaa projektin kokonaisuuden hallintaa.

Seuraavassa luvussa tarkastellaan käytännössä, miten Spec Kit -työkalu otetaan käyttöön ja miten sitä voidaan käyttää pienen esimerkkiprojektin toteuttamiseen.


## Spec Kit -työkalun käyttöönotto

Tässä ohjeessa lähdetään tilanteesta, jossa koneelle on jo asennettu **Git** ja käyttäjällä on olemassa **GitHub-tili**, johon on kirjauduttu.  

Jos Git-sovellusta ei vielä ole asennettu, ohjeet sen asentamiseen löytyvät Gitin sivuilta:  
https://git-scm.com/install/windows

### uv-paketinhallinnan asentaminen

Ennen varsinaisen Spec Kit -työkalun käyttöönottoa on varmistettava, että koneelle on asennettu **uv-paketinhallintasovellus**.  

Asennusohjeet löytyvät uv-projektin sivuilta:  
https://docs.astral.sh/uv/

Windowsissa uv voidaan asentaa avaamalla **PowerShell** ja suorittamalla seuraava komento:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
Asennuksen jälkeen uv-työkalu pitää lisätä Windowsissa tilin ympäristömuuttujiin. Tämä tapahtuu hakemalla Windowsin hakutoiminnolla 'Muokkaa tilin ympäristömuuttujia' ja avaamalla kyseinen toiminto. Käyttäjän muuttujien alta valitaan Path ja sen jälkeen painike 'Muokkaa'. Tästä avautuu ikkuna 'Muokkaa ympäristömuuttujaa', josta valitaan 'Uusi' ja lisätään hakemistopolku: C:\Users\OMAKÄYTTÄJÄTUNNUSTÄHÄN\.local\bin. Polun tallennus hyväksytään valitsemalla 'Ok' ja tämän jälkeen käynnistetään PowerShell uudelleen.

Kun uv-paketinhallinta on asennettu, päästään asentamaan Spec Kit -työkalu. Tämä tapahtuu edelleen PowerShellissä komennolla:
```powershell
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```
Tämä asentaa varsinaisen työkalun **Specify CLI:n**, joka on komentorivillä toimiva Spec Kit -työkalu.

Spec Kit itsessään ei sisällä varsinaista AI-avustajaa, vaan ennen varsinaista työkalun kokeilua pitää vielä asentaa haluttu koodausagentti. Spec Kit tukee monia erilaisia ja tässä käytetään **GitHub Copilotia**. Tämän asennus hoituu seuraavasti:
```powershell
winget install GitHub.Copilot
```

Tämän jälkeen pitää vielä kirjautua GitHub Copilot CLI:n kautta GitHub tilille. Tämä tapahtuu kirjoittamalla ensin komentoriville 'copilot' ja antamalla tämän jälkeen komento '/login'. Tämä käynnistää kirjautumisprosessin, jossa komentoriville näkyviin tuleva koodi pitää syöttää selaimessa aukeavalle GitHub-kirjautumissivulle.


## Esimerkkiprojekti

Tässä luvussa tarkastellaan käytännössä, miten Spec Kit -työkalua voidaan hyödyntää määrittelyvetoisessa ohjelmistokehityksessä. Esimerkkiprojektina toteutetaan yksinkertainen **matopeli (Snake)**, joka toimii pienenä ja selkeänä esimerkkinä Spec Kit -työskentelyprosessista.

Projektin tarkoituksena ei ole rakentaa mahdollisimman monimutkaista sovellusta, vaan havainnollistaa, miten ohjelmistokehitys voidaan aloittaa määrittelystä ja edetä vaiheittain suunnittelun kautta toteutukseen. Samalla nähdään, miten Spec Kitin tarjoamat komennot ja rakenteet auttavat jäsentämään projektin eri vaiheita ja tuottamaan tekoälylle selkeän kontekstin kehitystyötä varten.

Esimerkin aikana käydään läpi Spec Kit -työskentelyn keskeiset vaiheet: projektin määrittelyn laatiminen (spec), teknisen suunnitelman muodostaminen (plan) sekä toteutettavien tehtävien jäsentäminen (tasks). Näiden vaiheiden jälkeen projekti voidaan toteuttaa tekoälyavusteisesti (implement).

Spec Kit projektin alustus tapahtuu suorittamalla komentorivillä seuraava komento, jossa annetaan projektin nimi (tässä: my-project) ja samalla voidaan asettaa projektissa käytettävä AI-agentti (tässä: copilot). Komento tekee projektikansion ja alustaa sinne Spec Kit -rakenteen:

```powershell
specify init my-project --ai copilot
```

Tämän jälkeen siirrytään komentorivillä hakemistoon:
```powershell
cd my-project
```

Seuraavaksi käynnistetään Copilot CLI kirjoittamalla komentoriville komento:
```powershell
copilot
```

Copilot-sovelluksessa valitaan shift+tab -näppäinyhdistelmällä **autopilot**-tila.

![copilot constitution](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/constitution_prompti.PNG)

### Projektin periaatteiden määrittely
Aluksi projektille pitää generoida pohjasäännnöt eli projektin periaatteet, jotka toimivat ohjeina kaikille myöhemmille vaiheille. Tämä tapahtuu suorittamalla komentoriville käynnistetyssä copilot-sovelluksessa speckit.constitution-komento esimerkiksi seuraavasti:
```powershell
speckit.constitution Luo projektille periaatteet: koodin laatu, testattavuus, suorituskyky ja johdonmukainen käyttöliittymä
```
Tämän perusteella generoituu **constitution.md** tekstitiedosto, jonka yhteenveto komentoriville Copilot-sovelluksen tulostamana näyttää seuraavalta:![constitution](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/constitution_tulos_final.PNG)

### Projektin määrittely (spec)
Seuraavana vaiheena tehdään projektille määrittely, suorittamalla **speckit.specify** -komento. Tähän kirjoitetun määrittelyn ei tarvitse olla pitkä ja yksityiskohtainen. Riittää että siinä kerrotaan sovellukselta haluttavat ominaisuudet ylemmällä tasolla. Näiden tietojen perusteella AI-agentti rakentaa Spec Kitin pohjatiedoston ohjeiden perusteella varsinaisen määrittelydokumentin. Seuraavassa esimerkki tämän vaiheen suorittamisesta:
```powershell
speckit.specify Tee perinteinen matopeli sovellus, jossa on aloitusruutu, josta peli käynnistetään, itse
  peli-ikkuna ja pelin päätyttyä 'Game over'- näkymä, jossa näkyy myös Top10 tulokset pelissä. Jos pelaaja sai Top10
  pistemäärän pelin tuloksena, pelaaja ohjataan syöttämään oma nimensä ja tulos lisätään oikeaan kohtaan Top10
  tuloksia. Top10-näkymästä pitää päästä käynnistämään uusi peli
```
Tämän vaiheen lopputuloksena syntyy varsinainen määrittelydokumentaatio, **spec.md** -tiedosto. Tiedosto voidaan avata koodieditorilla ja tarkastella ja muokata sisältöä tarpeen mukaan. Tämän vaiheen yhteenveto komentorivillä näyttää seuraavalta:![spec](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/spec_tulos.PNG)

### Projektin suunnittelu (plan)
Kun projektin toiminnallinen määrittely on laadittu, seuraava vaihe on teknisen toteutuksen suunnittelu. Tässä vaiheessa määritellään keskeiset tekniset ratkaisut, kuten käytettävä ohjelmointikieli, keskeiset kirjastot sekä mahdolliset ulkoiset komponentit tai tietovarastot.

Spec Kitin **plan**-vaiheessa tekoälyä pyydetään muodostamaan projektin tekninen suunnitelma määrittelyn perusteella. Suunnitelma kuvaa esimerkiksi sovelluksen arkkitehtuurin, pääkomponentit sekä keskeiset tekniset valinnat. Tämä auttaa varmistamaan, että toteutus etenee johdonmukaisesti ja että kaikki myöhemmät kehitystehtävät perustuvat samaan tekniseen lähtökohtaan. 

Tässä esimerkissä valitaan ohjelmointikieleksi **Python**, graafisen käyttöliittymän kirjastoksi Qt-kirjaston Python-versio **PySide6** ja tietokantaratkaisuksi **SQLite**, jota käytetään Pythonin sisältämän sqlite3 kirjaston kautta.

Esimerkkiprojektissa suunnitelma muodostetaan Copilot-sovelluksessa seuraavasti:

```powershell
speckit.plan Sovelluksen ohjelmointikieli on Python ja sovellus käyttää Qt:n
  Pyside6-kirjastoa Pythonille. Top10-tulosten tallentamiseen käytetään paikallista sqlite
  tietokantaa.
```

Vaiheen tuloksena syntyy **plan.md** tiedosto ja vaiheen yhteenveto komentorivillä näyttää seuraavalta:![plan](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/plan_tulos.PNG)

### Toteutustehtävien määrittely (tasks)
Kun projektin määrittely ja tekninen suunnitelma on laadittu, seuraava vaihe on jakaa toteutus pienempiin ja hallittaviin kehitystehtäviin. Tässä vaiheessa määritellään, mitä konkreettisia työvaiheita projektin toteuttaminen vaatii ja missä järjestyksessä niiden toteuttamisessa voidaan edetä.

Spec Kitin **tasks**-vaiheessa AI-agentti muodostaa teknisen suunnitelman perusteella listan toteutettavista kehitystehtävistä. Jokainen tehtävä kuvaa jonkin yksittäisen toiminnallisuuden tai ohjelmiston osan toteuttamista. Näin projekti pilkotaan selkeisiin vaiheisiin, joita voidaan toteuttaa yksi kerrallaan.

Tämä vaihe on hyödyllinen AI-avusteisessa ohjelmistokehityksessä, koska tekoäly pystyy tuottamaan parempia tuloksia, kun tehtävät ovat rajattuja ja tarkasti määriteltyjä. Sen sijaan että tekoälylle annettaisiin suuri ja epämääräinen kokonaisuus, toteutus jaetaan pienempiin osiin, jotka on helpompi toteuttaa, tarkistaa ja testata vaiheittain.

Esimerkkiprojektissa toteutettavat kehitystehtävät generoidaan seuraavalla komennolla:

```powershell
speckit.tasks
```

Tämän perusteella syntyy **tasks.md** tekstitiedosto. Tämän esimerkin kohdalla projekti jakautuu kuuteen eri vaiheeseen ja niihin sisältyy yhteensä 46 tehtävää. Tehtävävaiheen yhteenveto komentorivillä on tässä esimerkissä seuraavanlainen:![tasks](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/task_tulos.PNG)

### Toteutus (implementation)
Kun edellä esitellyt vaiheet on suoritettu, päästään varsinaiseen sovelluksen toteutukseen ja koodin generointiin. Tämä käynnistetään seuraavalla komennolla:

```powershell
speckit.implement
```

**Implement**-vaiheessa AI-agentit käyttävät aiemmissa vaiheissa luotuja dokumentteja, kuten projektin määrittelyä (spec), teknistä suunnitelmaa (plan) sekä toteutettavien tehtävien listaa (tasks). Näiden perusteella tekoäly alkaa toteuttaa sovellusta vaiheittain.

Käytännössä toteutus etenee yleensä yksi tehtävä kerrallaan. Tekoäly generoi tarvittavaa lähdekoodia, luo uusia tiedostoja tai täydentää olemassa olevia tiedostoja projektin suunnitelman mukaisesti. Kehittäjä voi tämän jälkeen tarkistaa tuotetun koodin, testata sen toiminnan ja tarvittaessa pyytää tekoälyä tarkentamaan tai korjaamaan toteutusta.

Tämä työskentelytapa yhdistää määrittelyvetoisen kehityksen rakenteen ja AI-avusteisen ohjelmoinnin iteratiivisuuden: projektin kokonaisuus on kuvattu etukäteen, mutta toteutus etenee pieninä ja hallittavina vaiheina.

Seuraavassa kuvassa näkyy esimerkki Copilot-sovelluksen tulostamasta yhteenvedosta toteutusvaiheen lopuksi:![implementation](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/implementation_tulos.PNG)

Toteutusvaiheessa AI-agentti tuotti tehtävälistan mukaisesti testitapaukset, toteutti sovelluksen ohjelmakoodin ja suoritti testitapaukset. Testitapausten perusteella löytyneiden puutteiden korjaus tapahtui edelleen automaattisesti, ja lopputuloksena oli määrittelyn mukainen toimiva sovellus.

Tuotettu dokumentaatio ja ohjelmakoodi sijoitettiin AI-agentin toimesta automaattisesti luotuun hakemistorakenteeseen tarkoituksenmukaisiin kansioihin. Tämän pienen esimerkkiprojektin kohdalla sovelus jakautui ehkä liiankin pieniin tiedostokohtaisiin kokonaisuuksiin. Toisaalta rakenne edustaa selkeää ammattimaista lähestymistapaa, jonka perusteella projektin jatkaminen ja laajentaminen olisi helppoa.

![folder](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/hakemistorakenne_tulos.PNG)

Sovelluksen jatkokehittäminen onnistuu samassa ympäristössä kirjoittamalla komentorivillä Copilot CLI-sovellukselle uusia kehotteita. Aiemmissa vaiheissa tuotettuja määrittely- (spec.md), suunnitelma- (plan.md) ja tehtävälistatiedostoja (tasks.md) voidaan päivittää muokkaamalla niitä käsin tai AI-agentin avustuksella. Keskustelu AI-agentin kanssa tapahtuu projektin kontekstissa, agenttia voidaan esimerkiksi pyytää suorittamaan testitapaukset uudelleen tai jatkamaan sovelluksen kehittämistä uusien ominaisuuksien toteuttamiseksi.

## Tausta
Materiaali on tuotettu Seinäjoen ammattikorkeakoulussa osana Tekoälyn hyödyntäminen ja ohjelmistorobotiikka -hanketta, joka on Jatkuvan oppimisen ja työllisyyden palvelukeskuksen (JOTPA) rahoittama. Palvelukeskus edistää työikäisten osaamisen kehittämistä ja osaavan työvoiman saatavuutta. Palvelukeskuksen toimintaa ohjaavat opetus- ja kulttuuriministeriö sekä työ- ja elinkeinoministeriö.

![jotpa](https://github.com/SeAMKedu/Maarittelyvetoinen_kehitys_ja_SpecKit/blob/main/images/rahoittaja_jotpa.png)