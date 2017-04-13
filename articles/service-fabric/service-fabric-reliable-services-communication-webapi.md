<properties
   pageTitle="Kommunikáció a ASP.NET webes API-val szolgáltatás |} Microsoft Azure"
   description="Megtudhatja, hogy miként szolgáltatás kommunikációs lépésenkénti végrehajtása az ASP.NET webes API OWIN önálló szolgáltatója a megbízható szolgáltatások API segítségével."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Első lépések: OWIN önálló szolgáltatója szolgáltatás háló webes API-szolgáltatások

Azure Service háló a power helyezi a pálcikák is, hogy miként kívánja a felhasználókkal és a többi szolgáltatások kiválasztásakor. Ebben az oktatóanyagban koncentrál végrehajtási szolgáltatás kommunikációs nyissa meg a webes felület .NET (OWIN) önálló szolgáltatója szolgáltatás háló megbízható szolgáltatások API-ASP.NET webes API használata. Azt fogja elmélyedhet mélyen a megbízható szolgáltatások beépíthető kommunikáció API-val. Használjuk webes API-val is a lépésenkénti példa mutatja, hogy miként állíthat be egyéni kommunikációs figyelő.


## <a name="introduction-to-web-api-in-service-fabric"></a>Webes API szolgáltatás háló – bevezetés

ASP.NET webes API a népszerű és hatékony keretrendszer fölött a .NET-keretrendszer HTTP API-khoz készítéséhez. Ha Ön nem már ismerős keretében, olvassa el a [ASP.NET webes API 2 – első lépések](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) további.

Webes API szolgáltatás háló az azonos ASP.NET webes API ismert és közkedvelt. A különbség a hogyan, *host* a webes API-alkalmazások. Nem használ-e a Microsoft Internet Information Services (IIS). Különbség pontosabb megértéséhez, hogy most célszerű megszakítani két részre:

 1. A webes API-alkalmazás (beleértve a vezérlőket és az adatmodellek)
 2. A host (az érintett webkiszolgálóra, általában az IIS)

A webes API-alkalmazások magát nem változtatja meg. Azt nem különbözik írt kap múltbeli webes API-alkalmazásokat, és egyszerűen vigye fölé az alkalmazás kódja a legtöbb láthatja. De ha, amelyeket szolgáltatója IIS, ha az alkalmazás host némileg eltér az éppen használt lehet. Azt a használat részt szeretne lépni, mielőtt kezdjük valami ismerős: a webes API-alkalmazást.


## <a name="create-the-application"></a>Az alkalmazás létrehozása

Indítsa el a Visual Studio Skype 2015 vállalati egyetlen állapot nélküli szolgáltatással az új szolgáltatás háló alkalmazás létrehozásával:

