<properties
    pageTitle="Rugalmas adatbázis feladatok használatába"
    description="Rugalmas adatbázis feladatok használata"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Rugalmas adatbázis feladatok használatába

Rugalmas adatbázis feladatok (előzetes verzió) Azure SQL-adatbázis lehetővé teszi, hogy a megbízhatóság futtatása közben automatikusan újra próbálkozik, és kezeléséről a esetleges teljesítése garanciákkal több adatbázisok átterjedő parancsfájlokat az SQL-T. A rugalmas adatbázis feladat szolgáltatással kapcsolatos további tudnivalókért lásd a [szolgáltatás áttekintése lapon](sql-database-elastic-jobs-overview.md).

Ez a témakör a minta [rugalmas Adatbáziseszközök – első lépések](sql-database-elastic-scale-get-started.md)a található kiterjed. Amikor befejeződik, fog: megtudhatja, hogy miként hozhat létre és kezelhet csoport kapcsolódó az adatbázisok kezelése feladatokat. Nincs szükség a skála rugalmas eszközeivel annak érdekében, hogy rugalmas feladatok előnyei előnyeit.

## <a name="prerequisites"></a>Előfeltételek

Töltse le, és futtassa az [első lépések rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Hozzon létre egy shard térkép manager a minta alkalmazással

Itt hoz létre shard térkép manager több shards, az adatok beszúrását követi azokat a shards együtt. Már shards állíthat be őket a sharded adatokkal, hagyja ki az alábbi lépéseket, és helyezze át a következő szakaszban.

1. Építse fel, és futtassa az **első lépések rugalmas Adatbáziseszközök** minta alkalmazást. Kövesse a-ig lépés [letöltéséhez és a minta alkalmazás futtatásához](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools)a szakasz 7. Lépés 7 végén jelennek meg a következő parancssort:

    ![parancssor][1]

2.  A parancssorablakban írja be az "1", és nyomja le az **ENTER billentyűt**. Hoz létre a shard térkép manager, és összeadja a két shards a kiszolgálóra. Írja be a "3", majd nyomja le az **ENTER billentyűt**; Ismételje meg ezt a műveletet négyszer kell lenyomni. Ez a shards minta adatsorok szúrja be.

3.  Az [Azure-portálon](https://portal.azure.com) három új adatbázist a v12 kiszolgáló jelenítsen meg:

    ![A Visual Studióban megerősítő][2]

    Ezen a ponton azt hoz létre egy egyéni adatbázis gyűjteménye, amely a shard térkép adatbázisokra tükrözi. Ez lehetővé us hozhat létre, és hajtsa végre a feladatot, vegye fel az új tábla shards keresztül.

Itt azt volna általában shard térkép cél létrehozásához, a **New-AzureSqlJobTarget** parancsmaggal. A shard térkép manager-adatbázis adatbázis cél kell állítani, és kattintson a az adott shard térkép célként megadott. Ehelyett fogjuk számba veheti a kiszolgáló adatbázisokra, és az adatbázisok hozzáadása a fő adatbázist kivételével az új egyéni webhelycsoport.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Létrehoz egy egyéni webhelycsoport, és hozzáadja a minden adatbázisban a kiszolgálón az egyéni webhelycsoport célba fő kivételével.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Adatbázisok közötti végrehajtásához az SQL-T parancsfájl létrehozása

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Az adatbázisok az egyéni csoporthoz különböző parancsfájl végrehajtani a feladat létrehozása

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>A feladat végrehajtása 

Egy meglévő projektet hajtsa végre az alábbi PowerShell parancsprogramot használható:

A következő változó megfelelően a kívánt feladat nevét végrehajtott frissítése:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Egy egyetlen feladat végrehajtását állapotának beolvasása

Ugyanazon a **Get-AzureSqlJobExecution** parancsmag használata a **IncludeChildren** paraméter gyermek feladat végrehajtások, nevezetesen minden feladat végrehajtását minden adatbázisban a feladat célként megadott állam állapotának megtekintéséhez.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Több projekt végrehajtások végig az állapot megtekintése

A **Get-AzureSqlJobExecution** parancsmag több választható paraméterek megjelenítéséhez több feladat végrehajtások, szűrőn keresztül a megadott paramétereket használható tartalmaz. A következő bemutat néhány használja a Get-AzureSqlJobExecution lehetséges módjai:

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

## <a name="retrieve-failures-within-job-task-executions"></a>Feladat tevékenység végrehajtások belül hibák beolvasása

A JobTaskExecution objektum egy tulajdonságot a életciklusáról a tevékenység egy tulajdonság, egy üzenet együtt tartalmaz. Ha egy feladat feladat-végrehajtás nem sikerült, a életciklus tulajdonság *nem sikerült* állítja be, és az üzenet tulajdonság állítja be az eredményül kapott kivételhiba üzenete és a hozzá tartozó sorozattal. Ha nem sikerült egy feladatot, fontos, hogy nem sikerült egy adott feladathoz projektfeladatok részleteinek megtekintése.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Várakozás a befejezéséhez egy feladat végrehajtását

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
* Intervallum visszalépési együttható újrapróbálkozási: Együttható újrapróbálkozások következő különbségének kiszámításához használt.  A következő képletet használja: (kezdeti újra Interval) * Math.pow ((időtartomány visszalépési együtthatót számítja ki.), (próbálkozások száma) - 2). 
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
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
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

Rugalmas adatbázis feladatok feladatok lemondási kérelmeket támogatja.  Ha rugalmas adatbázis feladatok azt észleli, hogy jelenleg végrehajtott projekt lemondási kér, akkor próbálja le szeretné állítani a feladatot.

Kétféleképpen különböző, hogy rugalmas adatbázis feladatokat végezheti el a lemondási:

1. Lemondás a jelenleg végrehajtása tevékenységek: lemondást egy tevékenység aktuális futása közben lép fel, ha lemondást belül az aktuális végrehajtási méretarány a tevékenység kísérlet történt.  Példa: Ha jelenleg lemondást kísérlet során végrehajtott időigényes lekérdezés, az megszakítja a lekérdezést kísérlete lesz.
2. Sztornószámla tevékenység ismétlés: Lemondást a vezérlő szál lép fel, mielőtt a feladat végrehajtásához indul el, ha a vezérlő szál fog elkerülése a feladat megnyitása és deklarálása a kérést, mint megszakad.

A szülő feladat feladat lemondást kérésére a lemondás kérelem fog kell fogadja a szülő-feladat és az összes alárendelt feladatai.
 
A lemondás kérelem **Leállítása-AzureSqlJobExecution** parancsmag, és állítsa be a **JobExecutionId** paramétert.

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Egy feladat nevét és a feladatok előzményeinek törlése

Rugalmas adatbázis feladatok támogatja a feladatok aszinkron törlését. A feladat is törlésre, és a rendszer törli a feladatot, és minden feladat előzmény minden feladat végrehajtások a feladat befejezése után. A rendszer automatikusan nem megszakítja a aktív feladat végrehajtások.  

Ehelyett leállítása-AzureSqlJobExecution aktív feladat végrehajtások megszüntetése lehet hivatkozni.

Indíthatja el a feladat törlését, **Eltávolítás-AzureSqlJob** parancsmag, és állítsa be a **nyomtatási** paramétert.

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Hozzon létre egy egyéni adatbázis cél
Egyéni adatbázis célok rugalmas adatbázis feladatok végrehajtása közvetlenül vagy felvétele egy egyéni adatbázis csoporton belüli felhasználható definiálható. Mivel **Rugalmas adatbázis készletek** még nem közvetlenül támogatottak a PowerShell API-k keresztül, akkor egyszerűen hozhat létre egy egyéni adatbázis cél és egyéni adatbázis webhelycsoport cél, amely magában foglalja a készlet adatbázisokra.

Állítsa be megfelelően a kívánt adatbázis adatai a következő változók:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Hozzon létre egy egyéni adatbázis webhelycsoport cél
Egyéni adatbázis webhelycsoport cél végrehajtás ahhoz, hogy több adatbázis meghatározott célok keresztül definiálható. Egy adatbázis csoport létrehozását követően adatbázisok társítható az egyéni webhelycsoport célba.

Állítsa be megfelelően a kívánt egyéni webhelycsoport cél konfigurációt a következő változók:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Egy egyéni adatbázis webhelycsoport célba-adatbázisok hozzáadása

Egyéni adatbázis webhelycsoport célok hozzon létre egy csoportot az adatbázisok adatbázis célok társítható. Amikor egy projekt egyéni adatbázis webhelycsoport cél függvénynél jön létre, azt kibontva jelennek meg a végrehajtás időben a csoporthoz társított adatbázisok célba.

A kívánt adatbázis hozzáadása egy egyéni gyűjteményre:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Tekintse át az adatbázisok adatbázis egyéni webhelycsoport cél belül

A **Get-AzureSqlJobTarget** parancsmag használatával beolvasni a gyermek-adatbázisok egyéni adatbázis webhelycsoport cél belül. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Hozzon létre egy egyéni adatbázis webhelycsoport cél keresztül parancsfájl végrehajtásához feladatot

A **New-AzureSqlJob** parancsmag használatával hozzon létre egy projektet, az adatbázisok adatbázis egyéni webhelycsoport cél által meghatározott csoport szemben. Rugalmas adatbázis feladatok bontsa ki a feladat be az egyéni adatbázis webhelycsoport cél társított adatbázis mindegyike több alárendelt feladatokat, és a biztosítása, hogy a parancsfájl végrehajtása minden adatbázisban. Újra fontos, hogy parancsprogramokat idempotent rugalmassá a újrapróbálkozások lesz.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Adatgyűjtés adatbázisok között

**Rugalmas adatbázis feladatok** támogatja a lekérdezés végrehajtása az adatbázisok csoport keresztül, és az eredmények küld a megadott adatbázistáblát. A táblázat minden egyes adatbázisból a lekérdezés eredményének megtekintéséhez utólag lekérdezhető. Ezzel a megoldással egy aszinkron mechanizmusa lekérdezés végrehajtása sok adatbázis keresztül. Hiba esetek például átmenetileg nem érhető el, hogy az adatbázisok valamelyikével keresztül újrapróbálkozások automatikusan kezeli.

A megadott céltábla automatikusan létrejönnek, ha azt még nem létezik, a visszaadott eredmény beállítása sémájának egyező. Ha egy parancsfájl végrehajtása több eredményhalmazt ad vissza, rugalmas adatbázis feladatok csak küld a megadott céltábla első alkalommal.

Az eredmények gyűjteni megadott táblába parancsfájl hajtsa végre az alábbi PowerShell parancsprogramot használható. A parancsfájl feltételezi, hogy az SQL-T parancsfájl amely exportálja az eredményhalmaz egyetlen létrehozva és egyéni adatbázis webhelycsoport cél létrehozva.

Állítsa be a kívánt parancsfájl, a hitelesítő adatok és a végrehajtás cél a következőket:

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

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Hozzon létre, és indítsa el a feladat adatok gyűjtése felhasználási területei
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Hozzon létre egy feladat eseményindító feladat végrehajtását ütemezése

Az alábbi PowerShell parancsprogramot feladatról ütemezett létrehozására használható. A parancsfájl egy egy perc intervallum, de új-AzureSqlJobSchedule is támogat – DayInterval, - HourInterval, - MonthInterval, és - WeekInterval paraméterek. Hozható létre, amely csak egyszer végrehajtása ütemezések szerint áthaladó - alkalommal.

Ütemterv készítése:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Egy feladat eseményindító, hogy az ütemezést végrehajtott projekt létrehozása

Egy feladat eseményindító, hogy az ütemezést szerint végrehajtott projekt definiálható. Az alábbi PowerShell parancsprogramot az eseményindító feladat létrehozásához használható.

A következő változók felel meg a kívánt projektet és az ütemterv beállítása:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Az ütemezés végrehajtása feladat leállítása egy ütemezett társítást eltávolítása

Egy feladat eseményindító keresztül feladatról feladat végrehajtását megszakítja, hogy a feladat eseményindító távolíthatók el. Távolítsa el a feladat eseményindító le szeretné állítani a feladat végrehajtását a **Eltávolítása-AzureSqlJobTrigger** parancsmaggal ütemezés szerint.

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Importálása az Excel rugalmas adatbázis-lekérdezés eredménye

 Az eredmények a lekérdezés az Excel-fájl importálása

1. Indítsa el az Excel 2013-ban.
2.  Nyissa meg az **adatok** menüszalagján.
3.  Kattintson **A más forrásokból** , és kattintson **Az SQL Server**parancsra.

    ![Az Excel importálása más forrásokból][5]
4.  Írja be a kiszolgáló nevét, és jelentkezzen be az adatokat az **Adatkapcsolat varázsló utasításait** . Kattintson a **Tovább gombra**.
5.  **Jelölje ki a kívánt adatokat tartalmazó adatbázist**párbeszédpanelen jelölje ki a **ElasticDBQuery** adatbázist.
6.  A listanézetben jelölje ki a **Vevők** táblát, és kattintson a **Tovább**gombra. Kattintson a **Befejezés gombra**.
7.  Az **Adatok importálása** formában csoportban **Adja meg, hogy ez a munkafüzet adatainak megtekintéséhez**jelölje ki a **táblát** , és kattintson az **OK gombra**.

**Vevők** táblában tárolt különböző shards az összes sort az Excel-munkalap adataival.

## <a name="next-steps"></a>Következő lépések
Most már az Excel-adatok függvények használhatók. A kapcsolati karakterlánc használata a kiszolgáló nevét, az adatbázis neve és a hitelesítő adatok adatbázishoz való csatlakozáshoz a BI és az adatok integrációs eszközök a rugalmas lekérdezést. Győződjön meg arról, hogy az eszköz adatforrásként az SQL Server támogatott. Olvassa el a rugalmas lekérdezés adatbázis és a külső táblázatok, mint bármely más SQL Server-adatbázishoz, és a eszközzel kíván csatlakozni SQL Server-táblák.

### <a name="cost"></a>Költség
További ingyenesek rugalmas adatbázis-lekérdezés funkcióval nem. Azonban egyelőre Ez a funkció csak a prémium adatbázisok, az egyik végpontját, de a shards bármely szolgáltatási réteg lehet.

Árak információkat talál [SQL-adatbázis árak részleteket](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
