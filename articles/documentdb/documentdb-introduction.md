<properties 
    pageTitle="Bevezetés a DocumentDB, JSON adatbázis |} Microsoft Azure" 
    description="További tudnivalók: Azure DocumentDB, NoSQL JSON adatbázis. A dokumentum adatbázis nagy adatok, a rugalmas méretezhetőség és a magas elérhetősége épül." 
    keywords="JSON-adatbázisban, dokumentum-adatbázis"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Bevezetés DocumentDB: NoSQL JSON adatbázis

##<a name="what-is-documentdb"></a>Mi az DocumentDB?

DocumentDB olyan gyors és kiszámítható teljesítmény, nagy elérhetőségét, rugalmas méretezés, globális terjesztési és fejlesztési Kezeléstechnikai épített teljes körű felügyelt NoSQL adatbázis szolgáltatás. Adatbázisként séma ingyenes NoSQL DocumentDB biztosít, gazdag és a jól ismert SQL lekérdezési lehetőségek JSON adatok - biztosítva, hogy az olvasás 99 %-os szolgáltatott a 10 ezredmásodperc és a írások 99 %-os egységes alacsony késések szolgáltatott ezredmásodperc alatt 15. Ezek egyedi előnyöket kezdeményezése DocumentDB egy nagy alkalmas webes mobil, játék, és IoT és sok más alkalmazások zökkenőmentes méretezése és globális replikációs van szükség.

## <a name="how-can-i-learn-about-documentdb"></a>Hol találok DocumentDB kapcsolatban? 

Egy gyorsan DocumentDB tudnivalók és a funkció működés közben, kövesse ezeket a lépéseket: 

