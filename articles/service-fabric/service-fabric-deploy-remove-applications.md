<properties
   pageTitle="Szolgáltatás háló alkalmazás telepítési |} Microsoft Azure"
   description="Hogyan telepíthető, és a szolgáltatás háló alkalmazások eltávolítása"
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

# <a name="deploy-and-remove-applications-using-powershell"></a>Üzembe helyezéséhez és távolítsa el az alkalmazások PowerShell használatával

> [AZURE.SELECTOR]
- [A PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Egyszer- [alkalmazás típusa van már csomagolt][10], egy Azure Service háló fürthöz történő telepítés készen is. Telepítési a következő három lépésből:

1. Alkalmazáscsomag feltöltése
2. Regisztráljon az alkalmazás típusa
3. Az alkalmazás hozza létre

>[AZURE.NOTE] Ha a Visual Studio üzembe helyezése, és kattintson a helyi fejlesztési fürt alkalmazások hibakeresése használ, az alábbi lépéseket kezelése automatikusan keresztül egy PowerShell-parancsprogramot, az alkalmazás projekt parancsfájlok mappában található. Ez a cikk a Mi a parancsfájlok mivel vannak elfoglalva, hogy kívül Visual Studio ugyanazokat a műveleteket végezheti el a háttér tartalmaz.

## <a name="upload-the-application-package"></a>Alkalmazáscsomag feltöltése

Alkalmazáscsomag feltöltése helyezi a belső szolgáltatás háló összetevők által elérhető helyen. A PowerShell használatával végezze el a feltöltés. Ez a cikk PowerShell parancsok futtatása előtt mindig indíthat a szolgáltatás háló fürthöz való kapcsolódás [Csatlakozás-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) használatával.

Tegyük fel, hogy van egy *MyApplicationType* nevű mappát, amely tartalmazza a megfelelő alkalmazás jegyzék, szolgáltatás jegyzékfájlok és adatainak-kód-webhelyoszlopai csomagok. A [Másolás-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) parancs a csomag feltölti a fürt kép áruházból. A **Get-ImageStoreConnectionStringFromClusterManifest** parancsmag, a szolgáltatás háló SDK PowerShell-modult része, amely a kép áruházból kapcsolati karakterláncát szolgál.  Futtassa a SDK modul importálásához:

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Az alkalmazás-csomagok átmásolhatja *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* *c:\temp\MyApplicationType* (átnevezése a "hibakeresési" könyvtár "MyApplicationType"). Az alábbi példa a csomag feltöltések megjelenítése:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Regisztráljon az alkalmazáscsomag

Az alkalmazás típusa és a használatra a rendelkezésre álló alkalmazás jegyzék deklarált verzió alkalmazáscsomag regisztrálása teszi. A rendszer beolvassa a csomagot, az előző lépésben feltöltött, ellenőrizze a csomag (egyenértékű a [Próba-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) helyben fut), a csomag tartalma folyamat és a feldolgozott csomag egy belső rendszer helyre másolja.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

A [Külső.FÜGGV-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) parancs értéket ad vissza, csak azt követően a rendszer sikeresen másolta a alkalmazáscsomag. Alkalmazáscsomag tartalmának függ, hogy mennyi ezzel időt vesz igénybe. Ha szükséges, a **- TimeoutSec** paraméter egy hosszabb időtúllépés megadására használható. (Az alapértelmezés szerinti időtúllépési a 60 másodperc).

A [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) parancs megjeleníti a sikeres regisztrált alkalmazás típusú változatait.

## <a name="create-the-application"></a>Az alkalmazás létrehozása

Az alkalmazás a verziójával bármely alkalmazás típusú regisztrált keresztül a [New-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) parancsot sikerült létrehozhatnak. A minden alkalmazás nevét kell kezdődnie, a *háló:* rendszer, és minden alkalmazás példány egyedi lehet. A célalkalmazás-típus, az alkalmazás jegyzék meghatározott alapértelmezett szolgáltatások adott időben jönnek létre.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

A [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) parancs a összes szolgáltatásalkalmazás-példányok készített sikeresen, a teljes állapotát együtt jeleníti meg.

A [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) parancs az összes szolgáltatás példányainak sikeresen létrehozott egy adott alkalmazás példány sorolja fel. (Ha van ilyen) alapértelmezett szolgáltatások listája megtalálható.

Több szolgáltatásalkalmazás-példányok regisztrált alkalmazás típusú bármely adott verziójához hozhat létre. Minden alkalmazás példánya fut elkülönítési saját munka könyvtár és a folyamat.

## <a name="remove-an-application"></a>Alkalmazás eltávolítása

Alkalmazás-példány már nincs szükség, ha azt véglegesen távolíthatja el a [Eltávolítása-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) parancs használatával. Ez a parancs automatikusan törli az összes szolgáltatás tartoznak, valamint az alkalmazás véglegesen eltávolítja az összes szolgáltatás állapota. Ez a művelet nem vonható vissza, és az alkalmazás állapota sem állíthatók.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Ha már nincs szükség a kívánt alkalmazás: egy adott verzióját, a [Unregister-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) parancs használatával kell unregister. Használaton kívüli típusok regisztrációjának elengedi az adott típusú kép áruházból letölthető alkalmazás csomag tartalma által használt tárterület. Az alkalmazás típusa mindaddig, amíg ki nem alkalmazások létrehozásának ellen, és nem alkalmazás függőben lévő frissítések hivatkozik rá lehet nem regisztrált.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Másolás-ServiceFabricApplicationPackage kér egy ImageStoreConnectionString

A szolgáltatás háló SDK környezetet kell már rendelkezik a megfelelő alapértelmezéseinek beállítása. De ha szükséges, a minden parancs ImageStoreConnectionString egyeznie kell az érték, amely a szolgáltatás háló fürt használja. Ez a fürt jegyzék a [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) parancs keresztül beolvasott találhat:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Következő lépések

[Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)

[Szolgáltatás háló állapot – bevezetés](service-fabric-health-introduction.md)

[Diagnosztizálása és szolgáltatás háló szolgáltatás – problémamegoldás](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[A szolgáltatás háló céljával modell](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
