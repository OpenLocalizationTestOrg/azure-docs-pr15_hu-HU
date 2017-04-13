<properties
   pageTitle="Hozzon létre egy előtér az alkalmazás használatával ASP.NET Core |} Microsoft Azure"
   description="ASP.NET Core webes API project és a szolgáltatás közötti kommunikáció ServiceProxy használatával jelenítik meg a szolgáltatás háló alkalmazása a webhelyre."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/11/2016"
   ms.author="seanmck"/>


# <a name="build-a-web-service-front-end-for-your-application-using-aspnet-core"></a>A szolgáltatás előtér az alkalmazás használatával ASP.NET Core összeállítása

Alapértelmezés szerint Azure Service háló szolgáltatások nem rendelkeznek a weben nyilvános felhasználói felülete. Kattintva jelenítse meg az alkalmazás működésének HTTP-ügyfelek számára, akkor kell használni kívánt belépési pontjához és majd innen a az egyes szolgáltatások webes projekt létrehozása.

Ebben az oktatóanyagban azt fog emelje fel, ahol azt abbahagyta a [az első Visual Studio-alkalmazás létrehozása](service-fabric-create-your-first-application-in-visual-studio.md) oktatóprogram során, és az állapot-nyilvántartó számláló szolgáltatás elé webszolgáltatás hozzáadása. Ha még nem tette meg, lépjen vissza, és végezze el a oktatóanyag első.

## <a name="add-an-aspnet-core-service-to-your-application"></a>ASP.NET Core szolgáltatás az alkalmazás hozzáadása

ASP.NET Core egy könnyű, platformok – webes fejlesztési keretrendszer, amelyek a felhasználói felület modern webhely létrehozása, valamint a webes API-khoz is használhatja. A meglévő alkalmazáshoz helyezzen el egy ASP.NET webes API-projektet.

