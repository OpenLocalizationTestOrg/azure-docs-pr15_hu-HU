<properties
    pageTitle="A szolgáltatás Bus továbbítási használata .NET |} Microsoft Azure"
    description="Megtudhatja, hogy miként más két alkalmazásainak használata az Azure Service Bus továbbítási szolgáltatáshoz."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Az Azure Service Bus továbbítási szolgáltatáshoz használata

Ez a cikk ismerteti a szolgáltatás Bus továbbítási szolgáltatáshoz használatáról. A minták írt C#, és használja a Windows Communication Foundation (WCF) API található összeállítás szolgáltatás Bus kiterjesztésű. Többet szeretne tudni a szolgáltatás Bus továbbítási a [szolgáltatás Bus továbbítását üzenetküldés](service-bus-relay-overview.md) áttekintése című témakörben találhat.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Mi az a szolgáltatás Bus továbbítási?

A [szolgáltatás Bus *továbbítási* ](service-bus-relay-overview.md) szolgáltatáshoz lehetővé teszi az Azure adatközponthoz, mind a saját helyszíni vállalati környezetben futtatott hibrid alkalmazásokat összeállítása. A szolgáltatás Bus továbbítási Ez megkönnyíti, mivel az, hogy biztonságosan jelenítik meg a nyilvános felhőben, egy vállalati vállalati hálózaton belül anélkül, hogy megnyitná a kapcsolatot a tűzfalon vagy kell beavatkozni a cég hálózati infrastruktúrájába telepített Windows Communication Foundation (WCF)-szolgáltatásokra.

