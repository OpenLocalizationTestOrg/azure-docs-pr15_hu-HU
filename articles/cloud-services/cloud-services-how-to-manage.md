<properties 
    pageTitle="Gyakori felhőalapú szolgáltatás teendőkről (klasszikus) |} Microsoft Azure" 
    description="Megtudhatja, hogy miként kezelheti a felhőszolgáltatások az Azure klasszikus portálon." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/10/2016"
    ms.author="adegeo"/>





# <a name="how-to-manage-cloud-services"></a>Cloud Services kezelése

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-manage-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-manage.md)

Az Azure klasszikus portál **Cloud Services** területen lehet szolgáltatás szerepkör vagy a telepítés, egy gyártási szakaszos telepítés előléptetése, erőforrások csatolása a felhőalapú szolgáltatás, így lásd: az erőforrás-függőségek és az erőforrások közös méretezése és egy felhőalapú szolgáltatásba vagy egy telepítési törlése.


## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Útmutató: a felhőalapú szolgáltatások szerepkör vagy a telepítés frissítése

Ha módosítania kell a felhőalapú szolgáltatás alkalmazás kódját, használata **frissítése** az irányítópult, **Cloud Services** lapra vagy **példányok** lap. Egy egy vagy több összes szerepkört frissíthetők. Egy új szolgáltatás csomag és a szolgáltatás konfigurációs fájl feltöltése kell.

1. Az [Azure klasszikus portál](https://manage.windowsazure.com/)az irányítópult, **Cloud Services** lapra vagy **példányok** lapon kattintson a **frissítés**gombra.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. **Telepítési felirat**írja be egy nevet, amely azonosítja a példányban (például mycloudservice4). A telepítési címke a **rövid útmutató** az irányítópulton találhatók.

3. A **csomagot**használja a **Tallózás gombra** (.cspkg) frissítőcsomag-fájl feltöltése.

4. **Konfigurációs**segítségével **Keresse meg** a szolgáltatás konfigurációs fájl (.cscfg) feltöltése.

5. A **szerepkör**válassza az **összes** Ha frissíti a felhőalapú szolgáltatást az összes szerepkörének. Egy egyetlen szerepkör frissítés végrehajtani, jelölje be a frissíteni kívánt szerepkört. Akkor is, ha egy adott szerepkört frissítése lehetőséget választja, a frissítéseket a szolgáltatás konfigurációs fájl összes szerepkörének érvényesek.

6. Ha a frissítés módosítja a szerepkörök számát, vagy bármely szerepkör méretét, jelölje be a a **engedélyezése, ha megváltozik szerepkör méretű vagy szerepkörök száma frissítése** jelölőnégyzetet, ahhoz, hogy a frissítés, a folytatáshoz. 

    Ügyeljen arra, hogy egy szerepkört (Ez azt jelenti, hogy a mérete egy virtuális számítógépre, amelyen egy szerepkör-példány) vagy a szerepkörök számát méretének módosításához, ha minden szerepkör-példány (virtuális számítógép) kell újra leképezett és bármely helyi adatok el fog veszni.

7. Ha bármelyik szolgáltatás szerepkörök csak egy szerepkör-példányt, jelölje ki a **frissíteni, akkor is, ha egy vagy több szerepkör tartalmaz egy példány jelölőnégyzet** ahhoz, hogy a frissítés, a folytatáshoz. 

    Azure is csak garantálja 99.95 készültségi szolgáltatáselérhetőség egy felhőalapú szolgáltatás a frissítés során, ha minden szerepkörhöz legalább két szerepkör-példányok (virtuális gépeken futó). Amely lehetővé teszi, hogy egy virtuális gépen ügyfél összehívások, a másik frissítése közben.

8. Az gombra **az OK gombra** (jelölő) frissítése a szolgáltatást a kezdéshez.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Útmutató: a szakaszos telepítés termelési előléptetése telepítések felcserélése

**Felcserélése** segítségével előléptetése álló átmeneti tárolásra szolgáló telepítés termelési egy felhőalapú szolgáltatásba. Ha úgy dönt, hogy egy felhőalapú szolgáltatásba egy új kiadásának telepítéséhez, szakaszra, és tesztelje az új kiadásának a felhőalapú szolgáltatás fejlesztői környezet közben a felhasználók, akik használják a gyártási a jelenlegi kiadás. Ha készen áll az új verziójának megjelenése termelési előléptetése, használhatja a **felcserélése** válthat az URL-címeit, amely a két telepítések foglalkozik. 

A **Cloud Services** lapra vagy az irányítópulton telepítések válthat.

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)kattintson a **Cloud Services**.

