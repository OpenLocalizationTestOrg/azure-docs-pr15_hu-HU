<properties
    pageTitle="Adatbázis-kiszolgálón jelenleg nem áll rendelkezésre, az SQL-adatbázis csatlakoztatása |} Microsoft Azure"
    description="Az adatbázis elhárítása a kiszolgáló nem áll jelenleg elérhető hibák céljával SQL-adatbázishoz csatlakozik."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="adatbázis-kiszolgálón jelenleg nem áll rendelkezésre, az sql-adatbázis csatlakoztatása"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Hiba "Adatbázis-kiszolgálón jelenleg nem áll rendelkezésre" sql-adatbázishoz való csatlakozáskor
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Amikor az alkalmazás Azure SQL-adatbázishoz csatlakozik, az alábbi hibaüzenet jelenhet meg:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Ez a hibaüzenet akkor általában tranziens (rövid életű).

Ez a hiba történik, amikor az Azure-adatbázis folyamatban van helyezett (vagy újrakonfigurálni) és az alkalmazás megszakad a kapcsolat, az SQL-adatbázishoz. SQL-adatbázis konfigurálás események fordul elő, a tervezett esemény (Ha például egy szoftver frissítése) vagy a váratlan esemény (Ha például egy folyamat összeomlik vagy terheléselosztás). A legtöbb konfigurálás események általában rövid életű, és meg kell adni a kisebb, mint 60 másodperc legfeljebb. Azonban ezek az események alkalmanként több időt vesz igénybe, például amikor egy nagy tranzakció okozza-e egy hosszabb ideig futó helyreállítási Befejezés gombra.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Lépéseket tranziens kapcsolódási problémák megoldásához
1.  Jelölje be a [Microsoft Azure Service irányítópult](https://azure.microsoft.com/status) minden olyan ismert kimaradások, hogy mikor történt, ameddig a hibák alkalmazásával voltak jelentett idő alatt.
2. Csatlakozás egy felhőalapú szolgáltatásba, például kell számíthat, periodikus konfigurálás események és megvalósítása a Azure SQL-adatbázis-alkalmazások ezeket a hibákat helyett útburkoló alkalmazáshibák ezek a felhasználók kezelése logika próbálkozzon újra. Tekintse át a [hibák tranziens](sql-database-connectivity-issues.md) szakaszt, és a gyakorlati tanácsokat és további információt és az általános útmutató az [SQL-adatbázis fejlesztési áttekintése](sql-database-develop-overview.md) újra stratégiák tervezés. Lásd a részletekért a [Kapcsolat SQL-adatbázis és az SQL Server képtárak](sql-database-libraries.md) mintakódok.
3.  Adatbázis megközelíti a erőforrás korlátozások, mint a is egy tranziens csatlakozási probléma tűnnek. Olvassa el a [teljesítménnyel kapcsolatos problémák elhárítása](sql-database-troubleshoot-performance.md)című témakört.
4.  Ha továbbra is kapcsolódási problémákat, vagy ha, amelynek az alkalmazás a hiba lép fel, mint 60 másodperc vagy a hiba több előfordulását megjelenik az adott napi, a fájl **Első támogatja** az [Azure támogatja a](https://azure.microsoft.com/support/options) webhelyen kiválasztásával Azure támogatási kérelmet.

## <a name="next-steps"></a>Következő lépések
- Ha egy másik hibaüzenet jelenik meg, kiértékelésének eredménye a [hibaüzenet jelenik meg](sql-database-develop-error-messages.md) tartalom a probléma okát bemutatásához.
- Ha a probléma állandó, látogasson el az útmutató a [Gyakori csatlakozási](sql-database-troubleshoot-common-connection-issues.md)problémák elhárítása Azure SQL-adatbázishoz.
