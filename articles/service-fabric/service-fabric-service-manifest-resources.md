<properties
   pageTitle="Adja meg a szolgáltatás háló szolgáltatási végpontok |} Microsoft Azure"
   description="Hogyan ismertetik a szolgáltatás jegyzék, például hogy miként állíthatja be a HTTPS-végpont végpont források"
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

# <a name="specify-resources-in-a-service-manifest"></a>Adja meg a szolgáltatás jegyzék erőforrások

## <a name="overview"></a>– Áttekintés

A szolgáltatás jegyzék lehetővé teszi, hogy a információforrások, amelyek deklarálva/módosítani kell a lefordított kód megváltoztatása nélkül szeretné a szolgáltatás által használt. Azure Service háló a szolgáltatás végpontjának erőforrások konfigurációs támogatja. A szolgáltatás jegyzék megadott források az access a SecurityGroup az alkalmazás jegyzék a via szabályozható. Az erőforrások nyilatkozat lehetővé teszi, hogy az alábbi források segítségével módosíthatók telepítési idő, ami azt jelenti, a szolgáltatás nem kell egy új konfigurációs eljárást vezet be. A ServiceManifest.xml fájl sémadefiníciója telepítve van a szolgáltatás háló SDK és a *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*eszközöket.

## <a name="endpoints"></a>Végpontok

Ha a szolgáltatás jegyzék egy végpont erőforrás van definiálva, szolgáltatás háló portokat a fenntartott alkalmazás porttartományt oszt ki Ha olyan portot kifejezetten nincs megadva. Például tekintse meg az *ServiceEndpoint1* a bekezdés után a nyilvánvalóan kódtöredékének megadott végpontot. Ezenkívül szolgáltatások is kérhet egy erőforrás egy adott portot. Másik fürthöz csomópontok futó szolgáltatás kópiák rendelhetők különböző portszámokat, miközben ugyanazon a csomóponton futó szolgáltatás replikái megosztása a port. A szolgáltatás replikák használhatja ezeket a portokat igény szerint a replikáció és a ügyfél kérések figyelését.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Hivatkozhat [konfigurálása megbízható, szűrőként működő szolgáltatások megbízható](service-fabric-reliable-services-configuration.md) olvasható további információ a végpontok hivatkozó a config csomag beállításait fájl (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Példa: a szolgáltatás HTTP végpont megadása

A következő szolgáltatás jegyzék egy TCP végpont és két HTTP végpont erőforrást a meghatározza a &lt;erőforrások&gt; elemet.

HTTP végpontjait a program automatikusan vezérlés szolgáltatás háló használna.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Példa: a szolgáltatásbeli HTTPS végpont megadása

A HTTPS protokollt kiszolgáló hitelesítésről és ügyfél kiszolgáló közötti kommunikáció titkosításához is használják. HTTPS engedélyezése a szolgáltatás háló szolgáltatásban, adja meg a protokoll a *-> erőforrások végpontok végpont ->* szakasz szolgáltatás jegyzék, a végpont *ServiceEndpoint3*korábbi látható módon.

>[AZURE.NOTE] A szolgáltatás, protokoll alkalmazása a frissítés során nem módosíthatók, azt egy szakítószilárdságának alkotó módosítása nélkül.


Íme egy példa, hogy be kell állítania a HTTPS-ApplicationManifest. Meg kell adni a ujjlenyomat a tanúsítvány. A EndpointRef EndpointResource ServiceManifest, amelynek beállíthatja a HTTPS protokollt a hivatkozás. Egynél több EndpointCertificate is hozzáadhat.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
