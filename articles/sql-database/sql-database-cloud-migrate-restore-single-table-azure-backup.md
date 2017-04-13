<properties
    pageTitle="Egyetlen Azure SQL-adatbázis biztonsági másolatának visszaállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure SQL-adatbázis biztonsági mentése egyetlen visszaállítani."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Hogyan lehet egyetlen visszaállítani egy SQL Azure-adatbázis biztonsági mentése

Előfordulhatnak olyan helyzet, amelyben véletlenül módosított SQL-adatbázisban adatokat, és most szeretné helyreállítani az egyetlen szóban forgó tábla. Ez a cikk ismerteti az SQL-adatbázis [automatikus biztonsági mentést](sql-database-automated-backups.md)visszaállítása az adatbázis egy táblát.

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Előkészületek: nevezze át a táblát, és az adatbázis biztonsági másolatának visszaállítása
1. Azonosítsa az Azure SQL-adatbázishoz, amelyet a visszaállított másolatra lecserélése a táblázatot. A táblázat átnevezése a Microsoft SQL Management Studio segítségével. Nevezze át a táblázatot, mint például &lt;táblanév&gt;_old.

    **Megjegyzés:** Elkerülése végett, győződjön meg arról, hogy nem fut a táblázatot, amelyet a rendszer átnevezése a tevékenység. Ha problémákat tapasztal, győződjön meg arról, hogy ehhez a művelethez a karbantartási időszakokban.

2. Az adatbázis biztonsági másolatának visszaállítása pontra a helyreállítani kívánt [Pont-In_Time visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore) lépésekkel kívánt időpontot.

    **Megjegyzések**:
    - A visszaállított adatbázist lesz a DBName + időbélyeg formátumban Ha például **Adventureworks2012_2016-01-01T22-12Z**. Ez a lépés nem írja felül a meglévő adatbázis neve a kiszolgálón. Ez egy biztonsági mértéket, és rendelkezik szándékában lehetővé teszi, hogy ellenőrizze a visszaállított adatbázist, mielőtt az aktuális adatbázis, és nevezze át a visszaállított adatbázist használható.
    - Az egyszerű az összes teljesítmény-meghatározási prémium automatikusan biztonsági másolat készül a szolgáltatás a biztonsági másolat adatmegőrzési mértékek, attól függően, hogy a réteg változó:

| Adatbázis visszaállítása | Egyszerű szint | Szabványos rétegek | Prémium rétegek |
| :-- | :-- | :-- | :-- |
|  Az idő pont visszaállítása |  Bármelyik visszaállítási pont belül napján|Bármelyik visszaállítási pont 35 napon belül| Bármelyik visszaállítási pont 35 napon belül|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Másolja a táblázat a visszaállított adatbázist az SQL-adatbázis áttelepítési eszközzel
1. Töltse le és telepítse az [SQL-adatbázis áttelepítése varázsló](https://sqlazuremw.codeplex.com).

2. Nyissa meg az SQL adatbázis áttelepítése varázsló lapon **Válassza a folyamat** , jelölje be az **elemzés/áttelepítése az adatbázis**, és kattintson a **Tovább gombra**.
![SQL-adatbázis áttelepítése varázsló - folyamat kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. **Csatlakozás a kiszolgálóhoz** párbeszédpanelen az alábbi beállítások érvénybe:
 - **Kiszolgáló neve**: az SQL Azure-példány
 - **Hitelesítés**: **SQL Server-hitelesítés**. Írja be a bejelentkezési hitelesítő adatait.
 - **Adatbázis**: **fő DB (a lista minden adatbázisban)**.
 - **Megjegyzés:** Alapértelmezés szerint a varázsló menti a bejelentkezési adatokat. Ha azt szeretné, jelölje be a **Bejelentkezési adatok elfelejti**.
![SQL-adatbázis áttelepítése varázsló - válassza a forrás - lépés: 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Az **Adatforrás kiválasztása** párbeszédpanelen jelölje be a visszaállított adatbázist nevét a forrásaként **Előkészületek** szakaszból, és kattintson a **Tovább gombra**.

    ![SQL-adatbázis áttelepítése varázsló - válassza a forrás - lépés: 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. **Objektumok kiválasztása** párbeszédpanelen jelölje ki a **Válassza ki az adatbázis-objektumok** lehetőséget, és válassza a cél kiszolgálóhoz áttelepíteni kívánt a table(or tables).
![SQL-adatbázis áttelepítése varázsló - objektumok kiválasztása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. **Parancsfájl varázsló Összegzés** lapon kattintson az **Igen gombra** rákérdez, hogy készen áll a készítése egy SQL-parancsfájl kapcsolatban. Akkor is a vezérlőt, amellyel a TSQL parancsfájl későbbi felhasználás céljából menteni.
![SQL-adatbázis áttelepítése varázsló - parancsprogram varázsló összegzése](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Az **Eredmények összegzés** lapon kattintson a **Tovább**gombra.
![SQL-adatbázis áttelepítése varázsló - eredmények összefoglalása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. A **Target kiszolgálói kapcsolat beállítása** lapon kattintson a **Csatlakozás a kiszolgálóhoz**, és írja be a részleteket az alábbi képlettel történik:
    - **Kiszolgáló neve**: cél kiszolgálói példány
    - **Hitelesítés**: **SQL Server-hitelesítés**. Írja be a bejelentkezési hitelesítő adatait.
    - **Adatbázis**: **fő DB (a lista minden adatbázisban)**. Ez a beállítás a cél kiszolgálón az adatbázisok sorolja fel.

    ![SQL-adatbázis áttelepítése varázsló - tároló kiszolgáló kapcsolat beállítása](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Kattintson a **Csatlakozás**gombra, jelölje be a céladatbázisban, amely szeretné helyezni a táblázatot, és kattintson a **Tovább gombra**. Ez célszerű fejezze be a korábban létrehozott parancsprogram futtatása, és meg kell jelennie a céladatbázisban másolja az újonnan áthelyezett táblázat.

## <a name="verification-step"></a>Ellenőrző lépésben
1. A lekérdezés, és győződjön meg arról, hogy az adatok megmaradnak az újonnan másolt táblázat tesztelje. Amint megerősíti húzhatja az átnevezett táblázatos formában **Előkészületek** szakaszban. (például &lt;táblanév&gt;_old).

## <a name="next-steps"></a>Következő lépések

[SQL-adatbázis automatikus biztonsági mentést](sql-database-automated-backups.md)
