# Sekalaista tietoturva-asiaa

## Sovelluksen autorisaatio, autentikointi, ympäristömuuttujat ja tekoäly
**Autentikointi** tarkoittaa käyttäjän henkilöllisyyden varmistamista esimerkiksi kirjautumisen avulla. **Autorisointi** puolestaan määrittää, mitä oikeuksia käyttäjällä on sovelluksessa. Esimerkiksi pääsyä näkymiin tai oikeuteen poistaa tai muokata objekteja voi olla rajoitettu käyttäjäroolien perusteella.

Jos sovelluksessa on eri rooleja, niin käyttäjien autorisoinnissa täytyy olla tarkkana. Ei riitä, että käyttäjän oikeudet käyttää toiminnallisuutta tarkistetaan vain frontin puolella. Tarkistukset täytyvät olla toteutettuna myös backendissä, jotta toimintoja ei voida suorittaa autentikoinnin tai autorisoinnin ohi esimerkiksi suoria API-kutsuja tekemällä. Tekoälytyökaluja voidaan myös käyttää hyödyksi mahdollisten tietoturva-aukkojen huomaamiseen. Lopullinen vastuu tietoturvasta on kaikilla. Siksi on hyvä käydä ratkaisuja läpi yhdessä ryhmän ja teknisenohjaajan kanssa. 

**Ympäristömuuttujat, henkilötiedot tai muut salassa pidettävät asiat eivät saa päätyä julkiseksi esimerkiksi GitHubiin.** Salaisuuksia hallitaan esimerkiksi .env tiedostoilla ja lisäämällä lokaalisti pidettävät tiedostojen nimet tai pelkästään tyypit .gitignoreen. On myös huomattava, että tällaiset asiat eivät saisi olla myöskään julkisesti saatavan docker kuvan sisällä. .gitignore ja .dockerignore tiedostot kannattaa lisätä repositorioon jo kehityksen alkuvaiheessa. Esimerkkiä voi ottaa .gitignoreen tästä ja .dockerignoreen tästä. 

**Tarkkaavaisuutta vaaditaan myös AI työkaluja käytettäessä.** Tekoälytyökalujen käyttäjillä on vastuu ottaa selvää miten tietyt (henkilökohtaiset) tiedostot voidaan konfiguroida pois työkalun nähtäviltä. Lisäksi osa datasta tai koodista voi olla salaista: on hyvä tarkistaa asiakkaalta mitä saa antaa työkalun analysoitavaksi etenkin jos koodi on yksityistä. Mitään henkilötietoja tai muuta luottamuksellista dataa ei saa käsitellä millään tekoälytyökalulla, ei edes CurreChatillä.

**Tietoturva on jokaisen sovelluskehittäjän vastuulla.** Tekoälytyökalut ovat tulleet jäädäkseen ja niiden vastuullinen käyttö on jopa suotavaa. Kuitenkin generoitu koodi tulisi käydä läpi ja ymmärtää sen toiminta. Perusasiat kannattaa opetella yhä huolella ja koodausrutiinia pitää yllä. Mitä paremmin ymmärtää jonkin ohjelmointikielen ominaisuuksia ja alaa, jolle sovellusta tuotetaan, sitä parempia ja tarkempia prompteja saa aikaiseksi.

**Ennen commitin luomista ja puskemista GitHubiin tai vastaavaan, tarkista vielä lisätyt (tai poistetut) rivit.** Tällä usein välttää väärien asioiden julkaisemisen.

## Testidata
- Generoi data: älä käytä omia tai muiden henkilötietoja testidatana!

