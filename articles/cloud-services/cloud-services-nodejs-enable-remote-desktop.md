<properties 
    pageTitle="Távoli asztal (Node.js) cloud Services engedélyezése" 
    description="Megtudhatja, hogy miként engedélyezése a távoli asztali access-a virtuális gépeken futó az Azure Node.js alkalmazást." 
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

# <a name="enabling-remote-desktop-in-azure"></a>Engedélyezi a távoli asztali Azure-ban

Távoli asztali lehetővé teszi, hogy az Azure-ban futó szerepkör-példány asztali érhető el. A távoli asztali kapcsolat használatával állítsa be a virtuális gép vagy az alkalmazással kapcsolatos hibák elhárítása.

> [AZURE.NOTE] Ez a cikk Node.js alkalmazások Azure felhőalapú szolgáltatások is érinti.


## <a name="prerequisites"></a>Előfeltételek

- Telepítse és állítsa be a [Azure Powershell](../powershell-install-configure.md).
- Ha egy Azure felhőalapú szolgáltatásba, Node.js alkalmazások terjesztése. További tudnivalókért olvassa el a [létre és helyezhetnek üzembe a Node.js alkalmazások, ha egy felhőalapú szolgáltatásba, Azure](cloud-services-nodejs-develop-deploy-app.md)című témakört.


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Lépés: 1: Azure PowerShell használatá a távoli asztali access-szolgáltatás konfigurálása

Távoli asztal használatához frissítenie kell az Azure szolgáltatás meghatározása és konfigurálása a felhasználónév, a jelszó és a tanúsítvány. 

Olyan számítógépről, amely tartalmazza az alkalmazás a forrásfájlokat, hajtsa végre az alábbi lépéseket.

1. **A Windows PowerShell** Futtatás rendszergazdaként. (A **Start menüből** vagy **Kezdőképernyőjén**, keressen rá **A Windows PowerShell**.)

2.  Nyissa meg a szolgáltatás definition (.csdef) és a szolgáltatás beállítása (.cscfg) fájlokat tartalmazó könyvtár.

3. Írja be a PowerShell-parancsmagot:

        Enable-AzureServiceProjectRemoteDesktop

4. A parancssorba írja be a felhasználónevét és jelszavát.

    ![Enable-azureserviceprojectremotedesktop][enable-rdp]

3.  Írja be a PowerShell-parancsmagot a módosítások közzétételére:

        Publish-AzureServiceProject

    ![közzététel azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Lépés: 2: A szerepkör-példány csatlakoztatása

Az update szolgáltatás meghatározása a közzététel után csatlakozhat az szerepkör-példányt.

1.  Az [Azure klasszikus portálon]kattintson a **Felhőszolgáltatások** , és válassza a a szolgáltatásban.

    ![Azure klasszikus portál][cloud-services]

2.  Kattintson a **példány**, és válassza a **gyártási** vagy **előkészítése** az példányokat a szolgáltatásbeli megtekintéséhez. Válasszon egy példányt, és kattintson a **Csatlakozás** az oldal alján.

    ![A példányok lap][3]

2.  Amikor a **Csatlakozás**gombra kattintott, a a böngésző kéri, hogy .rdp fájlt menteni. Nyissa meg a fájlt. (Például ha az Internet Explorer használata esetén kattintson a **Megnyitás**gombra.)

    ![Kérdezzen rá a .rdp fájl megnyitását vagy mentését][4]

3.  A fájl megnyitásakor az alábbi biztonsági üzenet jelenik meg:

    ![A Windows biztonsági figyelmeztetést][5]

4.  Kattintson a **Csatlakozás**, és a példány eléréséhez hitelesítő adatok beírása egy biztonsági figyelmeztetést fog megjelenni. Írja be a jelszót az [1] létrehozott [lépés 1: távoli asztali access-Azure PowerShell használatával a szolgáltatás konfigurálása], és kattintson **az OK**gombra.

    ![felhasználónevével és jelszavával figyelmeztető üzenet][6]

A kapcsolat létrejött, a távoli asztali kapcsolat jeleníti meg az asztali példány Azure-ban. 

![Távoli asztali kapcsolat][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Lépés 3: Távoli asztali hozzáférés letiltása a szolgáltatás konfigurálása 

Ha már nincs szüksége távoli asztali kapcsolatok, a szerepkör példányaiban a felhőben, [Azure PowerShell]használatával távoli asztali hozzáférés letiltása lehetőséget.

1.  Írja be a PowerShell-parancsmagot:

        Disable-AzureServiceProjectRemoteDesktop

2.  Írja be a PowerShell-parancsmagot a módosítások közzétételére:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>További források

- [Távoli hozzáférés szerepkör-példányok Azure-ban] 
- [Távoli asztali változatában Azure szerepkörök]
- [NODE.js Developer Center](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klasszikus portál]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Távoli hozzáférés szerepkör-példányok Azure-ban]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Távoli asztali változatában Azure szerepkörök]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 