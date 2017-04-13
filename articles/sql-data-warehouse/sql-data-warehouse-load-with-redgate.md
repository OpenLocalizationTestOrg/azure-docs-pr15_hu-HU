<properties
   pageTitle="Adatok betöltése SQL adatraktár Redgate tartozó adatok Platform Studio segítségével |} Microsoft Azure"
   description="Megtudhatja, hogy miként használja a cég Redgate adatok Platform Studio adatok raktári felhasználási területei."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Adatok betöltése Redgate adatok Platform Studio

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Adatok gyári](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Ebben az oktatóanyagban megtudhatja, hogyan helyezze át az adatokat az egy helyszíni SQL Server Azure SQL-adatraktár [Redgate tartozó adatok Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) segítségével. Adatok Platform Studio vonatkozik, a legmegfelelőbb kompatibilitási javításokat és optimalizálásokat, így a leggyorsabban SQL adatraktár – első lépések.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) a partnere partnereikről Microsoft, amely biztosítja a különböző SQL Server-eszközöket. Ez a funkció az adatok Platform Studio kereskedelmi és kereskedelmi célú tevékenységre nem használható szabadon elérhetővé tett.

## <a name="before-you-begin"></a>Első lépések
### <a name="create-or-identify-resources"></a>Hozzon létre vagy az erőforrások azonosítása

Ebben az oktatóanyagban indításához van szükség:

- **Helyszíni SQL Server-adatbázishoz**: SQL adatraktár szeretné importálni kívánt adatokat kell származnia egy helyszíni SQL Server (verzió 2008R2 vagy újabb). Adatok Platform Studio nem adatok importálása, közvetlenül az Azure SQL-adatbázis vagy szövegfájlokból.
- **Azure tároló fiók**: adatok Platform Studio szakaszában az Azure Blob-tárolóban lévő adatok betöltése azt az SQL adatraktár előtt. A tárterület-fiókot kell használnia a "erőforrás-kezelő" telepítési modell (alapértelmezett) a "Klasszikus" telepítési modell helyett. Ha nem rendelkezik a tárterület-fiókkal, megtudhatja, hogy miként tárterület-fiók létrehozása. 
- **SQL-adatraktár**: Ebben az oktatóanyagban lép az adatok helyszíni SQL Server SQL adatraktár, így egy adatraktár online van szükség. Ha még nem rendelkezik egy adatraktár megtudhatja, hogy miként hozhat létre az Azure SQL-adatraktár.

