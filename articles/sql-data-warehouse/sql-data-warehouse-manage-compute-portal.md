<properties
   pageTitle="Az Azure SQL-adatraktár (Azure portal) számítási power kezelése |} Microsoft Azure"
   description="Azure portál tevékenységek kezelése a power számítja ki. Méretezés erőforrások eredményhez tartozó DWUs számítja ki. Vagy mutasson az egérrel, és folytathatja a számítási erőforrások költségeinek mentése."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Az Azure SQL-adatraktár (Azure portal) számítási power kezelése

> [AZURE.SELECTOR]
- [– Áttekintés](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [A PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [TÖBBI](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Méretezés teljesítmény méretezés kifelé az erőforrások és a terhelést változó igényeit memória számítja ki. Mentés költségeket méretezési vissza az erőforrások által nem csúcs időpontok vagy felfüggesztése számítási során teljesen. 

A gyűjtemény a tevékenységek az Azure portal használja:

- Méretezés számítási
- Szünet számítási
- Önéletrajz számítási

További tudnivalókért lásd: [kezelése – áttekintés számítja ki][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Méretezés számítási power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Számítási erőforrások módosítása:

1. Nyissa meg az [Azure portál][], nyissa meg az adatbázist, és kattintson a **Méretezés**.

    ![Kattintson a méretezés][1]

1. Balra vagy jobbra a DWU beállítás módosításához húzza a csúszkát a Méret lap.

    ![Csúszka][2]

1. Kattintson a **Mentés**gombra. A megerősítést kérő üzenet jelenik meg. Kattintson az **Igen** vagy **nem** a megszakításhoz.

    ![Kattintson a Mentés gombra.][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Szünet számítási

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Adatbázis szüneteltetése:

1. Nyissa meg az [Azure-portálra][] , és nyissa meg az adatbázist. Figyelje meg, hogy az Állapot oszlop értéke **Online**. 

    ![Kapcsolati állapot][6]

1. Számítási és a memória-erőforrások felfüggesztheti, kattintson a **Szünet**, és a megerősítést kérő üzenet jelenik meg. Kattintson az **Igen** vagy **nem** a megszakításhoz.

    ![Erősítse meg a szünet][7]

1. Az adatbázis SQL adatraktár elindul, állapota **felfüggesztése**is.
2. **Felfüggesztve**állapot esetén a szünet művelet befejeződött, és már nem készül az előfizetést terhelő DWUs.

    ![Szünet állapota][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Önéletrajz számítási

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Adatbázis folytatása:

1. Nyissa meg az [Azure-portálra][] , és nyissa meg az adatbázist. Figyelje meg, hogy az állapot **Felfüggesztve**. 

    ![Szünet adatbázis][4]

1. Folytatni szeretné az adatbázis kattintson a **Start**gombra, és megjelenik egy tájékoztató üzenet. Kattintson az **Igen** vagy **nem** a megszakításhoz.

    ![Erősítse meg az önéletrajz][5]

1. SQL-adatraktár az adatbázis indítása, az állapot "Resuming" is.
2. **Online**állapot esetén az adatbázis készen áll a.

    ![Kapcsolati állapot][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
További információ a [kezelés áttekintése][]című témakörben találhat.

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Kezelés – áttekintés]: ./sql-data-warehouse-overview-manage.md
[Kezelni a számítási – áttekintés]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portál]: http://portal.azure.com/
