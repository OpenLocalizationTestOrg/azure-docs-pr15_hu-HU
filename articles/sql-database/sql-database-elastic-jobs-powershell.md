<properties 
    pageTitle="Létrehozása és kezelése a PowerShell használatá rugalmas adatbázis feladatok" 
    description="A PowerShell használatával készletek Azure SQL-adatbázis kezelése" 
    services="sql-database" documentationCenter=""  
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="create-and-manage-a-sql-database-elastic-database-jobs-using-powershell-preview"></a>Létrehozása és kezelése egy SQL-adatbázis rugalmas adatbázis feladatok (előzetes verzió) PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](sql-database-elastic-jobs-create-and-manage.md)
- [A PowerShell](sql-database-elastic-jobs-powershell.md)



**Rugalmas adatbázis feladatok** (előzetes verzió), a PowerShell API-k lehetővé teszik, hogy a csoport adatbázisok, amelyek ellen parancsfájlok hajtja végre. Ebből a cikkből megtudhatja, hogyan hozhat létre és kezelhet **Rugalmas adatbázis feladatok** PowerShell-parancsmagok használata. [Rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md)című témakörben találhat. 

## <a name="prerequisites"></a>Előfeltételek
* Egy Azure-előfizetést. Az ingyenes próbaverzióra olvassa el a [hónap próba-előfizetésre](https://azure.microsoft.com/pricing/free-trial/)című témakört.
* A rugalmas adatbázis eszközzel létrehozott adatbázisok csoportja. [Első lépések rugalmas Adatbáziseszközök](sql-database-elastic-scale-get-started.md)című témakör tartalmaz.
* Azure PowerShell. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).
* **Rugalmas adatbázis feladatok** A PowerShell-csomag: lásd: [a feladatok rugalmas adatbázis telepítése](sql-database-elastic-jobs-service-installation.md)

### <a name="select-your-azure-subscription"></a>Jelölje ki az Azure előfizetés

Jelölje ki az előfizetést az előfizetési azonosító (**-SubscriptionId**) vagy -előfizetésre van szüksége a (**-SubscriptionName**) nevet. Ha több előfizetéssel, futtassa a **Get-AzureRmSubscription** parancsmagot, és a kívánt előfizetés adatainak másolása az eredményhalmaz. Ha befejezte az előfizetési adatokat, futtassa az alábbi parancsmag az előfizetés beállítása alapértelmezettként, nevezetesen a cél létrehozásához és kezeléséhez feladatok:

    Select-AzureRmSubscription -SubscriptionId {SubscriptionID}

A [PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx) használatát, valamint PowerShell-parancsprogramok rugalmas adatbázis feladatok végrehajtása javasolt.

## <a name="elastic-database-jobs-objects"></a>Rugalmas feladatok az adatbázis-objektumok

Az alábbi táblázat meg az objektum diagramtípusokat **Rugalmas adatbázis feladatok** és a leírás és a megfelelő PowerShell API-hoz.

<table style="width:100%">
  <tr>
    <th>Objektumtípus</th>
    <th>Leírás</th>
    <th>Kapcsolódó PowerShell API-hoz</th>
  </tr>
  <tr>
    <td>Hitelesítő adatok</td>
    <td>Felhasználónév és jelszó parancsfájlok végrehajtását vagy DACPACs alkalmazásának adatbázisokhoz kapcsolódáskor használni. <p>A jelszó titkosítva küldene, és az adatbázis-feladatok rugalmas adatbázis tárolási előtt.  A jelszó visszafejti a hitelesítő adatok létrehozott és a telepítési parancsfájlt feltöltött keresztül a rugalmas adatbázis feladatok szolgáltatást.</td>
    <td><p>Get-AzureSqlJobCredential</p>
    <p>Új AzureSqlJobCredential</p><p>Set-AzureSqlJobCredential</p></td></td>
  </tr>

  <tr>
    <td>Parancsfájl</td>
    <td>Végrehajtási adatbázisok közötti használandó parancsfájl Transact-SQL nyelvben.  A parancsprogram kell többen idempotent lesz, mivel a szolgáltatás újra próbálkozik hibák, a parancsfájl végrehajtását.
    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Get-AzureSqlJobContentDefinition</p>
    <p>Új AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>

  <tr>
    <td>DACPAC</td>
    <td><a href="https://msdn.microsoft.com/library/ee210546.aspx">Adatok szintű alkalmazás </a> csomag adatbázisokhoz alkalmazhatók.

    </td>
    <td>
    <p>Get-AzureSqlJobContent</p>
    <p>Új AzureSqlJobContent</p>
    <p>Set-AzureSqlJobContentDefinition</p>
    </td>
  </tr>
  <tr>
    <td>Adatbázis cél</td>
    <td>Mutasson az Azure SQL-adatbázis adatbázis és a kiszolgáló nevét.

    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    </td>
  </tr>
  <tr>
    <td>Shard térkép cél</td>
    <td>Egy adatbázis cél és egy hitelesítő adatokat rugalmas adatbázis shard interaktív tárolva megállapításához használandó kombinációja.
    </td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    <p>Set-AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Egyéni webhelycsoport-tároló</td>
    <td>Végrehajtási közösen használni kívánt adatbázisokat definiált csoportja.</td>
    <td>
    <p>Get-AzureSqlJobTarget</p>
    <p>Új AzureSqlJobTarget</p>
    </td>
  </tr>
