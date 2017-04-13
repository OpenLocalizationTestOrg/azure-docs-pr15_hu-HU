<properties 
    pageTitle="Egy PowerShell használatával Azure SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása |} Microsoft Azure" 
    description="Módosítsa a szolgáltatási réteg, és Azure SQL-adatbázis teljesítményszint bemutatja, hogyan méretezheti az SQL-adatbázis felfelé vagy lefelé a PowerShell. A PowerShell-Azure SQL-adatbázis árak réteg módosítása." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) PowerShell SQL-adatbázishoz


> [AZURE.SELECTOR]
- [Azure portál](sql-database-scale-up.md)
- [**A PowerShell**](sql-database-scale-up-powershell.md)


Szolgáltatási rétegek teljesítményszint le azokat a funkciókat és erőforrások érhető el az SQL-adatbázis és frissíthető, az alkalmazások módosítása az egyéni igényeknek. A részletekért olvassa [Szolgáltatási rétegek](sql-database-service-tiers.md).

Megjegyzendő, hogy a szolgáltatási réteg módosítása, illetve az adatbázis teljesítményszint hoz létre az eredeti adatbázis új teljesítmény szintre, és ezután átvált kapcsolatok a replika. Adatokat nem elvész folyamat során, de során a rövid pillanatig, amikor azt átkapcsolhatná a replika, az adatbázis-kapcsolatot le van tiltva, így néhány tranzakciók nézetbeli előfordulhat, hogy állítható vissza. Ebben az ablakban változik, de átlagosan 4 másodperc alatt, és a esetek 99 %-nél több nem legfeljebb 30 másodperc. Nagyon ritkán különösen akkor, ha létezik a nagyméretű számok nézetbeli szorzatmomentum kapcsolatok a tranzakciók le vannak tiltva ebben az ablakban lehetséges, hosszabb.  

Az időtartam, a teljes skála telefonos folyamat előtt és után a változtatások mindkét a méret és szolgáltatási réteg az adatbázis függ. Ha például van módosítani szeretné, az vagy egy szabványos szolgáltatási réteg belül 250 GB adatbázis 6 órán belül be kell fejeződnie. Az adatbázis teljesítményszint belül a támogatási szolgáltatás réteg módosítandó azonos méretű akkor be kell fejeződnie 3 órán belül.


- Vissza az adatbázis léptetheti, az adatbázis a cél szolgáltatási réteg maximális hossza kisebbnek kell lennie. 
- Adatbázis [Geo replikációs](sql-database-geo-replication-portal.md) engedélyezett frissítéskor először frissítenie kell a másodlagos adatbázisokat a kívánt teljesítmény réteg az elsődleges adatbázis frissítése előtt.
- Visszalépés az egy prémium szolgáltatási réteg-ról, amikor először lezárása kell minden Geo replikációs kapcsolat. Követheti a le szeretné állítani az elsődleges és másodlagos aktív adatbázisok közötti replikációs folyamat [egy üzemszünetek helyreállítása](sql-database-disaster-recovery.md) című részben ismertetett lépéseket.
- A visszaállítás szolgáltatásajánlatok a különböző szolgáltatási rétegek eltérőek. Ha meg van Visszalépés-ról, elvesznek a lehetősége, ha vissza szeretne állítani egy időben, vagy egy alsó biztonsági Adatmegőrzés időtartama van. További tudnivalókért olvassa el az [Azure SQL-adatbázis biztonsági mentése és visszaállítása](sql-database-business-continuity.md)című témakört.
- Az adatbázis új tulajdonságainak nem vonatkoznak a lépést mindaddig, amíg a módosítások befejezése.



**Ez a cikk elvégzéséhez az alábbiakra van szükség:**

- Egy Azure-előfizetést. Ha van szüksége az Azure előfizetéssel egyszerűen **Ingyenes FIÓKOT** a lap tetején kattintson, és ezután térjen vissza az Ez a cikk Befejezés.
- Microsoft Azure SQL-adatbázisban. Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Az SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása

Futtassa a [beállítása AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) parancsmag és beállítása a **-RequestedServiceObjectiveName** az teljesítmény szintű a kívánt árak réteg; például *S0*, *S1*, *S2*, *S3*, *P1*, *P2*...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Példa a PowerShell-parancsprogramot az SQL-adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása

Csere ```{variables}``` a (ne kerüljön bele a kapcsos zárójel) értékű.

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Következő lépések

- [És méretezése](sql-database-elastic-scale-get-started.md)
- [Csatlakozás és a lekérdezés SSMS SQL-adatbázishoz](sql-database-connect-query-ssms.md)
- [Exportálás az Azure SQL-adatbázishoz](sql-database-export-powershell.md)

## <a name="additional-resources"></a>További források

- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](http://azure.microsoft.com/documentation/services/sql-database/)
- [Az SQL azure-adatbázis parancsmagok] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/tár vagy azure/mt619433(v=azure.300\).aspx.aspx)