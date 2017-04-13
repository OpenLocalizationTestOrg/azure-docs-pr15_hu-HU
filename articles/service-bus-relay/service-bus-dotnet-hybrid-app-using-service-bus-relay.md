<properties
    pageTitle="Hibrid a-helyszíni és felhőbeli alkalmazás (.NET) |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy .NET-a-helyszíni és felhőbeli hibrid alkalmazást, az Azure Service Bus továbbítási használatával."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET-a-helyszíni és felhőbeli hibrid alkalmazást Azure Service Bus továbbítási használatával

## <a name="introduction"></a>– Bevezetés

Ez a cikk ismerteti, hogyan hozhat létre egy hibrid felhő alkalmazást a Microsoft Azure és a Visual Studio. Az oktatóprogram tartalma feltételezi, hogy nincs semmilyen előzetes használatában Azure. 30 perc lehetősége van, több Azure az erőforrásokat használó be egy alkalmazást, és futtatása a felhőben.

Ismerkedhet meg:

-   Bemutatja, hogyan hozhat létre, vagy egy meglévő webszolgáltatás fogyasztási alkalmazkodás által egy webes megoldás.
-   Az Azure Service Bus továbbítási szolgáltatáshoz használatáról az Azure alkalmazások és webes szolgáltatás között adatokat osszanak máshol üzemeltetett.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Hogyan segít a szolgáltatás Bus továbbítási az hibrid megoldásokkal

Üzleti megoldások általában tevődnek össze saját, egyedi és új üzleti követelmények és a megoldások és rendszerek helyen már meglévő funkcióit vonatkozik megírt kód kombinációi.

Megoldás építészek indítja skála követelmények és az alsó működési költségeket könnyebben kezelésével használni a felhőben. Ezzel találják, hogy meglévő szolgáltatási eszközök értekezletállapota megjelenjen, mint az építőelemek megoldásukat belül a vállalati tűzfal és kijelentkezés – egyszerűen szeretnének elérni az access a felhőben megoldás. Több belső szolgáltatások nem épített, illetve oly módon, hogy azok is lehet egyszerűen téve a vállalati hálózathoz szélén is.

A szolgáltatás Bus továbbítási készült használatieset webszolgáltatások meglévő Windows Communication Foundation (WCF), és azokat a szolgáltatásokat biztonságosan hozzáférhetővé tétele megoldásokra, amelyek nem kell beavatkozni a cég hálózati infrastruktúrájába vállalati pereme lakik. Meglévő környezetükben belül továbbra is telepített ilyen szolgáltatás Bus továbbítási szolgáltatások, de azok delegálása a bejövő levelek figyeli munkamenetek és a felhőben tárolt szolgáltatás Bus kérelmeket. Szolgáltatás Bus is megakadályozza, hogy azokat a szolgáltatásokat az illetéktelen hozzáférés [Megosztott Access-aláírás](../service-bus-messaging/service-bus-sas-overview.md) (Társítások)-hitelesítés használatával.

## <a name="solution-scenario"></a>Megoldás eset

Ebben az oktatóanyagban létrehoz egy ASP.NET-webhelyet, amely lehetővé teszi, hogy a termék készlet lapon termékek listája látható.

![][0]

Az oktatóprogram tartalma feltételezi, hogy termékinformációk egy meglévő helyszíni rendszerben, és használja a szolgáltatás Bus továbbítási elérje a rendszer. Ez a webszolgáltatás, amely egy egyszerű konzol alkalmazásra elindul, és egy a memóriában beállítása termékek mögött van szimulált. Konzol alkalmazás futtatásához a saját számítógépen, és a webes szerepkör üzembe Azure be lesz. Ezzel a módszerrel, látni fogja hogyan a webes szerepkör az Azure adatközponthoz futó valóban visszahívja juttathatnak a számítógépre, akkor is, ha a számítógép mögött legalább egy tűzfalat, és a cím hálózati címfordító réteg majdnem biztosan elhelyezkednek.

A kezdőlap, a kész webalkalmazás képernyőképe a következő:

![][1]

## <a name="set-up-the-development-environment"></a>A fejlesztői környezet beállítása

