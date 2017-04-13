<properties
   pageTitle="A DocumentDB több régióival elkészítésének |} Microsoft Azure"
   description="További információ az adatok több régióban elérhető Azure DocumentDB, teljes körű felügyelt NoSQL adatbázis-szolgáltatás."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Több elem régió DocumentDB fiókoknál kialakítása

> [AZURE.NOTE] Globális DocumentDB adatbázisok eloszlása általában elérhető, és minden újonnan létrehozott DocumentDB fiókok automatikusan engedélyezett. Ahhoz, hogy minden meglévő fiókok globális terjesztési dolgozunk, de időközben, ha azt szeretné, hogy engedélyezve van a fiók globális terjesztési [ügyfélszolgálatát](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , és azt fogja engedélyezze azt meg most.

[Globális terjesztési](documentdb-distribute-data-globally.md)előnyeit ügyfélalkalmazásokban adhatja meg a dokumentum műveletek elvégzésére használandó régiók rendezett preferencia listáját. Ez a kapcsolat házirend elvégezhető. Az Azure DocumentDB fiók konfigurációja, az aktuális területi elérhetősége és a megadott preferencia-sorrend lista alapján, a legtöbb optimális végpont választja ki a végrehajtásához írási és olvasási műveletek SDK csomagjában talál. 

Ez a beállítás lista a DocumentDB ügyfélprogramban SDK kapcsolat inicializálásakor van megadva. A SDK fogadja el a "PreferredLocations" választható paraméter Azure régiók azaz olyan rendezett listát.

A SDK automatikusan küld összes írások az aktuális terület írni. 

Az összes olvasása a PreferredLocations lista első elérhető terület küld. Ha nem sikerül a kérést, az ügyfél fog sikertelen lefelé a listában a következő területre, és így tovább. 

Az ügyfél SDK csak megkísérli a régiók olvasni a megadott PreferredLocations. Igen például ha három régióban érhető az adatbázis-fiókot, de az ügyfél csak adja meg az írás régiók két PreferredLocations, majd nincs olvasás szolgáltató Kilépés a írási területről, még akkor is, ha a feladatátvevő.

Az alkalmazás az aktuális írási végpontot ellenőrizheti és olvasása, jelölje be a két tulajdonságait, WriteEndpoint és ReadEndpoint 1.8 SDK verzióban, illetve feletti elérhető a SDK által választott végpontot. 

Ha a PreferredLocations tulajdonság nincs beállítva, a felszolgált az aktuális írási terület összes kérelmet kell. 


## <a name="net-sdk"></a>.NET SDK
A SDK kód módosítások nélkül is használható. A SDK ebben az esetben automatikusan mindkét olvasás arra utasítja, és az aktuális írási területen írja. 

A .NET SDK 1,8 és újabb verziója a DocumentClient konstruktor ConnectionPolicy paramétere Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations nevű tulajdonságot tartalmaz. Ez a tulajdonság nem típusú webhelycsoport `<string>` és a régió névsor kell tartalmaznia. A karakterlánc-értékek vannak formázva [Azure régiók] terület neve oszloponként [ regions] , szóközök nélkül előtti vagy utáni az első oldal és a kurzor utolsó karakter.

Az aktuális írási és olvasási végpontok érhetők el a DocumentClient.WriteEndpoint és DocumentClient.ReadEndpoint rendre.

> [AZURE.NOTE] Az URL-címeit, a végpontok élettartamú állandók nem tekinthető. A szolgáltatás lehet frissíteni ezek bármely pontján. A SDK automatikusan kezeli a változás.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS és a Python SDK
A SDK kód módosítások nélkül is használható. Ebben az esetben a SDK fog automatikusan közvetlen olvasása és írása az aktuális is írása régió. 

Az 1,8 és minden egyes SDK újabb verziója a DocumentClient konstruktor ConnectionPolicy paramétere új tulajdonság neve DocumentClient.ConnectionPolicy.PreferredLocations. Ez akkor paraméteres karakterláncok tömbjét, hogy mi a régió nevek listája. A nevek / a régió neve oszlop az [Azure területek] vannak formázva [ regions] lapot. A csoportosított objektumban AzureDocuments.Regions is használhatja az előre definiált állandók

Az aktuális írási és olvasási végpontok érhetők el a DocumentClient.getWriteEndpoint és DocumentClient.getReadEndpoint rendre.

> [AZURE.NOTE] Az URL-címeit, a végpontok élettartamú állandók nem tekinthető. A szolgáltatás lehet frissíteni ezek bármely pontján. A SDK automatikusan kezeli a változás.

Az alábbi példa egy olyan NodeJS/Javascript. Python és Java azonos típusúak követi.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>TÖBBI 
Miután egy adatbázis-fiókot elérhetővé tett több régióban, ügyfelek elérhetőségét is lekérdezhetők az alábbi URI GET kérésekben végrehajtásával.

    https://{databaseaccount}.documents.azure.com/

A szolgáltatás által visszaadható régiók és a megfelelő DocumentDB végpont URL-címe a replikáinak listáját. A válasz az aktuális írási terület jelzik. Az ügyfél választhatja ki a megfelelő végpont a továbbiakban REST API-kérelmek az alábbi képlettel történik.

Példa válasz

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Az összes helyezése, a bejegyzés és a DELETE kérések kell lépnie a megjelölt írási URI
-   Minden keresése és más csak olvasható kérelmek (például lekérdezések) léphet bármelyik végpontját az ügyfél választási lehetőségek

Írás a csak olvasható régiók kérések meghiúsul HTTP hibakóddal 403 ("tiltott").

Ha az írási terület az ügyfélprogram első keresési szakaszban után megváltozik, későbbi írást az írás előző régióra HTTP hibakóddal 403 ("tiltott") sikertelen lesz. Az ügyfél majd SZEREZHETI be újra a frissített írási régióban megszerezni régiók listáját.

## <a name="next-steps"></a>Következő lépések

További információ a terjesztésével adatait globálisan DocumentDB az alábbi cikkekben:

- [Adatok globálisan DocumentDB terjesztése](documentdb-distribute-data-globally.md)
- [Egységesebb szintek](documentdb-consistency-levels.md)
- [Hogyan működik a átviteli több régióival](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Az Azure portálon régiók hozzáadása](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