![Új szolgáltatás háló-alkalmazás létrehozása](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Visual Studio sablon használata a webes API állapot nélküli szolgáltatáshoz is elérhetők. Ebben az oktatóanyagban azt, amely mi volna, ha lehetőséget választotta, ez a sablon létrehozása a webes API projekt fogja létrehozása.

Jelöljön ki egy üres állapot nélküli szolgáltatás projektet megtudhatja, hogyan hozhat létre a webes API projekt létrehozása, vagy indítsa el az állapot nélküli szolgáltatás webes API-sablon, és kövesse.  

![Hozzon létre egy egyetlen állapot nélküli szolgáltatás](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Első lépésként webes API néhány NuGet csomagokban csoportosítani. A csomag azt használni kívánt Microsoft.AspNet.WebApi.OwinSelfHost. A csomag magában foglalja a minden szükséges webes API-csomagok és a *host* csomagok. Ez lesz fontos később.

![Hozzon létre a webes API NuGet csomag-kezelővel](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

A csomagok telepítése után megkezdheti a webes API projekt alapszerkezetét, épület. Ha már használta a webes API-t, a projekt szerkezetének nagyon ismerősnek kell kinéznie. Kezdje hozzáadásával egy `Controllers` könyvtárat és egy egyszerű értékek vezérlő:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Következő lépésként vegyen fel egy indítási osztály a projekt legfelső szintű regisztrálhatja a továbbítás, valamint formatters és bármilyen egyéb konfigurációs beállítása. Ez akkor is, ahol webes API csatlakoztatja az *host*, amely fog javított változat később újra. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Ez az alkalmazás részére. Ezen a ponton hogy állított be csak az egyszerű webes API projekt elrendezés. Eddig, ne eltér sokkal írt kap múltbeli webes API projektek vagy az egyszerű webes API-sablon. Az üzleti logikai funkcióinak a szokásos módon kerül a vezérlőket és az adatmodellek.

Most már Mi a teendő, hogy azt is ténylegesen futtathatja szolgáltatója kapcsolatban?

## <a name="service-hosting"></a>A szolgáltatás szolgáltatónál

A szolgáltatás háló a szolgáltatás fut *szolgáltatás host folyamata*a szolgáltatáskód futtató végrehajtható fájl. A megbízható szolgáltatások API segítségével írásakor szolgáltatás a szolgáltatás project csak olyan végrehajtható fájl, és a szolgáltatás típusa és fut, a kód lefordítja. Az IGAZ, a legtöbb esetben a szolgáltatás háló a .NET egy service írásakor. Ha az állapot nélküli szolgáltatás project Program.cs nyit meg, érdemes jelenik meg:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Ha, amely a következőképpen néz gyanúsan a belépési pontjához konzol alkalmazás, mert az van.

A szolgáltatás host folyamatot és a szolgáltatás regisztrációs további részleteket a jelen cikk túlra vannak. De fontos tudni, hogy *a szolgáltatáskód fut a saját folyamat*most.

## <a name="self-host-web-api-with-an-owin-host"></a>Önálló megbízhat a webes API-OWIN host

Tekintve, hogy a webes API-alkalmazás kód üzemelteti saját folyamat, hogyan tegye meg a csatlakoztatni azt felfelé webkiszolgálóra? Írja be a [OWIN](http://owin.org/). OWIN egyszerűen szerződés .NET webalkalmazások és a webes kiszolgálók között. Hagyományos ASP.NET (legfeljebb MVC 5) használata esetén a webalkalmazás szorosan ahhoz az IIS System.Web keresztül. Azonban webes API hajtja végre OWIN, úgy, hogy van-e kapcsolni webalkalmazás olvasásakor az érintett webkiszolgálóra, amelyen meg. Emiatt használható *önálló üzemeltetett* OWIN webkiszolgálóra elindíthatja a saját folyamat. Ez tökéletesen a szolgáltatás háló üzemeltetési modellel csak leírt illeszkedik.

Ebben a cikkben Katana használjuk a OWIN állomással a webes API-alkalmazáshoz. Katana [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) és a Windows [HTTP-kiszolgáló API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)-ra épülő Megnyitás-forrás OWIN host megvalósítása.

> [AZURE.NOTE] Többet szeretne tudni a Katana, lépjen a [Katana webhelyet](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Egy gyors áttekintést olvashat arról, hogyan használhatja a Katana önálló tárolni a webes API olvassa el a [Használata OWIN Self-Host ASP.NET webes API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api)című témakört.


## <a name="set-up-the-web-server"></a>Az érintett webkiszolgálóra beállítása

A megbízható szolgáltatások API hol csatlakoztathatja kommunikációs készleteket, amelyek lehetővé teszik a felhasználók és az ügyfelek a szolgáltatáshoz való kapcsolódáshoz kommunikációs bejegyzés pontot biztosít:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Az érintett webkiszolgálóra (és más kommunikáció Papírhalom használhatja a jövőben, például WebSockets) kell használni a ICommunicationListener felület megfelelően integrálása a rendszer. Ennek az oka az alábbi lépéseket a további látható lesz.

Első lépésként hozzon létre, amely ICommunicationListener OwinCommunicationListener nevű osztály:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

A ICommunicationListener felület bemutat három kommunikációs figyelő a szolgáltatásbeli kezelése:

 - *OpenAsync*. Indítsa el a kérések figyelését.
 - *CloseAsync*. A kérések figyelését leállítása, bármilyen repülés kérések Befejezés és leállítása.
 - A *megszakításhoz*. Lépjen ki minden, és azonnal leállítja.

Első lépésként tagokat magánjellegű osztály dolgot kell a figyelő működik. Fog ezek inicializált konstruktorának keresztül, és később is használatban beállításakor listening URL-CÍMÉT.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>OpenAsync megvalósítása

Állítsa be az érintett webkiszolgálóra, két adatra van szükség:

 - *Egy URL-elérési út előtagot*. Bár ez nem kötelező, célszerű való beállítása ez most, hogy az alkalmazás biztonságos üzemeltetheti több webszolgáltatásokhoz.
 - *Olyan portot*.

Mielőtt olyan portot beszerzése az érintett webkiszolgálóra, fontos, hogy megismeri, hogy szolgáltatás háló biztosít az alkalmazási réteg, amely végpontként egy pufferelési az alkalmazás és a futó operációs rendszer között. Ilyen szolgáltatás háló biztosítja az *végpontjait* a szolgáltatások konfigurálása. Szolgáltatás háló biztosítja, hogy végpontok érhető el a szolgáltatás használatához. Ebben az esetben nem kell őket saját maga állítja be az alapul szolgáló OS környezetben. A szolgáltatás háló alkalmazás különböző környezetekben anélkül, hogy az alkalmazás bármilyen módosítást egyszerűen üzemeltetheti. (Például üzemeltetheti az Azure-ban vagy a saját adatközpontban ugyanazt a kérelmet.)

HTTP zárólap PackageRoot\ServiceManifest.xml beállítása:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Ez a lépés nem fontos, mivel a korlátozott hitelesítő adatait (Windows hálózati szolgáltatás) fut a szolgáltatás host folyamat. Ez azt jelenti, hogy a szolgáltatás nem hozzáférni HTTP végpont beállítása a saját. Végpont beállításaival szolgáltatás háló tudja a megfelelő hozzáférési vezérlőelem-lista (vezérlés) beállítása a figyeli a szolgáltatás URL-címe. Szolgáltatás háló is végpontok konfigurálása szabványos helyet biztosít.


Vissza a OwinCommunicationListener.cs elindíthatja OpenAsync végrehajtása. Ez a hol indítsa el az érintett webkiszolgálóra. Első lépésként beolvasni a végpont adatait, és az URL-címet, amely a figyeli a szolgáltatás hozzon létre. Az URL-cím attól függően, hogy a figyelő használják állapot nélküli szolgáltatás vagy az állapot-nyilvántartó szolgáltatás különböző lesz. Állapot-nyilvántartó szolgáltatás a figyelő kell hozzon létre egy egyedi címmel rendelkező minden állapot-nyilvántartó szolgáltatás kópia figyeli azt. Az állapot nélküli szolgáltatások sokkal egyszerűbb lehet a címet. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Figyelje meg, hogy a "http://+" Itt használatos. Győződjön meg arról, hogy az érintett webkiszolgálóra elérhető címét, például localhost, FQDN és a számítógép IP figyel az.

OpenAsync végrehajtása miért az érintett webkiszolgálóra (vagy bármely kommunikációs Papírhalom) hajtanak végre, közvetlenül a egy ICommunicationListener, hanem csak akkor nyissa meg a legfontosabb okok egyike `RunAsync()` a szolgáltatásban. A OpenAsync visszatérési értéke a cím, a webkiszolgáló figyel. A cím visszakerül a rendszer, amikor a szolgáltatással regisztrálja a címet. Szolgáltatás háló biztosít az API-val, amely lehetővé teszi az ügyfelek és más szolgáltatásokhoz, majd kérhet a cím szolgáltatás neve szerint. Ez fontos, mivel a szolgáltatás címe nem statikus. Szolgáltatások helyezi át a fürt, erőforrás és a elérhetősége céljából. Ez az eljárás, amely lehetővé teszi az ügyfelek a szolgáltatás listening címe feloldásához.

Amely szem előtt OpenAsync elindítja az érintett webkiszolgálóra, és figyeli a címét adja meg. Jegyzet, hogy azt figyeli "http://+", de mielőtt OpenAsync címét, adja meg a "+" az IP-címének vagy a jelenleg a csomópont FQDN cseréli. A cím, a függvény által visszaadott ezzel a módszerrel, mit regisztrált a rendszer. Érdemes emellett mi ügyfelek és más szolgáltatásokhoz jelenik meg, amikor egy webszolgáltatás címének, hogy kérni. Helyesen csatlakozhat ügyfélalkalmazások szükségük van egy tényleges IP-címének vagy teljesen minősített tartománynév webcímét.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Figyelje meg, hogy ez hivatkozik-e az indítási osztály a OwinCommunicationListener konstruktorához az átadott. Az érintett webkiszolgálóra indítási példány a webes API-alkalmazás bootstrap használt.

A `ServiceEventSource.Current.Message()` vonal jelöli a diagnosztikai események ablakban később kattintva erősítse meg, a webkiszolgáló sikeresen elindult, hogy az alkalmazás futtatásakor.

## <a name="implement-closeasync-and-abort"></a>Megvalósítása CloseAsync és megszakítása

Végezetül végre CloseAsync és a megszakítás le szeretné állítani az érintett webkiszolgálóra. Az érintett webkiszolgálóra, a kiszolgáló leíró OpenAsync során létrehozott ártalmatlanításának leállítható.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

Végrehajtási ebben a példában CloseAsync és a megszakítás egyszerűen leállítja az érintett webkiszolgálóra. Választhatnak CloseAsync hajtsa végre az érintett webkiszolgálóra további biztonságosan összehangolt leállítására. Például a leállítás sikerült megvárja, amíg a return előtt be kell fejeződnie repülés kérelmeket.

## <a name="start-the-web-server"></a>Indítsa el az érintett webkiszolgálóra

Most már készen áll, létrehozása, és visszatér az érintett webkiszolgálóra indításához OwinCommunicationListener egy példánya. Vissza a szolgáltatás class (WebService.cs) felülbírálása az `CreateServiceInstanceListeners()` módszer:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Az, ha a webes API- *alkalmazás* és a OWIN *host* végül felel meg. A host (OwinCommunicationListener) kap egy példánya az *alkalmazás* (webes API) az indítási osztály keresztül. Szolgáltatás háló majd kezelése az életciklus. Ezt a mintázatot általában követheti a minden kommunikációs Papírhalom.

## <a name="put-it-all-together"></a>Az összes közös formában

Ebben a példában nem kell tennie semmit sem a `RunAsync()` módszer, úgy, hogy a felülírási egyszerűen lehet eltávolítani.

A végleges szolgáltatás végrehajtása rendkívül egyszerű kell lennie. Csak a kapcsolati figyelő létrehozásához szükséges:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

A teljes `OwinCommunicationListener` osztály:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Most, hogy az összes csatolt helyen illesztette be, a projekt például egy tipikus webes API-alkalmazás megbízható szolgáltatások API belépési pontról és egy OWIN host kell kinéznie:


![Webes API megbízható szolgáltatások API belépési pontról és OWIN host](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Futtatni, és csatlakoztassa a böngészőn keresztül

Ha még nem tette, [a fejlesztői környezet beállítása](service-fabric-get-started.md).


Most összeállítása és a szolgáltatás üzembe. Nyomja le az **F5** Visual Studio létre, és az alkalmazás telepítéséhez. A diagnosztikai események ablakában meg kell jelennie egy üzenet, amely jelzi, hogy az érintett webkiszolgálóra megnyitott http://localhost:8281 /.


![Visual Studio diagnosztikai események ablak](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] A port már megnyitotta a számítógépen egy másik folyamat, ha az alábbi hibaüzenet jelenhet meg. Ez azt jelzi, hogy a figyelő nem nyitható meg. Ez a helyzet, ha próbáljon meg egy másik port ServiceManifest.xml a végpont konfiguráció.


A szolgáltatás fut, nyissa meg a böngészőt, és nyissa meg azt a [http://localhost:8281/api/értékek](http://localhost:8281/api/values) tesztelje azt.

## <a name="scale-it-out"></a>Ki méretezése

Méretezés kifelé állapot nélküli web Apps alkalmazások általában azt jelenti, további gépek hozzáadása pörgő be őket a webes alkalmazásokat. Szolgáltatás háló üzletifolyamat-tervező motor ehhez meg, valahányszor új csomópontok fürthöz kerülnek. Amikor létrehoz egy állapot nélküli szolgáltatás példányát, megadhatja a létrehozni kívánt példányainak száma. Szolgáltatás háló a fürt csomópontjai helyez el, hogy példányainak száma. És azt, hogy ne hozzon létre egynél több példány bármely egy csomóponton. Szolgáltatás háló mindig hozzon létre egy példány minden csomóponton **-1** -példány számának megadásával is találhatók. Ez biztosítja, hogy a fürt méretezése csomópontok hozzáadásakor a állapot nélküli szolgáltatás egy példánya létrejön az új csomóponton. Ezt az értéket is az szolgáltatás példányának egy tulajdonság, ezért a Ha hozza létre a szolgáltatás beállítása. Ezt megteheti a Powershellen keresztül:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Szintén ezt megteheti a Visual Studio állapot nélküli szolgáltatási projekt alapértelmezett szolgáltatás definiálása esetén:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Alkalmazás és a szolgáltatás példányainak létrehozásával kapcsolatos további tudnivalókért lásd: [az alkalmazás Deploy](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Következő lépések

[A szolgáltatás háló alkalmazás hibakeresése Visual Studio segítségével](service-fabric-debugging-your-application.md)
