<properties
   pageTitle="Segítség a szolgáltatás háló szolgáltatások biztonságos kommunikáció |} Microsoft Azure"
   description="Segítség a biztonságos kommunikáció az Azure Service háló fürt futó megbízható szolgáltatások áttekintése."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Súgó az Azure Service háló szolgáltatások biztonságos kommunikáció

Biztonsági egyike kommunikáció egyik legfontosabb tulajdonságát. A megbízható Services alkalmazás keretrendszer néhány beépített kommunikációs készleteket és eszközök, amelyek segítségével növelje biztonságát. Ez a cikk arról, hogy miként javíthatja a biztonsági, ha használ szolgáltatási távelérési és a Windows Communication Foundation (WCF) kapcsolati Papírhalom előadás.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Biztonságossá szolgáltatás távelérési szolgáltatási használata esetén

Egy meglévő [Példa](service-fabric-reliable-services-communication-remoting.md) , amelyből megtudhatja, hogy miként állíthatja be az megbízható szolgáltatások távelérési fogjuk használni. Biztonságossá szolgáltatás távelérési szolgáltatási használata esetén, kövesse az alábbi lépéseket:

1. Hozzon létre egy csatolófelület, `IHelloWorldStateful`, amely meghatározza, hogy a módszereket, lesz elérhető a szolgáltatás a távoli eljáráshívás. A szolgáltatás által használt `FabricTransportServiceRemotingListener`, amelynek van deklarálva a `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` névtér. Ez egy `ICommunicationListener` végrehajtása távelérési funkciókat tartalmazza.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Adja hozzá figyelő beállításai és a hitelesítő adatokat.

    Győződjön meg arról, hogy a szolgáltatás kommunikációs védelmét elősegítő használni kívánt tanúsítványra telepítve van a fürt csomópontjait. Két módon, hogy figyelő beállításai és a hitelesítő adatokat biztosíthat:

    1. Adja meg közvetlenül a a szolgáltatáskód:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Adja meg a [config csomag](service-fabric-application-model.md)használatával:

        Adja hozzá a `TransportSettings` az settings.xml fájl szakaszába.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        Ebben az esetben a `CreateServiceReplicaListeners` módszer fog kinézni:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Ha felvesz egy `TransportSettings` az settings.xml fájl bármely az előtag nélkül részében `FabricTransportListenerSettings` tölti alapértelmezés szerint ebben a szakaszban lévő összes beállítást.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         Ebben az esetben a `CreateServiceReplicaListeners` módszer fog kinézni:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Amikor felhívja módszerek kattintson a védett szolgáltatás használata helyett a távelérési Papírhalom használatával a `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` hozzon létre egy proxyjának, használhatja a osztály `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. A sikeres `FabricTransportSettings`, amely tartalmaz `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Ha az ügyfél kód szolgáltatás részét képező fut, betöltése `FabricTransportSettings` settings.xml fájlból. Hozzon létre egy TransportSettings szakaszt, amely hasonlít a szolgáltatáskód korábbi látható módon. Végezze el a következő módosításokat az ügyfél kódot:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Ha az ügyfélnek nem fut a szolgáltatás részét képező, létrehozhat egy client_name.settings.xml fájl ugyanazon a helyen, ahol a client_name.exe van. Ezután hozzon létre egy TransportSettings szakaszt a fájlt.

    Hasonlóan, mint a szolgáltatást, ha felvesz egy `TransportSettings` bármilyen ügyfél settings.xml/client_name.settings.xml, az előtag nélkül szakasz `FabricTransportSettings` tölti alapértelmezés szerint ebben a szakaszban lévő összes beállítást.

    Ebben az esetben a korábbi kód még tovább egyszerűsített:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>A szolgáltatás használata esetén a WCF-alapú kommunikációs Papírhalom biztonságossá

Egy meglévő [Példa](service-fabric-reliable-services-communication-wcf.md) , amelyből megtudhatja, hogy miként állíthatja be a WCF-alapú kommunikációs Papírhalom megbízható szolgáltatások fogjuk használni. Biztonságosabbá szeretné egy szolgáltatás használata esetén a WCF-alapú kommunikációs Papírhalom, kövesse az alábbi lépéseket:

1. A szolgáltatás, frissítenie kell a WCF-kapcsolati figyelő biztonságossá (`WcfCommunicationListener`) létrehozott. Ehhez módosítása a `CreateServiceReplicaListeners` módot.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Az ügyfél a `WcfCommunicationClient` az előző [példában](service-fabric-reliable-services-communication-wcf.md) létrehozott osztályjegyzetfüzet változatlan marad. Felülbírálása van szüksége, de a `CreateClientAsync` metódusát `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Használat `SecureWcfCommunicationClientFactory` hozhat létre olyan WCF kommunikációs ügyfél (`WcfCommunicationClient`). Az ügyfél segítségével szolgáltatás módszerek meghívásához.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Következő lépések

* [Webes API a OWIN megbízható szolgáltatások](service-fabric-reliable-services-communication-webapi.md)
