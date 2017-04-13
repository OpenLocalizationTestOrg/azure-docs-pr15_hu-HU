<properties
   pageTitle="Méretezhetőség szolgáltatás háló szolgáltatások |} Microsoft Azure"
   description="Megtudhatja, hogy miként méretezheti szolgáltatás háló szolgáltatások"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Szolgáltatás háló alkalmazások méretezése
Azure Service háló egyszerűen terheléselosztó szolgáltatások, partíciót és kópiák méretezhető alkalmazások épülnek fürt csomópontjait. Ez a maximális erőforrás-kihasználtság lehetővé teszi.

Magas skála szolgáltatás háló alkalmazások kétféleképpen érhető el:

1. A partíciók szintjén méretezése

2. A szolgáltatás neve szintjén méretezése

## <a name="scaling-at-the-partition-level"></a>A partíciók szintjén méretezése
Szolgáltatás háló támogatja az egyes szolgáltatásainak szétválasztás több kisebb partíciót be. Particionáló rendszerek által támogatott típusú [áttekintése szétválasztás](service-fabric-concepts-partitioning.md) ismerteti. Az összes partíciót replikái fürt csomópontok közötti helyezkednek el. Fontolja meg egy alacsony kulcsot 0, a nagy kulcsa 99 és négy partíciót egy ranged particionálási séma használó szolgáltatás. Egy három-csomópont fürt a szolgáltatás lehet, hogy kártyaformátumban minden csomóponton az erőforrások megosztása, mint az ábrán négy kópiákkal:

![Partition elrendezés három csomópontokkal](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Csomópontok számának növelése lehetővé teszi, hogy a szolgáltatás háló a erőforrásainak, az új csomóponton használatát áthelyezésével néhány replikán üres csomópontot. A szolgáltatás négy csomópontok számának növelésével most már minden csomópontjának (különböző partíciók), jobban erőforrás-kihasználtság és a teljesítmény lehetővé tevő futó három kópiák.

![Partition elrendezés négy csomópontokkal](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>A szolgáltatás neve szintjén méretezése
A szolgáltatás példány egy példányára az alkalmazás nevét, és a szolgáltatás típusa nevét (lásd: a [szolgáltatás háló alkalmazás életciklusa](service-fabric-application-lifecycle.md)). Szolgáltatás kibocsátása során, adja meg a partíciót (lásd: a [szolgáltatás háló szétválasztás szolgáltatások](service-fabric-concepts-partitioning.md)) program használható.

Az első szintű méretezés el a szolgáltatás neve. Új példányokat egy szolgáltatást, a szétválasztás, mint a régebbi szolgáltatás példányainak válik a foglalt kiosztható engedélyszinteket hozhat létre. Ezzel új szolgáltatás fogyasztói busier melyeket helyett a kisebb elfoglalt szolgáltatás példányainak használja.

Egy növelése a beosztását, valamint a növekvő vagy csökkenő partíciót megszámolja, hogy hozzon létre új szolgáltatás példány partíciót egy újat. Ez a lehetőség összetettsége, azonban bármely igénybe ügyfelek szükség az, hogy mikor és hogyan a másképp elnevezett szolgáltatás használatát.

### <a name="example-scenario-embedded-dates"></a>Példa példa: dátumok beágyazott
A szolgáltatás neve részeként dátumadatok használandó egyik lehetséges forgatókönyv lenne. Használhat például egy olyan minden olyan ügyfeleknek, akik a 2013-ban csatlakozott egy konkrét névvel ellátott szolgáltatás-példány és a felhasználók, akik 2014-es a tartományhoz egy másik nevet. A elnevezési séma lehetővé teszi, hogy programozás útján növelésével a neveket, attól függően, hogy a dátum (2014-es az alábbi módszerek, mint az szolgáltatás-példányt 2014-es létrehozható igény).

Ezt a megközelítést azonban az alkalmazás-specifikus elnevezési információkat, amelyek szolgáltatás háló Tudásbázis hatókörén kívüli használó ügyfelek alapján.

- *Egy elnevezési konvenció*: A 2013-ban, amikor az alkalmazás élő kerül, háló nevű szolgáltatás létrehozása: / alkalmazás/service2013. A második 2013 negyedév közelében hoz létre egy másik szolgáltatás háló nevű: / alkalmazás/service2014. Az alábbi szolgáltatások vannak azonos típusú szolgáltatás. Ezt a megközelítést a az ügyfélnek kell alkalmaznia logika a megfelelő szolgáltatás neve, az év alapján létrehozni.

- *A keresés szolgáltatást használ*: másik mintája egy másodlagos keresési szolgáltatás, ami lehetővé teszi a nevét a szolgáltatás kívánt kulcs megadásához. A keresési szolgáltatás majd hozhat létre új szolgáltatás példányok. A keresési szolgáltatás magát nem megőrzi az alkalmazás adatokról, csak a szolgáltatásnevek létrehozott adatait. Így év-alapú a fenti példában az ügyfél volna első névjegykártyára a keresési szolgáltatás megtudhatja, hogy az adatok kezelésére szolgáló évszám szolgáltatás nevét, és majd használja a szolgáltatás neve a tényleges művelet végrehajtása. Az első keresési eredményének gyorsítótárba helyezhető.

## <a name="next-steps"></a>Következő lépések

Szolgáltatás háló fogalmak a további tudnivalókért lásd: a következő:

- [A szolgáltatás háló szolgáltatások elérhetősége](service-fabric-availability-services.md)

- [Szétválasztás szolgáltatás háló szolgáltatások](service-fabric-concepts-partitioning.md)

- [Létrehozásához és kezeléséhez állam](service-fabric-concepts-state.md)
