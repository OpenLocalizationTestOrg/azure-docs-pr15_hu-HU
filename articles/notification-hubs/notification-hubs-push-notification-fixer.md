<properties 
    pageTitle="Azure értesítés hubok - diagnosztizálása útmutató" 
    description="Azure értesítés hubok kapcsolatos gyakori hibák diagnosztizálása szabályai." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure értesítés hubok - diagnosztizálása útmutató

##<a name="overview"></a>– Áttekintés

Azt az Azure értesítés hubok ügyfelek meghallgatásához a leggyakoribb kérdések egyike van hogyan megállapíthatja, hogy miért nem látják az alkalmazás kódmentes küldött értesítés jelenik meg az ügyfél eszköz – Ha, és miért értesítések megszakadt és a javítás módja. Ebben a cikkben azt fogja hajtsa végre a különféle okok miért értesítések kerülhetnek, vagy ne zárja az eszközön. Azt is, amelyben elemzése és tudni, a kiváltóok módszereit keresztül fog megjelenni. 

Első lépésként rendkívül fontos, ha meg szeretné érteni, hogy hogyan Azure értesítés hubok verembe küldi értesítéseket az eszközökön.
![][0]

Egy tipikus küldés értesítési folyamat az üzenet érkezik az **alkalmazás kódmentes** az **Azure értesítés központi (NH)** , amely az egyes feldolgozási összes regisztrációk, figyelembe véve a beállított címkék és a címke kifejezések határozza meg a "célok", tehát minden a regisztrációk van szükség a leküldéses értesítést szeretne kapni a. Ezek regisztrációk közül a támogatott platformok – iOS, Google, a Windows, a Windows Phone, át is kiterjedhet Kindle és az Android Kína Baidu. A célok legyenek kialakítva, miután NH, majd az értesítéseket, veremmutatót felosztása több köteg regisztrációk, az eszköz platformra keresztül adott **Leküldéses értesítést szolgáltatás (PNS)** – például az Apple APNS, GCM a Google stb. NH hitelesíti a megfelelő PNS beállíthatja az Azure klasszikus portálra értesítési központi konfigurálása lapon a hitelesítő adatok alapján. A PNS továbbítja az **Ügyféleszközök**az értesítést. Ez az a platform ajánlott módja előadása a leküldéses értesítéseket, és figyelje meg, hogy a végleges láb szállítási értesítés történik a platform PNS és az eszköz között. Ezért négy fő összetevői - *ügyfél*, az *alkalmazás kódmentes*van, *Azure értesítés hubok (NH)* és a *Leküldéses értesítést szolgáltatások (PNS)* , és ezek egyikét okozhat értesítések megszakad. További információra kíváncsi, ez architektúra [Értesítés hubok áttekintése]érhető el.

Értesítések előadásához hiba akkor fordulhat elő a kezdeti próba előkészítése során, fázis, amely jelezheti, hogy a konfigurációs probléma, vagy előfordulhat, hogy hol teljes vagy részleges az értesítéseket is kell első kihagyott néhány mélyebb alkalmazás jelző vagy minta probléma üzenetküldés gyártási fordul elő. Szakaszban alatti fog megnézi az egyes milyen, melyek akkor azt tapasztalhatja egyértelmű és néhány mások nem annyira a gyakori alaptartományban különböző kihagyott értesítéseket helyzeteket. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure értesítések központi nem megfelelő konfiguráció 

Azure értesítés hubok van szüksége, és hitelesítse magát a fejlesztői alkalmazást engedélyezni szeretné a sikeres értesítést küld a megfelelő PNS környezetében. Ez történik lehetséges Fejlesztőeszközök fiók létrehozása és a megfelelő platform (Google, Apple Windows stb.), és ezután regisztrálása alkalmazásuk hol hozzá őket a hitelesítő adatait, amelyek kell beállítania a portálon értesítés hubok csoportban a fejlesztő által. Ha nincs értesítések keresztül készít, első lépésként annak érdekében, hogy a megfelelő hitelesítő adatok vannak-e beállítva az egyező őket az alkalmazás platform adott Fejlesztőeszközök fiókjukat alapján létre értesítést-központban kell lennie. Az [Első lépések oktatóanyagok] kifejezésre keresve hasznos lehet ahhoz, hogy a step by step módon keresztül folyamathoz lépjen. Íme néhány gyakori hibás beállításokat:

