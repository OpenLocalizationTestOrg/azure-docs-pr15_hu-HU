<properties
   pageTitle="Tesztelhetőségi: Szolgáltatás kommunikációs |} Microsoft Azure"
   description="Szolgáltatás kapcsolati található egy kritikus integrációs szolgáltatási háló alkalmazást. Ez a cikk azt ismerteti, hogy tervezési szempontjait és tesztelés technikákat."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/06/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-testability-scenarios-service-communication"></a>Szolgáltatás háló tesztelhetőségi alkalmazási helyzetek: kommunikáció szolgáltatás

Microservices és építészeti stílusok szolgáltatás készült felület természetesen Azure Service háló. Az elosztott architektúrákban ilyen típusú componentized microservice alkalmazások általában tevődnek össze kell egymással beszélhet több szolgáltatásban. A legegyszerűbb esetekben akkor is általában ha legalább egy állapot nélküli webszolgáltatás és az állapot-nyilvántartó adatokat tároló szolgáltatás, amely segítségével kell tájékoztatást adni.

Szolgáltatás kapcsolati található egy kritikus integráció az alkalmazás, mivel az egyes szolgáltatásokhoz közzététele egy távoli API egyéb szolgáltatásokhoz. API határai csoportja, amely magában foglalja a I/O általában használata bizonyos ápolói tesztelése és az érvényességi jó mennyiségű szükséges.

Több dolgot esetén e szolgáltatás határai együtt vannak vezetékes elosztott rendszerben:

 - *Protokoll*. Használ-megnövelt együttműködésre HTTP, vagy egy egyéni bináris protokoll a maximális sebesség?
 - A *hiba kezelésének*. Állandó és ideiglenes (tranziens) hiba kezelésének módját? Mi történik, amikor egy másik csomópontjának helyezi a szolgáltatás?
 - *Időkorlátok és a késés*. Állással alkalmazásokban hogyan fog szolgáltatás rétegekre kezelje a késés az objektumhalomban keresztül, és a felhasználó számára?

A beépített szolgáltatás kommunikációs összetevők szolgáltatás háló által biztosított egyikét használja vagy a saját, tesztelje a szolgáltatások között kapcsolati generál e fontos, hogy az alkalmazás tűrőképessége biztosítása.

## <a name="prepare-for-services-to-move"></a>Felkészülés a szolgáltatások áthelyezése

Előfordulhat, hogy Navigálás szolgáltatás példányainak idővel. Ez a különösen igaz, ha a betöltés mértékek az optimális erőforrás egyéni szabott terheléselosztás vannak beállítva. Szolgáltatás háló helyezi át a szolgáltatás példányokat elérhetőségének maximalizálása még frissítések, feladatátadás, méretezési és más esetek, amikor egy elosztott rendszer élettartama során fellépő alatt.

A fürt Navigálás szolgáltatásokat, mint az ügyfelek és más szolgáltatások kell készüljön fel az alábbi két forgatókönyvet kezelje, ha a beszélgetés a szolgáltatáshoz:

- A szolgáltatás példány vagy partíciót replika helyezte, nyújthassunk azt az utolsó óta. Ez a szolgáltatás életciklus normál része, és meg kell várt, és az alkalmazás élettartama során.
- A szolgáltatás példány vagy partíciót replika éppen áthelyezése. Bár a másikra az egyik csomópontra szolgáltatás feladatátvételének szolgáltatás háló nagyon gyorsan fordul elő, lehetnek késleltetést az elérhetőség, ha a kapcsolati összetevő a szolgáltatás lassú indításához.

Ez a helyzet biztonságosan kezelése fontos sima futó rendszer. Ehhez Felhívjuk a figyelmét arra, hogy:

