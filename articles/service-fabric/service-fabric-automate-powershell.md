<properties
    pageTitle="Alkalmazáskezelési szolgáltatás háló automatizálhatja a PowerShell használatával |} Microsoft Azure"
    description="Telepítése, frissítése, tesztelése és az alkalmazások szolgáltatás háló eltávolítása a PowerShell használatával."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>A PowerShell használatá alkalmazás életciklusáról automatizálása

A [szolgáltatás háló alkalmazás életciklus](service-fabric-application-lifecycle.md) számos tulajdonságát automatizálható.  Ez a cikk bemutatja, hogyan telepítése, frissítése, eltávolítása és Azure Service háló alkalmazások tesztelése a gyakori feladatok automatizálása a PowerShell használatával.  Felügyelt, és az alkalmazás management HTTP API-khoz is elérhetők. Lásd: [alkalmazás életciklus](service-fabric-application-lifecycle.md) további információt.  

## <a name="prerequisites"></a>Előfeltételek
Mielőtt viszi a cikk a feladatokat, ne felejtse el:

+ Megismerkedhet a szolgáltatás háló fogalmak ismertetett [szolgáltatás háló technikai áttekintése](service-fabric-technical-overview.md).
+ [Telepítse a futtatókörnyezet, SDK, és eszközök](service-fabric-get-started.md), amelyen telepíti a **ServiceFabric** PowerShell-modult.
+ [Engedélyezése PowerShell parancsprogramot végrehajtása](service-fabric-get-started.md#enable-powershell-script-execution).
+ Indítsa el a helyi fürtre.  Indítsa el a számítógépre rendszergazdaként PowerShell új ablakban, és futtassa a fürt telepítési parancsfájlt a SDK mappából:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Ebben a cikkben futtatásához bármely PowerShell-parancsait, először a helyi szolgáltatás háló fürthöz csatlakozni [**Csatlakozás-ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx)használatával:`Connect-ServiceFabricCluster localhost:19000`
+ Az alábbi feladatokat kell egy v1 alkalmazás telepítéséhez és egy v2 alkalmazás csomagja frissítésre. Töltse le a [ **WordCount** minta alkalmazás](http://aka.ms/servicefabricsamples) (az első lépések minta). Építse fel, és az alkalmazás a Visual Studióban (kattintson a jobb gombbal a **WordCount** a megoldás Intézőben, valamint a select **csomag**) csomag. Másolja a v1 csomagra `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` való `C:\Temp\WordCount`. Másolás `C:\Temp\WordCount` való `C:\Temp\WordCountV2`, frissítés v2 alkalmazáscsomag létrehozásával. Nyissa meg `C:\Temp\WordCountV2\ApplicationManifest.xml` szövegszerkesztőben. Módosítsa a **ApplicationTypeVersion** attribútum "1.0.0" "2.0.0" alkalmazás verziószámát frissítése az **ApplicationManifest** elemet. Mentse a módosított ApplicationManifest.xml fájlt.

## <a name="task-deploy-a-service-fabric-application"></a>Feladat: A szolgáltatás háló alkalmazások telepítése

Már beépített és az alkalmazás csomagolt (vagy után alkalmazáscsomag letöltött), telepítheti az alkalmazást a helyi szolgáltatás háló fürtre. Bevezetésének tényezői alkalmazáscsomag feltöltése regisztrálása az alkalmazás típusától és az alkalmazás-példány létrehozása. Ebben a szakaszban a képernyőn megjelenő utasításokat segítségével fürtre egy új alkalmazás telepítéséhez.

### <a name="step-1-upload-the-application-package"></a>Lépés: 1: Alkalmazáscsomag feltöltése
Alkalmazáscsomag feltöltése a kép tároló helyezi a helyen belső szolgáltatás háló összetevők érhető el.  Alkalmazáscsomag alkalmazás jegyzék szolgáltatás jegyzékfájlok és kódot, konfigurálását, és az alkalmazás és a szolgáltatás példányainak létrehozása adatok csomagok tartalmazza.  A [**Másolás-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) parancs feltölti a csomagot. Példa:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Lépés: 2: Regisztráljon az alkalmazás típusa
Az alkalmazás típusa és a használatra a rendelkezésre álló alkalmazás jegyzék deklarált verzió alkalmazáscsomag regisztrálása teszi. A rendszer beolvassa a csomagban, a következő lépés: 1 a feltöltött, ellenőrizze a csomag (egyenértékű a [**Próba-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) helyben fut), a csomag tartalma folyamat és a feldolgozott csomag egy belső rendszer helyre másolja.  Futtassa a [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) parancsmagot:

```powershell
Register-ServiceFabricApplicationType WordCount
```
A fürt regisztrált alkalmazás típusok megtekintéséhez futtassa a [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) parancsmagot:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>3 lépés: Az alkalmazás-példány létrehozása
Az alkalmazás verziójával bármely alkalmazás típusú regisztrált a [**New-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) parancs használatával sikeresen lehet létrehozni. A minden alkalmazás nevét a deklarálva, üzembe idő és kell kezdődnie a **háló:** rendszer, és minden alkalmazás példány egyedi lehet. Az alkalmazás írja be a nevet és az alkalmazás típusú verziója az alkalmazáscsomag **ApplicationManifest.xml** fájlban vannak deklarálva. Ha bármelyik alapértelmezett szolgáltatások a célalkalmazás-típus, az alkalmazás jegyzék lettek megadva, akkor azokat jönnek létre adott időben.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

A [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) parancs a összes szolgáltatásalkalmazás-példányok készített sikeresen, a teljes állapotát együtt jeleníti meg. A [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) parancs megjeleníti a szolgáltatás példányainak sikeresen létrehozott egy adott alkalmazás példányt. Alapértelmezett szolgáltatások (ha van ilyen) felsorolása.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Feladat: Szolgáltatás háló alkalmazás frissítése
Frissíthet egy frissített alkalmazáscsomag egy korábban telepített szolgáltatást háló alkalmazást. Ebben a feladatban frissíti a telepített WordCount alkalmazása "tevékenység: a szolgáltatás háló alkalmazás telepítéséhez." Olvassa el a [szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md) további információt.

Ahhoz, hogy ez a példa az egyszerű dolgot, csak az alkalmazás verziószáma frissült a Előfeltételek létrehozott WordCountV2 alkalmazás csomagban. További reálisan példa a szolgáltatás kódot, konfigurációs vagy adatok fájlok frissítése és újbóli létrehozása és az alkalmazás a frissített verziót számokkal csomagolása volna járnak.  

### <a name="step-1-upload-the-updated-application-package"></a>Lépés: 1: A frissített alkalmazáscsomag feltöltése
Az WordCount v1 alkalmazás áll készen áll a verziófrissítésre. Be rendszergazdaként, és írja be a [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)egy PowerShell-ablak megnyitása esetén megjelenik az alkalmazás típusú WordCount verzióval 1.0.0 telepítve van.  

A frissített alkalmazáscsomag most másolása a szolgáltatás háló kép áruházból (alkalmazáscsomagok tároló szolgáltatás háló szerint). A paraméter **ApplicationPackagePathInImageStore** arról tájékoztat, hogy hol azt meg az alkalmazáscsomag szolgáltatás háló. A következő parancsot a alkalmazáscsomag másolja **WordCountV2** a kép tárolóban lévő:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Lépés: 2: Regisztrálhatja a frissített alkalmazás típusa
A következő lépésként az alkalmazás új verziójának regisztrálása szolgáltatás háló, amely lehet végrehajtani a [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) parancsmag használatával:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>3 lépés: A frissítés megkezdése
Alkalmazás frissítések különböző frissítési paraméterek időtúllépései és állapot feltétel alkalmazható. Olvassa el a további [alkalmazások frissítési paraméterek](service-fabric-application-upgrade-parameters.md) és a [frissítési folyamat](service-fabric-application-upgrade.md) dokumentumokat. Szolgáltatások és a példányok kell _megfelelő_ a frissítés után.  Állítsa a **HealthCheckStableDuration** 60 másodperc (így a szolgáltatások megfelelő előtt a frissítés során a következő frissítés tartományhoz legalább 20 másodpercig).  Is beállíthatja a **UpgradeDomainTimeout** 1200 másodpercek és a **UpgradeTimeout** 3000 másodpercre. Végül állítsa a **UpgradeFailureAction** **Visszaállítás**, amely kéri, hogy szolgáltatás háló visszaállítja az előző verzióhoz képest az alkalmazást, ha a frissítés során felmerülő hibákat.

A [**Kezdés-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) parancsmaggal elkezdheti az alkalmazás frissítése:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Ne feledje, hogy az alkalmazás neve megegyezik a korábban telepített v1.0.0 alkalmazás nevét (háló: / WordCount). Szolgáltatás háló használja ezt a nevet azonosíthatja, hogy melyik alkalmazás első frissítését. Ha az adatokon való műveletvégzés túl rövid, előforduló időtúllépése hiba üzenet arról, hogy a problémát. Olvassa el a [Hibaelhárítás alkalmazás frissítésekhez](service-fabric-application-upgrade-troubleshooting.md), vagy az adatokon való műveletvégzés növelje.

### <a name="step-4-check-upgrade-progress"></a>Lépés: 4: Jelölőnégyzet frissítési folyamat
Figyelheti a frissítési folyamat alkalmazás [szolgáltatás háló](service-fabric-visualizing-your-cluster.md)Explorerrel, illetve a [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) parancsmag használatával:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

A néhány perc alatt a [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) parancsmag nem jeleníti meg, hogy az összes frissítési tartományok frissítették (befejeződött).

## <a name="task-test-a-service-fabric-application"></a>Feladat: A szolgáltatás háló alkalmazás tesztelése

Kiváló minőségű szolgáltatások elkészítéséhez fejlesztőknek engedélyezni szeretné kipróbálni a szolgáltatást a stabilitás mellékletei nem megbízható infrastruktúra hibák jutva szükségük van. Szolgáltatás háló fejlesztők jutva hibafa műveletek és szolgáltatások jelenlétében hibák tesztelése a chaos és feladatátvételi Tesztelési forgatókönyvek használatával adja vissza.  Olvassa el a [tesztelhetőségi áttekintése](service-fabric-testability-overview.md) további információt.

### <a name="step-1-run-the-chaos-test-scenario"></a>Lépés: 1: Futtassa a chaos próba eset
A chaos próba forgatókönyv hibák között a teljes szolgáltatás háló fürt hoz létre. Az alkalmazási példát tömöríti a hibák, hónapok vagy évek néhány órát általában látható. Magas hibafa sebességének időosztásos hibák kombinációi, ellenkező esetben kihagyott sarok esetek találja meg. Az alábbi példa a chaos próba forgatókönyv 60 percig fut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Lépés: 2: Futtassa a feladatátvevő próba eset
A feladatátvételi tesztelése forgatókönyv célok egy adott szolgáltatás partíciót helyett a teljes fürt és az egyéb szolgáltatások változnak elhagyja. Szimulált szolgáltatás érvényesítési hibáinak keresztül kiszámítására az alkalmazási példát, az üzleti logikai funkcióinak futása közben. A szolgáltatás adatérvényesítési hiba igénylő további vizsgálat hibát jelez. A feladatátvevő vizsgálat kapott az csak egy hiba egyszerre, nem pedig a próba is jutva több hibák chaos eset. Az alábbi példa a feladatátvevő vizsgálat 60 percig fut a háló: / WordCount/WordCountService szolgáltatást.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Feladat: Szolgáltatás háló alkalmazás eltávolítása
A telepített alkalmazások egy példányának törlése, a kiépített alkalmazás típusa eltávolítása a fürt, és az alkalmazás-csomag eltávolítása a ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Lépés: 1: Távolítsa el az alkalmazás
Alkalmazás-példány már nincs szükség, ha azt véglegesen távolíthatja el az [**Eltávolítás-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) parancsmag használatával. Ezzel is automatikusan törli az alkalmazás véglegesen eltávolítja az összes szolgáltatás állapota tartozó szolgáltatások. Ez a művelet nem vonható vissza, és az alkalmazás állapota sem állíthatók.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Lépés: 2: Az alkalmazás típusa unregister parancsot.
Ha már nincs szüksége egy adott verzióját-alkalmazás típusa, unregister [**Unregister-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) parancsmag használatával. A kép áruházban alkalmazáscsomag által használt tárterület még nem használt típusok regisztrációjának elengedi. Az alkalmazás típusa mindaddig, amíg nincsenek szemben, vagy függőben hivatkozik alkalmazás frissítések megjelenítésre alkalmazások nem regisztrált lehet.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>3 lépés: Az alkalmazás-csomag eltávolítása
Az alkalmazás típusa nem regisztrált, miután a kép tároló [**Eltávolítása-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) parancsmag használatával alkalmazáscsomag eltávolítható.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Következő lépések
[Szolgáltatás háló alkalmazás életciklus](service-fabric-application-lifecycle.md)

[Alkalmazások telepítése](service-fabric-deploy-remove-applications.md)

[Alkalmazás frissítése](service-fabric-application-upgrade.md)

[Azure Service háló parancsmagok](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service háló tesztelhetőségi parancsmagok](https://msdn.microsoft.com/library/azure/mt125844.aspx)