1. **Általános**
 
    a) Győződjön meg róla, hogy az értesítési központi nevének (nélkül elírásából adódnak) ugyanaz:

    - Amikor regisztrál az ügyféltől 
    - Ha szeretné elküldeni a értesítések a kódmentes, a  
    - Ha PNS hitelesítő adatok állította be, és 
    - Az ügyfél és a kódmentes állította be, akinek Társítások hitelesítő adatait. 
        
    b) Győződjön meg arról, hogy a helyes Társítások konfigurációs karakterláncok használ az ügyfél és az alkalmazás kódmentes. Egy szabály a legjobb megoldás, mint kell használnia a **DefaultListenSharedAccessSignature** az ügyfélen és **DefaultFullSharedAccessSignature** meg az alkalmazás kódmentes (engedélyezni szeretné, hogy értesítést küld a NH jogosultsági diagramalakzatok)

2. **Az Apple leküldéses értesítést szolgáltatás (APNS) konfigurálása**
 
    Két különböző hubok – egy gyártási meg kell tartania, és egy másik tesztelési célú. Ez azt jelenti, hogy a fogja használni egy külön hubhoz védőfalas környezetben tanúsítvány és a tanúsítvány fogja használni egy külön hubhoz gyártási feltöltése. Nem próbálják meg feltölteni különböző típusú oklevélsablonok az azonos hubon keresztül csatlakozott, akkor jelenhet meg értesítés hibák a sorral lejjebb. Ha saját maga abban a helyzetben, ha véletlenül feltöltött saját tanúsítvány különböző típusú ugyanahhoz a hubhoz talál, ajánlatos az-központban, és ezután friss. Ha valamiért nem tudja törölni a központi, majd legalább, törölnie kell a meglévő regisztrációk a központból. 

3. **A Google Cloud üzenetküldés (GCM) konfigurálása** 

    a) Győződjön meg arról, hogy engedélyezi a "A Google Cloud üzenetküldés for Android" csoportban a felhőben projekt. 
    
    ![][2]
    
    b) Győződjön meg arról, hogy a "Server kulcs" létrehozása közben GCM hitelesíteni a hitelesítő adatok beszerzése a mely NH fogja használni. 
    
    ![][3]
    
    c) Győződjön meg arról, hogy az ügyfél, amely az irányítópult szerezhet teljesen numerikus egyed "Projekt Azonosítót" konfigurálva:
    
    ![][1]

##<a name="application-issues"></a>Alkalmazás-hibák

1) **Címkék és kifejezések nyomon követése**

A közönség szakaszokhoz címkék vagy címke kifejezések szolgáltatást használ, érdemes mindig lehetséges, hogy amikor az értesítést küld, akkor nem található a megadása esetén a Küldés hívásban címkék/címke kifejezések alapján cél. Célszerű, tekintse át a regisztrációk győződjön meg arról, hogy nincsenek-e címkék, amelynek megfelelően értesítésének elküldése, és ellenőrizze az értesítési visszaigazolás csak az adott regisztrációk ügyfélszámítógépekről. Pl. Ha szeretné elküldeni a címkével ellátott értesítést "Sports" a regisztrációk a NH befejeződött volna az összes mondja ki a címke "Politikában", akkor nem küld bármilyen eszközről. Összetett eset jelenthet hol csak regisztrálta "Címke A" vagy "Címke B", de az értesítés küldése közben címke kifejezések céloz "Címke A & & címke B". Szakaszában önálló diagnosztizálása tippek az alábbi módon, ahol áttekintheti a regisztrációk, a címkék rendelkeznek együtt. 

2) **Sablon kapcsolatos problémák**

Sablonok használata, majd győződjön meg arról, hogy ha követik a [sablon útmutatást]a ismertetett irányelvek. 

3) **Érvénytelen regisztrációk**

