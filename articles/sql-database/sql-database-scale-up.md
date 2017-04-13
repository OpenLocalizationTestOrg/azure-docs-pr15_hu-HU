<properties
    pageTitle="Azure SQL-adatbázishoz szolgáltatás réteg és a teljesítmény szintjének módosítása |} Microsoft Azure"
    description="Módosítsa a szolgáltatási réteg, és Azure SQL-adatbázis teljesítményszint szemlélteti, hogyan lehet az SQL-adatbázis méretezni, felfelé vagy lefelé. A árak réteg Azure SQL-adatbázis módosítása."
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


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Szolgáltatás réteg és a teljesítmény szintjének módosítása (árak réteg) az Azure portálon SQL-adatbázishoz


> [AZURE.SELECTOR]
- [**Azure portál**](sql-database-scale-up.md)
- [A PowerShell](sql-database-scale-up-powershell.md)


Szolgáltatási rétegek teljesítményszint le azokat a funkciókat és erőforrások érhető el az SQL-adatbázis és frissíthető, az alkalmazások módosítása az egyéni igényeknek. A részletekért olvassa [Szolgáltatási rétegek](sql-database-service-tiers.md).

Megjegyzendő, hogy a szolgáltatási réteg módosítása, illetve az adatbázis teljesítményszint hoz létre az eredeti adatbázis új teljesítmény szintre, és ezután átvált kapcsolatok a replika. Adatokat nem elvész folyamat során, de során a rövid pillanatig, amikor azt átkapcsolhatná a replika, az adatbázis-kapcsolatot le van tiltva, így néhány tranzakciók nézetbeli előfordulhat, hogy állítható vissza. Ebben az ablakban változik, de átlagosan 4 másodperc alatt, és a esetek 99 %-nél több nem legfeljebb 30 másodperc. Nagyon ritkán különösen akkor, ha létezik a nagyméretű számok nézetbeli szorzatmomentum kapcsolatok a tranzakciók le vannak tiltva ebben az ablakban lehetséges, hosszabb.  

Az időtartam, a teljes skála telefonos folyamat előtt és után a változtatások mindkét a méret és szolgáltatási réteg az adatbázis függ. Ha például van módosítani szeretné, az vagy egy szabványos szolgáltatási réteg belül 250 GB adatbázis 6 órán belül be kell fejeződnie. Az adatbázis teljesítményszint belül a támogatási szolgáltatás réteg módosítandó azonos méretű akkor be kell fejeződnie 3 órán belül.


Használja az információkat az [Azure SQL-adatbázis szolgáltatási rétegek és teljesítményszint](sql-database-service-tiers.md) alapján határozza meg a megfelelő szolgáltatás réteg és a teljesítmény az Azure SQL-adatbázis.

- Vissza az adatbázis léptetheti, az adatbázis a cél szolgáltatási réteg maximális hossza kisebbnek kell lennie. 
- Adatbázis [Geo replikációs](sql-database-geo-replication-overview.md) engedélyezett frissítéskor először frissítenie kell a másodlagos adatbázisokat a kívánt teljesítmény réteg az elsődleges adatbázis frissítése előtt.
- Visszalépés egy szolgáltatási réteg-ról, amikor először lezárása kell minden Geo replikációs kapcsolat. 
- A visszaállítás szolgáltatásajánlatok a különböző szolgáltatási rétegek eltérőek. Ha meg van Visszalépés-ról, elvesznek a lehetősége, ha vissza szeretne állítani egy időben, vagy egy alsó biztonsági Adatmegőrzés időtartama van. További tudnivalókért olvassa el az [Azure SQL-adatbázis biztonsági mentése és visszaállítása](sql-database-business-continuity.md)című témakört.
- Az adatbázis réteg árak módosítása nem változtatja meg az adatbázis maximális mérete. Az adatbázis módosításához a maximális méret [Transact-SQL nyelvben (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) vagy a [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)használata.
- Az adatbázis új tulajdonságainak nem vonatkoznak a lépést mindaddig, amíg a módosítások befejezése.



**Ez a cikk elvégzéséhez az alábbiakra van szükség:**

- Microsoft Azure SQL-adatbázisban. Ha nem rendelkezik egy SQL-adatbázissal, hozzon létre egy az ebben a cikkben leírt lépéseket követve: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Az adatbázis szolgáltatás réteg és a teljesítmény szintjének módosítása


Nyissa meg az adatbázis kívánja méretezni, felfelé vagy lefelé az SQL-adatbázis lap:

1.  Az [Azure-portálon](https://portal.azure.com)kattintson a **További szolgáltatások** > **SQL-adatbázisait**.
2.  Kattintson a módosítani kívánt adatbázist.
3.  Kattintson az **SQL-adatbázis** lap **árak réteg (méretarány DTUs)**:

    ![réteg árak][1]

1.  Válasszon egy új réteget, majd kattintson a **Jelölje ki**:

    Kattint, **Válassza a** kérést méretarányra módosíthatja a árak réteg. Az adatbázis méretétől függően a méretezés művelet is eltarthat egy kis időt, teljes (lásd az információ, ez a cikk tetejére).

    > [AZURE.NOTE] Az adatbázis réteg árak módosítása nem változtatja meg az adatbázis maximális mérete. Az adatbázis módosításához a maximális méret [Transact-SQL nyelvben (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) vagy a [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)használata.

    ![Jelölje ki a réteg árak][2]

3.  Kattintson a jobb felső sarokban lévő értesítési ikon (harang):

    ![értesítések][3]

    Kattintson az értesítési szövegre a jobb oldali ablaktáblában, ahol megtekintheti a kérelem állapotának megnyitásához.




## <a name="next-steps"></a>Következő lépések

- Módosítsa a [Transact-SQL nyelvben (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) vagy a [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)használata az adatbázis maximális mérete.
- [És méretezése](sql-database-elastic-scale-get-started.md)
- [Csatlakozás és a lekérdezés SSMS SQL-adatbázishoz](sql-database-connect-query-ssms.md)
- [Exportálás az Azure SQL-adatbázishoz](sql-database-export.md)

## <a name="additional-resources"></a>További források

- [Üzleti folytonosságot – áttekintés](sql-database-business-continuity.md)
- [SQL-adatbázis dokumentáció](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
