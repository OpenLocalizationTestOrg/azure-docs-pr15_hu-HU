<properties
   pageTitle="A szolgáltatás háló alkalmazás életciklus |} Microsoft Azure"
   description="Fejlesztése, üzembe helyezése, tesztelés, frissítés, karbantartása és szolgáltatás háló alkalmazások eltávolítása ismerteti."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Szolgáltatás háló alkalmazás életciklus
Előrehaladtával a más platformokhoz készült Azure Service háló alkalmazás általában a következő fázison keresztül: tervezés, fejlesztési, tesztelés, telepítési, frissítés, karbantartási és eltávolítása. Szolgáltatás háló osztályú támogatást nyújt a teljes alkalmazás életciklusáról felhő alkalmazások, a telepítés, napi kezelési és karbantartás fejlesztését esetleges leszerelje. A szolgáltatás modell lehetővé teszi, hogy egymástól függetlenül részt az alkalmazás életciklus számos különféle szerepköröket. Ez a cikk áttekintést nyújt az API-k és az hogyan alkalmazza őket a szolgáltatás háló alkalmazás életciklusáról szakaszainak egész a különféle szerepköröket.

## <a name="service-model-roles"></a>Szolgáltatás modell szerepkörök
A szolgáltatás modell szerepkörök a következők:

- **Szolgáltatás Fejlesztőeszközök**: fejleszt, amely újra halfajon, és a azonos típusú, vagy a különböző típusú több alkalmazásokban használt elemes és általános szolgáltatások. Ha például egy várólista szolgáltatást hozhat létre a Feladatkezelő (segélyszolgálat) vagy az e-kereskedelmi kérelem (bevásárlókosarát) használható.

- **Alkalmazásfejlesztő**: hoz létre alkalmazást integrálja az egyes konkrét követelmények vagy esetek kielégítéséhez szolgáltatások gyűjteménye. Például az e-kereskedelmi webhely előfordulhat, hogy integrálni a "JSON állapot nélküli előfeldolgozó szolgáltatás," "árverés megbízható, szűrőként működő" és "Várólista megbízható, szűrőként működő szolgáltatás" össze egy auctioning megoldás.

- **Alkalmazás-rendszergazda**: a (konfigurációs sablon paraméterek kitöltése) alkalmazás konfigurációja, a telepítési (hozzárendelés-a rendelkezésre álló erőforrásokat) és a szolgáltatások minőségének döntéseket. Ha például alkalmazás rendszergazda úgy dönt, hogy a nyelv (az Amerikai Egyesült Államok-angol nyelven) vagy a japán-japán, például az alkalmazás. A különböző, telepített alkalmazások beállíthatja, hogy különböző beállításokat.

- **Operátor**: az alkalmazás konfigurációjának alkalmazások és az alkalmazás-rendszergazda által megadott követelmények üzembe helyezése. Operátor például rendelkezések és üzembe helyezése az alkalmazást, és ellenőrzi, hogy fut Azure-ban. Operátorok figyelheti a alkalmazás állapot és a teljesítmény adatai, és a fizikai infrastruktúrát karbantartása, szükség szerint.


## <a name="develop"></a>Kidolgozása
1. A *szolgáltatás Fejlesztőeszközök* fejleszt különböző szolgáltatások [Megbízható szereplők](service-fabric-reliable-actors-introduction.md) vagy [Megbízható szolgáltatások](service-fabric-reliable-services-introduction.md) programozási modellt használja.
2. A *szolgáltatás Fejlesztőeszközök* deklaratív ismerteti a fejlett szolgáltatás egy vagy több kódot, beállításainak és adatok csomagból álló szolgáltatás nyilvánvalóan fájlban.
3. Az *alkalmazás fejlesztője* kattintson a másik szolgáltatás típusú alkalmazás hoz létre.
4. Az *alkalmazás fejlesztője* által a szolgáltatás jegyzékfájlok alkotó szolgáltatásokat és megfelelően felülbírálása és konfigurációs és telepítési beállítások alkotó szolgáltatások paraméterezése hivatkozó deklaratív-alkalmazás jegyzék alkalmazás típus mutatja be.

[Első lépések a megbízható szereplők](service-fabric-reliable-actors-get-started.md) , és [megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md) talál példákat.

## <a name="deploy"></a>Terjesztése
1. *Alkalmazás rendszergazdájának* igazítja az egy adott alkalmazást, hogy a megfelelő paramétereket **ApplicationType** elem az alkalmazás jegyzék megadásával szolgáltatás háló fürtre telepíthető az alkalmazás típusa igényekhez.

