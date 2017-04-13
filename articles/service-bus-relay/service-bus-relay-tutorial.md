<properties 
    pageTitle="Szolgáltatás Bus továbbítási oktatóprogram |} Microsoft Azure"
    description="Hozzon létre egy szolgáltatás Bus ügyfél, alkalmazás vagy szolgáltatás használ szolgáltatási Bus továbbítási."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Szolgáltatás Bus továbbítási oktatóprogram

Ebben az oktatóanyagban ismerteti, hogyan hozhat létre egy egyszerű szolgáltatás Bus ügyfélalkalmazás és a szolgáltatás Bus "továbbítási" lehetőségeivel szolgáltatás. A megfelelő oktatóanyagban, amelyet használ szolgáltatási Bus [brokered üzenetküldés](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)a [Szolgáltatás Bus Brokered üzenetküldés .NET oktatóprogram](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md)című témakör tartalmaz.

Ebben az oktatóanyagban keresztül végezhető műveletek lehetővé teszi a szolgáltatás Bus ügyfél- és -alkalmazások létrehozásához szükséges lépések megértéséhez. A WCF mint például a szolgáltatás olyan olyan elemet, amely egy vagy több végpontok közzététele, amelyek közzététele egy vagy több szolgáltatás műveletek. A szolgáltatás végpontjának-címét, hol találhatók a szolgáltatást, egy ügyfél kell a szolgáltatást, és a szerződést, amely definiálja az ügyfeleknek a szolgáltatás által biztosított funkcionalitás kommunikálni információkat tartalmazó kötést határozza meg. Fő a WCF és szolgáltatás Bus szolgáltatás közötti különbség, hogy a végpont a felhőben, hanem a számítógépen helyben van téve.

Ebben az oktatóanyagban keresztül témakörök sorozatában dolgozik, után lesz egy futó szolgáltatás, és egy ügyfél, a műveletek a szolgáltatás hívhatja. Az első témakör ismerteti, hogyan állíthat be egy fiókot. A következő lépések ismertetik definiálásáról a szerződést használó szolgáltatások, a szolgáltatás megvalósításáról és a szolgáltatás konfigurálása a kódot. Azok is ismertetik, hogyan lehet a szolgáltató, és futtassa a szolgáltatást. A létrehozott szolgáltatást önálló szolgáltatott és ugyanazon a számítógépen futó az ügyfél és a szolgáltatás. Beállíthatja a szolgáltatás kódot vagy a konfigurációs fájl használatával.

Az utolsó három lépést ismertetik, hogyan lehet az ügyfél-alkalmazás létrehozása, állítsa be az ügyfélalkalmazás, és hozzon létre és egy ügyfél, a funkciók a fogadó is elérhető.

Az összes ebben a szakaszban hivatkozott témakörök tartalmaznak feltételezik, hogy a fejlesztői környezet Visual Studio használ.

## <a name="sign-up-for-an-account"></a>A fiókjához regisztráció

Első lépésként névtér létrehozása, és szerezze be a megosztott Access-aláírás (Társítások) billentyűt. Névtér az alkalmazás oszlopazonosító nyújt a minden alkalmazás szolgáltatás Bus keresztül elérhetővé tett. Társítások kulcs automatikusan generált, szolgáltatás névteret létrehozásakor. Szolgáltatás névtere, ha Társítások kulcs itt szolgáltatás Bus hitelesítést végezni az access, az alkalmazás hitelesítő adatait.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>WCF szolgáltatási szerződés szolgáltatás Bus használata meghatározása

A szolgáltatási szerződés Itt adhatja meg, hogy milyen műveleteket (a Web service terminológia módszerek vagy függvények) a szolgáltatás lehetővé teszi. Szerződéseket C++, C# vagy Visual Basic felhasználói felülete megadásával jönnek létre. Mindegyik módszernek felületén megfelel egy adott szolgáltatás műveletet. Minden kapcsolat [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútum, és az egyes műveletek [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútum kell rendelkeznie. A módszer a [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumot tartalmazó felületet nincs a [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútum, ha ez a módszer nem megjelenő. Az alábbi műveleteket kódját a példában az eljárást követve megadva. A szerződési és -szolgáltatások nagyobb vitafórum lásd: a [tervezése és a szolgáltatások végrehajtási](https://msdn.microsoft.com/library/ms729746.aspx) WCF dokumentációjában.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>A szolgáltatás Bus szerződés létrehozását felületet

