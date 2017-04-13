<properties
   pageTitle="Alapértelmezett TEMP mappa mérete túl kicsi a szerepkör |} Microsoft Azure"
   description="Egy felhőalapú szolgáltatás szerepkörhöz adhatja meg a TEMP mappa korlátozott mennyiségű. Ebben a cikkben hasznos tanácsokat a fogyatkozó problémáját kiküszöbölése."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Alapértelmezett TEMP mappa mérete túl kicsi a egy felhőalapú szolgáltatás webes/dolgozó szerepkör

Az alapértelmezett ideiglenes könyvtár egy felhőalapú szolgáltatás dolgozó vagy webhely szerepkör mérete legfeljebb 100 MB, teljes bizonyos pontján válhat tartalmaz. Ez a cikk ismerteti, hogyan a tárhelye a ideiglenes könyvtár elkerülése érdekében.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Miért terület fogyni?

A szokásos Windows környezeti változók TEMP és TT az alkalmazást futtató kód érhetők el. TEMP és a TT mérete legfeljebb 100 MB tartalmazó egyetlen könyvtárra mutat. Ezt a címtárat tárolt adatokat nem állandó át a felhőalapú szolgáltatás; az életciklus Ha a rendszer a szerepkör példányok egy felhőalapú szolgáltatásba, a címtárban törölve van.

## <a name="suggestion-to-fix-the-problem"></a>A probléma megoldásához javaslatok

Végrehajtja a következő lehetőségek közül:

- Helyi tároló erőforrás beállítása, valamint elérheti őket közvetlenül a TEMP vagy TT használata helyett. Hozzáférhet az alkalmazáson belül futó kód helyi tároló erőforrásként, hívja fel a [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) módszert. 

- Állítsa be a helyi tároló erőforrásként, és mutasson a TEMP és TT könyvtárak, mutasson a helyi tároló erőforrás elérési útja. Ez a módosítás belül a [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) módszert kell elvégezni.

Az alábbi példa mutatja be a cél könyvtárak módosításának TEMP és a TT OnStart módszer belül:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Következő lépések

Olvassa el a blogbejegyzésbe, amely ismerteti, [hogyan Azure szerepkör ASP.NET ideiglenes webmappa méretének növeléséhez](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

További [hibaelhárítási cikkek](/?tag=top-support-issue&product=cloud-services) a felhőszolgáltatások megtekintése

Megtudhatja, hogy miként felhőalapú szolgáltatás szerepkör kapcsolatos problémák elhárítása az Azure PaaS számítógépes diagnosztikai adatok használatával, [Kevin Williamson blogsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)megtekintése.
