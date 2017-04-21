#<a name="data-management-and-business-analytics"></a>Adatok kezelése és üzleti elemző

Kezelése az adatok és elemzése a felhőben fontos csak, mint máshol. Lehetővé teszi, hogy ehhez, Azure biztosít technológiák adattartomány relációs és nem relációs adatok használata. Ez a cikk bemutatja az beállításokat. 

##<a name="table-of-contents"></a>A tartalomjegyzék      

- [BLOB-tárolóhoz](#blob)
- [Egy adatbázis-kezelő egy virtuális gépen futó](#dbinvm)
- [SQL-adatbázis](#sqldb)
    - [SQL-adatok szinkronizálása](#datasync)
    - [SQL adatszolgáltatás virtuális gépeken futó használatával](#datarpt)
- [Táblatároló](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>BLOB-tárolóhoz

A word "blob" rövid a "nagy bináris objektum", és pontosan leírja, mely egy BLOB: bináris adatokat gyűjteménye. Még akkor is, ha egyszerű is legyenek, BLOB hasznosak igazán. [Ábra 1](#Fig1) Azure Blob-tárolóhoz alapjait mutatja be.

<a name="Fig1"></a>![BLOB ábrája][blobs]
 
**Ábra 1: Azure Blob-tárolóhoz bináris adatokat tárolja - BLOB - tárolók.**

BLOB használatához először létrehoz egy Azure *tárterület-fiókot*. Részeként adja meg az Azure adatközponthoz, amely az objektumokat, amelyet Ön hozott létre ehhez a fiókhoz tárolni fogja. Él, ahol minden blob-hoz létre a tárterület-fiók néhány tároló tartozik. Blob eléréséhez az alkalmazás egy URL-CÍMÉT az űrlapot, nyújtja:

http://&lt;*StorageAccount*&gt;.blob.core.windows.net/&lt;*tároló*&gt;/&lt;*BlobName*&gt;

&lt;*StorageAccount* &gt; tároló új fiók létrehozásakor hozzárendelt egyedi azonosító közben &lt; *tároló* &gt; és &lt; *BlobName* &gt; vannak bizonyos tároló és azon belül blob nevét. 

Azure BLOB két különböző típusú biztosít. A következő lehetőségek közül:

- *Továbbfejlesztett fájlblokkolás* BLOB, amelyek az adatok legfeljebb 200 GB is tartalmazhat. Javaslatokat tesz a nevét, mint blokk blob oszlik néhány blokkok számát. Ha hiba történik a továbbfejlesztett fájlblokkolás blob átvitele közben, adatújraküldést folytathatja a legutóbbi tiltás helyett a teljes blob ismét elküldeni. Blokkokból álló BLOB egy általános tárolási megközelítés, és ma legyenek a leggyakrabban használt blob-típus.

- Oldalszámokat egy adjon, *oldal* BLOB. Lap BLOB-elérésű készült, és hogy mindegyik meg van osztva egyes oldalak száma. Az alkalmazás ingyenes olvashatók, és az egyes lapokat, véletlenszerűen írja be a blob. Az Azure virtuális gépeken futó, például hoz létre VMs használja lap BLOB állandó tárolására OS lemez és adatok lemezt is.

Blokkokból álló BLOB vagy lap BLOB választja, hogy alkalmazások hozzáférhet blob-adatokat számos különböző módon. A beállítások az alábbiak:

- Közvetlenül a RESTful (tehát HTTP-alapú) protokoll eléréséhez. Azure-alkalmazások és a külső alkalmazásai, köztük a helyszíni, futó alkalmazások használhatja ezt a beállítást.
- Használja az Azure tároló ügyfél tárban, amely több Fejlesztőeszközök rövid felületet biztosít fölött nyers RESTful blob access Protocol (protokoll). Azure-alkalmazások és a külső alkalmazásokban még egyszer érheti el a tár használatával BLOB.
- Azure meghajtók beállítást, amely lehetővé teszi, oldal blob tekinti a helyi meghajtón egy NTFS fájlrendszer Azure alkalmazás használatával. Az alkalmazás a lap blob néz ki egy szabványos fájlírási webböngészőn hétköznapi Windows fájlrendszerben. Erre valójában beolvassa, és elküldi az alapul szolgáló lap blob, amely az Azure-meghajtóra. 

Hardveres hibák szüntetni, és javíthatja az elérhetőség, minden blob-Azure adatközpontban három számítógépei között van replikált. Írás blob frissíti az összes három példányban, így később olvasást nem fogja látni a várttól eltérően működik eredmények. Is megadhatja, hogy egy blob-adatok másolásának az azonos geo azonban legalább 500 mérföld nem vagyok a gépnél egy másik Azure adatközponthoz. A blob frissítés néhány percen belül a másolása, *geo replikációs*nevű történik, és vészhelyreállítás esetében hasznos lehet.

Adatok BLOB is elérhetővé tehetők a Azure *Tartalom kézbesítési hálózati (CDN)*keresztül. Által gyorsítótár tucatnyi kiszolgálók a világon a blob-adatok másolatait, a CDN is való hozzáférés gyorsítása, amely többször érhető el. 

Egyszerű, mint BLOB olyan sok esetben a helyes választás. Tárolásához és a folyamatos átvitelű videó és hang példák egyértelmű, valamint biztonsági másolatok készítése és más típusú adatok archiválása. A fejlesztők BLOB tartása strukturálatlan adatokat őket, mint bármilyen típusú is használhatja. Egy egyszerű módszer tárolására és elérni a bináris adatokat problémákat meglepő hasznos lehet.


## <a name="dbinvm"></a>Egy adatbázis-kezelő egy virtuális gépen futó

Sok alkalmazás ma támaszkodhat valamilyen adatbázis-kezelő rendszer (adatbázis-kezelő). Relációs például SQL Server rendszereket a leggyakrabban használt választási lehetőségek, de nem relációs megközelítés nevükön *NoSQL* technológiák első további népszerű minden nap. Ahhoz, hogy a felhőben alkalmazások kezelése beállításokkal, Azure virtuális gépeken futó lehetővé teszi, hogy egy adatbázis-kezelő futtatását (relációs vagy NoSQL) egy virtuális gép. [2 ábrán](#Fig2) látható, hogy miként ez az SQL Server.

<a name="Fig2"></a>![Az SQL Server egy virtuális gépen diagram][SQLSvr-vm]
 
**Ábra 2: Azure virtuális gépeken futó lehetővé teszi, hogy egy adatbázis-kezelő BLOB által biztosított adatmegőrzési egy virtuális gép rendszerrel.**

A fejlesztők és az adatbázis-rendszergazdák ebben az esetben a következőképpen néz sokkal ugyanazt a szoftvert futtató saját adatközponthoz. Az alábbi példával például, szinte minden SQL Server-funkciók használhatók, és a rendszer teljes rendszergazdai hozzáférése van. Is az adatbázis-kiszolgáló, természetesen kezelése feladata mintha helyileg futó volt.

[2 ábrán](#Fig2) látható, a helyi lemezen a virtuális gép fut a kiszolgáló tárolja az adatbázisok jelennek meg. A magában foglalja a azonban minden e lemezre írása az Azure blob. (Ez hasonlít használata a SAN saját adatközpontban, például egy logikai sokkal eljáró blob.) Bármely Azure blob-az adatokról van replikált háromszor belül egy adatközponthoz és, ha azt egy másik adatközponthoz ugyanabban a régióban geo replikált kér. Akkor is használni szeretné a beállítások, például tükrözés nagyobb megbízhatóság az SQL Server-adatbázishoz. 

Az SQL Server használni egy virtuális úgy, hogy a hibrid-alkalmazás létrehozása, ha az adatok webről Azure az alkalmazás logika a helyszíni futása közben. Például ez lehet értelme Ha egynél több helyen és a különféle mobileszközök alkalmazásokat meg kell osztania ugyanazokat az adatokat. Ahhoz, hogy az a felhő adatbázis és a helyszíni logika egyszerűbb közötti kommunikációt, egy szervezet segítségével Azure virtuális hálózati kapcsolat létrehozása a virtuális magánhálózaton (VPN) között az Azure adatközponthoz és a saját helyszíni adatközponthoz.


## <a name="sqldb"></a>SQL-adatbázis

Sok felhasználó egy adatbázis-kezelő futó egy virtuális az első lehetőséget, hogy az mind a felhőben strukturált adatok kezelésére. Azt nem csak választási, azonban nem, nem, mindig a legjobb választás. Egyes esetekben az adatok platformot használja a szolgáltatás (PaaS) megközelítés kezelése több értelme. Azure SQL-adatbázissal, amely lehetővé teszi a művelet a relációs adatoknak nevezett PaaS technológiát tartalmaz. [Ábra 3](#Fig3) , amely bemutatja ezt a beállítást. 

<a name="Fig3"></a>![SQL-adatbázis ábrája][SQL-db]
 
**Ábra 3: Az SQL-adatbázis egy megosztott PaaS relációs tárhelyszolgáltatáshoz biztosít.**

SQL-adatbázis nem nyújt túl ügyfelek az SQL Server saját fizikai példányát. Több elem bérlői szolgáltatás, ehelyett ügyfelek logikai SQL-adatbázis-kiszolgálóval biztosít. Az összes ügyfelek megosztása számítási és tárolási kapacitása ahhoz, hogy a szolgáltatás. És Blob-tárolóhoz, az összes az adatok SQL-adatbázisban vannak tárolva három ugyanarra a számítógépre belül egy Azure adatközpont, ahol az adatbázisok beépített magas elérhetőség (HA).

Az alkalmazás SQL-adatbázis hasonlít az SQL Server. Alkalmazások SQL lekérdezést a táblák relációs ellen, T SQL tárolt eljárásokkal, és hajtsa végre a tranzakciók több tábla között. Mivel az alkalmazások az táblázatos adatok adatfolyam (TDS) protokollal, ugyanazt a protokollt, SQL Server eléréséhez használt SQL-adatbázis eléréséhez azokat is adatok és feldolgozása entitás keretrendszer, ADO.NET, JDBC és más már jól ismert adatokat az access kapcsolatok. 

De mivel SQL-adatbázis Azure adatközpontokban futó felhőalapú szolgáltatás, nem kell kezelése a rendszer fizikai szempontjait, például lemezterület közül. Még nem kell aggódnia amiatt, hogy a szoftver frissítése vagy egyéb követő felügyeleti feladatok kezelése. Továbbra is a vezérlőket saját adatbázisok, természetesen sémák és a felhasználói bejelentkezések, beleértve minden ügyfél szervezete, de a unalmas felügyeleti feladatok számos végzett meg. 

SQL-adatbázis hasonlít az SQL Server alkalmazások, miközben meg pontosan megegyezik egy adatbázis-kezelő fizikai vagy virtuális gépen futó nem működnek. Megosztott hardver fut, mert a betöltés az összes felelősséget ügyfelei helyezett el, hogy a hardver és a teljesítmény változhat. Ez azt jelenti, hogy a működésére mondjuk SQL-adatbázisban tárolt eljárás eltérőek lehetnek az egynapos a következőre. 

Ma SQL-adatbázis segítségével legfeljebb 150 gigabájt tartó adatbázis. Munka az adatbázisokkal nagyobb van szüksége, ha a szolgáltatás lehetővé a swayek *összevonási*. Ehhez az adatbázis-adminisztrátorhoz két vagy több *összevonási tagok*, amelyek a saját séma használata egy másik adatbázist hoz létre. Adatok kiterjed át ezeket a tagokat, amit gyakran nevezik *sharding*, az egyes tagok rendelt egyedi *összevonási billentyűt*. Az alkalmazás problémák SQL-lekérdezések ellen ezeket az adatokat az összevonási billentyűt, amely azonosítja az összevonási tag a lekérdezés megadásával Megcélozhat kell. Ezzel a hagyományos relációs megközelítés segítségével nagy mennyiségű adatot. Mint mindig amelyek kompromisszumok; lekérdezések sem tranzakciók is kiterjedhet összevonási tagok, például. De relációs PaaS szolgáltatás a legjobb megoldás, és a kompromisszumok elfogadhatók, amikor használata SQL összevonási lehet a megfelelő megoldást.

SQL-adatbázis futó alkalmazások Azure vagy máshol, például a helyszíni adatközpontban is használhatják. A relációs adatok felhőben alkalmazásokra jól használható, valamint a helyszíni alkalmazásokat, amelyek a felhőben adattárolás előnyeivel. Egy mobilalkalmazás előfordulhat, hogy támaszkodhat SQL-adatbázis kezelése a megosztott relációs adatok, például mint elképzelhető, hogy a világ több kereskedők futtató készlet kérelmet.

SQL-adatbázis gondolkodni hatványra egyértelmű (és fontos) probléma: amikor kell futtatja az SQL Server egy virtuális és célszerűbb SQL-adatbázis esetén? Szokás szerint vannak kompromisszumok, és így két megközelítés célszerűbb függ, hogy az igényeknek megfelelően alakíthatja. 

Egy egyszerű foglalkoznia módja SQL-adatbázis megtekintheti, hogy új alkalmazások, miközben egy virtuális SQL Server esetén célszerűbb helyez át egy meglévő helyszíni alkalmazás a felhőbe. Is lehet hasznos lehet ahhoz, hogy tekintse meg ezt a döntési, több egyedi módon azonban. Például SQL-adatbázis használata egyszerűbb, hiszen a minimális beállítási és felügyeleti. De futó SQL Server egy virtuális beállíthatja, hogy több kiszámíthatóan teljesítmény – nem egy megosztott szolgáltatás - és SQL-adatbázis-nál nagyobb nem szövetséges adatbázisok is támogat. Továbbra is SQL-adatbázis biztosít az adatok és a feldolgozás beépített replikációs hatékony, biztosítva egy nagy-elérhetőség az adatbázis-kezelő nagyon kevés munkahelyi. Az SQL Server lehetőséget további és beállítások némileg szélesebb halmazának, SQL-adatbázis is egyszerűbb a beállítása és jelentősen kevesebb munkát kezeléséhez.

Végül fontos, hogy SQL-adatbázis nincs-e az egyetlen PaaS adatok szolgáltatás elérhető Azure felhívja. Microsoft-partnerek, valamint az egyéb beállítások megadása. Például ClearDB felajánlja a MySQL-PaaS kínáló, miközben Cloudant eladja egy NoSQL lehetőséget. PaaS adatszolgáltatások sok esetben a megfelelő megoldást, és így ezt a megközelítést adatkezelés az Azure fontos része.


### <a name="datasync"></a>SQL-adatok szinkronizálása

Közben SQL-adatbázis kezelése belül egy egyetlen Azure adatközponthoz adatbázisonként három példányát, akkor a Azure adatközpontokkal között nem automatikusan bizonyos adatok. Ehelyett biztosít SQL adatszinkronizálás egy szolgáltatás, ezt is használhatja. [Ábra 4](#Fig4) látható, hogy miként ez.

<a name="Fig4"></a>![SQL-adatok szinkronizálása ábrája][SQL-datasync]
 
**Ábra 4: SQL adatok szinkronizálása SQL-adatbázisban adatok szinkronizálása az adatok más Azure-ban és a helyszíni adatközpontokkal.**

A diagramon azt láthatja, ahogy az SQL-adatok szinkronizálása különböző helyeken adatok névjegymappába szinkronizálhatja. Tegyük fel, hogy futtat egy alkalmazást több Azure adatközpont esetén, például SQL-adatbázisban tárolt adatokkal. SQL-adatok szinkronizálása is használhatja, hogy az adatok szinkronizált állapotának fenntartása. SQL-adatok szinkronizálása is is adatszinkronizálás egy Azure adatközponthoz és az SQL Server-helyszíni adatközpontban fut egy példánya. Ez lehet hasznos, ha egy helyi példányt az adatok helyszíni alkalmazások által használt és a felhő másolatának Azure futó alkalmazások által használt fenntartása. És bár nem az ábrán látható, az SQL-adatok szinkronizálása is használható adatszinkronizálás SQL-adatbázis és az SQL Server Azure vagy máshol egy virtuális futó.

Szinkronizálás kétirányú, és meghatározhatja, hogy pontosan milyen adatokat szinkronizálja a rendszer, és milyen gyakran történik. (Atomi nem az adatbázisok közötti szinkronizálást, azonban - és mindig legalább néhány késleltetés.) Pedig használja, de adatszinkronizálás SQL-szinkronizálás beállítása teljesen konfigurációs-alapú; Nincs írni kód.


### <a name="datarpt"></a>SQL adatszolgáltatás virtuális gépeken futó használatával

Adatbázis adatokat tartalmaz, amikor valaki célszerű fog adatokat használó jelentések létrehozásához. Azure futtatását is lehetővé teszi az SQL Server Reporting Services (SSRS) az Azure virtuális gépeken futó, amely megegyezik az futó helyszíni SQL Server Reporting Services. Kattintson a Azure SQL-adatbázisban tárolt adatok jelentések futtatása SSRS is használhatja.  [Ábra 5](#Fig5) jeleníti meg, hogyan működik a a folyamat.

<a name="Fig5"></a>![SQL-jelentés ábrája][SQL-report]
 
**Ábra 5: Az SQL Server Reporting Services fut az Azure virtuális gépeken futó SQL-adatbázisban tárolt adatok jelentéskészítési szolgáltatásokat nyújt. .**

A felhasználók láthatják a jelentést, mielőtt valaki határozza meg, mi a jelentés így néz (lépés: 1). Kattintson egy virtuális SSRS, az ehhez két eszközök valamelyikével: SQL Server Data Tools, SQL Server 2012 részét vagy a megelőző tevékenység Business Intelligence (üzleti Intelligencia) Development Studio. SSRS, az alábbi jelentés definíciók a jelentés Definition Language (RDL) kell megadni. A jelentés RDL-fájlok létrehozása után azok egy virtuális (lépés: 2) a felhőben van feltöltve. A jelentés definícióját készen áll a használata.

Ezután az alkalmazás felhasználó fér hozzá a jelentés (3 lépés). Az alkalmazás a kérelem átadja a SSRS virtuális (lépés: 4), amely a partnerlista, szüksége van az adatok beolvasása SQL-adatbázis vagy más adatforrásokból (5 lépés). SSRS használja ezeket az adatokat, és jeleníti meg a a jelentés (6 lépés), majd a megfelelő RDL fájlokat a jelentés visszatér az alkalmazás (7 lépés), amely megjeleníti a felhasználó (lépés 8).

Jelentés beágyazása egy alkalmazást, az alkalmazási példát ábrán, nem csak. Érdemes emellett jelentések megtekintése a Jelentéskezelőből a virtuális, a virtuális a SharePoint vagy más módokon is. Jelentések is is lehet kombinálni, egy jelentést, amely tartalmazza a másikra mutató hivatkozást.

Kattintson az Azure virtuális SSRS lehetővé teszi a felhőben jelentéskészítési megoldásként teljes funkcionalitását. Jelentések bármely SSRS által támogatott adatforrás használható. Alkalmazások és a jelentések beágyazott kód vagy a támogatási egyéni viselkedése összeállítások lehetnek. Jelentés végrehajtását és megjelenítését gyorsak, mert a kiszolgáló tartalom bejelentése és motor közös kiszolgálón futó azonos virtuális.



## <a name="tblstor"></a>Táblatároló

Relációs adatok sok esetekben lehet hasznos, de még nem mindig a helyes választás. Ha az alkalmazás nagyon nagy mennyiségű módszerektől strukturált adatok gyors és egyszerű hozzáférésre van szüksége, például egy relációs adatbázisban nem működik jól. Egy NoSQL technológia valószínűleg jobb választás.

Azure Táblatárolóhoz NoSQL megközelítés ehhez hasonló látható. Ellentétben a nevét Táblatároló szabványos relációs táblák nem támogatja. Ehelyett biztosít, úgynevezett *kulcs/érték tárolása*, adatok társítása egy adott billentyű, majd az alkalmazás hozzáférést engedélyezem adatok, mert az a billentyűt. [Ábra 6](#Fig6) alapvető mutatja be.

<a name="Fig6"></a>![Alkalmazza a táblatároló][SQL-tblstor]
 
**Ábra 6: Azure Táblatárolóhoz a kulcs/érték üzlet gyors, nagy mennyiségű adatot egyszerű hozzáférést biztosít.**

BLOB, például táblázatokat Azure tároló fiók társítva. Táblázatok vannak más néven sokkal például BLOB az űrlap URL

http://&lt;*StorageAccount*&gt;.table.core.windows.net/&lt;*táblanév*&gt;

Az ábrán az látható, mint táblákra oszlik néhány partíciók, másik számítógépre is, amelyek tárolhatók száma. (Ez a sharding, az űrlap SQL összevonási a.) Azure-alkalmazások és a futó máshol alkalmazások érheti el vagy RESTful OData protokollt, vagy az Azure tárolási ügyfél tár tartalmazó táblázatot.

Egy táblázat minden egyes partíciót néhány *szervezetek*, akár 255 *tulajdonságait*tartalmazó számát tartalmazza. Minden a tulajdonságnak nevét, a (például a bináris, a logikai, a dátum és idő, a Int vagy a karakterlánc) típusú és egy értéket. Eltérően relációs tárolásra az alábbi táblázat a rögzített séma van, és a táblázatból, így más szervezetek tulajdonságainak különböző típusú is tartalmazhat. Előfordulhat, hogy egy személy nevét, például tartalmazó, míg a egy másik személyhez a táblázatból, amelyben egy ügyfél-azonosító és hitelképesség két Int tulajdonságok karakterlánc tulajdonság.

Az alkalmazások azonosítása egy táblázaton belül egy adott személyhez, biztosít, adott entitás billentyűt. A kulcs két részből áll: *partíciót kulcs* , amely azonosítja az egy adott partíciót és, amely azonosítja, hogy entitás *sor billentyűt* . Az [ábrán 6](#Fig6)például az ügyfél partíciót egy és 3 sor kulcs egyed kéri, és Táblatároló ad eredményül adott egységek, beleértve az a tulajdonságokat tartalmaz.

Ennél a módszernél lehetővé teszi, hogy a nagy táblázatok – egy táblában legfeljebb 100 terabájt adatok - is tartalmazhat, és lehetővé teszi a gyors hozzáférést az adatokat tartalmaznak. Azt is elérhetővé teszi korlátozások, akkor jó helyen jár. Ha például nem nem támogatja a táblák vagy akár partíciót egy táblában átterjedő tranzakció alapú frissítéseket. Táblázat frissítések csoportja csak is csoportosítva egy atomi tranzakcióinak, ha az összes résztvevő vállalkozások a azonos partíciót a. Nincs is nincs mód a táblázat a lekérdezés alapján tulajdonságainak értékét, sem van-e támogatás illesztések a több tábla között. És relációs adatbázisoktól eltérően, táblák nem támogatja a tárolt eljárásokat.

Azure Táblatárolóhoz érdemes használni az alkalmazásokat, amelyek a nagy mennyiségű módszerektől strukturált adatok gyors, olcsó hozzáféréssel kell rendelkeznie. Például egy internetes alkalmazás felhasználók rengeteg felhasználóiprofil-adatok tárolására szolgáló táblák használhat. Gyors hozzáférést fontos, ebben az esetben, és az alkalmazás valószínűleg nem kell a teljes power az SQL. Sebességétől és a méret szerezhet be ezt a funkciót biztosítva is előfordul, hogy értelme, és így Táblatároló csak bizonyos problémákat jobb megoldás.


## <a name="hadoop"></a>Hadoop

Szervezetek adatraktárakhoz évtizedek van már épület. Ezek a leggyakrabban a relációs táblákban tárolt információ, gyűjtemények tudathatja a használata, és ismerje meg az adatokat, hogy számos különböző módon. SQL Server-például általában eszközök, például SQL Server Analysis Services használni ehhez.

De tegyük fel, hogy nem relációs adatok elemzéseket végezni szeretné. Az adatok sok űrlapok igénybe: a diagnosztikai eszközök orvosi és más képek érzékelők vagy RFID címkék, kiszolgálófarm clickstream adatok webalkalmazások által létrehozott naplófájlok adatait. Ezeket az adatokat is nagyon nagy, túl nagy a hagyományos adatraktár hatékony használható. Nagy adatok hasonló, ritka csak néhány évvel ezelőtti megjelenésével, most igazán közös váltak.

Az ilyen típusú nagy adatok elemzéséhez a iparágban nagymértékben közelítéssel a probléma megoldásán egyetlen: a nyitott-forrás technológia Hadoop. A virtuális gépeken futó, vagy a fizikai fürthöz Hadoop fut, terjesztése az adatok adott gépek között működik, és ezzel párhuzamosan feldolgozásával. A további gépek Hadoop van szüksége, minél gyorsabban azt követően is függetlenül működik, akkor működik.

Ez a probléma típusú egy nyilvános felhőbeli természetes megfelelő. Hanem egy helyszíni kiszolgálók, előfordulhat, hogy ülnie üresjárati nagy, az idő, Hadoop fut a felhőben lehetővé teszi, hogy hozzon létre (és díja) hadsereg VMs fenntartása, csak akkor, ha szüksége van rá. Még jobb további és további nagy Hadoop az elemezni kívánt adatokat jön létre a felhőben menti, akkor a beszúrási pont áthelyezése a problémát. Segít kihasználhatja az alábbi együttműködést, Microsoft Ez a témakör Hadoop szolgáltatás Azure. [Ábra 7](#Fig7) jeleníti meg ezt a szolgáltatást a legfontosabb összetevői.

<a name="Fig7"></a>![Hadoop ábrája][hadoop]

**Ábra 7: Azure a Hadoop fut, adatfeldolgozás használatával több virtuális gépeken futó párhuzamosan MapReduce feladatokat.**

Azure Hadoop használatához először megkéri a felhőben platform, Hadoop fürt létrehozásához szükséges VMs számának megadása. A Hadoop fürt beállítása saját kezűleg egy nem trivial feladatot, és Azure adunk meg, hogy van ilyesmire lehetőség. Ha végzett a fürt használ, akkor leállíthatja azt. Nincs szükség az fizetés számítási erőforrások, amely nem használ.

Hadoop-alkalmazást, más néven egy *feladatot*, vagyis *MapReduce*programozási modellt használ. Mint az ábrán az látható, a logika MapReduce feladat egyidejű VMs számos különböző fut. Adatok párhuzamosan feldolgozása, Hadoop elemezheti adatok sokkal több gyorsan, mint egyetlen-gép megoldásokat.

Azure, kattintson az adatok egy feladat működése MapReduce általában legyen blob-tárolóban lévő. A Hadoop azonban MapReduce feladatok a várt adatokat tárolja a *Hadoop elosztott fájl rendszer (hdfs) lehetőségre*. Fájlrendszerhez hasonlít Blob-tárolóhoz bemutatják, hogyan; egymás között másolják adatok több fizikai kiszolgálóin például. Helyett a duplikálás ezt a funkciót, a Azure Hadoop inkább közzététele Blob-tárolóhoz az Fájlrendszerhez API-k, az ábrán látható módon. A logika MapReduce feladatban lapon közönséges Fájlrendszerhez fájlok hozzáférhet, miközben a feladat valójában használata adatok BLOB-a részére. És a támogatási az esetben, ha több feladat futtatása fölé ugyanazokat az adatokat, a Azure Hadoop is lehetővé teszi BLOB adatokat másol be a teljes Fájlrendszerhez fut a VMs. 

MapReduce feladatok is gyakran Java nyelven írt ma, a Azure Hadoop támogató megközelítés. A Microsoft is felvette MapReduce feladatok létrehozása más nyelven, beleértve a C#, F # és JavaScript támogatása. A cél, elérhetővé a nagy adatok technológia könnyebben a fejlesztők egy nagyobb csoportot.

Fájlrendszerhez és MapReduce Hadoop egyéb technológiákat, amely lehetővé teszi az adatok elemzése a MapReduce feladat maguk írása nélkül személyek is tartalmaz. Malac például az adatkeresési nagy, miközben struktúra kínál HiveQL nevű SQL-hasonló nyelvek tervezett magas szintű nyelven is. Malac és a struktúra tényleges Fájlrendszerhez adatfeldolgozás MapReduce feladatok létrehozásához, de azok elrejtése összetettség a felhasználóktól. Mind a Azure Hadoop kapott.

A Microsoft Excel is biztosít egy HiveQL eszközillesztőt. Egy Excel beépülő modul használatával üzleti elemzők létrehozhat HiveQL lekérdezések (és így MapReduce feladatok) közvetlenül az Excelben, majd folyamat, és jelenítse meg az eredményeket, a PowerPivot és a más Excel-eszközök használatával. Azure a Hadoop, valamint egyéb technológiákat tartalmaz, például a gépi tanulási Mahout tárak, a diagram adatbányászati rendszer Pegasus és az egyéb.

Nagy adatelemzés fontos, és így Hadoop is fontos. Mert a Hadoop szolgáltatásként felügyelt Azure, a megszokott eszközök, például az Excel programban mutató hivatkozásokat találhat a Microsoft célja tétele a felhasználók szélesebb csoportja számára hozzáférhető e technológia.

Adatok mindenféle szélesebb körben fontos. Éppen ezért Azure körét kínálja adatok kezelési és üzleti elemző lehetőségeit. Bármilyen alkalmazás próbál létrehozni, akkor valószínű, hogy megtalálja valamit, amit az a felhő platform működnek.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
