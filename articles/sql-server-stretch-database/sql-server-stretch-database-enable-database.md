<properties
    pageTitle="Kitöltés adatbázis engedélyezése adatbázis |} Microsoft Azure"
    description="Megtudhatja, hogy miként adatbázis konfigurálása Nyújtás adatbázis."
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
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Adatbázis Nyújtás adatbázis engedélyezése

Meglévő adatbázis konfigurálása Nyújtás adatbázis, jelölje ki a **tevékenységeket |} Kitöltés |} Engedélyezése** az SQL Server Management Studio nyissa meg a **Nyújtás engedélyezése adatbázis** varázslót az adatbázis. Is használhatja a Transact\-SQL-adatbázis Nyújtás ahhoz, hogy az adatbázis.

Ha bejelöli a **feladatok |} Kitöltés |} Engedélyezése** az egy adott tábla, és hogy nem még engedélyezett az adatbázis Nyújtás adatbázis, a varázsló konfigurálja az adatbázist Nyújtás adatbázis, és lehetővé teszi a válassza ki azokat a táblákat a folyamat részeként. Kövesse az ebben a témakörben helyett [Nyújtás adatbázis engedélyezése a tábla](sql-server-stretch-database-enable-database.md)című témakör lépéseit.

KCS2 Nyújtás adatbázis engedélyezése adatbázis és tábla igényel\_tulajdonosi engedélyekkel. Kitöltés adatbázis engedélyezése egy adatbázisban is vezérlő adatbázis engedéllyel kell rendelkeznie.

 >   [AZURE.NOTE] Később Ha letiltja Nyújtás adatbázis, ne feledje, hogy a kitöltés adatbázis letiltása, az adatbázis és tábla nem törli a távoli objektum. Ha törölni szeretné a távoli tábla vagy a távoli adatbázis, akkor engedje el az Azure adatkezelési portál használatával. A távoli objektumok továbbra is Azure költségek merülnek fel, amíg nem törli őket kézzel.

## <a name="before-you-get-started"></a>Mielőtt elkezdené

-   Adatbázis konfigurálása Nyújtás, mielőtt azt javasoljuk, hogy a kitöltés adatbázis Advisor adatbázisok és a táblázatokat, amelyek a kitöltés használatára jogosult futtatása. A kitöltés adatbázis Advisor is azonosítja blokkoló problémák. További információt a című témakörben [azonosítása adatbázisok és táblák Nyújtás adatbázis](sql-server-stretch-database-identify-databases.md).

-   Tekintse át a [vonatkozó korlátozások adatbázis egymástól](sql-server-stretch-database-limitations.md).