Mielőtt elkezdené Azure-alkalmazások fejlesztésével, eszközökhöz, és állítsa be a fejlesztői környezet.

1.  Telepítse az Azure SDK .NET az [első eszközök és SDK][] lapról.

2.  Kattintson a **telepítse a SDK** használja a Visual Studio verzióját. A lépéseket, ebben az oktatóanyagban Visual Studio 2015 használja.

4.  Amikor futtatni vagy mentené a telepítő kéri, kattintson a **Futtatás**parancsra.

5.  A **Webes Platform telepítő**kattintson a **telepítés** gombra, és a telepítés folytatásához.

6.  A telepítés befejezése után, lehetősége van a szolgáltatás fejlesztését az alkalmazás indításához szükséges. A SDK tartalmaz, amelyekkel egyszerűen a Visual Studio Azure alkalmazások fejlesztői eszközökkel. Ha nincs telepítve a Visual Studio, a SDK telepíti az ingyenes Visual Studio Express.

## <a name="create-a-namespace"></a>Névtér létrehozása

Kezdje el az Azure Service Bus funkcióival, először létre kell hoznia szolgáltatás névteret. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Hozzon létre egy helyszíni kiszolgálót

Első lépésként fog (mock) a helyszíni termék katalógus rendszer generál. Legyen meglehetősen egyszerű; egy teljes szolgáltatás felülettel, amely azt próbálja integráció egy helyszíni tényleges-katalógus rendszer képviselik láthatja.

Ehhez a projekthez egy Visual Studio konzol alkalmazást, és az [Azure Service Bus NuGet csomagot](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) használ a szolgáltatás Bus tárak és a konfigurációs beállítások.

### <a name="create-the-project"></a>A projekt létrehozása

1.  Rendszergazdai jogosultságokkal használatával indítsa el a Microsoft Visual Studio. Rendszergazdai jogosultságokkal a Visual Studio indításához kattintson a jobb gombbal a **Visual Studio** program ikonjára, és kattintson a **Futtatás rendszergazdaként**.

2.  A Visual Studióban, a **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.

3.  **Telepített sablonok** **Visual C#**, kattintson a **New**. A **név** mezőbe írja be a nevét, **ProductsServer**:

    ![][11]

4.  Kattintson az **OK gombra** a **ProductsServer** projektet létrehozni.

7.  Ha már telepítette a Visual Studio csomag NuGet menedzserének, ugorja át a következő lépéssel. Látogasson el a [NuGet][] , és kattintson a [Telepítés NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). A megjelenő utasításokat követve telepítse a NuGet csomag manager, majd indítsa újra a Visual Studio.

7.  Megoldás Explorerben kattintson a jobb gombbal a **ProductsServer** projektet, majd **NuGet csomagok kezelése**gombra.

8.  Kattintson a **Tallózás** fülre, majd keressen `Microsoft Azure Service Bus`. Kattintson a **telepítés**gombra, és fogadja el a használati feltételeket.

    ![][13]

    Figyelje meg, hogy a szükséges ügyfélprogram szerelvények most hivatkozunk.

9.  Adja hozzá a termék szerződés új osztály. A megoldás Intézőben kattintson a jobb gombbal a **ProductsServer** projekt, és kattintson a **Hozzáadás**gombra, és kattintson az **Osztályjegyzetfüzet**.

10. A **név** mezőbe írja be a nevét, **ProductsContract.cs**. Kattintson a **Hozzáadás**gombra.

11. Cserélje ki a névtér definíció **ProductsContract.cs**, a következő kódot, amely meghatározza a szolgáltatás a szerződést.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Cserélje ki a névtér definíció Program.cs, a következő kódot, amely a felhasználóiprofil-szolgáltatás és a hozzá tartozó host.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. A megoldás Intézőben és a **App.config** fájlra duplán kattintva nyissa meg a Visual Studio-szerkesztőben. A **alján &lt;rendszer. ServiceModel&gt; ** elem (de még mindig belül &lt;rendszer. ServiceModel&gt;), az alábbi XML-kódot. Ügyeljen arra, a nevét, valamint a névtér, *yourKey* a korábban a portálról lekért Társítások kulccsal *yourServiceNamespace* lecserélése:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. A App.config, még a ** &lt;appSettings&gt; ** elem, a csere a csatlakozási karakterlánc értéke a korábban kapott a portálról kapcsolattal. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Nyomja le a **Ctrl + Shift + B** , vagy a **Szerkesztés** menüben kattintson a **Szerkesztés megoldás** az alkalmazás összeállítása és az eddig a munka pontosságának ellenőrzése.

