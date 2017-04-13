<properties
   pageTitle="A PowerShell használatával szolgáltatás háló alkalmazás frissítése |} Microsoft Azure"
   description="A szolgáltatás háló-alkalmazások telepítése, a kód megváltoztatása és a PowerShell használatá frissítésének helyezése a felület az alábbiakban ismertetjük."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>A PowerShell használatával szolgáltatás háló alkalmazás frissítése

> [AZURE.SELECTOR]
- [A PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

A leggyakrabban használt, és ajánlott frissítési megközelítés, a felügyelt közbeni frissítés.  Azure Service háló figyeli a frissítendő alkalmazás állapotának állapot házirendek alapján. Az update domain (UD) frissítését, szolgáltatás háló kiértékeli a alkalmazás állapot, és a következő frissítés tartományhoz folyamán vagy attól függően, hogy az állapot házirendek a frissítés sikertelen lesz.

A felügyelt alkalmazás frissítése végezhető el a felügyelt natív API-hoz, PowerShell vagy többi. Visual Studio segítségével frissítés végrehajtása, tanulmányozza [a Visual Studio segítségével az alkalmazás frissítése](service-fabric-application-upgrade-tutorial.md).

Az alkalmazás rendszergazdájának figyeli a szolgáltatás háló közbeni a kiértékelés házirendet, amely annak megállapításához, hogy az alkalmazás megfelelő használ szolgáltatási háló is konfigurálhatja. Ezenkívül a rendszergazda tenni, ha az állapot értékelés (például egy automatikus visszaállítás módon.) nem sikerül a művelet konfigurálható tulajdonsággal Ez a szakasz végigvezeti a PowerShell használó SDK minták egyik egy felügyelt frissítés.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Lépés: 1: Építse fel, és telepítse az Visual objektumok minta


Készítése és közzététele az alkalmazás az alkalmazás projekten, **VisualObjectsApplication,** kattintson a jobb gombbal, és válassza a **Közzététel** parancsot.  További tudnivalókért olvassa el a [szolgáltatás háló alkalmazás frissítése az oktatóprogram](service-fabric-application-upgrade-tutorial.md)című témakört.  Másik lehetőségként PowerShell is használhatja az alkalmazás telepítéséhez.

> [AZURE.NOTE] Mielőtt PowerShell bármelyik szolgáltatás háló parancsot is felhasználhatók, először kell segítségével csatlakozzon a fürt a `Connect-ServiceFabricCluster` parancsmag. Hasonlóképpen feltételezett értéke, hogy a fürt már be van állítva a helyi számítógépre. A témakör [a szolgáltatás háló fejlesztői környezet beállításához](service-fabric-get-started.md).

Létrehozása a projekt a Visual Studióban, után paranccsal a PowerShell **Másolása-ServiceFabricApplicationPackage** alkalmazáscsomag másolása a ImageStore. A következő lépésként regisztrálni a szolgáltatás háló futtatókörnyezet a **Register-ServiceFabricApplicationPackage** parancsmaggal az alkalmazást. Az utolsó lépésként egy példánya az alkalmazás indítása a **New-ServiceFabricApplication** parancsmag használatával.  Ezeket a lépéseket a **központi telepítés** menüpont használata a Visual Studio papírlapnak megfelelő.

Most [Szolgáltatás háló Explorer tekintheti meg a fürt és az alkalmazás](service-fabric-visualizing-your-cluster.md)is használhatja. Az alkalmazás, amely lehet használni szeretné az Internet Explorer a címsorban [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) beírásával webszolgáltatás rendelkezik.  Meg kell jelennie néhány lebegő vizuális objektumok beszúrási pont áthelyezése a képernyőn.  Emellett az alkalmazás állapotának ellenőrzéséhez **Get-ServiceFabricApplication** is használhatja.

## <a name="step-2-update-the-visual-objects-sample"></a>Lépés: 2: A vizuális objektumok minta frissítése

Megfigyelheti, hogy az 1 telepített verziójával vizuális objektumok forgassa el. Ez az alkalmazás egy adott vizuális objektumok is elforgatása vegyük frissíteni.

Jelölje ki a VisualObjects megoldást VisualObjects.ActorService projektnek, és nyissa meg a StatefulVisualObjectActor.cs fájlt. Belül a fájlt, nyissa meg azt a módot `MoveObject`, ki megjegyzést `this.State.Move()`, és vegye ki a megjegyzésjeleket `this.State.Move(true)`. Ez a változás az objektumok elforgatása, a szolgáltatás frissítése után.

Azt is frissítenie kell a *ServiceManifest.xml* fájlt (a PackageRoot) a projekt **VisualObjects.ActorService**. Frissítse a *CodePackage* és a szolgáltatás verzió 2.0-s és a megfelelő sorokat a *ServiceManifest.xml* fájlban.
A Visual Studio *Cikkét fájlok szerkesztése* parancs használata után a, kattintson a jobb gombbal a nyilvánvalóan fájl módosításához a megoldást is.


Után a változtatások a a jegyzék az alábbiakhoz hasonlóan kell kinéznie (a kiemelt részeket módosítások megjelenítése):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Most már a *ApplicationManifest.xml* fájlt (a **VisualObjects** projekt csoportban a **VisualObjects** megoldást csoportban található) 2.0-s verziója a **VisualObjects.ActorService** projekt frissül. Ezeken kívül az alkalmazás verziójával frissül 2.0.0.0 a 1.0.0.0. A *ApplicationManifest.xml* a következő kódtöredékének hasonlóan kell kinéznie:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Most hozza létre a project úgy, hogy csak a **ActorService** projekt kijelölése, majd kattintson a jobb gombbal és a Visual Studio **Build** parancs kiválasztása. Ha bejelöli **az összes újjáépítése**, frissítenie kell az összes projekt esetében, a verziók óta a kódot szeretne megváltozott. A következő, a frissített alkalmazás ***VisualObjectsApplication***a jobb gombbal, válassza a szolgáltatás háló menüre, és válassza a **csomag**vegyük telepítőcsomagot. Ez a művelet létrehoz egy alkalmazáscsomag, hogy telepíthető.  A frissített alkalmazás áll készen áll arra, hogy telepíthető.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Lépés 3: Állapot házirendek használata mellett dönt, és a paraméterek frissítése

Ismerkedjen meg az [alkalmazás frissítése paramétereket](service-fabric-application-upgrade-parameters.md) és a [frissítési folyamat](service-fabric-application-upgrade.md) egy jól érthető a különböző frissítési paraméterek adatokon való műveletvégzés és alkalmazott állapot feltétel kéréséhez. Ez a forgatókönyv a szolgáltatás állapota kiértékelés feltétel van az alapértelmezett (és ajánlott) értékek, ami azt jelenti, hogy szolgáltatásokat és a példányok kell _megfelelő_ a frissítés után.  

Azonban nézzük növeli a *HealthCheckStableDuration* 60 másodpercre (így a szolgáltatások megfelelő előtt a frissítés során a következő frissítés tartományhoz legalább 20 másodpercig).  Lássunk is beállíthatja a *UpgradeDomainTimeout* 1200 másodperc lesz, és a *UpgradeTimeout* 3000 másodperc kell.

Végezetül lássunk is beállíthatja a *UpgradeFailureAction* visszaállítása. Ehhez a lehetőséghez szolgáltatás háló visszaállítása az előző verzióhoz képest az alkalmazást, ha a frissítés során találkozik kapcsolatos problémák megoldásához. Ezért a frissítés (a 4) indításakor az alábbi paramétereknek adható:

FailureAction = visszaállítása

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Lépés: 4: Felkészülés a frissítésre alkalmazás

Az alkalmazás letöltése beépített, és készen áll a verziófrissítésre áll. Ha megnyit egy PowerShell-ablak be rendszergazdaként, és írja be a **Get-ServiceFabricApplication**, hagyja azt jelzi, hogy az alkalmazás típusú 1.0.0.0 **VisualObjects** telepítve van.  

Alkalmazáscsomag csoportban a következő relatív elérési utat, ha Ön nem tömörített a szolgáltatás háló SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*vannak tárolva. Keresse meg a "Csomag" mappa az adott directory alkalmazáscsomag tároló. Jelölje be a időbélyegeket annak érdekében, hogy-e a legújabb verziójában (Előfordulhat, hogy módosítania az elérési út megfelelően is).

Most vegyük másolása a frissített alkalmazáscsomag a szolgáltatás háló ImageStore (alkalmazáscsomagok tároló szolgáltatás háló szerint). A paraméter *ApplicationPackagePathInImageStore* arról tájékoztat, hogy hol azt meg az alkalmazáscsomag szolgáltatás háló. Azt a frissített alkalmazás illesztette be "VisualObjects\_V2" (szükség lehet újra megfelelően módosítsa az elérési út), az alábbi paranccsal.

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

A következő lépésként az alkalmazás regisztrálása szolgáltatás háló, amely végezhető el az alábbi parancsot:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Ha az előző parancs nem sikerül, akkor valószínű, hogy szüksége van-e a szolgáltatások újra kell építenie. Említett a 2, akkor előfordulhat, hogy webszolgáltatás verziójától, valamint frissítése.

## <a name="step-5-start-the-application-upgrade"></a>5 lépés: Az alkalmazás frissítés megkezdése

Most hogy készen áll a alkalmazás frissítés megkezdése a következő parancs használatával:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Az alkalmazás neve megegyezik a *ApplicationManifest.xml* fájlt meg lett leírt módon. Szolgáltatás háló használja ezt a nevet azonosíthatja, hogy melyik alkalmazás első frissítését. Ha az adatokon való műveletvégzés túl rövid, előfordulhatnak egy hibaüzenet arról, hogy a problémát. A hibaelhárítási szakaszban, vagy az adatokon való műveletvégzés növeléséhez.

Most az alkalmazás frissítési bevétel, mint hírcsatornának szolgáltatás háló Intézővel, vagy az alábbi PowerShell használatával parancs: **Get-ServiceFabricApplicationUpgrade háló: / VisualObjects**.

Néhány perc alatt az állapotot, szerezte be az előző PowerShell-parancs használatával tartalmaznia kell, hogy az összes frissítése tartományok frissítették (befejeződött). És a böngésző ablakában a vizuális objektumok elforgatása van lépések keresse meg!

Is próbálkozhat a 2-es verzióra gyakorlat 1 vagy 2-es verziójú 3-as verzióra történő frissítés. Áttérés a 2-es verziójú 1-es verziójú is figyelembe veszi frissítést. Az adatokon való műveletvégzés és állapot házirendek, hogy saját maga őket ismert lejátszása. Ha egy Azure fürthöz telepít, a paraméterek meg kell adnia megfelelően. Célszerű az adatokon való műveletvégzés amelyekről beállításához.


## <a name="next-steps"></a>Következő lépések

[Frissítés a Visual Studio segítségével az alkalmazás](service-fabric-application-upgrade-tutorial.md) végigvezeti az alkalmazás frissítésre Visual Studio segítségével.

Szabályozhatja, hogy hogyan az alkalmazás a [frissítési paraméterek](service-fabric-application-upgrade-parameters.md)használatával frissíti.

Úgy, hogy az alkalmazás frissítések kompatibilis tanulási [adatszerializáció](service-fabric-application-upgrade-data-serialization.md)használatáról.

Megtudhatja, hogy miként használja a speciális funkcióit, az alkalmazás [speciális témakörökre](service-fabric-application-upgrade-advanced.md)mutató hivatkozással frissítésekor.

Gyakori problémák megoldása az alkalmazás frissítések hivatkozva [Hibaelhárítás alkalmazás frissítése](service-fabric-application-upgrade-troubleshooting.md)című témakör lépéseit.
