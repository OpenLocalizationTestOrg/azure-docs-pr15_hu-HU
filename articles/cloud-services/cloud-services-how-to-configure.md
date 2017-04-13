<properties 
    pageTitle="Egy felhőalapú szolgáltatásba (klasszikus portal) beállítása |} Microsoft Azure" 
    description="Megtudhatja, hogy miként cloud services konfigurálása az Azure-ban. Ismerkedjen meg a felhőben szolgáltatás beállításai frissítése és konfigurálása a szerepkör-példányok távoli eléréséhez." 
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

Beállíthatja a leggyakrabban használt beállításait egy felhőalapú szolgáltatásba, az Azure klasszikus portálon. Vagy, ha szeretné, hogy közvetlenül a konfigurációs fájlok frissítése, töltse le a szolgáltatás konfigurációs fájl frissíteni, és töltse fel a frissített fájlt és frissítése a felhőbeli szolgáltatástól a módosításokat. Mindkét esetben a konfigurációs frissítések elküldte azokat az összes szerepkör példányaiban.

Az Azure klasszikus portálon is lehetővé teszi, hogy [a távoli asztali kapcsolat egy szerepkör az Azure Cloud Services engedélyezése](cloud-services-role-enable-remote-desktop.md)

Azure csak biztosítható 99.95 készültségi szolgáltatáselérhetőség során a konfigurációs frissítések minden szerepkör legalább két szerepkör-példányok esetén. Amely lehetővé teszi, hogy egy virtuális gépen ügyfél összehívások, a másik frissítése közben. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.

## <a name="change-a-cloud-service"></a>Egy felhőalapú szolgáltatásba módosítása

1. Az [Azure klasszikus portálon](http://manage.windowsazure.com/)kattintson a **Felhőszolgáltatások**, kattintással jelölje ki azt a felhőalapú szolgáltatást, és kattintson a **beállítás**.

    ![A konfiguráció lapon](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    **A lap** figyelése konfigurálása, szerepkör beállításainak frissítheti, és válassza ki a vendég operációs rendszer és a család szerepkör-példányok. 

2. **Figyelés**a felügyeleti szint részletes vagy minimális, és konfigurálása a diagnosztika kapcsolat karakterláncok részletes figyelése szükséges.

3. Szolgáltatás szerepkörök (szerepkörök szerint csoportosítva) frissítheti az alábbi beállításokat:
    
    >**Beállítások**  
    >Módosítsa az egyéb konfigurációs beállítások a *ConfigurationSettings* elemeket a szolgáltatás beállítása (.cscfg) fájl a megadott értékeket.
    >
    >**Tanúsítványok**  
    >A tanúsítvány ujjlenyomat egy szerepkör az SSL-titkosítás használt módosítása. Ha módosítani szeretné egy tanúsítványt, először töltse fel az új tanúsítvány (a **tanúsítványok** lapon). Frissítse a ujjlenyomatot a tanúsítvány karakterláncban jelenik meg a szerepkör-beállításokat.

4. **Operációs rendszer**operációs rendszer család vagy szerepkör-példányok verzió módosítása, vagy válassza az **automatikus** ahhoz, hogy az automatikus frissítések az operációs rendszer verziószáma. Az operációs rendszer beállítások alkalmazása a webes és dolgozó szerepeket, de nem érintik virtuális gépeken futó.

    A telepítés során minden szerepkör-példány a legutóbbi operációs rendszer verziója van telepítve, és az operációs rendszerek alapértelmezés szerint automatikusan frissülnek. 
    
    Ha egy másik operációs rendszereken miatt böngészőkompatibilitási követelményei a kód futtatásához a felhőalapú szolgáltatás, megadhatja az operációs rendszer család és verzió. Ha úgy dönt, hogy egy adott operációs rendszer verziója, a felhőbeli szolgáltatástól frissítések automatikus operációs rendszer felfüggeszti a vannak. Szüksége lesz az operációs rendszerek frissítéseket biztosítása.
    
    Ha az alkalmazásokat, hogy a legutóbbi operációs rendszer verziója az összes kompatibilitási problémák megoldásához értékre állításával az operációs rendszer verziószáma **automatikus**engedélyezheti automatikus operációs rendszer frissítéseit. 
    
    ![Operációs rendszer beállításai](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. A beállítások mentéséhez és a szerepkör példányaiban leküldéses őket, kattintson a **Mentés**gombra. (Kattintson az **Elvetés elemet** a módosítások.) **Mentse** és **elvetése** kerülnek be a parancssáv a beállítás módosítása után.

## <a name="update-a-cloud-service-configuration-file"></a>Egy felhőalapú szolgáltatás konfigurációs fájl módosítása

1. Töltse le egy felhőalapú szolgáltatás konfigurációs fájl (.cscfg) az aktuális beállításokkal. **A lap a felhőalapú szolgáltatás** kattintson a **Letöltés**gombra. Ezután kattintson a **Mentés**gombra, és kattintson a **Mentés másként** menteni a fájlt.

2. Miután frissítette a szolgáltatás konfigurációs fájl, akkor tölthet fel, és a konfigurációs frissítések telepítése:

    1. **A lap** kattintson a **Feltöltés**elemre.
    
        ![Töltse fel a konfiguráció](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. **Konfigurációs fájl**használatával **böngészése** jelölje ki a frissített .cscfg fájlt.
    
    3. Ha a felhőalapú szolgáltatás csak egy példánya van bármely szerepköröket tartalmaz, jelölje be a a **konfiguráció alkalmazása, akkor is, ha egy vagy több szerepköröket tartalmaz egy példányát** jelölőnégyzetet, a szerepkörök, a folytatáshoz a konfigurációs frissítések engedélyezése.
    
        Legalább két példánya minden szerepkör határozza meg, kivéve az Azure legalább 99.95 készültségi nem garantálja konfigurációs szolgáltatásfrissítések során a felhőalapú szolgáltatás elérhetőségét. További tudnivalókért olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.
    
    4. Kattintson az **OK gombra** (be van jelölve). 


## <a name="next-steps"></a>Következő lépések

* Megtudhatja, hogyan [egy felhőalapú szolgáltatás üzembe](cloud-services-how-to-create-deploy.md).
* Állítsa be [egyéni tartománynevet](cloud-services-custom-domain-name.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage.md).
* [Távoli asztali kapcsolat szerepet Azure Cloud Services engedélyezése](cloud-services-role-enable-remote-desktop.md)
* [Az ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md)beállítása.
