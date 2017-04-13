<properties 
    pageTitle="DocumentDB adatbázis kérdések – gyakori kérdések |} Microsoft Azure" 
    description="Azure DocumentDB kapcsolatos gyakori kérdésekre választ kaphat JSON NoSQL dokumentum adatbázis szolgáltatást. Adatbázis kérdésekre beosztását, teljesítményszint és méretezését." 
    keywords="Adatbázis-kérdések gyakran ismételt kérdések, documentdb, azure, a Microsoft azure"
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
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Gyakori kérdések a DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Adatbázis kérdések a Microsoft Azure DocumentDB alapjai

### <a name="what-is-microsoft-azure-documentdb"></a>Mi az Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB a egy blazing gyors, bolygónk nagyméretű NoSQL dokumentum adatbázis-,-a-szolgáltatás, amely biztosít a multimédiás lekérdezése mutasson az adatokra séma ingyenes, konfigurálható és megbízható teljesítményt nyújtsa segít, és lehetővé teszi, hogy gyors fejlesztése, biztosítása egy felügyelt platform mögött a power elérje a a Microsoft Azure. DocumentDB a webes mobil, játék jobb megoldás, és IoT alkalmazások kiszámíthatóan átviteli, magas elérhetőségét, alacsony időtartama és séma ingyenes adatmodell esetén főbb követelményeknek. DocumentDB séma rugalmasság és a JSON natív adatmodell használatával az indexelés rich biztosítja, és többdokumentumos tranzakció-támogatás az integrált JavaScript tartalmazza.  
  
