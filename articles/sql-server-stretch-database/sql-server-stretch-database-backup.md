<properties
    pageTitle="Biztonsági másolat Nyújtás engedélyező adatbázisok |} Microsoft Azure"
    description="Megtudhatja, hogy miként készítsen biztonsági másolatot Nyújtás\-engedélyezve van az adatbázisokat."
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
    ms.date="10/14/2016"
    ms.author="douglasl"/>

# <a name="backup-stretch-enabled-databases"></a>Biztonsági másolat Nyújtás engedélyező adatbázisok

Adatbázis biztonsági mentése a hibák, a hibák és a katasztrófák számos típusú helyreállítása nyújt segítséget.  

-   Ki kell biztonsági másolatot készíteni a Nyújtás\-engedélyezve van az SQL Server-adatbázisok.  

-   Microsoft Azure automatikusan biztonsági másolatot készít a távoli Nyújtás adatbázis van kerül át az SQL Server Azure az adatokat.  

>    [AZURE.NOTE] Biztonsági másolat része csak egy egy teljes magas elérhetőségéről és az üzleti folytonosságot megoldás. További tudnivalókat a magas elérhetősége megoldásainak [magas elérhetőségét](https://msdn.microsoft.com/library/ms190202.aspx).

## <a name="back-up-your-sql-server-data"></a>Készítsen biztonsági másolatot az SQL Server-adatok  

Biztonsági másolat készítése a Nyújtás\-engedélyezve van az SQL Server-adatbázisok továbbra is használja az éppen használt SQL Server biztonsági módszereket. Ha további információra lásd: a [biztonsági mentése és visszaállítása az SQL Server-adatbázisait](https://msdn.microsoft.com/library/ms187048.aspx).

Kitöltés engedélyező SQL Server-adatbázis biztonsági mentése csak a helyi adatok és az adatok jogosult áttelepítési azon a ponton, amikor a biztonsági mentés futtatásakor a tartalmazzák. \(Arra alkalmas adat, amely még nem lett áttelepíteni, de az áttelepítési beállításokat, a táblázatok alapján Azure áttelepített adatok.\) Ez az úgynevezett **Sekély** biztonsági, és nem tartalmaz adatokat már átkerül az Azure.  

## <a name="back-up-your-remote-azure-data"></a>A távoli Azure adatok biztonsági mentéséhez   

Microsoft Azure automatikusan biztonsági másolatot készít a távoli Nyújtás adatbázis van kerül át az SQL Server Azure az adatokat.  

### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure csökkenthető a kockázat adatvesztés az automatikus biztonsági mentés  
Az SQL Server-adatbázishoz Nyújtás Azure szolgáltatás védi az automatikus tároló pillanatfelvételek a távoli adatbázis legalább 8 óránként. Megtartja a 7 napig, hogy tartománya lehetséges visszaállítása biztosítson minden pillanatkép.  

### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure csökkenthető a kockázat geo az adatvesztés\-redundancia  
Azure-adatbázis biztonsági másolatok mappájában vannak tárolva geo\-felesleges Azure tároló (TS\-GRS), ezért geo\-alapértelmezés szerint felesleges. GEO\-felesleges tároló replikálja az adatok, amely az elsődleges régió távolabb mérföld több száz másodlagos területére. Az elsődleges és másodlagos régiók az adatok van replikált minden háromszor külön hibafa tartományok és a frissítési tartományok keresztül. Ez biztosítja, hogy az adatok tartós az még esetében a teljes területi üzemszünetek vagy katasztrófa jeleníti meg az egyik az Azure régiók nem érhető el.

### <a name="stretchRPO"></a>Kitöltés adatbázis csökkenti a az adatok elvesztését az Azure adatainak ideiglenes megőrzése áttelepített sorok szerint
Miután Nyújtás adatbázis áttelepítése jogosult sorok az SQL Server Azure, megtartja a e táblázatsorok átmeneti tárolásra szolgáló legalább 8 órát. Ha az Azure-adatbázis biztonsági másolatának visszaállítani, nyújtás adatbázis az SQL Server és a Azure-adatbázisok egyeztetése a sorok az átmeneti tárolásra szolgáló táblázat mentett használja.

Az Azure adatok biztonsági másolatának visszaállításához után futtassa újra a Nyújtás a tárolt eljárás [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) \-engedélyezése a távoli Azure-adatbázis SQL Server-adatbázishoz. **Sys.sp_rda_reauthorize_db**futtatásakor Nyújtás adatbázis automatikusan összehangolása az SQL Server és a Azure-adatbázisok.

Ha növelni szeretné a Nyújtás adatbázis ideiglenes megtartja az átmeneti tárolásra szolgáló táblázat áttelepített adatokat órák számát, futtassa a tárolt eljárás [sys.sp_rda_set_rpo_duration](https://msdn.microsoft.com/library/mt707766.aspx) , és adja meg a a 8-nál nagyobb órák számát. Döntse el, hogy mennyi adatot, amely megőrzi, vegye figyelembe az alábbi tényezőket:
-   Az automatikus Azure biztonsági mentést (legalább 8 óránként) gyakorisága.
-   Ismerik fel a problémát, és úgy dönt, hogy visszaállítása biztonsági másolatból a probléma után szükséges időt.
-   Az Azure visszaállítási művelet időtartamát.

> [AZURE.NOTE] Az SQL Server szükséges térközt Nyújtás adatbázis ideiglenes megtartja az átmeneti tárolásra szolgáló táblázat adatokat növelésével nő.

Jelölje be az adatokat tartalmazó Nyújtás adatbázis jelenleg megőrzi ideiglenes az átmeneti tárolásra szolgáló táblázat órák számát, futtassa a tárolt eljárás [sys.sp_rda_get_rpo_duration](https://msdn.microsoft.com/library/mt707767.aspx).

## <a name="see-also"></a>Lásd még:

[Kezelése és nyújtás adatbázis – problémamegoldás](sql-server-stretch-database-manage.md)

[sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Biztonsági mentése és visszaállítása az SQL Server-adatbázis](https://msdn.microsoft.com/library/ms187048.aspx)