## <a name="create-an-aspnet-application"></a>ASP.NET-alkalmazás létrehozása

Ebben a részben egy egyszerű ASP.NET-alkalmazás, amely megjeleníti az adatok beolvasása a termék szolgáltatás rendszer generál.

### <a name="create-the-project"></a>A projekt létrehozása

1.  Győződjön meg arról, hogy a Visual Studio fut-e rendszergazdai jogosultságokkal.

2.  A Visual Studióban, a **fájl** menüben kattintson az **Új**gombra, és kattintson a **Projekt**.

3.  **Telepített sablonok** **Visual C#**, kattintson az **ASP.NET webalkalmazást**. A projekt **ProductsPortal**neve. Kattintson **az OK**gombra.

    ![][15]

4.  A **sablon kiválasztása** listában kattintson a **MVC**parancsra. 

6.  Jelölje be a szolgáltatója **a felhőben**.

    ![][16]

5. Kattintson a **Módosítás hitelesítési** gombra. **Hitelesítés módosítása** párbeszédpanelen válassza a **Nincs hitelesítés**, és kattintson **az OK**gombra. Ebben az oktatóprogramban az alkalmazás, nem egy felhasználónak bejelentkezés esetén üzembe helyezése.

    ![][18]

6.  Az **Új ASP.NET projekt** párbeszédpanel a **Microsoft Azure** csoportban győződjön meg arról, **a felhőben szolgáltató** ki, és hogy **Alkalmazás szolgáltatás** be van jelölve a legördülő listában.

    ![][19]

7. Kattintson az **OK gombra**. 

8. Ekkor meg kell adnia egy új webalkalmazás Azure erőforrásokat. Kövesse az összes [egy új webalkalmazás konfigurálása Azure források](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app)szakaszában. Ezután térjen vissza az oktatóanyag, és folytassa a következő lépéssel.

5.  A megoldás Intézőben kattintson a jobb gombbal a **modellek** majd kattintson a **Hozzáadás**gombra, majd kattintson **osztály**. A **név** mezőbe írja be a nevét, **Product.cs**. Kattintson a **Hozzáadás**gombra.

    ![][17]

### <a name="modify-the-web-application"></a>A webes alkalmazás módosítása

1.  A Visual Studio Product.cs fájlt az alábbi kódot cserélje ki a meglévő névtér-definíciót.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Megoldás Explorerben bontsa ki a **vezérlők** mappát, majd a **HomeController.cs** fájlra duplán kattintva nyissa meg a Visual Studióban.

3. Cserélje ki a meglévő névtér definíciót **HomeController.cs**, a következő kódot.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Megoldás Explorerben bontsa ki a Views\Shared mappát, majd kattintson duplán a **_Layout.cshtml** nyissa meg a Visual Studio-szerkesztőben.

5.  **BÁRDOS a termékek**minden előfordulását **Saját ASP.NET-alkalmazás** módosítása

6. A **Kezdőlap**, **kapcsolatos**és **partner** eltávolítása. A következő példában a kiemelt kód törlése.

    ![][41]

7.  Megoldás Explorerben bontsa ki a Views\Home mappát, majd kattintson duplán a **Index.cshtml** nyissa meg a Visual Studio-szerkesztőben.
    A fájl teljes tartalmának cserélje le a következő kódot.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  A munka pontosságának ellenőrzése eddig lenyomja a **Ctrl + Shift + B** össze a projekt.


### <a name="run-the-app-locally"></a>Futtassa az alkalmazást helyi meghajtóra

Futtassa az alkalmazást, ellenőrizze, hogy működik.

