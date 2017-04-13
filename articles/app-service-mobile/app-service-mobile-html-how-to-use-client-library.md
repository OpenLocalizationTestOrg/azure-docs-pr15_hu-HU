<properties
    pageTitle="A JavaScript SDK használatáról az Azure mobilalkalmazások"
    description="V Azure Mobile-alkalmazások használatát mikéntjéről"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>A JavaScript-ügyfél-tár használata Azure mobilalkalmazásai

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad a legújabb [JavaScript SDK Azure Mobile-alkalmazások]használatával esetei végezheti el. Ha még kezdő az Azure Mobile-alkalmazások, először töltse ki [Azure Mobile alkalmazások Quick Start] hozzon létre egy kódmentes, és hozzon létre egy táblázatot. Ebből az útmutatóból azt kiemelése a mobil kódmentes használja a HTML és JavaScript-webalkalmazásokban.

## <a name="supported-platforms"></a>Támogatott platformok

Azt korlátozza a fő böngészők és az utolsó aktuális verzióira böngészőtámogatás: a Google Chrome, a Microsoft Edge, a Microsoft Internet Explorer és a Mozilla Firefox.  A függvény minden viszonylag modern böngészőben SDK megakadályozási.

A csomag egyetemes JavaScript modul, mint van meghatározva, így globals, AMD, támogat, és formázza a CommonJS.

##<a name="Setup"></a>A telepítő és vonatkozó követelmények

Ez az útmutató feltételezi, hogy egy kódmentes létrehozott egy táblát. Ez az útmutató feltételezi, hogy a táblázat ugyanarra a sémára, mint a tábla rendelkezik-e oktatóanyagok.

Az Azure Mobile alkalmazások JavaScript SDK telepítése keresztül elvégezhető a `npm` parancsot:

```
npm install azure-mobile-apps-client --save
```

Miután telepítette, a tár található `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  A fájl másolása a webes területre.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

A tár egy ES2015 modul belül például Browserify és Webpack, valamint AMD tárról CommonJS környezetben is használható.  Példa:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Útmutató: felhasználók hitelesítő

Azure alkalmazás szolgáltatás támogatja a hitelesítése és különböző, külső Identitásszolgáltatók használata app felhasználói engedélyezése: Facebook, a Google, a Microsoft-Account és a Twitteren. Engedélyeket állíthat be a meghatározott műveletekhez csak hitelesített felhasználóknak a hozzáférés korlátozására táblázatokban. Hitelesített felhasználói azonosítója segítségével engedélyezési szabályok megvalósítását a kiszolgálóoldali parancsfájlokat. További tudnivalókért lásd: az [első lépések a hitelesítési] oktatóprogram.

Két hitelesítési flow támogatottak: egy kiszolgáló forgalmat és az ügyfél folyamat.  A kiszolgáló folyamat nyújt a legegyszerűbb hitelesítési változat, a szolgáltató webes hitelesítési felületén támaszkodik. Az ügyfél áramlás lehetővé teszi, hogy eszköz-specifikus funkciók mélyebb integrációja például single-sign-on, akkor szolgáltatófüggő SDK támaszkodik.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Útmutató: az alkalmazás mobilszolgáltatás külső átirányítási URL-címek konfigurálása.

JavaScript-alkalmazások különböző típusú egy visszacsatolási képesség segítségével OAuth felhasználói felület flow kezelni.  Ezek a funkciók a következők:

* A szolgáltatás helyben fut
* Élő Újratöltés használata ionos keretében
* Alkalmazás szolgáltatás átirányítása hitelesítéshez. 

Helyben fut problémákat okozhatnak, mert az alapértelmezés szerint a mobilalkalmazás kódmentes való hozzáférés engedélyezése csak konfigurált alkalmazás szolgáltatás hitelesítési. Hitelesítés engedélyezése a server helyileg futása közben alkalmazás szolgáltatás beállításainak módosításához kövesse az alábbi lépéseket:

1. Jelentkezzen be az [Azure portál]
2. Nyissa meg a mobilalkalmazás kódmentes.
3. A **FEJLESZTŐESZKÖZÖK** menüben válassza **az erőforrás explorer** .
4. Kattintson az **Ugrás** nyissa meg az erőforrás explorer, a mobilalkalmazás kódmentes az új lap vagy ablakában.
5. Bontsa ki a **config** > az alkalmazás**authsettings** csomópontot.
6. Kattintson az Erőforrás szerkesztésének engedélyezése a **Szerkesztés** gombra.
7. Keresse meg a **allowedExternalRedirectUrls** elemet, amely a null kell lennie. Adja meg az URL-címek tömb:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Az URL-címeit, a tömb cserélje ki a szolgáltatás, amely ebben a példában az URL-címek `http://localhost:3000` a helyi Node.js minta szolgáltatás. Akkor is használhatja `http://localhost:4400` a Továbbgyűrűző szolgáltatás vagy valamilyen más URL-cím, az alkalmazás beállításaitól függően.

8. A lap tetején kattintson **Olvasási/írási**, majd **HELYEZZE** a módosítások mentéséhez.

Meg kell az azonos visszacsatolási URL-címek hozzáadása a CORS whitelist beállításokat:

1. Lépjen vissza az [Azure-portálon].
2. Nyissa meg a mobilalkalmazás kódmentes.
3. Kattintson a **CORS** **API** menü.
4. Egyes URL-CÍMÉT az **Engedélyezett forrásokból** üres szövegmezőben adja meg.  Egy új szövegdobozba jön létre.
5. Kattintson a **MENTÉS** gombra.
    
Miután frissíti a kódmentes, fogja tudni használni az alkalmazást az új visszacsatolási URL-címeit.

<!-- URLs. -->
[Azure mobil alkalmazások első lépések]: app-service-mobile-cordova-get-started.md
[Első lépések a hitelesítés]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portál]: https://portal.azure.com/
[A JavaScript SDK Azure mobilalkalmazásai]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

