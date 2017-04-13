<properties
   pageTitle="Első lépések a üzembe helyezése, és a helyi fürtre alkalmazások frissítése |} Microsoft Azure"
   description="A helyi szolgáltatás háló fürtre beállítása, azt meglévő alkalmazások telepítése, és frissítse az alkalmazást."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Első lépések a üzembe helyezése, és a helyi fürtre alkalmazások frissítése
Az Azure Service háló SDK tartalmaz egy teljes helyi fejlesztői környezet, amelyek segítségével gyorsan első lépések a üzembe helyezése, és a helyi fürthöz alkalmazások kezelése. Ebben a cikkben hozzon létre egy helyi fürt, azt a meglévő alkalmazások telepítése, és frissítse az összes, a Windows PowerShell új verzió alkalmazást.

> [AZURE.NOTE] Ez a cikk tartalma feltételezi, hogy Ön már [a fejlesztői környezet beállítása](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Hozzon létre egy helyi fürthöz
A szolgáltatás háló fürtre alkalmazást telepítheti a hardver-erőforrások csoportját képviseli. Általában fürt tevődnek össze tetszőleges 5-gépek sok ezer. A szolgáltatás háló SDK azonban a a fürt konfiguráció egyetlen gépen futó tartalmaz.

Fontos, ha meg szeretné érteni, hogy a szolgáltatás háló helyi fürtre nem jelenik meg egy irányító vagy simulatort használja. Több elem gép fürt megtalálható platform ugyanazt kódot fusson. Az egyetlen különbség, hogy fut-e a platform folyamatok, amely általában helyezkednek egy gépen öt gépek között.

A SDK kétféle módon állíthatja be a helyi fürtre biztosít: a Windows PowerShell-parancsprogramot, és a helyi felülete rendszer tálca alkalmazást. Ebben az oktatóanyagban fogjuk kiszámolni az PowerShell parancsprogramot.

> [AZURE.NOTE] Ha már létrehozott egy helyi fürt üzembe helyezné a Visual Studio alkalmazás által, kihagyhatja ebben a szakaszban.


1. Indítsa el a PowerShell új ablakban rendszergazdaként.

2. Futtassa a fürt telepítési parancsfájlt a SDK mappából:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Fürt beállítása néhány percet vesz igénybe. A telepítés befejezése után kimeneti hasonlóan kell megjelennie:

    ![Fürt beállítási kimeneti][cluster-setup-success]

    Most már készen áll a teendő a fürt alkalmazás telepítése.

## <a name="deploy-an-application"></a>Alkalmazások telepítése
A szolgáltatás háló SDK keretek és szerszámok hozhat létre az alkalmazások fejlesztői széles körű tartalmazza. Ha érdeklik a létrehozásuk alkalmazások a Visual Studióban, olvassa el a [az első szolgáltatás háló Visual Studio-alkalmazás létrehozása](service-fabric-create-your-first-application-in-visual-studio.md)című témakört.

Ebben az oktatóanyagban fogjuk használni egy létező minta alkalmazás (más néven WordCount), hogy azt a platform – például telepítési, ellenőrzése és frissítés management tulajdonságát segítségével koncentrálhat.


1. Indítsa el a PowerShell új ablakban rendszergazdaként.

2. A szolgáltatás háló SDK PowerShell-modult importálni.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Az alkalmazás letöltése és telepítése, például C:\ServiceFabric tárolásához könyvtár létrehozása.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. Létrehozott [Töltse le a WordCount alkalmazás](http://aka.ms/servicefabric-wordcountapp) azt a helyet.  Megjegyzés: a Microsoft Edge böngésző a fájlt menti a *.zip* kiterjesztésű.  Módosítsa a megfelelő fájlkiterjesztés *.sfpkg*.

5. A helyi fürthöz csatlakozhat:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Hozzon létre egy új alkalmazást a SDK telepítő paranccsal nevét és elérési útját a alkalmazáscsomag.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Ha az összes mellé kerül, meg kell jelennie kimeneti, például a következőket:

    ![A helyi fürthöz alkalmazások telepítése][deploy-app-to-local-cluster]

7. Ha látni szeretné az alkalmazás, a művelet, indítsa el a böngészőben, és kattintson az [http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html). Meg kell jelennie:

    ![A telepített alkalmazások felhasználói felület][deployed-app-ui]

    A WordCount alkalmazása rendkívül egyszerű. Ügyféloldali JavaScript-kód létrehozása véletlen öt karakterből álló "szavakat", majd az alkalmazás keresztül ASP.NET webes API közvetítése tartalmazza. Állapot-nyilvántartó szolgáltatás nyomon követi a megszámlált szavak száma. Azok a vannak particionálva a Word az első karakter alapján. Az [első lépések a minták](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/)WordCount alkalmazásban a forráskód talál.

    Az alkalmazás, amely azt rendszerbe négy partíciót tartalmazza. Kezdődő szavakat, és a-tól G az első partíciót tárolja, így Fájlsablonok tárolási helye a második partíciót N keresztül H betűvel kezdődő szavakat, és így tovább.

## <a name="view-application-details-and-status"></a>Alkalmazás részletek és állapota
Most, hogy telepítette az alkalmazást, nézzük meg az app adatait a PowerShellben részét.

1. A lekérdezés az összes telepített alkalmazások a fürt:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Feltételezve, hogy csak telepítette a WordCount alkalmazást, valami hasonló:

    ![A PowerShell az összes telepített alkalmazások lekérdezésére][ps-getsfapp]

2. Nyissa meg a következő szint WordCount alkalmazásban szereplő szolgáltatások csoportját lekérdezésével.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Az alkalmazás a PowerShell-szolgáltatások lista][ps-getsfsvc]

    Az alkalmazás két szolgáltatás – az előtér és az állapot-nyilvántartó szolgáltatás, a szavakat kezelő tevődik össze.

3. Végül nézze meg az listája az WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![A szolgáltatás partíciók PowerShell megtekintése][ps-getsfpartitions]

    Az összes szolgáltatás háló PowerShell-parancsait, például akkor használt parancsok megadása bármely fürt, amely nem lehet csatlakozni, helyi vagy távoli érhetők el.

    Egy további vizuális erre a fürt együttműködhet a webes szolgáltatás háló Explorer eszköz navigálással [http://localhost:19080/Explorer](http://localhost:19080/Explorer) böngészőben is használhatja.

    ![A szolgáltatás háló Intézőben alkalmazás részleteinek megtekintése][sfx-service-overview]

    > [AZURE.NOTE] Szolgáltatás háló Explorer kapcsolatos további információért olvassa el a [háló Intézővel fürt megjelenítése](service-fabric-visualizing-your-cluster.md)című témakört.

## <a name="upgrade-an-application"></a>Alkalmazás frissítése
Szolgáltatás háló nincs – legrövidebb leállás frissítések biztosít az alkalmazás állapotának ellenőrzési azt összesíti a fürt keresztül. Nézzük frissíthető egy egyszerű WordCount alkalmazását.

Az alkalmazás letöltése az új verzió megszámolja csak egy magánhangzóként kezdődő szavakat. A frissítés összesíti, mint az alkalmazás működésének két változásai láthatja. Először, amelynél az száma növekszik, mennyi díjat kell lassú, mivel kevesebb szót számítanak alatt. Második mivel az összes partíciót csak egy minden tartalmazza az első partíciót két magánhangzót (A és az E) van, a darabszám ahányat indítása kell az outpace a többi.

1. [A WordCount v2 csomag letöltése](http://aka.ms/servicefabric-wordcountappv2) ezen a helyen, ahol letöltötte az v1 csomag.

2. Térjen vissza a PowerShell ablakát, és használja a SDK frissítés parancs az új verzió regisztrálhatja a fürt. Kezdje el a háló frissítése: / WordCount alkalmazást.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Meg kell jelennie a PowerShellben, mint a frissítés az alábbihoz hasonló látványtervek kezdetének kimeneti.

    ![A PowerShell frissítési folyamat][ps-appupgradeprogress]

3. Noha a frissítés eljárás, akkor előfordulhat, hogy egyszerűbb Lync-állapota háló Intézőből. Indítsa el a böngészőablakban, és kattintson a [http://localhost:19080/Explorer](http://localhost:19080/Explorer). Bontsa ki az **alkalmazások** a bal oldalon a fában, majd válassza a **WordCount**, és végül **háló: / WordCount**. Az alapvető tudnivalók lapon ekkor megjelenik a frissítés állapotának azt során a fürt frissítési tartományok keresztül.

    ![A szolgáltatás háló Intézőben frissítési folyamat][sfx-upgradeprogress]

    A frissítés során minden egyes tartományhoz keresztül, a állapot-ellenőrzései annak érdekében, hogy az alkalmazás nem a megfelelő viselkedik történik.

4. Ha az a háló szolgáltatások számára a korábbi lekérdezés futtatásakor: / WordCount alkalmazás, láthatja, hogy megváltozott WordCountService verzióját, de a WordCountWebService verziója nem történt meg:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Lekérdezés alkalmazásszolgáltatások frissítés után][ps-getsfsvc-postupgrade]

    Ezzel kijelöli a hogyan kezeli a szolgáltatási háló az alkalmazás frissítése. Úgy, hogy elérje készletet szolgáltatások (vagy a kód/konfigurációs csomagokat, azokat a szolgáltatásokat belül), amelyek módosultak, amely lehetővé teszi a folyamat gyorsabb frissítése, illetve megbízhatóbb.

5. Végül vegye figyelembe az alkalmazás új verzió viselkedését a böngésző vissza. Megfelelően működjön, a darabszám lassabban halad-e, és az első partíciót végződése a mennyiségi kicsit több.

    ![Az alkalmazás új verziójának megtekintése a böngészőben][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Törlése

Befejezés, mielőtt fontos ne feledje, hogy a helyi fürtre valós. Alkalmazások továbbra is futtatása a háttérben fut, amíg nem törli őket.  Attól függően, hogy az alkalmazások jellegét jelentős erőforrásokat a számítógépen futó alkalmazás egy vehet igénybe. Az alkalmazások és a fürt kezelése több lehetőség közül választhat:

1. Ha el szeretné távolítani az egyes alkalmazások és az összes adatot, futtassa a következő:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Másik lehetőségként az alkalmazás törlése a **Műveletek** menü szolgáltatás háló Explorer vagy a helyi menü az alkalmazás lista nézetben, a bal oldali ablaktáblában.

    ![Törlés az alkalmazás csak a szolgáltatási háló Explorer][sfe-delete-application]

2. Az alkalmazás törlése a fürt, miután 1.0.0 és WordCount alkalmazás típusú 2.0.0 verziók unregister parancsot. Törlés az alkalmazáscsomagok, beleértve a kódot és konfigurálása a fürt kép áruházból távolítja el.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Vagy a szolgáltatás háló Intézőben, válassza a **Leépíteni szöveg** alkalmazásához.

3. Állítsa le a fürt de az alkalmazás adatokat és a nyomkövetési naplók, kattintson a **Helyi fürtre leállítása** a rendszer tálca alkalmazásban.

4. A fürt teljesen törléséhez kattintson a **Helyi fürtre távolítsa el** a rendszer tálca alkalmazásban. Ez a beállítás egy másik lassú telepítési a következő alkalommal, amikor a Visual Studióban nyomja le az F5 eredményezi. Távolítsa el a helyi fürtre, csak akkor, ha nem szeretné használni egy kis időt, vagy ha a szükséges erőforrások visszanyeréséhez.

## <a name="1-node-and-5-node-cluster-mode"></a>1 csomópontot, és az 5 csomópont fürt mód

Alkalmazások fejlesztéséhez helyi fürtre dolgozik, ha gyakran megtalálta saját maga kódírás, hibakeresése során, módosítás kód, rövid közelítések módon hibakeresési stb. Optimalizálásához ezt a folyamatot, a helyi fürtre két módban futtatható: 1 csomópont vagy 5 csomópontot. Mindkét fürt mód van juttatások.
5 csomópont fürt mód lehetővé teszi, a tényleges fürtre végezhető. Feladatátvevő esetek tesztelése, további példányok és a szolgáltatások kópiák.
1 csomópont fürt módban gyors telepítéséhez és a regisztrációs szolgáltatások segít gyors érvényesítéséhez kód segítségével a szolgáltatás háló futtatókörnyezet van optimalizálva.

1 csomópont fürt mód, mind 5 csomópont fürt módja nem egy irányító vagy simulatort használja. Több elem gép fürt megtalálható platform ugyanazt kódot fusson.

> [AZURE.NOTE] Ez a funkció SDK verzió 5,2 és feletti érhető el.

Egy 1 csomópont fürt fürt módjának megváltoztatása, használja a szolgáltatás háló helyi felülete vagy PowerShell használata az alábbi módon:

1. Indítsa el a PowerShell új ablakban rendszergazdaként.

2. Futtassa a fürt telepítési parancsfájlt a SDK mappából:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Fürt beállítása néhány percet vesz igénybe. A telepítés befejezése után kimeneti hasonlóan kell megjelennie:
    
    ![Fürt beállítási kimeneti][cluster-setup-success-1-node]

Ha a kezelő szolgáltatás háló helyi használja:

![Váltás a fürt üzemmódja][switch-cluster-mode]

> [AZURE.WARNING] Ha fürt mód megváltoztatása az aktuális fürtre van eltávolítása a csoportból a rendszer, és új fürt készül. A fürt, kell tárolt adatok törlődnek, fürt mód megváltoztatásakor.

## <a name="next-steps"></a>Következő lépések
- Most, hogy telepítve vannak, és néhány beépített alkalmazás frissítése, akkor [próbálja felépíteni a Visual Studio saját](service-fabric-create-your-first-application-in-visual-studio.md).
- Minden, az ebben a cikkben a helyi fürt végrehajtott műveletek, valamint az [Azure fürt](service-fabric-cluster-creation-via-portal.md) hajtható végre.
- A frissítés, hogy a jelen cikkben végrehajtott egyszerű volt. Lásd: a [dokumentáció frissítése](service-fabric-application-upgrade.md) további tudnivalók a power és a frissítési szolgáltatás háló rugalmasság.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
