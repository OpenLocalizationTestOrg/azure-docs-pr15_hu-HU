1. Jelentkezzen be a az [Azure-portálon].

2. Kattintson a **+ Új** > **webes + Mobile** > **Mobile alkalmazást**, majd nevezze el a mobilalkalmazás kódmentes.

3. Az **Erőforráscsoport**jelölje be a meglévő erőforráscsoport, vagy hozzon létre egy újat (ugyanazt a nevet a az alkalmazást használják.) 
 
    Válasszon egy másik alkalmazás szolgáltatás tervet, vagy hozzon létre egy újat. További információt az alkalmazás szolgáltatások tervek és az új csomag létrehozása az, hogy egy másik árak első csoportba tartozó, és a kívánt helyre a [áttekintése](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)című témakörben találhat Azure alkalmazás szolgáltatás csomagok meg szeretné vizsgálni.

4. Az **alkalmazás szolgáltatáscsomagja**az alapértelmezett csomag (a [szabványos réteg](https://azure.microsoft.com/pricing/details/app-service/)) be van jelölve. Emellett bejelölheti a másik csomagra, vagy [Hozzon létre egy újat](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Az alkalmazás szolgáltatáscsomagja beállításai határozzák meg a [helyet, funkcióiról, költség, és erőforrások kiszámítására](https://azure.microsoft.com/pricing/details/app-service/) az alkalmazással társított. 

    Miután úgy dönt, kattintson a csomagot, kattintson a **Létrehozás**gombra. Ezzel létrehoz, hogy a mobilalkalmazás kódmentes. 
    
6. Kattintson az új Mobile alkalmazásban kódmentes- **Beállítások** lap, a **rövid útmutató az első** > illő ügyfél alkalmazás > **Csatlakozás adatbázis**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Kattintson az **adatbázisból származó adatok hozzáadása** lap **SQL-adatbázis** > **létrehozása új adatbázis**, írja be az adatbázis **nevét**, válasszon egy árak réteg, majd kattintson a **kiszolgáló**.  Az új adatbázis újra felhasználhatja. Ha már van egy adatbázis ugyanazon a helyen, akkor ehelyett **használata egy meglévő adatbázishoz**. Az adatbázis más helyen sávszélesség-költségek és a magasabb késés miatt nem javasolt.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. A lap az **új kiszolgálót** írja be a **kiszolgáló neve** mezőben egyedi kiszolgáló nevét, adja meg a bejelentkezési és a jelszót, jelölje be az **Engedélyezés azure szolgáltatások kiszolgáló elérésére**, és kattintson az **OK gombra**. Ez az új adatbázist hoz létre.

9. Vissza az **adatbázisból származó adatok hozzáadása** lap kattintson a **kapcsolati karakterlánc**, írja be az adatbázis jelszóval értékeket, és kattintson az **OK gombra**. Várjon néhány percet, amíg az adatbázist, hogy a folytatás előtt sikeresen rendszerbe.

<!-- URLs. -->
[Azure portálon]: https://portal.azure.com/
