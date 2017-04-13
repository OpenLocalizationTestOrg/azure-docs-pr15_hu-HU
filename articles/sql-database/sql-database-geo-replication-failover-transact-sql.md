<properties 
    pageTitle="Tervezett vagy nem tervezett feladatátvevő kezdeményezzen a Transact-SQL Azure SQL-adatbázis |} Microsoft Azure" 
    description="Tervezett vagy nem tervezett feladatátvevő kezdeményezzen Azure SQL-adatbázis használata a Transact-SQL nyelvben" 
    services="sql-database" 
    documentationCenter="" 
    authors="CarlRabeler" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Tervezett vagy nem tervezett feladatátvevő kezdeményezzen a Transact-SQL Azure SQL-adatbázis


> [AZURE.SELECTOR]
- [Azure portál](sql-database-geo-replication-failover-portal.md)
- [A PowerShell](sql-database-geo-replication-failover-powershell.md)
- [AZ SQL-T](sql-database-geo-replication-failover-transact-sql.md)


Ez a cikk bemutatja, hogyan kezdeményezése egy másodlagos SQL-adatbázishoz való áttérés használata a Transact-SQL nyelvben. Geo-replikáció beállításához olvassa el a [Geo-replikáció konfigurálása Azure SQL-adatbázis](sql-database-geo-replication-transact-sql.md)című témakört.



Feladatátvevő kezdeményez, a következőkre lesz szüksége:

- Az elsődleges, DBManager található bejelentkezési van db_ownership a helyi adatbázis lehetővé teszi a geo-replikáció, és a partnerek kiszolgálói, amelyhez a replikáció Geo fog konfigurálni DBManager.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Az új elsődleges lesz a másodlagos adatbázis előléptetése tervezett feladatátvevő kezdeményezése

Az **Adatbázis módosítása** utasítást a meglévő az elsődleges, másodlagos válhat lefokozása tervezett módon, az új elsődleges adatbázis lesz a másodlagos adatbázis előléptetése is használhatja. Ez az utasítás végrehajtása a fő adatbázissal, amelyben a geo replikált másodlagos adatbázishoz, amely az előléptetett található Azure SQL-adatbázis logikai kiszolgálón. Ez a funkció készült tervezett feladatátvételt, például a DR gyakorlatokat során, és igényel, az elsődleges adatbázis érhető el.

A parancs az alábbi munkafolyamatot hajtja végre:

1. Ideiglenes – Váltás a replikáció szinkron módba, a másodlagos kell kiürítette összes nyitott tranzakciók okozó, így akadályozva az összes új tranzakciók;

2. Váltás a két adatbázisok a Geo replikációs partnerség a szerepköröket.  

