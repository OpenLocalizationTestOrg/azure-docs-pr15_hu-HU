<properties
    pageTitle="Adatbázisok Nyújtás engedélyező visszaállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként Nyújtás visszaállítása\-engedélyezve van az adatbázisokat."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="douglasl"/>

# <a name="restore-stretch-enabled-databases"></a>Adatbázisok Nyújtás engedélyező visszaállítása

Állítsa vissza a biztonsági másolatot az adatbázisról lévő hibák, a hibák és a katasztrófák számos különböző típusú visszaállításához szükség esetén.

További tudnivalókat a biztonsági másolat [biztonsági Nyújtás engedélyező adatbázisok](sql-server-stretch-database-backup.md)talál.

>   [AZURE.NOTE] Biztonsági másolat része csak egy egy teljes magas elérhetőségéről és az üzleti folytonosságot megoldás. További tudnivalókat a magas elérhetősége megoldásainak [magas elérhetőségét](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="restore-your-sql-server-data"></a>Az SQL Server-adatok visszaállítása
Hardveres hiba vagy sérült helyreállítása, állítsa vissza a Nyújtás\-engedélyezve van az SQL Server-adatbázis biztonsági másolatból. Továbbra is használja az éppen használt SQL Server visszaállítása módszereket. További információt a olvassa el a [Visszaállítás és helyreállítás – áttekintés](https://msdn.microsoft.com/library/ms191253.aspx)című témakört.

Az SQL Server-adatbázis visszaállításához után futtassa újra a csatlakozást a Nyújtás között a tárolt eljárás **sys.sp_rda_reauthorize_db** \-engedélyezve van az SQL Server-adatbázishoz, és a távoli Azure-adatbázis. További információk olvassa el a [között az SQL Server-adatbázishoz, és a távoli Azure-adatbázis kapcsolatának helyreállítása](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database)című témakört.

## <a name="restore-your-remote-azure-data"></a>A távoli Azure adatok visszaállítása

### <a name="recover-a-live-azure-database"></a>Élő Azure-adatbázis helyreállítása
Az SQL Server-Nyújtás adatbázis-szolgáltatás Azure pillanatfelvételek a összes élő adatokat legalább 8 óránként Azure tároló pillanatképek használata. Ezek a pillanatképek 7 napig is megőrződnek. Így ha vissza szeretne állítani az adatok közül legalább 21 pontok az elmúlt 7 nap felfelé az idő, amikor az utolsó pillanatkép felvételének időben.

Visszaállításához élő Azure-adatbázis egy korábbi pontra az idő az Azure portál használatával tegye az alábbiakat.

1. Jelentkezzen be az Azure-portálra.
2. A képernyő bal oldalán válassza a **Tallózás gombra** , és válassza a **SQL-adatbázisait**.
3. Nyissa meg azt az adatbázist, és jelölje ki.
4. Az adatbázis a lap tetején kattintson a **Visszaállítás**gombra.
5. Adjon meg egy új **adatbázis neve**, válassza ki a **Visszaállítási pontra** , és kattintson a **Létrehozás**gombra.
6. Az adatbázis visszaállítása folyamat elindul, és **értesítések**használatával ellenőrizhető.

### <a name="recover-a-deleted-azure-database"></a>A törölt Azure-adatbázis helyreállítása
A SQL Server-adatbázishoz Nyújtás Azure a szolgáltatásnak az adatbázis-pillanatfelvétel adatbázis megszakad, és azt 7 napig megtartja előtt. Miután ez megtörtént, már nem megtartja az élő adatbázisból pillanatképek. Ezzel a radírral törölt adatbázis visszaállítása a pont, amikor törölték.

Törölt Azure adatbázis visszaállítása a pont, ha törölte az Azure portál használatával, hajtsa végre a következő műveleteket.

1. Jelentkezzen be az Azure-portálra.
2. A képernyő bal oldalán válassza a **Tallózás gombra** , és válassza a **SQL-kiszolgálók**.
3. Keresse meg a kiszolgáló, és jelölje ki.
4. Görgessen le a kiszolgáló lap Műveletek, és kattintson a **Törölt adatbázisok** csempére.
5. Jelölje ki a visszaállítani kívánt törölt adatbázist.
5. Adjon meg egy új **adatbázis nevét** , és kattintson a **Létrehozás**gombra.
6. Az adatbázis visszaállítása folyamat elindul, és **értesítések**használatával ellenőrizhető.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Az SQL Server-adatbázishoz, és a távoli Azure-adatbázis közötti kapcsolat visszaállítása

1.  Ha a visszaállított Azure-adatbázis más névvel vagy egy másik régióbeli csatlakozni, futtassa a tárolt eljárás [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) az előző Azure-adatbázis leválasztása.  

2.  Futtassa újra a helyi Nyújtás a tárolt eljárás [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) \-engedélyezve van az Azure-adatbázis adatbázist.  

    -   Adja meg a meglévő adatbázis hatóköre hitelesítő adatok egy sysname vagy egy varchar\(128\) értéket. \(Nem használja az varchar\(max\).\) A hitelesítő adatok nevét a nézetben kikeresheti **sys.database\_hatóköre\_hitelesítő adatok**.  

    -   Adja meg, hogy készítsen másolatot a távoli adatforrásból elemet, és a Másolás (ajánlott) csatlakozzon.  

    ```tsql  
    USE <Stretch-enabled database name>;
    GO
    EXEC sp_rda_reauthorize_db
        @credential = N'<existing_database_scoped_credential_name>',
        @with_copy = 1 ;  
    GO
    ```  

## <a name="see-also"></a>Lásd még:

[Kezelése és nyújtás adatbázis – problémamegoldás](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Biztonsági mentése és visszaállítása az SQL Server-adatbázis](https://msdn.microsoft.com/library/ms187048.aspx)
