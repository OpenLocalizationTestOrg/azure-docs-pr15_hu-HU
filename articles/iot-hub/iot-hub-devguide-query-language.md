<properties
 pageTitle="Fejlesztőeszközök útmutató - lekérdezési nyelv |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - lekérdezési nyelv beolvasásához twins, a módszereket és a feladatok adatait a IoT központból használt leírása"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Hivatkozás - lekérdezési nyelv twins és feladatok

## <a name="overview"></a>– Áttekintés

IoT központi biztosít hatékony SQL-szerű nyelven [eszköz twins] vonatkozó adatok beolvasásához[ lnk-twins] és a [feladatok][lnk-jobs]. Ez a cikk bemutatja:

* A főbb funkciói bemutatása IoT központi lekérdezésnyelv, és
* A nyelv részletes leírását.

## <a name="getting-started-with-twin-queries"></a>Kettős lekérdezések – első lépések

[Eszköz twins] [ lnk-twins] tetszőleges JSON objektumok is tartalmazhat, címkéket és a Tulajdonságok parancsot. IoT központi lehetővé teszi, hogy lekérdezés eszköz twins összes kettős adatokat tartalmazó egyetlen JSON dokumentumként.
Tegyük fel például, hogy a IoT központi twins lenniük, a következő:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT központi az eszköz twins közzététele, mint egy dokumentum a webhelycsoport **eszközök**nevű.
Így a következő lekérdezés olvassa be a teljes készlete eszköz twins:

        SELECT * FROM devices

> [AZURE.NOTE] [IoT központi SDK] [ lnk-hub-sdks] nagy eredmények lapozási támogatja.

IoT webhely segítségével a szűrés tetszőleges feltételt tartalmazó twins beolvasásához. Ha például

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

a twins beolvassa a **location.region** címkével értékre **US**.
Logikai operátorok és a számtani, összehasonlító táblázat támogatott is, például

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

olvassa be az összes twins telemetriai percenként ritkán küldésére beállított, az Amerikai Egyesült Államokban található. A könnyebb hogy az is lehetséges tömbállandók használata **az** és (a) **nA** operátorok együtt. Ha például

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

olvassa be az összes twins jelentett WiFi-csatlakozás vagy vezetékes kapcsolatot. A [WHERE záradék] hivatkozni[ lnk-query-where] a szűrési lehetőségek a teljes hivatkozás szakaszában.

Csoportosítás és az összesítések szintén támogatottak. Ha például

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

az egyes telemetriai konfiguráció állapota eszközök számát adja vissza.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

A fenti példa mutatja be, hogy olyan helyzet, ahol három eszközök jelentett sikeres konfigurációs, két továbbra is a beállítások alkalmazásával, és egy jelzett hibát.

### <a name="c-example"></a>Példa a C#