<tr>
    <td>Egyéni webhelycsoport alárendelt cél</td>
    <td>Adatbázis cél hivatkozott egy egyéni gyűjteményből.</td>
    <td>
    <p>AzureSqlJobChildTarget hozzáadása</p>
    <p>Eltávolítás-AzureSqlJobChildTarget</p>
    </td>
  </tr>

<tr>
    <td>Feladat</td>
    <td>
    <p>A végrehajtás elindítása vagy ütemezett teljesítéséhez használható feladat paramétereinek meghatározása.</p>
    </td>
    <td>
    <p>Get-AzureSqlJob</p>
    <p>Új AzureSqlJob</p>
    <p>Set-AzureSqlJob</p>
    </td>
  </tr>

<tr>
    <td>Feladat végrehajtását</td>
    <td>
    <p>Egy parancsfájl, vagy egy DACPAC alkalmazása a célalkalmazás hitelesítő adatok használata az adatbázis-kapcsolatot a hibák teljesítéséhez szükséges feladatokat tároló végrehajtási házirendhez szerint kezeli.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Kezdés-AzureSqlJobExecution</p>
    <p>Leállítás-AzureSqlJobExecution</p>
    <p>Várakozás-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Feladat-végrehajtás feladat</td>
    <td>
    <p>Munka egyetlen egysége a feladat teljesítése érdekében.</p>
    <p>Ha egy projektfeladat sikeresen nem tudja, hogy hajtsa végre, a rendszer naplózza az eredményül kapott kivételhiba üzenete és egy új egyező projektfeladat létrejön és a megadott végrehajtási házirend összhangban hajtja végre.</p></p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecution</p>
    <p>Kezdés-AzureSqlJobExecution</p>
    <p>Leállítás-AzureSqlJobExecution</p>
    <p>Várakozás-AzureSqlJobExecution</p>
  </tr>

<tr>
    <td>Feladat-végrehajtás házirend</td>
    <td>
    <p>Vezérlők feladat-végrehajtás időtúllépései, újrapróbálkozási korlátai és intervallumok újrapróbálkozások között.</p>
    <p>Rugalmas adatbázis feladatok egy alapértelmezett-feladat végrehajtása szabályt, amely az egyes kísérletek gyakoriságát exponenciális visszalépési okozhat a feladat tevékenység hibák lényegében végtelen újrapróbálkozások tartalmazza.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobExecutionPolicy</p>
    <p>Új AzureSqlJobExecutionPolicy</p>
    <p>Set-AzureSqlJobExecutionPolicy</p>
    </td>
  </tr>

<tr>
    <td>Ütemterv</td>
    <td>
    <p>Ideje végrehajtására kerüljön sor a feladatról időköz vagy egyetlen egyszerre specifikációja alapján.</p>
    </td>
    <td>
    <p>Get-AzureSqlJobSchedule</p>
    <p>Új AzureSqlJobSchedule</p>
    <p>Set-AzureSqlJobSchedule</p>
    </td>
  </tr>

<tr>
    <td>Feladat indítók</td>
    <td>
    <p>Megfeleltetés egy feladatot, és az ütemezést eseményindítók feladat végrehajtását az ütemezés szerint között.</p>
    </td>
    <td>
    <p>Új AzureSqlJobTrigger</p>
    <p>Eltávolítás-AzureSqlJobTrigger</p>
    </td>
  </tr>
</table>