2. *Operátor* [ **CopyApplicationPackage** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) vagy a [ **Másolás-ServiceFabricApplicationPackage** parancsmag](https://msdn.microsoft.com/library/azure/mt125905.aspx)használatával alkalmazáscsomag feltölti a fürt kép áruházból. Alkalmazáscsomag tartalmazza, az alkalmazás jegyzék és szolgáltatás csomagok gyűjteménye. Háló szolgáltatás üzembe helyezése a kép tárolja, ez lehet egy Azure blob-tárolóban vagy a szolgáltatás háló rendszer szolgáltatás alkalmazáscsomag alkalmazásokat.

3. Az *operátorok* majd kiépítése a cél fürt [ **ProvisionApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), a [ **Register-ServiceFabricApplicationType** parancsmag](https://msdn.microsoft.com/library/azure/mt125958.aspx)vagy a [többi **rendelkezni az alkalmazások** művelet](https://msdn.microsoft.com/library/azure/dn707672.aspx)feltöltött alkalmazáscsomag az alkalmazás típusát.

4. Az alkalmazás-kiépítése után *operátor* elindítja az alkalmazást a [ **CreateApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), a [ **New-ServiceFabricApplication** parancsmag](https://msdn.microsoft.com/library/azure/mt125913.aspx)vagy a [többi **Alkalmazás létrehozása** művelet](https://msdn.microsoft.com/library/azure/dn707676.aspx) *alkalmazás rendszergazda* által megadott paramétereket.

5. Az alkalmazás telepítését követően *operátor* [ **CreateServiceAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), a [ **New-ServiceFabricService** parancsmag](https://msdn.microsoft.com/library/azure/mt125874.aspx)vagy használja a [többi **Szolgáltatás hozzon létre** művelet](https://msdn.microsoft.com/library/azure/dn707657.aspx) hozhat létre új szolgáltatás példányok elérhető szolgáltatások alapuló alkalmazásához.

6. Az alkalmazás letöltése fut a szolgáltatás háló fürt.

[Alkalmazás Deploy](service-fabric-deploy-remove-applications.md) talál példákat.

## <a name="test"></a>Teszt
1. A helyi fejlesztési fürt vagy próba fürtre bevezetéshez *szolgáltatás Fejlesztőeszközök* futása után a beépített feladatátvevő próba forgatókönyvet az [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) és [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) osztályoknak, vagy a [ **Invoke-ServiceFabricFailoverTestScenario** parancsmag](https://msdn.microsoft.com/library/azure/mt644783.aspx)használatával. A feladatátvevő tesztelési forgatókönyv megadott szolgáltatás fut, fontos áttűnések és annak érdekében, hogy az továbbra is elérhető és munka feladatátadás keresztül.

2. A *szolgáltatás Fejlesztőeszközök* majd fut, a beépített chaos próba forgatókönyvet az [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) és [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) osztályoknak, illetve a [ **Invoke-ServiceFabricChaosTestScenario** parancsmag](https://msdn.microsoft.com/library/azure/mt644774.aspx)használatával. A chaos próba forgatókönyv kapott több csomópontot, a kód csomag és a replika hibák véletlenszerűen a fürt be.

3. A *szolgáltatás Fejlesztőeszközök* [vizsgálatok szolgáltatás kapcsolati](service-fabric-testability-scenarios-service-communication.md) szerzői próba esetek, amelyek körül a fürt elsődleges kópiák áthelyezése alapján.

[A hibafa-elemzési szolgáltatás bemutatása](service-fabric-testability-overview.md) témakörben további információt.

## <a name="upgrade"></a>Frissítés
1. A *szolgáltatás Fejlesztőeszközök* frissíti a létrehozott alkalmazás alkotó helyezése és/vagy megoldja a hibák, és biztosít a szolgáltatás jegyzék új verziója.

2. Az *alkalmazás fejlesztője* felülírja parameterizes egységes szolgáltatások konfigurálása és a telepítési beállítások, és az alkalmazás jegyzék új verzióját. Az alkalmazás fejlesztője ezzel a paranccsal beépíti a szolgáltatás jegyzékfájlok új verziója az alkalmazásba, és egy frissített alkalmazáscsomag alkalmazás típusú új verziót biztosít.

3. *Alkalmazás rendszergazdájának* a cél alkalmazásba az új verzió, az alkalmazás típusú megfelelő a megfelelő paramétereket frissítésével.

5. *Operátor* feltölti a frissített alkalmazáscsomag a fürt kép tároló [ **CopyApplicationPackage** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) vagy a [ **Másolás-ServiceFabricApplicationPackage** parancsmag](https://msdn.microsoft.com/library/azure/mt125905.aspx)használatával. Alkalmazáscsomag tartalmazza, az alkalmazás jegyzék és szolgáltatás csomagok gyűjteménye.

6. *Operátor* kiépítése a cél fürt az alkalmazás új verziója, a [ **ProvisionApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), a [ **Register-ServiceFabricApplicationType** parancsmag](https://msdn.microsoft.com/library/azure/mt125958.aspx)vagy a [többi **rendelkezni az alkalmazások** művelet](https://msdn.microsoft.com/library/azure/dn707672.aspx)használatával.

7. *Operátor* frissíti a célalkalmazás az új verzió [ **UpgradeApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), a [ **Kezdő-ServiceFabricApplicationUpgrade** parancsmag](https://msdn.microsoft.com/library/azure/mt125975.aspx)vagy a [többi **alkalmazás frissítése** művelet](https://msdn.microsoft.com/library/azure/dn707633.aspx).

8. *Operátor* ellenőrzi a [ **GetApplicationUpgradeProgressAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), a [ **Get-ServiceFabricApplicationUpgrade** parancsmag](https://msdn.microsoft.com/library/azure/mt125988.aspx)vagy az [ **Alkalmazás frissítése a haladás első** többi művelet](https://msdn.microsoft.com/library/azure/dn707631.aspx)frissítés végrehajtásának.

9. Ha szükséges, a az *operátor* módosítja, és újra alkalmazza a határozza meg a jelenlegi alkalmazás frissítés [ **UpdateApplicationUpgradeAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), a [ **Frissítés-ServiceFabricApplicationUpgrade** parancsmag](https://msdn.microsoft.com/library/azure/mt126030.aspx)vagy a [többi **Frissítés alkalmazás frissítése** művelet](https://msdn.microsoft.com/library/azure/mt628489.aspx).

10. Ha szükséges, az *operátort* visszaállítja a jelenlegi alkalmazás frissítés [ **RollbackApplicationUpgradeAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), a [ **Kezdő-ServiceFabricApplicationRollback** parancsmag](https://msdn.microsoft.com/library/azure/mt125833.aspx)vagy a [többi **Visszaállítás alkalmazás frissítése** művelet](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Szolgáltatás háló frissíti a célalkalmazás fut a fürt alkotó szolgáltatásai közül a elérhetőségét megtartásával.

Lásd: az [alkalmazás, frissítse az oktatóprogram](service-fabric-application-upgrade-tutorial.md) példákat.

## <a name="maintain"></a>Karbantartása
1. Az operációs rendszer frissítése és a javítások szolgáltatás háló felületek, az összes a fürt futó alkalmazás elérhetősége zökkenőmentes az Azure infrastruktúra.

2. Frissítések és a szolgáltatás háló platformra javítások szolgáltatás háló frissíti magát az alkalmazások, a a fürthöz elérhetősége megtartásával.

3. *Alkalmazás rendszergazdájának* a jóváhagyása hozzáadása vagy eltávolítása a fürt csomópontok korábbi kapacitás kihasználtsági adatok és a tervezett jövőbeli igény szerinti elemzése után.

4. *Operátor* ad, és az *alkalmazás-rendszergazda*által megadott csomópontok eltávolítja.

5. Amikor új csomópontok kerülnek, vagy meglévő csomópontok törlődjenek a fürt, szolgáltatás háló automatikusan betöltés-balances futó alkalmazást a fürt optimális teljesítmény eléréséhez minden csomópontra.

## <a name="remove"></a>Eltávolítása
1. *Operátor* törölheti a fürt futó szolgáltatás példányára [ **DeleteServiceAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), az [ **Eltávolítás-ServiceFabricService** parancsmag](https://msdn.microsoft.com/library/azure/mt126033.aspx)vagy a [többi **Szolgáltatás törlése** a művelet](https://msdn.microsoft.com/library/azure/dn707687.aspx)a teljes alkalmazás törlése nélkül.  

2. *Operátor* is törölheti a példány alkalmazás és a [ **DeleteApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), az [ **Eltávolítás-ServiceFabricApplication** parancsmag](https://msdn.microsoft.com/library/azure/mt125914.aspx)vagy az [ **Alkalmazás törlése** többi művelet](https://msdn.microsoft.com/library/azure/dn707651.aspx)szolgáltatásai.

3. Az alkalmazások és szolgáltatások leállt, miután az az *operátor* is leépítése [ **UnprovisionApplicationAsync** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), a [ **Unregister-ServiceFabricApplicationType** parancsmag](https://msdn.microsoft.com/library/azure/mt125885.aspx)vagy az [ **alkalmazás leépíteni** többi művelet](https://msdn.microsoft.com/library/azure/dn707671.aspx)típusának alkalmazásban. Az alkalmazás típusa leépítése nem távolítja el a alkalmazáscsomag a ImageStore. Alkalmazáscsomag manuálisan el kell távolítania.

4. *Operátor* alkalmazáscsomag eltávolítja a ImageStore [ **RemoveApplicationPackage** módszer](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) vagy az [ **Eltávolítás-ServiceFabricApplicationPackage** parancsmag](https://msdn.microsoft.com/library/azure/mt163532.aspx)használatával.

[Alkalmazás Deploy](service-fabric-deploy-remove-applications.md) talál példákat.

## <a name="next-steps"></a>Következő lépések

További tájékoztatást a fejlesztése tesztelés, és kezelni a szolgáltatás háló alkalmazások és szolgáltatások talál:

- [Megbízható szereplők](service-fabric-reliable-actors-introduction.md)
- [Megbízható szolgáltatások](service-fabric-reliable-services-introduction.md)
- [Alkalmazások telepítése](service-fabric-deploy-remove-applications.md)
- [Alkalmazás frissítése](service-fabric-application-upgrade.md)
- [Tesztelhetőségi – áttekintés](service-fabric-testability-overview.md)
- [TÖBBI-alapú alkalmazás életciklus-minta](service-fabric-rest-based-application-lifecycle-sample.md)
