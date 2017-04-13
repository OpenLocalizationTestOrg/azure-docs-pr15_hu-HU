<properties 
    pageTitle="SQL-adatbázis XEvent nézeteihez kódját |} Microsoft Azure" 
    description="Mintaként szolgáló kétfázisú kódot, amely bemutatja a nézeteihez célhelyet Azure SQL-adatbázis a bővített esemény PowerShell és a Transact-SQL nyelvben biztosít. Azure tároló ebben az esetben szükséges része." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Fájl cél eseménykód SQL-adatbázisban bővített eseményekre vonatkozóan:

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Teljes kód minta kívánt bővített esemény rögzítési és a jelentés információit robusztus lehetőséget.


A Microsoft SQL Server a [nézeteihez cél](http://msdn.microsoft.com/library/ff878115.aspx) esemény kimeneti értékeket egy helyi merevlemez-fájlba tárolására szolgál. De ilyen fájlok nem érhetők el Azure SQL-adatbázishoz. Az Azure tárhelyszolgáltatáshoz inkább a nézeteihez cél használható használjuk.


Ez a témakör bemutatja a minta kétfázisú kódot:


- PowerShell-Azure tároló tároló létrehozása a felhőben.

- A Transact-SQL nyelvben:
 - A tároló Azure tároló hozzárendelése egy nézeteihez cél.
 - Hozzon létre az esemény indítása és így tovább.


## <a name="prerequisites"></a>Előfeltételek


- Az Azure-fiók és az előfizetés. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)jelentkezhetnek.


- Bármely adatbázis táblájához is létrehozhat.
 - Másik lehetőségként hozhat [létre egy **AdventureWorksLT** bemutató adatbázis](sql-database-get-started.md) perc alatt.


- SQL Server Management Studio (ssms.exe), ideális esetben a legújabb havi frissítés verzióját. A legújabb ssms.exe a töltheti le:
 - [Töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx)című témakört.
 - [Közvetlen hivatkozás letöltéséhez.](http://go.microsoft.com/fwlink/?linkid=616025)


- [Azure PowerShell-modulok](http://go.microsoft.com/?linkid=9811175) telepítve kell rendelkeznie.
 - A modulokat például – **Új-AzureStorageAccount**parancsokat tartalmaznak.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>1 fázis: Azure tároló tároló PowerShell kódját


A PowerShell fázis 1 kétfázisú kód minta.

A parancsprogram szolgáló parancsok jelenjenek meg, miután egy lehetőség, előző futtatni, és rerunnable kezdődik.



1. Egyszerű szövegszerkesztőben, például Notepad.exe illessze be a PowerShell-parancsprogramot, és a parancsfájlt, a bővítmény **.ps1**fájlként mentheti.

2. Indítsa el a PowerShell ISE rendszergazdaként.

3. A parancssorba írja be a<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>és nyomja le az ENTER billentyűt.

4. A PowerShell ISE nyissa meg a **.ps1** fájlt. Futtassa a.

5. A parancsprogram először indítja el, amelyben bejelentkezik az Azure új ablakban.
 - A munkamenet megszakítása nélkül futtassa újra a parancsfájl, esetén a **Hozzáadás-AzureAccount** parancs a megjegyzések kényelmes lehetőséget.


![A PowerShell ISE, Azure modul telepítve van, készen áll a parancsfájl futtatására.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Ajánljuk figyelmébe a néhány megnevezett értékek, amelyek a PowerShell-parancsprogramot kinyomtat végződik. Ezeket az értékeket kell szerkesztenie a Transact-SQL nyelvben parancsfájl, a következő fázis 2 be.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>2 fázis: Azure tároló tároló használó Transact-SQL-kódot


- A kódot a következő példában 1 szakasza egy PowerShell-parancsprogramot létrehozása az Azure tároló tároló futtatta.
- Ezután a fázis 2, a következő Transact-SQL nyelvben parancsfájl kell használnia a tároló.


A parancsprogram szolgáló parancsok jelenjenek meg, miután egy lehetőség, előző futtatni, és rerunnable kezdődik.


A PowerShell-parancsprogramot, néhány megnevezett értékek nyomtatva azt befejeződött. Módosítania kell a Transact-SQL nyelvben parancsfájlt ezeket az értékeket. **Teendők** a Transact-SQL nyelvben parancsfájl, keresse meg a csomópontok szerkesztése találja.


1. Nyissa meg az SQL Server Management Studio (ssms.exe).

2. Az SQL Azure-adatbázis-adatbázis csatlakoztatása.

3. Kattintson egy új lekérdezést ablak megnyitásához.

4. A következő Transact-SQL nyelvben parancsprogram beillesztése a lekérdezés ablaktáblában.

5. Minden **teendő** keresse meg a parancsfájlt, és végezze el a szükséges szerkesztéseket.

6. Mentse, és futtassa a parancsfájl.


&nbsp;


> [AZURE.WARNING] A Társítások kulcsmező értékét az előző PowerShell parancsprogramot által generált kezdődhet a "?" (kérdőjel). Amikor a Társítások billentyűt a következőt az SQL-T használ, be kell *távolítsa el a sortávolságot "?"*. Egyéb biztonsági blokkolhatja a erőfeszítéseket.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Ha nem sikerül csatolása futtatásakor a cél, akkor leállíthatja, és indítsa újra az esemény munkamenetet:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Kimenet


A Transact-SQL nyelvben parancsfájl befejeztével kattintson a cella alatt a **event_data_XML** oszlop fejlécére. Egy **<event>** elem jelenik meg, amely egy UPDATE utasítás jeleníti meg.

Íme egy **<event>** elemet, amelyet a tesztelés során jött létre:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Az előző Transact-SQL-parancsfájlokat a következő rendszer függvényt használni, olvassa el a event_file:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Bővített események származó adatok megtekintésének speciális beállításainak magyarázata címen érhető el:

- [Speciális bővített események cél adatainak megtekintése](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Az SQL Server futtatásához a kód minta konvertálása


Tegyük fel, hogy az előző Transact-SQL nyelvben minta futtatásához a Microsoft SQL Server megy végbe.


- Az egyszerűség kedvéért célszerű teljesen **C:\myeventdata.xel**például egy egyszerű fájl cserélje ki az Azure tárterület-tároló használatára. A fájlt a számítógépen, amelyen az SQL Server helyi merevlemez-meghajtó szeretne írni.


- Bármilyen típusú Transact-SQL-utasításait a **DIAMINTA kulcs létrehozása** és **HITELESÍTŐ létrehozása**nem kellene.


- A **Munkamenet esemény létrehozása** kimutatásban a **Tároló hozzáadása** záradékban cserélje a hozzárendelt Http érték végzett **filename =** **C:\myfile.xel**például egy teljes elérési útját karakterlánccal.
 - Nincs Azure tárterület-fiók szükséges kell vonni.


## <a name="more-information"></a>További információ


Fiókok és Azure tárhelyszolgáltatáshoz a tárolók további információt találhat:

- [A .NET Blob-tárolóhoz használata](../storage/storage-dotnet-how-to-use-blobs.md)
- [Elnevezéséről és hivatkozó tárolók BLOB és metaadatok](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [A legfelső szintű tároló használata](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Lecketervező 1: Tárolt hozzáférési házirend és a átengedése aláírás létrehozása az Azure tároló a](http://msdn.microsoft.com/library/dn466430.aspx)
    - [2 a lecke: Hozzon létre egy SQL Server hitelesítő átengedése aláírás használata](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

