<properties
    pageTitle="Oktatóanyag használ szolgáltatási Bus többi továbbítását üzenetküldés |} Microsoft Azure"
    description="Hozzon létre egy egyszerű szolgáltatás Bus továbbítási gazdaalkalmazás, amely a többi-alapú illesztő közzététele."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Szolgáltatás Bus továbbítási többi oktatóprogram

Ebben az oktatóanyagban ismerteti, hogyan hozhat létre egy egyszerű szolgáltatás Bus gazdaalkalmazás, amely a többi-alapú felhasználói felülete közzététele. TÖBBI lehetővé teszi, hogy egy webes ügyfélprogram, például egy webböngészőben, a szolgáltatás Bus API-k elérése a HTTP-kérések keresztül.

Ebben az oktatóanyagban használ szolgáltatási Bus a többi szolgáltatás összeállításához Windows Communication Foundation (WCF) többi programozási modell. Többet olvassa el a [WCF REST alkalmazásprogramozási modell](https://msdn.microsoft.com/library/bb412169.aspx) és [tervezése és a szolgáltatások végrehajtási](https://msdn.microsoft.com/library/ms729746.aspx) WCF dokumentációjában.

## <a name="step-1-create-a-service-namespace"></a>Lépés: 1: Szolgáltatás névtér létrehozása

Első lépésként névtér létrehozása, és szerezze be a megosztott Access-aláírás (Társítások) billentyűt. Névtér az alkalmazás oszlopazonosító nyújt a minden alkalmazás szolgáltatás Bus keresztül elérhetővé tett. Társítások kulcs automatikusan generált, szolgáltatás névteret létrehozásakor. Szolgáltatás névtere, ha Társítások kulcs itt szolgáltatás Bus hitelesítést végezni az access, az alkalmazás hitelesítő adatait.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Lépés: 2: Definiálása szolgáltatás Bus használata WCF többi-alapú szolgáltatási szerződés

Szerint más szolgáltatás Bus szolgáltatásokkal, a többi stílusú szolgáltatás létrehozásakor meg kell határoznia a szerződést. A szerződés megadja a host támogatja műveleteket. A gondolatot szolgáltatási művelettel webes szolgáltatás szolgál. Szerződéseket C++, C# vagy Visual Basic felhasználói felülete megadásával jönnek létre. Mindegyik módszernek felületén megfelel egy adott szolgáltatás műveletet. A [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumot minden kapcsolat kell alkalmazni, és a [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútumot minden műveletet kell alkalmazni. A módszer, amelynek a [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) felületet nincs a [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), ha ez a módszer nem megjelenő. Az alábbi műveleteket kódjába jelenik meg a következő eljárás példában.

Egy egyszerű szolgáltatás Bus szerződést, és a többi stílusú szerződés közötti elsődleges különbség a [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)tulajdonság hozzáadása: [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Ez a tulajdonság lehetővé teszi a felületén a módszer feleltesse meg a másik oldalon a felület módszer. Ebben az esetben fogjuk használni [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) HTTP GET módszer hivatkozni szeretne. Ez a szolgáltatás Bus pontosan beolvasásához, és a felület küldött parancsok értelmezése lehetővé teszi.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>A szolgáltatás Bus szerződés létrehozását felületet

1. Nyissa meg a Visual Studio rendszergazdaként: kattintson a jobb gombbal a **Start** menüben a program, és kattintson a **Futtatás rendszergazdaként**.

2. Konzol alkalmazás új projekt létrehozása. Kattintson a **fájl** menüre, majd válassza az **Új**, jelölje be a **Projekt**. **Új projekt** párbeszédpanelen kattintson a **Visual C#**, jelölje ki a **New** -sablont, és **ImageListener**nevet. Használja az alapértelmezett **helyre**. Kattintson az **OK gombra** a projekt létrehozása.

3. C# projekt, a Visual Studio létrehoz egy `Program.cs` fájlt. Az osztály tartalmaz egy üres `Main()` módszer, konzol alkalmazás projekt össze helyesen az szükséges.

4. Szolgáltatás Bus és **System.ServiceModel.dll** mutató hivatkozások hozzáadása a projekthez a szolgáltatás Bus NuGet csomag telepítésével. A csomag automatikusan hozzáadja a szolgáltatás Bus tárak, valamint a WCF **System.ServiceModel**mutató hivatkozásokat. Kattintson a jobb gombbal a **ImageListener** projekt megoldás Explorerben, és válassza a **NuGet csomagok kezelése**. Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

5. Hozzá kell adnia kifejezetten **System.ServiceModel.Web.dll** hivatkozást a projekthez:

    egy. A megoldás Intézőben kattintson a jobb gombbal a projekt **hivatkozások** mappát, és kattintson a **Hivatkozás hozzáadása**gombra.

    b. **Hivatkozás hozzáadása** párbeszédpanelen kattintson a **keret** fülre a bal oldalon, és a **Keresés** mezőbe, írja be a **System.ServiceModel.Web**. Jelölje be a **System.ServiceModel.Web** jelölőnégyzetet, majd kattintson az **OK gombra**.

6. Adja hozzá a következő `using` kimutatások Program.cs fájl tetején.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) a névtér, amely lehetővé teszi, hogy a programból való elérését WCF főbb tulajdonságait. Szolgáltatás Bus számos az objektumok és attribútumok a WCF meghatározására használ szolgáltatási szerződés. A legtöbb szolgáltatás Bus továbbítási alkalmazásai a névtér fogja használni. Hasonlóképpen [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) segítségével határozza meg a csatornát, amely az objektumot, amelyen keresztül kommunikáció a szolgáltatás Bus és az ügyfél webböngészőben. Végül [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) tartalmaz típusát, amely lehetővé teszi a webalkalmazások létrehozására.

7. Nevezze át a `ImageListener` **Microsoft.ServiceBus.Samples**névtér.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Közvetlenül a nyitó kapcsos zárójel a névtér nyilatkozat megadása után új felület **IImageContract** nevű és a **ServiceContractAttribute** attribútum alkalmazása a felület érték `http://samples.microsoft.com/ServiceModel/Relay/`. A névtér érték a teljes körét a kódot használó névtér különböznek egymástól. A névtér értéket ezt a szerződést szolgál egy egyedi azonosítót, és a vonatkozó információk kell rendelkeznie. További tudnivalókért olvassa el a [Szolgáltatás verziószámozás](http://go.microsoft.com/fwlink/?LinkID=180498)című témakört. A névtér kifejezetten megadása megakadályozza, hogy az alapértelmezett névtér értéket kell a Szerződés neve.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Belül a `IImageContract` felület, egy módszer a egyetlen művelet deklarálása a `IImageContract` szerződés felületén közzététele, és alkalmazza a `OperationContractAttribute` a módszerrel a nyilvános Bus szolgáltatási szerződés részeként elérhetővé tenni kívánt attribútum.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. A **OperationContract** attribútum adja hozzá a **WebGet** értéket.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Ha így lehetővé teszi, hogy szolgáltatás Bus HTTP GET kérelmek `GetImage`, és az eredményül adott értékek a fordítandó `GetImage` be egy HTTP GETRESPONSE választ. Az oktatóprogram a fogja használni a webböngészőben, ez a módszer eléréséhez, és a kép megjelenítése a böngészőben.

11. Közvetlenül a után a `IImageContract` definícióját, mindkét öröklő csatorna deklarálása a `IImageContract` és `IClientChannel` felületek.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    A csatorna keresztül, amely a szolgáltatás és az ügyfél adják át az információkat egymáshoz a WCF-objektum. A csatorna később a gazdaalkalmazás hoz létre. Szolgáltatás Bus ezután használja a csatorna átadni a HTTP GET kérelmek a böngészőből **GetImage** Önnél. Szolgáltatás Bus is használja a csatorna **GetImage** visszatérési értéke készíthet, és az ügyfél böngésző-HTTP GETRESPONSE lefordítása azt.

12. A **Szerkesztés** menüben kattintson a **Megoldás összeállítása** a munka pontosságát eddig megerősítéséhez.

### <a name="example"></a>Példa

A következő kódot, amely definiálja Bus szolgáltatási szerződés egyszerű felhasználói felülete jeleníti meg.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Lépés 3: Megvalósítása szolgáltatás Bus használata WCF többi-alapú szolgáltatási szerződés

A többi stílusú szolgáltatás Bus szolgáltatás létrehozásához a szerződést, amely definiálja egy csatolófelület, először létre. A következő lépés, a felület végrehajtásához. Ez magában foglalja a felhasználó által definiált **IImageContract** felületet **ImageService** nevű osztály létrehozása. A szerződést, végrehajtása után kattintson a felület-App.config fájl használatával állítsa be. A konfigurációs fájl szükséges adatait az alkalmazást, például a szolgáltatás neve, a szerződést, és a protokoll használatával kommunikál a szolgáltatás Bus használt típusú nevét tartalmazza. Az alábbi műveleteket kódjába eljárással példában megadva.

Az előző lépéseket, és nincs nagyon kevés különbsége a többi stílusú szerződést, és egy egyszerű Bus szolgáltatási szerződés végrehajtása.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>A többi stílusú szolgáltatás Bus szerződés végrehajtásához

1. Hozzon létre egy új osztály **ImageService** elnevezve közvetlenül a **IImageContract** felület definícióját. A **ImageService** osztály a **IImageContract** felület hajtja végre.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Más felület megvalósítás hasonlóan alkalmazhat a definíció egy másik fájlban. Jó helyen jár, ebben az oktatóanyagban végrehajtása jelenik meg a felület definíció ugyanabban a fájlban és `Main()` módot.

2. A [ServiceBehaviorAttribute attribútum](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) attribútum jelzi, hogy az osztály-a WCF-szerződés végrehajtásának **IImageService** osztály vonatkoznak.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    A korábban említett, a névtér nem hagyományos névteret. Ehelyett része a WCF-architektúra, amely azonosítja a szerződést. További információt a WCF dokumentációjában [Adatok szerződés nevek](https://msdn.microsoft.com/library/ms731045.aspx) témakörében talál.

3. A .jpg kép hozzáadása a projekthez.  

    Ez a kép, amely a szolgáltatást a fogadó böngészőben jeleníti meg. Kattintson a jobb gombbal a projektet, majd kattintson a **Hozzáadás**gombra. Válassza a **Meglévő elemet**. Használja a **Meglévő elem hozzáadása** párbeszédpanelen tallózással keresse meg a megfelelő .jpg, és kattintson a **Hozzáadás**gombra.

    Fel a fájlt, győződjön meg arról, hogy **Az összes fájl** be van jelölve a legördülő lista mellett a **fájlnév:** mezőben. Az oktatóprogram hátralevő részében azt feltételezi, hogy a kép nevével "kép.jpg". Ha a másik fájlra, ki kell nevezze át a képet, vagy módosítsa a kódot egyenlíti.

4. Győződjön meg arról, hogy a futó szolgáltatás megtalálhatja a képfájlt, **Megoldás Explorerben** kattintson a jobb gombbal a képfájlt, majd kattintson a **Tulajdonságok**gombra. A **Tulajdonságok** ablaktáblában állítsa **kimeneti könyvtár másolása** **Ha újabb példány**.

5. A **System.Drawing.dll** összeállítás mutató hivatkozás hozzáadása a projekthez, és is hozzáadhat a következő tartozó `using` kimutatások.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Vegye fel a következő konstruktor bitkép betöltő, és el kívánja küldeni az ügyfél böngészője előkészíti a **ImageService** osztály.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Közvetlenül az előző kód után adja hozzá a következő **GetImage** módszer a **ImageService** osztály való visszatéréshez a képet tartalmazó HTTP üzenetbe.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Ez a megvalósítás **MemoryStream** a kép beolvasása és előkészítése a böngésző streaming használja. Azt elindul, nullánál adatfolyam helyét, a adatfolyam tartalom jpeg deklarálása és adatfolyamként továbbítja az adatokat.

8. A **Szerkesztés** menüben kattintson a **Megoldás összeállítása**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>A konfiguráció esetében a webszolgáltatás használ szolgáltatási Bus meghatározása

1. Kattintson duplán a **Megoldást Explorer** **App.config** nyissa meg a Visual Studio-szerkesztőben.

    A **App.config** fájl hasonló, akkor a WCF-konfigurációs fájl, és a szolgáltatás neve, a végpontok (Ez azt jelenti, hogy a hely szolgáltatás Bus ügyfelek, valamint a hosts egymással közzététele) és a kötés tartalmazza (a kommunikáció használt protokoll típusa). A fő különbség, hogy a beállított szolgáltatás végpontjának [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) kötés, amely nem része a .NET-keretrendszer hivatkozik.

2. A `<system.serviceModel>` XML-elem a WCF-elem, amely definiálja egy vagy több szolgáltatást. Ebben az esetben segítségével adja meg a szolgáltatás neve és a végpontot. Alján a `<system.serviceModel>` elem (, de továbbra is belül `<system.serviceModel>`), adja hozzá a `<bindings>` elemet, amelyet a következő tartalommal rendelkezik. Az alkalmazásban használt kötések határozza meg. Több kötések határozhatja meg, de ebben az oktatóprogramban az definiálandó csak az egyik.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Ebben a lépésben szolgáltatás Bus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) kötést **relayClientAuthenticationType** **nincs**értékre van állítva az határozza meg. Ez a beállítás azt jelzi, használja a kötés zárólap nincs szükség egy ügyfél hitelesítő.

3. Után a `<bindings>` elem hozzáadása egy `<services>` elemet. A kötések hasonlóan definiálhatók több szolgáltatás egyetlen kereséskonfigurációs fájlban. Jó helyen jár ebben az oktatóanyagban definiálni csak egyet.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Ezt a lépést, amely a korábban beállított alapértelmezett **webHttpRelayBinding**használja a szolgáltatás konfigurálása Is használja az alapértelmezett **sbTokenProvider**, amely a következő lépés van meghatározva.

4. Után a `<services>` elemet, hozzon létre egy `<behaviors>` "SAS_KEY" helyettesítve a *Megosztott Access-aláírás* (Társítások) billentyűt az [Azure portál][]kapott, a következő tartalommal elemet.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. A App.config, még a `<appSettings>` elem, a teljes kapcsolatot a korábban kapott a portálról kapcsolattal karakterláncértéket csere. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. A **Szerkesztés** menüben kattintson a **Megoldás összeállítása** a teljes megoldás.

### <a name="example"></a>Példa

A következő kódrészlet egy többi alapú szolgáltatást futtató szolgáltatás Bus a **WebHttpRelayBinding** kötés segítségével szerződés és szolgáltatás végrehajtására jeleníti meg.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

A következő példa bemutatja a App.config fájlt, a szolgáltatásokhoz kapcsolódó.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Lépés: 4: Tárolni a többi-alapú WCF szolgáltatás Bus szolgáltatás használata

Ebben a lépésben ismerteti, hogyan konzol alkalmazást használ szolgáltatási Bus a webszolgáltatás futtatásához. A példában az eljárást követve megadva az ebben a lépésben megírt kód teljes listája.

### <a name="to-create-a-base-address-for-the-service"></a>A szolgáltatás alapcím létrehozása

1. Az a `Main()` deklaráció működik, a változó tárolásához a névtér a szolgáltatás Bus projekt létrehozása. Győződjön meg arról, hogy le szeretné cserélni `yourNamespace` az imént létrehozott szolgáltatás névtere a nevet.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Bus szolgáltatás hozzon létre egy egyedi URI a névtér nevét használja.

2. Hozzon létre egy `Uri` alapcímének példány a szolgáltatás, amely a névtér alapul.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Hozhat létre, és állítsa be a webes szolgáltatás állomáswebhelyén

- Hozzon létre a webes szolgáltatás host, ebben a szakaszban korábban létrehozott URI-címet használja.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    A szolgáltatás állomás a gazdaalkalmazás elindítja a WCF-objektumot. Ez a példa átadja a host szeretne létrehozni (egy **ImageService**), valamint a címet, amelyre meg szeretné elérhetővé tenni a gazdaalkalmazás típusát.

### <a name="to-run-the-web-service-host"></a>A web service host futtatása

1. Nyissa meg azt a szolgáltatást.

    ```
    host.Open();
    ```
    Most már fut a szolgáltatás.

2. Egy üzenet jelzi, hogy fut-e a szolgáltatást, és hogyan szüntetheti meg a szolgáltatás jeleníti meg.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Ha elkészült, zárja be a szolgáltatás host.

    ```
    host.Close();
    ```

## <a name="example"></a>Példa

A következő példa tartalmazza a szolgáltatási szerződés és a fenti lépések végrehajtása az oktatóprogram, és egy konzol alkalmazásban a szolgáltatás üzemelteti. A következő kódot fordítási ImageListener.exe nevű végrehajtható be.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>A kód lefordítása

A megoldás kiépítése, után végezze el az alkalmazás futtatásához a következő:

1. Nyomja le az **F5 billentyűt**, vagy tallózással keresse meg a végrehajtható fájl helyét (ImageListener\bin\Debug\ImageListener.exe), futtassa a szolgáltatást. Fontos az alkalmazás fut, a következő lépés végrehajtásához szükséges.

2. Másolja és illessze be a címet a parancssorból jelenik meg a kép a böngésző címsorába.

3. Ha befejezte, nyomja le az **ENTER billentyűt** a parancssorablakban bezárja az alkalmazást.

## <a name="next-steps"></a>Következő lépések

Most, hogy a szolgáltatás Bus továbbítási szolgáltatáshoz használó alkalmazás már készített, talál bővebb információkat az üzenetek továbbítását az alábbi cikkekben:

- [Azure Service Bus építészeti – áttekintés](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [A szolgáltatás Bus továbbítási szolgáltatáshoz használata](service-bus-dotnet-how-to-use-relay.md)

[Azure portál]: https://portal.azure.com