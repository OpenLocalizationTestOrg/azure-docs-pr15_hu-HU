<properties
   pageTitle="Szereplők diagnosztika és a figyeléshez |} Microsoft Azure"
   description="Ez a cikk ismerteti a diagnosztikai és a teljesítmény figyelése a szolgáltatás háló megbízható szereplők futtatókörnyezet, az események és a teljesítmény számláló által kibocsátott szolgáltatásait."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnosztikai és a teljesítmény megbízható szereplők figyelése
A megbízható szereplők futtatókörnyezet bocsát [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) események és a [Teljesítmény számláló](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Ezek az összefüggéseket be, hogy üzemel-e a futtatókörnyezet szükséges és teljesítményét figyelve és hibaelhárítási segítséget.

## <a name="eventsource-events"></a>EventSource események
A megbízható szereplők futási idejű EventSource szolgáltató neve "Microsoft-ServiceFabric-szereplők". Amikor a szereplő alkalmazás folyamatban van [a Visual Studióban indítja](service-fabric-debugging-your-application.md)az [Események diagnosztika](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) ablak a forrás eseményeinek jelennek meg.

Eszközök és technológiák összegyűjtése és/vagy EventSource események megtekintése az példák [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md), [Szemantikai naplózás](https://msdn.microsoft.com/library/dn774980.aspx)és a [Microsoft TraceEvent tárat](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Kulcsszavak
Egy vagy több kulcsszavakkal társított összes eseményének, a megbízható szereplők EventSource tartoznak. Gyűjtött események szűrését lehetővé teszi. A következő kulcsszó bittel határozzák meg.

|Bit|Leírás|
|---|---|
|0x1|A fontos eseményeket, amelyek a háló szereplők futtatókörnyezet működésének beállítása|
|0x2|Szereplő módszer hívások leíró események csoportja. További tudnivalókért lásd: a [bevezető témakört: a szereplők](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Szereplő állam kapcsolódó eseményeket csoportja. További információért témakörében [szereplő állam kezelése](service-fabric-reliable-actors-state-management.md).|
|0x8|A szereplő a bekapcsolása-alapú feldolgozási kapcsolódó eseményeket csoportja. További információért témakörében [feldolgozási](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Teljesítmény számláló
A megbízható szereplők futási idejű határozza meg, hogy a következő teljesítmény számláló kategóriákat.

|Kategória|Leírás|
|---|---|
|Szolgáltatás háló szereplő|Tartozó számlálók Azure Service háló szereplők, például időt készített szereplő állapot mentése|
|Szolgáltatás háló szereplő módszer|Számláló adott módszerek szolgáltatás háló szereplők megvalósítani, például gyakoriságának egy szereplő módszer meghívott|

A fenti kategóriák tartalmaz egy vagy több számláló.

A [Windows teljesítmény figyelő](https://technet.microsoft.com/library/cc749249.aspx) alkalmazás, a Windows operációs rendszer alapértelmezés szerint elérhető összegyűjtése és tekinthetik meg a számláló teljesítményadatokat használható. [Azure diagnosztika](../cloud-services/cloud-services-dotnet-diagnostics.md) így egy másik számláló teljesítményadatokat gyűjteni, majd feltölti Azure táblákat.

### <a name="performance-counter-instance-names"></a>Teljesítmény számláló példány nevek
Nagyszámú szereplő szolgáltatások vagy szereplő szolgáltatás partíciót tartalmazó fürt szereplő teljesítmény számláló példányai nagyszámú lesz. A teljesítmény számláló példány nevét segítséget nyújthat az azonosító az adott [partíciót](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) és szereplő módszer (ha van), hogy a teljesítmény számláló példány hozzá van rendelve.

#### <a name="service-fabric-actor-category"></a>Szolgáltatás háló szereplő kategória
A kategória `Service Fabric Actor`, számláló példány nevek felelnek meg a következő formátumban:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* a szolgáltatás háló partíciót Azonosítót, amely a teljesítmény számláló példányhoz társított karakterlánc ábrázolása. A partíciók azonosító egy globálisan egyedi azonosítója, és annak karakteres keresztül jön létre a [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) módszer formátum be "D".

*ActorRuntimeInternalID* egy 64 bites egész szám, a belső használatra a háló szereplők futási idejű által létrehozott karakterlánc ábrázolása. Ez annak egyediségét ütközés más teljesítmény számláló példány nevek elkerülése érdekében és a teljesítmény számláló példány neve szerepel. Felhasználók adatainak értelmezéséhez teljesítmény számláló példány nevét a részét nem próbálkozhat.

Az alábbi képen a számláló példány nevét, amelyhez tartozik egy számláló a `Service Fabric Actor` kategória:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

A fenti példában `2740af29-78aa-44bc-a20b-7e60fb783264` a szolgáltatás háló partíciót azonosító karakterlánc ábrázolása és `635650083799324046` a 64 bites azonosító számára a futtatókörnyezet adatait a belső létrehozott használható.

#### <a name="service-fabric-actor-method-category"></a>Szolgáltatás háló szereplő módszer kategória
A kategória `Service Fabric Actor Method`, számláló példány nevek felelnek meg a következő formátumban:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* pedig a szereplő módszer a teljesítmény számláló példányhoz társított neve. A módszer név formátumát néhány logika a háló szereplők Runtime, hogy a neve, és a Windows teljesítmény számláló példány nevét maximális hossza korlátozó olvashatóságát alapján határozza meg.

*ActorsRuntimeMethodId* 32 bites egész szám, a belső használatra a háló szereplők futtatókörnyezet által létrehozott karakterlánc ábrázolása. Ez annak egyediségét ütközés más teljesítmény számláló példány nevek elkerülése érdekében és a teljesítmény számláló példány neve szerepel. Felhasználók adatainak értelmezéséhez teljesítmény számláló példány nevét a részét nem próbálkozhat.

*ServiceFabricPartitionID* a szolgáltatás háló partíciót Azonosítót, amely a teljesítmény számláló példányhoz társított karakterlánc ábrázolása. A partíciók azonosító egy globálisan egyedi azonosítója, és annak karakteres keresztül jön létre a [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) módszer formátum be "D".

*ActorRuntimeInternalID* egy 64 bites egész szám, a belső használatra a háló szereplők futtatókörnyezet által létrehozott karakterlánc ábrázolása. Ez annak egyediségét ütközés más teljesítmény számláló példány nevek elkerülése érdekében és a teljesítmény számláló példány neve szerepel. Felhasználók adatainak értelmezéséhez teljesítmény számláló példány nevét a részét nem próbálkozhat.

Az alábbi képen a számláló példány nevét, amelyhez tartozik egy számláló a `Service Fabric Actor Method` kategória:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

A fenti példában `ivoicemailboxactor.leavemessageasync` módszer neve `2` hozza létre a futtatókörnyezet belső használatra, 32 bites azonosítója `89383d32-e57e-4a9b-a6ad-57c6792aa521` a szolgáltatás háló partíciót azonosító karakterlánc ábrázolása és `635650083804480486` esetében a futtatókörnyezet belső adatait a 64 bites azonosítója használható.

## <a name="list-of-events-and-performance-counters"></a>Események és a teljesítmény számláló listája

### <a name="actor-method-events-and-performance-counters"></a>Szereplő módszer események és a teljesítmény számláló
A megbízható szereplők futtatókörnyezet az alábbi események [szereplő módszerek](service-fabric-reliable-actors-introduction.md#actors)kapcsolódó bocsát ki.

|Esemény neve|Esemény azonosítója|Szint|Kulcsszavak|Leírás|
|---|---|---|---|---|
|ActorMethodStart|7|Részletes|0x2|Szereplők futtatókörnyezet egy szereplő metódus indításához.|
|ActorMethodStop|8|Részletes|0x2|Egy szereplő módszer befejeződött végrehajtása. Ez azt jelenti, hogy a futtatókörnyezet aszinkron hívás a szereplő módszerrel visszatér, és a visszaadott szereplő módszerrel tevékenység befejeződött.|
|ActorMethodThrewException|9|Figyelmeztetés|0x3|Kivétel történt-szereplő módszer végrehajtása során vagy a futtatókörnyezet aszinkron hívás a szereplő módszerrel, vagy a tevékenység végrehajtása során az szereplő módszerrel adja vissza. Ebben az esetben azt jelzi, hogy valamilyen hiba van szüksége a vizsgálat szereplő kódban.|

A megbízható szereplők futtatókörnyezet teszi közzé a következő teljesítmény számláló szereplő módszerek végrehajtásával kapcsolatos.

|Kategória neve|Számláló neve|Leírás|
|---|---|---|
|Szolgáltatás háló szereplő módszer|Meghívásához/Sec|A szereplő módszerrel meghívott másodpercenként hányszor|
|Szolgáltatás háló szereplő módszer|Átlagos ezredmásodperc / meghívás|Idő ezredmásodpercben a szereplő módszerrel végrehajtani készített|
|Szolgáltatás háló szereplő módszer|A kivételek elő/Sec|Számát, hogy a szereplő szolgáltatás módszer a kivételt / szekundum|

### <a name="concurrency-events-and-performance-counters"></a>Feldolgozási események és a teljesítmény számláló
A megbízható szereplők futtatókörnyezet az alábbi események [feldolgozási](service-fabric-reliable-actors-introduction.md#concurrency)kapcsolódó bocsát ki.

|Esemény neve|Esemény azonosítója|Szint|Kulcsszavak|Leírás|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Részletes|0x8|Ebben az esetben a egy szereplő minden egyes új leadása elején íródott. A függőben lévő bekapcsolása-alapú feldolgozási megőrző szereplő zár váró szereplő hívások számát tartalmazza.|

A megbízható szereplők futtatókörnyezet teszi közzé a következő teljesítmény számláló feldolgozási kapcsolódó.

|Kategória neve|Számláló neve|Leírás|
|---|---|---|
|Szolgáltatás háló szereplő|# Várakozás a szereplő zárolása szereplő hívások|Függőben lévő bekapcsolása-alapú feldolgozási megőrző szereplő zár váró szereplő hívások száma|
|Szolgáltatás háló szereplő|Várja meg a zárolási egy átlagos ezredmásodperc|Eltelt idő (ezredmásodperc) bekapcsolása-alapú feldolgozási megőrző szereplő zárolás|
|Szolgáltatás háló szereplő|Ezredmásodperces átlagos szereplő zárolás|Idő (ezredmásodpercben), amelynek a szereplő van zárolását|

### <a name="actor-state-management-events-and-performance-counters"></a>Szereplő állam management események és a teljesítmény számláló
A megbízható szereplők futási idejű az alábbi események [szereplő állam kezelésével](service-fabric-reliable-actors-state-management.md)kapcsolatos bocsát ki.

|Esemény neve|Esemény azonosítója|Szint|Kulcsszavak|Leírás|
|---|---|---|---|---|
|ActorSaveStateStart|10|Részletes|0x4|Szereplők futtatókörnyezet szereplő állam menteni.|
|ActorSaveStateStop|11|Részletes|0x4|Szereplők futtatókörnyezet befejeződött mentése szereplő állapotát.|

A megbízható szereplők futtatókörnyezet teszi közzé a következő teljesítmény számláló szereplő állam kezelésével kapcsolatos.

|Kategória neve|Számláló neve|Leírás|
|---|---|---|
|Szolgáltatás háló szereplő|Átlagos ezredmásodperc / Mentés művelet.|Idő ezredmásodperc szereplő állapot mentése|
|Szolgáltatás háló szereplő|Átlagos ezredmásodperc / terhelési művelet.|Idő ezredmásodpercben szereplő állam betöltése|

### <a name="events-related-to-actor-replicas"></a>Szereplő kópiák kapcsolódó eseményeket
A megbízható szereplők futási idejű az alábbi események [szereplő kópiák](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors)kapcsolódó bocsát ki.

|Esemény neve|Esemény azonosítója|Szint|Kulcsszavak|Leírás|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Tájékoztató|0x1|Szereplő replika másik szerepkör elsődleges. Ez azt jelenti, hogy a szereplők a partíciók belül az ezen jön létre.|
|ReplicaChangeRoleFromPrimary|2|Tájékoztató|0x1|Szereplő replika másik szerepkör nem elsődleges. Ez azt jelenti, hogy a szereplők a partíciók az ezen belül már nem jön létre. Nincs új kérések szereplők, ezen belül a már létrehozott kerülnek. A szereplők ilyenkor törlődnek, a folyamatban lévő kérelmek végrehajtása után.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Szereplő aktiválása és inaktiválása események és a teljesítmény számláló
A megbízható szereplők futtatókörnyezet az alábbi események [szereplő aktiválása](service-fabric-reliable-actors-lifecycle.md)és inaktiválása bocsát ki.

|Esemény neve|Esemény azonosítója|Szint|Kulcsszavak|Leírás|
|---|---|---|---|---|
|ActorActivated|5|Tájékoztató|0x1|Egy szereplő aktiválva van.|
|ActorDeactivated|6|Tájékoztató|0x1|Egy szereplő inaktiválva.|

A megbízható szereplők futtatókörnyezet teszi közzé a következő teljesítmény számláló szereplő aktiválása és inaktiválása.

|Kategória neve|Számláló neve|Leírás|
|---|---|---|
|Szolgáltatás háló szereplő|OnActivateAsync ezredmásodperces átlagos|Idő ezredmásodpercben OnActivateAsync módszer végrehajtani készített|

### <a name="actor-request-processing-performance-counters"></a>Szereplő összehívás feldolgozása teljesítmény számláló
Amikor egy ügyfél elindítja a via szereplő proxy objektum módszer, a szereplő szolgáltatás a hálózaton keresztül küldött üzenet kérelem eredményez. A szolgáltatás a kérő üzenet folyamatok, és válasza az ügyfélnek. A megbízható szereplők futtatókörnyezet teszi közzé a következő teljesítmény számláló kapcsolódó szereplő összehívás feldolgozása.

|Kategória neve|Számláló neve|Leírás|
|---|---|---|
|Szolgáltatás háló szereplő|# a megválaszolatlan kérdések|A szolgáltatás feldolgozása kérések száma|
|Szolgáltatás háló szereplő|Kérés / átlagos ezredmásodpercben|Eltelt idő (ezredmásodperc) szolgáltatás kérelmének feldolgozása|
|Szolgáltatás háló szereplő|Kérés deszerializáláshoz átlagos ezredmásodpercben|Eltelt idő (ezredmásodperc) deszerializálásához szereplő összehívási üzenetben, amikor őket a szolgáltatást.|
|Szolgáltatás háló szereplő|Válasz szerializáláshoz átlagos ezredmásodpercben|Eltelt idő (ezredmásodperc) a szereplő válaszüzenetet a szolgáltatást a szerializálni, mielőtt a visszajelzés elküldése az ügyfélnek|

## <a name="next-steps"></a>Következő lépések
 - [A szolgáltatás háló platform megbízható szereplők használata](service-fabric-reliable-actors-platform.md)
 - [Szereplő API hivatkozási dokumentáció](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Minta kódot.](https://github.com/Azure/servicefabric-samples)
