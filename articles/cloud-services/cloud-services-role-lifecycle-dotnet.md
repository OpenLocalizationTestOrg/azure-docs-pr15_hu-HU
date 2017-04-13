<properties 
pageTitle="Kezelheti a Felhőbeli szolgáltatástól életciklus-események |} Microsoft Azure" 
description="Megtudhatja, hogyan használható a életciklusra módszerek egy felhőalapú szolgáltatásba szerepkör .NET" 
services="cloud-services" 
documentationCenter=".net" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="customize-the-lifecycle-of-a-web-or-worker-role-in-net"></a>Egy webhely vagy dolgozó szerepkör .NET életciklusának testreszabása

Amikor létrehoz egy dolgozó szerepkört, kibővíti az [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) osztály, amely biztosítja, hogy felülírja az módszereket, amelyekkel életciklus események megválaszolása. Webes szerepkörök az osztály nem kötelező, így életciklus események válaszolnia kell használni.

## <a name="extend-the-roleentrypoint-class"></a>Kijelölés méretének növelése a RoleEntryPoint osztály

A [RoleEntryPoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx) osztály tartalmaz, amely az Azure által meghívott módszerek amikor azt **elindul**, **fut,**és **leállítja** a webes vagy dolgozó szerepkör. Másik lehetőségként felülbírálhatja ezeket a módszereket, amelyekkel szerepkör inicializálni, szerepkör leállítás sorozatok vagy a szerepkör a végrehajtás szál kezelése. 

**RoleEntryPoint**bővítésekor tartsa szem előtt a módszerek az alábbiak egyike:

-   [OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) és [OnStop](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) módszerek olyan logikai értéket vissza, ezért ezeket a módszereket **FALSE értéket** eredményül.

     A kód **hamis**értéket ad vissza, ha a szerepkör folyamat váratlanul megszakad, bármilyen előfordulhat, hogy már a helyükön leállítási folyamat futtatása nélkül. Általánosságban elmondható ne a **OnStart** módszer **hamis** visszaadása.
     
-   Bármelyik nem kezelt kivételt belül **RoleEntryPoint** módszer túlterhelés esetén nem kezelt kivételt kell kezelni.

     Kivétel belül az életciklus módszerekkel esetén Azure emelni fogja [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) eseményt, és kattintson a folyamat megszakad. Miután a szerepkör offline állapotban van, Azure által újraindul. Esetén nem kezelt kivételt fordul elő, amikor a [leállítása](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.stopping.aspx) az esemény nem következik be, és a **OnStop** módszerrel nem hívja meg.

Ha szerepköre nem indul el, vagy a inicializálása, elfoglalt, és a megállás államok között újrafelhasználás van, akkor a kód értesítő esetén nem kezelt kivételt belül minden alkalommal, amikor a szerepkör újraindítása életciklus események egyikét. Ebben az esetben a [UnhandledException](https://msdn.microsoft.com/library/system.appdomain.unhandledexception.aspx) esemény segítségével határozza meg a probléma okát, a kivétel, és kezelje megfelelően. A [Futtatás](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) metódus, indítsa újra a szerepkört okozó is előfordulhat, hogy Visszatérés a szerepének. Telepítési államok kapcsolatos további tudnivalókért [Szerepkörök](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)című témakör tartalmaz gyakori problémák amely okai lehetnek a Lomtárban.

> [AZURE.NOTE] Az **Azure-eszközök a Microsoft Visual Studio** segítségével kialakítása az alkalmazást, ha a szerepkör a project-sablonok automatikusan bővítése a **RoleEntryPoint** osztály, a *WebRole.cs* és *WorkerRole.cs* fájlokban.

## <a name="onstart-method"></a>OnStart módszer

Amikor a szerepkör-példány online állapotba kerül Azure által a **OnStart** módszer neve. A OnStart kód fut, amíg a szerepkör-példány van megjelölve, **elfoglalt** , és nincs külső forgalmat a terheléselosztó által irányítja át azt. Ez a módszer vonhatnak inicializálni, például eseménykezelők végrehajtási és [Azure diagnosztika](cloud-services-how-to-monitor.md)indítása felülbírálhatja.

**OnStart** **Igaz**értéket ad vissza, ha a példány sikerült inicializálni van, és Azure felhívja a **RoleEntryPoint.Run** módszert. **OnStart** **hamis**értéket ad vissza, ha a szerepkör ér véget, azonnal minden tervezett leállítási sorozatok végrehajtása nélkül.

A következő példa bemutatja, hogyan **OnStart** módszer felülbírálása. Ez a módszer állítja be, és elindítja a diagnosztikai monitoron, a szerepkör-példány indításakor, és a naplózás adatok továbbítása a tárterület-fiók beállítása:

```csharp
public override bool OnStart()
{
    var config = DiagnosticMonitor.GetDefaultInitialConfiguration();

    config.DiagnosticInfrastructureLogs.ScheduledTransferLogLevelFilter = LogLevel.Error;
    config.DiagnosticInfrastructureLogs.ScheduledTransferPeriod = TimeSpan.FromMinutes(5);

    DiagnosticMonitor.Start("DiagnosticsConnectionString", config);

    return true;
}
```

## <a name="onstop-method"></a>OnStop módszer

A **OnStop** módszer neve után egy szerepkör-példány offline állapotban van, az Azure, és a folyamat kilép előtt. Ezt a módszert, ha fel szeretne hívni a szerepkör-példány írnia leállítása szükséges kód felülbírálhatja.

> [AZURE.IMPORTANT] **OnStop** módszer futó kód kívül a felhasználó által kezdeményezett leállítása okokból hívásakor befejezéséhez korlátozott időre van. Ez az idő elteltével a folyamat megszakad, ezért gondoskodnia kell arról, hogy gyorsan futtassa a **OnStop** módszer lehet kódot, vagy eltűr száma nem fut. A **OnStop** módszer neve után a **leállítása** az esemény következik.


## <a name="run-method"></a>A módszer futtatása

A **Futtatás** módszer a szerepkör-példány hosszabb ideig futó szál végrehajtásához felülbírálhatja.

A **Futtatás** módszer felülbírálása használata nem kötelező; az alapértelmezett megvalósítás elindul olyan szál, amely alvó örökkévalóság állapotban van. A **Futtatás** módszer felülbírálása, ha a kód végtelen időre szóló kell legyen. Ha a **Futtatás** módot ad vissza, a szerepkör biztonságosan automatikusan a rendszer; Ez azt jelenti Azure hatványra **leállítása** eseményt, és a hívások a **OnStop** módszer, hogy a Leállítás sorozatok hajtható végre, mielőtt offline módba a szerepkör.


### <a name="implementing-the-aspnet-lifecycle-methods-for-a-web-role"></a>Végrehajtási egy webes szerepkör ASP.NET életciklus módszerei

ASP.NET életciklus módszerek, nem a **RoleEntryPoint** osztály által biztosított inicializálni és a Leállítás sorozatok egy webes szerepkör kezelésének is használhatja. Ez akkor lehet hasznos, kompatibilitási célra, ha vannak portolása a Azure egy meglévő ASP.NET alkalmazást. ASP.NET életciklus módszerek **RoleEntryPoint** módszerek belül az úgynevezett. A **alkalmazás\_indítása** módszer a **RoleEntryPoint.OnStart** módszer befejezése után neve. A **alkalmazás\_vége** módszer neve előtt a **RoleEntryPoint.OnStop** módszer neve.

## <a name="next-steps"></a>Következő lépések
Megtudhatja, hogy miként hozhat [létre egy felhőalapú szolgáltatás csomagot](cloud-services-model-and-package.md).