Feltételezve, hogy helyesen konfigurált az értesítési-központban, és a címkék/címke kifejezéseket használták a megkeresése, amelyekhez az értesítéseket szeretne küldeni érvényes célok megfelelően így, NH akkor indul el, párhuzamosan – minden egyes üzeneteket küld egy sor olyan regisztrációk tétel több feldolgozás kötegekben ki. 

> [AZURE.NOTE] Mi a feldolgozás párhuzamosan, mivel azt nem garantálja a sorrendben, amelyben az értesítések kerülnek. 

Azure értesítések központi most egy "a legtöbb egyszer" üzenet kézbesítési modell van optimalizálva. Ez azt jelenti, hogy egy megszüntetése ismétlődések azt megpróbálja, hogy nincs értesítések többször érkeznek egy eszközt. Győződjön meg róla, ez azt nézze át a regisztrációk, és győződjön meg arról, hogy csak egy üzenetet küld egy eszközazonosítót ténylegesen küldése az üzenet a PNS előtt. Minden tétel a szolgáltatás elküldi a PNS, amely viszont elfogadása és a regisztrációk érvényesítése, akkor lehet, hogy a PNS észleli egy vagy több regisztrációk kötegben, hiba történt az Azure NH hibát ad vissza, és leállítja a feldolgozása, ezáltal leválasztása teljesen, hogy a köteg. Ez az APN, amely a TCP-adatfolyam protokollt használó különösen igaz. Bár azt szolgáltatáshoz optimalizált a legtöbb után a kézbesítés szakaszt, ez az utóbbi esetben azt a hibát okozó regisztráció eltávolítása az adatbázist, majd próbálkozzon újra az értesítési kézbesítési – a többség az eszközöket, hogy a köteg.