![Továbbítás fogalmak](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

A szolgáltatás Bus továbbítási lehetővé teszi a meglévő vállalati környezetben host WCF-szolgáltatások. Majd delegálása, a bejövő levelek figyeli munkamenetek és a WCF-szolgáltatások a szolgáltatás Bus szolgáltatás Azure belül futó részvételi. Ez lehetővé teszi, hogy az jelenítik meg az alábbi szolgáltatások Azure-ban futó alkalmazás kód, illetve hordozható dolgozók vagy extranetes partner környezetben. Szolgáltatás Bus lehetővé teszi, hogy biztonságosan a ezek a szolgáltatások külön szinten lévő hozzáférő személyek szabályozása. Egy hatékony és biztonságos módon alkalmazás funkcióinak és az adatok a meglévő vállalati megoldásokat, és kihasználhatja azt a felhőből biztosít.

Ez a cikk bemutatja, hogyan lehet a szolgáltatás Bus továbbítási WCF webszolgáltatás, elérhetővé tett, TCP csatorna kötés, amely a két fél közötti biztonságos beszélgetés használata létrehozásához használja.

## <a name="create-a-service-namespace"></a>Szolgáltatás névtér létrehozása

Előkészületek nélkül a szolgáltatás Bus továbbítási Azure-ban, először létre kell hoznia egy névtér. Névtér tartalmazó tároló biztosít megcímezheti szolgáltatás Bus erőforrások az alkalmazáson belül.

A szolgáltatás névtere létrehozása:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>A szolgáltatás Bus NuGet csomag beszerzése

A [szolgáltatás Bus NuGet csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) legkönnyebben a szolgáltatás Bus API és állítsa be az alkalmazás az összes szolgáltatás Bus függőségek. A projektben a NuGet csomag telepítéséhez tegye a következőket:

1.  Megoldás Explorerben kattintson a jobb gombbal a **hivatkozást**, majd **NuGet csomagok kezelése**gombra.
2.  Keresse meg a "Szolgáltatás Bus", és jelölje ki a **Microsoft Azure Service Bus** elemet. Kattintson a **telepítés** gombot a telepítés befejezéséhez, majd zárja be a következő párbeszédpanelt:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Szolgáltatás Bus használatával elérhetővé tenni, és a webszolgáltatás SOAP TCP felhasználása

Egy meglévő WCF SOAP webszolgáltatás külső felhasználás elérhetővé, szeretné kell módosítást végez a szolgáltatás kötések és címét. A konfigurációs fájl módosításainak szükség lehet vagy kell, hozzá kód módosításai, attól függően, hogy hogyan állíthatja be és a WCF-szolgáltatások beállítva. Figyelje meg, hogy WCF lehetővé teszi, hogy több hálózati végpontokat fölé ugyanazt a szolgáltatást, így a meglévő belső végpontok lehetőség segítségével megőrizheti a külső hozzáférés szolgáltatás Bus végpontok hozzáadása egy időben közben.

Ebben a feladatban összeállítása egy egyszerű WCF-szolgáltatás, és egy szolgáltatás Bus figyelő hozzáadása. Ebben a gyakorlatban a Visual Studio néhány ismerős feltételezi, és ezért nem haladjon végig az adott létrehozása a projekt részleteit, és. Ehelyett a kód összpontosít.

Mielőtt hozzákezdene ezeket a lépéseket, hajtsa végre az alábbi eljárást követve állítsa be a környezet:

1.  Visual Studio két projektet, "Ügyfélprogram" és "Szolgáltatás", a megoldást belül tartalmazó console-alkalmazás létrehozása.
2.  A Microsoft Azure Service Bus NuGet csomag hozzá mindkét projektet. A csomag hozzáadása a projekthez szükséges összeállítás hivatkozásokat.

### <a name="how-to-create-the-service"></a>A szolgáltatás létrehozása

Első lépésként hozzon létre a szolgáltatást, magát. Bármely WCF-alapú szolgáltatás legalább három különálló részből áll:

-   Milyen üzenetváltás és műveletek vannak hívható leíró szerződést meghatározása.
-   Említett szerződés végrehajtását.
-   A Host, amely a WCF-alapú szolgáltatás üzemelteti, és több végpontok közzététele.

Ebben a részben mintakódok cím minden összetevőt.

A szerződés definiálja egy lépésben `AddNumbers`, összeadja a két szám és az eredményt adja vissza. A `IProblemSolverChannel` felület lehetővé teszi, hogy az ügyfél, a proxy élettartam könnyebben kezelheti. Az ilyen felületet létrehozása javasoljuk egy. Érdemes a szerződés definíciója üzembe egy külön fájlba, hogy a "Ügyfél" és a "Szolgáltatás" projektekből fájlra mutató hivatkozás, de be mindkét projektet is másolja a kódot.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

A szerződés helyen végrehajtása trivial.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>A szolgáltatás szolgáltató programozás útján konfigurálása

A szerződés és végrehajtási helyben most üzemeltetheti a szolgáltatást. Szolgáltatója egy [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) objektumban, amely az szolgáltatás példányainak kezelése gondoskodik fordul elő, és az üzenetek meghallgatása, a végpontok szolgáltatja. A következő kódot a szolgáltatást a rendszeres helyi végpont és egy szolgáltatás Bus végpontot, az alábbi módokon egymás mellett, belső és külső végpontok szemléltetésére is beállítható. A karakterlánc *névtér* cserélje a névtér neve és a *yourKey* szerezte be az előző lépésben a telepítő Társítások kulccsal.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

A példában szereplő hoz létre azonos szerződés végrehajtása a végpontokat. Egy helyi, és egy van tervezett szolgáltatás Bus keresztül. A főbb különbségek a kötések; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) a helyi egy és [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) a szolgáltatás Bus végpont és a címeket. A helyi végpont különböző port a helyi hálózaton címmel rendelkezik. A szolgáltatás Bus végpont címmel rendelkezik egy végpont a karakterlánc állhatnak `sb`, a névtér nevét, és az elérési út "solver." Ennek eredményeképpen a URI `sb://[serviceNamespace].servicebus.windows.net/solver`, a szolgáltatás végpontjának azonosítja a szolgáltatás Bus TCP végpontjának egy külső teljesen minősített tartománynév. Ha a kódot be a helyőrzők cseréje a `Main` függvény **a szolgáltatásalkalmazás** funkcionális szolgáltatás lesz. Ha azt szeretné, hogy a szolgáltatást, hogy kizárólag a szolgáltatás Bus figyelje, távolítsa el a helyi endpoint rekorddeklarálás.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>A szolgáltatás szolgáltató beállítása az App.config fájl

