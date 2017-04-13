<properties
    pageTitle="Adatbázisok és táblák Nyújtás adatbázis azonosítása Nyújtás adatbázis Advisor futtatásával |} Microsoft Azure"
    description="Megtudhatja, hogy miként adatbázisok és táblák Nyújtás adatbázis jelölt azonosításához."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Adatbázisok és táblák Nyújtás adatbázis azonosítása Nyújtás adatbázis Advisor futtatásával

Adatbázisok és táblák Nyújtás adatbázis jelölt azonosítása, töltse le a SQL Server 2016 észlel, és futtassa a Nyújtás adatbázis Advisor. Kitöltés adatbázis Advisor is azonosítja blokkoló problémák.

## <a name="download-and-install-upgrade-advisor"></a>Töltse le és telepítse a tanácsadó frissítése
Letöltheti és telepítheti észlel az [alábbi](http://go.microsoft.com/fwlink/?LinkID=613421). Ez az eszköz nem szerepel az SQL Server telepítési adathordozóját.

## <a name="run-the-stretch-database-advisor"></a>A kitöltés adatbázis Advisor eszköz futtatása

1.  Frissítési Advisor futtatása.

2.  Jelölje ki az **esetek**, és válassza a **NYÚJTÁS adatbázis ADVISOR futtatása**.

3.  Kattintson a **Futtatás Nyújtás adatbázis Advisor** lap **Kiválasztása ADATBÁZISOK elemzése című témakört**.

4.  **Jelölje ki az adatbázisok** lap adja meg, és válassza ki a kiszolgáló nevét, valamint a hitelesítési adatait. Kattintson a **Csatlakozás**gombra.

5.  Megjelenik a kijelölt kiszolgálón adatbázisok listája. Jelölje ki az elemezni kívánt adatbázisait. Kattintson a **Kijelölés**gombra.

6.  A **Kitöltés adatbázis Advisor futtatása** a lap kattintson a **Futtatás**parancsra.  Az elemzés fut.

## <a name="review-the-results"></a>Tekintse át az eredményeket

1.  Az elemzés a **Analyzed adatbázisok** lap a befejezésekor, jelölje ki azt az adatbázisokra, amelyek, elemezheti, hogy az **elemzés eredménye** lap megjelenítéséhez.

    Az **elemzés eredménye** lap a kijelölt adatbázis ajánlott az alapértelmezett ajánlási feltételeknek eleget tevő táblázatok sorolja fel.

2.  Az **elemzés eredménye** lap táblák listájában jelölje ki azt a javasolt táblákat a **tábla eredmények** lap megjelenítéséhez.

    Problémák amelyek gátolják, ha a **táblázat eredmények** lap a kijelölt tábla a blokkolási problémákat jeleníti meg. Tájékoztatást blokkoló észlelni Nyújtás adatbázis tanácsadó lásd: a [Nyújtás adatbázis korlátozások](sql-server-stretch-database-limitations.md).

3.  A **táblázat eredmények** lap zárolási problémák listájában válassza a problémák további tudnivalókat a a kijelölt probléma megjelenítéséhez, és felajánlja a kezelési lépéseket. A javasolt kezelési lépések végrehajtása, ha be szeretné állítani a kijelölt tábla Nyújtás adatbázis.

## <a name="next-step"></a>Következő lépés
Engedélyezze a Nyújtás adatbázis.

-   Ahhoz, hogy Nyújtás adatbázis egy **adatbázist**, olvassa el a [Nyújtás adatbázis engedélyezi az adatbázis](sql-server-stretch-database-enable-database.md)című témakört.

-   Ahhoz, hogy Nyújtás adatbázis egy másik **tábla**, ha Nyújtás már engedélyezve van az adatbázist, olvassa el a [Nyújtás adatbázis engedélyezése a tábla](sql-server-stretch-database-enable-table.md)című témakört.

## <a name="see-also"></a>Lásd még:

[Kitöltés adatbázis korlátai](sql-server-stretch-database-limitations.md)

[Adatbázis Nyújtás adatbázis engedélyezése](sql-server-stretch-database-enable-database.md)

[Táblázat Nyújtás adatbázis engedélyezése](sql-server-stretch-database-enable-table.md)

[Az összes témakörök Azure SQL Server-adatbázis Nyújtás szolgáltatás](sql-server-stretch-database-index-all-articles.md)