## <a name="supported-elastic-database-jobs-group-types"></a>Támogatott rugalmas adatbázis feladatok csoport típusok
A feladat végrehajtja a Transact-SQL nyelvben (T-SQL) parancsfájlok vagy DACPACs alkalmazását át az adatbázisok csoport. A feladat át az adatbázisok csoport végrehajtandó leadásakor a feladat "kibővíti" a gyermek feladatok, ahol minden hajt végre olyan egyetlen adatbázison kért végrehajtása a csoport be. 
 
A csoportok hozható létre két típusa van: 

* [Shard térkép](sql-database-elastic-scale-shard-map-management.md) csoportban: egy feladat shard térkép célba elküldésekor a feladat kérdezi le az aktuális készlete shards határozza meg a shard térkép, és ekkor létrehoz gyermek minden shard feladatok shard térképen.
* Egyéni webhelycsoport csoport: egyéni definiált adatbázisok csoportja. Amikor egy feladatot egy egyéni webhelycsoport célként, létrehoz gyermek-adatbázisonként feladatok jelenleg az egyéni gyűjteményben.

## <a name="to-set-the-elastic-database-jobs-connection"></a>A rugalmas adatbázis beállítása feladatok a kapcsolat

Kapcsolat van szüksége, a feladatok *vezérlő adatbázis* a feladatok API-k használata előtt kell állítani. A következő parancsmagot eseményindítók bukkan fel a felhasználónevet és jelszót rugalmas adatbázis feladatok telepítésekor létrehozott kérő hitelesítő adatok ablak. Ez a témakör belül minden példák tegyük fel, az első lépést már már elvégzett.

Nyissa meg az adatbázis rugalmas feladatok kapcsolat:

    Use-AzureSqlJobConnection -CurrentAzureSubscription 

## <a name="encrypted-credentials-within-the-elastic-database-jobs"></a>Titkosított hitelesítő adatokat rugalmas adatbázis feladatok belül

Adatbázis hitelesítő adatai a feladatok *vezérlő adatbázis* szúrhatók be a jelszóval titkosított. Ahhoz, hogy feladatokat végrehajtani, várjon egy kis időt (használatával a projekt ütemezését) hitelesítő adatainak tárolása szükség.
 
Titkosítási működik, a telepítési parancsfájlt részeként létrehozott tanúsítvány keresztül. A telepítési parancsfájlt hoz létre, és a tanúsítvány feltölti a titkosított jelszavait visszafejtése Azure felhő helyezését. Az Azure Felhőszolgáltatásba később tárolja a nyilvános kulcs a feladatok *vezérlő adatbázist* , amely lehetővé teszi, hogy a PowerShell API vagy klasszikus portál Azure felületet anélkül, hogy a tanúsítvány helyileg telepítve legyen a megadott titkosítás belül.
 
A hitelesítő adatok jelszavak titkosított és biztonságos a csak olvasható hozzáféréssel rendelkező felhasználó rugalmas adatbázis-feladatok objektumok. De rosszindulatú felhasználó rugalmas adatbázis feladatok objektumok írási és olvasási hozzáférést jelszó kibontásához. Hitelesítő adatok célja, hogy a feladat végrehajtások ismételten használhatók. Hitelesítő adatok át a céladatbázis kapcsolatok létrehozásakor. Jelenleg köre nincs korlátozva az egyes hitelesítő használt céladatbázis, rosszindulatú felhasználó hozzáadhat a rosszindulatú felhasználó felügyelheti adatbázis adatbázis cél. A felhasználó azt követően is az adatbázis jelszavát a hitelesítő adatok eléréséhez kiválasztásával feladat indítása.

Rugalmas adatbázis feladatokhoz biztonsággal kapcsolatos gyakorlati tanácsok a következők:

