<properties 
pageTitle="Kommunikáció a Cloud Services szerepkörök |} Microsoft Azure" 
description="A felhőalapú szolgáltatások szerepkör-példányok végpontokat (http, https, tcp, udp) megadott őket, vagy más szerepkör-példányok között a külső kommunikáció állhat." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Szerepkör-példányok azure-ban kommunikáció engedélyezése

Felhőalapú szolgáltatás szerepkörök kommunikáció belső és külső kapcsolaton keresztül. Miközben belső kapcsolatok úgynevezett **belső végpontok**a külső kapcsolatok **beviteli végpontok** neve. Ez a témakör ismerteti, hogyan lehet módosítani a [definiált szolgáltatás](cloud-services-model-and-package.md#csdef) végpont létrehozásához.


## <a name="input-endpoint"></a>Beviteli végpont
A beviteli végpont használatos, ha meg szeretné elérhetővé tenni a külső olyan portot. A Protocol (protokoll), és a portja a végpontot, amely akkor alkalmazza a mind a belső és külső portokhoz a végpont megadása Tetszés szerint megadhatja a végpont különböző belső port [Helyi_port](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) attribútumot tartalmazó.

A beviteli végpont használható a következő protokollok: **http, https, tcp, udp**.

Beviteli végpont létrehozásához a **InputEndpoint** gyermekelem hozzáadása egy webhelyen vagy a dolgozó szerepkör az **Végpontok** elemet.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Beviteli végpont példány
Beviteli végpontok példány hasonlóak végpontok de lehetővé teszi a szövegbeviteli minden egyes szerepkör az előfordulásra vonatkozóan adott nyilvános portok feleltesse meg a terheléselosztó port átirányítás segítségével. Megadhatja, hogy egy nyilvános elérésű portot, vagy egy porttartományt.

A példány beviteli végpont csak használható **tcp** - és **UDP-** protokoll.

A webhelyen vagy a dolgozó szerepkör a **Végpontok** elemének a **InstanceInputEndpoint** gyermekelem hozzáadása egy példány beviteli végpont létrehozásához.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Belső végpont
Belső végpontok példány-példány kommunikáció érhetők el. A port nem kötelező, és adja meg, ha egy dinamikus port hozzá van rendelve az végpontot. Egy porttartományt használható. Legfeljebb öt belső végpontok szerepkörönként van.

A belső végpont használható a következő protokollok: **http, tcp, udp, bármilyen**.

Belső beviteli végpont létrehozásához a **InternalEndpoint** gyermekelem hozzáadása egy webhelyen vagy a dolgozó szerepkör az **Végpontok** elemet.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Egy porttartományt is használhatja.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Dolgozó szerepkörök és webes szerepkörök összehasonlítása

Egy kis különbség van a végpontok dolgozó és a webes szerepkörök való munkavégzés során. A webes szerepkör legalább egy beviteli végpontot a **HTTP** protokollal kell rendelkeznie.


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>A .NET SDK használatával zárólap eléréséhez
Az Azure felügyelt tár bemutat szerepkör-példányok futásidőben kapcsolatba lépni. Egy szerepkör-példány belül futó kódból meghallgathatja a többi szerepkör-példányok és a végpontok megléte adatait, valamint az aktuális szerepkör-példány információt.

> [AZURE.NOTE] Szerepkör-példányok a felhőszolgáltatásában futtató, és, amely meghatározza, hogy legalább egy belső végpont adatait csak meghallgathatja. Szerepkör-példányok egy másik szolgáltatás fut adatokat nem tudja kérni.

A [példányok](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) tulajdonsággal beolvasni a szerepkörbe példányát. Először a [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) használatával az aktuális szerepkör-példány hivatkozását adja eredményül, és a [szerepkör](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) tulajdonsággal a szerepkört, magát hivatkozását adja eredményül.

Amikor csatlakozik egy programozás útján keresztül a .NET SDK szerepkör-példányt, érdemes viszonylag könnyen elérhető legyen a végpont adatait. Például után már felvette a szerepkör környezet, szerezheti be a port a kód egy adott végpont:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

A **példányok** tulajdonság **RoleInstance** objektumok gyűjteménye adja eredményül. Ebben a gyűjteményben mindig az aktuális példány tartalmazza. Ha a szerepkör nem definiálja egy belső végpontot, a gyűjteménybe az aktuális példány, de más példányok tiltása. A gyűjtemény szerepkör-példányok száma mindig lesz abban az esetben, ha nincs belső végpont a szerepkör által definiált 1. A szerepkör a belső zárólap határozza meg, ha saját példányok találhatók a futtatókörnyezet, és a példányok a szerepkör a szolgáltatás konfigurációs fájl a megadott számú felel meg a webhelycsoport példányok száma.

> [AZURE.NOTE] Az Azure felügyelt tár nem teszik lehetővé a többi szerepkör-példány állapotának meghatározása, de alkalmazhat az ilyen orvosi vizsgálatok saját maga Ha ezt a funkciót a szolgáltatásban. [Azure diagnosztika](cloud-services-dotnet-diagnostics.md) segítségével szerepkör példánya fut információhoz juthat.

Annak megállapításához, a portszámot egy belső végponton egy szerepkör-példányt, a [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) tulajdonsággal egy szótár objektum végpontjának neve és a megfelelő címek és IP-portok tartalmazó vissza. A [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) tulajdonság IP-címet és egy megadott végpont port adja vissza. A **PublicIPEndpoint** tulajdonság egy terheléselosztása végpontot portja adja eredményül. A **PublicIPEndpoint** tulajdonság IP-cím részét nem használatos.

Íme egy példa szerepkör-példányok iteráló.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Íme egy példa, hogy a elérhetővé tett végpont kap a szolgáltatás definition keresztül, és elindítja a kapcsolatokat figyeli dolgozó szerepkörbe.

> [AZURE.WARNING] Ez a kód egy telepített szolgáltatás csak működni fog. Ha futtatja az Azure kiszámítania irányító, szolgáltatás konfigurációs elemek, amelyek közvetlen port végpontokat (**InstanceInputEndpoint** elemeket) létrehozása a program figyelmen kívül hagyja.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Szerepkör kommunikációs szabályozhatja a hálózati forgalmat szabályok
Után belső végpontok határozza meg, hogyan szerepkör-példányok tudnak kommunikálni, a többi vezérlő felveheti hálózati forgalmának engedélyezésére szabályok (a végpontok létrehozott alapján). A következő ábrán néhány tipikus esetei szerepkör kommunikációs szabályozása:

![Hálózati forgalmának engedélyezésére szabályok felhasználási területei] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Hálózati forgalmának engedélyezésére szabályok felhasználási területei")

Az alábbi példa mutatja szerepkör-definíciók a szerepkörökhöz az előző ábrán látható. Minden egyes szerepkör-definíció definiált legalább egy belső végpont tartalmazza:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Szerepkörök közötti kommunikáció korlátozás akkor fordulhat elő, a belső végpontjait rögzített, és automatikusan portok egyaránt.

Alapértelmezés szerint után van adva a belső zárólap, kommunikáció is flow bármely szerepkörből korlátozás nélkül a szerepkör a belső végpontra. Kapcsolati korlátozása, fel kell vennie egy **NetworkTrafficRules** elemet a szolgáltatás-definíciós fájl **ServiceDefinition** elemre.

### <a name="scenario-1"></a>Forgatókönyv 1
Csak engedélyezni **WorkerRole1** **WebRole1** a hálózati forgalmának engedélyezésére.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Forgatókönyv 2
Csak lehetővé teszi a hálózati forgalmat a **WebRole1** **WorkerRole1** és **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Forgatókönyv 3
Csak engedélyezi a hálózati forgalmának engedélyezésére az **WebRole1** **WorkerRole1**és **WorkerRole1** **WorkerRole2**szeretne.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Forgatókönyv 4
Csak engedélyezi a hálózati forgalmának engedélyezésére az **WebRole1** **WorkerRole1**, **WebRole1** való **WorkerRole2**, **WorkerRole1** **WorkerRole2**szeretne.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Az elemek használják feletti egy XML-séma referencia megtalálható [Itt](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Következő lépések
További információ a Felhőbeli szolgáltatástól [modell](cloud-services-model-and-package.md).