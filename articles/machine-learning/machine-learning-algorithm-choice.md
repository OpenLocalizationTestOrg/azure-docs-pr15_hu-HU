<properties
    pageTitle="Gépi tanulási algoritmusok kiválasztásáról |}] A Microsoft Azure"
    description="A felügyelt és felügyelet nélküli tanulás Azure gépi tanulási algoritmusok fürtözés, a minősítés vagy regressziós kísérletekben kiválasztásának módjáról."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Hogyan lehet a Microsoft Azure gépi tanulási algoritmusok kiválasztása

A "Milyen tanulási algoritmust kell használni gép?" kérdésre a válasz mindig "Attól függ." Ez függ a méret, minőség és adatok jellegének. Attól függ, mit szeretne tenni a választ. Ez attól függ, hogy hogyan a matematikai algoritmus lett lefordítva utasítások a számítógéphez. Azt határozza meg, és hogy mennyi idő van. Még a leginkább tapasztalt adatok tudósok algoritmus fogja végrehajtani a legjobb előtt azokat nem lehet megállapítani.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>A gép tanuló algoritmus Cheat lap

A **Microsoft Azure gép tanulás algoritmus Cheat lap** segítségével válassza ki a megfelelő gép tanulás algoritmusokat a Microsoft Azure gép tanulás könyvtárból a prediktív analytics megoldások algoritmus.
Ez a cikk végigvezeti annak használatát.

> [AZURE.NOTE] A csal lap letöltése, és kövesse a cikk és menjen [tanulás algoritmus csal lap Microsoft Azure gép tanulás Studio gép](machine-learning-algorithm-cheat-sheet.md).

Csal lap van egy nagyon különleges közönség szem előtt: a kezdő adatok kutató undergraduate szintű gépi tanulás, és próbál kezdeni Azure gép tanulás Studio algoritmus válassza. Ez azt jelenti, hogy néhány általánosítást és oversimplifications teszi, de ez fog mutatni, biztonságos irányba. Azt is jelenti, hogy az számos itt fel nem sorolt algoritmusok. Azure gép tanulás növekedésével magában foglaló rendelkezésre álló módszerek több teljes körű azokat beírjuk.

Ezek az ajánlások olyan lefordított visszajelzés és a tippek a nagy mennyiségű adat tudósok és gépi tanulás szakértők. Azt nem állapodnak meg mindent, de a vélemények összehangolása a nyers konszenzust már megpróbáltam. A legtöbb nézeteltérés kimutatásainak kezdődik "Annak függvénye..."

### <a name="how-to-use-the-cheat-sheet"></a>A csal lap használata

Az elérési utat és algoritmus feliratokat a diagramon, olvassa el "a * &lt;elérési út címke&gt; * használja * &lt;algoritmus&gt;*." Például *"sebesség* használata *két osztály logisztikus regresszió*." Bizonyos esetekben egynél több ágra vonatkoznak.
Néha ezek közül egyik sem lesz tökéletes. Azok vagyunk szolgál szabály a görgetőgomb ajánlások, így nem kell aggódnia, hogy pontos.
Több adat tudósok I nyújthassunk az említett, a csak meg arról, hogy keresse meg a legjobb algoritmus módja próbálja meg azokat.

