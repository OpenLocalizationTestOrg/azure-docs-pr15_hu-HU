<properties
   pageTitle="Megbízható szolgáltatások WCF kommunikációs Papírhalom |} Microsoft Azure"
   description="A beépített WCF-kapcsolati Papírhalom szolgáltatás háló ügyfél-szolgáltatás WCF kommunikáció a megbízható szolgáltatásokat biztosít."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>Kommunikáció a WCF-alapú Papírhalom megbízható szolgáltatások
Megbízható szolgáltatások keretében lehetővé teszi, hogy az illető szolgáltatás szeretne használni kívánt kommunikációs objektumhalomban válassza a szerzők szolgáltatás. Azok a kapcsolati egymást fedő tetszés szerinti [CreateServiceReplicaListeners vagy CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) módszerek – által eredményül adott **ICommunicationListener** keresztül is csatlakoztasson. A keretrendszer a kapcsolati Papírhalom, a Windows Communication Foundation (WCF) alapú szolgáltatás szerzők ki szeretné használni a WCF-alapú kommunikációs megvalósítása.

## <a name="wcf-communication-listener"></a>WCF kommunikációs figyelő
A WCF-specifikus végrehajtását **ICommunicationListener** **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** osztály által biztosított.

Közpénzek ne tegyük fel van még típusú szolgáltatási szerződés`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

A szolgáltatás a következő módon létrehozunk egy WCF kommunikációs figyelő.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Ügyfelek számára a WCF-kapcsolati Papírhalom írása
Írásához ügyfelek szolgáltatások kommunikálni WCF használatával, a keretrendszer **WcfClientCommunicationFactory**, amely [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md)WCF-specifikus végrehajtását.

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

A WCF-kapcsolati csatorna webböngészőn keresztül elérhetők a **WcfCommunicationClientFactory**által létrehozott **WcfCommunicationClient** .

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Ügyfél-kódot a **WcfCommunicationClientFactory** hajtja végre **ServicePartitionClient** a szolgáltatás végpontjának megállapítja, és a kommunikáció a szolgáltatást a **WcfCommunicationClient** együtt használható.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Az alapértelmezett ServicePartitionResolver feltételezi, hogy az ügyfél azonos fürt a szolgáltatás fut-e. Ha, amely esetben ServicePartitionResolver-objektum létrehozása fázisban és a fürt kapcsolat végpontok.

## <a name="next-steps"></a>Következő lépések
* [A megbízható szolgáltatások távelérési távoli eljáráshívás](service-fabric-reliable-services-communication-remoting.md)

* [Webes API a OWIN megbízható szolgáltatások](service-fabric-reliable-services-communication-webapi.md)

* [Megbízható szolgáltatások kommunikáció biztonságossá tétele](service-fabric-reliable-services-secure-communication.md)
