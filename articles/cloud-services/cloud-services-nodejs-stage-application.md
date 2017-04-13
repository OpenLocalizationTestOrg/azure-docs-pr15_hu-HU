<properties 
    pageTitle="Egy felhőalapú szolgáltatás telepítése (Node.js) szakasz |} Microsoft Azure" 
    description="További információ a fejlesztői környezet az Azure alkalmazás telepítéséhez, majd a használatáról virtuális IP (virtuális) felcserélése munkakörnyezetben telepítéséhez." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Az alkalmazások az Azure előkészítése

Egy csomagolt alkalmazást telepítheti a fejlesztői környezet előtt helyezze át az éles üzemi környezetben, amelyben az alkalmazás érhető el az interneten vizsgálandó Azure-ban. A fejlesztői környezet hasonlít pontosan a munkakörnyezetben, azzal a különbséggel, hogy csak egy kódolt URL-cím Azure által létrehozott szakaszos alkalmazással hozzáférhet. Hogy helyesen működik-e az alkalmazás ellenőrzését követően azt telepíthető az éles üzemi környezetben egy virtuális IP (virtuális) felcserélése hajt végre.

> [AZURE.NOTE] Az ebben a cikkben leírt lépéseket csak az Azure felhőalapú szolgáltatások csomópontot alkalmazásainak vonatkoznak.

## <a name="step-1-stage-an-application"></a>Lépés: 1: Az alkalmazások szakaszban

Ebben a feladatban bemutatja, hogy miként az alkalmazások szakaszban a **Microsoft Azure PowerShell**használatával.

1.  Közzététel szolgáltatást, ha egyszerűen adják át a **-tárolóhely** paraméter a **Közzététel-AzureServiceProject** parancsmag használatával.

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Jelentkezzen be az [Azure klasszikus portálra] , és jelölje ki a **Felhőbeli szolgáltatások**. Miután a felhőbeli szolgáltatástól hoz létre, és a **fejlesztői** oszlop állapota **futó**lett módosítva, kattintson a szolgáltatás neve.

    ![a portál futó szolgáltatás megjelenítése][cloud-service]

3.  Jelölje ki az **Irányítópult**, és válassza a **fejlesztői**.

    ![felhőalapú szolgáltatás irányítópult][cloud-service-dashboard]

4. Megjegyzés: a **Webhely URL-címe** bejegyzés jobbra lévő érték. A DNS által generált Azure kódolt belső azonosító neve.

    ![webhely URL-címe][cloud-service-staging-url]

Most ellenőrizheti, hogy az alkalmazás megfelelően működik a fejlesztői környezet segítségével a fejlesztői webhely URL-CÍMÉT.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Lépés: 2: Csere VIP által gyártási alkalmazás frissítése

Az alkalmazás a fejlesztői környezet frissített verziójának ellenőrzését követően, gyorsan elérhetővé teheti azt gyártási által lecserélése a virtuális IP-címei (VIP), a átmeneti és üzemi környezetben.

> [AZURE.NOTE] Ez a lépés feltételezi, hogy már rendszerbe egy gyártási alkalmazást és az alkalmazás frissített verziójának előkészített.

1.  Jelentkezzen be az [Azure klasszikus portált], kattintson a **Cloud Services** , és válassza a szolgáltatás neve.

2.  Az **Irányítópult**lapon jelölje be a **fejlesztői**, és kattintson a **felcserélése** az oldal alján gombra. Ekkor megnyílik a virtuális csere párbeszédpanel.

    ![virtuális csere párbeszédpanel][vip-swap-dialog]

3.  Tekintse át az információkat, és kattintson **az OK**gombra. A két telepítések kezdje el az átmeneti tárolásra szolgáló telepítési átvált gyártási és – váltás a éles üzemi átmeneti frissítése.

Sikeresen előkészített a telepítéshez, és egy éles üzemi frissítette VIP lecserélése a fejlesztői telepítését.

## <a name="additional-resources"></a>További források

- [A szolgáltatásfrissítés telepítéséről termelési lecserélése VIP Azure-ban]

[Azure klasszikus portál]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[A szolgáltatásfrissítés telepítéséről termelési lecserélése VIP Azure-ban]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
