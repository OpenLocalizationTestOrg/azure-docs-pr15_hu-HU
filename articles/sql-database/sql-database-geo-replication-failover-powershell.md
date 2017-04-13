<properties 
    pageTitle="Tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure SQL-adatbázis PowerShell |} Microsoft Azure" 
    description="Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis PowerShell használatával" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Tervezett vagy nem tervezett feladatátvevő kezdeményezzen PowerShell Azure SQL-adatbázis



> [AZURE.SELECTOR]
- [Azure portál](sql-database-geo-replication-failover-portal.md)
- [A PowerShell](sql-database-geo-replication-failover-powershell.md)
- [AZ SQL-T](sql-database-geo-replication-failover-transact-sql.md)


Ez a cikk bemutatja, hogyan SQL-adatbázis PowerShell tervezett vagy nem tervezett feladatátvevő elindítására. Geo-replikáció beállításához olvassa el a [Geo-replikáció konfigurálása Azure SQL-adatbázis](sql-database-geo-replication-powershell.md)című témakört.



## <a name="initiate-a-planned-failover"></a>Tervezett feladatátvevő kezdeményezése

A **Set-AzureRmSqlDatabaseSecondary** parancsmag használata a **Átváltó** paraméter előléptetése válik az új elsődleges adatbázis meglévő az elsődleges, másodlagos válhat lefokozása másodlagos adatbázis. Ezt a funkciót készült tervezett feladatátvételt, például katasztrófa helyreállítási gyakorlatokat, alatt, és igényel, az elsődleges adatbázis érhető el.

A parancs az alábbi munkafolyamatot hajtja végre:

1. A replikáció ideiglenes szinkron módba vált. Ennek hatására a másodlagos kell kiürítette összes nyitott tranzakciók.

2. Váltás a szerepkörök, a két adatbázisok a Geo replikációs partnerség.  

A sorozat garantálja, hogy a két adatbázisa szinkronizálva van, váltás a szerepkörök, és ezért akkor fordul elő az adatvesztés előtt. Van egy rövid idő, ameddig mindkét adatbázisok nem érhetők el (a 0-25 másodperc) sorrendjéről közben vannak a szerepkörökhöz. A teljes műveletet kell vennie egy perc alatt normál körülmények befejezéséhez. További tudnivalókért olvassa el a [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx)című témakört.




Ezzel a parancsmaggal a másodlagos adatbázis átirányítása az elsődleges, a folyamat befejezése ad vissza.

A következő parancsot a szerepkörök, a kiszolgáló "KISZ2" csoportban az erőforrás csoport "rg2" az elsődleges "mydb" nevű adatbázist vált. Az eredeti elsődleges, amelyhez "db2" csatlakozott átvált másodlagos a két adatbázis teljes mértékben szinkronizálása után.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] A ritka esetben célszerű lehet, hogy a művelet nem hajtható végre, és előfordulhat, hogy nem válaszol. Ebben az esetben a felhasználó hívja fel a kötelező feladatátvevő parancs (tervezett feladatátvétel) és fogadja el az adatok elvesztését.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Az elsődleges adatbázisból tervezett áttérni a másodlagos adatbázis kezdeményezése


A **Set-AzureRmSqlDatabaseSecondary** parancsmag **– Feladatátvétel** és **- AllowDataLoss** paraméterekkel segítségével előléptetése válik az új elsődleges adatbázis tervezett módon másodlagos adatbázis a lefokozás meglévő az elsődleges, másodlagos lesz egy időben, ha az elsődleges adatbázis már nem érhető el a kényszer.

Ha elérhetőség az adatbázis visszaállítása kritikus és adatvesztést elfogadható készült vészhelyreállítás ezt a funkciót. Kényszerített feladatátvevő meghívásakor a megadott másodlagos adatbázis azonnal lesz az elsődleges adatbázist, és megkezdi a írási tranzakciók elfogadásával. Amint az eredeti elsődleges adatbázisa tudja a kényszerített feladatátvevő művelet után az új elsődleges adatbázist az újracsatlakozáshoz, növekményes biztonsági másolatot az eredeti elsődleges adatbázis hoznak, és a régi elsődleges adatbázist történik, az új elsődleges adatbázisnak; másodlagos adatbázisba Ezt követően érdemes csupán az új elsődleges replikáját.

De pont az idő visszaállítása nem támogatja a másodlagos adatbázisok, ha másnak szeretné a régi elsődleges adatbázishoz, amely az új elsődleges adatbázisba volna nem replikált lekötött helyreállítási adatok, mert meg kell folytatni az adatbázis visszaállítása az ismert napló Backup CSS.

> [AZURE.NOTE] Ha a parancs ki, ha online elsődleges és másodlagos vannak a régi elsődleges lesz az új másodlagos azonnal adatszinkronizálás nélkül. Ha az elsődleges végrehajtása tranzakciók, amikor a parancs az adatvesztést fordulhat.


Ha az elsődleges adatbázis több formátumú másodlagos zónák a parancs részben fog sikerülni. A másodlagos, amelyen a parancs végrehajtása Elsődleges válnak. A régi elsődleges azonban továbbra is elsődleges, azaz a két elsődleges érjen véget nem következetes állapotban felfüggesztett replikációs kapcsolat. A felhasználónak kell manuálisan javítása az ebben a konfigurációban "eltávolítása másodlagos" API-t használja ezeket az elsődleges adatbázisokat.


A következő parancs – Váltás a szerepkörök, az adatbázis neve "mydb", az elsődleges, ha az elsődleges nem érhető el. Az eredeti elsődleges, amelyhez "mydb" csatlakozott átvált másodlagos után vissza online. Ezen a ponton a szinkronizálás az adatok elvesztését vonhat.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Következő lépések   

- Után feladatátvételt ellenőrizze a hitelesítési követelményeket, a kiszolgáló és az adatbázis meg az új elsődleges. A részletekért olvassa [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md).
- Megtudhatja, hogy aktív Geo replikációs, beleértve a előtti használatával katasztrófa után helyreállítása és helyreállítási lépéseket, és egy katasztrófa helyreállítási részletező elvégzéséhez, lásd: [Katasztrófa helyreállítási gyakorlatokat](sql-database-disaster-recovery.md)
- Sasha Nosov blogbejegyzésbe kapcsolatos aktív Geo replikációs olvassa el a [fókuszban lévő új Geo replikációs funkciók](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) című témakört.
- Aktív Geo replikációs használható felhő alkalmazások tervezéséről további tudnivalókért lásd [tervezése felhő alkalmazások használatával Geo replikációs üzemkészségének](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Rugalmas adatbázis készletek aktív Geo replikációs használatáról további tudnivalókért lásd [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Üzleti continurity áttekintése a [Üzleti folytonosságot áttekintése](sql-database-business-continuity.md) című témakörben találhat.
