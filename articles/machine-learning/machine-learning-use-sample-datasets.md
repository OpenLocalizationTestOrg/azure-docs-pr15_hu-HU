<properties
    pageTitle="A minta adatkészletek használata a gépi tanulási Studio |} Microsoft Azure"
    description="A minta modellekben Machine Learning Studio szereplő használt adatkészletek leírását. A minta adatkészletek a kísérletek is használhatja."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>A minta adatkészletek használata az Azure gépi tanulási Studio

[top]: #machine-learning-sample-datasets

Azure gépi tanulási az új munkaterület létrehozásakor alapértelmezés szerint látható egy minta adatkészletek és a kísérletek számát. A minta adatkészletek számos az [Azure Cortana üzletiintelligencia-gyűjtemény](http://gallery.cortanaintelligence.com/)a minta modellek által használt, és mások szerepelnek, példaként különféle típusú adatokra gépi tanulási felhasználási módjuk látható.

Néhány ezek adatkészletek érhetők el az Azure Blob-tárolóhoz. Az alábbi adatkészletek az alábbi táblázat nyújt közvetlen hivatkozást. Az [Adatok importálása] használatával a kísérletek a következő adatkészletek is használhatja[ import-data] modulra.

A következő mintát adatkészletek többi tartalmazó csoportban találhatók **Mentett adatkészleteket** a modul paletta bal oldalán a kísérlet vászon megnyitásakor, vagy hozzon létre egy új kísérlet Machine Learning Studio.
A is használhatja ezeket adatkészletek közül a saját kísérlet a kísérlet vászon húzásával.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Adatkészlet neve</th>
  <th align=left>Adatkészlet leírása</th>
</tr>

<tr>
  <td valign=top>Felnőtt népszámlálás bevétel bináris besorolás adatkészlet</td>
  <td valign=top>
Munka felnőttek használata 16 korát az > 100 korrigált jövedelem index 1994 népszámlálás adatbázis egy részét.<p> </p><b>Szintaxis:</b> demográfiai használatával előre, hogy egy személy szerez évente több mint 50K személyek sorolják be.<p> </p><b>Kapcsolódó kutatás:</b> Kohavi, j, Becker, b, (1996). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>A repülőterek kódok adatkészlet</td>
  <td valign=top>
Amerikai repülőterek kódok.<p> </p>Ez az adatkészlet tartalmaz egy sor minden amerikai repülőterek, airport szám és a neve mellett a hely várost és kezeléséről.
  </td>
</tr>

<tr>
  <td valign=top>Rakodótere ár adatok (nyers)</td>
  <td valign=top>
Által automobiles információt kezdeményezése és az ár, beleértve a modell szolgáltatások, például henger és MPG, valamint egy biztosítási kockázat pontszámhoz számát.<p> </p>A kockázat pontszám kezdetben társított automatikus ár, és a folyamat, mint symboling ellátó matematikusok ismert tényleges kockázat majd beállítani. + 3 értéket azt jelzi, hogy az automatikus kockázatos, és a 3 értéket, hogy azt valószínűleg igazán megbízható.<p> </p><b>Szintaxis:</b> a kockázat pontszám előrejelzésére funkciók, a regressziós vagy multivariate besorolás használatával. <p> </p><b>Kapcsolódó kutatás:</b> Schlimmer, J.C. (1987). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Kerékpárhoz bérleti UCI adatkészlet</td>
  <td valign=top>
UCI kerékpárhoz bérleti adatkészlet tőke Bikeshare vállalattól, amely megőrzi a Washington Adatközpont kerékpárhoz bérleti hálózaton tényleges adatok alapján.<p> </p>Az adatkészlet órát, összesen 17,379 sorok 2011 és 2012, a naponta egy sor rendelkezik. Az óránkénti kerékpárhoz bérleti díjak tartománya 1-től a 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB képe</td>
  <td valign=top>
Nyilvánosan hozzáférhető képfájl CSV-adatok alakulnak.<p> </p>A kép konvertálása kódját kapja a <strong>szín használata a K-eszközök fürtözés quantization</strong> modell részletei lapon.
  </td>
</tr>

<tr>
  <td valign=top>Vérnyomás adományozásra adatok</td>
  <td valign=top>
A VÉRTRANSZFÚZIÓS Service Center a Hsin-Chu város, Tajvan vérnyomás adományozók adatbázisa adatainak egy részét.<p> </p>Adományozók adatokat tartalmaz a hónap utolsó adományozásra óta), és a gyakoriság, vagy a teljes oldalszám adományok, az utolsó adományozásra óta eltelt idő, és adományok vérnyomás összege.<p> </p><b>Szintaxis:</b> a cél, előre besorolás keresztül, hogy az adományozók adományok vérnyomás március 2007, ahol 1 azt jelzi, egy adományozók során a cél időszak és a 0 nem adományozók. <p> </p><b>Kapcsolódó kutatás:</b> Yeh, úgy, (2008). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos <p> </p>Yeh, e-Cheng Yang, király-Jang, és gedélyezett, a hívás koppintson-Ming, "Tudásbázis feltárás csökkentett modellben Bernoulli sorozat, a" szakértő rendszerek alkalmazásokkal, 2008, <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>A címjegyzék véleményezések az Amazon</td>
  <td valign=top>