* Korlátozza a megbízható személyekhez API-k használatát.
* Hitelesítő adatok a legalacsonyabb projektfeladat végrehajtásához szükséges jogosultságokkal kell rendelkeznie.  További információt a [hitelesítés és engedélyek](https://msdn.microsoft.com/library/bb669084.aspx) SQL Server MSDN-cikk is láthatja.

### <a name="to-create-an-encrypted-credential-for-job-execution-across-databases"></a>Egy feladat végrehajtását titkosított hitelesítő adatainak létrehozása adatbázisok között

Hozzon létre egy új titkosított hitelesítő adatait, hogy a [**Get-hitelesítőadat-parancsmag**](https://technet.microsoft.com/library/hh849815.aspx) kér, felhasználónevet és jelszót, amelyet a [**New-AzureSqlJobCredential parancsmag**](https://msdn.microsoft.com/library/mt346063.aspx)juthat el.

    $credentialName = "{Credential Name}"
    $databaseCredential = Get-Credential
    $credential = New-AzureSqlJobCredential -Credential $databaseCredential -CredentialName $credentialName
    Write-Output $credential

### <a name="to-update-credentials"></a>Hitelesítő adatok frissítése

Ha módosítja a jelszavakat, a [**Set-AzureSqlJobCredential parancsmag**](https://msdn.microsoft.com/library/mt346062.aspx) , és állítsa be a **CredentialName** paramétert.

    $credentialName = "{Credential Name}"
    Set-AzureSqlJobCredential -CredentialName $credentialName -Credential $credential 

## <a name="to-define-an-elastic-database-shard-map-target"></a>Egy rugalmas adatbázis shard térkép cél meghatározása

Egy shard beállítása ( [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md)használatával létrehozott) adatbázisokra szemben a feladat végrehajtásához használja a shard térkép adatbázis célhelyként. Ez a példa a rugalmas adatbázis ügyfél tár használatával létrehozott sharded alkalmazás szükséges. [Első lépések rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md)című témakör tartalmaz.

A shard térkép manager-adatbázis adatbázis cél kell beállítani, és kattintson az adott shard térkép meg kell adni a célként.

    $shardMapCredentialName = "{Credential Name}"
    $shardMapDatabaseName = "{ShardMapDatabaseName}" #example: ElasticScaleStarterKit_ShardMapManagerDb
    $shardMapDatabaseServerName = "{ShardMapServerName}"
    $shardMapName = "{MyShardMap}" #example: CustomerIDShardMap
    $shardMapDatabaseTarget = New-AzureSqlJobTarget -DatabaseName $shardMapDatabaseName -ServerName $shardMapDatabaseServerName
    $shardMapTarget = New-AzureSqlJobTarget -ShardMapManagerCredentialName $shardMapCredentialName -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapDatabaseServerName -ShardMapName $shardMapName
    Write-Output $shardMapTarget

## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Adatbázisok közötti végrehajtásához az SQL-T parancsfájl létrehozása

Végrehajtási az SQL-T parancsprogramjait esetén erősen ajánlott lehet elkészíteni őket, hogy [idempotent](https://en.wikipedia.org/wiki/Idempotence) és rugalmassá hibák szemben. Rugalmas adatbázis feladatok próbálkozik parancsfájl végrehajtása, valahányszor végrehajtása egy hiba, függetlenül attól, az osztályozási a hiba lép fel.

A [**New-AzureSqlJobContent parancsmag**](https://msdn.microsoft.com/library/mt346085.aspx) használatával létrehozása és mentése végrehajtási parancsfájl és paramétereket állíthat be a **- ContentName** és **- CommandText** .

    $scriptName = "Create a TestTable"

    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
        CREATE TABLE TestTable(
            TestTableId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="create-a-new-script-from-a-file"></a>Hozzon létre egy új parancsfájl-fájlból
Ha a T-SQL nyelvben parancsfájl belül a fájl meg van határozva, ezzel a paranccsal a parancsfájl importálása:

    $scriptName = "My Script Imported from a File"
    $scriptPath = "{Path to SQL File}"
    $scriptCommandText = Get-Content -Path $scriptPath
    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

### <a name="to-update-a-t-sql-script-for-execution-across-databases"></a>Az SQL-T parancsfájl végrehajtási elterjedjenek adatbázisok  

A PowerShell-parancsprogramot frissíti a meglévő parancsfájl az SQL-T parancs szövegét.

Állítsa be megfelelően a kívánt parancsfájl definíció kell állítani a következő változók:

    $scriptName = "Create a TestTable"
    $scriptUpdateComment = "Adding AdditionalInformation column to TestTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
    CREATE TABLE TestTable(
        TestTableId INT PRIMARY KEY IDENTITY,
        InsertionTime DATETIME2
    );
    END
    GO

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    ALTER TABLE TestTable
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO"

### <a name="to-update-the-definition-to-an-existing-script"></a>Frissítse a definíció meglévő parancsfájl

    Set-AzureSqlJobContentDefinition -ContentName $scriptName -CommandText $scriptCommandText -Comment $scriptUpdateComment 

## <a name="to-create-a-job-to-execute-a-script-across-a-shard-map"></a>Parancsfájl végrehajtani keresztül shard térkép feladat létrehozásához

A PowerShell-parancsprogramot egyes shard skála rugalmas shard leképezés keresztül indítja el a feladat végrehajtásához parancsfájl.

Állítsa be a következő változók a kívánt parancsfájl és a cél:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $credentialName = "{Credential Name}"
    $shardMapTarget = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName 
    $job = New-AzureSqlJob -ContentName $scriptName -CredentialName $credentialName -JobName $jobName -TargetId $shardMapTarget.TargetId
    Write-Output $job

## <a name="to-execute-a-job"></a>A feladat végrehajtásához 

A PowerShell-parancsprogramot egy már meglévő feladatot hajt végre:

A következő változó megfelelően a kívánt feladat nevét végrehajtott frissítése:

    $jobName = "{Job Name}"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="to-retrieve-the-state-of-a-single-job-execution"></a>Egy egyetlen feladat végrehajtását állapotának beolvasása

A [**Get-AzureSqlJobExecution parancsmag**](https://msdn.microsoft.com/library/mt346058.aspx) , és adja meg a **JobExecutionId** paraméter feladat végrehajtását állapotának megtekintéséhez.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecution = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId
    Write-Output $jobExecution

Ugyanazon a **Get-AzureSqlJobExecution** parancsmag használata a **IncludeChildren** paraméter gyermek feladat végrehajtások, nevezetesen minden feladat végrehajtását minden adatbázisban a feladat célként megadott állam állapotának megtekintéséhez.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="to-view-the-state-across-multiple-job-executions"></a>Az állapot megtekintése több feladat végrehajtások keresztül

A [**Get-AzureSqlJobExecution parancsmag**](https://msdn.microsoft.com/library/mt346058.aspx) még több választható paraméterek megjelenítéséhez több feladat végrehajtások, szűrőn keresztül a megadott paramétereket használható. A következő bemutat néhány használja a Get-AzureSqlJobExecution lehetséges módjai:

Minden aktív feladat a felső szintű végrehajtások beolvasása:

    Get-AzureSqlJobExecution

Az összes legfelső szintű feladat végrehajtások, beleértve az inaktív feladat végrehajtások beolvasása:

    Get-AzureSqlJobExecution -IncludeInactive

Az összes alárendelt feladat végrehajtások egy megadott feladat végrehajtását azonosítója, többek között az inaktív feladat végrehajtások beolvasása:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Ütemezett használatával létrehozott összes feladat végrehajtások beolvasásához / munka kombinációja, beleértve az inaktív feladatok:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Az összes feladat célba juttatása megadott shard térkép, beleértve az inaktív feladatok beolvasása:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Egy adott egyéni gyűjteményben, beleértve az inaktív feladatok kiválasztásával az összes feladat beolvasása:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
A feladat tevékenység végrehajtások belül egy adott feladat végrehajtását listáját beolvasása:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Feladat feladat-végrehajtás részletei beolvasása:

Az alábbi PowerShell parancsprogramot egy feladat feladat-végrehajtás, amely különösen hasznos végrehajtási hibákat tapasztal hibakeresése során részleteinek megtekintéséhez használható.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="to-retrieve-failures-within-job-task-executions"></a>Feladat tevékenység végrehajtások belül hibák beolvasása

Az **objektum JobTaskExecution** a a tevékenység egy tulajdonság, egy üzenet együtt életciklusáról tulajdonságait tartalmazza. Ha egy feladat feladat-végrehajtás nem sikerült, a életciklus tulajdonság *nem sikerült* állítja be, és az üzenet tulajdonság állítja be az eredményül kapott kivételhiba üzenete és a hozzá tartozó sorozattal. Ha nem sikerült egy feladatot, fontos, hogy nem sikerült egy adott feladathoz projektfeladatok részleteinek megtekintése.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="to-wait-for-a-job-execution-to-complete"></a>Egy feladat végrehajtását befejezéséhez léptetése

Az alábbi PowerShell parancsprogramot megvárja, amíg egy feladat feladat elvégzéséhez használható:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Egyéni végrehajtási házirend létrehozása

Rugalmas adatbázis feladatok támogatja a feladatok indításakor alkalmazható egyéni végrehajtás házirendek létrehozása.
  
Végrehajtási házirendek jelenleg definiálása teszi lehetővé:

* Név: Az adatvégrehajtás-házirend azonosítója.
* Feladat határideje: Teljes időt, mielőtt a feladat megszakad rugalmas adatbázis feladatok által.
* Kezdeti újrapróbálkozási időköz: Intervallum várakozás után első újra gombra.
* Maximális újrapróbálkozási időköz: Vonalvég újrapróbálkozási időszakok használni.
* Intervallum visszalépési együtthatóját újrapróbálkozási: Együtthatóját kiszámításához használt újrapróbálkozások között a következő időtartomány.  A következő képletet használja: (kezdeti újra Interval) * Math.pow ((időtartomány visszalépési együtthatót számítja ki.), (próbálkozások száma) - 2). 
* Kísérletek maximális száma: A maximális száma megpróbálja belül a feladat végrehajtásához.

Az alapértelmezett végrehajtás házirendet a következő értékeket használja:

* Név: Alapértelmezett végrehajtási házirend
* Feladat határideje: 1 hét
* Kezdeti újrapróbálkozási időköz: 100 ezredmásodpercben
* Maximális újrapróbálkozási időköz: 30 percben
* Intervallum együttható újra: 2
* Kísérletek maximális száma: 2 147 483 647

A kívánt végrehajtási házirend létrehozása:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval 
    -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Egyéni adatvégrehajtás-házirend módosítására

A frissíteni kívánt adatvégrehajtás házirend módosítása:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Egy feladat megszakítása

Rugalmas adatbázis feladatok lemondási kérelmeket, a feladatok támogatja.  Ha rugalmas adatbázis feladatok azt észleli, hogy jelenleg végrehajtott projekt lemondási kér, akkor próbálja le szeretné állítani a feladatot.

Kétféleképpen különböző, hogy rugalmas adatbázis feladatokat végezheti el a lemondási:

1. A Mégse jelenleg a tevékenységek végrehajtása: lemondást egy tevékenység aktuális futása közben lép fel, ha lemondást belül az aktuális végrehajtási méretarány a tevékenység kísérlet történt.  Példa: Ha jelenleg lemondást kísérlet során végrehajtott időigényes lekérdezés, az megszakítja a lekérdezést kísérlete lesz.
2. Újrapróbálkozások tevékenység megszakítása: lemondást a vezérlő szál lép fel, mielőtt a feladat végrehajtásához indul el, ha a vezérlőelem szál elkerülése a tevékenység indítása és deklarálása a kérést, mint megszakad.

A szülő feladat feladat lemondást kérésére a lemondás kérelem fog kell fogadja a szülő-feladat és az összes alárendelt feladatai.
 
A lemondás kérelem [**leállítása-AzureSqlJobExecution parancsmag**](https://msdn.microsoft.com/library/mt346053.aspx) , és állítsa be a **JobExecutionId** paramétert.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="to-delete-a-job-and-job-history-asynchronously"></a>Feladat törlése és előzményei aszinkron feladat

Rugalmas adatbázis feladatok támogatja a feladatok aszinkron törlését. A feladat is törlésre, és a rendszer törli a feladatot, és minden feladat előzmény minden feladat végrehajtások a feladat befejezése után. A rendszer automatikusan nem megszakítja a aktív feladat végrehajtások.  

Meghívása [**Leállítása-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346053.aspx) aktív feladat végrehajtások megszakításához.

Indíthatja el a feladat törlését, [**Eltávolítás-AzureSqlJob parancsmag**](https://msdn.microsoft.com/library/mt346083.aspx) , és állítsa be a **nyomtatási** paramétert.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="to-create-a-custom-database-target"></a>Egy egyéni adatbázis célalkalmazás létrehozása

Egyéni adatbázis célok közvetlen végrehajtása, illetve felvétele egy egyéni adatbázis csoporton belüli határozhatja meg. Például mert **Rugalmas adatbázis készletek** még nem közvetlenül támogatottak PowerShell API-k használata, létrehozhat egy egyéni adatbázis cél és egyéni adatbázis webhelycsoport cél, amely magában foglalja a készlet adatbázisokra.

Állítsa be megfelelően a kívánt adatbázis adatai a következő változók:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="to-create-a-custom-database-collection-target"></a>Egyéni adatbázis webhelycsoport cél létrehozása

A [**New-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) parancsmag használatával egyéni adatbázis webhelycsoport cél végrehajtásának engedélyezéséhez keresztül több adatbázis meghatározott célok meghatározása. Egy adatbázis-csoport létrehozása után az egyéni webhelycsoport-tároló társított lehet adatbázisok.

Állítsa be megfelelően a kívánt egyéni webhelycsoport cél konfigurációt a következő változók:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="to-add-databases-to-a-custom-database-collection-target"></a>Adatbázis hozzáadása egy egyéni adatbázis webhelycsoport cél

Egy adott egyéni gyűjteményre adatbázis hozhatja létre a [**Hozzáadás-AzureSqlJobChildTarget**](https://msdn.microsoft.comlibrary/mt346064.aspx) parancsmag.

    $databaseServerName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Tekintse át az adatbázisok adatbázis egyéni webhelycsoport cél belül

A [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) parancsmag használatával beolvasni a gyermek-adatbázisok egyéni adatbázis webhelycsoport cél belül. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Hozzon létre egy egyéni adatbázis webhelycsoport cél keresztül parancsfájl végrehajtásához feladatot

A [**New-AzureSqlJob**](https://msdn.microsoft.com/library/mt346078.aspx) parancsmag használatával hozzon létre egy projektet, az adatbázisok adatbázis egyéni webhelycsoport cél által meghatározott csoport szemben. Rugalmas adatbázis feladatok bontsa ki a feladat be az egyéni adatbázis webhelycsoport cél társított adatbázis mindegyike több alárendelt feladatokat, és a biztosítása, hogy a parancsfájl végrehajtása minden adatbázisban. Újra fontos, hogy parancsprogramokat idempotent rugalmassá a újrapróbálkozások lesz.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Adatgyűjtés adatbázisok között

A feladat segítségével át az adatbázisok csoport lekérdezés végrehajtása, és elküldi az eredményeket egy adott tábla. A táblázat minden egyes adatbázisból a lekérdezés eredményének megtekintéséhez utólag lekérdezhető. Lekérdezés végrehajtása adatbázisok számos különböző aszinkron módszert ez biztosít. Sikertelen kísérletek keresztül újrapróbálkozások automatikusan kezeli.

A megadott céltábla automatikusan létrejönnek, ha még nem létezik. Az új tábla megfelelő a visszaadott eredmény beállítása sémája. Ha egy parancsprogramot több eredményhalmazt ad vissza, rugalmas adatbázis feladatok csak küldése az első a céltábla.

Az alábbi PowerShell parancsprogramot végrehajtja a parancsfájlt, és az eredmények gyűjti össze a megadott táblába. Ez a parancsfájl feltételezi, hogy az SQL-T parancsfájl létrehozás amely exportálja az eredményhalmaz egyetlen és egyéni adatbázis webhelycsoport cél létrehozott egy.

A parancsfájl a [**Get-AzureSqlJobTarget**](https://msdn.microsoft.com/library/mt346077.aspx) parancsmag. A paraméterek parancsfájl, a hitelesítő adatok és a végrehajtás cél beállításához:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="to-create-and-start-a-job-for-data-collection-scenarios"></a>Létrehozása és a feladat adatok gyűjtése felhasználási területei

A parancsfájl a [**Kezdés-AzureSqlJobExecution**](https://msdn.microsoft.com/library/mt346055.aspx) parancsmag.
 
    $job = New-AzureSqlJob -JobName $jobName 
    -CredentialName $executionCredentialName 
    -ContentName $scriptName 
    -ResultSetDestinationServerName $destinationServerName 
    -ResultSetDestinationDatabaseName $destinationDatabaseName 
    -ResultSetDestinationSchemaName $destinationSchemaName 
    -ResultSetDestinationTableName $destinationTableName 
    -ResultSetDestinationCredentialName $destinationCredentialName 
    -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="to-schedule-a-job-execution-trigger"></a>Egy feladat végrehajtását eseményindító ütemezése

Az alábbi PowerShell parancsprogramot ismétlődő ütemezést létrehozására használható. A parancsfájl használja a percek időköz, de [**Új-AzureSqlJobSchedule**](https://msdn.microsoft.com/library/mt346068.aspx) is támogat – DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paraméterek. Hozható létre, amely csak egyszer végrehajtása ütemezések szerint áthaladó - alkalommal.

Ütemterv készítése:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule 
    -MinuteInterval $minuteInterval 
    -ScheduleName $scheduleName 
    -StartTime $startTime 
    Write-Output $schedule

### <a name="to-trigger-a-job-executed-on-a-time-schedule"></a>A feladatok végrehajtása ütemezést indíthatja el

Egy feladat eseményindító, hogy az ütemezést szerint végrehajtott projekt definiálható. Az alábbi PowerShell parancsprogramot az eseményindító feladat létrehozásához használható.

[Új-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346069.aspx) használja, és adja meg a következő változók felel meg a kívánt feladatot, és ütemezett:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger
    -ScheduleName $scheduleName
    –JobName $jobName
    Write-Output $jobTrigger

### <a name="to-remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Ha el szeretne távolítani egy ütemezett társítást ütemezés végrehajtása a feladat leállítása

Egy feladat eseményindító keresztül feladatról feladat végrehajtását megszakítja, hogy a feladat eseményindító távolíthatók el. Távolítsa el a feladat eseményindító le szeretné állítani a feladat végrehajtását [**eltávolítása-AzureSqlJobTrigger parancsmag**](https://msdn.microsoft.com/library/mt346070.aspx)ütemezés szerint.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger 
    -ScheduleName $scheduleName 
    -JobName $jobName

### <a name="retrieve-job-triggers-bound-to-a-time-schedule"></a>Feladat indítók ütemezést kötött beolvasása

Az alábbi PowerShell parancsprogramot beszerzése, és megjeleníti az adott időpontban ütemezett regisztrált feladat eseményindítók használható.

    $scheduleName = "{Schedule Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -ScheduleName $scheduleName
    Write-Output $jobTriggers

### <a name="to-retrieve-job-triggers-bound-to-a-job"></a>Feladat indítók beolvasásához kötött a projekthez 

[Get-AzureSqlJobTrigger](https://msdn.microsoft.com/library/mt346067.aspx) segítségével beszerzése és regisztrált feladatot tartalmazó kimutatások megjelenítéséhez.

    $jobName = "{Job Name}"
    $jobTriggers = Get-AzureSqlJobTrigger -JobName $jobName
    Write-Output $jobTriggers

## <a name="to-create-a-data-tier-application-dacpac-for-execution-across-databases"></a>Az adatbázisok közötti végrehajtási adatok szintű alkalmazás (DACPAC) létrehozása

Hozzon létre egy DACPAC, lásd: az [adatok szintű alkalmazásokat](https://msdn.microsoft.com/library/ee210546.aspx). A [New-AzureSqlJobContent parancsmag](https://msdn.microsoft.com/library/mt346085.aspx)használatával egy DACPAC telepítéséhez. A DACPAC elérhető a szolgáltatás kell lennie. Töltse fel a létrehozott DACPAC Azure tárhely és a [Megosztott Access-aláírás](../storage/storage-dotnet-shared-access-signature-part-1.md) létrehozása az DACPAC az ajánlott.

    $dacpacUri = "{Uri}"
    $dacpacName = "{Dacpac Name}"
    $dacpac = New-AzureSqlJobContent -DacpacUri $dacpacUri -ContentName $dacpacName 
    Write-Output $dacpac

### <a name="to-update-a-data-tier-application-dacpac-for-execution-across-databases"></a>A végrehajtási adatok szintű alkalmazás (DACPAC) elterjedjenek adatbázisok

Meglévő DACPACs belül rugalmas adatbázis feladatok regisztrált naprakésszé tehetők mutasson az új URL-címe. A [**Set-AzureSqlJobContentDefinition parancsmag**](https://msdn.microsoft.com/library/mt346074.aspx) használatával egy meglévő regisztrált DACPAC tartozó DACPAC URI frissítése:

    $dacpacName = "{Dacpac Name}"
    $newDacpacUri = "{Uri}"
    $updatedDacpac = Set-AzureSqlJobDacpacDefinition -ContentName $dacpacName -DacpacUri $newDacpacUri
    Write-Output $updatedDacpac

## <a name="to-create-a-job-to-apply-a-data-tier-application-dacpac-across-databases"></a>Feladat történő alkalmazása egy adatok szintű alkalmazás (DACPAC) adatbázisok létrehozásához

Egy DACPAC belül rugalmas adatbázis feladatok hoztak létre, miután történő adatbázisok beállításcsoport a DACPAC alkalmazása egy feladatot hozhat létre. Az alábbi PowerShell parancsprogramot adatbázisok egyéni körében DACPAC feladat létrehozásához használható:

    $jobName = "{Job Name}"
    $dacpacName = "{Dacpac Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget 
    -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob 
    -JobName $jobName 
    -CredentialName $credentialName 
    -ContentName $dacpacName -TargetId $target.TargetId
    Write-Output $job 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-powershell/cmd-prompt.png
[2]: ./media/sql-database-elastic-jobs-powershell/portal.png
<!--anchors-->