-   Kitöltés adatbázis adatok Azure áttelepíti. Ezért kell rendelkeznie az Azure-fiók és számlázás előfizetést. URL-Azure fiókkal rendelkezik, [kattintson ide](http://azure.microsoft.com/pricing/free-trial/).

-   Rendelkezik a kapcsolatot, és jelentkezzen be az információkkal hozhat létre egy új Azure-kiszolgálót, vagy jelöljön ki egy meglévő Azure kiszolgálót van szükség.

## <a name="EnableTSQLServer"></a>Előfeltétel: Nyújtás adatbázis engedélyezése a kiszolgálón
Bejelölése előtt engedélyeznie Nyújtás adatbázis adatbázis és tábla, engedélyeznie kell azt a helyi kiszolgálón. Ehhez a művelethez a rendszergazdák vagy serveradmin engedélyeket.

-   Ha rendelkezik a szükséges rendszergazdai engedélyekkel, az **Adatbázis engedélyezése a Nyújtás** varázsló a Nyújtás konfigurálja a kiszolgálót.

-   Ha nem rendelkezik a szükséges engedélyekkel, a rendszergazda ahhoz, hogy a beállítás manuálisan futtatásával **sp\_konfigurálása** előtt a varázslót, vagy a rendszergazda a varázslót.

Kézi engedélyezése Nyújtás adatbázis a kiszolgálón, futtassa a **sp\_konfigurálása** , és kapcsolja be a **távoli data – archív** lehetőséget. A következő példa lehetővé teszi, hogy a **távoli data – archív** beállítást állítsa az érték 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
További című témakörben [konfigurálása a távoli adatok archiválása Server-beállítás](https://msdn.microsoft.com/library/mt143175.aspx) és [sp_configure (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>A varázsló segítségével Nyújtás adatbázis engedélyezése egy adatbázisban
Nyújtás varázsló engedélyezése adatbázis megtudni beleértve az információ, hogy meg kell adnia és a választási lehetőségek, ellenőrizze, akkor olvassa el az [első lépések a engedélyezése adatbázis Nyújtás varázsló futtatásával](sql-server-stretch-database-wizard.md)rendelkező.

## <a name="EnableTSQLDatabase"></a>Használja a Transact\-SQL-adatbázis Nyújtás adatbázis engedélyezése
Bejelölése előtt engedélyeznie Nyújtás adatbázis egyes táblázatokban, engedélyeznie kell azt az adatbázishoz.

KCS2 Nyújtás adatbázis engedélyezése adatbázis és tábla igényel\_tulajdonosi engedélyekkel. Kitöltés adatbázis engedélyezése egy adatbázisban is vezérlő adatbázis engedéllyel kell rendelkeznie.

1.  Mielőtt elkezdené, válassza a Nyújtás adatbázis áttelepítése az adatok egy meglévő Azure kiszolgálót, vagy hozzon létre egy új Azure kiszolgálót.

2.  Az Azure-kiszolgálón tűzfal szabály létrehozása az IP-címtartományokat az SQL Server, amely lehetővé teszi az SQL Server kommunikáció és a távoli kiszolgáló.

    Később könnyen megtalálja az értékeket kell lennie, vagy a tűzfalat szabály létrehozása az Azure kiszolgáló csatlakozás objektum Intézőből az SQL Server Management Studio (SSMS) szerint. SSMS megkönnyíti a szabály létrehozása az alábbi IP-cím szükséges értékeket tartalmazó párbeszédpanelt nyitja meg.

    ![A SSMS tűzfal szabály létrehozása][FirewallRule]

3.  SQL Server-adatbázis konfigurálása Nyújtás adatbázis, az adatbázis egy adatbázis fő kulcsot tartalmaz. Az adatbázis fő billentyűt a Nyújtás adatbázis használ a távoli adatbázishoz való csatlakozáshoz használt hitelesítő adatok titkosítja. Íme egy példa létrehoz egy új adatbázisban fő kulcsot.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    További tudnivalókat az adatbázis fő billentyűt lásd: [DIAMINTA kulcs létrehozása (a Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) és a [fő adatbázist kulcs létrehozása](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Adatbázis Nyújtás adatbázis konfigurálásakor kell adnia a hitelesítő adatok Nyújtás adatbázis használható az alkalmazás helyszíni SQL Server és a távoli Azure kiszolgáló közötti kommunikáció. Két lehetőség közül választhat.

    -   Megadhat egy rendszergazdai hitelesítő adatait.

        -   Ha engedélyezi a Nyújtás adatbázis varázslóval, a hitelesítő adatok adott időpontban is létrehozhat.

        -   Ha Nyújtás adatbázist ahhoz, hogy **Az adatbázis módosítása**futtatásával, akkor a hitelesítő adatok manuális futtatása előtt **Adatbázis módosítása** ahhoz, hogy Nyújtás adatbázis létrehozása.

        Íme egy példa létrehoz egy új hitelesítő adatait.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        További tudnivalók a hitelesítő adatok című témakörben [Létrehozása adatbázis hatóköre hitelesítő adatok (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/mt270260.aspx). ALTER bármely HITELESÍTŐ engedélyeket létrehozása a hitelesítő adatokat igényel.

    -   Az SQL Server szövetséges szolgáltatásfiók használatával kommunikál a távoli Azure kiszolgáló, az alábbi feltételek teljesülése összes is használhatja.

        -   A szolgáltatásfiók, amely alatt az SQL Server-példány fut a tartományi fiók.

        -   A tartományi fiók egy tartományt, amelynek az Active Directory és az Azure Active Directory identitásszolgáltatóval összevont tartozik.

        -   A távoli Azure server Azure Active Directory-hitelesítés támogatására van beállítva.

        -   A szolgáltatásfiók, amely alatt az SQL Server-példány fut a távoli Azure kiszolgálón dbmanager vagy rendszergazdák fiókként kell beállítania.

5.  Adatbázis konfigurálása Nyújtás adatbázis, futtassa az adatbázis módosítása parancsot.

    1.  A kiszolgáló argumentum neveként egy meglévő Azure kiszolgálót, beleértve a `.database.windows.net` a név egy részét \- például `MyStretchDatabaseServer.database.windows.net`.

    2.  Küldje el a hitelesítő adatok argumentum egy meglévő rendszergazdai hitelesítő adatait, vagy adja meg a külső\_szolgáltatás\_fiók = ON. Az alábbi példában egy meglévő hitelesítő biztosít.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Következő lépések
-   [Tábla engedélyezése Nyújtás adatbázis](sql-server-stretch-database-enable-table.md) ahhoz, hogy további táblákat.

-   [Monitor Nyújtás adatbázis](sql-server-stretch-database-monitor.md) adatainak áttelepítési állapotának megtekintéséhez.

-   [Mutasson az egérrel, és folytassa a Nyújtás adatbázis](sql-server-stretch-database-pause.md)

-   [Kezelése és nyújtás adatbázis – problémamegoldás](sql-server-stretch-database-manage.md)

-   [Biztonsági másolat Nyújtás engedélyező adatbázisok](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Lásd még:

[Azonosítsa az adatbázisok és táblák Nyújtás adatbázis](sql-server-stretch-database-identify-databases.md)

[ALTER adatbázis megadása (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
