<properties 
    pageTitle="Gyakori DocumentDB használjon esetekben |} Microsoft Azure" 
    description="Tudnivalók a felső DocumentDB esetek öt használata: felhasználók létrehozott tartalmat, az eseménynaplózást, katalógusadatok, a felhasználói beállítások adatok és dolog Internet (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Gyakori DocumentDB használati eset
Ez a cikk néhány gyakori használati eset áttekintést nyújt a DocumentDB.  Ebben a cikkben a javaslatok szolgálhat a kiindulási pontként fejleszt DocumentDB alkalmazáshoz.   

Ez a cikk elolvasása, után is az alábbi kérdésekre választ: 
 
- Mik azok a közös használat esettel DocumentDB?
- Mik azok a előnyei DocumentDB felhasználóként generált tartalom áruház?
- Mik azok a előnyei DocumentDB katalógus adatokat tárolni?
- Mik azok a előnyei DocumentDB megegyezik egy eseménynaplójának tárolni?
- Mik azok a előnyei DocumentDB a felhasználói beállítások adatként tárolni?
- Mik azok a előnyei DocumentDB egy adatokat tároló dolog Internet (IoT) rendszerekhez?

## <a name="common-use-cases-for-documentdb"></a>DocumentDB esettel közös használata
Azure DocumentDB egy olyan általános célú NoSQL adatbázis, az alkalmazások és a használati eset széles köre használt. Érdemes bármely alkalmazásban, amely alacsony sorrend a ezredmásodperc a válaszidő van szüksége, és méretezze gyorsan kell kinek ajánljuk. Az alábbiakban néhány DocumentDB, hogy jól illeszkedik a nagy teljesítményű alkalmazások attribútuma.

- DocumentDB natív módon partíciók magas rendelkezésre állásának és méretezhetőség: az adatok.
- DocumentDB meg a kis-késés sorrend a ezredmásodperc a válaszidő biztonsági SSD tároló tartalmaz.
- Egységesebb szintek például esetleges, munkamenet és határolt staleness DocumentDB's támogatása alacsony költség-a teljesítmény arány tesz lehetővé. 
- DocumentDB rugalmas adatok környezetbarát árak modell, amely méter tárterület és a teljesítmény független tartalmaz.
- DocumentDB által fenntartott átviteli modell értelmez olvasása és írása helyett Processzor/memória/IOPs az alapul szolgáló eszközök kapacitása száma szerint teszi lehetővé.
- DocumentDB a tervező segítségével, tömeges kérelem kötet napi kérések milliárd sorrendjében szeretne méretezni.

Amikor webes mobil, játék és IoT alkalmazásokról kell alacsony válaszidő és kezelheti a nagy mennyiségű olvasása és írása a következő attribútumok különösen hasznos. 

## <a name="user-generated-content"></a>Felhasználói létrehozott tartalmat
A közös használatieset-DocumentDB tárolására és felhasználói lekérdezés és hoz létre (UGC) tartalma webes mobilalkalmazások, különösen közösségi hálózat alkalmazások.  Néhány példa a UGC csevegésekben, twitterre, blogbejegyzések, minősítések és megjegyzések.  Gyakran a közösségi hálózat alkalmazásokban a UGC egy keverése, tulajdonságok, a címkék és a kapcsolatok, a program nem korlátozza a képzési struktúra, a szabad formátumú szöveg.   

Tartalom például csevegések, megjegyzések és bejegyzések tárolható DocumentDB átalakítások vagy relációs hozzárendelés réteghez összetett objektum nélkül.  Adatok tulajdonságait, illetve is megfelelően követelményeket, a fejlesztők Ismétlés a alkalmazás kódot, így előléptetése gyors fejlesztése, egyszerűen módosítani.  

Alkalmazások, amelyek különböző közösségi hálózatok integrálása sémák módosítása a következő hálózati kell reagálni.  Adatok automatikusan indexelt alapértelmezés szerint a DocumentDB, mint adatok készen áll a bármikor lekérdezendő.  Ezért ezeket az alkalmazásokat van a saját igényeik szerint előrejelzések beolvasásához rugalmasságot.       