A több adatbázis kérdésekre a válaszok és üzembe helyezése, és ezzel a szolgáltatással, útmutatást [DocumentDB dokumentáció lapon](https://azure.microsoft.com/documentation/services/documentdb/)talál.

### <a name="what-kind-of-database-is-documentdb"></a>Milyen típusú adatbázist az DocumentDB?
DocumentDB NoSQL dokumentum tájolását adatbázis adatokat tároló JSON formátumban.  DocumentDB automatikus contained adatokat, beágyazott struktúrákat, amely egy gazdag [SQL lekérdezési nyelvhelyesség-ellenőrzés](documentdb-sql-query.md)DocumentDB kérdezhetők támogatja. DocumentDB nagy teljesítményű tranzakció alapú feldolgozása kiszolgálóoldali JavaScript [tárolt eljárások, eseményindítók, és a felhasználó által definiált függvények](documentdb-programming.md)keresztül biztosít. Az adatbázis támogatja a fejlesztői hangolható konzisztencia szintek társított [teljesítményszint](documentdb-performance-levels.md)is.
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>DocumentDB adatbázisok rendelkeznek a táblázatok, például egy relációs adatbázisban (RDBMS)?
DocumentDB nem, a dokumentumok JSON tartozó tárolja az adatokat.  DocumentDB erőforrásokat a további tudnivalókért lásd [DocumentDB erőforrás modell és fogalmak](documentdb-resources.md). Hogyan különböznek az NoSQL megoldások DocumentDB például relációs megoldások kapcsolatos további tudnivalókért olvassa el a [NoSQL viewben SQL](documentdb-nosql-vs-sql.md)című témakört.

### <a name="do-documentdb-databases-support-schema-free-data"></a>Támogatják DocumentDB adatbázisok séma ingyenes adatok?
Igen, DocumentDB lehetővé teszi, hogy alkalmazásokat sémadefiníciója és útmutatók nélkül tetszőleges JSON dokumentumok tárolásához. Adatok érhető el közvetlenül a DocumentDB SQL-lekérdezés felületen lekérdezés.   

### <a name="does-documentdb-support-acid-transactions"></a>Támogatja a DocumentDB savas tranzakciók?
Igen, DocumentDB támogatja a dokumentumok közötti tranzakciók JavaScript tárolt eljárások és indítók kifejezve. Tranzakciók belül minden egyes webhelycsoport egyetlen partíciót hatóköre, és az összes savas szemantikáját végrehajtott vagy semmi elszigetelt más egyidejű végrehajtása kód és a felhasználói kérések.  Kivételek a JavaScript-kód alkalmazás kiszolgáló egymás végrehajtását keresztül is elő, ha a teljes tranzakció visszaáll. Tranzakciók kapcsolatos további tudnivalókért olvassa el a [program az adatbázis-tranzakciók](documentdb-programming.md#database-program-transactions)című témakört.

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Mik azok a jellemző használata esettel DocumentDB?  
DocumentDB érdemes használni az új webhely mobil, játék, és hol automatikus méretezés kiszámíthatóan teljesítmény elérése érdekében gyors ezredmásodperc válaszidő sorrendjét, és az azt jelenti, hogy a lekérdezés mutasson az adatokra séma ingyenes IoT alkalmazások fontos. DocumentDB házirendkezelő gyors fejlesztés és a folyamatos példányaiban alkalmazás adatmodellek támogatása. Alkalmazások, amelyek a felhasználói létrehozott tartalmat, és adatok kezelése [DocumentDB közös használata esettel](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Hogyan DocumentDB kiszámíthatóan teljesítményt nyújtanak?
[Kérés egység](documentdb-request-units.md) méri DocumentDB az átvitel. 1 Licencelési 1KB dokumentum kérése a kapacitásának felel meg. Minden DocumentDB művelet, Olvasás, írások, SQL-lekérdezések és tárolt eljárás végrehajtások a művelet befejezéséhez szükséges a teljesítmény alapján mérvadó Licencelési értéket tartalmaz Processzor, IO és memóriát, és milyen mindegyik hatással van az alkalmazás átviteli gondolkodni helyett érdemes elképzelnie pedig egy egyetlen Licencelési mértéket.

Minden egyes DocumentDB gyűjtemény a értelmez RUs átviteli másodpercenként a kiépített átviteli foglalható. Alkalmazások bármely skála meg is teljesítményteszt egyes kérések felmérni, a Licencelési értékeket és rendelkezést gyűjtemények kezeléséhez kérelem egységek összegeként összes kérések között. Is méretezheti vagy a webhelycsoport átviteli lefelé, az alkalmazás evolve szükségleteinek méretezni. Kérés egységek további információt és segítséget annak megállapítása, a webhelycsoport van szüksége, olvassa el [kezelése teljesítmény- és kapacitásbeli](documentdb-manage.md) és próbálja meg a [átviteli Számológép](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Az DocumentDB HIPAA kompatibilis?
Igen, a DocumentDB kompatibilis HIPAA. HIPAA létesít közzétételét, használatára vonatkozó követelmények és megőrzése egyenként azonosítható rendszerállapot adatait. További tudnivalókért lásd: a [Microsoft adatvédelmi központ](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Mik azok a tárterületkorlátok a DocumentDB? 
Nem tárolható egy webhelycsoport DocumentDB az adatok mennyiségét elméleti nincs korlátozva. Ha szeretné, hogy több mint 250 GB-nyi egyetlen gyűjtemény adatainak tárolására, állítsa a [Kapcsolatfelvétel az ügyfélszolgálattal](documentdb-increase-limits.md) , hogy a fiók kvóta nagyobb. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Mik azok a DocumentDB átviteli határain? 
Támogató gyűjtemény is DocumentDB, átviteli mennyiségét elméleti nincs korlátozva van, ha a terhelést terjeszthetők nagyjából egyenlő méretű darabot elég sok termékkulcsok között. Ha ki szeretne meghaladja a 250 000 kérhet erőforrás-mennyiség/második felhasználónkénti webhelycsoport vagy a fiókot, tájékoztassa a [Kapcsolatfelvétel az ügyfélszolgálattal](documentdb-increase-limits.md) , hogy a fiók kvóta nagyobb. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Microsoft Azure DocumentDB mennyibe kerül?
Az [DocumentDB árak részletei](https://azure.microsoft.com/pricing/details/documentdb/) lapon, a részletekért olvassa el. DocumentDB használati költségek határozzák gyűjtemények használja, a gyűjtemények korábban online állapotban, órák számát a szám és a felhasznált tárolása és az egyes kiépített kapacitása. 

### <a name="is-there-a-free-account-available"></a>Van-e egy ingyenes fiókra elérhető?
Ha nem Azure, jelentkezzen a [Azure ingyenes fiókot](https://azure.microsoft.com/free/), amely lehetővé teszi 30 nap, és próbálja ki az Azure szolgáltatások 200 $. Vagy ha egy Visual Studio-előfizetést, [az ingyenes Azure credits havonta 150 $](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) használatára jogosult bármely Azure szolgáltatás használata.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Hogyan kaphatok további segítséget DocumentDB?
Ha segítségre van szüksége bármelyik, keresse meg a kapcsolatfelvételi a [Túlcsordulás Papírhalom](http://stackoverflow.com/questions/tagged/azure-documentdb), a [Azure DocumentDB MSDN fejlesztői fórumok](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), vagy [mérnöki DocumentDB csapattal 1:1 Csevegés](http://www.askdocdb.com/)ütemezése. Naprakész állapot legújabb DocumentDB híreket és szolgáltatásainak, kövesse az us a [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB beállítása

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Hogyan tudom-előfizetés a Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB érhető el az [Azure-portálon][azure-portal].  Először meg kell regisztráció Microsoft Azure előfizetéshez.  Amikor regisztrál az előfizetés a Microsoft Azure, akkor is egy fiókot DocumentDB Azure-előfizetéséhez. DocumentDB fiók hozzáadása, tanulmányozza [a DocumentDB adatbázis-fiók létrehozása](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Mi az a fő kulcs?
A diaminta kulcsa a biztonsági jogkivonat összes erőforrás-fiók elérésére. Kulccsal személyek van és olvasási hozzáférést az összes erőforrásában található az adatbázis-fiókot. Legyen óvatos, amikor fő billentyűk terjesztése. Az elsődleges kulcs és a másodlagos fő kulcs érhetők el a **billentyűk **lap a [Azure portál][azure-portal]. Kapcsolatos további tudnivalókért olvassa el a [nézet, másolás, és újragenerálása hívóbetűk](documentdb-manage-account.md#keys)című témakört.

### <a name="how-do-i-create-a-database"></a>Hogyan hozhatok létre adatbázis?
Adatbázisait az [Azure-portálon]() [DocumentDB adatbázis létrehozása](documentdb-create-database.md)egy a [DocumentDB SDK](documentdb-sdk-dotnet.md), illetve a [REST API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx)keresztül ismertetett módon hozhat létre.  

### <a name="what-is-a-collection"></a>Mi az a webhelycsoport?
Egy webhelycsoport JSON dokumentumokat és a kapcsolódó JavaScript-alkalmazás logika tároló. Egy webhelycsoport számlázható egységek, ahol a [költség](documentdb-performance-levels.md) határozzák meg a teljesítmény és storaged használt. Webhelycsoportok egy vagy több partíciót/kiszolgálón is kiterjedhet, és tároló vagy átviteli gyakorlatilag korlátlan mennyiségű kezelésére is méretezhető.

Webhelycsoportok is a számlázási szervezetek számára DocumentDB. Minden egyes webhelycsoport számlázva óránként a kiépített átviteli és a rendelkezésre álló tárterület méretének használt alapján. További tudnivalókért olvassa el a [DocumentDB árak](https://azure.microsoft.com/pricing/details/documentdb/)című témakört.  

### <a name="how-do-i-set-up-users-and-permissions"></a>Hogyan állíthatom be a felhasználók és engedélyek?
Felhasználók és engedélyek közül a [DocumentDB SDK](documentdb-sdk-dotnet.md) , akár a [REST API -khoz](https://msdn.microsoft.com/library/azure/dn781481.aspx)is létrehozhat.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB elleni fejlesztésével kapcsolatos adatbázis kérdések

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Hogyan elvégzendő indításakor elkészítésének DocumentDB szemben?
A .NET, Python, Node.js, JavaScript és Java [SDK](documentdb-sdk-dotnet.md) érhetők el.  A fejlesztők is is kihasználhatja a [HTTP API -khoz RESTful](https://msdn.microsoft.com/library/azure/dn781481.aspx) vezérléséhez DocumentDB erőforrások számos különböző platformokon és nyelvek. 

A DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)és [Python](documentdb-python-samples.md) SDK minták GitHub elérhetők.

### <a name="does-documentdb-support-sql"></a>Támogatja a DocumentDB SQL?
A DocumentDB SQL lekérdezési nyelv a lekérdezés funkciókat támogatja az SQL-továbbfejlesztett részét. A DocumentDB SQL lekérdezési nyelv biztosít hierarchikus rich és relációs operátorok és bővítési keresztül JavaScript-alapú felhasználó által definiált függvények (UDF). A nyelvhelyesség-ellenőrzés JSON lehetővé teszi, hogy modellezési JSON dokumentumok mint a fák, a címkék a fastruktúrájú csomópontok, mind a DocumentDB Automatikus indexelés technikákat, valamint a DocumentDB az SQL-lekérdezés helyesírását által használt.  SQL nyelvhelyesség-ellenőrzés használatát adja című témakör tartalmaz a [Lekérdezés DocumentDB] [ query] cikk.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Mik azok a DocumentDB által támogatott adattípusok?
A támogatott DocumentDB egyszerű adattípusok ugyanazok, mint JSON. JSON karakterláncok, számok (dupla pontosság IEEE754) és logikai – IGAZ, hamis, és a NULL értékek áll egy egyszerű típusú rendszert tartalmaz.  Összetettebb adattípusokat, például a dátum és idő, globálisan egyedi azonosítója, Int64 és mértani is képviselteti JSON és DocumentDB egyaránt beágyazott objektumok a {} operátor és a [] operátorral tömböknek kell létrehozni. 

### <a name="how-does-documentdb-provide-concurrency"></a>Hogyan biztosítja a DocumentDB feldolgozási?
DocumentDB támogatja az optimista feldolgozási vezérlő (OCC) HTTP entitás címkék vagy etags keresztül. Minden DocumentDB erőforrásnak megvan-e egy etag, és a etag a kiszolgálón van beállítva, valahányszor a dokumentum változásáról. A etag élőfej- és az aktuális érték szereplő összes válaszüzenetek. Döntse el, ha egy erőforrás frissíteni kell a kiszolgáló engedélyezése Etags használható az If-egyezés fejlécekkel. Az If-egyezés érték összevetni etag értéket. Ha a etag kiszolgáló etag értéke megegyezik, az erőforrás automatikusan frissítve lesznek. A etag már nem érvényes, ha a kiszolgáló elutasítja a műveletet egy "HTTP 412 előfeltétel hiba" válasz kódot. Az ügyfél majd kell az erőforrás szerezheti be az aktuális etag értéket, az erőforrás refetch. Ezeken kívül etags kínál If-nincs – egyezés élőfej határozza meg, hogy szükség van-e az erőforrás újra lehívása. 

Optimista feldolgozási a .NET, használja a [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) osztály. .NET minta [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) a DocumentManagement mintában a github témakörben talál.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Hogyan végre DocumentDB a tranzakciók?
DocumentDB támogatja a tranzakciók nyelvi integrált JavaScript tárolt eljárások és eseményindítók keresztül. A hatóköre a webhelycsoport Ha egyetlen-partíciót gyűjteménye, vagy azonos értékű partíciót kulcs a gyűjteményben, dokumentumokat, ha a webhelycsoport particionált pillanatkép elkülönítési parancsfájlok belül az összes adatbázis-műveletek végrehajtása. A dokumentumok korábbi verzióinak (ETags) pillanatképét venni a tranzakció kezdetekor, és csak akkor, ha a parancsfájl sikeres lekötött. A JavaScript hibát okoz, ha a tranzakció vissza állítva. További információt a [DocumentDB kiszolgálóoldali programozási](documentdb-programming.md) talál.

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Hogyan lehet Beszúrás dokumentumok tömeges DocumentDB be? 
Háromféleképpen Beszúrás dokumentumok tömeges DocumentDB be:

- Az adatok áttelepítési eszköz, az [adatok importálása az DocumentDB](documentdb-import-data.md)leírt módon.
- Az Azure-portálon [tömeges hozzáadása a dokumentum Intézővel dokumentumok](documentdb-view-json-document-explorer.md#BulkAdd)ellenkezőjét Explorer dokumentumot.
- Tárolt eljárás, [DocumentDB kiszolgálóoldali programozási](documentdb-programming.md)leírt módon.

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB támogatja az erőforrás-hivatkozás gyorsítótárazás?
Igen, mivel DocumentDB RESTful szolgáltatás, erőforrás-hivatkozások megváltoztatható és gyorsítótárba helyezhető. DocumentDB ügyfelek megadhat egy "Ha-nincs – egyezés" fejléc olvasás bármely erőforrás, például a dokumentum vagy a webhelycsoport és a helyi másolja át, amikor a kiszolgáló verziója van módosítani frissítés szemben. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Érhető el helyi példányának DocumentDB?
Most egy helyi DocumentDB-példány nem érhető el. A helyi irányító és a szavazás, a [Visszajelzés fórum](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)állapotának nyomon követéséhez.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