1. Kattintson a jobb gombbal a **Start** menüben a program, és válassza a **Futtatás rendszergazdaként**nyissa meg a Visual Studio rendszergazdaként.

2. Konzol alkalmazás új projekt létrehozása. Kattintson a **fájl** menüre, majd válassza az **Új** **Projekt**lehetőségre. Az **Új projekt** párbeszédpanelen kattintson **Visual C#** (Ha **Visual C#** nem jelenik meg, keresse a **Más nyelven**). Kattintson a **New** sablonra, és **EchoService**nevet. Kattintson az **OK gombra** a projekt létrehozása.

    ![][2]

3. Telepítse a Service Bus NuGet csomagot. A csomag automatikusan hozzáadja a szolgáltatás Bus tárak, valamint a WCF **System.ServiceModel**mutató hivatkozásokat. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) a névtér, amely lehetővé teszi a WCF főbb tulajdonságait programozás útján történő eléréséhez. Szolgáltatás Bus számos az objektumok és attribútumok a WCF meghatározására használ szolgáltatási szerződés.

    A megoldás Intézőben kattintson a jobb gombbal a megoldást, és kattintson a **Megoldás NuGet csomagok kezelése**. Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Győződjön meg arról, hogy a projekt nevét a **változatokat** mezőben van-e jelölve. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

    ![][3]

3. A megoldás Intézőben a Program.cs fájlra duplán kattintva nyissa meg azt a szerkesztő, ha azt még nem nyílt meg.

4. Adja hozzá a következő utasítások segítségével a fájl tetején:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Az alapértelmezett nevének **EchoService** való **Microsoft.ServiceBus.Samples**névtér nevének módosítása

    >[AZURE.IMPORTANT] Ebben az oktatóanyagban használja, a C# névtér **Microsoft.ServiceBus.Samples**, amely a névtér az [a WCF-ügyfél konfigurálása](#configure-the-wcf-client) lépésben a konfigurációs fájl használatban van felügyelt szerződés típusú. Megadhatja, hogy minden névtér azt szeretné, ha ez a példa; generál Az oktatóprogram jó helyen jár, kivéve, ha majd módosítsa a névtér a szerződés és az alkalmazás konfigurációs fájl ennek megfelelően a szolgáltatás nem működik. A névtér az App.config fájl a megadott ugyanaz, mint a C# fájlok megadott névteret kell lennie.

1. Közvetlenül után a `Microsoft.ServiceBus.Samples` névtér-deklaráció belül a névtér, definiálása nevű új kapcsolat, de `IEchoContract` , és alkalmazza a `ServiceContractAttribute` attribútum a felület **http://samples.microsoft.com/ServiceModel/Relay/**névtér érték. A névtér érték a teljes körét a kódot használó névtér különböznek egymástól. Ehelyett a névtér értékét használja egy egyedi azonosítót, a jelen szerződés. A névtér kifejezetten megadása megakadályozza, hogy az alapértelmezett névtér értéket kell a Szerződés neve.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Általában csak a szolgáltatási szerződés névtér verziója információkat tartalmazó elnevezési séma tartalmazza. A szolgáltatási szerződés névtér verziója adataival lehetővé teszi, hogy megjelent fontosabb változásokról elkülönítése által definiálása az új névtér az új szolgáltatási szerződés, és adjon meg azt az új végpont a szolgáltatásokat. Ezzel a módszerrel ügyfelek továbbra is használhatja a régi szolgáltatási szerződés anélkül, hogy frissülnek. A verzióadatok dátum vagy összeállítás szám állhat. További tudnivalókért olvassa el a [Szolgáltatás verziószámozás](http://go.microsoft.com/fwlink/?LinkID=180498)című témakört. Ebben az oktatóanyagban alkalmazásában a szolgáltatási szerződés névtér az elnevezési séma nem tartalmaz vonatkozó információk.