## Vääriä asioita sisältävä commit tehty lokaalisti
- `git reset` on hyödyllinen: [lue lisää](https://git-scm.com/docs/git-reset) 

## Mitä tehdä, jos salaisuus tai henkiltietoja versionhallintaan (GitHub)
- Älä salaa asiaa! On tärkeää reagoida asiaan nopeasti.
- Jos salaisuuksia julkaistu GitHubiin, niin salasanat, apiavaimet tai muut vastaavat on vaihdettava välittömästi.
- Henkilötietoja sisältävien sql dumppien,tai muiden vastaavien tiedostojen osalta paras vaihtoehto on
  1. Laittakaa repository yksityiseksi
  2. Ottakaa yhteyttä ohjaajaan ja tekniseenohjaajaan, jotta repositorio saadaan palautettua tilaan, jossa historia on palautettua virheellistä committia edeltävään tilaan
- Myös ryhmän tuki on tärkeää ja ketään ei tulisi mollata tai turhaan syyttää tapahtuneesta. Parempi on pohtia sitä, mikä johti tilanteeseen ja miten vastaavaat tilanteet vältetään jatkossa. Myös ohjaajalle on hyvä selittää tilanne.

## OpenShift-/okd-konttialustojen konfiguraatiotiedostot
`Secret` -tiedostot **eivät** saa olla julkisesti esillä. `ConfigMap`-tiedostojen ei tulisi sisältää mitään salaista. Mutta koska sinne voi lisätä periaatteessa mitä vain, se ovat pontentiaalinen uhka vuotaa tietoja, joten näiden kanssa tulee olla erityisen huolellinen.

## Muuta
Jos käytätte palveluita tai luotte esimerkiksi gmail tai muita tunnuksia kehityksen aikana, mutta jäävät turhaksi, niin ne kannattaa ehdottomasti poistaa käytön loppuessa.

Yleisenä nyrkkisääntönä voi pitää, että jos tiedostossa on plain text tai vaikka base64-enkoodattuja tokeneita, käyttiksiä ja salasanoja tai muuta vastaavaa tietoa, niin ei saa päätyä julkiseen repoon tai edes git-historiaan.

## Ympäristöspesifejä linkkejä
### Node
#### .npmrc konfiguraatio
Tiedoston .npmrc konfiguraatio auttaa varautumaan [supply chain -hyökkäyksiä]((https://www.cloudflare.com/learning/security/what-is-a-supply-chain-attack/)) vastaan. Lisäämällä viikon viiveen uusien riippuvuuksien lataukseen pienennetään todennäköisyyttä sille, että kehittäjä ajaisi koneellaan haitallista koodia. Tähänkään ei voi sokeasti luottaa. Etenkin uusia, itselle vieraita paketteja asennellessa on hyvä noudattaa tervettä varovaisuutta.

0. npm version tulee olla vähintään v11.10.0. Version tarkistus. `npm --version`. Version voi päivittää globaalisti komennolla `npm install -g npm@latest`.

1. Tiedosto [`.npmrc`](https://docs.npmjs.com/cli/v8/configuring-npm/npmrc) luodaan projektin juureen samaan paikkaan package.json tiedoston kautta

- Joissakin repoissa on monta eri node-projektia: reposta löytyy eri paikoista `package.json`-tiedosto. `.npmrc`-tiedosto tulee olla samoissa paikoissa kuin `package.json`-tiedosto. Tiedoston sisältö:  

  ### .npmrc
  ```sh
  ignore-scripts=true
  min-release-age=7
  save-prefix=
  ```
  **ignore-scripts=true** -> estää pakettien ensi-/jälkiasennusskriptien toiminnan. Näitä on legitiimissä käytössä erittäin harvoin.  
  **min-release-age=7** -> estää alle viikon vanhojen pakettien asennuksen. Mahdolliset haavoittuvuudet huomataan ja korjataan yleensä aikaisemmin.  
  **save-prefix=** -> npm (jos ei eksplisiittisesti kielletä) asettaa asennettujen pakettien versionumeron eteen `^`-symbolin `package.lock`-tiedostossa, joka sallii paketin minor/patch -versioiden noston ajaessa `npm install`.

2. Tarkista konfiguraatio ajamalla node projektin juuresssa

    `npm config list`

- Komennon ulostulosta tulisi löytyä luodun `.npmrc`-tiedoston asetukset. 


3. Varmista, että konfigaraatiot tulevat Docker-kuvaan esimerkiksi kopioimalla se **Dockerfilessä** komennolla


    ```Dockerfile
    COPY ./package* ./
    COPY ./.npmrc ./ 
    # jonka jälkeen
    RUN npm ci
    # tai
    # RUN npm install
    ```

4. Cypress post installation tarvittaessa komennolla (esimerkiksi actions workflow)

    `npx cypress install`

## Python
Myös Pythonilla kehitettäessä käytetään ulkoisia kirjastoja. [Supply chain -hyökkäyksiä]((https://www.cloudflare.com/learning/security/what-is-a-supply-chain-attack/)) voidaan välttää esimerkiksi määrittämällä täsmällinen versionumero ([esim. pip työkalussa](https://pip.pypa.io/en/stable/cli/pip_index/#cmdoption-uploaded-prior-to)). Kuitenkin uusien, itselle tuntemattomien pakettien asentamisessa kannattaa noudattaa tervettä varovaisuutta.

Jupyter Notebookien avulla pystyy yhdistämään koodia ja tekstiä mukavasti sekä datan visualisointi onnistuu hyvin. Ennen kuin notebookin sisällön puskee julkiseksi, on hyvä tarkistaa, että eihän outputissa ole printattuna mitään ympäristömuuttujia tai muuta sellaista, jota ei tulisi julkisesti esittää. Yleisesti voi olla hyvä tapa tyhjentää notebookin koodiblokkien tulosteen ennen committia.

Pythonilla ohjelmoidessa on hyvä käyttää virtuaaliympäristöä, johon tarvittavat ohjelmat asennetaan.


## Github repojen workflow't ja asetukset
Mahdollisten riippuvuuksien päivittämistarpeen tarkistaminen voidaan osittain automatisoida esimerkiksi Renovate botin avulla. Kirjastojen ikä on usein varsin nuori. **[Workflow'n](https://docs.github.com/en/actions/concepts/workflows-and-actions/workflows) ajo voidaan haluta estää tietyiltä käyttäjiltä.** Alla pari esimerkkiä, joissa renovate botin tekemät testien ajot on estetty feature brancheille ja pullareille konfiguroiduissa workflow tiedostoissa (`.github/workflows/.*y(a)ml`):
- pull_request, **if lauseke** tulee olla jokaisen jobin alussa

```
on:
  pull_request:
    branches: ['master']
jobs:
  lint:
    if: ${{ github.event.pull_request.user.login != 'renovate[bot]' }}
    runs-on: ubuntu-latest
    ...
```
- feature branch, laita esimerkin **if lauseke** jokaisen jobin alkuun
```
on:
  push:
    branches-ignore: ['master']
jobs:
  lint:
    if: ${{ github.event.pusher.username != 'renovate[bot]' }}
    runs-on: ubuntu-latest
    ...
```
- Ylläolevissa taupauksissa ei haluttu triggeröidä työjonoa. Botti haluttiin kuitenkin pitää muistuttamassa pakettien uusista versioista.
- Ei kannata mahdollistaa puskemista mainiin kelle tahansa: Lisää branch protection rules, jos ei ole. Push oikeudet kannattaisi rajoittaa vain organisaation jäseniin. Projektitasolla Settings > General > Features voi kliksuttaa ![image](img/github_pull_req_restrict.png)

Näillä toimilla yritetään välttää haitallisen koodin eksymistä palvelimille ja toisaalta turhat actions ajot.
