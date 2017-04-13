<properties
    pageTitle="Azure mobilalkalmazásai Apache Cordova beépülő modul segítségével"
    description="Azure mobilalkalmazásai Apache Cordova beépülő modul segítségével"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Hogyan használható a Apache Cordova ügyfél könyvtár Azure mobilalkalmazásai

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad a legújabb [Azure Mobile-appokról Apache Cordova beépülő modul]használatával esetei végezheti el. Ha még kezdő az első teljes [Azure Mobile alkalmazások Quick Start] hozzon létre egy a kódmentes Azure Mobile-alkalmazások hozzon létre egy táblázatot, és töltse le a beépített Apache Cordova projekt. Ebből az útmutatóból azt kiemelése a ügyféloldali Apache Cordova bővítményt.

## <a name="supported-platforms"></a>Támogatott platformok

Ez az SDK támogatja Apache Cordova v6.0.0, és később iOS, Android és Windows eszközöket.  A platform a rendszer támogatja az alábbi képlettel történik:

* Android API 19-24 (KitKat nugát keresztül)
* iOS 8.0-s vagy újabb verziója.
* Windows Phone 8.0-ban
* Windows Phone 8.1
* Univerzális Windows platformra

##<a name="Setup"></a>A telepítő és vonatkozó követelmények

Ez az útmutató feltételezi, hogy egy kódmentes létrehozott egy táblát. Ez az útmutató feltételezi, hogy a táblázat ugyanarra a sémára, mint a tábla rendelkezik-e oktatóanyagok. Ez az útmutató is feltételezi, hogy hozzáadta a Apache Cordova bővítményt a kódot.  Nem tette, ha a beépülő modul Apache Cordova adhat hozzá a projekthez a parancssorban:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

[Az első Apache Cordova-alkalmazás]létrehozásával kapcsolatos további tudnivalókért lásd: a dokumentációt.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Útmutató: felhasználók hitelesítő

Azure alkalmazás szolgáltatás támogatja a hitelesítése és különböző, külső Identitásszolgáltatók használata app felhasználói engedélyezése: Facebook, a Google, a Microsoft-Account és a Twitteren. Engedélyeket állíthat be a meghatározott műveletekhez csak hitelesített felhasználóknak a hozzáférés korlátozására táblázatokban. Hitelesített felhasználói azonosítója segítségével engedélyezési szabályok megvalósítását a kiszolgálóoldali parancsfájlokat. További tudnivalókért lásd: az [első lépések a hitelesítési] oktatóprogram.

Amikor hitelesítési Apache Cordova alkalmazás, a következő Cordova bővítmények kell érhető el:

* [cordova-bővítmény-eszköz]
* [cordova-bővítmény-inappbrowser]

Két hitelesítési flow támogatottak: egy kiszolgáló forgalmat és az ügyfél folyamat.  A kiszolgáló folyamat nyújt a legegyszerűbb hitelesítési változat, a szolgáltató webes hitelesítési felületén támaszkodik. A ügyfél folyamat lehetővé teszi, hogy az eszköz-specifikus funkciók integrálása mélyreható például single-sign-on, akkor szolgáltatófüggő eszköz-specifikus SDK támaszkodik.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Útmutató: az alkalmazás mobilszolgáltatás külső átirányítási URL-címek konfigurálása.

Különböző típusú Apache Cordova alkalmazások egy visszacsatolási képesség segítségével OAuth felhasználói felület flow kezelni.  A localhost OAuth felhasználói felület flow problémákat okozhatnak, mivel a hitelesítési szolgáltatás csak tudja, hogyan kell használni a szolgáltatás alapértelmezés szerint.  Hibás OAuth felhasználói felület flow példája:

- A Továbbgyűrűző irányító.
- Élő ionos az Újratöltés gombra.
- A mobil kódmentes helyben fut
- A mobil kódmentes fut egy másik Azure alkalmazás szolgáltatás, mint a egy nyújtó hitelesítési.

A helyi beállítások hozzáadása a konfiguráción lépései:

1. Jelentkezzen be az [Azure portál]
2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintson a mobil alkalmazás nevére.
3. Kattintson az **Eszközök menü**
4. A OBSERVE menüben kattintson az **erőforrás explorer** parancsra, majd kattintson az **Ugrás**gombra.  Megnyílik egy új ablak vagy fülre.
5. Bontsa ki a **config**, a webhelyen, a bal oldali navigációs **authsettings** csomópontot.
6. Kattintson a **Szerkesztés** gombra
7. Keresse meg a "allowedExternalRedirectUrls" elemet.  NULL értékre vagy értékek tömbjét lehetséges, hogy beállított.  Módosítsa az értéket a következő értéket:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Cserélje ki az URL-címeit, a szolgáltatás URL-címét.  Többek között (az Node.js minta szolgáltatás) "http://localhost:3000" vagy "http://localhost:4400" (a a Továbbgyűrűző szolgáltatás).  Az alábbi URL-azonban olyan példa - a helyzet, beleértve a példákban említett szolgáltatások eltérőek lehetnek.
8. Kattintson a képernyő jobb felső sarkában a **Olvasási/írási** gombra.
9. Kattintson a zöld gomb **HELYEZI el** .

A beállítások mentése ezen a ponton.  Ne zárja be a böngésző ablakában a beállítások befejezéséig mentést.
Az alábbi visszacsatolási URL-is hozzá az alkalmazás szolgáltatás CORS beállításait:

1. Jelentkezzen be az [Azure portál]
2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintson a mobil alkalmazás nevére.
3. A beállítások lap automatikusan megjelenik.  Ha nem, kattintson az **Összes beállításai**parancsra.
4. Kattintson a **CORS** az API menüjében.
5. Az URL-címet, amelyet szeretne, írjon be a megadott, és nyomja le az Enter billentyűt.
6. Adja meg a további URL-címet, szükség szerint.
7. Kattintson a **Mentés** gombra a beállítások mentéséhez.

Körülbelül 10 – 15 másodperc az új beállítások érvénybe léptetéséhez vesz igénybe.

##<a name="register-for-push"></a>Útmutató: Regisztrálás a leküldéses értesítések

Leküldéses értesítések kezelése a [phonegap-bővítmény-leküldéses] telepítése.  A beépülő modul egyszerűen hozzáadhatók használata a `cordova plugin add` parancs a parancssorban, vagy a mely számjegy bővítmény telepítő Visual Studio keresztül.  A következő kódot a Apache Cordova alkalmazásban regisztrálja az eszközt a leküldéses értesítések:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Az értesítés hubok SDK segítségével küldje el a leküldéses értesítéseket a kiszolgálóról.  Soha ne küldjön a leküldéses értesítések közvetlenül az ügyfelek számára. Ha így felhasználható szeretne elindítani egy megtagadását szolgáltatás támadások értesítés hubok vagy a PNS szemben.  A PNS sikerült bA eredményeként ilyen támadásokat a forgalmat.

<!-- URLs. -->
[Azure portál]: https://portal.azure.com
[Azure mobil alkalmazások első lépések]: app-service-mobile-cordova-get-started.md
[Első lépések a hitelesítés]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure mobilalkalmazásai Apache Cordova beépülő modul]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[az első Apache Cordova alkalmazás]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap-bővítmény-leküldéses]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova-bővítmény-eszköz]: https://www.npmjs.com/package/cordova-plugin-device
[cordova-bővítmény-inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