Egy kísérlet, amely megpróbál ellen ugyanazokat az adatokat több algoritmusok és eredményeinek összevetése a [Cortana intelligencia galéria](http://gallery.cortanaintelligence.com/) példa: [több osztály osztályozó összehasonlítása: felismerés levél](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Letöltése és nyomtatása egy ábra, amely a gép tanulás Studio lehetőségeinek áttekintése, lásd: [áttekintő ábrája Azure gép tanulás Studio szolgáltatásokat](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Gépi tanulás változatok

### <a name="supervised"></a>Ellenőrzött

Felügyelt tanulási algoritmusok példák alapján előrejelzéseket készíthet. Például részvényárfolyamokkal történelmi veszély Találgatások jövőbeni áron használható. Minden képzési használt példa célállapotba kamat értéke – ebben az esetben a tőzsdei árat. A felügyelt tanuló algoritmus e Értékcímkék mintákat keres. Használhatja bármely információt, amely lényeges lehet – a nap a hét, a szezon, a vállalat pénzügyi adatait, ipari tevékenység típusa, a zavaró geopolicitical események jelenléte – és minden algoritmus különböző mintázatok keres. Az algoritmus talált a legjobb minta után azt is, használja a minta címke nélküli vizsgálati adatok előrejelzéseket készíthet – holnapi árak.

Ez a gép tanulás egy olyan népszerű és hasznos. Egy kivétellel minden az Azure gép tanulási modulok felügyelete tanulási algoritmusok. Többféle különleges ellenőrzött tanulás, Azure gép tanulás belül jelennek meg: osztályozás, regressziós és rendellenességet kimutatására.

* **Osztályozás**. Az adatok kategória előre használatban van, ha ellenőrzött tanulás rövidítése osztályozás. Ez a helyzet a "macska" vagy a "kutya" képként kép hozzárendelése során. Ha csak két választási lehetőség, ezt **két osztály** vagy a **binomiális osztályozás**nevezik. Ha több kategóriába sorolhatók, mint a NCAA március Madness sportverseny nyertese előrejelzésénél, ez a probléma **több osztály besorolás**néven ismert.

* **Regressziós**. Érték alatt szolgáltassák, mint a tőzsdei árakhoz, ellenőrzött tanulás regressziós neve.

* **Anomáliák érzékelésére**. Néha a cél az, hogy egyszerűen szokatlan adatpontok azonosítása. A csalások felderítésével például bármely igen ritka hitelkártya kiadások minták gyanúsak. A lehetséges változatok így számos, és az így néhány, vagyis nem megvalósítható, hogy megtudja, milyen csalárd tevékenység példák néz. A megközelítés, amely anomáliák érzékelésére egyszerűen megtudhatja, milyen szokásos tevékenység néz ki (nem csalárd előzménytranzakciókat használatával), és mindent, ami jelentősen eltérő azonosításához.

### <a name="unsupervised"></a>Felügyelet

A felügyelet nélküli tanulás adatpontok nincsenek hozzájuk rendelt címkék rendelkezik. Ehelyett egy felügyelet nélküli tanuló algoritmus célja valamilyen módon az adatok szervezése, illetve a szerkezetét írják le. Ez jelentheti a fürtök a csoport vagy megnézzük az összetett adatokat, hogy úgy tűnik, egyszerűbb vagy több szervezett különböző módok keresése.

### <a name="reinforcement-learning"></a>Tanulás megerősítése

A tanulás megerősítése az algoritmus kívánt műveletre az egyes adatpont válaszul kapja. A tanuló algoritmus is kap egy jutalom jel rövid idő későbbi, jelezve, hogy jó döntés volt.
Ennek alapján az algoritmus módosítása a stratégia a legmagasabb ellenszolgáltatás elérése érdekében. Jelenleg nincs tanulás algoritmus Azure gép tanulási modulok megerősítése. Tanulás megerősítése esetén gyakori, robotics, ahol egy adatpont érzékelő mérési egy pontján időben lesz, és az algoritmust kell választania a következő művelet a robot. Továbbá természetes dolog internetes alkalmazások alkalmasak.

## <a name="considerations-when-choosing-an-algorithm"></a>Szempontok az algoritmus kiválasztása

### <a name="accuracy"></a>Pontosság

Bevezetés a legpontosabb válasz lehetséges nem mindig szükséges.
Néha közelítő számára, attól függően, hogy milyen kíván használni. Ha ez a helyzet, lehet kivágni a feldolgozási idő jelentősen több közelítő módszert az elvéreztetés. További közelítő módszerek egy másik előnye, hogy természetesen általában [overfitting](https://youtu.be/DQWI1kvmwRg)elkerülése érdekében.

### <a name="training-time"></a>Képzési idő

A perc vagy óra szükséges a vonat modell száma nagy visszhangot algoritmusok között változik. Képzési idő pontossága gyakran szorosan kötődik – egyet általában kíséri, a másik. Ezenkívül egyes algoritmusok érzékenyebbek az adatpontok száma mint mások.
Korlátozott idő esetén azt is meghajtó algoritmus választását, különösen akkor, ha az adatkészlet nagy.

### <a name="linearity"></a>Lineáris

Gépi tanulási algoritmusok rengeteg legyen linearitás használja. Lineáris osztályozás algoritmusok feltételezik, hogy osztályokat lehet elválasztani egyenes (vagy a magasabb dimenziós analóg). Ezek közé tartozik a logisztikus regresszió és vektoros gépek támogatása (megfelelően végrehajtott Azure gép tanulás).
Lineáris regressziós algoritmusok feltételezik adatok trendje kövesse az egyenes vonal. Ezek a feltételezések nem rossz bizonyos problémák, de másokra léptetnek pontosságát.

![Nem lineáris osztály bounday][1]

***Nem lineáris osztály határ*** *-a lineáris besorolási algoritmus támaszkodva eredményezne, alacsony pontosság*

![Adatokat és nem lineáris trend][2]

***Adatokat és nem lineáris trend*** *-egy lineáris regressziós módszer eredményezne a szükségesnél sokkal nagyobb hibák*

A veszélyek ellenére lineáris algoritmusok nagyon népszerű első sorban a támadás. Ezek általában algorithmically egyszerű és gyors, a vonat.

### <a name="number-of-parameters"></a>A paraméterek száma

Paraméterek a kezelőgombjainak kapcsolja egy algoritmus beállítása során kap egy adatok tudósok is. Számok, amelyek hatással vannak az algoritmus viselkedését, például a hibatűrés vagy közelítés vagy beállítások között az algoritmus működése változatai. A képzési idő és az algoritmus pontossága néha lehet meglehetősen érzékeny kap a megfelelő beállításokat. Sok paraméterekkel algoritmusok általában a legtöbb próba és hiba található egy jó kombináció szükséges.

Azt is megteheti nincs [paraméter abszolút](machine-learning-algorithm-parameters-optimize.md) modul blokk Azure gépi tanulás, amely automatikusan bármilyen részletesség választja, minden paraméter kombináció. Bár egy nagyszerű mód arra, hogy ellenőrizze, hogy a paraméterrel adhatja meg már átnyúló, vonat modell szükséges idő exponenciálisan paraméterek számát növeli.

A fejjel, hogy sok paraméter általában tekintettel azt jelzi, hogy egy algoritmus nagyobb rugalmasságot. Nagyon jó pontosság azt gyakran érhet el. Meghatározott paraméter beállítások megfelelő kombinációját is megtalálhatja.

### <a name="number-of-features"></a>Szolgáltatások száma

Bizonyos típusú adatok, szolgáltatások száma nagyon nagy lehet képest adatpontok számát. Ez gyakran a helyzet genetics vagy szöveges adatok. A szolgáltatások nagy száma is bog le néhány tanulási algoritmusok, így a képzési idő a hosszú unfeasibly. Vektoros gépek támogatása különösen alkalmasak ebben az esetben (lásd alább).

### <a name="special-cases"></a>Különleges esetek

Egyes tanulási algoritmusok ellenőrizze az adatok vagy a kívánt eredményeket szerkezete adott feltételezéseket. Ha talál egy, az igényeinek leginkább megfelelő, azt tudhatja meg a leghasznosabb eredmények, pontosabb előrejelzéseket vagy gyorsabb képzési idő.

|**Algoritmus**|**Pontosság**|**Képzési idő**|**Lineáris**|**Paraméterek**|**Jegyzetek**|
|---|:---:|:---:|:---:|:---:|---|
|**Két osztályú besorolás**| | | | | |
|[logisztikus regresszió](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[határozat erdő](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[határozat Dzsungel](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Kevés a memória-helyigénye|
|[csillapítja döntési fa](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Nagy memória-helyigénye|
|[Neurális hálózati](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[átlagolt perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[vektoros gép támogatja.](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Jó nagy szolgáltatás-készletek|
|[helyileg mély support vector gép](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Jó nagy szolgáltatás-készletek|
|[Bayes' pont gép](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Több osztály besorolás**| | | | | |
|[logisztikus regresszió](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[határozat erdő](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[határozat Dzsungel](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Kevés a memória-helyigénye|
|[Neurális hálózati](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[egy-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|A kiválasztott két osztálymetódus tulajdonságok|
|**Regressziós**| | | | | |
|[lineáris](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Lineáris Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[határozat erdő](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[csillapítja döntési fa](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Nagy memória-helyigénye|
|[gyors erdő ki osztóérték](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Pont előrejelzéseket, hanem felosztások|
|[Neurális hálózati](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[További testreszabási lehetőség.](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Műszakilag log-lineáris. Számlálás előrejelzéséhez.|
|[sorszám](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Sorszám sorrendje előrejelzéséhez|
|**Anomáliák érzékelésére**| | | | | |
|[vektoros gép támogatja.](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Különösen hasznos a nagy szolgáltatás beállítása|
|[PEM alapú anomáliák érzékelésére](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-eszközök](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|A fürtözés algoritmus|


**Algoritmus tulajdonságai:**

**●** - kiváló pontossága, gyors képzési időpontok és lineáris használatát mutatja

**○** - jó pontosságát és mérsékelt képzési idő megjelenítése

## <a name="algorithm-notes"></a>Algoritmus jegyzetek

### <a name="linear-regression"></a>Lineáris regresszió

Ahogy korábban említettük, az [lineáris regressziós](https://msdn.microsoft.com/library/azure/dn905978.aspx) elfér egy sor (vagy a sík, vagy hyperplane) az adatkészlet. A workhorse, egyszerű és gyors, de lehet, hogy túlságosan simplistic a problémák.
Jelölje be a [lineáris regressziós oktatóprogram](machine-learning-linear-regression-in-azure.md).

![Adatok lineáris trend][3]

***Adatok lineáris trend***

### <a name="logistic-regression"></a>Logisztikus regresszió

Bár a neve "regressziós" megtévesztően tartalmaz, a logisztikus regresszió ténylegesen [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) és a [két-osztályú](https://msdn.microsoft.com/library/azure/dn905994.aspx) besorolás hatékony eszköz. Gyors és egyszerű. A tény, hogy használ s "-alakú görbe helyett egyenes lehetővé teszi, hogy természetes alkalmasnak adatok csoportokba kell osztani. Logisztikus regresszió ad lineáris osztály határait, így használatakor, ellenőrizze, hogy lineáris közelítéséről valamit is él, a.

![Csak egyetlen szolgáltatás két osztályú adatok logisztikus regresszió][4]

***Két osztályadatok csak egy szolgáltatás egy logisztikus regresszió*** *-az osztály határ az a pont, ahol egy logisztikus görbét csak olyan közel mindkét osztály*

### <a name="trees-forests-and-jungles"></a>Fák, erdők és jungles

Határozat erdők ([regressziós](https://msdn.microsoft.com/library/azure/dn905862.aspx), [két osztályú](https://msdn.microsoft.com/library/azure/dn906008.aspx)és [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), a határozat jungles ([két-osztály](https://msdn.microsoft.com/library/azure/dn905976.aspx) - és [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) és csillapítja döntési fák ([regressziós](https://msdn.microsoft.com/library/azure/dn905801.aspx) és [két osztályú](https://msdn.microsoft.com/library/azure/dn906025.aspx)) alapulnak döntési fák tanulás fogalma eligazodást gép. Döntési fák sok változata van, de mindegyiknek ugyanazt – a szolgáltatás terület olyan régiókban, többnyire azonos címkékből tovább. Ezek lehetnek régiók egységes kategória vagy állandót, attól függően, hogy e besorolás vagy regressziós végzi.

![Döntési fa bázisterületeit adható meg a szolgáltatás][5]

***A döntési fa bázisterületeit a szolgáltatás terület nagyjából egységes értékek területekre***

Szolgáltatás adható önkényesen kis területekre lehet osztani, mert sokkal egyszerűbb elképzelni, osztás finoman elég ahhoz, hogy egy adatpont régiónként – overfitting szélsőséges példát. Ennek elkerülése érdekében nagy számú fák vannak épített hozott különleges matematikai óvatosan a fák nem közötti kapcsolatot. A "határozat"erdő átlagos egy fa, amely nem overfitting. Határozat az erdők nagy mennyiségű memóriát használhat. Határozat jungles egy változata, amely kissé hosszabb képzési időt rovására kevesebb memóriát fogyaszt.

Csillapítja döntési fák ne overfitting tovább lehet, hogy hány alkalommal és milyen néhány adatpont engedélyezettek, az egyes régiókban korlátozásával. Az algoritmus sorozatát, amelyek kompenzálják a hiba előtt a fa által hagyott megtanulja fák, szerkezetek. Eredménye egy nagyon pontos tanuló, ami megváltoztatná a nagy memóriát használja. A részletes műszaki leírás kivétele [Friedman's eredeti papír](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Gyors erdő ki osztóérték regressziós](https://msdn.microsoft.com/library/azure/dn913093.aspx) döntési fák, ahol meg szeretné ismerni a nem csak a tipikus (középső) értéket az a régió, hanem a terjesztési quantiles formájában lévő adatokhoz különleges esetére változata.

### <a name="neural-networks-and-perceptrons"></a>Neurális hálózatok és perceptrons

Neurális hálózatok olyan, agy-ösztönző tanulási algoritmusok kiterjedő [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), a [két osztály](https://msdn.microsoft.com/library/azure/dn905947.aspx)és a [regressziós](https://msdn.microsoft.com/library/azure/dn905924.aspx) problémákat. Egy végtelen különféle származnak, de belül Azure gép tanulás Neurális hálózatok összes irányított aciklikus diagramok formájában. Ez azt jelenti, hogy beviteli funkciók használatához a kérelemben előre (soha ne visszafelé) rétegek sorozata előtt a kimenetek kapcsolva. A két réteg felhasználandó anyagok különböző kombinációival súlyozott, összesítve, és átadni a következő réteg. Ez a kombináció egyszerű számítások kifinomult osztály szegélyek és adatok trendek, látszólag információ: mágikus képessége eredményez. Az ilyen jellegű több réteges hálózatok a "mély tanulási", amely sok technikai jelentések és fantasztikus üzemanyag hajtható végre.

A nagy teljesítményű nem tartalmaz szabad, bár. Neurális hálózatok, különösen a szolgáltatások rengeteg nagy adatkészletekkel vonat hosszú időt vesz igénybe. További paraméterek, mint a legtöbb algoritmusok, ami azt jelenti, hogy a paraméter abszolút bővíti a képzési idő nagy visszhangot is rendelkeznek.
És azon overachievers, aki szeretné [megadni, a saját hálózati struktúra](http://go.microsoft.com/fwlink/?LinkId=402867), a lehetőségek nem inexhaustible.

<a name="boundaries-learned-by-neural-networks6"></a>![Neurális hálózatok által megtanult határok][6]
---------------------------

***Neurális hálózatok által megtanult határainak lehet bonyolult és szabálytalan***

[Két osztály átlagolt perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) skyrocketing képzési idő Neurális hálózatok válasz. Lineáris osztály határok biztosít hálózati szerkezetet használ. Mai szabványok által szinte primitív, de már hosszú ideje dolgozik az abroncsnyomásmérők erős, és elég kicsi ahhoz, hogy megtudja, gyorsan.

### <a name="svms"></a>SVMs

Support vector gépek (SVMs) található a határ, amely elválasztja egymástól az osztályok által, a lehető legszélesebb margó. Ha nem egyértelműen szét a két osztály, az algoritmusok azok is a legjobb határ találja meg. Azure gép tanulás írt, mint a [két-osztályú SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) nem ez egy egyenes vonal. (Az SVM beszél, használja a lineáris kernel.) Mivel így lineáris közelítését, is képes futni meglehetősen gyorsan. Ha a valóban helyezi szolgáltatás intenzív adatokat, például szöveget vagy genomikus van. Ezekben az esetekben SVMs képesek külön osztályok, gyorsabb és kisebb, mint a legtöbb más algoritmusok kívül csak mérsékelt mennyiségű memóriát igénylő overfitting.

![Support vector gép osztály határ][7]

***A szokásos támogatási vektor gép osztály határ maximalizálja a nyereség szétválasztása két osztályba***

Egy másik termék, a Microsoft Research, a [két osztály helyi mély SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) egy nem-lineáris változata, amely megőrzi a lineáris verzió sebesség és a memória hatékonyságát a legtöbb SVM. Ideális esetben, ahol adja meg a lineáris módszer nem elég pontos választ. A fejlesztők folyamatosan gyors bontásához, a probléma a sok kis lineáris SVM problémák által. Olvassa el a [teljes leírása](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) a megtudhatja, hogyan azok lekért ki ez a trükk.

Nem lineáris SVMs ügyes bővítményében az [osztály egy SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) rajzol egy határt, a teljes adatkészletet szorosan elolvassuk. Célszerű az anomáliák érzékelésére. Minden olyan mértékben tartoznak e határ új adatpontok elég szokatlan kell megjegyezni.

### <a name="bayesian-methods"></a>Bayesian módszerek

Bayesian módszerek van kívánatos minőségi: ezek overfitting elkerülése érdekében. Ez azért van így, így néhány feltételezéseket előzetesen a válasz valószínű eloszlása. E megközelítés egy másik byproduct, hogy nagyon kevés paramétert. Borzas gép tanulás mindkét Bayesian algoritmusok osztályozás ([két osztályú Bayes pont gép](https://msdn.microsoft.com/library/azure/dn905930.aspx)) és a regressziós ([lineáris regressziós Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)) rendelkezik.
Fontos megjegyezni, hogy ezek feltételezik, hogy az adatok megosztása vagy illeszkedő egyenes vonallal.

A történelmi feljegyzések a Microsoft Research, Bayes' pont gép fejlesztették ki. Néhány kivételesen gyönyörű elméleti munka mögöttük rendelkeznek. Az érdekelt hallgató irányul, az [eredeti cikk a JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) és az [osztályon blog Kristóf László által](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Speciális algoritmusok

Ha egy nagyon különleges cél lehet Szerencséje van. A gépi tanulás Azure gyűjteményben vannak algoritmusok szakosodott rangfokozat előrejelző ([sorszám regressziós](https://msdn.microsoft.com/library/azure/dn906029.aspx)), count előrejelző ([Poisson regressziós](https://msdn.microsoft.com/library/azure/dn905988.aspx)) és anomáliák érzékelésére (egy [fő összetevőit elemző](https://msdn.microsoft.com/library/azure/dn913102.aspx) és [vektoros gép támogatja](https://msdn.microsoft.com/library/azure/dn913103.aspx)s alapján alapján).
És van egy egyedülálló fürtözési algoritmust is ([K-eszközöket](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![PEM alapú anomáliák érzékelésére][8]

***PEM alapú anomáliák érzékelésére*** *-túlnyomó része az adatok a méretektől jelentősen terjesztési gyanúsak pontok beleesnek stereotypical elosztás;*

![Adatok csoportosítása K eszközökkel][9]

***Egy adathalmaz K-eszközökkel 5 fürtök csoportosítva***

Van továbbá egy ensemble [egy-v-all multiclass osztályozó](https://msdn.microsoft.com/library/azure/dn905887.aspx), amely az N osztályú besorolás probléma bontja N-1 két osztályú besorolása problémákat. A pontosság, a képzési időt és a lineáris tulajdonságai a két osztály osztályozó használt határozza meg.

![Két osztály osztályozó alkot egy három-osztály osztályozó][10]

***Űrlap egy három-osztály osztályozó össze egy pár kettő-osztály besorolások***

Borzas gép tanulás egy hatékony gépi tanulás keretrendszere [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf)cím alatt is.
VW kategorizálási itt, szakmai, mivel azt is megtudhat az osztályozás és a regressziós és részben címke nélküli adatokból is kaphat. Konfigurálható több tanulási algoritmusok, veszteség függvények és optimalizálási algoritmusok egyikét használja. Készült a talajtól fel hatékony, párhuzamos és nagyon gyors. Ridiculously nagy szolgáltatáscsoportok kevés látható erőfeszítéssel kezeli.
Elindult, és a Microsoft Research által saját John Langford által vezetett, VW egy képlet egy tétel a készlet autó algoritmusok mezőjében. Nem minden probléma VW elfér, de ha Öné nem, valószínűleg érdemes eléri a maximumot, a tanulási görbe az összeköttetésen. Egyben [önálló nyílt forráskód](https://github.com/JohnLangford/vowpal_wabbit) több nyelven is elérhető.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
