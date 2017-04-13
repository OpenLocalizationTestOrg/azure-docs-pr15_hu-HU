<properties
   pageTitle="Megbízható szolgáltatások kommunikációs – áttekintés |} Microsoft Azure"
   description="A megbízható szolgáltatások kommunikációs modell, beleértve a nyitó hallgatók szolgáltatásokra vonatkozó, végpontok megoldása és szolgáltatások közötti kommunikáció áttekintése."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>A megbízható szolgáltatások kommunikáció API-k használata

Azure Service háló, mint egy platform teljesen agnostic kapcsolatos szolgáltatások közötti kommunikációt. Az összes protokollok és összefoglalások elfogadhatók, a HTTP UDP. Válassza ki, hogyan szolgáltatások kommunikáció kell a service fejlesztők van. A megbízható Services alkalmazás keretrendszer beépített kommunikációs készleteket, valamint az API-hoz, amelyeket az egyéni kommunikációs összetevők összeállításához használhat. 

## <a name="set-up-service-communication"></a>Szolgáltatás kommunikáció beállítása

A megbízható szolgáltatások API szolgáltatás kommunikációs egyszerű felületet használja. Nyissa meg a szolgáltatás zárólap, egyszerűen végrehajtja a kapcsolat:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

A kapcsolati figyelő végrehajtása kiegészítése visszahelyezi az a osztály szolgáltatás alapú többszörösen majd.

Az állapot nélküli szolgáltatások:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Az állapot-nyilvántartó szolgáltatások:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Mindkét esetben lépjen vissza hallgatók gyűjteménye. Több végpontok, esetleg használatával különböző protokollok használatával több hallgatók figyelni a szolgáltatás lehetővé teszi. Előfordulhat például, a HTTP-figyelő és egy külön WebSocket figyelő. Minden egyes figyelő kapja a nevét, és az eredményül kapott gyűjteménye *neve: cím* párban JSON objektumként jelöli, amikor egy ügyfél kér a szolgáltatás példányának vagy partíciót a listening címeket.

Az állapot nélküli szolgáltatás a felülírási ServiceInstanceListeners gyűjteménye adja eredményül. Egy ServiceInstanceListener hozhat létre egy ICommunicationListener függvényt tartalmaz, és elnevezi azt. Az állapot-nyilvántartó szolgáltatások a felülírási ServiceReplicaListeners gyűjteménye adja eredményül. Ennek oka az állapot nélküli megfelelőjük némileg eltér a ServiceReplicaListener megnyitása egy másodlagos példányon ICommunicationListener lehetősége van. Nemcsak használhatók több kommunikációs hallgatók egy szolgáltatásból, de is megadhatja, melyik hallgatók összehívások elfogadása példányon másodlagos és melyek csak példányon elsődleges meghallgatásához.

Például egy ServiceRemotingListener, amely csak példányon elsődleges RPC hívásokat is van, és a HTTP-példányon másodlagos kéri, hogy mi olvasási második, egyéni figyelő:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Egy szolgáltatás több hallgatók létrehozásakor minden figyelő **kell** adni egy egyedi nevet.

Végezetül ismertetik a végpontok a szolgáltatást a [szolgáltatás cikkét](service-fabric-application-model.md) végponton csoportjában az szükséges.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

A kapcsolati figyelő elérhetik azt a végpontot feladatoknak a `CodePackageActivationContext` a a `ServiceContext`. A figyelő Ezután megkezdheti a kérések figyelését megnyitáskor.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Végpont erőforrások teljes szolgáltatás csomag közös, és azok osztja szolgáltatás háló a szolgáltatás csomag aktiválásakor. Az azonos ServiceHost kiszolgálón több szolgáltatás kópiák is lehet ugyanaz az ugyanazt a portot. Ez azt jelenti, hogy a kapcsolati figyelő támogatja-e port megosztása. Az ajánlott módszereket, ezzel a kapcsolati figyelő és replikált/példány azonosító partíciót melyikkel hoz létre a meghallgatását cím szolgál.

### <a name="service-address-registration"></a>Szolgáltatás cím regisztráció

