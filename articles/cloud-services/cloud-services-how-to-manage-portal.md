<properties 
    pageTitle="Gyakori felhőalapú szolgáltatás felügyeleti feladatok |} Microsoft Azure" 
    description="Megtudhatja, hogy miként kezelheti a felhőszolgáltatások az Azure-portálon. Az alábbi példákban az Azure-portálra." 
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
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Cloud Services kezelése

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-manage-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-manage.md)

A felhőalapú szolgáltatás kezelik az Azure portál **Cloud Services (klasszikus)** területén. Ez a cikk ismerteti a néhány gyakori műveleteket szeretné a felhőalapú szolgáltatások kezelésekor. Mely tartalmazza a módosítása, törlése, méretezése és termelési szakaszos telepítés támogatása.

További információ arról, hogy miként méretezheti a felhőalapú szolgáltatás érhető el [az alábbi](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Útmutató: a felhőalapú szolgáltatások szerepkör vagy a telepítés frissítése

Ha módosítania kell a felhőalapú szolgáltatás alkalmazás kódját, használja a **frissítése** a felhőalapú szolgáltatás lap. Egy egy vagy több összes szerepkört frissíthetők. Ha frissíteni szeretné, feltöltheti egy új szolgáltatás csomagot vagy a szolgáltatás konfigurációs fájl.

1. Az [Azure portálon][]válassza ki a frissíteni kívánt felhőszolgáltatásba. Ebben a lépésben a felhőalapú szolgáltatás példány lap megnyitása

2. A lap kattintson a **frissítés** gombra.

    ![Frissítés gomb](./media/cloud-services-how-to-manage-portal/update-button.png)

3. A telepítési frissíteni egy új szolgáltatás csomagfájlt (.cspkg) és a szolgáltatás konfigurációs fájl (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Tetszés szerint** frissítse a telepítési címke és a tárterület-fiókot. 

5. Ha bármelyik szerepkörök csak egy szerepkör-példányt, jelölje be az **üzembe, még akkor is, ha egy vagy több szerepköröket tartalmaz egy példányát** ahhoz, hogy a frissítés, a folytatáshoz. 

    Azure is csak garantálja 99.95 készültségi szolgáltatáselérhetőség egy felhőalapú szolgáltatás a frissítés során, ha minden szerepkörhöz legalább két szerepkör-példányok (virtuális gépeken futó). A két szerepkör-példányok egy virtuális gép fog összehívások ügyfél közben a másik frissül.

6. Jelölje be a **telepítés megkezdése** , hogy a frissítés után a csomag a feltöltés befejeződött alkalmazza az.

7. Kattintson **az OK gombra** a kezdéshez frissítése a szolgáltatás.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Útmutató: a szakaszos telepítés termelési előléptetése telepítések felcserélése

Ha úgy dönt, hogy egy felhőalapú szolgáltatásba, a szakasz egy új kiadásának telepítéséhez, és az új kiadásának tesztelje a felhőalapú szolgáltatás átmeneti tárolásra szolgáló környezetben. **Felcserélése** használatával válthat az URL-címeit, amely a két telepítések foglalkozik, és egy új kiadásának termelési előléptetése. 

A **Cloud Services** lapra vagy az irányítópulton telepítések válthat.

1. Az [Azure portálon][]válassza ki a frissíteni kívánt felhőszolgáltatásba. Ebben a lépésben a felhőalapú szolgáltatás példány lap megnyitása

2. A lap kattintson a **felcserélése** gombra.

    ![Cloud Services felcserélése](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Az alábbi megerősítést kérő üzenet nyílik meg.

    ![Cloud Services felcserélése](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Miután megbizonyosodott róla a telepítési információ, kattintson az **OK gombra** a telepítések megjelenítését.

    A telepítési felcserélése gyorsan történik mivel a beállítást, amely módosítja a virtuális IP-címek (VIP) a telepítésekhez.

    Szeretné menteni a számítási költségek, törölheti az átmeneti tárolásra szolgáló telepítési Miután megbizonyosodott róla, hogy a várt módon működnek-e a éles üzemi.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Útmutató: hivatkozás egy erőforrás egy felhőalapú szolgáltatásba

Az Azure portálon nem összekapcsolása erőforrások, mint az aktuális Azure klasszikus portálon működik. További források ehelyett üzembe az azonos erőforráscsoport a felhőalapú szolgáltatást használja.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Útmutató: telepítések és egy felhőalapú szolgáltatásba törlése

Törölhet is egy felhőalapú szolgáltatásba, törölnie kell minden meglévő telepítés.

Szeretné menteni a számítási költségek, törölheti az átmeneti tárolásra szolgáló telepítési Miután megbizonyosodott róla, hogy a várt módon működnek-e a éles üzemi. Vannak, a számítási költségek leállt telepített szerepkör-példányok számlát kapni.

A következő eljárással a telepítő vagy a felhőszolgáltatásában törlése. 

1. Az [Azure portálon][]válassza ki a törölni kívánt felhőszolgáltatásba. Ebben a lépésben a felhőalapú szolgáltatás példány lap megnyitása

2. A lap kattintson a **Törlés** gombra.

    ![Cloud Services felcserélése](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. A teljes felhőszolgáltatásában törlése, jelölje be a **Felhőbeli szolgáltatástól és a telepítéshez** , vagy jelölje ki a **éles üzemi** , vagy a **fejlesztői telepítési**.

    ![Cloud Services felcserélése](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Kattintson a képernyő alján a **Törlés** gombra.

5. A felhőbeli szolgáltatástól törléséhez kattintson a **Törlés felhőszolgáltatásában**. Ezután a megerősítést kérő párbeszédpanelen kattintson az **Igen**gombra.

> [AZURE.NOTE]
> Ha egy felhőalapú szolgáltatás törlődik, és részletes ellenőrzésére van beállítva, törölnie kell az adatokat kézzel a tárterület-fiókjából. Hol találhatók a mértékek táblák információt [ismertető](cloud-services-how-to-monitor.md) témakört is.

[Azure portál]: https://portal.azure.com

## <a name="next-steps"></a>Következő lépések

* [A felhőalapú szolgáltatás általános konfigurálása](cloud-services-how-to-configure-portal.md).
* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy-portal.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name-portal.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md)beállítása.