A sorozat garantálja, hogy a két adatbázisa szinkronizálva van, váltás a szerepkörök, és ezért akkor fordul elő az adatvesztés előtt. Van egy rövid idő, ameddig mindkét adatbázisok nem érhetők el (a 0-25 másodperc) sorrendjéről közben vannak a szerepkörökhöz. Ha az elsődleges adatbázis több másodlagos adatbázisok, a a parancs automatikusan újra a többi formátumú másodlagos zónák az új elsődleges csatlakozhat.  A teljes műveletet kell vennie egy perc alatt normál körülmények befejezéséhez. További tudnivalókért lásd: [Az adatbázis módosítása (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/mt574871.aspx) és [Szolgáltatási rétegek](sql-database-service-tiers.md).


Kövesse az alábbi lépéseket a tervezett feladatátvevő elindítására.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz, egy másodlagos geo replikált adatbázist helyezkedik el.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Az alábbi **Adatbázis módosítása** utasítás segítségével a másodlagos adatbázis váltani az elsődleges szerepet.

        ALTER DATABASE <MyDB> FAILOVER;

4. Kattintson a **végrehajtás** futtassa a lekérdezést.

>[AZURE.NOTE] Ritkán akkor lehet, hogy a művelet nem hajtható végre, és lefagyott jelenhetnek meg. Ebben az esetben a felhasználó a kötelező feladatátvevő parancs végrehajtása és fogadja el az adatok elvesztését.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Az elsődleges adatbázisból tervezett áttérni a másodlagos adatbázis kezdeményezése

Az **Adatbázis módosítása** utasítás előléptetése válik az új elsődleges adatbázis tervezett módon másodlagos adatbázis meglévő az elsődleges, másodlagos lesz egy időben, ha az elsődleges adatbázismodell már nem érhető el, a lefokozás kényszerítése is használhatja. Ez az utasítás végrehajtása a fő adatbázissal, amelyben a geo replikált másodlagos adatbázishoz, amely az előléptetett található Azure SQL-adatbázis logikai kiszolgálón.

Ha elérhetőség az adatbázis visszaállítása kritikus és adatvesztést elfogadható készült vészhelyreállítás ezt a funkciót. Kényszerített feladatátvevő meghívásakor a a megadott másodlagos adatbázis azonnal lesz az elsődleges adatbázist, és megkezdi a elfogadása írási tranzakciók. Amint az eredeti elsődleges még tudni az új elsődleges adatbázist az újracsatlakozáshoz, növekményes biztonsági másolatot az eredeti elsődleges adatbázison származik, és a régi elsődleges adatbázist az új elsődleges adatbázisnak; másodlagos adatbázisba lett Ezt követően érdemes csupán az új elsődleges szinkronizálása replikáját.

Azonban pont az idő visszaállítása nem támogatott a másodlagos adatbázisok, ha a felhasználó kívánja a régi elsődleges adatbázishoz, amely volna nem lett replikált az új elsődleges adatbázis előtt a kényszerített feladatátvételi történt lekötött adatok helyreállítása, mert a felhasználónak kell állíthatja helyre a adatvesztés támogatási vesznek.

Ha az elsődleges adatbázis több másodlagos adatbázisok, a a parancs automatikusan újra a többi formátumú másodlagos zónák az új elsődleges csatlakozhat.

Kövesse az alábbi lépéseket feladatátvételhez elindítására.

1. Management Studio eszközben kapcsolódni a logikai Azure SQL-adatbázis-kiszolgálóhoz, egy másodlagos geo replikált adatbázist helyezkedik el.

2. Nyissa meg az adatbázisok mappát, bontsa ki a **Rendszer adatbázisok** mappát, kattintson a jobb gombbal a **fő**, és kattintson az **Új lekérdezést**.

3. Az alábbi **Adatbázis módosítása** utasítás segítségével a másodlagos adatbázis váltani az elsődleges szerepet.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Kattintson a **végrehajtás** futtassa a lekérdezést.

>[AZURE.NOTE] Ha a parancs ki, ha online elsődleges és másodlagos vannak a régi elsődleges lesz az új másodlagos azonnal adatszinkronizálás nélkül. Ha az elsődleges végrehajtása tranzakciók, amikor a parancs az adatvesztést fordulhat.



## <a name="next-steps"></a>Következő lépések   

- Után feladatátvételt ellenőrizze a hitelesítési követelményeket, a kiszolgáló és az adatbázis meg az új elsődleges. A részletekért olvassa [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md).
- Megtudhatja, hogy aktív Geo replikációs, beleértve a előtti használatával katasztrófa után helyreállítása és helyreállítási lépéseket, és egy katasztrófa helyreállítási részletező elvégzéséhez, lásd: [Vészhelyreállítás](sql-database-disaster-recovery.md)
- Sasha Nosov blogbejegyzésbe kapcsolatos aktív Geo replikációs olvassa el a [fókuszban lévő új Geo replikációs funkciók](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/) című témakört.
- Aktív Geo replikációs használható felhő alkalmazások tervezéséről további tudnivalókért lásd [tervezése felhő alkalmazások használatával Geo replikációs üzemkészségének](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Rugalmas adatbázis készletek aktív Geo replikációs használatáról további tudnivalókért lásd [rugalmas készlet Vészhelyreállítási stratégiák meghatározása](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Üzleti continurity áttekintése a [Üzleti folytonosságot áttekintése](sql-database-business-continuity.md) című témakörben találhat.
