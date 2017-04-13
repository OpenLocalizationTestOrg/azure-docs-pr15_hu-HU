<properties
   pageTitle="Frissítés a Windows Server egy különálló szolgáltatás háló fürthöz |} Microsoft Azure"
   description="A szolgáltatás háló kódot és/vagy egy különálló szolgáltatás háló fürt fürt frissítési mód beállításáról futó konfigurációs frissítése"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>A Windows Server különálló szolgáltatás háló fürt frissítése

> [AZURE.SELECTOR]
- [Azure fürthöz](service-fabric-cluster-upgrade.md)
- [Különálló fürthöz](service-fabric-cluster-upgrade-windows-server.md)

Olyan modern rendszerekhez bővíthetőség megtervezése billentyűt a termék hosszú távú sikeres eléréséhez. A szolgáltatás háló fürtre egy erőforrást, amely az Ön tulajdonában. Ez a cikk azt ismerteti, hogyan biztosíthatja, hogy a fürt mindig fut a szolgáltatás háló verziók kód konfigurációk.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>A fürt futó háló-verziót szabályozása

Beállíthatja, hogy a fürt háló szolgáltatásfrissítések letölteni, ha a Microsoft által kiadott egy új verziója, vagy válassza ki, jelölje ki a kívánt a fürt kell az támogatott háló verzióját. 

Ehhez a "fabricClusterAutoupgradeEnabled" fürt konfigurálása beállítás IGAZ vagy hamis.


>[AZURE.NOTE] Ellenőrizze, hogy az mindig a szolgáltatás háló verzióját futtató fürt megtartása. És azt a szolgáltatást háló új verziójának megjelenése bejelentése, az előző verzióhoz képest legalább adott dátumtól 60 nap után támogatás vége van megjelölve. Az új verziókban bejelentett [az szolgáltatás háló termékcsapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Új kiadásának kiválasztásához, majd érhető el. 


Az új verzió csak akkor, ha használja egy gyártási stílusú csomópont konfigurációt, ahol az egyes szolgáltatás háló csomópont lefoglalása külön fizikai vagy virtuális gép frissíthet a fürt. Fejlesztési fürt, ha vannak egynél több szolgáltatás háló csomópontok virtuális gép vagy egyetlen fizikai, ahol kell lefelé a fürt tear és hozza létre ismét az új verzióval.


Vannak olyan két különböző munkafolyamat-re történő frissítésének a fürt legkésőbbi vagy támogatott szolgáltatás háló verziója. Egy kapcsolatot automatikus letöltése a legújabb verzió esetében, és a második, amelyek nincs kapcsolat töltheti le a legújabb szolgáltatás háló fürtre vonatkozóan.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>A fürt frissíteni a kapcsolatot töltse le a legújabb kódot és konfigurációs 

Az alábbi lépésekkel frissítése a fürt verzióját, ha a fürt csomópontok [http://download.microsoft.com](http://download.microsoft.com) internetkapcsolattal rendelkeznek 

Esetében [http://download.microsoft.com](http://download.microsoft.com)kapcsolatot hogy időközönként ellenőrzi, hogy az új szolgáltatás háló verzióiban elérhető.


Új szolgáltatás háló verziója érhető el, ha a csomag letöltése helyi meghajtóra a fürthöz és frissítésre kiépítve. Ezenkívül tájékoztatja a az új verzió ügyfele, a rendszer helyezi explicit fürt állapot figyelmeztetést az alábbihoz hasonló:

"Az aktuális fürt verzió [verzió #] támogatási befejeződik [dátum].", ha a fürt legújabb verzióját a figyelmeztető üzenet eltűnik.


#### <a name="cluster-upgrade-workflow"></a>Frissítési munkafolyamat fürt.
 
A fürt állapot figyelmeztetés jelenik meg, miután kell tegye a következőket:

1. Csatlakozzon a fürthöz minden olyan számítógépről, hogy a gép szerint vannak listázva a fürt csomópontok rendszergazdai hozzáféréssel rendelkezik. Ezt a parancsfájlt a gép nem kell a fürt része

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. A lista beszerzése az szolgáltatás háló verziói, amelyek frissíthet

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    egy kimenet kell kapnia alábbihoz hasonló:

    ![háló verzió beszerzése][getfabversions]

3. Elérhető a [Kezdés-ServiceFabricClusterUpgrade PowerShell cmd](https://msdn.microsoft.com/library/mt125872.aspx) használatával verziók valamelyikét egy fürt frissítés elindításához

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Figyelemmel követheti az állapotát, a frissítési szolgáltatás háló explorer be- vagy power rendszerhéj a következő parancs futtatásával

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Miután a problémákat, amelyek következtében a visszaállítás van rögzített, kell a frissítés ismét kezdeményezzen ezeket a lépéseket, mielőtt követve.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Frissítse a fürt a <U>nincs kapcsolat</u> töltheti le a legújabb kódot és konfiguráció

Az alábbi lépésekkel frissítése a fürt verzióját, ha a fürt csomópontok **nem rendelkezik** internetkapcsolattal [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Ha éppen nem csatlakozik az internet fürt futtatja, be kell figyelni az háló-termékcsapat blogjában egy új kiadásának értesítést kaphat. Bármely fürt egészségügyi figyelmeztetés figyelmezteti, hogy a rendszer **nem** helyezze.  

1. Módosítsa a fürt konfigurálása be a következő tulajdonságot hamis értékre.

        "fabricClusterAutoupgradeEnabled": false,

és a konfigurációs frissítés elindításához. [Kezdés-ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) hivatkozhat használatát részletekért. A fürt nyilvánvalóan verzió verziója a clusterConfig.JSON is van. Ellenőrizze, hogy frissítése előtt kikapcsolás kikapcsolása a konfigurációs frissítés.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Frissítési munkafolyamat fürt.
 


1. Töltse le a legújabb a csomag [szolgáltatás háló fürt létrehozása a windows Server](service-fabric-cluster-creation-for-windows-server.md) -dokumentum 


1. Csatlakozzon a fürthöz minden olyan számítógépről, hogy a gép szerint vannak listázva a fürt csomópontok rendszergazdai hozzáféréssel rendelkezik. Ezt a parancsfájlt a gép nem kell a fürt része 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. A letöltött csomag másolja a fürt kép áruházból.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Regisztráció a másolt csomag 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Rendelkezésre álló verziók valamelyikét egy fürt frissítés elindításához. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Figyelemmel követheti az állapotát, a frissítési szolgáltatás háló explorer be- vagy power rendszerhéj a következő parancs futtatásával

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Miután a problémákat, amelyek következtében a visszaállítás van rögzített, kell a frissítés ismét kezdeményezzen ezeket a lépéseket, mielőtt követve.



## <a name="next-steps"></a>Következő lépések
- A [szolgáltatás háló fürt háló beállításainak](service-fabric-cluster-fabric-settings.md) testreszabása
- Megtudhatja, hogyan kell [a fürt és kicsinyítés méretezése](service-fabric-cluster-scale-up-down.md)
- További tudnivalók: [alkalmazás frissítése](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