> [AZURE.NOTE] Ha a tárterület-fiók és a adatraktár az ugyanabban a régióban hoz létre a teljesítmény növelése.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Lépés: 1: Jelentkezzen be az adatok Platform Studio-az Azure-fiók
Nyissa meg a böngészőt, és keresse meg az [Adatok Platform Studio](https://www.dataplatformstudio.com/) webhelyet. Jelentkezzen be az azonos Azure-fiók a tárterület-fiók és az adatok raktári létrehozásához használt. Ha az e-mail címét a munkahelyi vagy a iskolai fiók és egy Microsoft-fiókkal, ügyeljen arra, válassza ki azt a fiókot, amely az erőforrások hozzáféréssel rendelkezik.

> [AZURE.NOTE] Ha első alkalommal adatok Platform Studio segítségével, rendszer kéri, hogy az alkalmazás engedélyt az Azure erőforrások kezelése.

## <a name="step-2-start-the-import-wizard"></a>Lépés: 2: A varázsló elindításához.
Az DPS fő képernyőn válassza az importálás Azure SQL-adatraktár hivatkozásra az importálás varázsló elindításához.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>3 lépés: Az adatok Platform Studio átjáró telepítése
A helyszíni SQL Server-adatbázishoz való csatlakozáshoz kell az DPS átjáró telepítése. Az átjáró, egy ügyfél ügynököt, hogy a helyszíni környezet hozzáférést biztosít az adatok kibontása és feltölti a tárterület-fiókjába. Az adatok soha nem áthaladt Redgate's kiszolgálókhoz. Az átjáró telepítése:

1.  Kattintson az **Átjáró létrehozása** hivatkozásra.
2. Töltse le és telepítse az átjáró a megadott installer használatával

![][2]

> [AZURE.NOTE] Az átjáró is telepítve van a forrásadatbázis SQL Server hálózati hozzáféréssel rendelkező bármely a gépen. Éri el a Windows-hitelesítés használatával az aktuális felhasználói hitelesítő SQL Server-adatbázishoz.

A telepítés után az átjáró állapota Connected változik, és meg, kattintson a Tovább gombra.

## <a name="step-4-identify-the-source-database"></a>Lépés: 4: Azonosítsa a forrás-adatbázis
Az *Adja meg a kiszolgáló neve* mezőbe írja be a a kiszolgáló neve, amely az adatbázist tároló, és válassza a **Tovább gombra**. A legördülő menüből válassza a használni kívánt adatokat szeretne importálni adatbázist.

![][3]

DPS megvizsgálja az importálandó táblákat a kijelölt adatbázis. Alapértelmezés szerint DPS importálja az adatbázis összes táblát. Jelölje be, vagy jelölésének törlésével táblázatok kibontása az összes táblázat hivatkozásra. Jelölje ki a előrelépés a következő gombra.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Lépés 5: Válassza a szakasz az adatokat egy tárterület-fiókkal
DPS kéri, a hely előkészítése az adatokat. Válasszon ki egy meglévő tárterület-fiókot az előfizetésből, és válassza a **Tovább gombra**.

> [AZURE.NOTE] DPS hozzon létre egy új blob-tárolóhoz kiválasztott tároló fiók, és a különböző mappa használata az egyes importáláshoz.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Lépés a 6: Jelölje be a adatraktár
Ezután jelölje be az adatimportálás a egy online [Azure SQL-adatraktár](http://aka.ms/sqldw) adatbázisban. Miután megadta az adatbázist, meg kell adni a hitelesítő adatait, és csatlakozzon a adatbázishoz, és kattintson a **Tovább gombra**.

![][5]

> [AZURE.NOTE] DPS a forrás adattáblák egyesíti a adatraktár. DPS figyelmeztet, ha a táblázat neve igényel, hogy felülírja az adatraktár létező táblák. Tetszés szerint törölheti a adatraktár bármelyik meglévő objektumok törlése önkéntes által importálás előtt összes meglévő objektumot.

## <a name="step-7-import-the-data"></a>7 lépés: Az adatok importálása
DPS megerősíti, hogy szeretné, hogy importálja az adatokat. Egyszerűen kattintson a Start importálása gombra az adatok importálása a kezdéshez.

![][6]

DPS adatábrázolás kibontása, majd feltölti az adatokat a helyszíni SQL Server-és az SQL adatraktár importálása végrehajtásának végrehajtásának megjelenítő jeleníti meg.

![][7]

Miután az importálás befejeződése után DPS az adatok importálása összegzését és végrehajtott kompatibilitási javítását módosítása jelentés jeleníti meg.

![][8]

## <a name="next-steps"></a>Következő lépések
SQL-adatraktár belül az adatok feltárása, először megtekintése:

- [Lekérdezés Azure SQL-adatraktár (Visual Studio)][]
- [A Power BI adatok ábrázolása][]

Ha többet szeretne megtudni a Redgate tartozó adatok Platform Studio:

- [Látogasson el a DPS kezdőlapra.](http://www.dataplatformstudio.com/)
- [Bemutatja a Channel9 DPS](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Egyéb módjai áttelepítendő, és töltse be az adatok ábrázolása SQL adatraktár áttekintést talál:

- [A megoldás áttelepítése SQL adatraktár][]
- [Adatok betöltése Azure SQL-adatraktár](./sql-data-warehouse-overview-load.md)

Fejlesztési vonatkozó további tippek a az [SQL adatraktár fejlesztése – áttekintés](./sql-data-warehouse-overview-develop.md)című témakörben találhat.

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Lekérdezés Azure SQL-adatraktár (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[A Power BI adatok ábrázolása]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[A megoldás áttelepítése SQL adatraktár]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md