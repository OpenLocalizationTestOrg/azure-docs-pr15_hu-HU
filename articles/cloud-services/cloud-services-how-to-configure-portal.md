<properties 
    pageTitle="Egy felhőalapú szolgáltatásba (portal) beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként cloud services konfigurálása az Azure-ban. Ismerkedjen meg a felhőben szolgáltatás beállításai frissítése és konfigurálása a szerepkör-példányok távoli eléréséhez. Az alábbi példákban az Azure-portálra." 
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
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Cloud Services konfigurálása

> [AZURE.SELECTOR]
- [Azure portál](cloud-services-how-to-configure-portal.md)
- [Azure klasszikus portál](cloud-services-how-to-configure.md)

Beállíthatja a leggyakrabban használt beállításait egy felhőalapú szolgáltatásba, az Azure-portálon. Vagy, ha szeretné, hogy közvetlenül a konfigurációs fájlok frissítése, töltse le a szolgáltatás konfigurációs fájl frissíteni, és töltse fel a frissített fájlt és frissítése a felhőbeli szolgáltatástól a módosításokat. Mindkét esetben a konfigurációs frissítések elküldte azokat az összes szerepkör példányaiban.

Is kezelheti őket az a felhő szolgáltatás szerepkörök, vagy a távoli asztali példányát.

Azure csak biztosítható 99.95 készültségi szolgáltatáselérhetőség során a konfigurációs frissítések minden szerepkör legalább két szerepkör-példányok esetén. Amely lehetővé teszi, hogy egy virtuális gépen ügyfél összehívások, a másik frissítése közben. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.

## <a name="change-a-cloud-service"></a>Egy felhőalapú szolgáltatásba módosítása

Az [Azure portál](https://portal.azure.com/)megnyitása, után nyissa meg a felhőalapú szolgáltatás. További lehetőségek kezelése számos tulajdonságát. 

![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)

A **Beállítások** lap, ahol **tulajdonságainak**módosítása, megváltoztathatja a **konfigurációja**, a **tanúsítványok**, beállítási **riasztási szabályok**kezelése és felhőalapú szolgáltatást hozzáférési jogosultsággal rendelkező **felhasználók** kezelése az **összes** vagy **beállításait** hivatkozások megnyitják.

![Azure felhőalapú szolgáltatást a beállítások lap](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Az operációs rendszer a felhőalapú szolgáltatást használja az **Azure portálon**nem módosítható, csak ezt a beállítást, az [Azure klasszikus portal](http://manage.windowsazure.com/)segítségével módosíthatja. Ezt a részletes [Itt](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Figyelése

A felhőalapú szolgáltatás értesítéseket vehet. Kattintson a **Beállítások** > **Értesítés szabályok** > **Hozzáadás értesítés**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Itt beállíthatja az értesítés. A **Mertic** legördülő listából, és beállíthatja a következő típusú adatok értesítés.

- Olvassa el a lemez
- Lemezen írási
- A hálózati
- Hálózati meg
- Processzor százalékos 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>A metrikus mozaik figyelése konfigurálása

**Beállítások**használata helyett > **Figyelmeztetési szabályokat**, hogy kattint a **Felhőbeli szolgáltatástól** lap **Figyelés** szakaszában a metrikus csempék közül.

![Felhőbeli szolgáltatástól figyelése](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

További lehetőségek testre szabhatja a diagram használni a csempére, vagy adja hozzá egy szabályt.


## <a name="reboot-reimage-or-remote-desktop"></a>Indítsa újra a rendszert, reimage vagy távoli asztali

Egyelőre nem konfigurálhatók az **Azure portál**távoli asztal. Beállíthatja azonban az [Azure klasszikus portál](cloud-services-role-enable-remote-desktop.md) [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), vagy [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)keresztül. 

Először kattintson a felhőalapú szolgáltatás példánya.

![Felhőalapú szolgáltatás példányának](./media/cloud-services-how-to-configure-portal/cs-instance.png)

A lap az, hogy megnyílik uou kezdeményezése egy távoli asztali kapcsolaton keresztül, távolról indítsa újra a példány vagy távolról reimage (Indítás friss képével) a példány.

![Felhőalapú szolgáltatás példány gomb](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>A .cscfg átkonfigurálása

Előfordulhat, hogy dolgozóját, hogy cloud szolgáltatás – a [szolgáltatás config (cscfg)](cloud-services-model-and-package.md#cscfg) fájlt. Először meg kell a .cscfg fájl letöltése, módosítsa, majd töltse fel.

1. Kattintson a **Beállítások** ikonra, vagy a **minden elérhető beállítás** hivatkozásra kattintva nyissa meg a **Beállítások** lap.

    ![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Kattintson a **konfiguráció** elemre.

    ![Konfigurációs lap](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Kattintson a **Letöltés** gombra.

    ![Letöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Miután frissítette a szolgáltatás konfigurációs fájl, akkor tölthet fel, és a konfigurációs frissítések telepítése:

    ![Töltse fel](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Jelölje ki a .cscfg fájlt, és kattintson az **OK gombra**.

            
## <a name="next-steps"></a>Következő lépések

* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy-portal.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md)beállítása.