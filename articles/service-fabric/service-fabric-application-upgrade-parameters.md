
<properties
   pageTitle="Frissítés alkalmazása: frissítési paraméterek |} Microsoft Azure"
   description="Paraméterek szolgáltatás háló-alkalmazást, például állapot-ellenőrzései végrehajtásához és automatikusan visszavonása a frissítés házirendek frissítéssel kapcsolatos ismerteti."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Alkalmazás frissítési paraméterei

Ez a cikk ismerteti a különböző paraméterek az Azure Service háló alkalmazás a frissítés során alkalmazható. A paraméterek neve és az alkalmazás verziójának tartalmazza. , Amelyek az adatokon való műveletvégzés és a frissítés során alkalmazott állapot-ellenőrzései belül, és azokat meg, hogy házirendek kell alkalmazni, ha a frissítés sikertelen lesz.


<br>

| Paraméter | Leírás |
| --- | --- |
| ApplicationName | Az alkalmazás frissíti a rendszer neve. Példa: háló: / VisualObjects, háló: / ClusterMonitor  |
| TargetApplicationTypeVersion | Az alkalmazás verziójának írja be, hogy a frissítés célokat. |
| FailureAction | A művelet úgy szolgáltatás háló olyankor, amikor a frissítés sikertelen lesz. Az alkalmazás előfordulhat, hogy állítható vissza a frissítés előtti verzióját (visszaállítás), vagy a frissítés előfordulhat, hogy le kell állítani a jelenlegi frissítés tartományban. Az utóbbi esetben a frissítési üzemmód is kézi változik. Engedélyezett értékei visszaállítás és a kézi választógombot. |
| HealthCheckWaitDurationSec | A ideig várjon (másodpercben), a frissítés előtt szolgáltatás háló frissítési tartományban befejeződése után az alkalmazás állapotának értékeli ki. Ez az időtartam tekinteni az alkalmazás futnia kell előtt meg lehet tekinteni megfelelő idő is. Ha az állapot-ellenőrzés halad, a frissítési folyamat a következő frissítés tartományhoz folytatódik.  Nem sikerül az állapot-ellenőrzés, szolgáltatás háló várakozik az állapot-ellenőrzés újra újraküldése, a HealthCheckRetryTimeout eléréséig előtt intervallumhoz (UpgradeHealthCheckInterval). Az alapértelmezett javasolt érték pedig 0 másodperc. |
| HealthCheckRetryTimeoutSec | Az időtartam (másodperc), hogy szolgáltatás háló továbbra is végezze el az állapot kiértékelés előtt deklarálása a frissítés, ahogy sikertelen volt. Az alapértelmezett érték 600 másodperc. Ez az időtartam HealthCheckWaitDuration elérése után kezdődik. Ez a HealthCheckRetryTimeout belül szolgáltatás háló előfordulhat, hogy hajtsa végre az alkalmazás egészségügyi több állapot-ellenőrzései. Az alapértelmezett érték 10 perc, és az alkalmazás kell testreszabható meg megfelelően. |
| HealthCheckStableDurationSec | Az időtartam (másodpercben) ellenőrizze, hogy az alkalmazás stabil áthelyezése a következő frissítés tartomány vagy a frissítés végrehajtása előtt. A várakozási időtartam módosításoktól a rendszer nem észleli a megfelelő állapot az állapot-ellenőrzés végrehajtása utáni szolgál. Az alapértelmezett érték 120 másodperc, és az alkalmazás kell testreszabható meg megfelelően. |
| UpgradeDomainTimeoutSec | Maximális ideje (másodpercben) egyetlen frissítési tartomány frissítése. A időtúllépési lejár, ha a frissítés leáll, és folytatja a UpgradeFailureAction beállítása alapján. Az alapértelmezett érték soha nem (végtelen) és az alkalmazás kell testreszabható meg megfelelően. |
| UpgradeTimeout | A teljes frissítés alkalmazott (másodpercben) időkorlátja. A időtúllépési lejár, ha a frissítés végpontok és UpgradeFailureAction induljanak. Az alapértelmezett érték soha nem (végtelen) és az alkalmazás kell testreszabható meg megfelelően. |
| UpgradeHealthCheckInterval | A gyakoriság, hogy be van-e jelölve az állapot állapotát. A paraméter _cikkét_ _fürt_ ClusterManager részében megadva, és nem szerepel a frissítési parancsmag részeként. Az alapértelmezett értéke 60 másodperc.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Szolgáltatás állapota kiértékelés alkalmazása a frissítés során

<br>
Állapot kiértékelés feltételek esetén nem kötelező. A rendszerállapot értékelési szempontokat frissítésének indításakor nincs megadva, ha a szolgáltatás háló használja az alkalmazás állapot házirendek az alkalmazás-példány ApplicationManifest.xml megadott.


<br>

