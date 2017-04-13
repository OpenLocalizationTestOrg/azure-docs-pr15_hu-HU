<properties
   pageTitle="Az esemény-nyomkövetés elhárítása |} Microsoft Azure"
   description="A leggyakoribb problémákat történt a Microsoft Azure Service háló szolgáltatások telepítésével."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Amikor rendszerbe állítják Azure Service háló szolgáltatások a kapcsolatos gyakori problémák megoldása

[Visual Studio hibakeresési eszközök](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)könnyen használható futtat services developer a számítógépen, akkor. Távoli fürt [állapotjelentések](service-fabric-view-entities-aggregated-health.md) esetén mindig egy jó kiindulási pontot. Ezeket a jelentéseket a legegyszerűbben vannak PowerShell vagy [SFX](service-fabric-visualizing-your-cluster.md)keresztül. Ez a cikk feltételezi, hogy a távoli fürtre hibakeresés alatt, és, hogyan használhatja ezeket az eszközöket közül az alapszintű megértése.

##<a name="application-crash"></a>Alkalmazás összeomlik
A "partíciót nem éri el kópia vagy példány darab Megcélozhat" jelentés egy jó utal, hogy a szolgáltatás összeomlik. Megtudhatja, hogy a szolgáltatás összeomlik, amikor megnyitja kissé további vizsgálat. A szolgáltatást futtató méretezés a legjobb barátja egy sor olyan jól átgondolt halad lesznek.  Javasoljuk, hogy ezeket a nyomkövetési naplók gyűjteni, és megoldást például [Rugalmas keresési](service-fabric-diagnostic-how-to-use-elasticsearch.md) használatának megjelenítési és a Keresés a halad próbálja meg [Azure diagnosztika](service-fabric-diagnostics-how-to-setup-wad.md) .

![SFX partíciót állapota](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Szolgáltatás vagy szereplő inicializálni során
Esetleges kivételek inicializálni van a szolgáltatás típusa, mielőtt elindítja a folyamat összeomolhat. Az ilyen összeomlik az alkalmazás eseménynaplójába jelennek meg a hiba a szolgáltatásból.
Ezek a leggyakoribb kivételeket a szolgáltatás van inicializált előtt meg szeretné tekinteni.

***System.IO.FileNotFoundException***

Ez a hiba legtöbbször hiányzó összeállítás függőségek miatt. Jelölje be a Visual Studio vagy a csomópont nyitva CopyLocal tulajdonságot.

***System.Runtime.InteropServices.COMException***
 *a System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 Ez azt jelzi, hogy a regisztrált szolgáltatás neve nem egyezik meg a szolgáltatás jegyzék.

[Azure diagnosztika](service-fabric-diagnostics-how-to-setup-wad.md) beállítható automatikusan feltölteni az összes csomópont alkalmazás eseménynaplójában talál.

###<a name="runasync-or-onactivateasync"></a>RunAsync() vagy OnActivateAsync()
Az összeomlást inicializálni vagy a regisztrált szolgáltatás típusa vagy szereplő futtatása során történik, ha a program kell kivételhiba lépett Azure Service háló szerint. Ezek a EventSource szolgáltatóktól részletesen a "Következő lépések" szakasz tekintheti meg.

## <a name="next-steps"></a>Következő lépések

További információ a meglévő diagnosztika szolgáltatás háló által biztosított:

* [Megbízható szereplők diagnosztika](service-fabric-reliable-actors-diagnostics.md)
* [Megbízható szolgáltatások diagnosztika](service-fabric-reliable-services-diagnostics.md)
