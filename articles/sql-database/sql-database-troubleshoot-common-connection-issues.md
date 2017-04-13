<properties
    pageTitle="Azure SQL-adatbázishoz leggyakoribb csatlakozási problémák elhárítása"
    description="Azonosítása és megoldása a kapcsolat kapcsolatos gyakori hibákra Azure SQL-adatbázis lépéseket."
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

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Azure SQL-adatbázishoz kapcsolódási problémák elhárítása

Ha nem sikerül a kapcsolat Azure SQL-adatbázishoz, [hibaüzeneteket](sql-database-develop-error-messages.md)kap. Ez a témakör az egy központi témakört, amely segítséget nyújt az Azure SQL-adatbázis kapcsolódási problémák elhárítása. Csatlakozási problémákat [okoz a közös](#cause) található, a [hibaelhárításhoz](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) , amely segít a probléma identitás és megoldásuk [tranziens hibák](#troubleshoot-transient-errors) és [állandó vagy nem tranziens hibák](#troubleshoot-the-persistent-errors)megoldására javasolja. Végül [a vonatkozó cikkek Azure SQL-adatbázis csatlakozási problémákat tapasztal a](#all-topics-for-azure-sql-database-connection-problems)jeleníti meg.

Ha a csatlakozási problémákat tapasztal, próbálkozzon a hibaelhárítás lépéseket a jelen cikkben ismertetett.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>OK

Kapcsolódási problémák is okozhatja az alábbiak egyikét:

- Ajánlott eljárások a és a tervezési útmutató alkalmazása az alkalmazás tervezési folyamat során nem sikerült.  [SQL-adatbázis fejlesztési áttekintése](sql-database-develop-overview.md) , az első lépések című témakörben találhat.
- Azure SQL-adatbázis konfigurálás
- Tűzfal beállításai
- A kapcsolat időtúllépési
- Nem a megfelelő bejelentkezési információ
- Források hasznos Azure SQL-adatbázishoz jutni számára vonatkozó maximális érték

Általánosságban elmondható kapcsolódási problémák Azure SQL-adatbázishoz csoportosíthatók az alábbi képlettel történik:

- [Ideiglenes (tranziens) hibák (rövid életű vagy szaggatott)](#troubleshoot-transient-errors)
- [Állandó vagy nem tranziens hibák (hibák rendszeresen ismétlődő)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Próbálja ki az Azure SQL-adatbázis csatlakozási problémákat tapasztal a hibaelhárító

Ha egy adott kapcsolati hiba, próbálja meg [Ez az eszköz](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), amely segít gyorsan identitás és a probléma megoldásához.

## <a name="troubleshoot-transient-errors"></a>Ideiglenes (tranziens) hibák elhárítása
Ha az alkalmazás az ideiglenes (tranziens) hibák történnek, tekintse át a kapcsolatos hibák elhárítása és a hibák gyakoriságának csökkentése kapcsolatban a tippek a következő témaköröket:

- [Adatbázis hibaelhárítási &lt;x&gt; kiszolgálón &lt;y&gt; nem érhető el (hiba: 40613)](sql-database-troubleshoot-connection.md)
- [Hibaelhárítás diagnosztizálása és megakadályozhatja, hogy az SQL-kapcsolat hibák és tranziens hibák SQL-adatbázis](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>(Nem tranziens hibák) állandó hibák elhárítása

Ha az alkalmazás nem folyamatosan Azure SQL-adatbázis csatlakoztatása, általában jelzi problémát az alábbiak egyikét:

- A tűzfal beállításainak. Az Azure SQL-adatbázisa vagy ügyféloldali tűzfal blokkolja a Azure SQL-adatbázis-kapcsolatot.
- Ügyféloldali konfigurálás hálózati: például egy új IP-cím vagy a proxykiszolgáló.
- Felhasználói hiba: például rosszul csatlakozási paramétereket, például a kapcsolati karakterlánc be a kiszolgáló nevét.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Lépéseket állandó kapcsolódási problémák megoldásához

1.  Állítsa be [tűzfalszabályokat](sql-database-configure-firewall-settings.md) lehetővé teszi az ügyfél IP-címet.
2.  Összes tűzfalat az ügyfél és az Internet között ellenőrizze, hogy port 1433 meg nyitva a kimenő kapcsolatok. Olvassa el [az SQL Server hozzáférés engedélyezése a Windows tűzfal konfigurálása](https://msdn.microsoft.com/library/cc646023.aspx) További olvasnivaló.
3.  Ellenőrizze a kapcsolati karakterláncot, és más kapcsolatbeállításokat. A kapcsolati karakterláncot az című [kapcsolódási problémák témakört](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Jelölje be a szolgáltatás állapota az irányítópult. Ha úgy gondolja, hogy van egy területi üzemszünetek, olvassa el a [egy üzemszünetek helyreállítása](sql-database-disaster-recovery.md) a lépések új területére helyreállítása című témakört.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Az összes témakörök Azure SQL-adatbázis kapcsolódási problémák

Az alábbi táblázat a minden kapcsolat probléma témakör, amely közvetlenül az Azure SQL-adatbázis szolgáltatás vonatkozik.


| &nbsp; | Cím | Leírás |
| --: | :-- | :-- |
| 1 | [Azure SQL-adatbázishoz kapcsolódási problémák elhárítása](sql-database-troubleshoot-common-connection-issues.md) | Ez a csatlakozási problémákat tapasztal az Azure SQL-adatbázis hibaelhárítási céloldal. Ismerteti, hogyan azonosítása és megoldása tranziens hibák és állandó vagy nem tranziens hibák Azure SQL-adatbázisban. |
| 2 | [Hibaelhárítás diagnosztizálása és megakadályozhatja, hogy az SQL-kapcsolat hibák és tranziens hibák SQL-adatbázis](sql-database-connectivity-issues.md) | Megtudhatja, hogy miként kapcsolatos hibák elhárítása diagnosztizálása és megakadályozhatja, hogy egy SQL-kapcsolati hiba vagy a ideiglenes (tranziens) hiba Azure SQL-adatbázisban. |
| 3 | [Ideiglenes (tranziens) hibafa-kezelési szóló általános útmutatás](best-practices-retry-general.md) | Azure SQL-adatbázishoz való csatlakozáskor tranziens hiba kezelésének szóló általános útmutatás ismertetése |
| 4 | [A Microsoft Azure SQL-adatbázis kapcsolódási problémák elhárítása](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Az eszköz identitás segítséget nyújt a probléma megoldásához internetkapcsolat hibáinak. |
| 5 | [Hibaelhárítás "adatbázis &lt;x&gt; kiszolgálón &lt;y&gt; jelenleg nem áll rendelkezésre. Próbálja meg ismét a kapcsolat később"hibaüzenet](sql-database-troubleshoot-connection.md) | Megtudhatja, hogy miként azonosítása és megoldása a 40613 hiba: "adatbázis &lt;x&gt; kiszolgálón &lt;y&gt; jelenleg nem áll rendelkezésre. Próbálja meg újra a kapcsolat később." |
| 6 | [SQL-hibakódok az SQL-adatbázis-ügyfélalkalmazásokban: adatbázis-kapcsolati hiba és egyéb problémák](sql-database-develop-error-messages.md) | SQL-hibakódok információt nyújt az SQL-adatbázis ügyfélalkalmazásokban, például adatbázis kapcsolat kapcsolatos gyakori hibákra, az adatbázis-példány problémák és a általános hibák. |
| 7 | [Egyetlen adatbázisok Azure SQL-adatbázis teljesítmény útmutató](sql-database-performance-guidance.md) | Ismerteti, hogy melyik szolgáltatási réteg mindig az alkalmazás. Az alkalmazás az Azure SQL-adatbázis hatékony kihasználása finombeállítása javaslatok is tartalmaz. |
| 8 | [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md) | Mintakódok mutató hivatkozásokat tartalmaz a különböző technológiák való kapcsolódáshoz és Azure SQL-adatbázis használata használható. |
| 9 | Frissítsen az Azure SQL-adatbázis v12 lap ([Azure portál](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Itt-re történő frissítésének meglévő Azure SQL-adatbázis V11 kiszolgálók és adatbázisok Azure SQL-adatbázis V12 Azure portal segítségével vagy Powershellhez útmutatást. |


## <a name="next-steps"></a>Következő lépések

- [Azure SQL-adatbázis teljesítménnyel kapcsolatos problémák elhárítása](sql-database-troubleshoot-performance.md)
- [Azure SQL-adatbázis engedélyekkel kapcsolatos problémák megoldása](sql-database-troubleshoot-permissions.md)
- [Az összes témakörökben találhat az Azure SQL-adatbázis-szolgáltatás](sql-database-index-all-articles.md)
- [A Microsoft Azure dokumentációja keresése](http://azure.microsoft.com/search/documentation/)
- [Az SQL Azure-adatbázis szolgáltatás a legújabb frissítések megtekintése](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>További források

- [SQL-adatbázis fejlesztése – áttekintés](sql-database-develop-overview.md)
- [Ideiglenes (tranziens) hibafa-kezelési szóló általános útmutatás](../best-practices-retry-general.md)
- [A kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md)
- [A tanulási javaslat az Azure SQL-adatbázis használata](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [A tanulási javaslat a rugalmas adatbázis-szolgáltatások és eszközök használata](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 
