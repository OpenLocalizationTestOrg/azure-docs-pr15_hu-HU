<properties
    pageTitle="Ügyfél Churn gépi tanulási használatával elemzése |} Microsoft Azure"
    description="Az integrált modell elemzése és az ügyfél tejeskanna pontozási fejlesztését esettanulmány"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Ügyfél Churn elemzése Azure gépi tanulási használatával

##<a name="overview"></a>– Áttekintés
Ez a témakör bemutatja egy hivatkozás végrehajtása egy ügyfél tejeskanna analysis projekt Azure gépi tanulási Studio segítségével épül. A cikk ismerteti társított általános modellek holistically a ipari ügyfél tejeskanna, a probléma megoldásához. Azt is, amely a gépi tanulási kialakításának modellek pontosságát mérésére, és azt mérje fel, hogy a továbbfejlesztéséhez vonatkozó útmutatást.  

### <a name="acknowledgements"></a>Nyugták

A kísérlet történt fejlesztése, és Serge Berger, egyszerű adatok tudósok Microsoft és Roger Barga, a Microsoft Azure gépi tanulási korábban termék Manager tesztelése. Azure dokumentáció csapattag gratefully nyugtázza a szakértelem, és a tanulmány megosztási köszönetet őket.

>[AZURE.NOTE] Ez a kísérlet használt adatok nincs nyilvánosan elérhető. Egy példa tejeskanna elemzéshez gépi tanulási modell létrehozásának című témakörben: [Telco churn modell sablon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana üzletiintelligencia](http://gallery.cortanaintelligence.com/) -dokumentumtárban


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>A probléma az ügyfél tejeskanna
A fogyasztói piaci és az összes vállalati szektorok vállalkozások kell kezelni az tejeskanna. Előfordul, hogy tejeskanna túlzott és házirend döntéseket befolyásolja. A hagyományos megoldás, a nagy-innovációkká alakuljon churners előrejelzésére és megoldhatja az igényeik marketingkampányok segítõ szolgáltatáson keresztül, vagy speciális felmentésekről alkalmazásával. E két megközelítés iparágban az üzleti és még egy adott fogyasztói fürt belül egy industry (például távközlési) változhat.

A közös tényező, hogy az vállalkozások kell ezeket az ügyfél speciális adatmegőrzési erőfeszítéseket kisméretűvé. A természetes módszertan így pontszám minden tejeskanna valószínűsége az ügyfél és a legfelső N melyeket cím lenne. A legjobb vevők lehet, hogy a legtöbb jövedelmező lehetőségekből; például bonyolultabb esetekben profit függvény dolgozik speciális felmentéssel jelöltek a kijelölés közben. Azonban az alábbi szempontok olyan holisztikus tejeskanna kezelésére vonatkozó stratégia csak egy részét. Vállalkozások is figyelembe fiók kockázatok (és kapcsolódó kockázat hibatűrést), a szint és a költség beavatkozás és egyértelmű felhasználói szegmens.  

##<a name="industry-outlook-and-approaches"></a>Üzleti az outlook és az alábbi módszerek
Összetett kezelésének tejeskanna egy elért ipari meg. A klasszikus példája a távközlési ágazat hol előfizetők ismert gyakran váltani egy szolgáltatót a másikra. A önkéntes tejeskanna fontos elsődleges. Ezenkívül szolgáltatók van halmozott *illesztőprogramok churn*, jelentős ismereteket tényezők, amelyek, hogy a meghajtó felhasználók válthat.

Például a kézibeszélőt vagy más eszközhöz választási lehetőségek, a mobiltelefon üzleti tejeskanna jól ismert illesztőprogram. Emiatt a népszerű házirend, hogy kézibeszélő árát subsidize az új előfizetők és a meglévő ügyfelek frissítés teljes ár töltés alatt. Régebben a házirend vezetett között egy szolgáltatótól hopping ügyfelek kapott egy új árengedményt, és a finomíthatja a stratégiák szolgáltatók még kéri.

Az kézibeszélőt ajánlataiban magas illékonyság tényezővel gyorsan nagyon érvényteleníti tejeskanna aktuális kézibeszélőt modellek alapján mintái. Ezenkívül mobiltelefonok melyek nem csak távközlési eszközök; Ezek egyben módon kimutatások (fontolja meg az iPhone-alapú), és a közösségi kiszámítására normál távközlési adatkészletek hatókörén kívüli.

Nettó modellezése az eredménye, hogy nem tudja-e a hang házirend tervez egyszerűen tejeskanna ismert okai kiküszöbölésével. A folyamatos modellezési stratégia, többek között a kockák változók (például döntési fák) számszerűsítése klasszikus modellek valójában **kötelező**.

Nagy adatkészletek ügyfeleik változatával szervezetek hajt végre nagy adatok analytics (különösen, nagy adatokon alapuló tejeskanna észlelési), a probléma hatékony megközelítés. További információ a nagy adatok megközelítés ETL szakasz ajánlások tejeskanna problémája is megkeresheti.  

##<a name="methodology-to-model-customer-churn"></a>Modell ügyfél tejeskanna módszer
Gyakori problémák megoldása folyamat oldhatja meg az ügyfél tejeskanna olyan téglalapként ábrázol a számok 1-3:  

1.  A kockázat modell lehetővé teszi, hogy bemutatókkal műveletek hatása a valószínűség és a kockázat.
2.  Egy beavatkozás modell lehetővé teszi, hogy hogyan beavatkozás szintjét alkalmazásról hatással lehet a valószínűség tejeskanna és az ügyfél mennyiségét bemutatókkal élettartamának (CLV).
3.  Az elemzés házirendkezelő egy minőségi elemzés, amelyek a felhasználói szegmens az optimális ajánlat előadásához függvénynél megelőző marketingkampányhoz hívásból van.  

![][1]

Előre megjelenésű eljárás a legjobb módszer tejeskanna kezelni, de azt összetettsége megtalálható: felkínálunk egy a modellben több archetype és a modellek nyomkövetési függőségeket fejlesztéséhez. A kapcsolati modellek között van beágyazva, a következő ábrán látható módon:  

![][2]

*Ábra 4: Az egyesített több modell archetype*  

Az modelljei közötti kölcsönhatás kulcs esetén a holisztikus megközelítés végzi a felhasználói adatmegőrzési vagyunk. Minden egyes modell feltétlenül csökkenti az idő; a architektúra ezért egy implicit loop (hasonlóan, mint a archetype, állítsa a SZÍNELOSZLÁS-DM adatok adatbányászati szabványos, [***3***]).  

A marketing döntés kockázat szegmens/dekompozíciós általános ciklus még mindig általános szerkezetet, és sok üzleti problémák vonatkoznak. Tejeskanna analysis egyszerűen erős képviselőjének ebben a csoportban, a problémák, mivel azonban a bonyolult üzleti problémák, amelyek nem teszi lehetővé a cserélendő egyszerűbb megoldást összes jellemzők. A régi megközelítés churn közösségi részei nem kifejezetten aktív a megközelítésben, de a közösségi szempontok beágyazva a modellezési archetype az azok bármelyik modell lenne.  

Egy érdekes összeadás nagy adatok analytics. Az aktuális távközlési és kiskereskedelmi vállalkozások összegyűjtése ügyfeleik részletes adatait, és azt is egyszerűen előre, hogy a modellben több kapcsolat szükséges felmerülő trendek, például a internetes a dolog, amit és minden eszközök, amelyek lehetővé teszik az üzleti alkalmazása intelligens megoldások rétegeinek a megadott gyakori tendenciát válnak.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>A modellezési archetype végrehajtási gépi tanulási Studio
A probléma csak leírt tekintve ActiveX-vezérlőket a legjobb módja az integrált modellezési és megközelítés pontozási végrehajtása Ebben a részben azt fogja bemutatják, hogyan azt megvalósítani ez Azure gépi tanulási Studio segítségével.  

A több elem modell megközelítés, kell, a globális archetype az tejeskanna tervezésekor. Még a pontozási (prediktív) részét a megoldás több modell kell lennie.  

Az alábbi ábra mutatja a prototípusának létrehozott, amely alkalmaz a gépi tanulási Studio tejeskanna előrejelzésére négy pontozási algoritmusok. A több elem modell-alapú megközelítés alkalmazásával azért nem csak egy ensemble besorolás, pontosságának növelése érdekében, de is és a igazítás túlzott vírusvédelmet és kiválasztásának jobbá tétele érdekében elfogadott szolgáltatás hozzon létre.  

![][3]

*Ábra 5: Egy tejeskanna megközelítés modellezése az prototípusának*  

A következő szakaszokban a modell, amely azt gépi tanulási Studio használatával megvalósított pontozási prototípusának olvashat bővebben.  

###<a name="data-selection-and-preparation"></a>Adatok kijelölés és előkészítése
A modellek létrehozásához használt adatok és pontszámhoz ügyfelek adatainak védelme érdekében ügyfél kódolták adatokat tartalmazó származik CRM függőleges megoldás. Az adatok az Amerikai Egyesült Államokban 8000 előfizetések információt és három források egyesít: kiépítési adatokat (előfizetés metaadatok), a tevékenység adatait (a rendszer használatát) és az ügyfél-támogatási adatait. Az adatok nem tartalmaz olyan üzleti kapcsolatos adatokat a vevők; Ha például nem tartalmazza hűséges metaadatok vagy a hitelkártya értékek.  

Az egyszerűség kedvéért ETL és adattisztítási folyamatok ki a hatókör mivel feltételezzük, hogy adatok előkészítése már rendelkezik-e már végzett máshol.   

Modellezési funkció kijelölés alapján előzetes pontosságként megadott érték közül a kiszámítására, a folyamatot, a véletlen erdő modul használó szereplő pontozási. Azt a gépi tanulási Studio végrehajtásához a az átlag, a medián és a tartományokat képviselő szolgáltatások számítja ki. Ha például összegzések minőségi adatokhoz, például a felhasználói tevékenység minimális és maximális értékek jelöltük.    

Azt is rögzített időbeli adatok a legutóbbi hat hónapra. Azt a adatok elemzése egy évig tart, és azt megállapítani, hogy akkor is, ha volt-statisztikailag jelentős trendek, tejeskanna hatással nagyban csökken hat hónapra.  

A legfontosabb pont, hogy a teljes folyamat ETL, szolgáltatásainak kiválasztása, beleértve és modellezése a gépi tanulási Studióban, a Microsoft Azure adatforrásokat használó megtörtént.   

Az alábbi ábrán is láthatja az adatokat, használt szemléltetése  

![][4]

*Ábra 6: Kivonat adatforrást (kódolták)*  

![][5]


*Ábra 7: Adatforrás kiolvasott funkciók*
 
> Figyelje meg, hogy az adatok privát, és ezért a modell és adatokat nem lehet megosztani.
> Nyilvánosan elérhető adatok használata hasonló modell, nézze át az alábbi példa a [Cortana üzletiintelligencia-gyűjtemény](http://gallery.cortanaintelligence.com/)kísérletezhet: [Telco ügyfél Churn](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Ha többet szeretne tudni, hogyan alkalmazhat egy tejeskanna analysis modell Cortana üzletiintelligencia-csomagot használ, is javasoljuk [ezt a videót](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) , a Program vezető hétköznapon Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>A prototípusának a felhasznált algoritmusokról

A következő négy gépi tanulási algoritmusok használatával összeállítása a prototípusának (nincs Testreszabás):  

1.  Logisztikus regresszió (LR)
2.  Csillapítja döntési fa (BT)
3.  Átlagolt perceptron (alkalmazás)
4.  Támogatás vektoros gépi (SVM)  


Az alábbi ábra bemutatja a kísérlet tervezési felületre, amely jelzi a sorrendben, amelyben a modellek létrehozott egy részét:  

![][6]  


*Ábra 8: Gépi tanulási Studio modellek létrehozása*  

###<a name="scoring-methods"></a>A pontozási módszerek
Egy címkével ellátott oktatás adatkészlet használatával a négy modellek azt pontszáma.  

Azt is a pontozási adatkészlet Társítások vállalati bányászati 12 asztali verzióját használatával beépített hasonló modellhez feladatot. A Társítások modell és az összes négy gépi tanulási Studio modellek pontosságát mérni azt.  

##<a name="results"></a>Eredmény
Ebben a részben bemutató azt az eredmények információ a modellek alapján a pontozási adatkészlet pontosságát.  

###<a name="accuracy-and-precision-of-scoring"></a>És pontozási pontossága
A végrehajtás Azure gépi tanulási általában Társítások mögött van, a pontosság %-kal 10 – 15 (terület alatt görbe vagy AUC).  

Azonban a legfontosabb a tejeskanna mérőszáma a téves besorolás ráta: Ez azt jelenti, hogy, mint a besorolás által előre jelzett legnépszerűbb vagy legnagyobb N churners, hogy ezek közül ténylegesen did **nem** tejeskanna, és a speciális kezelés még érkezett? Az alábbi ábra a téves besorolás ráta összes modellek hasonlítja össze:  

![][7]


*Ábra 9: Passau prototípusának görbe területen*

###<a name="using-auc-to-compare-results"></a>Hasonlítsa össze az eredmények AUC használatával
Terület alatt görbe (AUC) egy mérőszám, amely egy *separability* között pozitív és negatív populáció az értékek elosztása globális mértéke. Hasonlóan, mint a hagyományos címzett operátor jellemző (ROC) diagramot, de egy fontos különbség a AUC mérőszám nincs szükség megadott küszöbértékét értékét adja meg. Ehelyett, áttekintheti az eredmények át **az összes** lehetséges lehetőségeket. Viszont a hagyományos ROC grafikon Megadja, hogy pozitív a függőleges tengely és a hamis pozitív esetén, a vízszintes tengelyen, és a besorolás küszöbértékét változik.   

AUC általában szolgál áron történő változásának mértéke különböző algoritmusok (vagy másik rendszerek), mivel lehetővé teszi, hogy az AUC értékük alapján összehasonlítani modellek. Ez az a népszerű megközelítés, például meteorológia és biosciences iparágban. Így AUC helyén besorolás teljesítmény értékeléséhez használt eszköz.  

###<a name="comparing-misclassification-rates"></a>Téves besorolás díjak összehasonlítása
Azt hasonlítja össze a téves besorolás díjak a szóban forgó az adatkészlet körülbelül 8000 előfizetések CRM adatok felhasználásával.  

-   A Társítások téves besorolás ráta 10 – 15 % volt.
-   A gépi tanulási Studio téves besorolás ráta a felső 200-300 churners 15-20 %-os volt.  

A távközlési üzleti, a fontos csak a churn segítõ szolgáltatás vagy más speciális kezelés biztosításával őket a legmagasabb kockázatot rendelkező felhasználóknak címet. Gépi tanulási Studio végrehajtására, hogy a szempontból éri el a eredmények hatékonysága eléri a Társítások modell.  

Hasonlóképpen pontosság oka fontosabb, mint a pontosság azt érdeklik főleg megfelelően osztályozásához potenciális churners.  

Az alábbi ábra a Wikipedia a kapcsolatot, színes, könnyen értelmezhető ábra szemlélteti:  

![][8]

*Ábra 10: Útján pontosságát és a pontosság között*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Pontosságát és a pontosság eredmények csillapítja döntési fa modellhez  

A következő diagramon a gépi tanulási prototípusának használatával történik a legpontosabb a négy modellek között kell csillapítja döntési fa modell pontozási nyers eredményeit jeleníti meg:  

![][9]

*Ábra 11: Csillapítja döntési fa modell jellemzők*

##<a name="performance-comparison"></a>Teljesítmény összehasonlítása
A sebesség, amelynél adatok pontszáma volt a gépi tanulási Studio modellek és Társítások vállalati bányászati 12.1 asztali verzióját használatával létrehozott összehasonlítható modell összehasonlítja azt.  

Az alábbi táblázat összefoglalja a teljesítmény algoritmusok:  

*Tartalomjegyzék 1. Általános teljesítménybeli (pontosság) algoritmusok*

| LR|BT|ALKALMAZÁS|SVM|
|---|---|---|---|
|Átlagos modell|A legjobb modell|Underperforming|Átlagos modell|

A modellek gépi tanulási Studio nagymértékben névérték volt az adatvégrehajtás, de a pontosság sebességének 15-25 %-kal outperformed Társítások is.  

##<a name="discussion-and-recommendations"></a>Vitafórum és ajánlások
A távközlési iparágban több eljárások van kiderült elemzése tejeskanna, például:  

-   Mértékek négy alapvető kategóriába tartozó származhat:
    -   **Személy (például egy előfizetési)**. Az előfizetés és/vagy a vevő, amelyek tárgya: tejeskanna vonatkozó alapadatok kiépítése.
    -   **Tevékenység**. Szerezze be az személyhez, például a bejelentkezések száma kapcsolódó összes lehetséges használati adatok.
    -   A **támogatási szolgálatát**. Harvest ügyfél támogatási naplók jelzi, hogy az előfizetés volna, problémák vagy az ügyfélszolgálat kapcsolati adatait.
    -   **Versenyelemzési és az üzleti adatok**. Szerezze be a minden lehetséges a vevő adatait (például lehet nem érhető el vagy nyomon követése nehezen).
-   Használja a sürgős meghajtó szolgáltatásainak kiválasztása a. Ez azt jelenti, hogy a csillapítja döntési fa modell mindig a megígérésével megközelítés.  

Ezekben a kategóriákban e hoz létre, amely lehetővé teszi tényezőktől kategóriánkénti, formázott indexek alapján egy egyszerű *mérvadó* megközelítésben a vevők tejeskanna veszélyének azonosítására kell elegendő a finomabb. Sajnos bár ez állomástól egyértelmű tűnik, akkor a hamis ismertetése. Oka az, hogy tejeskanna a időbeli hatással, és a tényezők churn általában tranziens állapotban. Mi a ma elhagyása figyelembe vevő vezet eltérő lehet a holnap, és bizonyára legyen különböző hat hónap múlva. Emiatt a *probabilisztikus* modell kezdetén.  

Az üzleti, amely általában inkább egy üzleti intelligencia készült megközelítés analytics gyakran kihagyott a e fontos megfigyelés, főleg mivel az egy könnyebben felkínálása és beengedi az egyszerű automatizálási.  

A cikk megvásárlását fel önkiszolgáló analytics gépi tanulási Studio segítségével azonban, hogy a négy kategóriák adatait, részleg vagy osztály által osztályozott válik gépi tanulási tejeskanna információ értékes forrás.  

Az Azure gépi tanulási lesz egy újabb izgalmas lehetőség az azt jelenti, hogy egy egyéni modul felvétele már elérhető előre definiált modulok gyűjteményének. Ezt a funkciót lényegében hoz létre, jelölje ki a tárakban, és függőleges piacokon sablonok létrehozása lehetőséget. Azure gépi tanulási piacon fontos differentiator.  

Azt kívánom továbbra is kapcsolatban, ez a témakör nagy adatok analytics, különösen kapcsolatos.
  
##<a name="conclusion"></a>Elfogadásáról
Ez a papír kijelölve megközelítése problémájának közös ügyfél tejeskanna egy általános keretrendszer használatával mutatja be. Azt a egy prototípusának a modellek pontozási tekinti, és az Azure gépi tanulási használatával megvalósított azt. Végezetül azt értékelni pontosságát és a prototípusának megoldás kapcsolatban a Társítások hasonló algoritmusok teljesítményét.  

**További információ:**  

Megváltozott a nyomtatópapír segítséget? Kérjük, küldje el visszajelzését. Mondja el, az 1 (gyenge) méretaránnyal a-5 (kiváló), hogyan szeretné Ön értékelése a papír, és miért van, adta meg minősítése? Példa:  

-   Vannak, minősítés azt magas jó példák, kiváló képernyőképek, törölje a jelet ír, vagy más ok miatt?
-   Vannak, minősítés, alacsony gyenge példákat, zavaros képernyőképek vagy világos írási miatt?  

A visszajelzés segítséget nyújt engedje fel az azt tanulmányok minőségének fejlesztését.   

[Visszajelzés küldése](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Hivatkozások
[1] prediktív Analytics: túl az előrejelzések Nyugat McKnight, információkezelés július/augusztus 2011, p.18 – 20.  

[2] a Wikipedia-cikket: [pontosságát és a pontosság](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [SZÍNELOSZLÁS-DM 1.0: részletes adatok adatbányászati útmutató](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [nagy adatok Marketing: hatékonyabban folytatni az ügyfelekkel, és a meghajtó érték](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco churn modell sablon](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) [Cortana üzletiintelligencia](http://gallery.cortanaintelligence.com/) -dokumentumtárban 
 
##<a name="appendix"></a>Függelék

![][10]

*Ábra 12: Tejeskanna prototípusának bemutató pillanatképét*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
