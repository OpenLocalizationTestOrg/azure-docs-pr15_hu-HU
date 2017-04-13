<properties
    pageTitle="A kapcsolat SQL-hiba, ideiglenes (tranziens) hiba elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként kapcsolatos hibák elhárítása diagnosztizálása és megakadályozhatja, hogy egy SQL-kapcsolati hiba vagy a ideiglenes (tranziens) hiba Azure SQL-adatbázisban. "
    keywords="SQL-kapcsolat, a kapcsolati karakterlánc, a csatlakozási problémákat tapasztal, a ideiglenes (tranziens) hiba, a kapcsolati hiba"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Hibaelhárítás diagnosztizálása és megakadályozhatja, hogy az SQL-kapcsolat hibák és tranziens hibák SQL-adatbázis

Ez a cikk ismerteti, hogyan megakadályozhatja, hogy, elhárítása, diagnosztizálása és csatlakozási hibák és az ügyfélalkalmazás lép fel, ha azt kommunikáljon Azure SQL-adatbázis tranziens hibákról csökkentésében. Megtudhatja, hogy miként újrapróbálkozási logika beállítását, a kapcsolati karakterlánc összeállítása és egyéb csatlakozási beállítások megadása.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Ideiglenes (tranziens) hibák ideiglenes (tranziens)

Tranziens hiba - is tranziens hibafa - tartalmaz, az alapul szolgáló probléma okát, hamarosan megoldásához magát. Egy alkalmi tranziens hibák oka az, ha az Azure rendszer gyorsan értékeként hardver-erőforrások jobb terheléselosztási különböző munkaterhelésekből. A legtöbb gyakran teljes kisebb, mint 60 másodpercben konfigurálás események. A konfigurálás időtartamához során szükség lehet a csatlakozási problémákat tapasztal az Azure SQL-adatbázishoz. Azure SQL-adatbázis csatlakoztatása az alkalmazások kell épített várt tranziens a hibák, kezelheti őket a újrapróbálkozási logikájának végrehajtása a kód helyett a felhasználók alkalmazás hibaként útburkoló őket.