Akkor is hibainformációk a sikertelen kézbesítési kísérlet az Azure értesítés hubok REST API-k használata regisztráció ellen: [üzenet Telemetriai: értesítő üzenet Telemetriai első](https://msdn.microsoft.com/library/azure/mt608135.aspx) és [PNS visszajelzés](https://msdn.microsoft.com/library/azure/mt705560.aspx). Lásd: a [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) például kódot.

##<a name="pns-issues"></a>PNS kapcsolatos problémák

Miután az értesítő üzenet megkapta a megfelelő PNS majd feladata annak végzi az értesítés az eszközt. Azure értesítés hubok kívül az alábbi a képet, és nincs hozzáféréssel rendelkezik, amikor, vagy ha az értesítés fog az eszközön, kézbesítése. Mivel a platform értesítési szolgáltatások meglehetősen hatékony, értesítések eléréséhez eszközök néhány másodperc alatt a PNS az általában. Ha a PNS azonban van szabályozásának Azure értesítés hubok alkalmazhatja az exponenciális vissza stratégia ki, és ha a PNS 30 percig is elérhető marad, majd felkínálunk egy házirend helyen jár le, és azokat az üzeneteket véglegesen közvetlen. 

Ha egy PNS megpróbál értesítést, de az eszköz offline állapotban, az értesítés tárolja a PNS korlátozott ideig, és kézbesítve az eszközre, amikor elérhetővé válik. Egy app csak egy legutóbbi vonatkozó vannak tárolva. Ha több értesítést küld az eszköz kapcsolat nélküli módban, minden új értesítést eredményezi, hogy az előzetes értesítést figyelmen kívül hagyható. Viselkedését megőrzése a csak a legújabb értesítés nevezik értesítések az APN coalescing és becsukása a GCM (amely összecsukás kulcsot használ). Ha az eszköz marad offline ideig, nem őrződnek meg, hogy tárolt értesítések. Adatforrás - [APN útmutatást] & [GCM útmutató]

Az Azure értesítés hubok - átviheti coalescing kulcs használatával az általános HTTP-fejlécben keresztül `SendNotification` API (.NET SDK – például `SendNotificationAsync`), amely szintén veszi HTTP-fejlécek, amelyek másként átadott azt javasoljuk, hogy a megfelelő PNS. 

##<a name="self-diagnose-tips"></a>Önálló diagnosztizálása tippek

Itt azt megvizsgálja a különböző eszközöket diagnosztizálása és a legfelső szintű minden olyan értesítési központi problémákat okoznak:

###<a name="verify-credentials"></a>Hitelesítő adatok ellenőrzése

1. **PNS developer Portal segítségével**

    Ellenőrizze a megfelelő PNS developer Portal segítségével (APNS, GCM, WNS stb) az [Első lépések oktatóanyagok]segítségével a őket.

2. **Azure klasszikus portál**

    Nyissa meg a beállítás lapon tekintheti át és a hitelesítő adatok felel meg azokat a PNS developer Portal segítségével nyert. 

    ![][4]

###<a name="verify-registrations"></a>Regisztráció ellenőrzése

1. **Visual Studio**

    Visual Studio fejlesztési használatakor, Csatlakozás Microsoft Azure és megtekintése és kezelése kitöltenem, az Azure szolgáltatást, többek között az értesítések központi "Kiszolgáló Intézőből". Ez akkor elsősorban hasznos fejlesztők/próba-környezetben. 

    ![][9]

    Megtekintheti és kezelése a központi amelynek megfelelően legyenek rendezve kategorizálják platform natív regisztrációk vagy sablon regisztrációs, címkéket, PNS azonosító, regisztrációs azonosító és a lejárati dátum. Regisztráció menet – akkor hasznos, tegyük fel, ha a szerkesztendő címkéket is szerkesztheti. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio funkciókat regisztrációk szerkesztése csak korlátozott számú regisztrációk fejlesztők/vizsgálat során kell használni. Javítsa ki a regisztrációk tömeges szükség esetén, érdemes az exportálási/importálási regisztrációs funkció használata az alábbiakban ismertetjük - [Exportálási/importálási regisztrációk] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Szolgáltatás Bus explorer**

    Vevőknek ServiceBus explorer az alábbiakban ismertetjük - [ServiceBus Explorer] megtekintésére és azok értesítés központi kezelésére használható. Egy megnyitott forrásprojektben code.microsoft.com - [ServiceBus Explorer kód] elérhető

###<a name="verify-message-notifications"></a>Ellenőrizze a szóló értesítések

1. **Azure klasszikus portál**

    Látogathat el a "Hibakeresési" lap próba értesítéseket küldeni az ügyfelek erre szolgáló bármilyen szolgáltatás kódmentes be és futtatása nélkül. 

    ![][7]

2. **Visual Studio**

    Próba értesítést küldhet a Visual Studio comforts is:

    ![][10]

    Erről további Itt – a Visual Studio értesítés központi Azure explorer szolgáltatásai 
    
    - [VIEWBEN kiszolgáló Explorer – áttekintés]
    - [VIEWBEN kiszolgáló Explorer blogbejegyzés - 1]
    - [VIEWBEN kiszolgáló Explorer blogbejegyzés - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Nem sikerült értesítések hibakeresési / értesítés eredmény áttekintése

**EnableTestSend tulajdonság**

Értesítés hubok keresztül értesítést küld, amikor először azt csak kap várakozik minden cél megállapítása feldolgozása elvégzendő NH, és majd végül NH elküldi a PNS. Ez azt jelenti, hogy REST API-val, vagy az ügyfél SDK használja, a Küldés hívás a sikeres visszatérési csak azt jelenti, hogy, hogy az üzenet van már sikeresen aszinkron az értesítési központi. Mi történt, amikor NH ahányat használ az üzenetet küldeni PNS betekintést nem nyújt túl. Az értesítésben, a rendszer nem megérkezni az az ügyféleszközről, ha nincs lehetősége, hogy amikor az üzenet kézbesítése PNS próbált NH, hiba történt, például a tartalom fájlméret túllépve a PNS a megengedett vagy konfigurálva NH a hitelesítő adatok érvénytelen stb. Úgy juthat az PNS hibák betekintést, azt vezettek [EnableTestSend szolgáltatás]nevű tulajdonságot. Ez a tulajdonság esetén automatikusan aktiválja a portálon vagy Visual Studio ügyfél próba üzenetek küldésére, és ezért lehetővé teszi, hogy hibakeresési részletes információkat talál. Ezzel a példában a .NET SDK hol érhető el most véve API-khoz keresztül, és végül hozzáadódik az összes ügyfelet SDK. Szeretne használni ezt a többi beszélgetést, egyszerűen "tesztelése" pl. nevű végén található a Küldés hívás paraméteres lekérdezési karakterlánc összefűzése 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Példa (.NET SDK)*
 
Tegyük fel, .NET SDK natív bejelentési értesítés küldése szolgáltatást használ:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`fog egyszerűen állapot `Enqueued` anélkül, hogy mi történt a leküldéses bármely betekintést végrehajtása végén. Most is használhatja a `EnableTestSend` inicializálásakor logikai tulajdonság a `NotificationHubClient` és is használható lesz a PNS hiba lépett fel az értesítés küldése kapcsolatos részletes állapotát. A Küldés hívást ide vissza, mert csak visszaadása, miután NH nyilvánított az értesítés PNS az így megállapíthatja, hogy további ideig tart. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Példa eredménye*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Ez az üzenet jelzi, vagy érvénytelen hitelesítő adatok vannak beállítva az értesítési-központban vagy a regisztrációk a központi a problémát, és a javasolt tanfolyam lenne a regisztráció törli, és az ügyfél hozza létre ismét az üzenet elküldése előtt mindig legyen. 
 
> [AZURE.NOTE] Figyelje meg, hogy ez a tulajdonság e erősen folyamatban van, és így csak akkor kell használnia a fejlesztők/tesztkörnyezetben regisztrációk való korlátozott. Csak elküldjük hibakeresési értesítések 10 eszközöket. Azt is, hogy legfeljebb 10 percenkénti hibakeresési küld feldolgozása. 

###<a name="review-telemetry"></a>Telemetriai tekintse át 

1. **Azure klasszikus portál használata**

    A portál lehetővé teszi a tevékenység gyors áttekintést kaphat a az értesítési központi. 
    
    a) a lapról "irányítópult" megtekintheti a regisztrációk, értesítések, valamint / platform hibák összesített nézete. 
    
    ![][5]
    
    b) is felvehet számos más platform adott mértékek különösen a bármely PNS adott hibát amikor megpróbálja a értesítést küld a PNS NH mélyebb nézze meg a "Képernyő" fülre. 
    
    ![][6]
    
    c), kell kezdődnie a **Bejövő üzeneteket**, **Bejegyzési műveletek** **Sikeres értesítések** áttekintése és kattintson / platform fülre, és tekintse át a PNS adott hibákat. 
    
    d) Ha az értesítési-központban megfelelően beállítva a hitelesítési beállítások, majd PNS hitelesítési hiba jelenik meg. Ez a PNS hitelesítő adatok ellenőrzése áttekintő képet. 

2) **Hozzáférés**

További részletek az alábbi- 

- [Programozási Telemetriai Access]
- [Telemetriai hozzáférésének API-khoz minta] 

> [AZURE.NOTE] Több telemetriai kapcsolódó funkciók **Exportálási/importálási regisztrációk**, például az **API-khoz Telemetriai hozzáférésének** stb csak akkor érhetők szabványos réteg. Ha megpróbálja használja ezeket a funkciókat, ha az ingyenes vagy egyszerű réteg ilyenkor kivételhiba üzenete erről a SDK és a HTTP 403 (tiltott) használatakor közvetlenül a REST API-hoz a használatuk során. Győződjön meg arról, hogy Ön át lett helyezve Standard felfelé az első csoportba tartozó Azure klasszikus portálon keresztül.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Értesítés hubok – áttekintés]: notification-hubs-push-notification-overview.md
[Az első lépéseket ismertető oktatóanyagok]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Sablon útmutató]: https://msdn.microsoft.com/library/dn530748.aspx 
[APN-útmutató]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM útmutató]: http://developer.android.com/google/gcm/adv.html
[Exportálási/importálási regisztrációk]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer kódot.]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[VIEWBEN kiszolgáló Explorer – áttekintés]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VIEWBEN kiszolgáló Explorer blogbejegyzés - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VIEWBEN kiszolgáló Explorer blogbejegyzés - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend szolgáltatás]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programozási Telemetriai Access]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetriai hozzáférésének API-khoz minta]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 