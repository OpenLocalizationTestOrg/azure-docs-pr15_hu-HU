<properties 
    pageTitle="Adatok gyári használatieset - termék javaslatok" 
    description="Tudjon meg többet az Azure Data Factory más szolgáltatásokkal együtt használatával megvalósított használatieset." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016" 
    ms.author="shlo"/>

# <a name="use-case---product-recommendations"></a>Használatieset - termék javaslatok 

Azure Data Factory a Cortana intelligencia programcsomag megoldás gyorsítók végrehajtásához használt sok szolgáltatások közül.  Lásd: [Üzletiintelligencia-csomagja Cortana](http://www.microsoft.com/cortanaanalytics) lap a csomagja kapcsolatban további tájékoztatást. A dokumentum, amely Azure felhasználók már megoldódott és Azure Data Factory és más összetevő Cortana Üzletiintelligencia-szolgáltatások használatával megvalósított közös használatieset azt ismertetik.

## <a name="scenario"></a>Eset

Online kereskedők gyakran szeretné rávenni a termékek megvásárlásához szerint legvalószínűbb érdekli, és ezért valószínűleg vásárolnia termékekkel bemutató őket az ügyfeleknek. Ehhez az online kereskedők kell a felhasználó online felhasználói felület testreszabása, hogy az adott felhasználó személyre szabott termék javaslatok használatával. Ezek a személyre szabott javaslatok fel a jelenlegi és korábbi vásárlás viselkedés adatok termékadatok, újonnan bevezetett márka és terméktámogatási és ügyfélszolgálatának szegmens adatok alapján történik.  Ezenkívül biztosítanak a felhasználó termék javaslatok alapján elemzése a kombinált minden felhasználó általános használatát működést.

Az alábbi kereskedők célja optimalizálás a felhasználó kattintson-értékesítés Dokumentumkonvertálás és magasabb árbevétel hírnévpontszámokat.  A konvertálás érnek által előadásához környezetfüggő, viselkedés termék javaslatok ügyfél érdeklődési és a műveletek alapján. A használatieset-optimalizálhatja az ügyfelek számára kívánt vállalkozások példa online kereskedők használja azt. Ezen elvek azonban minden üzleti, amelyek ügyfelei körül a termékek és szolgáltatások vesznek és tartalmasabbá teszi ügyfeleik vásárlási a személyre szabott termék javaslatok szeretne alkalmazni.

## <a name="challenges"></a>Kihívásokkal kapcsolatban

Nincsenek sok kihívásokkal kapcsolatban, hogy online kereskedők arc meg az ilyen típusú használatieset végrehajtása. 

Első lépésként különböző méretű és az alakzatok adatokat kell okozhatnak több adatforrásból, mind a helyszíni és felhőbeli. Ezeket az adatokat termék adatai, a korábbi ügyféladatok viselkedés és a felhasználói adatok tartalmazza, a felhasználó megnyitja a online kiskereskedelem webhelyet. 

Második, személyre szabott termék javaslatok kell ésszerűen és pontosan idézhet számított és előre jelzett. Nemcsak a termék arculatának és működését, és a böngésző ügyféladatokkal online kereskedők is kell ügyfeleink visszajelzései szeretne a legjobb termékek javaslatok a felhasználó meghatározása kiszámíthatja a múltbeli vásárlások tartalmazza. 

A javaslatok harmadik, közvetlenül a felhasználót gördülékeny böngészési és élmény vásárlásával megadására, és küldje el a legutóbbi és a vonatkozó javaslatok terméket kell lennie. 

Végezetül kereskedők mérje azok megközelítés hatékonyságát általános felfelé-értékesítés nyomon követésével és idegen-értékesítés kattintson a konvertálás az értékesítési sikeres, és módosítsa a jövőbeli ajánlások kell.

## <a name="solution-overview"></a>Megoldás – áttekintés

Ez a Példa használatieset végrehajtása, és a valós Azure felhasználók megvalósítani Azure Data Factory és más Cortana intelligencia összetevő-szolgáltatások, például [HDInsight](https://azure.microsoft.com/services/hdinsight/) és a [Power BI](https://powerbi.microsoft.com/)segítségével.

Az online viszonteladótól használja, mint a adattárolási beállításainak egész a munkafolyamat-Azure Blob-tárolóban, egy helyszíni SQL server, Azure SQL-adatbázis és a relációs adatok intelligens.  A blob-tárolóban ügyféladatok viselkedés ügyféladatok és termék adatokat tartalmazza. A termék adatok termékadatok arculatának tartalmazza, és egy termék katalógus tárolt helyszíni egy SQL adatraktár. 

Kombinált és használhat az előadáshoz személyre szabott javaslatok alapján ügyfél érdeklődési és a műveleteket, miközben a felhasználó megnyitja a webhelyen a katalógus termékek termék ajánlási rendszerbe géppel adatot. A vevők is megjelenik a termékek azok készíteni a termékhez kapcsolódó alapján általános webhely szokásai, amelyek nem való bármilyen egyetlen felhasználó.

![Használatieset-diagram](./media/data-factory-product-reco-usecase/diagram-1.png)

Nyers webes naplófájlok gigabájt naponta készül az online viszonteladótól webhelyről félig strukturált fájlként. A nyers webes naplófájlok és az ügyfél- és katalógusadatok rendszeresen okozhatnak a az adatok gyári globálisan telepített adatok mozgását használatával szolgáltatásként Azure blobtárolóhoz be. A nyers naplófájlok a nap hosszú távú tároló blob-tárolóhoz formázva (év és hónap) alapján.  [Azure hdinsight szolgáltatáshoz](https://azure.microsoft.com/services/hdinsight/) a blob-tárolóban lévő nyers naplófájlok partition, és a struktúra, és a malac parancsfájlokkal skála j.leny naplók feldolgozása szolgál. A particionált webes adatokat naplózza a tanulási javaslat rendszer hoz létre a személyre szabott termék ajánlások géphez szükséges ráfordítások kibontásához kattintson feldolgozása.

Az ajánlási a gépi tanulási ebben a példában használt rendszer egy Megnyitás gépi tanulási javaslat platform a [Apache Mahout](http://mahout.apache.org/).  Az alkalmazási példát olyan [Azure gépi tanulási](https://azure.microsoft.com/services/machine-learning/) vagy egyéni modell alkalmazhatók.  A Mahout modellt előre a hasonló a webhelyen, a teljes szokásai alapján elemei között, és a személyre szabott javaslatok alapján az adott felhasználó létrehozásához használja.

Végül az eredményhalmaz személyre szabott termék ajánlások kerül a relációs adatok intelligens fogyasztási által a viszonteladótól webhelyet.  Az eredményhalmaz sikerült is egy másik alkalmazás blob-tárolóhoz közvetlenül elérhető, vagy más fogyasztói és használati eset további tárolja került.

## <a name="benefits"></a>Legfontosabb előny

A megoldást optimalizálása a termék ajánlási stratégia és a igazítás, akkor az üzleti célokat teljesülnie kell az online viszonteladótól termékkihelyezési és az értékesítési célok. Ezenkívül voltak üzemeltető és kezelheti a termék ajánlási munkafolyamat hatékony, a megbízható és a költség hatékony módon. A megoldás egyszerűen frissíti a modellt, és finomhangolása értékesítési kattintson a konvertálás a siker küszöbértéke a mértékek alapján hatékonyságát. Azure Data Factory használatával voltak elhagyja a időigényes és költséges kézi felhő az erőforrás-kezelés és igény szerinti felhő az erőforrás-kezelés áthelyezése. Ezért voltak, pénz, időt takaríthat meg, és az idő csökkentése megoldást üzembe. Leszármazással adatnézetek és műveleti szolgáltatásállapot vált, egyszerűen ábrázolása és elhárítása intuitív Data Factory nyomon követésére és kezelése az Azure portál elérhető felhasználói felület. A megoldás most ütemezett és kezelni, hogy kész adatainak megbízható előállított és felhasználók kézbesítve és adatok és a feldolgozás függőségek automatikusan kezelhetők az emberi beavatkozás nélkül.

Azáltal, hogy a személyre szabott vásárlási funkciókat, a létrehozott egy további versenytársak, jelentésfelhasználóknak ügyfél online viszonteladótól tapasztal, és fokozhatja értékesítési és a teljes felhasználói elégedettséget, ezért.



  