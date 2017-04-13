<properties 
    pageTitle="Esemény hubok hitelesítési és biztonsági modell áttekintése |} Microsoft Azure"
    description="Esemény hubok hitelesítési és biztonsági modell áttekintése."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Esemény hubok hitelesítési és biztonsági modell – áttekintés

Az esemény hubok biztonsági modellje megfelel az alábbi követelményeknek:

- Csak olyan eszközöket, hogy érvényes hitelesítő adatokkal küldhet adatokat esemény-hubon keresztül csatlakozott.
- Egy eszközt nem megszemélyesítés egy másik eszközt.
- Egy engedélyezetlen eszközt is blokkolhatja adatok küldésének esemény-hubon keresztül csatlakozott.

## <a name="device-authentication"></a>Eszköz hitelesítés

Az esemény hubok biztonsági modell [Megosztott Access-aláírás (Társítások)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) tokenek és *esemény közzétevők*alapul. Egy esemény publisher egy virtuális végpontot egy esemény hubhoz határozza meg. A publisher csak használható üzenetek küldéséhez esemény-hubon keresztül csatlakozott. Még nem lehetséges közzétevőtől származó üzenetek fogadására.

Általában egy esemény hubhoz alkalmaz egy publisher egy eszközt. Valamelyik, az közzétevők, egy esemény hubhoz küldött összes üzenet belül adott esemény elosztót sorbaállítva. Közzétevők engedélyezze külön hozzáférés-vezérlés és szabályozása.

Az egyes egyedi token, az eszköz feltöltött van-e hozzárendelve. A tokenek, hogy az egyes egyedi jogkivonat hozzáférést biztosít a különböző egyedi közzétevő darab termék készült. Jogkivonat rendelkezik eszközt csak egy publisher, de nem a publisher küldhet. Ha több eszközzel azonos megosztásához minden ezekre az eszközökre közzétevő oszt ki.

Bár nem javasolt, akkor lehet ellátására alapelemeket, esemény-hubon keresztül csatlakozott közvetlen hozzáférést biztosító eszközök. Bármilyen eszközről, amelynek jogkivonat tárolja, ez közvetlenül az adott esemény elosztót üzeneteket küldhet. Az ilyen eszköz nem esetektől szabályozásának. Ezenkívül az eszköz nem kell feketelistára teszi, az adott esemény elosztót küld.

Az összes tokenek bejelentkezve Társítások használatával. Az összes tokenek általában ugyanazzal a kulccsal bejelentkezve. Eszközök nem ismerik a kulcsot. Ez a tokenek gyártási eszközök megakadályozza.

### <a name="create-the-sas-key"></a>A Társítások kulcs létrehozása

Egy esemény hubok névtér létrehozásakor az Azure esemény hubok **RootManageSharedAccessKey**nevű 256 bites Társítások kulcs hoz létre. A fő támogatás küldése, meghallgatása és a névtér jogok kezelése. További billentyűk hozhat létre. Javasoljuk, hogy a egy kulcsot, hogy támogatást küldeni engedélyeket az adott esemény-központban konzerv. Ez a témakör a hátralévő, feltételezett értéke, hogy a következő kulcsot nevű `EventHubSendKey`.

Az alábbi példa létrehoz egy csak küldés kulcsot, amikor létrehozásáról az esemény-központban:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Tokenek készítése

A Társítások kulccsal tokenek hozhat létre. Csak egy jogkivonat egy eszközt kell dolgoznia. Tokenek az alábbi módszerrel kattintson előállított. Az összes tokenek jönnek létre a **EventHubSendKey** gombbal. Minden egyes jogkivonat egyedi URI van-e hozzárendelve.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Ez a módszer hívásakor a URI kell megadni `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Az összes tokenek, a URI megegyezik, kivéve a `PUBLISHER_NAME`, amelyek minden jogkivonat különböző kell lennie. Ideális esetben `PUBLISHER_NAME` kap, hogy a token eszköz azonosítója jelöl.

Ezzel a módszerrel hoz létre a következő szerkezettel token:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

A token lejárati idő a január 1, 1970 másodpercben van megadva. Az alábbi képen egy token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Általában a tokenek van egy hasonló, vagy az eszköz a élettartam meghaladja élettartam. Ha az eszköz képes szerezzen be egy új jogkivonat, a dolgozók rövidebb élettartam a tokenek használható.

### <a name="devices-sending-data"></a>Adatok küldése eszközök

A tokenek létrehozása után az egyes saját egyedi token van kiépítve.

Ha az eszköz adatokat küld egy esemény elosztóhoz, az eszköz a jogkivonat, a Küldés kérésével címkéket tartalmaznak. Lehallgatás, illetve a ellopásának megakadályozására a token támadó elkerülése érdekében az eszközt, és az esemény-központban közötti kommunikáció kell csatornán egy titkosított.

### <a name="blacklisting-devices"></a>Feketelista eszközök

Ha támadó ellopják jogkivonat, a a támadó is megszemélyesítés az eszközt, amelynek jogkivonat ellopott. Egy eszközt blacklisting jeleníti meg az eszköz használhatatlanná mindaddig, amíg az eszköz számítható ki egy új jogkivonat, amely egy másik közzétevőt használja.

## <a name="authentication-of-back-end-applications"></a>Hitelesítés háttér-alkalmazások

Hitelesítést végezni az eszközök által generált adatok felhasználása háttér-alkalmazásokat, esemény hubok alkalmaz egy olyan biztonsági modellel, amely hasonlít az a modellt, amellyel szolgáltatás Bus témakörök. Egy esemény hubok fogyasztói csoport függvény egyenértékű a szolgáltatás Bus témakörben előfizetést. Egy ügyfél fogyasztói csoport hozhat létre, ha az értekezlet-összehívást a fogyasztói csoport létrehozása a jogkivonat, hogy támogatást jogosultságok kezelése, az esemény-központban, illetve a névtér, amelyhez az esemény-központban tartozik. Egy ügyfél engedélyezett használhatnak fogyasztói csoport adatait, ha a fogadási kérelmet jogkivonat, amely fogadási jogokat adott fogyasztói csoport, az esemény-központban vagy a névtér, amelyhez az esemény-központban tartozik.

Az aktuális verziójának szolgáltatás Bus Társítások szabályok nem támogatja az egyes előfizetések. Ugyanez igaz esemény hubok fogyasztói csoportok. Társítások támogatása a jövőben belekerül a két szolgáltatás.

Az egyes fogyasztói csoportok Társítások hitelesítési hiányában a Társítások billentyűk segítségével összes fogyasztói csoport biztonságos közös használatával. Ezt a megközelítést lehetővé teszi, hogy az alkalmazás bármely olyan esemény központi fogyasztói csoportok adatainak felhasználni.

## <a name="next-steps"></a>Következő lépések

Többet szeretne tudni az esemény hubok, látogasson el az alábbi témaköröket:

- [Esemény hubok – áttekintés]
- Egy [üzenetben megoldás aszinkron] használ szolgáltatási Bus sorban várakozó.
- Egy teljes [minta alkalmazást használó esemény hubok].

[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[aszinkron üzenetben megoldás]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