Közösségi alkalmazások számos közötti globális skála és járhat szokásai is mutathat.  Rugalmasság a méretezés az adatokat tároló fontos az alkalmazási réteg méretezze át a megfelelő használatát igény szerint.  Úgy méretezheti meg további adatokat partíciót egy DocumentDB fiókon hozzáadásával.  Több területek között ezenkívül további DocumentDB fiókokat is létrehozhat. Régió DocumentDB szolgáltatáselérhetőség a [Régiók Azure](https://azure.microsoft.com/regions/#services)című témakör tartalmaz.   

## <a name="catalog-data"></a>Katalógusadatok
Katalógus adatok használatát esetek tárolja, és a lekérdezés például személyek, a helyek és a termékek entitás attribútumok járnak.  Néhány példa a katalógusadatok felhasználói fiókokat, a termék katalógusok, az eszköz nyilvántartó IoT és anyagok rendszerek számla.  Ezeket az adatokat az attribútumok eltérőek lehetnek, és alkalmazás követelmények igazítása időmennyiségét módosíthatja.  

Fontolja meg egy termékkatalógus példa egy autós kijelzők szállító. Minden kijelző a saját attribútumok kívül minden részét megosztása közös attribútum lehet.  Egy adott részére attribútumok továbbá módosíthatja az a következő év, amikor megjelenik az új modell.  A JSON-dokumentum mentése, mint DocumentDB rugalmas sémák támogatja, és lehetővé teszi, hogy a beágyazott tulajdonságok adatok megjelenítése, és így jól illeszkedik a termék katalógusadatok tárolásához.       

## <a name="logging-and-time-series-data"></a>Naplózás és idősorok adatok
Nagy mennyiségű gyakran kibocsátott a alkalmazás naplózás, és előfordulhat, hogy van változó attribútumai a telepített alkalmazások verziót vagy alapján összetevő eseményeknek.  Összetett kapcsolatok és kemény szerkezetek nem korlátozza a naplóadatokat. Egyre naplóadatok megőrződnek JSON formátumban mivel JSON hozza létre, és könnyen olvasható az emberek.
   
A szokásos vannak eseménynaplójának adatokhoz kapcsolódó két fő használati eset.  Az első használati eset is végezhet az alkalmi lekérdezések hibaelhárítási adatainak egy részét.  Hibaelhárítás, alatt lévő adatok egy részhalmazát először keresi ki a naplókat, általában idősorok által.  Ezután a Lehatolási hiba szintek vagy hibaüzenetek jelennek meg az adatkészlet szűréssel történik. Ez az adott eseménynaplók tárolása DocumentDB pedig előny. Naplóadatok DocumentDB tárolt automatikusan indexelt alapértelmezés szerint, és így bármikor lekérdezendő készen is. Ezeken kívül naplóadatok is lehet állandó keresztül adatok partíciót idő adatsorként. Az adatmegőrzési házirend használati hideg-tárolóhoz régebbi naplók is változatwebhelyeken.          

A második használatieset tényezői: hosszan futó végrehajtott offline naplóadatok nagy mennyiségű adat analytics feladatok.  A használati eset többek között kiszolgáló elérhetőségének elemzés, alkalmazás hiba elemzési és clickstream adatok elemzéséhez.  Általában Hadoop hajtsa végre az alábbi típusú elemzések használják.  DocumentDB Hadoop összekötő DocumentDB adatbázisok működhet adatforrások és mosdók malac struktúra, és a térkép/kicsinyítés feladatokhoz. A Hadoop összekötőre, az DocumentDB részletekért olvassa el [feladat futtatása olyan Hadoop DocumentDB és hdinsight szolgáltatásból lehetőségre](documentdb-run-hadoop-with-hdinsight.md).      

## <a name="gaming"></a>Játék
Az adatbázis réteg játék alkalmazások fontos összetevői fut. Modern játékok végezze el a grafikus feldolgozás mobile/konzolban ügyfélalkalmazásokon, de a felhőben, például játék stat, az együttműködést a közösségi médiumokkal és a Max-pontszámhoz ranglisták testre szabott és személyre szabott tartalom támaszkodhat. Játékok csak nagyon kevés késések az Olvasás és egy érdekes megadására írások játék élmény, és az adatbázis réteg kell kezelni a kérelem díjak követéséhez új játék indítást és szolgáltatásfrissítések során.

Tömeges méreteket játékok például által használt DocumentDB [a kiállniuk csendes: nincs férfi Land](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) [Következő játékok](http://www.nextgames.com/), és [Halo 5: felügyeletet](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Mindkét használjon esetekben, DocumentDB főbb előnyei volt a következőket:

- DocumentDB lehetővé teszi, hogy a teljesítmény tolja felfelé vagy lefelé, elastically. Kezelheti a frissítése a profil- és az egyidejű gamers millióit tucatnyi a stat egyetlen API hívásbeállítások játékok lehetővé teszi.
- DocumentDB ezredmásodperc olvasás támogatja, és elkerülése érdekében bármelyik késedelmes jelentések játék során ír.
- Az Automatikus indexelés DocumentDB meg több különböző tulajdonságok valós idejű ellen szűrés lehetővé teszi, hogy, például keresse meg a belső player azonosítók, vagy azok GameCenter, Facebook, a Google-azonosítók, vagy egy guild player tagsága alapuló lekérdezés játékosokat. Ez a lehetőség összetett indexelés vagy sharding infrastruktúra nélkül.
- Közösségi szolgáltatások, például a játék csevegés üzeneteit, player guild tagságot, befejeződött problémáit, nagy-pontszámhoz ranglisták és közösségi diagramok egyszerűbb egy rugalmas séma használata.
- DocumentDB szolgáltatásként felügyelt platform-mint-a-(PaaS) szükséges minimális beállítása és kezelése gyors közelítés engedélyezése működik, és piacra idő csökkentése.


## <a name="user-preferences-data"></a>Felhasználói beállítások adatok
Ma a legtöbb modern webes és mobilalkalmazások jár az összetett nézetek és szolgáltatások. Ezek a nézetek és szolgáltatások általában dinamikus, felhasználói beállítások és hangulatának megfelelően élelmezési Megjelenés és márkajegyek igényeinek.  Alkalmazások így kell lennie tudja beolvasni a személyre szabott beállítások hatékony annak érdekében, hogy a felhasználói felület elemei és tapasztalatok gyorsan jeleníti meg. 

JSON-hatékony formátuma nem csak könnyű, de is egyszerűen által értelmezhető JavaScript felhasználói felület elrendezés adatok jelöl.  DocumentDB kínál, amelyek lehetővé teszik a kis késés írása gyors olvasás hangolható konzisztencia szintek. Így lehet, hogy az adatok célba juttatni a vezeték-hatékony eszközöket többek között a JSON-dokumentumként személyre szabott beállítások DocumentDB a felhasználói felület elrendezés adatok tárolására.

## <a name="iot-and-device-sensor-data"></a>IoT és eszköz szenzoradatokat
IoT használati eset gyakran megosztása egyes mintázatok hogyan azok ingest, folyamat és tárolja az adatokat.  Első lépésként adatok bevitele, amely a különféle nyelvek eszköz érzékelőktől adatok felszakadásáig is ingest e rendszerek lehetővé teszi.  Ezután a folyamat, és valós idejű háttérismeretek forrásául adatfolyam adatok elemzése. És utoljára azonban nem legalább legtöbb Ha nem az összes adat fog ahányat land a tárolóban alkalmi lekérdezésekor és kapcsolat nélküli elemzéséhez.    

Microsoft Azure kínál az IoT valószínűleg kihasználják gazdag szolgáltatások használata esetek.  Azure IoT services szolgáltatást többek között az Azure esemény hubok, Azure DocumentDB, Azure Értékáram-elemzés, Azure értesítés központi, Azure gépi tanulási, Azure hdinsight szolgáltatáshoz és PowerBI olyan. 

Az adatok felszakadásáig Azure esemény hubok is okozhatnak, szerint a magas átvitt adatok bevitel alacsony válaszidejű kínál. A funneled adatok keresztül a szervezetbe dolgozható fel a valós idejű betekintést kell, hogy az Azure Értékáram-elemzés valós idejű ábrázolja. Adatok lekérdezésére alkalmi töltődnek be a DocumentDB. Miután az adatok DocumentDB tölti be, az adatok készen áll a lekérdezhetők.  Az adatok DocumentDB is alkalmazható hivatkozási adatok valós idejű analytics részeként. Ezeken kívül adatok további Iskolás, illetve csatlakozás DocumentDB adatok HDInsight malac, struktúra vagy térkép/csökkentés feladatok által feldolgozott.  Finomított adatok majd betöltése vissza az DocumentDB tartalmazó jelentések készítéséhez.   

Egy minta IoT megoldás DocumentDB, EventHubs és vihar a [hdinsight-vihar – példák tárházából GitHub](https://github.com/hdinsight/hdinsight-storm-examples/)című témakör tartalmaz.

További információt a IoT az Azure szeretne rendelni a [a dolog Internet létrehozása](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx)című témakör tartalmaz.

## <a name="next-steps"></a>Következő lépések
 
Első lépések DocumentDB, hozzon létre egy [fiókot](https://azure.microsoft.com/pricing/free-trial/) , és kövesse a a [tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/documentdb/) DocumentDB ismertetése, és keresse meg a szükséges információkat. 

Vagy, ha többet szeretne tudni DocumentDB alkalmazó felhasználók, a következő ügyfél írások használhatók:

- A [következő játékok](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). A Walking kézbesítetlen: nincs férfi Valentin Land játék soars # 1 Azure DocumentDB által támogatott.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hogyan Halo 5 végrehajtani, közösségi játék Azure DocumentDB használatával.
- A [Cortana Analytics-gyűjteményben](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics gyűjtemény - méretezhető közösségi webhely Azure DocumentDB épül.
- [Kezelheti](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Multinacionális cégek globális betekintést rugalmas felhő technológiákkal percben integrátor vezető adja vissza.
- [Hírek Köztársaság](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Intelligenciával kapcsolatos funkciók felvétele a tájékoztatási célra a tevékenykedik polgárok híreket. 
- [Nemzetközi SGS](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Egységes szín végig a földgömb fő márka SGS kapcsolja be. És SGS bekapcsolja az Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globális vezetője Telenor áthelyezése az indítási sebességének használja a felhőben. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). A tároló, a jövő gyors szöveg.keres és a könnyű adatáramlást futtatható.
