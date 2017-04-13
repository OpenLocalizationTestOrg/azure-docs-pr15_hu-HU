<properties
   pageTitle="Service háló és a központi telepítés tárolók |} Microsoft Azure"
   description="A szolgáltatás és a tárolók használatát microservice alkalmazások telepítéséhez. Ez a cikk ismerteti a lehetőségeket nyújtó szolgáltatás háló tárolók és a Windows tároló kép telepítéséről fürt be"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Kép: Telepítéséhez Windows tároló szolgáltatás háló

> [AZURE.SELECTOR]
- [A Windows tároló terjesztése](service-fabric-deploy-container.md)
- [Docker tároló terjesztése](service-fabric-deploy-container-linux.md)

Ez a cikk végigvezeti a Windows tárolók indexelése services épület. 

>[AZURE.NOTE] Ez a funkció jelenleg nem érhető el a Windows Server 2016-ban való használatra és Linux előzetes verzióban. Ez lesz elérhető a következő kiadását szolgáltatás háló a Windows Server 2016 előzetes verzióban és a későbbi kiadásokban a támogatott ezt követően.

Szolgáltatás háló segítséget nyújt a microservices, amelyek indexelése állnak alkalmazások létrehozásába több tároló-funkciókkal rendelkezik. Ezek az úgynevezett indexelése szolgáltatások. 

A funkciókat tartalmazza;

- Tároló kép telepítési és aktiválási
- Erőforrás irányítási
- Tárházba hitelesítés
- A host port hozzárendelése tároló port
- A tároló-tároló feltárás és a kommunikáció
- Állítsa be, és a változók környezet beállítása

Lehetővé teszi, a régi szép minden egyes funkciók viszont a alkalmazásba felvenni kívánt indexelése szolgáltatás csomagolást.

## <a name="packaging-a-windows-container"></a>A Windows tároló csomagolása

