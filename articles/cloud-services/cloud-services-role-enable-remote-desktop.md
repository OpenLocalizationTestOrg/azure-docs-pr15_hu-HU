<properties 
pageTitle="Távoli asztali kapcsolat szerepet Azure Cloud Services engedélyezése" 
description="Az azure felhő szolgáltatásalkalmazás engedélyezése a távoli asztali kapcsolatok konfigurálása" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Távoli asztali kapcsolat szerepet Azure Cloud Services engedélyezése

>[AZURE.SELECTOR]
- [Azure klasszikus portál](cloud-services-role-enable-remote-desktop.md)
- [A PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Távoli asztali lehetővé teszi, hogy szerepkörbe Azure-ban futó asztali érhető el. Távoli asztali kapcsolat használatával kapcsolatos hibák elhárítása és az alkalmazás problémáinak diagnosztizálása, futása közben is. 

Engedélyezheti egy távoli asztali kapcsolat a szerepkör a fejlesztés során a távoli asztali modulokat a szolgáltatás definíciójának felvételével, vagy választhatja engedélyezése a távoli asztali bővítmény keresztül távoli asztali. Az előnyben részesített megközelítés, használhatja a távoli asztali bővítmény, mint a távoli asztali engedélyezheti az alkalmazás telepítve van a anélkül, hogy az alkalmazás újratelepítése után. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Állítsa be a távoli asztali az Azure klasszikus portálról
Az Azure klasszikus portált a távoli asztali bővítmény megközelítés használ, így a távoli asztali engedélyezheti után az alkalmazás telepítve van. **A lap a felhőalapú szolgáltatás** lehetővé teszi engedélyezése a távoli asztali, majd a helyi rendszergazdafiók a virtuális gépeken futó használja, a tanúsítvány használt hitelesítési és a lejárati dátum beállítása. 


1. Kattintson a **Felhőszolgáltatások**, kattintással jelölje ki azt a felhőalapú szolgáltatást, és kattintson a **beállítás**.

2. Kattintson a **távoli**.
    
    ![Távoli felhőszolgáltatások](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Az összes szerepkör-példányok újra kell indítani, ha először engedélyezése a távoli asztali, majd kattintson az OK gombra (be van jelölve). A számítógép újraindítása elkerülése érdekében a jelszó titkosítására használt tanúsítvány telepítenie kell a szerepkör. [Töltse fel a felhőbeli szolgáltatástól tartozó tanúsítvány](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) újraindításra megelőzése, és térjen vissza az ezen a párbeszédpanelen.
    

3. **Szerepkörök**jelölje ki a kívánt szerepkört, frissítése, és válassza az **összes** az összes szerepkörének.

4. Végezze el az alábbi módosításokat:
    
    - Ahhoz, hogy a távoli asztali, jelölje be a **Távoli asztal engedélyezése** jelölőnégyzetet. Távoli asztali letiltásához törölje a jelet a jelölőnégyzetből.
    
    - Hozzon létre egy fiókot a szerepkör példányaiban távoli asztali kapcsolat.
    
    - Frissítse a meglévő fiók jelszava.
    
    - Jelöljön ki egy feltöltött tanúsítványt, használja a hitelesítéshez (feltöltés a tanúsítvány **Töltse fel** a **tanúsítványok** lapon), vagy hozzon létre egy újat.. 
    
    - A távoli asztali konfiguráció elévülési dátumának módosítása

5. Ha konfigurációs módosításokat végzett, kattintson az **OK gombra** (be van jelölve).


## <a name="remote-into-role-instances"></a>Szerepkör-példányok be távoli
Miután engedélyezte a szerepkörök távoli asztali távoli is be egy szerepkör-példány különböző eszközök között.

Kapcsolódás egy szerepkör-példányt az Azure klasszikus portálról:
    
  1.   Kattintson a **példány** nyissa meg a **példányok** lapot.
  2.   Jelölje ki a egy szerepkör-példányt, amelyen beállította a távoli asztal.
  3.   Kattintson a **Csatlakozás**gombra, és az utasításokat követve nyissa meg az asztalon. 
  4.   Kattintson a **Megnyitás** , majd **Csatlakozás** a távoli asztali kapcsolat indítása. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Az egy szerepkör-példány távoli Visual Studio használata

A Visual Studióban, a kiszolgáló Explorer:

1. Bontsa ki a **Azure\\Cloud Services\\[felhőalapú szolgáltatás neve]** csomópontot.
2. Bontsa ki a **fejlesztői** vagy a **gyártási**.
3. Bontsa ki az adott szerepkört.
4. Kattintson a jobb gombbal a szerepkör-példányok közül, kattintson a **Csatlakozás távoli asztali... használ**, és írja be a felhasználónevét és jelszavát. 

![Kiszolgáló explorer távoli asztali](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Ismerkedés a RDP-fájlt a PowerShell használatával
A [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag segítségével beolvashatja a RDP-fájlt. Használhatja a RDP-fájlt a távoli asztali kapcsolat elérheti a felhőalapú szolgáltatást.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Az RDP-fájlt az szolgáltatás felügyeleti REST API-k letöltése
A [RDP-fájl letöltése](https://msdn.microsoft.com/library/jj157183.aspx) többi művelet segítségével töltse le a RDP-fájlt. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Távoli asztali konfigurálása a szolgáltatás-definíciós fájl

Ezzel a módszerrel engedélyezése a távoli asztali fejlesztés során az alkalmazáshoz. Ez a módszer igényli titkosított jelszavak tárolni a szolgáltatás konfigurációban fájl és valamilyen frissítés a távoli asztali konfigurációja lenne szükség egy újratelepítés az alkalmazás. Ha el szeretné kerülni az alábbi downsides a fentebb ismertetett távoli asztali bővítmény alapú megközelítés kell használnia.  

Visual Studio segítségével, [engedélyezze a távoli asztali kapcsolat](../vs-azure-tools-remote-desktop-roles.md) használja a szolgáltatás definíciós fájl megközelítés.  
Az alábbi lépéseket ismertetik a módosításokat, a szolgáltatás modellfájlt szükséges ahhoz, hogy a távoli asztal. Visual Studio fog automatikusan hoz létre a módosítások közzétételekor.

### <a name="set-up-the-connection-in-the-service-model"></a>A szolgáltatás modell a kapcsolat beállítása 
Használja az **import** elemet a **távelérési** és a **RemoteForwarder** modul importálhatja a [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) fájlt.

A szolgáltatás-definíciós fájl a következőképpen fog kinézni a következő példa a `<Imports>` hozzáadása elemet.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
A [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) fájl kell a következő példa hasonló, vegye figyelembe a `<ConfigurationSettings>` és `<Certificates>` elemeket. A megadott tanúsítvánnyal kell lennie, [töltenek fel a felhőalapú szolgáltatást](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>További források

[Cloud Services konfigurálása](cloud-services-how-to-configure.md)