>[AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez, [telepíteni]kell a .NET Core 1.0[dotnetcore-install].

1. A megoldás Intézőben **szolgáltatások** gombbal az alkalmazás projekten belül, és válassza a **hozzáadása > új szolgáltatás háló**.

    ![Új szolgáltatás hozzáadása az meglévő alkalmazáshoz][vs-add-new-service]

2. A **szolgáltatás létrehozása** lapon válassza a **ASP.NET Core** , és adja meg egy nevet.

    ![ASP.NET webszolgáltatás választása a az új szolgáltatási párbeszédpanel][vs-new-service-dialog]

3. A következő oldal biztosít ASP.NET Core projektsablonok. Ne feledje, hogy ezek látni szeretné, ha létrehozott egy ASP.NET Core projekten kívüli egy szolgáltatás háló alkalmazás azonos sablonok. Ebben az oktatóanyagban azt fogja válassza a **Webes API -val**. Az azonos fogalmak alkalmazhat azonban teljes webalkalmazás létrehozása.

    ![ASP.NET-projekt típusának kiválasztása][vs-new-aspnet-project-dialog]

    Amikor a webes API-projekt létrejött, az alkalmazás két szolgáltatás lesz. Az alkalmazás összeállítása továbbra is, mint a további szolgáltatások ad hozzá a pontosan megegyező módon. Minden egyes független verziószámmal és frissített lehet.

>[AZURE.TIP] ASP.NET Core services létrehozásával kapcsolatos további tudnivalókért lásd: az [ASP.NET Core dokumentációt](https://docs.asp.net).

## <a name="run-the-application"></a>Futtassa az alkalmazást

Egy értelmezhető mi azt már elvégzett, most az új alkalmazás telepítéséhez és nézze meg az alapértelmezett működés, amely a ASP.NET Core webes API-sablont tartalmaz.

1. Nyomja le az F5 Visual Studio szeretné hibakeresése az alkalmazást.

2. Telepítésének befejeződése után a Visual Studio elindítja a legfelső szintű ASP.NET webes API-szolgáltatás – valami hasonló http://localhost:33003 a böngészőben. A portszámot véletlenszerűen, és a számítógépen lévő eltérőek lehetnek. A ASP.NET Core webes API-sablon nem biztosít alapértelmezett működés a legfelső szintű, így a hiba jelenik meg a böngészőben.

3. Adja hozzá `/api/values` azt a helyet, a böngészőben. Ez a meghívják a `Get` módszerrel a webes API-sablon a ValuesController. Az alapértelmezett válasz, a sablon – egy JSON tömb két karakterláncot tartalmazó által biztosított ad vissza:

    ![ASP.NET Core webes API sablonból eredményeként alapértelmezett értékek][browser-aspnet-template-values]

    Az oktatóanyag végén azt fogja felváltotta ezeket az alapértelmezett értékeket a legutóbbi számláló értéket az állapot-nyilvántartó szolgáltatásból.


## <a name="connect-the-services"></a>A szolgáltatások összekapcsolása

Szolgáltatás háló hogyan kommunikálni megbízható szolgáltatások teljes rugalmasságot biztosít. Egyetlen alkalmazásból lehet, hogy szolgáltatásokat, amelyek a TCP, más szolgáltatások, amelyek egy HTTP REST API keresztül érhető el, és továbbra is más szolgáltatások, amelyek sockets webhelyen keresztül érhető el keresztül érhető el. A rendelkezésre álló beállítások és a kompromisszumok érintett hátteréről [Communicating szolgáltatásokkal](service-fabric-connect-and-communicate-with-services.md). Ebben az oktatóanyagban hogy fog hajtsa végre az egyszerűbb eljárások egyikét, és használja a `ServiceProxy` / `ServiceRemotingListener` a SDK csomagjában osztályok.

Az a `ServiceProxy` (modellezni távoli eljáráshívás vagy RPC) megközelítés, megadhatja a használni kívánt szolgáltatás a nyilvános szerződés felületet. Ezután használja a kapcsolat kattintva állítsa elő a szolgáltatás személlyel proxy osztály.


### <a name="create-the-interface"></a>A kapcsolat létrehozása

Azt a program ekkor elkezdi használni kívánt a szerződést, az állapot-nyilvántartó szolgáltatás és az ügyfelek, beleértve a ASP.NET Core projekt között a felület létrehozásával.

1. A megoldás Intézőben, kattintson a jobb gombbal a megoldás, és válassza az **Add** > **Új projektet**.

2. A bal oldali navigációs ablaktáblában válassza a **Visual C#** tétel, és válassza az a **Osztály-tár** sablon. Győződjön meg arról, hogy a .NET-keretrendszer verzió **4.5.2**értéke.

    ![Az állapot-nyilvántartó szolgáltatás felület projekt létrehozása][vs-add-class-library-project]

3. Ahhoz, hogy használható felületet `ServiceProxy`, a IService felület származhat. A kapcsolat is tartalmazni fogja a szolgáltatás háló NuGet csomagok egyikéhez. A csomag hozzáadásához kattintson a jobb gombbal az új osztály tár projekten, és válassza a **NuGet csomagok kezelése**.

4. Keresse meg a **Microsoft.ServiceFabric.Services** csomagot, és telepítse azt.

    ![A szolgáltatások NuGet csomag hozzáadása][vs-services-nuget-package]

5. Az a osztálytár hozzon létre egy csatolófelület egyetlen módszer, `GetCountAsync`, és a felület IService terjed ki.

    ```c#
    namespace MyStatefulService.Interfaces
    {
        using Microsoft.ServiceFabric.Services.Remoting;

        public interface ICounter: IService
        {
            Task<long> GetCountAsync();
        }
    }
    ```


### <a name="implement-the-interface-in-your-stateful-service"></a>Az állapot-nyilvántartó szolgáltatás a felület megvalósítása

Most, hogy a felület definiált van, az állapot-nyilvántartó szolgáltatás végrehajtásához szükséges.

1. Az állapot-nyilvántartó szolgáltatás hozzáadása a osztály tár projektet, amely tartalmazza a felület.

    ![Az osztály tár projekt mutató hivatkozás hozzáadása az állapot-nyilvántartó szolgáltatásban][vs-add-class-library-reference]

2. Keresse meg az osztály öröklő `StatefulService`, például: `MyStatefulService`, és azt végrehajtásához bővítése a `ICounter` felület.

    ```c#
    using MyStatefulService.Interfaces;

    ...

    public class MyStatefulService : StatefulService, ICounter
    {        
          // ...
    }
    ```

3. Most végrehajtja a megadott egyetlen módszer a `ICounter` felület `GetCountAsync`.

    ```c#
    public async Task<long> GetCountAsync()
    {
      var myDictionary =
        await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");

        using (var tx = this.StateManager.CreateTransaction())
        {          
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            return result.HasValue ? result.Value : 0;
        }
    }
    ```


### <a name="expose-the-stateful-service-using-a-service-remoting-listener"></a>Az állapot-nyilvántartó szolgáltatás használ szolgáltatási távelérési figyelő indításához

Az a `ICounter` végrehajtani, az állapot-nyilvántartó szolgáltatás kell hívható más szolgáltatások engedélyezése utolsó lépéseként felülete kommunikációs csatorna megnyitásához. Az állapot-nyilvántartó szolgáltatások, a szolgáltatás háló nevű bírálhat módszert biztosít `CreateServiceReplicaListeners`. Ez a módszer is megadhat egy vagy több kommunikációs hallgatók, kommunikáció, amely esetén engedélyezni szeretné a szolgáltatás függően.

>[AZURE.NOTE] A kapcsolati állapot nélküli szolgáltatások csatorna megnyitásához egyenértékű módszer nevezik `CreateServiceInstanceListeners`.

Ebben az esetben azt helyére kerül a meglévő `CreateServiceReplicaListeners` módszer, és adja meg egy példányának `ServiceRemotingListener`, hívható keresztül ügyfelektől RPC zárólap létrehoz amely `ServiceProxy`.  

```c#
using Microsoft.ServiceFabric.Services.Remoting.Runtime;

...

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new List<ServiceReplicaListener>()
    {
        new ServiceReplicaListener(
            (context) =>
                this.CreateServiceRemotingListener(context))
    };
}
```


### <a name="use-the-serviceproxy-class-to-interact-with-the-service"></a>A szolgáltatás használata az ServiceProxy osztály használatával

Az állapot-nyilvántartó szolgáltatás készen áll a fogadni más szolgáltatások érkező forgalmat. Így továbbra is veheti fel, a kód használatával kommunikál a ASP.NET webszolgáltatásból.

1. A ASP.NET projektben hozzáadása a osztálytár, amely tartalmazza a `ICounter` felület.

2. Kattintson a **Szerkesztés** menü megnyitásához a **Configuration Manager**. Meg kell jelennie alábbihoz hasonló:

    ![Konfigurációs manager megjelenítő osztálytár AnyCPU szerint][vs-configuration-manager]

    Figyelje meg, hogy az a osztály tár projekt **MyStatefulService.Interface**, bármely processzor össze van-e beállítva. A szolgáltatás háló megfelelően működjön, meg kell kell kifejezetten alkalmazásindítójukban x64. Kattintson a Platform tartalmazó legördülő listára, és válassza az **Új**, majd hozzon létre egy x64 platform beállításait.

    ![Új platform osztálytár létrehozása][vs-create-platform]

3. A Microsoft.ServiceFabric.Services csomag hozzáadása az ASP.NET projekt ugyanúgy, mint a korábban elvégzett az osztály tár projekthez. Ezzel a `ServiceProxy` osztály.

4. A **vezérlők** mappát, nyissa meg a `ValuesController` osztály. Megjegyzendő, hogy a `Get` módszer jelenleg csak tömbjét adja eredményül egy karakterlánc csomagolásukkor "érték1" és "érték2" – amely megfelel a mi bekerül a böngészőben korábbi verziójában. Ez a megvalósítás cserélje ki a következő kódot:

    ```c#
    using MyStatefulService.Interfaces;
    using Microsoft.ServiceFabric.Services.Remoting.Client;

    ...

    public async Task<IEnumerable<string>> Get()
    {
        ICounter counter =
            ServiceProxy.Create<ICounter>(new Uri("fabric:/MyApplication/MyStatefulService"), new ServicePartitionKey(0));

        long count = await counter.GetCountAsync();

        return new string[] { count.ToString() };
    }
    ```

    Kód első sora lesz kulcs. Az állapot-nyilvántartó szolgáltatás a ICounter proxy létrehozásához meg kell adnia két csatolt információk: a partíciót nevét és a szolgáltatás.

    Feldarabolása be másik időszakok, állapotukban alapján határozza meg, például egy ügyfél-azonosító vagy irányítószám kulcs szerinti skála állapot-nyilvántartó szolgáltatások szétválasztás is használhatja. Trivial alkalmazásban. az állapot-nyilvántartó szolgáltatás csak akkor van egy partíciót, így a kulcs nem számít. Bármelyik Ön által billentyűt a azonos partíciót vezet. További információk a szétválasztás szolgáltatások, megtudhatja, [hogy miként partíciót szolgáltatást háló megbízható](service-fabric-concepts-partitioning.md).

    A szolgáltatás neve, a képernyő háló URI: /&lt;application_name&gt;/&lt;service_name&gt;.

    E két csatolt adatokat a szolgáltatás háló egyedileg azonosító kérések célszerű lehet küldeni a számítógépre. A `ServiceProxy` osztály is zökkenőmentesen kezeli az esetben, ha a számítógépen, amelyen az állapot-nyilvántartó szolgáltatás partíciót nem sikerül egy másik számítógépen tartomá kell előléptetni. A hardverabsztrakciós ellenőrzi, hogy az ügyfél kódírás más szolgáltatásokhoz, amelyek jelentősen egyszerűbb kezelni.

    Miután van még a proxy, akkor egyszerűen meghívása a `GetCountAsync` módszer, és visszatér az eredményére.

5. Nyomja le az F5 billentyű ismételt lenyomásával a módosított alkalmazásnak a futtatására. Mint, Visual Studio automatikusan elindítja a webes projekt gyökerébe a böngészőben. A "api/értékek" elérési út hozzáadása, és meg kell jelennie a számláló aktuális értékét adja vissza.

    ![Állapot-nyilvántartó számláló érték jelenik meg a böngészőben][browser-aspnet-counter-value]

    Frissítse a böngészőt, a rendszeres időközönként frissítse a számláló érték.


>[AZURE.WARNING] A megadott a sablonban vércse, vagyis ASP.NET Core webkiszolgáló [közvetlen internetforgalom kezelésére szolgáló jelenleg nem támogatott](https://docs.asp.net/en/latest/fundamentals/servers.html#kestrel). Gyártási alkalmazási eseteit, érdemes megfontolni a ASP.NET Core végpontok mögött [API-kezelés] szolgáltatója[ api-management-landing-page] vagy egy másik internetes átjáró. Figyelje meg, hogy a szolgáltatás háló nem támogatott telepítéshez IIS belül.


## <a name="what-about-actors"></a>Mi a helyzet szereplők?

Ebben az oktatóanyagban összpontosít hozzáadása a webes előtér-alkalmazást egy állapot-nyilvántartó szolgáltatás kommunikált. Azonban kövesse felvegye a szereplők nagyon hasonlít modell. Hogy valójában némileg egyszerűbbé.

Visual Studio hoz létre, hogy a projektben szereplő, automatikusan létrehoz egy felület projekten meg. A felület használatával kommunikál a szereplő webes projekt az egy szereplő proxy szeretne is használhatja. A kommunikációs csatorna automatikusan megadva. Így nem kell tennie semmit sem, amely egyenértékű a létrehozó egy `ServiceRemotingListener` megismert, az állapot-nyilvántartó szolgáltatás ebben az oktatóanyagban.

## <a name="how-web-services-work-on-your-local-cluster"></a>A helyi fürt webszolgáltatásokhoz működése

Az általános pontosan ugyanazt a telepített a helyi fürt több gép fürtre háló szolgáltatási kérelmet telepítheti és lehet erősen benne, hogy Ön a várt módon működik. Ennek oka a helyi fürtre egyszerűen a egyetlen számítógépen össze van csukva öt-csomópont konfiguráció.

Amikor webszolgáltatásokhoz, azonban van egy fő nuance. Ha a fürt mögött egy terheléselosztó helyezkedik el, ahogyan ezt a Azure-ban, győződjön meg róla, hogy a webes szolgáltatások telepítik minden számítógépen, mivel a terheléselosztó fog egyszerűen ciklikus forgalmat a gépek között. Ehhez megadásával a `InstanceCount` értékének-"1" a szolgáltatás.

Ezzel ellentétben kellő webszolgáltatás helyben futtatni, akkor annak érdekében, hogy csak egy példánya fut a szolgáltatás. Egyéb esetben futtatja az ütközések feloldása az elérési utat és a port figyelését több folyamatokat. Emiatt a web service példányok száma "1" helyi telepítési legyen.

Különböző értékek másik környezet beállítása című témakörben talál [több környezetekben kezelése alkalmazás paramétereinek](service-fabric-manage-multiple-environment-app-configuration.md).

## <a name="next-steps"></a>Következő lépések

- [Fürt létrehozása az Azure-ban a felhőbe az alkalmazás telepítéséhez](service-fabric-cluster-creation-via-portal.md)
- [További tudnivalók a szolgáltatások kommunikáció](service-fabric-connect-and-communicate-with-services.md)
- [További tudnivalók a szétválasztás állapot-nyilvántartó szolgáltatások](service-fabric-concepts-partitioning.md)

<!-- Image References -->

[vs-add-new-service]: ./media/service-fabric-add-a-web-frontend/vs-add-new-service.png
[vs-new-service-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-service-dialog.png
[vs-new-aspnet-project-dialog]: ./media/service-fabric-add-a-web-frontend/vs-new-aspnet-project-dialog.png
[browser-aspnet-template-values]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-template-values.png
[vs-add-class-library-project]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-project.png
[vs-add-class-library-reference]: ./media/service-fabric-add-a-web-frontend/vs-add-class-library-reference.png
[vs-services-nuget-package]: ./media/service-fabric-add-a-web-frontend/vs-services-nuget-package.png
[browser-aspnet-counter-value]: ./media/service-fabric-add-a-web-frontend/browser-aspnet-counter-value.png
[vs-configuration-manager]: ./media/service-fabric-add-a-web-frontend/vs-configuration-manager.png
[vs-create-platform]: ./media/service-fabric-add-a-web-frontend/vs-create-platform.png


<!-- external links -->
[dotnetcore-install]: https://www.microsoft.com/net/core#windows
[api-management-landing-page]: https://azure.microsoft.com/en-us/services/api-management/