Ha az ügyfélprogram az ADO.NET, a program a tranziens hibára vonatkozó van említettük a throw, egy **SqlException**. A **szám** tulajdonság hasonlítható a témakör tetején tranziens hibák ellen: [az SQL-adatbázis ügyfélalkalmazásokban SQL-hibakódok](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Parancs és a kapcsolat

Ismételje meg az SQL-kapcsolat fog, vagy létesítsen újra, attól függően, hogy a következő:

* **Egy kapcsolatot próbálja meg egy ideiglenes (tranziens) hiba lép fel**: A kapcsolat újra meg kell próbálni késleltetése néhány másodpercig után.

* **Egy ideiglenes (tranziens) hiba lép fel egy SQL-lekérdezés parancsot**: kell a parancs nem azonnal újból. Ehelyett várakozás után kell frissen létre. Kattintson a parancsot kell ismételni.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>A folyamat működését tranziens hibáinak újrapróbálkozási


Tranziens hiba időnként fellépő ügyfélprogramok akkor megbízhatóbb, ha ismét logika tartalmaznak.


Amikor a program a 3 buli köztes keresztül Azure SQL-adatbázis kommunikál, kérdezni a szállítóval a köztes tranziens hibákat ismét logika tartalmazza.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Ismét alapelvei


- Nyisson meg egy kapcsolatot próbál meg kell ismételni, ha a hiba ideiglenes (tranziens).


- Egy ideiglenes (tranziens) hiba miatt sikertelen az SQL SELECT utasítás nem közvetlenül kell ismételni.
 - Ehelyett friss kapcsolatot létesíteni, és próbálkozzon újra a SELECT.


- Ha egy frissítés az SQL-utasítást tranziens hibával meghiúsul, friss kell hozható létre kapcsolat megismétlése a frissítés előtt.
 - Az újraküldés logikája gondoskodnia kell arról, hogy a teljes adatbázis-tranzakciók befejeződött, vagy, amely a teljes tranzakció visszaáll.


#### <a name="other-considerations-for-retry"></a>Újra az egyéb szempontok


- Nagyon türelemmel intervallumokkal hosszú idő a újrapróbálkozási kísérletek között is megadja egy köteg program, amelyek automatikusan elindul, munkaidő után, és amelyek délelőtt, előtt befejeződik.


- A felhasználói felület program az emberi tendencia figyelhető fel kell adnia túl hosszú a várakozás után célszerű figyelembe venni.
 - Azonban a megoldást nem lehet újra minden néhány másodpercig, mert adott házirend is öntsünk a rendszer kéréseivel.


#### <a name="interval-increase-between-retries"></a>Időtartomány növelése újrapróbálkozások között



Azt javasoljuk, hogy késleltetése a 5 másodpercig előtt az első újra gombra. Újra próbálkozik rövidebb, mint a felhőbeli szolgáltatástól overwhelming 5 másodperc kockázatok késleltetés után. Az egyes későbbi újra kell a késleltetés exponenciálisan, nagyobb legfeljebb 60 másodperc.

Az [SQL Server egyesítése (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)az az *időszak blokkolása* ADO.NET használó ügyfelek vitafórum érhető el.

Érdemes azt is, állítsa be egy újrapróbálkozások maximális számát, mielőtt a saját megszakítja a program.


#### <a name="code-samples-with-retry-logic"></a>Az újraküldés logikája mintakódok


Az újraküldés logikája különböző programozási nyelven, mintakódok, érhetők el:

- [A kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Az újraküldés logikája tesztelése


Az újraküldés logikája teszteléséhez hasonlóan, vagy hibát jelez, mint a program futása közben javítható.


##### <a name="test-by-disconnecting-from-the-network"></a>Tesztelje a hálózati kapcsolat bontása


Az újraküldés logikája tesztelheti egyik módja az ügyfélgépen beállított leválasztása a hálózatról, a program futása közben. A hiba a következő lesz:

- **SqlException.Number** = 11001
- Üzenet: "nincs ilyen ismert állomás"


Ismét elsőre részeként a program is javítsa a helyesírási, és próbálja meg csatlakozni.


Ahhoz, hogy ez gyakorlati, akkor húzza ki a számítógép hálózati a program indítása előtt. Kattintson a program ismer fel, amelyek hatására a program a futási idő paraméter:

1. Ideiglenes 11001 hozzáadása a tranziens tekinti hibák listáját.
2. Az első kapcsolat próbálja meg a szokásos módon.
3. A hiba kiszűri, miután 11001 eltávolítása a listából.
4. Egy tájékoztató üzenet jelzi a felhasználót a hálózatba a számítógéphez csatlakoztatni megjelenítéséhez.
 - Mutasson az egérrel további végrehajtási segítségével, a **Console.ReadLine** módszer vagy egy párbeszédpanel OK gombbal. A felhasználó a számítógép csatlakoztatva a hálózati után lenyomja a az Enter billentyűt.
5. Próbálja meg ismét csatlakozni, sikeres meglepő.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Tesztelje a adatbázis név hibás beírása, csatlakozáskor


A program szándékosan is elgépel a felhasználó nevét, mielőtt az első kapcsolódási kísérletre. A hiba a következő lesz:

- **SqlException.Number** = 18456
- Üzenet: "sikertelen bejelentkezés"WRONG_MyUserName"felhasználónál."


Ismét elsőre részeként a program is javítsa a helyesírási, és próbálja meg csatlakozni.


Ahhoz, hogy ez gyakorlati, a program, amelyek hatására a program a futási idő paraméter sikerült felismer:

1. Ideiglenes 18456 hozzáadása a tranziens tekinti hibák listáját.
2. "WRONG_" szándékosan hozzáadása a felhasználó nevét.
3. A hiba kiszűri, miután 18456 eltávolítása a listából.
4. Távolítsa el a felhasználónév "WRONG_".
5. Próbálja meg ismét csatlakozni, sikeres meglepő.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Kapcsolat újrapróbálkozási .NET SqlConnection paraméterei


A .NET-keretrendszer használatával Azure SQL-adatbázishoz osztály **System.Data.SqlClient.SqlConnection**kapcsolódik az ügyfélprogram, ha, használja a .NET 4.6.1 vagy újabb verzió, így a kapcsolat ismétlési funkció is élvezheti. Részletek az funkció [az alábbi](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Ha a **SqlConnection** objektum generál a [kapcsolati karakterláncot](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) , meg kell koordinálása az értékek között a következő paraméterek:

- ConnectRetryCount &nbsp; &nbsp; *(az alapértelmezett érték 1. Tartománya 0 és 255 közötti.)*
- ConnectRetryInterval &nbsp; &nbsp; *(az alapértelmezés 1 másodperc. Tartomány: 1 60 keresztül.)*
- A kapcsolat időtúllépési &nbsp; &nbsp; *(az alapértelmezett érték 15 másodperc. A tartománya 0-tól 2147483647)*


A kiválasztott értékeket pontosabban a következő egyenlő igaz kell tennie:

- A kapcsolat időtúllépési = ConnectRetryCount * ConnectionRetryInterval

Például ha az száma = 3 és intervallum = 10 másodperc, az időtúllépés, csak a 29 másodpercet volna nem igazán ad a rendszer elég időt a 3 és záró ismét a csatlakozás a: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Parancs és a kapcsolat


A **ConnectRetryCount** és **ConnectRetryInterval** paraméterek lehetővé teszik az **SqlConnection** objektum, ismételje meg a csatlakozás művelet jelzi, és a programot, például a vezérlő Visszatérés a program bothering nélkül. Az ismétlések akkor fordulhat elő, az alábbi esetekben:

- mySqlConnection.Open módszer hívás
- mySqlConnection.Execute módszer hívás

Van egy subtlety. Egy ideiglenes (tranziens) hiba lép fel, amíg a *lekérdezés* hajtja végre, ha a **SqlConnection** objektum nem ismételje meg a csatlakozás műveletet, és azt bizonyára nem újra a lekérdezést. **SqlConnection** nagyon gyorsan a kapcsolatot a lekérdezés-végrehajtási elküldése előtt ellenőrzi. A gyors ellenőrzés csatlakozási problémát észlel, ha a **SqlConnection** próbálkozások a csatlakozás műveletet. Ha sikeres az Ismét gombra, a lekérdezés, az adatvégrehajtás küldi.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount kombinálni kell alkalmazás újrapróbálkozási logika?

Tegyük fel, hogy az alkalmazás robusztus egyéni újrapróbálkozási logika tartalmaz. Akkor lehet, hogy újra a csatlakozás műveletet 4 alkalommal. Ha felvesz **ConnectRetryInterval** és **ConnectRetryCount** = 3 a kapcsolati karakterláncot az ismétlési számot és 4 * 3 = 12 növelik újrapróbálkozások. Előfordulhat, hogy nem kíván ilyen újrapróbálkozások nagy száma.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Azure SQL-adatbázis-kapcsolatot

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Kapcsolat: Kapcsolat-karakterlánc


A kapcsolati karakterlánc Azure SQL-adatbázishoz való csatlakozás szükséges némileg eltér a Kapcsolódás a Microsoft SQL Server-karakterlánc. A kapcsolati karakterláncot az adatbázis másolhatja az [Azure-portálon](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Kapcsolat: IP-cím


Az SQL-adatbázis azon kiszolgálóját, fogadja el a számítógéphez, amelyen az ügyfélprogram közlése IP-cím meg kell adnia. Ehhez az [Azure-portálon](https://portal.azure.com/)keresztül tűzfal beállításainak szerkesztése.


Ha elfelejtette konfigurálása az IP-cím, a program a praktikus hibaüzenet arról, hogy a szükséges IP-cím sikertelen lesz.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


További tudnivalókért lásd: [hogyan: SQL-adatbázis tűzfal beállításainak](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Kapcsolat: portok


Általában csak szüksége annak érdekében, hogy port 1433 nyissa meg a számítógépen, amelyen meg ügyfélprogram a kimenő kommunikáció számára.


Például az ügyfélprogram üzemelteti Windows rendszerű számítógépen, amikor a Windows tűzfal az állomáson lehetővé teszi, port 1433 megnyitásához:


1. Nyissa meg a Vezérlőpultot
2. &gt;Minden Vezérlőpult-elem
3. &gt;A Windows tűzfal
4. &gt;Speciális beállítások
5. &gt;Kimenő szabályok
6. &gt;Műveletek
7. &gt;Új szabály


Ha az ügyfélprogram-Azure virtuális gépen (virtuális) helyezkedik el, olvassa el:<br/>[Túl 1433 ADO.NET 4.5-portok és SQL-adatbázis V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Háttér-információiról portokat és IP-cím cofiguration, lásd: [Azure SQL-adatbázis tűzfal](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Kapcsolat: ADO.NET 4.6.1.


Ha a program ADO.NET osztályok **System.Data.SqlClient.SqlConnection** például használja a Azure SQL-adatbázishoz való csatlakozáshoz, azt javasoljuk, hogy a .NET-keretrendszer 4.6.1 verzióját használja, vagy újabb verziójában.


ADO.NET 4.6.1:

- Azure SQL-adatbázis nincs nagyobb megbízhatóság kapcsolat **SqlConnection.Open** módszerrel megnyitásakor. Az **Open** metódus most már megfelelő legjobb munkamennyiség újrapróbálkozási mechanizmusok válasz tranziens hibákra, és egy bizonyos a kapcsolat időtúllépési időszakban hibák.
- Támogatja a kapcsolatkészletezés. Ide tartoznak egy hatékony annak ellenőrzése, hogy működik-e a program lehetőséget kínál a kapcsolat-objektumot.



A kapcsolat készletből kapcsolati objektum használatakor azt javasoljuk, hogy a program ideiglenesen a Bezárás gombra a kapcsolat nem azonnal használat. Kapcsolat ismételt megnyitása nem drága az új kapcsolat létrehozása módja.


Ha ADO.NET 4.0-s vagy régebbi verzióiban, azt javasoljuk, hogy frissítsen a legújabb ADO.NET.

- Kezdve November 2015 azt is megteheti [ADO.NET 4.6.1 letöltése](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnosztikai

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnosztikai: Ellenőrzéséhez segédprogramok csatlakoztatása


Ha a program hibát Azure SQL-adatbázishoz való csatlakozáshoz egy diagnosztikai, hogy az segédprogram való csatlakozáskor. Ideális a segédprogram volna csatlakozzon a program által használt ugyanabban a tárban.


Bármely Windows rendszerű próbálja meg ezeket a segédprogramok:

- SQL Server Management Studio (ssms.exe), amely ADO.NET használatával kapcsolódnak.
- Sqlcmd.exe, amelyeket a [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)használatával kapcsolódnak.


Miután létrejött, ellenőrizze, hogy egy rövid SQL-VÁLASZTÓ lekérdezés működik-e.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnosztikai: A nyitott portokat ellenőrzése


Tegyük fel, hogy azt gyanítja, hogy kapcsolatfelvételi port problémák miatt nem működnek. A számítógépen, amely a jelentést a portbeállításokat segédprogram futtathatja.


Linux a következő segédprogramok lehet hasznos:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (A példában szereplő érték kell az IP-címének módosítása.)


A Windows a [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) segédprogram hasznos lehet. Íme egy példa végrehajtás, amely megkérdezi, hogy az Azure SQL-adatbázis kiszolgálója port helyzet, és amely hordozható számítógépes verziójában lett futtatni:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnosztikai: A hibák naplózása


Szakaszos hibát előfordul, hogy a legjobb vizsgált, általános mintát kimutatása napi vagy heti fölé.


A diagnosztikai naplózás az összes hibát, amelyekkel találkozik által segítheti az ügyfélnek. Előfordulhat, hogy a naplóbejegyzések összehangolására Azure SQL-adatbázis naplózza magát belső hiba adatokkal.


Vállalati tár 6 (EntLib60) felügyelt .NET osztályok naplózás megkönnyítése kínál:

- [5 – ugyanolyan egyszerű, mint a napló kikapcsolása alá: segítségével a naplózás-alkalmazás letiltása](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnosztikai: Vizsgálja meg a rendszer naplók hibák


A lekérdezés naplók hiba és egyéb adatok megadása az alábbiakban néhány Transact-SQL nyelvben SELECT utasításokat.


| Lekérdezés napló | Leírás |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | A [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) nézet egyes eseményeket, például a néhány tranziens hibákat és kapcsolódási hibák okozhatnak információt nyújt.<br/><br/>Ideális a **start_time** vagy **end_time** értékek is összehangolására információkkal, amikor a az ügyfélprogram tapasztalt problémák.<br/><br/>**Tipp:** Csatolnia kell futtatni a **fő** adatbázist. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | A [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) nézet olyan esemény típusa, további diagnosztika az összesített számát.<br/><br/>**Tipp:** Csatolnia kell futtatni a **fő** adatbázist. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnosztika: Az SQL-adatbázis naplóban probléma események keresése


A napló Azure SQL-adatbázis probléma eseményekről bejegyzésekhez is kereshet. Próbálkozzon az alábbi Transact-SQL SELECT utasítás a **fő** adatbázist:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>A sys.fn_xe_telemetry_blob_target_read_file néhány visszaadott sorok


Ezután van visszaadott sorok néz. A null látható értékei gyakran nem null értékű, a többi sorban lévő.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Vállalati tár 6


Vállalati tár 6 (EntLib60) keretében .NET osztályok, amely segít a megvalósítása robusztus ügyfelek felhőalapú szolgáltatást, amelyek egyike az Azure SQL-adatbázis szolgáltatás. Keresse meg az egyes területeknek, amelyben EntLib60 segítheti első meglátogatják dedikált témakörök:

- [6 – április 2013 vállalati tárban](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Ismét logika tranziens hibák kezelésére egy területet, ahol EntLib60 segítséget:

- [4 – perseverance, az összes Triumphs titkos: Tiltás tranziens hiba kezelésének alkalmazás használata](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] A forrás kódját EntLib60 nyilvános [letöltése](http://go.microsoft.com/fwlink/p/?LinkID=290898)érhető el. A Microsoft nem áll szándékában, hogy a további szolgáltatás és karbantartás frissítések EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Az ideiglenes (tranziens) hibákat, majd próbálkozzon újra EntLib60 osztályok


A következő EntLib60 osztályok különösen hasznosak újrapróbálkozási összefüggés. Mind a nem, vagy további a névtér **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**csoportban:

*A névtér* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** osztály
 - **Adatválaszték tulajdonságai**


- **ExponentialBackoff** osztály


- **SqlDatabaseTransientErrorDetectionStrategy** osztály


- **ReliableSqlConnection** osztály
 - **ExecuteCommand** módszer


A névtér **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** osztály

- **NeverTransientErrorDetectionStrategy** osztály


Az alábbiakban EntLib60 kapcsolatos információkra mutató hivatkozások:

- Ingyenes [címjegyzék letöltése: Microsoft Enterprise-tárba, 2nd Edition fejlesztői útmutató](http://www.microsoft.com/download/details.aspx?id=41145)

- Gyakorlati tanácsok: újrapróbálkozási logikájának egy kiváló részletes ismertető [Általános útmutatásának újrapróbálkozási](../best-practices-retry-general.md) rendelkezik.

- [Vállalati](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) tár - alkalmazás letiltása tranziens hiba kezelésének 6.0 NuGet letöltése

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: A naplózás blokk


- Naplózás Tiltás lehetővé teszi, hogy nagymértékben rugalmas és konfigurálható megoldás:
 - Hozzon létre, és naplózás számos különböző helyeken tárolja.
 - Kategorizálás és üzenetek szűrése.
 - Környezetfüggő, amely hasznos Hibakeresés és nyomkövetés, valamint a naplózás és az általános naplózás követelmények vonatkozó információk gyűjtését.


- A Naplózás tiltása a naplózás a napló cél szolgáltatásai abstracts, úgy, hogy az alkalmazás kódja egységes, a helyet, és írja be a cél naplózás áruház függetlenül.


Című témakör tartalmaz: [5 -, egyszerűen, alá tartozó kikapcsolása a napló: a naplózás alkalmazás blokk használatával](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient módszer forráskód


Ezután a **SqlDatabaseTransientErrorDetectionStrategy** osztály a C# forráskód **IsTransient** mód van. A forráskód értelmezi milyen hibákat voltak tekinthető tranziens és újrapróbálkozási április 2013 kezdve az worthy.

Számos **//comment** vonalak el lett távolítva másoláshoz hangsúlyozni olvashatóság érdekében.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Következő lépések

- Hibaelhárítási más közös Azure SQL-adatbázis kapcsolódási problémákat, látogasson el a [kapcsolódási problémák elhárítása a Azure SQL-adatbázishoz](sql-database-troubleshoot-common-connection-issues.md).

- [Az SQL Server-kapcsolatkészletezés (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Újrapróbálkozás* Apache 2.0-s licencelt általános célú ismételt tár **Python**egyszerűsítése érdekében a tevékenység hozzáadása a újrapróbálkozási viselkedés szinte bármilyen nyelven íródott.](https://pypi.python.org/pypi/retrying)