1. A két perc megtekintés [DocumentDB Újdonságok?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) videó bemutatja a DocumentDB használatának előnyeit.
2. Megtekintés az a három percet [DocumentDB létrehozása az Azure a](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) videó, amely kezdőlépéseket kell megtennie a DocumentDB az Azure portál használatával.
3. Látogasson el a [Lekérdezés játszótéri](http://www.documentdb.com/sql/demo), ahol is végigvezetik másik tevékenységeket, ha többet szeretne tudni a multimédiás lekérdezésekor funkcionalitást DocumentDB. Ezután a védőfalas lapra fölé központi és saját egyéni SQL-lekérdezések futtatása, és DocumentDB kísérletezhet.

Ezt követően lépjen vissza Ez a cikk hol azt fogja alaposabban.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Milyen funkciókat és a fontosabb funkciói nem DocumentDB ajánlja fel?  

Azure DocumentDB kínál az alábbi főbb funkciók és előnyökkel jár:

-   **Elastically méretezhető átviteli és tárolási:** Egyszerűen méretezése, vagy le az alkalmazás igényekhez DocumentDB JSON-adatbázis méretezni. Az adatok lemezen tárolja a folytonos állapota (SSD) a alacsony kiszámíthatóan késések. DocumentDB gyakorlatilag korlátlan tárhely méretét és kiépített átviteli méretezheti gyűjtemények nevű JSON adattárolás tárolók támogatja. Elastically méretezheti DocumentDB kiszámíthatóan teljesítménnyel zökkenőmentes az alkalmazás növekedésével. 

-   **Több területre replikáció:** DocumentDB átlátszó replikálja az adatok minden terület, már a DocumentDB fiókjához tartozó, úgy közben konzisztencia, az elérhetőség és a teljesítmény elérése érdekében az összes megfelelő garanciákkal között kompromisszumok nyújtó globális hozzáférés az adatokhoz igénylő alkalmazások fejlesztéséhez. DocumentDB átlátszó területi feladatátvevő a több elem webhelykezelési API-k és az azt jelenti, hogy átviteli és tárolása elastically méretezheti át a földgömb biztosít. Megtudhatja, hogy több az [adatok globálisan DocumentDB terjesztése](documentdb-distribute-data-globally.md).

-   **Alkalmi lekérdezések jól ismert SQL-szintaxisa:** Eltérő JSON dokumentumok tárolására DocumentDB belül és a lekérdezés ezeket a dokumentumokat – már jól ismert SQL-szintaxisa. DocumentDB ingyenes erősen egyidejű lakat használja, a napló strukturált automatikusan indexelni az összes dokumentumtartalom indexelési eljárást. Ezzel a multimédiás valós idejű lekérdezések séma javaslatok, a másodlagos indexek és a nézetek megadása nélkül. További [Lekérdezés DocumentDB](documentdb-sql-query.md). 

-   **JavaScript végrehajtása az adatbázisban:** Express-alkalmazás logika tárolt eljárások, eseményindítók és a felhasználó által definiált függvények (UDF) szabványos JavaScript használatával. Ez lehetővé teszi, hogy az alkalmazás logika anélkül, hogy az alkalmazás és az adatbázis sémája közötti eltérés mutasson az adatokra működjön. DocumentDB biztosít a teljes tranzakció alapú végrehajtását JavaScript alkalmazás logika közvetlenül a adatbázismotor belül. A JavaScript-mély integrációt lehetővé teszi, hogy a Beszúrás, a csere, törlés és JavaScript-programon belül a VÁLASZTÓ műveletek végrehajtása elszigetelt tranzakcióként. További [DocumentDB kiszolgálóoldali](documentdb-programming.md)programozáshoz.

-   **Hangolható konzisztencia szintek:** Négy jól meghatározott konzisztencia szintek optimális csökkentés közötti összhangot, és a teljesítmény elérése érdekében jelölje ki. A lekérdezések és olvasási műveletek DocumentDB kínál négy különböző konzisztencia szintek: erős, határolt staleness, munkamenetet, és az esetleges. Ezek a részletes, pontosan meghatározott konzisztencia szintek teszi lehetővé, hogy a hang kompromisszumok összhangot, elérhetőségét és időtartama között. További [konzisztencia szintek maximális elérhetőség és a teljesítmény DocumentDB használatával](documentdb-consistency-levels.md).

-   **Teljes körű felügyelt:** Feleslegessé adatbázis- és erőforrások kezelésére. Egy teljes körű felügyelt Microsoft Azure szolgáltatás, nem kell a virtuális gépeken futó kezelése, telepítése, és állítsa be a szoftver, kezelése a méretezés vagy kezelni az összetett adatok szintű frissítések. Minden adatbázisban automatikusan a biztonsági mentésben és területi hibák ellen védett. Egyszerűen DocumentDB fiók hozzáadása és kiépítése a beosztását, hogy, így kiemelése az alkalmazás nem működik, és az adatbázis kezelése. 

-   **Terv kattintva nyitható meg:** Első lépések gyorsan meglévő ismeretek és eszközök használatával. Programozási elleni DocumentDB egyszerű, udvariasan, és nem szükséges új eszközök elfogadja vagy egyéni bővítések JSON vagy JavaScript igazodjon. Hozzáférhet az adatbázis funkciókat, például CRUD, a lekérdezés és a JavaScript feldolgozása egy egyszerű RESTful HTTP-kapcsolaton keresztül. DocumentDB meglévő formázás, nyelvek és szabványai-mérőórákat foglal magába kínálja a magas érték adatbázis funkciók fölött őket.

-   **Automatikus indexelés:** Alapértelmezés szerint DocumentDB [automatikusan indexek](documentdb-indexing.md) az adatbázis összes dokumentumot, és nem várt vagy igényel séma vagy a másodlagos indexek létrehozása. Nem minden index szeretne? Ne aggódjon, érdemes [a JSON-fájlok az elérési út lemondása](documentdb-indexing-policies.md) is.

##<a name="data-management"></a>Hogyan DocumentDB adatok kezelése?

Azure DocumentDB kezeli jól definiált adatbázis-erőforrások JSON adatoknak. Ezek az erőforrások magas elérhetőség replikált és a logikai URI egyedi címmel rendelkező. DocumentDB felajánlja a különféle egy egyszerű HTTP-alapú RESTful programozási modell összes erőforrás. 

Az adatbázis-fiókot DocumentDB férhet hozzá az Azure DocumentDB egyedi névtér. Létrehozhat egy adatbázis-fiókot, mielőtt az Azure előfizetéssel, és férhet hozzá az Azure szolgáltatások számos kell rendelkeznie. 

DocumentDB belül minden erőforrás modellezni és JSON dokumentumot tárolja. Erőforrások kezelése elemeket, amelyeket JSON dokumentumok metaadatokat tartalmazó, és a hírcsatornákat, amelyek elemgyűjteményeket. A megfelelő hírcsatornák elemkészletek található.

Az alábbi képen az DocumentDB erőforrások közötti kapcsolatokat ismerteti:

![DocumentDB, NoSQL JSON adatbázis erőforrások hierarchikus viszonya][1] 

Egy adatbázis-fiókot, ahol a egy sor olyan, adatbázisokat több gyűjtemények, amelyek tartalmazhatnak, tárolt eljárások, eseményindítók, UDF, dokumentumokat és kapcsolódó mellékleteket tartalmazó. Adatbázis van társítva mindegyike különböző más webhelycsoportok, tárolt eljárások, eseményindítók, UDF, dokumentumok vagy mellékletek hozzáférési jogosultsággal rendelkező felhasználók. Adatbázisok, a felhasználók, az engedélyek és a gyűjtemények rendszer által definiált erőforrások jól ismert sémával - dokumentumok, tárolt eljárások, eseményindítók, UDF és mellékletek tetszőleges tartalmaz, amíg a felhasználó által definiált JSON tartalmat.  

##<a name="develop"></a>Hogyan fejleszthet DocumentDB az alkalmazások?

Azure DocumentDB közzététele erőforrásokat a REST API-t, bármelyik alkalmas az, hogy a HTTP-/ HTTPS-kérések nyelv hívható keresztül. Emellett a DocumentDB felajánlja a különféle programozási tárak számos népszerű nyelvhez. A tárak egyszerűsítése számos tulajdonságát Azure DocumentDB használata kezelésével részleteket, például a gyorsítótárazás címet, kivétel kezelése, automatikus próbálkozások és így tovább. Tárak jelenleg érhetők el az alábbi nyelvek és platformokon:  

Letöltés | Dokumentáció
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET-tárban](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[NODE.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [NODE.js tárban](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-tárban](http://azure.github.io/azure-documentdb-java/)
[A JavaScript SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-tárban](http://azure.github.io/azure-documentdb-js/)
a #hiányzik | [Kiszolgálóoldali JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python tárban](http://azure.github.io/azure-documentdb-python/)

Túl egyszerű létrehozása, olvasása, módosítása és törlése műveletek, DocumentDB felületet biztosít a multimédiás SQL lekérdezési JSON dokumentumok beolvasása és kiszolgálóoldali támogatása a JavaScript alkalmazás logika tranzakció alapú végrehajtását. A lekérdezés és parancsfájl végrehajtás felületeken keresztül tárakat platform, valamint a REST API-k érhetők el. 

### <a name="sql-query"></a>SQL-lekérdezés
Azure DocumentDB támogatja a lekérdezés dokumentumokat, amelyek gyökerezik a JavaScript-típus rendszer, és a kifejezések támogatásával relációs, hierarchikus és térbeli lekérdezések SQL nyelvet használ. DocumentDB lekérdezési nyelv egy egyszerű, mégis hatékony felület lekérdezés JSON dokumentumok. A nyelv támogatja az ANSI SQL nyelvhelyesség-ellenőrzés csak egy részhalmazát, és hozzáadja a mély integrációja JavaScript objektum, tömbök, objektum építés és a hívás függvény. DocumentDB lekérdezés modelljének bármely explicit séma és az indexelő útmutatók nélkül az fejlesztője teszi lehetővé.

A felhasználó által definiált függvények (UDF) is DocumentDB regisztrált, és egyúttal bővítése a nyelvhelyesség-ellenőrzés a támogatási egyéni alkalmazás logika hivatkozott egy SQL-lekérdezés részeként. Ezek UDF JavaScript programként írt, és az adatbázis belül végre. 

A .NET fejlesztők számára DocumentDB is kínál a [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)részeként LINQ lekérdezés szolgáltató. 

### <a name="transactions-and-javascript-execution"></a>Tranzakciók és a JavaScript végrehajtása
DocumentDB alkalmazás logika beírása csupán a JavaScript írt elnevezett programok teszi lehetővé. Ezeket a programokat gyűjtemény regisztrált, és egy adott webhelycsoport belül a dokumentumokon az adatbázis-műveletek állíthatnak. A JavaScript az eseményindító, tárolt eljárás vagy felhasználó által definiált függvény végrehajtása lehet regisztrálni. Eseményindítók és a tárolt eljárásokat is létrehozása, olvasása, frissítése és törlése a dokumentumok, mivel a felhasználó által definiált függvények végrehajtása lekérdezés-végrehajtási logikájának nélkül írási hozzáféréssel a webhelycsoport részeként.

JavaScript-végrehajtási belül DocumentDB után a relációs adatbázisok rendszerek, modern helyettesíti a Transact-SQL nyelvben JavaScript által támogatott fogalmak modellezni. Minden JavaScript logikájának végrehajtása pillanatkép elkülönítési környezeti savas tranzakción belül. A végrehajtás során Ha a JavaScript okoz a kivétel, majd a teljes tranzakció megszakad.

## <a name="next-steps"></a>Következő lépések
Már rendelkezik egy Azure-fiók? Majd meg tudják elkezdeni használni az [Azure-portálon](https://portal.azure.com/#gallery/Microsoft.DocumentDB) DocumentDB: [Hozzon létre egy DocumentDB adatbázis-fiókot](documentdb-create-account.md).

Nincs Azure-fiókja? képes vagy:

- Fizessen elő az [Azure ingyenes próbaverziót](https://azure.microsoft.com/free/), amely lehetővé teszi 30 nap, és próbálja ki az Azure szolgáltatások 200 $. 
- Az MSDN-előfizetés birtokában [az ingyenes Azure credits havonta 150 $](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) használatára jogosult bármely Azure szolgáltatás használata. 

Ezután amikor készen áll a további, látogasson el a [tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/documentdb/) keresse meg az összes elérhető tanulási források. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