- Minden-szolgáltatás, csatlakoznia kell egy *címet* (például HTTP vagy WebSockets) figyel tartalmaz. Amikor egy szolgáltatás példányának vagy partíciót helyezi, a cím végpont változik. (Ugrás másik IP-címmel egy másik csomópontjának.) A beépített kommunikációs összetevők használja, ha őket újra feloldásával szolgáltatás címek ki fogja kezelni.
- Lehetnek olyan ideiglenes növelése szolgáltatás késés, a szolgáltatás példány elindul a figyelő be újra. Ez attól függ, hogy milyen gyorsan a szolgáltatást a figyelő követően megnyílik a szolgáltatás példánya kerül.
- Bármelyik meglévő kapcsolatok kell zárni, és a szolgáltatás új csomóponton megnyitása után újra meg kell. Egy szabályos csomópont leállítása vagy újraindítása lehetővé teszi a meglévő kapcsolatok kell leállítása ideje.

### <a name="test-it-move-service-instances"></a>Tesztelje: szolgáltatás példányainak áthelyezése

Szolgáltatás háló tesztelhetőségi eszközök segítségével készíthet a változatos módszerekkel, tesztelje a fenti esetek próba példa:

1. Állapot-nyilvántartó szolgáltatás elsődleges replika áthelyezése.

    Állapot-nyilvántartó szolgáltatás partíciót elsődleges replikáját okok tetszőleges számú helyezhető el. Ezzel a cél egy adott partíciót kattintva megtekintheti, hogy a szolgáltatások léphessenek az áthelyezés nagyon ellenőrzött módon elsődleges replikáját.

    ```powershell

    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService

    ```

2. Állítsa le a csomópontot.

    Csomópont leáll, amikor szolgáltatás háló helyezi át a szolgáltatás példányainak vagy a partíciók, amely a többi elérhető csomópont a fürt közül a csomóponton volt. Ezzel a vizsgálati olyan helyzet, ahol csomópont elvész a fürt és az összes szolgáltatás példányok és a csomóponton kópiák kell áthelyeznie.

    A csomópont leállíthatja a **Stop-ServiceFabricNode** PowerShell-parancsmag használatával:

    ```powershell

    PS > Restart-ServiceFabricNode -NodeName Node_1

    ```

## <a name="maintain-service-availability"></a>Szolgáltatás rendelkezésre álljon

Platformot, mint a szolgáltatás háló magas rendelkezésre állás a szolgáltatások célja. De szélső esetben mögöttes infrastruktúra probléma továbbra is okozhat, elérhetetlenség. Fontos, hogy ilyen helyzetekkel ismerkedhet túl tesztelése.

Állapot-nyilvántartó szolgáltatások használata a kvórum rendszerbe való replikáció magas elérhetősége állapotát. Ez azt jelenti, hogy kópiák határozatképességének kell-e írási műveletek végezhetők. Ritkán, például kiterjedt hardver kópiák határozatképességének nem feltétlenül vehető igénybe. Ezekben az esetekben nem tudja írási műveletek hajthatók végre, de továbbra sem lesznek olvasottként hajtanak végre.

### <a name="test-it-write-operation-unavailability"></a>Tesztelje: a művelet elérhetetlenség írása

A tesztelhetőségi eszközökkel szolgáltatás háló, helyezhet el egy hiba, melynek kvórumvesztést, mint a vizsgálat. Bár az ilyen példa ritka, fontos ügyfelek és állapot-nyilvántartó szolgáltatás függő szolgáltatások készíteni azokról a helyzetekről, ahol azok nem lehet kérések írni kezelheti. Fontos is, hogy az állapot-nyilvántartó szolgáltatás magát tudomást ezt a lehetőséget, és biztonságosan kommunikálhat azt hívóknak.

Kvórumvesztést **Invoke-ServiceFabricPartitionQuorumLoss** PowerShell parancsmaggal lehet szükség:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Ebben a példában be van állítva `QuorumLossMode` való `QuorumReplicas` jelzi, hogy szeretnénk kvórumvesztést jutva lefelé az összes kópiák vétele nélkül. Ezzel a módszerrel olvasási műveleteket is továbbra is lehetséges. Hol érhető el egy teljes partíciót példa teszteléséhez is beállíthatja a kapcsoló `AllReplicas`.

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a tesztelhetőségi műveletek](service-fabric-testability-actions.md)

[További tudnivalók a tesztelhetőségi felhasználási területei](service-fabric-testability-scenarios.md)
