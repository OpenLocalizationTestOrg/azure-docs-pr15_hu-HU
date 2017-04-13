<properties 
    pageTitle="Azure SQL-adatbázis tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure portál |} Microsoft Azure" 
    description="Tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure portálon Azure SQL-adatbázis" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Azure SQL-adatbázis tervezett vagy nem tervezett feladatátvevő kezdeményezése az Azure-portálra


> [AZURE.SELECTOR]
- [Azure portál](sql-database-geo-replication-failover-portal.md)
- [A PowerShell](sql-database-geo-replication-failover-powershell.md)
- [AZ SQL-T](sql-database-geo-replication-failover-transact-sql.md)


Ez a cikk bemutatja, hogyan az [Azure portál](http://portal.azure.com)másodlagos SQL-adatbázishoz való áttérés elindítására. Geo-replikáció beállításához olvassa el a [Geo-replikáció konfigurálása Azure SQL-adatbázis](sql-database-geo-replication-portal.md)című témakört.


## <a name="initiate-a-failover"></a>Feladatátvevő kezdeményezése

A másodlagos adatbázis lesz az elsődleges lehet másikra váltani.  

1. A [portál Azure](http://portal.azure.com) Tallózás a Geo replikációs partnerség az elsődleges adatbázishoz.
2. Válassza az SQL-adatbázis lap az **összes beállításai** > **Geo-replikáció**.
3. **Formátumú másodlagos zónák** , jelölje be az új elsődleges válik, és kattintson a **feladatátvevő**kívánt adatbázist.

    ![Feladatátvevő][2]

4. Kattintson az **Igen gombra** a feladatátvételi megkezdéséhez.

A parancs azonnal lesz a másodlagos adatbázis átváltani az elsődleges szerepet. 

Van egy rövid idő, ameddig mindkét adatbázisok nem érhetők el (a 0-25 másodperc) sorrendjéről közben vannak a szerepkörökhöz. Ha az elsődleges adatbázis több másodlagos adatbázisok, a a parancs automatikusan újra a többi formátumú másodlagos zónák az új elsődleges csatlakozhat. A teljes műveletet kell vennie egy perc alatt normál körülmények befejezéséhez. 

>[AZURE.NOTE] Ha az elsődleges online állapotban, és tranzakciók elvégzése, amikor a parancs az adatvesztést fordulhat.


## <a name="next-steps"></a>Következő lépések   

- Után feladatátvételt ellenőrizze a hitelesítési követelményeket, a kiszolgáló és az adatbázis meg az új elsődleges. A részletekért olvassa [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md).
- Megtudhatja, hogy aktív Geo replikációs, beleértve a előtti használatával katasztrófa után helyreállítása és helyreállítási lépéseket, és egy katasztrófa helyreállítási részletező elvégzéséhez, lásd: [Katasztrófa helyreállítási gyakorlatokat](sql-database-disaster-recovery.md)
- Sasha Nosov blogbejegyzésbe kapcsolatos aktív Geo replikációs olvassa el a [fókuszban lévő új Geo replikációs funkciók](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) című témakört.
- Aktív Geo replikációs használható felhő alkalmazások tervezéséről további tudnivalókért lásd [tervezése felhő alkalmazások használatával Geo replikációs üzemkészségének](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Rugalmas adatbázis készletek aktív Geo replikációs használatáról további tudnivalókért lásd [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Üzleti continurity áttekintése a [Üzleti folytonosságot áttekintése](sql-database-business-continuity.md) című témakörben találhat.




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