A lekérdezés funkciókat van téve a [C# szolgáltatás SDK] [ lnk-hub-sdks] az a osztály **RegistryManager** .
Íme egy példa egy egyszerű lekérdezés:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Figyelje meg, hogyan a **lekérdezési** objektum jön létre, oldal méretű (legfeljebb 1000), és kattintson a több oldal tudja visszaszerezni, hívja fel a **GetNextAsTwinAsync** módszerek többször.
Fontos tudni, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, hogy a deszerializálás beállítást választja, a lekérdezés által kért, azaz a két vagy a projekt-objektumok vagy előrejelzések használatakor használandó egyszerű Json.

### <a name="node-example"></a>Csomópont példa

A lekérdezés funkciókat van téve [csomópont szolgáltatás SDK] [ lnk-hub-sdks] a a a **beállításjegyzék** -objektumot.
Íme egy példa egy egyszerű lekérdezés:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Figyelje meg, hogyan a **lekérdezési** objektum jön létre, oldal méretű (legfeljebb 1000), és kattintson a több oldal tudja visszaszerezni, hívja fel a **nextAsTwin** módszerek többször.
Fontos tudni, hogy a lekérdezés vezérlőnek több **következő\***, attól függően, hogy a deszerializálás beállítást választja, a lekérdezés által kért, azaz a két vagy a projekt-objektumok vagy előrejelzések használatakor használandó egyszerű Json.

### <a name="limitations"></a>Korlátozások

Jelenleg előrejelzések összesítések használata esetén csak támogatottak, vagyis csak használható lekérdezések nem összesített `SELECT *`. Összesítési is, csak támogatottak csoportosítási együtt.

## <a name="getting-started-with-jobs-queries"></a>Feladatok lekérdezések – első lépések

[Feladatok] [ lnk-jobs] meg az eszközök műveletek végrehajtása ad lehetőséget. Minden egyes eszköz kettős a feladatok, amelynek **feladatok**nevű gyűjtemény részt is információkat tartalmazza.
Logikus,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Ebben a gyűjteményben jelenleg queriable **devices.jobs** IoT központi lekérdezési nyelvű szerint.

Ha például úgy juthat az összes feladatot (múlt és az ütemezett), amelyek befolyásolják egyetlen eszközt, használhatja az alábbi lekérdezés:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Fontos tudni, hogyan ezt a lekérdezést nyújt az eszköz-specifikus állapotát (illetve esetleg a közvetlen módszer választ) minden feladat adja vissza.
Akkor is az összes tulajdonság a **devices.jobs** gyűjteményben az objektumok tetszőleges logikai feltételt tartalmazó szűréséhez.
Ha például a következő lekérdezés:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

olvassa be az összes kettős kész update feladatok eszköz **myDeviceId** szeptember 2016 után létrehozott.

Akkor is beolvasásához egyetlen feladat eszköz eredményeit.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Korlátozások
Lekérdezések **devices.jobs** jelenleg nem támogatott:

* Előrejelzések, vagyis csak `SELECT *` lehetséges;
* Az eszköz a két feladat tulajdonságai ahogy alább látható; mellett hivatkozó feltételek
* Összesítések végrehajtása a adatain, például a darab, az Átl, csoportosítás.

## <a name="basics-of-an-iot-hub-query"></a>Egy központi IoT lekérdezést alapjai

Minden egyes IoT központi lekérdezés, ahol VÁLASZTÓ és a záradékok és választható WHERE és a GROUP BY záradék. Minden lekérdezés futtatása a JSON-dokumentumok, például eszköz twins gyűjteménye. A FROM záradék azt jelzi, hogy a dokumentum gyűjteményben**eszközök** méretű **devices.jobs**többször is lehet. Ezután a WHERE záradék a szűrő alkalmazása. Összesítések, ha ezt a lépést eredményeinek vannak csoportosítva, mint egy sor jön létre a GROUP BY záradékban, és az egyes csoportok megadva, a SELECT záradékban megadott.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM záradék.

A **< from_specification > FROM** záradék is vállalja csak két érték: **FROM eszközök**lekérdezés eszköz twins vagy **FROM devices.jobs**lekérdezés feladat eszköz lista részletei beállítást.

## <a name="where-clause"></a>A WHERE záradék

A **Hol < filter_condition >** záradék használata nem kötelező. Azt adja meg, amelyeknek a mezőértékei, amely a JSON-dokumentumokat a feladó gyűjteményben meg kell felelniük annak érdekében, hogy az eredmény része lesz. Bármely JSON dokumentum kiértékelésének eredménye "igaz" bekerülnek az eredménye a megadott feltételeknek.

Az engedélyezett feltételek megkötésekről [kifejezések és a feltételek]szakasz[lnk-query-expressions].

## <a name="select-clause"></a>SELECT záradék.

A SELECT záradékban (**< select_list > kiválasztása**) kötelező, és adja meg, milyen értékeket lehet beolvasni a lekérdezést. Adja meg a JSON értékek létre új JSON-objektumok, az egyes elemeit a feladó gyűjtemény szűrt (és tetszés szerint csoportosított) részhalmazát a kivetítés fázis hoz létre egy új JSON-objektum, a helyes összeállítás a SELECT záradékban megadott értékekkel.

Ez a SELECT záradék a nyelvhelyesség-ellenőrzés:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

Ha a JSON-dokumentumot, a feladó gyűjteményben bármely tulajdonsága **attribute_name** hivatkozik. Néhány példa a SELECT záradék megtalálható az [első lépések a kettős lekérdezések] [ lnk-query-getstarted] szakaszban.

Jelenleg kijelölt záradékok eltérő **kiválasztása \* ** csak az összegző lekérdezések twins támogatott.

## <a name="group-by-clause"></a>GROUP BY záradék

A **GROUP BY < group_specification >** záradék a következő egy nem kötelező lépés, amely a megadott a WHERE záradékban, és válassza ki a megadott a kivetítés előtt szűrő után futtatható. Azt a csoportok dokumentumok tulajdonság értéke alapján. A SELECT záradékban megadott összesített értékek létrehozásához használt ezekhez a csoportokhoz.

A következő példa egy lekérdezést a GROUP BY használatával:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

A hivatalos GROUP BY szintaxisa:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

Ha a JSON-dokumentumot, a feladó gyűjteményben bármely tulajdonsága **attribute_name** hivatkozik. 

A GROUP BY záradék jelenleg csak támogatott, amikor twins lekérdezése.

## <a name="expressions-and-conditions"></a>A kifejezések és feltételek

Magas szintű, egy *kifejezést*:

* Egy példány a JSON típusú (azaz logikai érték, szám, karakterlánc, tömb vagy objektum), az eredmény és
* Határozza meg az eszköz JSON-dokumentumot, és a beépített operátorok és a függvények használata állandók az adatok kezelésére.

*Feltétel* teljesülésének kifejezések, amelyek kiértékeléskor logikai értéket, azaz bármely állandó más, mint a logikai **Igaz** tekintendő **hamis** (együtt **null**, **nem meghatározott**, objektum vagy tömbben példányokban, tetszőleges karakterlánc és pontosan a logikai **hamis**).

A kifejezések szintaxisa a következő:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

Ha:

| Szimbólum | Meghatározása |
| -------------- | -----------------|
| attribute_name | A JSON-dokumentumot, a feladó gyűjteményben bármely tulajdonsága. |
| unary_operator | Bármely unáris operátor operátorok szakasz szerint. |
| binary_operator | Bináris műveleti operátorok szakasz szerint. |
| decimal_literal | Egy tizedes tört formátumban kifejezett lebegtetés. |
| hexadecimal_literal | A hexadecimális számjegyből álló karakterlánc követ "0 x" karakterlánc kifejezett szám. |
| string_literal | Szövegkonstanssal nulla vagy több Unicode-karakterek sorozatát vagy escape-sorozatok által jelölt Unicode-karakterláncok. Aposztrófok közé vannak szövegkonstanssal (aposztróf: ") vagy dupla idézőjel (idézőjel:"). Kilépés engedélyezett: `\'`, `\"`, `\\`, `\uXXXX` 4 hexadecimális számjegyből álló által meghatározott Unicode-karakterek. |

### <a name="operators"></a>Operátorok

Az alábbi operátorok használhatók:

| Családi | Operátorok |
| -------------- | -----------------|
| Számtani | +,-,*,/,% |
| Logikai | ÉS, VAGY NEM |
| Összehasonlítás |  =,! =, <>,, <, > =, <> |

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként lekérdezések futhat-alkalmazások használata [IoT központi SDK][lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md