Az Amazon, a amazon.com webhelyről Pennsylvania és egyetemista verzió kutatók (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">sentiment</a>) által készített könyvek ismertetése. Lásd: a tanulmány "adatainak, Bollywood, gémhez-listák és Blenders: tartomány kiigazítás Sentiment osztályozási" János Blitzer, megjelölés Dredze és Fernando Pereira; A társítási számítási nyelvi elemek feldolgozása (vezérlés), a 2007-es.<p> </p>Az eredeti adatkészlet prioritású 1, 2, 3, 4-es vagy 5 975K ellenőrzések tartalmaz. A véleményezések angol nyelven írt és az időtartamot, 1997-2007. Ez az adatkészlet lefelé mintát a 10 KB felülvizsgálatokra volt.
  </td>
</tr>

<tr>
  <td valign=top>Mell rák adatok</td>
  <td valign=top>
A megjelenő gyakran gépi tanulási szakirodalomban Oncology intézmény által biztosított három rák kapcsolatos adatkészleteket közül. A névjegy 300 darab funkcióival laboratóriumi vizsgálat a diagnosztikai adatok egyesíti.<p> </p><b>Szintaxis:</b> rák típusú osztályozásához, 9 tulajdonságok alapján, amelynek egy része lineáris és kockák. <p> </p><b>Kapcsolódó kutatás:</b> Wohlberg, W.H. és utca, W.N. és Mangasarian, O.L. (1995). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Mell rák funkciók <td valign=top>
Az adatkészlet röntgensugaras képek 102K gyanús terület (jelöltek) információkat tartalmaz, minden egyes leírt 117 funkciók. A szolgáltatások védett, és azok jelentését foglalja össze rejtve maradnak az adatkészlet készítői (Siemens Healthcare). 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Mell rák információ</td>
  <td valign=top>
Az adatkészlet röntgensugaras kép az egyes gyanús régiókra vonatkozó további információt tartalmaz. Minden egyes példa bemutatja (például címke betegek azonosítója, a teljes képet viszonyított javítás koordinátáit) az mell rák szolgáltatások adathalmazban megfelelő sor számát. Egyes betegek példák számos rendelkezik. Az igénybe vevő pácienseknek, akiknek van egy rák néhány példa pozitív és negatív. Betegek számára, akik nem rendelkeznek a rák, az összes példák negatív. Az adatkészlet 102K példákat tartalmaz. Az adatkészlet torzított, a címzett pontok 0,6 %-os pozitívak, a többi negatív. Az adatkészlet lett elérhetővé tett Siemens Healthcare.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>A megosztott CRM Appetency címkék</td>
  <td valign=top>
Címkék a KDD tölteni 2009-es ügyfél kapcsolati előrejelzési kérdés (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>A megosztott CRM tejeskanna címkék</td>
  <td valign=top>
Címkék a KDD tölteni 2009-es ügyfél kapcsolati előrejelzési kérdés (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>A megosztott CRM adatkészlet</td>
  <td valign=top>
Ezeket az adatokat a KDD tölteni 2009-es ügyfél kapcsolati előrejelzési beavatkozás igazolására szolgáló eljárás (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>) származik.<p> </p>Az adatkészlet 50K ügyfelek narancssárga francia távközlési vállalattól tartalmazza. Ügyfelek rendelkezik 230 anonimizált funkciók, amelyek 190 numerikus és 40 kockák. A szolgáltatások nagyon ritka.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>A megosztott CRM Upselling címkék</td>
  <td valign=top>
Címkék a KDD tölteni 2009-es ügyfél kapcsolati előrejelzési kérdés (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energy hatékonyság regressziós adatok</td>
  <td valign=top>
Szimulált energy profilokat, 12 különböző épület alakzatok alapján gyűjteménye. Az épület eltérő alábbiakra 8 funkciókat, például a területet, üveg terület terjesztési és a tájolás üveg termékenként.<p> </p><b>Szintaxis:</b> használja a regressziós vagy besorolás előrejelzésére minősítés alapján két tényleges értéket tartalmazó válaszok egyikeként energy hatékonyságát. Több elem osztály osztályozásához kerekítés a legközelebbi egész számra a válasz változó van. <p> </p><b>Kapcsolódó kutatás:</b> Xifara, válaszokhoz és Tsanas, válaszokhoz (2012). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Nézetbeli késlelteti adatok</td>
  <td valign=top>
Személy nézetbeli a pontos teljesítményadatokat az Amerikai Egyesült Államok Közlekedési Minisztérium (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">A pontos</a>) TranStats adatgyűjtési származik.<p> </p>Az adatkészlet bemutatja az időtartamot április-október 2013-ban. Azure Machine Learning Studio feltöltését, mielőtt az adatkészlet volt a következőképpen dolgozza fel:<ul><li>Az adatkészlet lett szűrve, hogy lefedje a a kontinentális USA-beli csak 70 leginkább foglalt repülőterek</li><li>A visszavont repülőjegyek voltak szerint 15 percnél hosszabb ideig késő címkézett</li><li>Elterelt repülőjegyek kiszűrte</li><li>Az alábbi oszlopok kiválasztásának: év, hónap, DayofMonth, DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15 és visszavonva</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Nézetbeli a pontos teljesítmény (nyers)</td>
  <td valign=top>
A rekordok az repülőgép nézetbeli érkezés és távozás: október 2011 az Egyesült Államokból.<p> </p><b>Szintaxis:</b> nézetbeli előrejelzésére. <p> </p><b>Kapcsolódó kutatás:</b> az Amerikai Egyesült Államok részleg, pihenőhelyek <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Erdő akkor indul el az adatok</td>
  <td valign=top>
Időjárási adatokat, például hőmérséklet és páratartalom indexek és északkeleti Portugália erdőtüzekkel rekordok kombinálva egy területről, szélsebesség tartalmaz.<p> </p><b>Szintaxis:</b> Ez a regressziós túl bonyolult feladat, ahol a cél erdőtüzek írott területe előrejelzésére. <p> </p><b>Kapcsolódó kutatás:</b> Cortez, P., és Morais, válaszokhoz (2008). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos <p> </p>[Cortez és Morais, 2007-es] O. Cortez és válaszokhoz Morais. A adatok Mining megközelítés adatokkal időjárási előrejelzési erdőtüzekkel. A J. Neves, M. f Santos és J. Machado Eds., mesterséges intelligencia, eljárás a 13 EPIA 2007 - portugál konferencia mesterséges intelligencia, December Guimarães, Portugália, oldal 512-523, 2007 új trendeket. APPIA, ISBN-13-AS 978-989-95618-0 – 9. A: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Német hitelkártya UCI adatkészlet</td>
  <td valign=top>
A UCI Statlog (német hitelkártya) adatkészlet (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + német + hitelképesség + adatok</a>), a german.data fájl használatával.<p> </p>Az adatkészlet sorolja a személyek által attribútumok, alacsony vagy magas hitelképesség-kockázatok leírt. Minden egyes például egy személy jelöli. Vannak 20 funkcióiról, numerikus és a kockák, és a bináris címkébe (a hitelkártya kockázat érték-). Ha magas követel kockázat tételek = 2, ha alacsony követel kockázat tételek = 1. A magas, alacsony kockázat vett példa misclassifying költségét értéke 1, mivel a lehető legalacsonyabb a magas kockázatú példa misclassifying költségét az 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB Movie címe?</td>
  <td valign=top>
Az adatkészlet tartalmaz, a Twitteren twitterre voltak minősítette filmek információt: IMDB film-azonosító film nevét és a műfaj, az év. Az adatkészlet 17K filmek szerepelnek. A papír "s a jelent meg az adatkészlet Dooms, t, De Pessemier és L. Martens-féle. MovieTweetings: Twitter gyűjtött adatkészlet minősítés film. Workshop Crowdsourcing és Recommender rendszerek esetében a 2013-ra RecSys CrowdRec emberi kiszámítása."
  </td>
</tr>

<tr>
  <td valign=top>Két Iris osztály adatokat</td>
  <td valign=top>
Az esetleg a legjobb ismert adatbázis a minta felismerés szakirodalomban találhatók. Az adathalmaz viszonylag kis 50 példák minden három iris fajták szirom mérték tartalmazó.<p> </p><b>Szintaxis:</b> találnia a méreteket iris típust.  <p> </p><b>Kapcsolódó kutatás:</b> Fisher, R.A. (1988). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Mozgókép Twitterre</td>
  <td valign=top>
Az adatkészlet a film Tweetings adatkészlet kibővített változatát. Az adatkészlet filmek, a Twitteren jól strukturált twitterre kiolvasott 170K minősítése tartalmaz. Egyes példányok egy tweetbe helyén, és egy sor bármelyik eleme: felhasználói azonosító, IMDB film azonosítója, minősítés, időbélyeg, e tweetbe, illetve a retweets e tweetbe a számát a Kedvencek számának. Az adatkészlet lett elérhetővé tett válaszokhoz Said, s Dooms a, b Loni és d Tikk Recommender rendszerek beavatkozás igazolására szolgáló eljárás 2014-es számára.
  </td>
</tr>

<tr>
  <td valign=top>Különböző Gépkocsikhoz MPG adatok</td>
  <td valign=top>
Ez az adatkészlet az adathalmaz Carnegie Mellon University StatLib gyűjteményének által biztosított is módosultak kissé verziója is. Az adatkészlet használta a 1983 amerikai statisztikai társítási részletes ismertetése.<p> </p>Az adatok üzemanyag-fogyasztási km / gallon együtt az adatokat a számot, henger, motor elmozdulás, lóerő, súly összesen és gyorsítás a különböző Gépkocsikhoz sorolja fel.<p> </p><b>Szintaxis:</b> előrejelzésére üzemanyag-fogyasztási 3 többértékű különálló attribútumok és 5 folyamatos attribútumok alapján. <p> </p><b>Kapcsolódó kutatás:</b> StatLib, Carnegie Mellon University (1993). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr>
  <td valign=top>Adatkészlet Pima indiai termelőktől cukorbetegség bináris besorolás</td>
  <td valign=top>
A cukorbetegség nemzeti intézmény és emésztőtraktus és vese betegség adatbázis adatainak egy részét. Az adatkészlet lett szűrt Pima indiai örökség női betegek koncentrálhat. Az adatok például glükóz és inulin szintek, valamint életmód tényezők orvosi adatait tartalmazza.<p> </p><b>Szintaxis:</b> előrejelzésére, hogy van-e cukorbetegség (bináris osztályozás) az alany. <p> </p><b>Kapcsolódó kutatás:</b> Sigillito kontra (1990). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos </td>
</tr>

<tr>
  <td valign=top>Éttermi ügyféladatok</td>
  <td valign=top>
Ügyfelek, beleértve a demográfiai és beállítások metaadatait csoportja.<p> </p><b>Szintaxis:</b> együtt a másik két adatok étterem, ez az adatkészlet segítségével képzése és egy recommender rendszer tesztelése. <p> </p><b>Kapcsolódó kutatás:</b> Bache, k és Lichman, M. (2013). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, iskolai az adatok és számítógép tudományos.
  </td>
</tr>

<tr>
  <td valign=top>Éttermi szolgáltatás adatok</td>
  <td valign=top>
Metaadat-alapú éttermek és a szolgáltatások, például a élelmiszer típusa, ebédlő stílus és a hely csoportja.<p> </p><b>Szintaxis:</b> együtt a másik két adatok étterem, ez az adatkészlet segítségével képzése és egy recommender rendszer tesztelése. <p> </p><b>Kapcsolódó kutatás:</b> Bache, k és Lichman, M. (2013). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, iskolai az adatok és számítógép tudományos.
  </td>
</tr>

<tr>
  <td valign=top>Éttermi minősítése</td>
  <td valign=top>
Minősítések képlettel éttermek felhasználói szintű 0-tól 2 tartalmazza.<p> </p><b>Szintaxis:</b> együtt a másik két adatok étterem, ez az adatkészlet segítségével képzése és egy recommender rendszer tesztelése. <p> </p><b>Kapcsolódó kutatás:</b> Bache, k és Lichman, M. (2013). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, iskolai az adatok és számítógép tudományos.
  </td>
</tr>

<tr>
  <td valign=top>Készült Annealing több osztály adatkészlet</td>
  <td valign=top>
Ez az adatkészlet tartalmaz (szélességét, vastagságát, típusú (sodrású, lap stb.) az eredményül kapott készült típusok a fizikai attribútumokkal rendelkező kísérletek primerek acél rekordjaival sorozata.<p> </p><b>Szintaxis:</b> előrejelzésére két numerikus osztály attribútumokat; keménység vagy erőssége. Előfordulhat, hogy is elemezheti az összefüggések attribútumok között.<p> </p>Készült jegyeit, hajtsa végre a beállítása szabvány SAE és más szervezetek határozza meg. Egy adott "kategória" (az osztályjegyzetfüzet változó) keres, és szeretné érteni a szükséges értékeket. <p> </p><b>Kapcsolódó kutatás:</b> Sterling, d & Buntine w (hiányzik). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, a iskolai az adatok és a számítógép tudományos <p> </p>Egy hasznos útmutató készült jegyeit megtalálható itt: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Teleszkóppal adatok</td>
  <td valign=top>
Magas energy gamma vesszenek felszakadásáig együtt háttérzajt rekordokat, mindkét szimulált Monte Carlo eljárással.<p> </p>A szimulációk szándékának pontosságának alapokról-alapú légköri Cherenkov gamma teleszkópok, statisztikai módszerek segítségével a kívánt jel (Cherenkov sugárzás zuhanyozókra) és a háttérzajt (hadronic zuhanyozókra kezdeményező: a felső légköri cosmic sugarakkal) közötti különbségek javítása volt.<p> </p>Az adatok előre feldolgozott Várjon egy nyújtott alakú fürthöz létrehozását a hosszú tengely a kamera központ felé lépések. A ellipszis (más néven Hillas paraméterek) jellemzői a kép paraméterek megkülönböztetés használható közé tartoznak.<p> </p><b>Szintaxis:</b> előrejelzésére, hogy mulatni képe jelöl jel vagy háttér zajt.<p> </p><b>Megjegyzések:</b> egyszerű besorolás pontosság nincs jelentősége, akkor ezeket az adatokat az osztályozásához a háttérben eseményeket, mint jel rosszabb, mint a jel esemény háttérként osztályozásához óta. Különböző osztályozónak összehasonlítása az ROC graph kell használni. Annak valószínűsége, a háttérben esemény fogad, mint jel alatti végre a következő küszöbértékek kell lennie: 0,01, 0,02, 0,05, 0,1 vagy 0,2.<p> </p>Megjegyzés:, hogy a háttér események (hadronic zuhanyozókra h) száma alábecsülték, mivel a tényleges mértékek, a h vagy zajt osztály jelöl események többségét. <p> </p><b>Kapcsolódó kutatás:</b> Bock, R.K. (1995). UCI gépi tanulási a tárházba <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, hitelesítésszolgáltató: Egyetemi a kaliforniai, iskolai vonatkozó információk </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Időjárási adatkészlet</td>
  <td valign=top>
Szárazföldi időjárási észrevételek óránkénti a NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">201304 201310 az egyesített adatok</a>).<p> </p>Az időjárási adatok airport időjárási állomások, az időtartamot április-október 2013 kiterjedő készült észrevételek foglalkozik. Azure Machine Learning Studio feltöltését, mielőtt az adatkészlet volt a következőképpen dolgozza fel:<ul><li>Időjárási állomáson azonosítók megfelelő airport azonosítók voltak megfeleltetve</li><li>Nem társított 70 leginkább foglalt repülőterek időjárási állomások kiszűrte</li><li>A dátum oszlopból lett külön évet, hónapot és napot oszlopra osztja fel</li><li>Az alábbi oszlopok kiválasztásának: AirportID, év, hónap, nap, idő, időzóna, SkyCondition, láthatóságát, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, szélsebesség, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, magasságmérő</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipédia SP 500 adatkészlet</td>
  <td valign=top>
Adatok származik Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) cikkek S & P 500 vállalatok, tárolja az XML-adatok alapján.<p> </p>Azure Machine Learning Studio feltöltését, mielőtt az adatkészlet volt a következőképpen dolgozza fel:<ul><li>Bontsa ki a szöveges tartalomba minden adott vállalathoz</li><li>Wiki formázás eltávolítása</li><li>Nem alfanumerikus karaktereket eltávolítása</li><li>Az összes szöveg átalakítása kisbetűssé</li><li>Ismert cég kategóriák lettek hozzáadva</li></ul><p> </p>Ne feledje, hogy egyes vállalatok a cikk nem található, ezért a rekordok számát kisebb, mint 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Az adatkészlet tartalmazza az ügyfél adatait, és kapcsolatos jelzést egy közvetlen levelezési marketingkampány válaszát. Minden egyes sor egy ügyfél jelöli. Az adatkészlet a 9-es elemeket felhasználói demográfiai és korábbi működését és a 3 címke oszlop tartalmaz (keresse fel az átalakítás és töltött).  Olvassa el az egy bináris oszlopot, amely jelzi, hogy egy ügyfél után a marketingkampány felkeresett, átalakítás azt jelzi, egy ügyfél vásárolt egy üzenetet, és töltött töltött összege.  Az adatkészlet lett elérhetővé tett Kevin Hillstrom MineThatData E-Mail Analytics és adatok Mining beavatkozás igazolására szolgáló eljárás számára.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Teszt példák az RCV1-V2 Reuters hírek adathalmazban szolgáltatásait. Az adatkészlet 781K hírek együtt azonosítójuk van (az adatkészlet első oszlopa). Minden egyes cikk tokenekre bontott stopworded, és kocsány. Az adatkészlet lett elérhetővé tett David. D Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Példák az RCV1-V2 Reuters hírek adathalmazban funkciói. Az adatkészlet 23K hírek együtt azonosítójuk van (az adatkészlet első oszlopa). Minden egyes cikk tokenekre bontott stopworded, és kocsány. Az adatkészlet lett elérhetővé tett David. D Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Adatkészlet: a KDD tölteni 1999 Tudásbázis feltárás és az adatok bányászati eszközök versenyhelyzetből (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Az adatkészlet letöltött (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) Azure Blob-tárolóban lévő és képzés és adatkészleteket tesztelése is tartalmaz. Az oktatóanyag adatkészlet körülbelül van 126K a sorok és 43 oszlopok, beleértve a feliratokat. 3 oszlopból a felirat adatainak részét, illetve 40 oszlopok, szám-karakterlánc/kockák szolgáltatások álló tanfolyamok a modell érhető el. A vizsgált adatokat tartalmaz a megközelítőleg azonos 43 oszlopokat, ahogy az oktatóanyag adatok példák tesztelése 22.5K.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1-v2.topics.qrels.csv</a></td>
  <td valign=top>
A témakör-hozzárendelések az RCV1-V2 Reuters hírek adatkészlet hír cikkeket. Több témakörök hír tevékenységekhez hozzárendelhető. Minden egyes sorára formátuma "<topic name>  <document id> 1 cm-es. Az adatkészlet 2.6M témakör hozzárendeléseket tartalmazza. Az adatkészlet lett elérhetővé tett David. D Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Ezeket az adatokat a KDD tölteni 2010 diák teljesítmény kiértékelés beavatkozás igazolására szolgáló eljárás (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Diák teljesítmény kiértékelés</a>) származik. A használt adata Algebra_2008_2009 oktatás megadása (Stamper, J., Niculescu-Mizil, válaszokhoz, Ritter, s, Gordon, G.J. és Koedinger, k. r. (2010). Algebra 2008-2009-e. Kérdés KDD tölteni 2010 oktatási adatok Mining beavatkozás igazolására szolgáló eljárás adatkészletet. <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">Downloads.jsp</a> vagy <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>találja meg.<p> </p>Az adatkészlet letöltött (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) Azure Blob-tárolóban lévő és a rendszer-oktatás student naplófájlok tartalmazza. A megadott szolgáltatásai probléma azonosítója és rövid leírását, diákok azonosítója, időbélyeg és hány kísérletek a diák, a probléma megoldását a megfelelő módon előtt. Az eredeti adatkészlet 8.9M rekordjait, akkor ez az adatkészlet volt az első 100 KB sorok lefelé mintát. Az adatkészlet különböző típusú 23, tabulátorral tagolt oszlopot tartalmaz: kockák, numerikus és időbélyeg.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