| Paraméter | Leírás |
| --- | --- |
| ConsiderWarningAsError | Alapértelmezett értéke hamis. Hibaként az alkalmazáshoz állapota figyelmeztetések a frissítés során alkalmazás állapotának kiértékelésekor. Alapértelmezés szerint a szolgáltatás háló nem értékeli figyelmeztető állapot események értéke lehet hibák (hibák), így a frissítés folytatható, akkor sem, ha figyelmeztető események.   |
| MaxPercentUnhealthyDeployedApplications | Alapértelmezett javasolt érték pedig 0. Adja meg a telepített alkalmazások (lásd az [állapotjelző szakasz](service-fabric-health-introduction.md)) is lehet megsérült, az alkalmazás számít sérült, és nem sikerül a frissítés előtt maximális száma. A paraméter határozza meg az alkalmazás állapota a csomóponton és segítséget nyújt a frissítés során felderítheti a problémákat. Általában a replikák az alkalmazás első terheléselosztás más csomópontot, amely lehetővé teszi, hogy az alkalmazás jelennek meg a megfelelő, így a frissítés folytatásához. A szigorú MaxPercentUnhealthyDeployedApplications állapot megadásával szolgáltatás háló gyorsan észleli az alkalmazáscsomag probléma, és olyan készíthet, amelyek egy gyors frissítés sikertelen. |
| MaxPercentUnhealthyServices | Alapértelmezett javasolt érték pedig 0. Az alkalmazás-példány, amely sérült az alkalmazás számít sérült, és nem sikerül a frissítés előtt adja meg a szolgáltatások maximális számát. |
| MaxPercentUnhealthyPartitionsPerService | Alapértelmezett javasolt érték pedig 0. Adja meg a szolgáltatás, amely sérült előtt számít, hogy a szolgáltatás sérült partíciót maximális száma. |
| MaxPercentUnhealthyReplicasPerPartition | Alapértelmezett javasolt érték pedig 0. Adja meg, amely sérült előtt számít, hogy a partíciót sérült partíciót kópiák maximális száma. |
| UpgradeReplicaSetCheckTimeout | **Állapot nélküli szolgáltatás**– egy frissítési tartományon belül szolgáltatás háló próbálja győződjön meg arról, hogy elérhetők-e a szolgáltatás további előfordulásait. Ha a cél példányok száma több, szolgáltatás háló vár egynél több példányt érhető el, a maximális időkorlátot felfelé. A időtúllépés megadva a UpgradeReplicaSetCheckTimeout tulajdonság segítségével. Ha lejár az időtúllépés, szolgáltatás háló megkezdené a frissítést, függetlenül attól, szolgáltatás példányainak száma. Ha a cél példányok száma, szolgáltatás nem várja meg, és azonnal megkezdené a frissítést. **Állapot-nyilvántartó szolgáltatás**– egy frissítési tartományon belül szolgáltatás háló próbálja gondoskodjon arról, hogy a kópiakészlet határozatképesség. Szolgáltatás háló elérhetővé teheti, akár egy maximális időkorlátját (a UpgradeReplicaSetCheckTimeout tulajdonságban meghatározott) határozatképesség várakozik. Ha lejár az időtúllépés, szolgáltatás háló megkezdené a frissítést, függetlenül attól, kvórum. Ez a beállítás nem, akiket soha nem (végtelen) közbeni előre és a 900 másodperc amikor visszaállítása. |
| ForceRestart | Ha frissíti a konfigurációs vagy adatok csomag a szolgáltatáskód frissítése nélkül, a szolgáltatás újraindítása csak akkor, ha a ForceRestart tulajdonság értéke igaz. Amikor befejeződött a frissítést, a szolgáltatás háló értesíti a szolgáltatást, hogy egy új konfigurációs csomagot vagy adatok csomagban érhető el. A szolgáltatás a felelős a módosítások alkalmazására. Ha szükséges, a szolgáltatás is újraindul. |



<br>
<br>
A MaxPercentUnhealthyServices MaxPercentUnhealthyPartitionsPerService és MaxPercentUnhealthyReplicasPerPartition feltétel egy alkalmazás példány szolgáltatás típusonként adható meg. Következő paraméterek per-szolgáltatás beállítása lehetővé teszi, hogy az alkalmazás különböző szolgáltatásai típusai eltérő kiértékelés házirendek tartalmaz. Egy átjáró állapot nélküli szolgáltatás típusa például egy MaxPercentUnhealthyPartitionsPerService, amely az adott alkalmazás példány egy állapot-nyilvántartó motor szolgáltatás típusa különbözik van.

## <a name="next-steps"></a>Következő lépések

[Az alkalmazás használatáról a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti az alkalmazás frissítésre Visual Studio segítségével.

[Az alkalmazás Powershell használatá frissítés](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti az alkalmazás frissítésre PowerShell használatával.

Úgy, hogy az alkalmazás frissítések kompatibilis tanulási [Adatszerializáció](service-fabric-application-upgrade-data-serialization.md)használatáról.

Megtudhatja, hogy miként használja a speciális funkcióit, az alkalmazás [Speciális témakörökre](service-fabric-application-upgrade-advanced.md)mutató hivatkozással frissítésekor.

Gyakori problémák megoldása az alkalmazás frissítések hivatkozva [Alkalmazás frissítése – hibaelhárítás](service-fabric-application-upgrade-troubleshooting.md)című témakör lépéseit.
 