1. Belül a `IEchoContract` felület, egy módszer a egyetlen művelet deklarálása a `IEchoContract` szerződés felületén közzététele, és alkalmazza a `OperationContractAttribute` a módszerrel a nyilvános Bus szolgáltatási szerződés részeként elérhetővé tenni kívánt attribútum.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Közvetlenül után a `IEchoContract` definition felület, mindkét öröklő csatorna deklarálhatnak `IEchoContract` és is a `IClientChannel` felület, mint az itt látható:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Csatorna az a WCF-objektum, amelyen keresztül, a host és az ügyfél át információkat egymáshoz. Később akkor fog kódírás szemben a csatorna visszhangja a két alkalmazás között.

1. A **Szerkesztés** menüben kattintson a **Megoldás létrehozása** , vagy nyomja le a **Ctrl + Shift + B** munkája pontosságát eddig megerősítéséhez.

### <a name="example"></a>Példa

A következő kódot, amely definiálja Bus szolgáltatási szerződés egyszerű felhasználói felülete jeleníti meg.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Most, hogy a kapcsolat létrejött, a felület alkalmazhat.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>WCF szerződés szolgáltatás Bus használandó végrehajtása

A szolgáltatás Bus továbbítási létrehozásához a szerződést, amely definiálja egy csatolófelület, először létre. A felület létrehozásával kapcsolatos további tudnivalókért lásd: az előző lépést. A következő lépés, a felület végrehajtásához. Ez a jár nevű osztály létrehozása `EchoService` , alkalmazza a felhasználó által definiált `IEchoContract` felület. A felület, végrehajtása után kattintson a felület App.config konfigurációs fájl használatával állítsa be. A konfigurációs fájl szükséges adatait az alkalmazást, például a szolgáltatás neve, a szerződést, és a protokoll használatával kommunikál a szolgáltatás Bus használt típusú nevét tartalmazza. Az alábbi műveleteket kódjába eljárással példában megadva. Az általánosabb vitafórum arról, hogy miként tudnak megvalósítani a szolgáltatási szerződés lásd: a [Végrehajtási szolgáltatási szerződés](https://msdn.microsoft.com/library/ms733764.aspx) WCF dokumentációjában.

1. Hozzon létre egy új osztály nevű `EchoService` definícióját után közvetlenül a `IEchoContract` felület. A `EchoService` eszközök osztály a `IEchoContract` felület. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Más felület megvalósítás hasonlóan alkalmazhat a definíció egy másik fájlban. Azonban az ebben az oktatóanyagban végrehajtása található a ugyanannak a fájlnak a felület-definícióját és a `Main` módot.

1. A [ServiceBehaviorAttribute attribútum](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribútum szeretné alkalmazni a `IEchoContract` felület. Az attribútum határozza meg, a szolgáltatás neve és a névtér. Ehhez a művelethez, miután a `EchoService` osztály a következőképpen jelenik meg:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Megvalósítása a `Echo` meghatározott módszer a `IEchoContract` a kapcsolat a `EchoService` osztály. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Kattintson a **Szerkesztés**gombra, majd kattintson a **Megoldás összeállítása** a munka pontosságát megerősítéséhez.

### <a name="to-define-the-configuration-for-the-service-host"></a>Az adatokat szolgáltató állomáson meghatározása

1. A beállítási fájl nagyon hasonlít a WCF-konfigurációs fájl. A szolgáltatás neve, végpont (Ez azt jelenti, hogy a hely szolgáltatás Bus ügyfelek, valamint a hosts egymással közzététele) és a kötést tartalmazza (a kommunikáció használt protokoll típusa). A fő különbség, hogy a beállított szolgáltatás végpontjának [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) kötés, amely nem része a .NET-keretrendszer hivatkozik. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) egyike, a szolgáltatás Bus által meghatározott kötések.

1. A **Megoldás Explorer**, a App.config fájlra duplán kattintva nyissa meg a Visual Studio-szerkesztőben.

2. Az a `<appSettings>` elem, a csere a helyőrzőket, a nevét, valamint a szolgáltatás névtere a Társítások főbb, hogy lépésben a vágólapra másolt egy korábbi. 

1. Belül a `<system.serviceModel>` címkék hozzáadása egy `<services>` elemet. Több szolgáltatás Bus alkalmazást egyetlen konfigurációs fájl határozhatja meg. Ebben az oktatóanyagban azonban csak az egyik megadja.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Belül a `<services>` elem hozzáadása egy `<service>` határozza meg a szolgáltatás neve elemet.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Belül a `<service>` elem, a végpont szerződés, valamint a kötelező a végpont típusú helyének megadása.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    A végpont határozza meg, ahol az ügyfelet a gazdaalkalmazás fog keresni. Később az oktatóprogram használja ezt a lépést URI, amely elérhetővé teszi a fogadó keresztül szolgáltatás Bus létrehozásához. A kötést deklarálása, hogy azt használja a TCP protokoll használatával kommunikál a szolgáltatás Bus.

1. A **Szerkesztés** menüben kattintson a **Megoldás összeállítása** a munka pontosságát megerősítéséhez.

### <a name="example"></a>Példa

A következő kódot jeleníti meg a szolgáltatási szerződés végrehajtását.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

A következő kódot jeleníti meg a szolgáltatási állomással tartozó App.config fájl egyszerű formátumát.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Host, és egy egyszerű webszolgáltatás regisztrálni a szolgáltatás Bus futtatása

Ebben a lépésben ismerteti, hogyan tehető függővé egy egyszerű szolgáltatás Bus szolgáltatás.

### <a name="to-create-the-service-bus-credentials"></a>A szolgáltatás Bus hitelesítő adatok létrehozása

1. A `Main()`, hozzon létre két változó, amelyben tárolni a névtér, és a Társítások főbb, amely a konzol ablakból olvasnak.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    A Társítások billentyűt a szolgáltatás Bus projekt eléréséhez később lesz. A névtér az paraméterként átadott `CreateServiceUri` URI szolgáltatás létrehozásához.

4. Egy [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) objektum használata esetén, az elemeket rekordként, hogy fogja használni, akkor a Társítások kulcs hitelesítő adatok típusát. Adja hozzá a következő kódot közvetlenül a kódot hozzáadni az utolsó lépésben után.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>A szolgáltatás alapcím létrehozása

1. A felvett az utolsó lépésben kódot követő létrehozása egy `Uri` alapcímének példány a szolgáltatás. A URI adja meg a szolgáltatás Bus rendszer, a névtér és elérési útját a felületet.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" a szolgáltatás Bus séma rövidített, és jelzi, hogy azt használja a TCP protokolljaként. Ez volt korábban is megjelölt a konfigurációs fájl [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) megadva, a kötést.
    
    Ebben az oktatóprogramban az URI van `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Hozhat létre, és állítsa be a szolgáltatás állomáswebhelyén

1. Csatlakozási mód beállításához `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    A csatlakozási mód mutatja be a protokoll használatával kommunikál a szolgáltatás Bus; használja a szolgáltatás HTTP vagy TCP. Az alapértelmezett beállítás használata `AutoDetect`, a szolgáltatás próbál csatlakozni szolgáltatás Bus TCP, ha az elérhető, és a HTTP Ha TCP nem érhető el. Ügyfél-kommunikációhoz, hogy ez különbözik a protokoll a szolgáltatás Megjegyzés: Itt adhatja meg. A protocol használt kötés határozza meg. A szolgáltatás például a [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) kötés, amely meghatározza, hogy (a szolgáltatás Bus elérhetővé tett) végpontját kommunikál ügyfelek HTTP használhatja. Azonos szolgáltatás **ConnectivityMode.AutoDetect** megadhatja, hogy a szolgáltatás kommunikál szolgáltatás Bus TCP felett.

1. Az ebben a szakaszban korábban létrehozott URI használatával, a szolgáltató állomáson létrehozása.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    A szolgáltatás állomás elindítja a szolgáltatást a WCF-objektumot. Ebben az esetben adja meg azt a létrehozni kívánt szolgáltatás típusa (egy `EchoService` típusa), és is a címet, amelyre, amelyre kattintva jelenítse meg a szolgáltatás.

1. A Program.cs fájl tetején [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) és [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx)mutató hivatkozások hozzáadása

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Vissza a `Main()`, állítsa be nyilvános hozzáférés engedélyezése a végpontot.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Ebben a lépésben tájékoztatja, hogy az alkalmazás is található nyilvánosan alapján vizsgálja meg a szolgáltatás Bus ATOM szolgáltatás Bus hírcsatorna beállítása a projekthez. Ha a **DiscoveryType** **magánjellegű**, egy ügyfél szeretné továbbra is a szolgáltatás eléréséhez. Azonban a szolgáltatás volna nem jelenik meg a szolgáltatás Bus névtér keresi. Ehelyett az ügyfél kellene tudja előre a végpontot elérési útját.

1. A szolgáltatás hitelesítő adatait a App.config fájlban definiált szolgáltatás végpontjait vonatkoznak:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Az előző lépésben leírtak, sikerült van deklarálva több szolgáltatások és a konfigurációs fájl lévő. Ha, a kód bejárása lenne a konfigurációs fájl- és minden végpontot, amelyre rá kell alkalmazni a hitelesítő adatok keresése. Az ebben az oktatóanyagban azonban a konfigurációs fájl csak egy végpont rendelkezik.

### <a name="to-open-the-service-host"></a>A szolgáltatás host megnyitása

1. Nyissa meg azt a szolgáltatást.

    ```
    host.Open();
    ```

1. Tájékoztassa a felhasználó, a szolgáltatás fut, és állítsa le a szolgáltatást ismertetik.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Ha elkészült, zárja be a szolgáltatás host.

    ```
    host.Close();
    ```

1. Nyomja le a **Ctrl + Shift + B** össze a projekt.

### <a name="example"></a>Példa

Az alábbi példa tartalmazza a szolgáltatási szerződés és a fenti lépések végrehajtása az oktatóprogram során, és egy konzol alkalmazásban a szolgáltatás üzemelteti. A következők EchoService.exe nevű végrehajtható össze.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Hozzon létre egy WCF-ügyfél, a szolgáltatási szerződés

A következő lépésként hozzon létre egy egyszerű szolgáltatás Bus ügyfélalkalmazás, valamint definiálhatók a szolgáltatási szerződés alkalmazhat későbbi lépéseiben. Jegyzet, hogy ezeket a lépéseket számos hasonló szolgáltatás létrehozásához használt lépéseit: szerződést, Szerkesztés egy App.config definiáló fájl hitelesítő adatokkal való csatlakozáshoz szolgáltatás Bus, és így tovább. Az alábbi műveleteket kódjába eljárással példában megadva.

1. Új projekt létrehozása az aktuális Visual Studio megoldás az ügyfél az alábbiak szerint:
    1. A megoldás Intézőben ugyanabban a megoldásban, amely tartalmazza a szolgáltatást kattintson a jobb gombbal a jelenlegi megoldást (nem a projekt), és kattintson a **Hozzáadás**gombra. Kattintson az **Új projekt**.
    2. Kattintson az **Új projekt hozzáadása** párbeszédpanel **Visual C#** (Ha **Visual C#** nem jelenik meg, keresse a **Más nyelven**), jelölje ki a **New** -sablont, és nevezze el **EchoClient**.
    3. Kattintson az **OK gombra**.
<br />

1. A megoldás Intézőben duplán a Program.cs fájlt a **EchoClient** projekt megnyitáshoz szerkesztőben, ha még nem már megnyitott.

1. A névtér neve átállítása az alapértelmezett nevének `EchoClient` való `Microsoft.ServiceBus.Samples`.

1. Telepítse a [Service Bus NuGet csomagot](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Kattintson a jobb gombbal a **EchoClient** projekt megoldás Explorerben, és válassza a **NuGet csomagok kezelése**. Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

    ![][3]

1. Adja hozzá a `using` utasítás a [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) névtér az Program.cs fájlban. 

    ```
    using System.ServiceModel;
    ```

1. A szolgáltatási szerződés definíció hozzáadása a névtér, az alábbi példában látható módon. Ne feledje, hogy ez a meghatározás a **szolgáltatás** projektben használt meghatározása azonos. Tetején kell kód hozzáadása a `Microsoft.ServiceBus.Samples` névtér.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Nyomja le a **Ctrl + Shift + B** össze az ügyfélnek.

### <a name="example"></a>Példa

A következő kódot a Program.cs fájl aktuális állapotát a EchoClient projekt jeleníti meg.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>A WCF-ügyfél konfigurálása

Ebben a lépésben hozzon létre egy egyszerű ügyfélalkalmazás a korábban létrehozott ebben az oktatóanyagban szolgáltatás hozzáférő App.config fájlként. A App.config fájl határozza meg, a szerződés, a kötést és a végpontjának neve. Az alábbi műveleteket kódjába eljárással példában megadva.

1. Megoldás Explorer a **EchoClient** projektben kattintson duplán a **App.config** kattintva nyissa meg a fájlt a Visual Studio-szerkesztőben.

2. Az a `<appSettings>` elem, a csere a helyőrzőket, a nevét, valamint a szolgáltatás névtere a Társítások főbb, hogy lépésben a vágólapra másolt egy korábbi.

1. Az system.serviceModel elemben hozzáadása egy `<client>` elemet.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Ebben a lépésben a WCF-stílus ügyfélalkalmazás definiálását deklarálása.

1. Belül a `client` elem határozza meg a nevét, szerződést, és a végpont kötés típusát.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Ebben a lépésben a végpontot, a szerződés, a szolgáltatás, és arra, hogy használ-e az ügyfélalkalmazás TCP szolgáltatás Bus kommunikálni az definiált neve határozza meg. A végpontjának neve a következő lépés ezen végpont konfigurációs URI szolgáltatással, amelyre a hivatkozás használatos.

1. Kattintson a **fájl**, majd a **Mentés**gombra.

## <a name="example"></a>Példa

A következő kódot a App.config fájlt a visszhang ügyfélprogram jeleníti meg.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>A WCF-ügyfél hívja fel a szolgáltatás Bus megvalósítása

Ebben a lépésben a szolgáltatásra, ebből az oktatóanyagból korábban létrehozott hozzáférő egyszerű ügyfélalkalmazás hajtja végre. Hasonlóan, mint a szolgáltatást, az ügyfél hajtja végre számos szolgáltatás Bus eléréséhez ugyanazokat a műveleteket:

1. A csatlakozási mód beállítása.
1. Hoz létre, amely a host szolgáltatás megkeresi a URI.
1. Határozza meg a hitelesítő adatokat.
1. A hitelesítő adatok vonatkozik a kapcsolatot.
1. Ekkor megnyílik a kapcsolatot.
1. Az alkalmazás-specifikus feladatokat hajt végre.
1. Megszünteti a kapcsolatot.

A főbb különbségek közül azonban, hogy az ügyfélalkalmazás csatorna való kapcsolódáshoz használ szolgáltatási Bus, mivel a szolgáltatás által hívást kezdeményez **ServiceHost**. Az alábbi műveleteket kódjába eljárással példában megadva.

### <a name="to-implement-a-client-application"></a>Ügyfélalkalmazás végrehajtásához

1. Csatlakozási mód beállításához **automatikus felismerése**. Adja hozzá a következő kódot belül a `Main()` **EchoClient** alkalmazása metódusát.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Adja meg a változót is tartsa lenyomva az ujját a konzolról olvasnak értékeket szolgáltatás névtere, illetve Társítások billentyűt.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. A URI, amely definiálja az állomás helyét a szolgáltatás Bus projekt létrehozása

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. A szolgáltatás névtér végpont a hitelesítőadat-objektum létrehozása.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. A konfiguráció a App.config fájlban leírt betöltő beépített csatorna létrehozása

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    A beépített csatorna, amely egy adott csatornába keresztül, amely a szolgáltatás és az ügyfél alkalmazások kommunikáció hoz létre egy WCF-objektum.

1. A szolgáltatás Bus hitelesítő adatok vonatkoznak.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Hozzon létre, és nyissa meg a csatornát, a szolgáltatás.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Írja be a egyszerű felhasználói felület és funkciók a visszhang mértékét.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Ne feledje, hogy a kód használja a csatorna objektum példányát a proxy a szolgáltatáshoz.

1. Zárja be a csatornát, és zárja be a gyár.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Az alkalmazások futtatásához

1. Nyomja le a **Ctrl + Shift + B** össze a megoldást. Ez a varázsló létrehozza az ügyfél project és a szolgáltatás project az előző lépésekben létrehozott is.

2. Mielőtt az ügyfélalkalmazás fut, meg kell győződjön meg arról, hogy fut-e a szolgáltatásalkalmazás. Megoldás Explorer a Visual Studióban kattintson a jobb gombbal a **EchoService** megoldást, majd kattintson a **Tulajdonságok**gombra.

3. Megoldás tulajdonságai párbeszédpanelen kattintson a **Projekt indítása**gombra, majd kattintson a **több indítási projekt** gombra. Ellenőrizze, hogy **EchoService** megjelenik a lista első. 

4. Állítsa a **művelet** mezőben a **EchoService** és a **EchoClient** projektekhez **indításához**.

    ![][5]

5. Kattintson a **Projekt függőségek**parancsra. A **projektek** mezőben jelölje ki a **EchoClient**. A **Jelszófüggő** mezőben ellenőrizze **EchoService** be van jelölve.

    ![][6]

6. Kattintson **az OK gombra** kattintva zárja be az a **tulajdonságokat** tartalmazó párbeszédpanel.

7. Nyomja le az **F5** mindkét projektet futtatásához.

8. Nyissa meg mindkét konzol ablak, és rákérdezzen a névtér neve. A szolgáltatás első futtatása, így **EchoService** konzol ablakban adja meg a névtér és nyomja le az **ENTER billentyűt**.

9. Ezután kéri a Társítások használatával. Írja be a Társítások billentyűt, és nyomja le az ENTER BILLENTYŰT.

    Az alábbiakban a példa eredménye a konzol ablakban. Figyelje meg, hogy az értékek megadott az alábbiakban például csak célokat.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    A szolgáltatásalkalmazás nyomtatja ki a konzol ablak a cím, amelyen figyeli, az alábbi példában látható módon.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. **EchoClient** konzol ablakban adja meg a szolgáltatásalkalmazás korábban megadott ugyanazokat az adatokat. Az előző lépésekkel adja meg az ügyfélalkalmazás azonos szolgáltatás névtere és Társítások kulcs értékeit.

11. Miután beírta a ezeket az értékeket, az ügyfél megnyílik egy csatornát, a szolgáltatás, és kéri, hogy írjon be valamilyen szöveget konzol eredménye a következő példában látható módon.

    `Enter text to echo (or [Enter] to exit):` 

    Írja be valamilyen szöveget, küldje el a szolgáltatási alkalmazást, és nyomja le az ENTER billentyűt. A szöveg küldi el a szolgáltatás a visszhang szolgáltatási művelet keresztül, és megjelenik a szolgáltatás konzol ablakban, ahogy az alábbi példa eredménye.

    `Echoing: My sample text`

    Az ügyfélalkalmazás megkapja a visszatérési értéke a `Echo` műveletet, amely az eredeti szövegre, és a konzol ablak kinyomtatja azt. Az ügyfél konzol ablakból példa eredménye a következő:

    `Server echoed: My sample text`

12. Szöveges üzenetek küldése az ügyféltől a szolgáltatás ily módon is. Ha befejezte, nyomja le az Enter az ügyfél- és konzol Windows mindkét alkalmazás befejezéséhez.

## <a name="example"></a>Példa

A következő példa bemutatja, hogyan hozhat létre egy ügyfélalkalmazás, hogy miként hívja fel a műveleteket a szolgáltatás, illetve kattintva zárja be az ügyfél, a művelet hívás befejezése után.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban mutatott, hogyan hozhat létre a szolgáltatás Bus ügyfél alkalmazás vagy szolgáltatás a szolgáltatás Bus "továbbítási" lehetőségeivel. Hasonló oktatóprogram, amelyet használ szolgáltatási Bus [üzenetküldés](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)a [Szolgáltatás Bus Brokered üzenetküldés .NET oktatóprogram](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md)című témakör tartalmaz.

További tudnivalók a szolgáltatás Bus, a következő témakörökben talál.

- [Szolgáltatás Bus üzenetküldés – áttekintés](../service-bus-messaging/service-bus-messaging-overview.md)
- [Szolgáltatás Bus alapjai](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Szolgáltatás Bus architektúra](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
