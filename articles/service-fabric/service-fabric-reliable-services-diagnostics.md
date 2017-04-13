<properties
   pageTitle="Állapot-nyilvántartó megbízható szolgáltatások diagnosztika |} Microsoft Azure"
   description="Diagnosztikai megbízható szolgáltatások megbízható, szűrőként működő funkciók"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnosztikai megbízható szolgáltatások megbízható, szűrőként működő funkciók
A megbízható, szűrőként működő megbízható szolgáltatások StatefulServiceBase osztály [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) események hibakeresési a szolgáltatást, az összefüggéseket a futtatókörnyezet hogyan működik-e, és hibaelhárítási segítséget nyújt használható bocsát ki.

## <a name="eventsource-events"></a>EventSource események
A megbízható, szűrőként működő megbízható szolgáltatások StatefulServiceBase osztály EventSource neve "Microsoft-ServiceFabric-szolgáltatások". Ha a szolgáltatás folyamatban van [a Visual Studióban indítja](service-fabric-debugging-your-application.md)az [Események diagnosztika](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) ablak a forrás eseményeinek jelennek meg.

Eszközök és technológiák összegyűjtése és/vagy EventSource események megtekintése az példák [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [A Microsoft Azure diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md)és a [Microsoft TraceEvent tárat](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Események

|Esemény neve|Esemény azonosítója|Szint|Esemény leírása|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Tájékoztató|Kibocsátott szolgáltatás RunAsync tevékenység indításakor|
|StatefulRunAsyncCancellation|2|Tájékoztató|Amikor a szolgáltatás RunAsync feladat megszakad.|
|StatefulRunAsyncCompletion|3|Tájékoztató|Amikor a szolgáltatás RunAsync feladat befejezése|
|StatefulRunAsyncSlowCancellation|4|Figyelmeztetés|Amikor a szolgáltatás RunAsync tevékenység túl sokáig tart a lemondás befejezéséhez|
|StatefulRunAsyncFailure|5|Hibaüzenet|Amikor a szolgáltatás RunAsync tevékenység kivételt okoz|

## <a name="interpret-events"></a>Események értelmezése

StatefulRunAsyncInvocation, StatefulRunAsyncCompletion és StatefulRunAsyncCancellation eseményeket hasznosak az szolgáltatás író – szolgáltatás, valamint a szolgáltatás kezdése, mondta le, vagy esetén befejeződött időpontját életciklusának megértéséhez. Ez akkor lehet hasznos, amikor szolgáltatásproblémák hibakereséshez, vagy a szolgáltatás életciklusáról ismertetése.

Szolgáltatás szövegírói kell figyeljen események StatefulRunAsyncSlowCancellation és StatefulRunAsyncFailure, mert azt jelzik, hogy a szolgáltatás kapcsolatos problémákat.

Amikor a szolgáltatás RunAsync() tevékenység kivételt okoz StatefulRunAsyncFailure kibocsátott. Általában kivételt hiba vagy a szolgáltatás hibáját jelzi. Emellett a kivétel hatására meghiúsító, a szolgáltatás, így egy másik csomópontjának kerül. Ez a művelet költséges lehet és késleltetheti bejövő felkérést, miközben a szolgáltatás kerül. Szolgáltatás szövegírói határozza meg a probléma okát, a kivétel, majd, ha lehetséges, hogy csökkentésében.

Amikor a RunAsync feladat törlését kér több időt vesz igénybe négy másodpercnél StatefulRunAsyncSlowCancellation kibocsátott. A szolgáltatás túl sokáig tart a lemondás befejezéséhez, amikor a lehetőségét, hogy a szolgáltatást, hogy gyorsan újra kell indítani egy másik csomópont hatással van. Ez hatással lehetnek a szolgáltatás teljes elérhetőségét.