1.  Győződjön meg arról, hogy a **ProductsPortal** az aktív projekt. Kattintson a jobb gombbal a megoldást Intézőben a projekt nevét, és válassza a **Beállítása, indítása projekt**.
2.  A Visual Studióban nyomja le az F5 billentyűt.
3.  Jelenjenek meg az alkalmazást, a böngészőben futó.

    ![][21]

## <a name="put-the-pieces-together"></a>A darab összeállított

A következő lépésként az ASP.NET-alkalmazás helyszíni termékek kiszolgálóval jelölje be a csatlakoztatni.

1.  Ha még nem szerepel a nyissa meg, a Visual Studio alkalmazásban nyissa meg újra a [ASP.NET-alkalmazás létrehozása](#create-an-aspnet-application) szakaszban létrehozott **ProductsPortal** projekt.

2.  A lépéssel "Egy helyszíni kiszolgálói létrehozása" szakaszban hasonló a NuGet csomag hozzáadása a project hivatkozásokat. Megoldás Explorerben kattintson a jobb gombbal a **ProductsPortal** projektet, majd **NuGet csomagok kezelése**gombra.

3.  Keresse meg a "Szolgáltatás Bus", és jelölje ki a **Microsoft Azure Service Bus** elemet. A telepítés befejezéséhez, majd a párbeszédpanel bezárásához.

4.  Megoldás Explorerben kattintson a jobb gombbal a **ProductsPortal** projektet, majd kattintson a **Hozzáadás gombra**, majd a **Meglévő elemet**.

5.  Nyissa meg a **ProductsContract.cs** fájl **ProductsServer** konzol projektből. Kattintással jelölje ki a ProductsContract.cs. Kattintson a lefelé mutató nyílra a mellett a **Hozzáadás gombra**, majd kattintson a **Hozzáadás hivatkozásként**gombra.

    ![][24]

6.  Most már a **HomeController.cs** fájlra a Visual Studio-szerkesztőben, és cserélje le a névtér meghatározása a következő kódot. Ne felejtse el a szolgáltatás névtere, és *yourKey* nevének *yourServiceNamespace* helyettesítése a Társítások használatával. Ezzel engedélyezi az ügyfél, hívja fel a helyszíni szolgáltatást, az eredményt, a hívás visszaadása.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  A megoldás Intézőben kattintson a jobb gombbal a **ProductsPortal** megoldás (Győződjön meg róla, hogy kattintson a jobb gombbal a megoldást, nem a projekthez). Kattintson a **Hozzáadás**gombra, majd kattintson a **Meglévő projekt**.

8.  Nyissa meg azt a **ProductsServer** projektet, majd kattintson duplán a **ProductsServer.csproj** megoldásfájlt fel szeretne venni.

9.  Annak érdekében, hogy az adatok megjelenítése a **ProductsPortal** **ProductsServer** futnia kell. A megoldás Intézőben kattintson a jobb gombbal a **ProductsPortal** megoldást, és válassza a **Tulajdonságok parancsot**. A **Tulajdonságlap** párbeszédpanel jelenik meg.

10. A bal oldalon kattintson a **Projekt indítása**. A jobb oldalon kattintson a **több indítási projektek hivatkozásra**. Győződjön meg arról, hogy **ProductsServer** és **ProductsPortal** jelenik meg, abban a sorrendben, **kezdje** meg műveletként mindkét.

      ![][25]

11. Továbbra is a **Tulajdonságok** párbeszédpanelen kattintson **A Project a függőségek** bal oldalán.

12. Kattintson a **projektek** listájában **ProductsServer**. Győződjön meg arról, hogy **ProductsPortal** **nincs** bejelölve.

14. Kattintson a **projektek** listájában **ProductsPortal**. Győződjön meg arról, hogy be van jelölve **ProductsServer** . 

    ![][26]

15. Kattintson a **Tulajdonságlap** párbeszédpanelen **az OK gombra** .

## <a name="run-the-project-locally"></a>A projekt helyileg futtatása

Ha tesztelni szeretné az alkalmazás helyi meghajtóra, a Visual Studióban nyomja le az **F5**. A helyszíni kiszolgálót (**ProductsServer**) első kell kezdődnie, majd a **ProductsPortal** alkalmazás kezdetének a böngészőablakban. Ebben az esetben jelenik meg, hogy a termék készlet listák adatok beolvasása a termék szolgáltatás a helyszíni rendszer.

![][10]

Nyomja le a **frissítése** **ProductsPortal** lapon. Minden alkalommal, amikor frissíti a lapot, a kiszolgáló alkalmazás egy üzenetben megjelenik amikor `GetProducts()` a **ProductsServer** nevezik.

Zárja be a következő lépés a folytatás előtt mindkét alkalmazást.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>A ProductsPortal projekt az Azure web app telepítése

A következő lépésként az **ProductsPortal** frontend átalakítása az Azure-webappokban. Először telepítse az **ProductsPortal** projektet, a [Deploy a webhelyen a project az Azure web App alkalmazásban](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app)szakasz lépéseket követve. Telepítésének befejeződése után térjen vissza az oktatóanyag, és folytassa a következő lépéssel.

> [AZURE.NOTE] Hibaüzenet jelenik meg a böngészőablakban az **ProductsPortal** webes projekt automatikusan elindul a telepítés után, meg. Várható, és oka az, hogy a **ProductsServer** alkalmazás még nem fut.

Másolja a vágólapra a telepített web app URL-CÍMÉT, amennyire szüksége lesz az URL-cím, a következő lépésben. Ez az URL-címet is szerezhet be az Azure alkalmazás szolgáltatási tevékenység ablakból a Visual Studio:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Web App beállítása ProductsPortal

Az alkalmazás futtatásához a felhőben, gondoskodnia kell arról, hogy **ProductsPortal** elindítása a Visual Studio a web App alkalmazásból.

1. A Visual Studióban kattintson a jobb gombbal a **ProjectsPortal** projekt, és válassza a **Tulajdonságok parancsot**.

3. A bal oldali oszlopban kattintson a **webhely**.

5. **Indítsa el a művelet** csoportban az **URL-cím indítása** gombra, és a szövegmezőbe írja be az URL a korábban telepített webalkalmazás; Ha például `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. A **fájl** menüben, a Visual Studióban az **Összes mentése**gombjára.

7. A Visual Studio Build menüben kattintson a **Megoldás újraépítéséhez**.

## <a name="run-the-application"></a>Futtassa az alkalmazást

2.  Nyomja le az F5 létre, és futtassa az alkalmazást. A helyszíni server (az **ProductsServer** konzol alkalmazás) első kell kezdődnie, majd a **ProductsPortal** alkalmazás kell kezdenie a munkát egy böngészőablakban, ahogy az alábbi képernyőfelvételen. Ismét figyelje meg, hogy a termék készlet adatok beolvasása a termék szolgáltatás a helyszíni rendszer sorolja fel, és megjelenítik a web App alkalmazásban. Jelölje be az URL-CÍMÉT a győződjön meg arról, hogy a **ProductsPortal** fut-e meg az Azure web app a felhőben. 

    ![][1]

    > [AZURE.IMPORTANT] A New **ProductsServer** fut, és visszaadhassa a kért adatokat a **ProductsPortal** alkalmazás képes kell lennie. A böngésző hibaüzenetet jelenít meg, ha Várjon néhány további másodpercig **ProductsServer** betöltése, és a következő üzenet megjelenítéséhez. Nyomja le az **frissítése** a böngészőben.

    ![][37]

3. Vissza a böngészőben nyomja le az ENTER **frissítése** **ProductsPortal** lapon. Minden alkalommal, amikor frissíti a lapot, a kiszolgáló alkalmazás egy üzenetben megjelenik amikor `GetProducts()` a **ProductsServer** nevezik.

    ![][38]

## <a name="next-steps"></a>Következő lépések  

További tudnivalók a szolgáltatás Bus, az alábbi forrásokban talál:  

* [Azure Service Bus][sbwacom]  
* [Szolgáltatás Bus sorban várakozó használata][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Eszközök és SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