A host App.config fájl használatával is beállítható. A szolgáltatás szolgáltatója ebben az esetben a kód megjelenik a következő példában.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Végpont-definícióban az App.config fájl áthelyezése. A NuGet csomag a App.config fájlt, hogy melyik szolgáltatás Bus szükséges konfigurációs kiterjesztéseinek már bejegyezte definíciók cellatartományban. A következő példa, amely pontosan megegyezik a az előző kódot, közvetlenül a **system.serviceModel** elem alatt jelenik meg. A példa feltételezi, hogy, hogy a projekt C# névtér **szolgáltatás**neve.
A helyőrzők cseréje a szolgáltatás Bus szolgáltatás névtere és Társítások billentyűt.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Miután elvégezte a módosítások, a szolgáltatás elindul, mint előtt, de az élő végpontokat: egy helyi és egy figyeli a felhőben.

### <a name="create-the-client"></a>Az ügyfél létrehozása

#### <a name="configure-a-client-programmatically"></a>Ügyfél programozás útján konfigurálása

A szolgáltatás használhatnak, a WCF-ügyfél használata egy [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektum állíthat össze. Szolgáltatás Bus Társítások használatával megvalósított biztonsági jogkivonat-alapú modellt használ. A [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) osztály jelöli a biztonsági jogkivonat-szolgáltató néhány közismert jogkivonat szolgáltatók visszaadó beépített gyári módszerekkel. Az alábbi példában a [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) módszer a megfelelő Társítások jogkivonat megszerzése kezelheti. A név és a kulcs azok kapott a portálról, az előző szakaszban leírtak szerint.

Első lépésként hivatkozást vagy másolni a `IProblemSolver` szerződés kódot a szolgáltatásból az ügyfél projektbe.

Majd helyettesítse be a kódot a `Main` az ügyfél ismét a helyőrző szöveg cseréje a szolgáltatás Bus névtér és Társítások kulcs metódusát.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Most már létrehozhat az ügyfél és a szolgáltatást, mutassa meg (először futtassa a szolgáltatás), és az ügyfél felhívja a szolgáltatást, és **9**nyomtatja. Futtathatja az ügyfél- és kiszolgálóoldali különböző számítógépeken, akár hálózaton keresztül, és a kommunikáció is működnek. Az ügyfél-kódot a felhőben vagy helyben is futtatható.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Ügyfél konfigurálása a App.config fájlban

A következő kódot megtudhatja, hogy miként állítsa be az ügyfél App.config fájl használatával.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Végpont-definícióban az App.config fájl áthelyezése. A következő példa, amely megegyezik a korábban már szerepel a felsorolásban kódot, közvetlenül a **system.serviceModel** elem alatt jelenik meg. Ebben az esetben, mielőtt ezt kell lecserélnie a helyőrzők a szolgáltatás Bus névtér és Társítások billentyűt.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta a szolgáltatás Bus továbbítási szolgáltatáshoz alapjait, kövesse az alábbi hivatkozásokra kattintva további.

- [Szolgáltatás Bus továbbítását üzenetküldés – áttekintés](service-bus-relay-overview.md)
- [Azure Service Bus építészeti – áttekintés](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Töltse le a szolgáltatás Bus minták [Azure minták][] , vagy a [szolgáltatás Bus minták – áttekintés][]című témakörben találhat.

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure minták]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [a szolgáltatás Bus minták – áttekintés]: ../service-bus-messaging/service-bus-samples.md