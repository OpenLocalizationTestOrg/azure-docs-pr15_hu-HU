<properties
    pageTitle="Első lépések Azure Active Directory Cordova |} Microsoft Azure"
    description="Hogyan hozhat létre egy Cordova alkalmazás, amely integrálódik az Azure Active Directory való bejelentkezéshez és Azure Active Directory felhívja API-k használata OAuth védett."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure Active Directory integrálása egy Apache Cordova alkalmazás

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova fejlesztését, HTML5-ös/JavaScript-alkalmazások, amelyek a teljes értékű natív alkalmazásként mobileszközökön futtatását is lehetővé teszi lehetővé teszi.
Azure Active Directory, a vállalati minőségű hitelesítési funkciók is adhat az Cordova alkalmazások. Köszönetnyilvánító Azure Active Directory natív SDK Körbefuttatás iOS, Android, a Windows áruház és a Windows Phone, a beépülő modul Cordova bővítheti az alkalmazás támogatja a bejelentkezési felhasználói Active Directory-fiókokkal, eléréséhez az Office 365-ben és az Azure API és még a hívások átirányítása a saját egyéni webes API védelme.

Ebben az oktatóanyagban fogjuk használni a Apache Cordova bővítményt az Active Directory hitelesítési tár (ADAL) egy egyszerű alkalmazás a következő szolgáltatásokkal javítása:

-   A mindössze néhány kódsorokat az Active Directory-felhasználói hitelesítő, és szerezze be a token az Azure Active Directory Graph API hívásakor.
-   A diagram API lekérdezéséhez könyvtárra, és megjeleníti az eredményt meghívásához használ, hogy a token  
-   A felhasználó a hitelesítési kérések minimalizálása ADAL jogkivonat gyorsítótárát kihasználhatja.

Annak érdekében, hogy ehhez kell:

2. Azure AD-alkalmazás regisztrálása
2. Kód hozzáadása a kérelem tokenek alkalmazás
3. A diagram API lekérdezése a token használandó kód hozzáadása és eredmények megjelenítéséhez.
4. A Cordova telepítési projekt létrehozása a javítani kívánt összes platform és a Cordova ADAL bővítményt, és tesztelje a megoldást emulátor.

## <a name="0--prerequisites"></a>*0. Előfeltételek*

Ebben az oktatóanyagban lehetővé teszi a befejezéséhez szükséges:

- Az Azure Active Directory bérlői alkalmazás fejlesztési jogokkal rendelkező fiók esetében
- A fejlesztői környezet segítségével Apache Cordova van konfigurálva  

Ha mindkét már állított be, kérjük, lépjen közvetlenül lépés: 1.

Ha még nincs telepítve az Azure AD-bérlő, keressen [beszerzéséhez útmutatást itt](active-directory-howto-tenant.md).

Ha még nincs telepítve a számítógépre beállítása Apache Cordova, telepítse az alábbiakat:

- [Mely számjegy](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (NPM csomag manager keresztül egyszerűen telepíthető: `npm install -g cordova`)

Megjegyzendő, hogy azok Ha működne, mind a Mac gépen

Minden cél platform használatának előfeltételei különböző.

- Létre, és futtassa a Windows Táblaszámítógép vagy a telefon app verziója
    - [Visual Studio 2013 a Windows Update 2 vagy újabb verziójában](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express vagy újabb verzió).
- Létre, és futtassa az iOS
    -   Xcode 5.x vagy újabb. Töltse le a http://developer.apple.com/downloads vagy a [Mac App Store áruházban](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios-sim](https://www.npmjs.org/package/ios-sim) – lehetővé teszi az iOS-alkalmazás indítása az iOS simulatort használja a parancssorból be (a terminált keresztül egyszerűen telepíthető: `npm install -g ios-sim`)

- Szerkesztés és az Android-alapú alkalmazás
    - Telepítse [Java fejlesztési Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verziójában. Győződjön meg arról, hogy `JAVA_HOME` (változó környezet) helyesen értéke szerint JDK telepítési helye (például C:\Program Files\Java\jdk1.7.0_75).
    - [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) telepítése, és adja hozzá `<android-sdk-location>\tools` helyét (például C:\tools\Android\android-sdk\tools) a `PATH` környezet változó.
    - Nyissa meg az Android-alapú SDK Manager (például keresztül Terminálszolgáltatások: `android`) és telepítése
    - *Android 5.0.1 (API-21)* platform SDK
    - *Android SDK Build-eszközök* verzió 19.1.0 vagy újabb
    - *Android támogatási tárházba* (Extrák)

  Android sdk bármelyik alapértelmezett irányító példány nem nyújt. Hozzon létre egy újat futtatásával `android avd` terminált, és kiválasztja *... létrehozása* , ha szeretné futtatni irányító Android alkalmazást. Ajánlott *Api szintű* 19 vagy újabb verziójával, lásd: [AVD Manager] (http://developer.android.com/tools/help/avd-manager.html) Android irányító és létrehozási beállítások további információt.


## <a name="1--register-an-application-with-azure-ad"></a>*1. regisztrálása az Azure Active Directory-alkalmazás*

Megjegyzés: Ez __a lépés nem kötelező__. Az oktatóprogram előre kiépített érték, amely lehetővé teszi, hogy a művelet a minta láthat anélkül, hogy minden kiépítési egyéni bérlőhöz módon megadva. Ajánlott azonban elvégzi ezt a lépést és megismerje a folyamat szerint lesz szükség esetén a saját alkalmazások hoz létre.

Azure Active Directory csak ki tokenek ismert alkalmazásokat. Azure AD az alkalmazás használata előtt kell hozzon létre egy bejegyzést, a bérlői webhelyén.  Új alkalmazás rögzítése a bérlői webhelyén

-   Jelentkezzen be az [Azure Kezelőportálja segítségével](https://manage.windowsazure.com)
-   A bal oldali navigációs sávon kattintson **Az Active** Directory
-   Jelölje ki a bérlő, ha ki szeretne regisztrálni az alkalmazást.
-   Kattintson az **alkalmazások** fülre, és kattintson a **Hozzáadás** gombra az alsó fiókban.
-   Az útmutatást követve hozzon létre egy új **Natív ügyfélalkalmazás** (annak ellenére, hogy Cordova alkalmazások HTML-alapú, azt is natív ügyfele alkalmazás létrehozása itt így `Native Client Application` beállítást; ellenkező esetben az alkalmazás nem működik).
    -   Az alkalmazás **neve** leírja a végfelhasználók alkalmazás
    -   Az **Átirányítás URI** az URI tokenek térjen vissza az alkalmazás segítségével. Adja meg `http://MyDirectorySearcherApp`.

Ha befejezte a regisztrációs, AAD rendel az alkalmazás egy egyedi ügyfél-azonosító.  Ezt az értéket a következő szakaszokban lesz szüksége: az újonnan létrehozott alkalmazás **beállítása** lapon találja.

Annak érdekében, hogy futtatni `DirSearchClient Sample`, az újonnan létrehozott alkalmazás engedélyt a lekérdezés az _Azure Active Directory Graph API_:
-   A **beállítás** lapon keresse meg az "Engedélyek való egyéb alkalmazások" szakasz.  A "Azure Active Directory" alkalmazás hozzáadása a **hozzáférést a bejelentkezett felhasználó a címtárban** engedélyt a **Meghatalmazott engedélyeit**.  Ezzel engedélyezi az alkalmazás a Graph API-felhasználóknak lekérdezéséhez.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. a minta alkalmazás tárházba az oktatóprogram szükséges másolása*

A rendszerhéj vagy parancsot írja be a következő parancsot:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. a Cordova-alkalmazás létrehozása*

Több módon Cordova alkalmazások létrehozását. Ebben az oktatóanyagban fogjuk használni a Cordova parancssor (CLI).
A rendszerhéj vagy parancsot írja be a következő parancsot:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

A mappastruktúra és állványon Cordova a projekt, a tartalom a starter projekt másolása a www almappát, amely hoz létre.
Az új DirSearchClient mappa áthelyezése.

    cd .\DirSearchClient

A diagram API meghívása szükséges whitelist bővítmény hozzáadása.

     cordova plugin add cordova-plugin-whitelist

Ezután a platformon támogatja a kívánt összes hozzáadásához. Annak érdekében, hogy van egy minta, szüksége lesz hajtsa végre az alábbi parancsok egyikét. Figyelje meg, hogy nem fogja tudni emulálására iOS Windows vagy a Windows/Windows Phone-t macre.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Végül a ADAL Cordova bővítmény is hozzáadása a projekthez.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. a hitelesíti a felhasználókat, és szerezze be tokenek AAD-kód hozzáadása*

Az alkalmazás, ha az oktatóprogram fejlesztéséhez bA csontos címtár keresés funkció, ahol a végfelhasználó írja be az alias bármelyik felhasználó a címtárban és ábrázolása néhány egyszerű attribútum nyújt.  A starter projekt meghatározása a egyszerű felhasználói felület (a www/index.html) az alkalmazás és a állványon huzalok egyszerű alkalmazás eseményt szervez, cycles, a felhasználói felület kötések, és találatok megjelenítési logika (a www/js/index.js). Által írt beállítást az identitás-feladatok végrehajtásával logika hozzáadása.

Az első lépés kell tennie az alkalmazás és a célként erőforrások azonosítására szolgáló AAD által használt protokoll értékeket a kódban bevezetésére. Ezeket az értékeket a token kérések később összeállításához lesz. Az alábbi kódtöredékének beszúrása a index.js fájl nagyon tetején.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

A `redirectUri` és `clientId` értékeinek meg kell egyezniük AAD alkalmazását ismertető értékeket. Megtalálhatja azokat a beállítás lapon az Azure-portálon az oktatóprogram korábbi részében lépés: 1 ismertetett módon.
Megjegyzés: Ha azt választotta, új alkalmazás nem, hogy a saját bérlőhöz regisztrált, egyszerűen beillesztheti az előre beállított a fenti értékeket ez –, amely lehetővé teszi, hogy a minta haladási látható, hogy meg kell mindig hozzon létre saját bejegyzést a termelési készült alkalmazások.

Következő lépésként el kell a tényleges jogkivonat kérelem kódot hozzáadni. A következő kódtöredékének közötti beszúrása a `search `és `renderdata `definíciók.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Érdemes megvizsgálni a függvény által a két fő részből bontásban.
Ez a példa akkor verziójához készült bármely bérlői, és egy adott megjegyzés kötni nem. Akkor használja az "/ közös" végpontot, amely lehetővé teszi, hogy a felhasználónak meg kell adnia a minden fiók hitelesítési időben, és a kérelem irányítja a bérlő tartozik.
Ez a módszer első része megvizsgálja az ADAL gyorsítótárból annak ellenőrzéséhez, hogy már létezik egy tárolt jogkivonat -, és ha van, akkor használja a bérlők ADAL újra inicializálhatóként érkezett. Az kell kerülni a további útmutatást, mint használata a "/ közös" mindig eredményez, de a felhasználónak meg kell adnia az új fiók kéri.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
A második részben a módszernek hajtja végre a megfelelő tokewn kérelmet.
A `acquireTokenSilentAsync` ADAL módszer kéri az adott erőforrás egy token térni bármely UX megjelenítése nélkül Amely akkor fordulhat elő, ha a gyorsítótár már van egy megfelelő hozzáférési jogkivonat tárolja, vagy ha a frissítés jogkivonat, amely egy új jogkivonat nélkül shwoing bármilyen kérdés kérjen használható.
Amely a kísérlet sikertelen, ha azt esni vissza `acquireTokenAsync` -jól láthatóan kéri, amely a felhasználó hitelesítést végezni.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Most, hogy elkészült a jogkivonat, hogy végül meghívása a Graph API-t, és hajtsa végre a kívánt keresési lekérdezést. A következő kódtöredékének beszúrása jobbra alatt a `authenticate` definition.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
A kiindulási pont fájlokat egy felhasználó alias beírása a szövegdobozban egy barebone UX megadni. Ez a módszer a lekérdezést, kombinálni a hozzáférési jogkivonat, küldje el a diagram és elemezni az eredmények ezt az értéket használja. Már szerepel a kiindulási pont fájl renderData módszer gondoskodik jelenítse meg az eredményeket.

## <a name="4-run"></a>*4. Futtatás*
Az alkalmazás végül készen áll a futásra! Akkor működik, akkor rendkívül egyszerű: Amikor elindítja az alkalmazást, írja be a szövegmezőbe a annak a felhasználónak szeretné kikeresni –, majd kattintson a gombra. Hitelesítés kéri. A sikeres hitelesítési és a sikeres keresés a keresett felhasználó tulajdonságainak jelenik meg. További futtatja a keresést végez a bármilyen kérdés gyorsítótárat a korábban megvásárolt jogkivonat jelenléte több megjelenítése nélkül.
Az alkalmazás futtatásához a konkrét lépéseket platform típusától függően változnak.

####<a name="windows-10"></a>Windows 10 esetén:

   Táblaszámítógép /:`cordova run windows --archs=x64 -- --appx=uap`

   A mobil (PC-re csatlakoztatott Windows10 mobileszköz igényel):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Megjegyzés__: kérheti fejlesztői licenccel bejelentkezés az első futtatása során. További részletekért lásd: a [fejlesztői licenccel](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>A Windows 8.1 vagy Táblaszámítógép:

   `cordova run windows`

   __Megjegyzés__: kérheti fejlesztői licenccel bejelentkezés az első futtatása során. További részletekért lásd: a [fejlesztői licenccel](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1 esetén:

   A csatlakoztatott eszköz futtatása`cordova run windows --device -- --phone`

   Alapértelmezett irányító futtatása`cordova emulate windows -- --phone`

   Használati `cordova run windows --list -- --phone` kattintva megtekintheti az összes elérhető tárolók és `cordova run windows --target=<target_name> -- --phone` az adott eszközön vagy irányító az alkalmazásnak a futtatására (például `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   A csatlakoztatott eszköz futtatása`cordova run android --device`

   Alapértelmezett irányító futtatása`cordova emulate android`

   __Megjegyzés__: Győződjön meg arról, hogy irányító példány *AVD* segítségével, amint azt az van *Előfeltételek* részben létrehozott.

   Használati `cordova run android --list` kattintva megtekintheti az összes elérhető tárolók és `cordova run android --target=<target_name>` az adott eszközön vagy irányító az alkalmazásnak a futtatására (például `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   A csatlakoztatott eszköz futtatása`cordova run ios --device`

   Alapértelmezett irányító futtatása`cordova emulate ios`

   __Megjegyzés__: Győződjön meg arról, hogy `ios-sim` futtatható a irányító csomagot. További részletekért lásd: *előfeltételekről* szakaszban.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Használat `cordova run --help` további információ, és futtassa a beállításokat.

A kész minta (nélkül a konfigurációs értékek) megadva hivatkozás, az [alábbi](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Most már áthelyezheti speciális (ok, és még érdekesebbé) esetek.  Előfordulhat, hogy szeretne kipróbálni:

[Biztonságos Node.js webes API az Azure Active Directory >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
