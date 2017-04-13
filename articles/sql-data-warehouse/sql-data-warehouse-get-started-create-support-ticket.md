<properties
   pageTitle="A támogatási jegyek létrehozása az SQL adatraktár |} Microsoft Azure"
   description="Hogyan lehet a támogatási jegyek létrehozása az Azure SQL-adatraktár."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>A támogatási jegyek SQL adatraktár létrehozása
 
Ha problémák lépnek fel a SQL adatraktár problémákat, hozzon létre egy támogató jegy, hogy mérnöki csoportunk segítséget.

## <a name="create-a-support-ticket"></a>A támogatási jegyek létrehozása

1. Nyissa meg az [Azure-portálon][].

2. A kezdőképernyőn kattintson a **Súgó + támogatási** csempére.

    ![Súgó + támogatás](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Kattintson a súgó + támogatási lap, kattintson a **Létrehozás támogatási kérelem**elemre.

    ![Új támogatási kérelem](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Jelölje ki a **kérelem típusa**.

    ![Kérés típusa](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Alapértelmezés szerint az egyes SQL server (pl. myserver.database.windows.net) 45,000 **DTU kvóta** rendelkezik. Ez a kvóta egyszerűen biztonsági korlátozás. A kvóta növelése támogatási jegy létrehozását, és válassza a *Kvóta* kérelem típust. A DTU kiszámításához van szüksége, a 7.5 szorzása a teljes [DWU][] szükséges. Kattintson egy SQL server két DW6000s tárolni szeretné, majd a 90,000 DTU kvóta kell kérnie.  Az aktuális DTU felhasználási a az SQL server-lap megtekintése a portálon. Felfüggesztett és a nem felfüggesztett adatbázis felé DTU kvóta számolja meg. 

5. Jelölje ki az **előfizetés** , amelyen a problémajelentési az adatbázist.

    ![Előfizetés](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Jelölje be az erőforrás **SQL adatraktár** .

    ![Erőforrás](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Válassza az [Azure terv támogatja][].

    - Minden támogatási szinten **kvóta és előfizetés számlázási** támogatás érhető el.
    - **Töréspont-fix** támogatási [Fejlesztőeszközök][], [Standard][], [Professional közvetlen][] vagy [Premier][] support keresztül kapja. Problémák töréspont-kijavítása Azure használatakor ügyfelek tapasztalt problémák, ha a megfelelő elvárásoknak, hogy a Microsoft a probléma oka.
    - **Fejlesztőeszközök tanácsadás** és **tanácsadó szolgáltatások** [Professional közvetlen][] és [Premier][] support szinten érhetők el. 
    
    Ha támogatja a terv kiemelt, akkor is jelenthet SQL adatraktár kapcsolatos problémák a [Microsoft Premier online portálon][].  [Azure támogatja a tervek]az[Azure támogatja a terv] talál bővebb információt az egyes támogatási, beleértve a hatókör válaszidő, árak, stb.  Azure kapcsolatos gyakori kérdésekre támogatja című témakör [Azure támogatja a gyakori kérdések][].  

    ![Támogatás terv](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. A **probléma típusa** és a **kategória**kiválasztása

    ![A probléma kategóriája](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. A probléma leírása, és válassza a vállalati hatás szintjét.

    ![Probléma leírása](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. A **kapcsolattartási adatait** a támogatási jegy előre kitöltött lesz. Szükség esetén frissítse a.

    ![Kapcsolattartási adatok](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Kattintson a **Create** a támogatási kérelem gombra.


## <a name="monitor-a-support-ticket"></a>A támogatási jegyek figyelése

A támogatási kérelem elküldését követően az Azure támogatási csoport Önnel a kapcsolatot. A kérés állapot és a részletek ellenőrzéséhez kattintson a **kezelés támogatási kérelmek** az irányítópulton.

![Állapot ellenőrzése](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Egyéb források

Emellett a [Túlcsordulás Papírhalom][] , vagy a [Azure SQL-adatok raktári MSDN-fórum][]az SQL adatraktár Közösséggel lehet csatlakozni.

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure portál]: https://portal.azure.com/
[Azure támogatási terv]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Fejlesztőeszközök]: https://azure.microsoft.com/support/plans/developer/  
[Normál]: https://azure.microsoft.com/support/plans/standard/  
[Profi közvetlen]: https://azure.microsoft.com/support/plans/prodirect/  
[Kiemelt]: https://azure.microsoft.com/support/plans/premier/  
[Azure támogatási gyakori kérdések]: https://azure.microsoft.com/support/faq/
[A Microsoft Premier online portálon]: https://premier.microsoft.com/
[Egymást fedő túlcsordulás]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL-adatok raktári MSDN-fórum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