2. Kattintson a felhőbeli szolgáltatások listáját, jelölje ki a felhőalapú szolgáltatást.

3. Kattintson a **felcserélése**.

    Az alábbi megerősítést kérő üzenet nyílik meg.

    ![Cloud Services felcserélése](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Miután megbizonyosodott róla a telepítési információ, kattintson az **Igen gombra** a telepítések megjelenítését.

    A telepítési felcserélése gyorsan történik mivel a beállítást, amely módosítja a virtuális IP-címek (VIP) a telepítési.

    Szeretné menteni a számítási költségek, ha biztos benne, hogy az új éles üzemi végrehajtásához meg a várt módon törölheti a fejlesztői környezet telepítését.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Útmutató: hivatkozás egy erőforrás egy felhőalapú szolgáltatásba

Ha meg szeretné jeleníteni a felhőalapú szolgáltatás függőségek más erőforrások, hozzákapcsolhatja Azure SQL-adatbázis-példány vagy a tárterület-fiókkal a felhőalapú szolgáltatást. Hivatkozás és szétválasztása erőforrások **Csatolt erőforrások** lapon, és majd figyelheti a felhőalapú szolgáltatás irányítópulton a használatát. Ha egy csatolt tárterület-fiókkal rendelkezik, ellenőrzése, hogy be van kapcsolva, kérések teljes figyelheti a felhőalapú szolgáltatás irányítópulton.

**Hivatkozás** segítségével egy új vagy meglévő SQL-adatbázis példány vagy tároló fiók csatolása a felhőalapú szolgáltatás. Ezután méretezheti együtt a felhőalapú szolgáltatás szerepkört, amely a **méret** lapon használja azt az adatbázist. (A tárhely fiók méretezze át automatikusan a használatát növekedése.) További információért megtudhatja, [hogy miként egy felhőalapú szolgáltatásba, és csatolt erőforrások méretezni](cloud-services-how-to-scale.md). 

Akkor is is figyelése, kezelése és az adatbázis méretezni a Azure klasszikus portál az **adatbázisok** csomópont. 

"Összekapcsolás" Ebben az értelemben erőforrás nem csatlakozni az alkalmazás az erőforrás. Ha hoz létre egy új adatbázis **-kapcsolaton**keresztül, szüksége lesz az alkalmazás kódja vehet fel a kapcsolati karakterláncot, és frissítse a felhőalapú szolgáltatást. Is kell hozzáadása a kapcsolati karakterláncot, ha az alkalmazás az erőforrásokat használó csatolt tárterület-fiókjában.

Az alábbi lépésekkel hogyan csatolja az új SQL-adatbázis-példány, új SQL-adatbázis kiszolgálója, ha egy felhőalapú szolgáltatásba rendszerre telepíthető.

### <a name="to-link-a-sql-database-instance-to-a-cloud-service"></a>Egy-egy SQL-adatbázis amelyben hivatkozni szeretne egy felhőalapú szolgáltatásba

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Cloud Services**. Kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatást.

2. Kattintson a **csatolt erőforrásokat**.

    Megnyílik a **Csatolt erőforrások** lap.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Kattintson a **hivatkozás egy erőforráshoz** vagy a **hivatkozás**gombra.

    A **Hivatkozás erőforrás** varázsló elindul.

    ![Hivatkozás 1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Kattintson az **Új erőforrás létrehozása** vagy a **meglévő erőforrás hivatkozás**.

5. Válassza az erőforrás, amelyre a hivatkozás típusát. Az [Azure klasszikus portált](http://manage.windowsazure.com/)kattintson az **SQL-adatbázis**. (Az előnézeti Azure klasszikus portálon nem támogatja a tárterület-fiók csatolásával egy felhőalapú szolgáltatásba.)

6. Az adatbázis-konfigurációt befejezéséhez kövesse a Azure klasszikus portál súgó az **SQL-adatbázisait** szakasz utasításait.

    Kövesse az üzenetmezőbe a csatolási művelet elért haladás.

    ![Hivatkozások folyamata](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkProgress.png)

    Amikor csatolása befejeződött, figyelheti a csatolt erőforrás irányítópulton a felhőalapú szolgáltatás állapotát. Csatolt SQL-adatbázis méretezés tudni megtudhatja, [hogy miként egy felhőalapú szolgáltatásba, és csatolt erőforrások méretezni](cloud-services-how-to-scale.md).

### <a name="to-unlink-a-linked-resource"></a>Csatolt erőforrás el

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Cloud Services**. Kattintson a nevére kattintva nyissa meg az irányítópulton a felhőalapú szolgáltatást.

2. Kattintson a **Csatolt erőforrásokat**, és válassza az erőforrás.

3. Kattintson a **leválasztása**gombra. Ezután kattintson az **Igen** gombra a megerősítést kérő párbeszédpanelen.

    SQL-adatbázis leválasztása nem befolyásolja az adatbázis vagy az alkalmazást az adatbázis-kapcsolatot. Továbbra is kezelheti az Azure klasszikus portál **SQL-adatbázisait** területén az adatbázist.



## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Útmutató: telepítések és egy felhőalapú szolgáltatásba törlése

Törölhet is egy felhőalapú szolgáltatásba, törölnie kell minden meglévő telepítés.

Szeretné menteni a számítási költségek, miután megbizonyosodott róla, hogy a várt módon működnek-e a éles üzemi törölheti a átmeneti tárolásra szolgáló üzembe. Szerepkör-példányok számlázott számítási költségek áll, akkor is, ha egy felhőalapú szolgáltatásba nem fut.

A következő eljárással a telepítő vagy a felhőalapú szolgáltatás törlése. 

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Cloud Services**.

2. Jelölje ki a felhőalapú szolgáltatást, és kattintson a **Törlés**gombra. (Az irányítópulton megnyitása nélkül, jelölje ki egy felhőalapú szolgáltatásba, kattintson bárhova a nevét, az a felhő szolgáltatás bejegyzés kivételével.)

    A fejlesztői vagy termelési telepítés esetén látni fogja az ablak alján az alábbihoz hasonló választási lehetőségeket tartalmazó menüt. Törölheti a felhőbeli szolgáltatástól, törölnie kell bármelyik meglévő telepítések.

    ![Menü törlése](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)


3. A telepítés törléséhez kattintson a **éles üzemi törlése** vagy **átmeneti tárolásra szolgáló telepítési törlése**gombra. Ezután a megerősítést kérő párbeszédpanelen kattintson az **Igen**gombra. 

4. Ha törli a felhőbeli szolgáltatástól, ismételje meg 3, ha szükséges, a többi üzembe törlése.

5. A felhőbeli szolgáltatástól törléséhez kattintson a **Törlés felhőszolgáltatásba**. Ezután a megerősítést kérő párbeszédpanelen kattintson az **Igen**gombra.

> [AZURE.NOTE]
> Ha részletes figyelése a felhőalapú szolgáltatás van beállítva, Azure nem törli a felügyeleti adatokat a tárterület-fiókjából a felhőbeli szolgáltatástól törlésekor. Szüksége lesz az adatok manuális törléséhez. Hol találhatók a mértékek táblák kapcsolatos információ "hogyan: részletes kívül az Azure klasszikus portál-adatok figyelése Access" a [Monitor Cloud Services hogyan](cloud-services-how-to-monitor.md).

## <a name="next-steps"></a>Következő lépések

 * [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure.md).
* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md)beállítása.