A *Szolgáltatás elnevezési* nevű rendszer szolgáltatás futtat, a szolgáltatás háló fürt. A elnevezési szolgáltatás nem szolgáltatások és a címeket, amelyek minden példány vagy a szolgáltatás replikáját figyeli a tartományregisztrálónál. Ha a `OpenAsync` metódusát egy `ICommunicationListener` befejeződött, a visszatérési értéket kapja regisztrált a elnevezési szolgáltatásban. A visszaadandó értéket a elnevezési szolgáltatásban közzétett kap egy olyan karakterlánc, amelynek értéke lehet bármi egyáltalán. A karakterlánc érték mi ügyfélalkalmazások jelenik, hogy kérni egy címet a szolgáltatás a elnevezési szolgáltatásból.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Szolgáltatás háló tartalmaz, amely lehetővé teszi az ügyfelek és más szolgáltatásokhoz, majd kérhet a cím szolgáltatás neve szerint API-khoz. Ez fontos, mivel a szolgáltatás címe nem statikus. Szolgáltatások helyezi át a fürt, erőforrás és a elérhetősége céljából. Ez az eljárás, amely lehetővé teszi az ügyfelek a szolgáltatás listening címe feloldásához.

> [AZURE.NOTE] Egy teljes segédlet, hogy miként írhat a egy `ICommunicationListener`, lásd: a [szolgáltatás háló webes API-szolgáltatások OWIN önálló szolgáltatónál](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Kommunikáció a szolgáltatás
A megbízható szolgáltatások API írni az ügyfelek, kommunikáció szolgáltatások a következő tárak ismertetése

### <a name="service-endpoint-resolution"></a>Szolgáltatás végpontjának felbontása
A szolgáltatás való kommunikáció első lépése a partíciót vagy a szolgáltatás szeretne beszélgetni példánya végpont címének feloldásához. A `ServicePartitionResolver` segédprogram osztály egy egyszerű egyszerű, amely segít a ügyfelek futásidőben szolgáltatás végpontjának határozza meg. A szolgáltatás háló terminológia a folyamat meghatározása a szolgáltatás végpontjának nevezik a *szolgáltatás végpontjának megoldás*.

Szolgáltatások belül fürt csatlakozhat `ServicePartitionResolver` alapértelmezett beállításainak használatával hozható létre. Ez az esetek többségében a javasolt használatát:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Egy másik fürthöz szolgáltatások csatlakozhat egy `ServicePartitionResolver` fürt átjáró végpontok halmazának hozhatja létre. Ne feledje, hogy átjáró végpontok fürt való csatlakozáshoz egyszerűen különböző végpontokon. Példa:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Azt is megteheti `ServicePartitionResolver` kaphatnak a függvény hozhat létre egy `FabricClient` belső használata: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`az objektum, amely a szolgáltatás háló fürt különféle adatkezelési műveletek a fürt kommunikálni van. Ez akkor hasznos, ha azt szeretné, hogy hogyan szabályozhatósága `ServicePartitionResolver` hogyan kommunikáljon a fürt. `FabricClient`hajt végre, belső gyorsítótárazás, és általában drága szeretne létrehozni, ezért fontos, hogy újra `FabricClient` példányok szerint. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

A feloldás mód majd a szolgáltatás vagy particionált Services szolgáltatás partíciót címét beolvasásához használt.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

A webszolgáltatás címének egyszerűen segítségével megoldható egy `ServicePartitionResolver`, de több munkához szükség, annak érdekében, hogy a megoldott cím helyes használható. Az ügyfélnek kell ellenőrzi, hogy a kapcsolatfelvétel tranziens hiba miatt sikertelen volt, és is meg kell ismételni (például szolgáltatás áthelyezték vagy átmenetileg nem érhető el), vagy egy állandó hiba (például szolgáltatás törölt vagy az erőforrás megszűnt). Szolgáltatás példányainak vagy kópiák lehet váltani a csomópont csomópont több ok miatt bármikor. A szolgáltatás címe és a rendszer `ServicePartitionResolver` lehet elavult, az idő az ügyfél kód megkísérli a csatlakozást. Ebben az esetben újra az ügyfélnek kell újra megoldani a címet. Az előző nyújtó `ResolvedServicePartition` azt jelzi, hogy a feloldó próbálkozzon újra egyszerűen lekérés nem pedig annak egy gyorsítótárazott címet.

Általában az ügyfél-kódot kell nem működik a `ServicePartitionResolver` közvetlenül. Létrejön, és kommunikációs ügyfél gyárak a megbízható szolgáltatások API továbbítja. A gyárak segítségével a feloldó belső egy használt kommunikáció a szolgáltatások ügyfél-objektum létrehozása.

### <a name="communication-clients-and-factories"></a>Kapcsolati ügyfelek és gyárakból

A kapcsolati gyári tárban, amely megkönnyíti a megszűnt szolgáltatást végpontok ismételt kapcsolatok tipikus hibafa-kezelési újrapróbálkozási mintát hajtja végre. A gyár tárat a újrapróbálkozási módot is biztosít, amíg Ön megadja-e a hiba-kezelők.

`ICommunicationClientFactory`az alap felületet, amelyet szolgáltatás háló szolgáltatás is beszélhet az ügyfél kapcsolati ügyfél gyár megvalósítani határoz meg. A CommunicationClientFactory végrehajtásának attól függ, hogy a kapcsolati Papírhalom, a szolgáltatás háló szolgáltatás által használt, ahol az ügyfélnek nem felel meg kapcsolatba lépni. A megbízható szolgáltatások API biztosít a `CommunicationClientFactoryBase<TCommunicationClient>`. Ezzel a megoldással alap végrehajtásának a `ICommunicationClientFactory` felület és a feladatokat, amelyek megegyeznek az összes kommunikációs készleteket hajtja végre. (Az alábbi műveleteket szerepeltetni, használja a `ServicePartitionResolver` határozza meg a szolgáltatás végpontjának). Ügyfelek általában az absztrakt CommunicationClientFactoryBase osztály kezelésére, amely a kapcsolati sorba az logika hajtja végre.

A kapcsolati ügyfél csak kap egy címet, és ezt használja a csatlakozást a szolgáltatáshoz. Az ügyfél használhatja azt szeretné, hogy bármilyen Protocol (protokoll).

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Az ügyfél gyári elsődlegesen az alábbiakért kommunikációs ügyfelek létrehozása. Ügyfelek, amely nem karbantartása állandó kapcsolat, például a HTTP-ügyfelek a gyár csak meg kell adni hozhat létre, és lépjen vissza az ügyfélnek. A gyár határozza meg, függetlenül attól a kapcsolat hozható létre ismét kell is kell érvényesíteni más protokollok állandó kapcsolat, bizonyos bináris protokollok, például karbantartása.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Végül egy kivétel kezelő felelős meghatározásához, hogy milyen műveletet végre kell hajtania kivételt fordul elő. A kivételek kategóriába **retriable** és **retriable nem**. 

 - **Nem retriable** kivételek egyszerűen első újra elő vissza az kíván a hívó féllel. 
 - A kivételek **Retriable** további kategorizálják **ideiglenes (tranziens)** , és **nem tranziens**.
  - A kivételek **tranziens** azok, amely egyszerűen anélkül, hogy újra feloldása a szolgáltatás végpontjának címe kell ismételni. Fogja az érintett tranziens hálózati problémák vagy szolgáltatás hibaüzenetek nincsenek képletkészítőkhöz, amely jelzi, hogy a szolgáltatás végpontjának címe nem létezik. 
  - **Nem tranziens** kivételek azok a szolgáltatás végpontjának címe újra kikeresési igénylő. Ezek kivételek, amely jelzi, hogy a szolgáltatás végpontjának nem érhető el, a szolgáltatás jelző helyezte át egy másik csomópontot. 

A `TryHandleException` lehetővé teszi a döntés egy adott kivételt. Ha azt **nem tudja,** milyen határozatok, hogy a kivétel, kell visszaadnia **hamis**. Ha azt **, hogy** Mi az, hogy határozattal, meg kell ennek megfelelően beállítani az eredményt, és **Igaz értéket**ad vissza.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>A teljes kép
Az egy `ICommunicationClient`, `ICommunicationClientFactory`, és `IExceptionHandler` egy kommunikációs protokoll épülnek egy `ServicePartitionClient` van az összes közös fussa azt, és bemutat alábbi összetevők körül hibafa-kezelés felülbírálata jelölőnégyzetet, és szolgáltatási partíciót cím felbontás le.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Következő lépések
 - Példa HTTP kommunikációs szolgáltatások [GitHUb minta projekt](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)között.

 - [A megbízható szolgáltatások távelérési távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)

 - [Webes API-t használó OWIN megbízható szolgáltatások](service-fabric-reliable-services-communication-webapi.md)

 - [WCF kommunikációs megbízható Services használatával](service-fabric-reliable-services-communication-wcf.md)
