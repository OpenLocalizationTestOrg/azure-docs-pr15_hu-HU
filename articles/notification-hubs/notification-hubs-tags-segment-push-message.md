<properties
    pageTitle="Útválasztás és a címke kifejezések"
    description="Ez a témakör ismerteti az Azure értesítés hubok Útválasztás és a címke kifejezések."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="routing-and-tag-expressions"></a>Továbbítás és a címke

##<a name="overview"></a>– Áttekintés

Címke kifejezések lehetővé tevő cél adott készleteket eszközök vagy pontosabban regisztrációk, ha keresztül értesítés hubok leküldéses értesítést küld.


## <a name="targeting-specific-registrations"></a>Adott regisztrációk célba juttatása

Csak úgy lehet cél adott értesítést regisztrációk címkék társíthatja őket, majd Megcélozhat címkék. Tárgyalt [Regisztrációs](notification-hubs-push-notification-registration-management.md)kezelése, annak érdekében, hogy a leküldéses értesítések fogadása alkalmazás rendelkezik regisztrálhatja a eszköz fogópont értesítés hubon. Amikor regisztráció értesítés hubon létrejött, az alkalmazás kódmentes küldhet leküldéses értesítéseket neki.
Az alkalmazás kódmentes választhat a regisztrációk célba az adott értesítés az alábbi módon:

1. **Közvetítése**: az értesítési-központban összes regisztrációja az értesítést kap.
2. **Címke**: a megadott címkét tartalmazó összes regisztrációja az értesítést kap.
3. **Címke kifejezés**: minden regisztrációk, amelynek címkék csoportja a megadott kifejezéssel a értesítést kap.

## <a name="tags"></a>Címkék

Címke lehet tetszőleges karakterlánc legfeljebb 120 karaktereket tartalmazó alfanumerikus és a következő nem alfanumerikus karaktereket: "_", ‘@’, "#", ".",":", "-". A következő példa bemutatja egy alkalmazást, amelyből bejelentési értesítés a csoportok konkrét zenét kaphat. Ebben az esetben egy egyszerű útvonal értesítések módja a különböző sávokat, ahogy az alábbi képen jelölő címkékkel címke regisztrációk.

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Ezen a képen az üzenet címkézett **Beatles** eléri csak a címke **Beatles**regisztrált a táblagépen.

Regisztráció címkék létrehozásával kapcsolatos további tudnivalókért olvassa el a [Regisztrációs kezelés](notification-hubs-push-notification-registration-management.md).

Küldés értesítések módszereinek használatával értesítések elküldheti a `Microsoft.Azure.NotificationHubs.NotificationHubClient` osztály a [Microsoft Azure értesítés hubok](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK csomagjában talál. Node.js, illetve a leküldéses értesítések REST API-khoz is használhatja.  Íme egy példa a SDK használatával.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




Címkék nem kell előre kiépített, és több alkalmazás különösebb is hivatkozhat. Ez a példa alkalmazás felhasználóinak például megjegyzések hozzáfűzése a sávok és toasts, nem csak a kedvenc sávokra kattintson a megjegyzések, hanem az összes megjegyzés kapni a ismerősök, függetlenül attól, amelyen megjegyzésekről a sávon. A következő képen látható, ebben az esetben egy példája:



![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Ezen a képen Anna a Beatles frissítései érdekli, akinek Péter érdekli a Wailers frissítéseit. Péter is kíváncsi Charlie a megjegyzéseket, és Charlie szerepel a Wailers iránt érdeklődik. A Beatles megjegyzést Charlie meg értesítés küldésekor Anna és a Péter kapta meg.

Több aggályokat címkéket (például "band_Beatles" vagy "follows_Charlie") is kódolását, miközben címkék olyan egyszerű karakterláncok, és nem értékű tulajdonságait. Regisztráció a csak a jelenléti vagy egy adott címke hiányában megegyezik.

Egy teljes lépésenkénti oktatóanyagban küldésének kamat csoportoknak a címkék használata a [Megszakítása hírek](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)című témakör tartalmaz.


## <a name="using-tags-to-target-users"></a>Cél felhasználók-címkék használata

Címkék használata másik úgy, hogy azonosítsa az adott felhasználó összes eszközt. Regisztráció címkével ellátott, amely tartalmazza a felhasználói azonosító, ahogy az alábbi képen is címkézésére:


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Ezen a képen a megjelölt üzenet uid:Alice eléri összes regisztrációk címkézett uid:Alice; Ennélfogva minden Anna eszközöket.


##<a name="tag-expressions"></a>Címke kifejezések

Előfordulhatnak olyan esetek értesítést regisztrációk csoportja, amely azonosítja, de nem egyetlen címkét a címkék logikai kifejezés célba tartalmaz.

Fontolja meg egy sports alkalmazást, amely egy emlékeztetőt Boston minden résztvevőjének kapcsolatos játék a piros Sox és Cardinals között. Ha az ügyfél alkalmazás kamat helyét és a csoportoknak a címkék regisztrálja, majd az értesítést kell kell célzott, ki a vörös Sox vagy a Cardinals szeretne Boston minden résztvevőjének. Ez a feltétel és a következő logikai kifejezés számítható ki:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Címke kifejezések tartalmazhat összes logikai operátorok, például: és (& &), vagy (|), és nem (!). Is tartalmazhatnak a zárójelek között. Címke azonban legfeljebb 20 címkéket tartalmaznak csak ORs; egyéb esetben 6 címkékre korlátozott.

Íme egy példa az értesítés küldése a SDK használatával címke kifejezésekkel együtt.


    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)"; 

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Socks</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