Amikor a tároló csomagolása, Visual Studio project-sablon vagy [Alkalmazáscsomag létrehozása manuálisan](#manually)választhatja. Visual Studio használata esetén az alkalmazás csomag szerkezetének és jegyzékfájlok hozza létre az új projekt varázsló meg (Ez elérhető lesz a következő verzióban).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Visual Studio segítségével egy meglévő tároló kép csomag

>[AZURE.NOTE] Az elkövetkező kiadásokban a Visual Studio SDK szerszámok tároló adni egy alkalmazást felveheti a végrehajtható Vendég ma hasonlóképpen fogja. [Deploy vendégként való szolgáltatás háló végrehajtható](service-fabric-deploy-existing-app.md) témakörben talál. Jelenleg kell elvégeznie kézi összecsomagolása alábbiakban leírtak szerint.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Manuális csomagolása, és a tároló telepítése
A manuális csomagolása indexelése szolgáltatás folyamata a következő lépések alapján történik:

1. A tárolók közzé a tárházba.
2. A csomag Directoryban tagoláshoz.
3. A szolgáltatás nyilvánvalóan fájl szerkesztése.
4. Az alkalmazás nyilvánvalóan fájl szerkesztése.

## <a name="container-image-deployment-and-activation"></a>Tároló kép telepítési és aktiválási.
A szolgáltatás háló [alkalmazásmodell](service-fabric-application-model.md)tároló jelöl egy alkalmazás állomáson, amelyben az több szolgáltatást kópiák kerülnek. Üzembe helyezéséhez és tároló aktiválása, vigye a tároló képet be nevét a `ContainerHost` a szolgáltatás jegyzék elemét.

Adja meg a szolgáltatás jegyzék egy `ContainerHost` majd a bejegyzés pontra, és adja meg a `ImageName` kell a tároló tárházba és a kép nevét. A következő részleges jegyzék látható, a tároló *myimage:v1* hívását *myrepo* nevű összegyűjti üzembe helyezése

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Beviteli parancsok a tároló képet adhat a választható megadásával `Commands` elem vesszővel tagolt parancs futtatása a tároló belül. 

## <a name="resource-governance"></a>Erőforrás irányítási
Erőforrás irányítási egy videofunkcióinak a tároló és korlátozza az erőforrásokat, a tároló használhatja az állomáson. A `ResourceGovernancePolicy`, az alkalmazás jegyzék megadott, lehetővé teszi, hogy az erőforrás-szolgáltatás kód csomag korlátozások deklarálhatnak. Erőforrás korlátai beállítható, hogy

- A memória
- MemorySwap
- CpuShares (relatív súly Processzor)
- MemoryReservationInMB  
- BlkioWeight (relatív súly BlockIO). 

>[AZURE.NOTE] Az elkövetkező kiadásokban támogatja az adott szövegblokkokat IO korlátozások, például IOPs megadására szolgáló, olvasási/írási/s és mások nem lehetséges.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Tárházba hitelesítés
Előfordulhat, hogy a tároló tárházba a bejelentkezési adatok megadására a tároló letölteni. A bejelentkezési hitelesítő adatait, az *alkalmazás* jegyzék megadott használatával adja meg a bejelentkezési adatokat, vagy SSH billentyűjét, a tároló kép letöltése a kép tárat.  A következő példa bemutatja egy *tesztfelhasználó nevet* és a jelszót egyszerű szöveges nevű fiókot. Ez **nem** ajánlott.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

A jelszó és egy tanúsítványt a számítógépre telepítve kell titkosítható.

A következő példa bemutatja a fiók neve *tesztfelhasználó nevet* a jelszóval titkosított *MyCert*nevű tanúsítvány használatával. Használhatja a `Invoke-ServiceFabricEncryptText` hozhat létre a jelszót a titkos titkosítás szöveget Powershell-parancsot. Nézze meg ez a cikk [szolgáltatás háló alkalmazásokban titkos kulcsok kezeléséről](service-fabric-application-secret-management.md) további információt. A titkos kulcs a titkosított jelszót a tanúsítvány be kell vetni a helyi számítógép-sávon módszer (a via ARM: az Azure). Ezt követően szolgáltatás háló a gépre üzembe helyezése a szolgáltatás csomag, esetén a titkos visszafejtése és a fióknév mellett hitelesíteni a használja ezeket a hitelesítő adatokat tároló tárat.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>A host port hozzárendelése tároló port
Beállíthat egy host portot használja a tároló megadásával kommunikáció egy `PortBinding` az alkalmazás jegyzék. A port kötés, hogy a szolgáltatás a tároló az állomáson porthoz belül nem figyel a port rendeli.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>A tároló-tároló feltárás és a kommunikáció
Használatával a `PortBinding` házirend tároló port képezhető egy `Endpoint` be a szolgáltatás jegyzék az alábbi példában látható módon. Az endpoint `Endpoint1` is adjon meg egy rögzített portszámot, például a 80-as port, vagy adjon meg nincs port egyáltalán, ebben az esetben a fürt alkalmazás porttartományt a véletlen port választja ki.

A vendégként való bekapcsolódáshoz tárolók, megadása egy `Endpoint` hasonlóan ez a szolgáltatás jegyzék lehetővé teszi, hogy a szolgáltatás háló a végpont automatikusan közzétenni a névhasználati szolgáltatás, hogy a fürt futó más szolgáltatások, észleljék a feloldás szolgáltatás többi lekérdezésekkel tároló. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Regisztrálás a névhasználati szolgáltatással, könnyen végrehajtható tároló-tároló kommunikációs be a kódot az, [fordított proxykiszolgáló](service-fabric-reverseproxy.md)használata a tárolóban. Kell tennie mindössze adja meg a fordított proxykiszolgáló http listening portja és a szolgáltatások, amelyek a megadásával, ezek a környezeti változók kommunikálni szeretne nevét. Nézze meg a következő szakaszban ehhez.  

## <a name="configure-and-set-environment-variables"></a>Konfigurálása és a változók környezet beállítása
Környezeti változók lehetnek megadott foe mindkét tárolók, vagy folyamatok/Vendég végrehajtható telepített szolgáltatások cikkét minden kód csomag a szolgáltatásban. A környezet változóértékkel lehet kifejezetten az alkalmazás jegyzék felülbírálva vagy megadott paraméterként alkalmazást a telepítés során.

A következő szolgáltatás jegyzék XML-kódrészlet példa megadása környezet változói kód csomagot. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Ezek a környezeti változók felülírható az alkalmazás nyilvánvalóan szintjén:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

A fenti példában egy kifejezett értéke megadott azt a `HttpGateway` környezeti változó (19000) érték közben `BackendServiceName` paraméter értéke keresztül a `[BackendSvc]` alkalmazás paraméter. Ez lehetővé teszi, hogy a értékének meghatározása `BackendServiceName`az az érték az alkalmazás telepítése időpontját, és nem szerepel egy rögzített értéket a jegyzék. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Töltse ki az alkalmazás és a szolgáltatás jegyzék példákat is tartalmaz
Egy példa alkalmazás jegyzék, amely mutatja az tároló szolgáltatások a következő:


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Az alábbi képen szolgáltatás jegyzék (az előző alkalmazás jegyzék megadva), amely tartalmazza a tároló funkciók

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
