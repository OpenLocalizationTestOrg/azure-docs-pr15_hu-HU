<properties
   pageTitle="Speciális megbízható szolgáltatások használatát |} Microsoft Azure"
   description="Tudjon meg többet a szolgáltatás háló megbízható szolgáltatások, a szolgáltatások a rugalmasabb speciális használatát."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Speciális programozási modell megbízható szolgáltatások használatát
Azure Service háló leegyszerűsíti írása és megbízható állapot nélküli és állapot-nyilvántartó szolgáltatások kezelése. Ez az útmutató megbízható szolgáltatások további vezérlők, és rugalmasság nyereség jogot a szolgáltatásokra speciális felhasználási lehetőségei az előadás. Ez az útmutató olvasása, előtt megismerkedhet [A megbízható szolgáltatások programozási modell](service-fabric-reliable-services-introduction.md).

Állapot-nyilvántartó és a állapot nélküli szolgáltatásokra van a felhasználó kód két elsődleges belépési pontról:

 - `RunAsync`található egy általános célú a szolgáltatáskód.
 - `CreateServiceReplicaListeners`és `CreateServiceInstanceListeners` az ügyfél kérések kommunikációs hallgatók megnyitása.
 
A legtöbb szolgáltatás a következő két belépési pontról megfelelőek. Ritkán további beállítási lehetőségekre a szolgáltatás életciklus szükség, ha további életciklus események érhetők el.

## <a name="stateless-service-instance-lifecycle"></a>Szolgáltatási állapot nélküli példány életciklus

Állapot nélküli szolgáltatás életciklus rendkívül egyszerű. Állapot nélküli szolgáltatás csak nyitott, zárva, vagy megszakadt. `RunAsync`az állapot nélküli szolgáltatás végrehajtása egy szolgáltatás példányának megnyitásakor, és törölni, amikor egy szolgáltatás példányának bezárása vagy megszakadt. 

Bár `RunAsync` kell lennie a megfelelő szinte minden esetben az meg van nyitva, zárja be, és a megszakítás eseményeinek állapot nélküli szolgáltatás is érhetők el:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  Amikor az állapot nélküli szolgáltatás példány használható OnOpenAsync neve. Bővített szolgáltatás inicializálni tevékenységek ekkor indítható el.

- `Task OnCloseAsync(CancellationToken)`
  Ha az állapot nélküli szolgáltatás példány fog biztonságosan leállítható OnCloseAsync neve. Ez akkor fordulhat elő, ha a szolgáltatáskód frissítése folyamatban van, a szolgáltatás példánya miatt terheléselosztás helyezi át vagy egy ideiglenes (tranziens) hiba lép fel. OnCloseAsync biztonságosan zárja be az erőforrások, minden olyan háttér feldolgozás leállítása, Befejezés külső állapot mentése vagy zárja be a létező kapcsolatok le használható.

- `void OnAbort()`
  Ha az állapot nélküli szolgáltatás példány kényszerítéssel leáll megszakításkor neve. Ha egy állandó hiba lép fel a csomópontra, vagy ha a szolgáltatás háló biztos, hogy nem tudják kezelni a szolgáltatás példánya életciklus belső hibák miatt általában Link.

## <a name="stateful-service-replica-lifecycle"></a>Állapot-nyilvántartó szolgáltatás replika életciklus

Állapot-nyilvántartó szolgáltatás kópia életciklus sokkal összetettebb, mint egy állapot nélküli szolgáltatás példánya. Ezenkívül megnyitása, bezárása és események megszakítása, állapot-nyilvántartó szolgáltatás kópia többször szerepkör változásairól élettartama alatt. Állapot-nyilvántartó szolgáltatás kópia megváltozásakor szerepkör a `OnChangeRoleAsync` esemény bekövetkezik:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync módosításakor az állapot-nyilvántartó szolgáltatás replika van szerepkört, például az elsődleges és másodlagos neve. Elsődleges kópiák kapnak írási állapota (engedélyezettek hozhat létre, és az írási megbízható gyűjtemények). Másodlagos kópiák kapnak állapota (csak Olvasás meglévő megbízható gyűjtemények). A legtöbb munkamennyiség, az állapot-nyilvántartó szolgáltatás, az elsődleges replika történik. Másodlagos kópiák csak olvasható ellenőrzés, jelentés létrehozásához, adatok adatbányászati vagy más csak olvasható feladatok hajthatják végre.

Állapot-nyilvántartó szolgáltatásban csak a elsődleges írási hozzáféréssel rendelkezik állapotát és így általában áll fenn a szolgáltatás mikor végrehajtásához tényleges munka. A `RunAsync` állapot-nyilvántartó szolgáltatás a módszer csak akkor, ha az állapot-nyilvántartó szolgáltatás replika elsődleges végrehajtása. A `RunAsync` módszer megszakad az elsődleges replikált szerepkör megváltozásakor távolabb elsődleges és a Bezárás gombra, és a megszakítás esemény során. 

Használja a `OnChangeRoleAsync` esemény lehetővé teszi, hogy a vonhatnak attól függően, hogy a replikált szerepkör, valamint választ szerepkör módosítása.

Állapot-nyilvántartó szolgáltatás, a függvény szemantikáját és használati eset állapot nélküli szolgáltatásként azonos négy életciklus események is biztosít:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Következő lépések
Szolgáltatás háló kapcsolatos speciális témaköröket az alábbi cikkekben talál:

- [Állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md)

- [Szolgáltatás háló állapot – bevezetés](service-fabric-health-introduction.md)

- [Hibaelhárítás rendszer állapotjelentések használata](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [A szolgáltatás háló fürt erőforrás-kezelő használatával szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)
