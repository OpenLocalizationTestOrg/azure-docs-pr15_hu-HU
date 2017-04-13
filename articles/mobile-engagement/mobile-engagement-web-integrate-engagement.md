<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet Web SDK integrációs |} Microsoft Azure"
    description="A legújabb frissítéseket, és az Azure Mobile tetszés szerint elmélyedhet webes SDK eljárások"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="02/29/2016"
    ms.author="piyushjo" />

#<a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Azure Mobile tetszés szerint elmélyedhet integráció a webes alkalmazásokban

> [AZURE.SELECTOR]
- [A Windows univerzális](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-alapú](mobile-engagement-android-integrate-engagement.md)

A jelen cikkben ismertetett eljárások ismertetik a legegyszerűbb módszer az analitikai és Azure Mobile tetszés szerint elmélyedhet a webalkalmazásban ellenőrző függvények aktiválása.

Kövesse a aktiválása a napló jelentések szükséges összes statisztikájának felhasználók, munkamenetek, tevékenységek, lefagy vagy technicals számítja ki. Az alkalmazás-függő statisztikák eseményeket, a hibák és a feladatokat, például aktiválnia kell a jelentések manuálisan az Azure Mobile tetszés szerint elmélyedhet API segítségével. További információért miként [használhatja a speciális Mobile tetszés szerint elmélyedhet webalkalmazás API-címkézés](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>– Bevezetés

[Töltse le a mobil részvételét Azure webes SDK csomagjában talál](http://aka.ms/P7b453).
A mobil tetszés szerint elmélyedhet Web SDK egyetlen JavaScript fájlként van teljesült azure engagement.js, amely rendelkezik a webhelyen vagy a webes alkalmazás minden oldalára.

> [AZURE.IMPORTANT] Futtatja a parancsfájlt, futtatnia kell egy parancsfájlt vagy kód kódtöredékének kézzel írt Mobile tetszés szerint elmélyedhet konfigurálása az alkalmazás.

## <a name="browser-compatibility"></a>Webböngészők kompatibilitása

A mobil tetszés szerint elmélyedhet Web SDK natív JSON kódolás és visszafejtés, tartományok AJAX-kérelmek (a W3C CORS specifikációja használna) mellett használ. Az alábbi böngészők kompatibilis:

* Microsoft Edge 12 +
* Az Internet Explorer 10 vagy újabb
* A Firefox 3.5-ös +
* A Chrome a 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Mobil tetszés szerint elmélyedhet konfigurálása

Írjon egy parancsfájlt, amelyet a globális létrehoz `azureEngagement` JavaScript-objektumra, ahogy a következő példában. A webhely lehet Többszörösök lapok, mert az Ez a példa feltételezi, hogy a parancsprogram minden oldalon található. Ebben a példában a JavaScript-objektum neve `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

A `connectionString` értéke az alkalmazás megjelenik az Azure-portálon.

> [AZURE.NOTE] `appVersionName`és `appVersionCode` nem kötelező. Azt javasoljuk, úgy beállítani, hogy őket, hogy analytics dolgozhat vonatkozó információk.

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Mobil tetszés szerint elmélyedhet parancsfájlok szerepeltetni a lapok
A lapok Mobile tetszés szerint elmélyedhet parancsfájlok hozzáadása a következő módon:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Vagy ilyen:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias

Miután betöltött a Mobile tetszés szerint elmélyedhet Web SDK parancsfájl, **tetszés szerint elmélyedhet** alias eléréséhez a SDK API-k hoz létre. Az alias nem használható a SDK konfiguráció megadása. Az alias Ez a dokumentáció hivatkozásként használják.

Tartsa szem előtt, hogy ha az alapértelmezett alias az weblapon egy másik globális változó ütközik, újra meg lehet adni azt a konfigurációban az alábbi képlettel történik a Mobile tetszés szerint elmélyedhet Web SDK betöltése előtt:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Egyszerű jelentés

Egyszerű jelentés Mobile tetszés szerint elmélyedhet bemutatja a munkamenet szintű statisztikai adatokat, például a felhasználók, a munkamenetek, a tevékenységek és a összeomlik statisztikájának.

### <a name="session-tracking"></a>Munkamenet nyomon követése

A mobil tetszés szerint elmélyedhet munkamenet meg van osztva tevékenységek egy sorozatát, minden egyes jelölt nevét.

Klasszikus webhelyet azt javasoljuk, hogy a webhely minden oldalára történő deklarálását egy másik tevékenységeket. A webhelyén vagy a webes alkalmazáshoz, amelyben az aktuális lap soha nem változik érdemes lehet nyomon követni a tevékenységeket a kisebb méret, például a lapon belül.

Mindkét esetben szeretne beírni, vagy módosítsa az aktuális felhasználói tevékenység, hívja fel a `engagement.agent.startActivity` függvény. Példa:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

A mobil tetszés szerint elmélyedhet kiszolgáló automatikusan lezárja a munkamenet megnyitása az alkalmazás lap bezárása után három percen belül.

Azt is megteheti, hogy is befejezéséhez manuálisan hívja fel `engagement.agent.endActivity`. Ez akkor állítja be az aktuális felhasználói tevékenység a "Üresjárati."  A munkamenet érjen véget 10 másodperc múlva, kivéve, ha új hívást kezdeményez `engagement.agent.startActivity` folytatja a munkamenetet.

Adhatja meg a 10 másodperccel a globális tetszés szerint elmélyedhet objektumban az alábbi képlettel történik:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] Nem használhatja `engagement.agent.endActivity` a a `onunload` visszahívási mivel ebben a szakaszban nem lehet AJAX-hívások.

## <a name="advanced-reporting"></a>Speciális jelentése

Tetszés szerint szeretné az alkalmazás-specifikus eseményeket, a hibák és a feladatok, ha szeretne a Mobile tetszés szerint elmélyedhet API-t használja. A mobil tetszés szerint elmélyedhet API keresztül érheti el a `engagement.agent` objektumot.

Hozzáférhet a Mobile tetszés szerint elmélyedhet a Mobile tetszés szerint elmélyedhet API speciális lehetőségeit. [A speciális Mobile tetszés szerint elmélyedhet webalkalmazás API-címkézés használata](mobile-engagement-web-use-engagement-api.md)című témakörben részletes az API-t.

## <a name="customize-the-urls-used-for-ajax-calls"></a>Az URL-címeit, AJAX-hívásokhoz használt testreszabása

Testre szabhatja az URL-címeit, amely a Mobile tetszés szerint elmélyedhet Web SDK használja. Ha például újra a napló URL-címe (a naplózás SDK végpont), felülbírálhatja a konfiguráció jelennek meg:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Ha az URL-cím függvények ad vissza, amely a kezdődik szöveget `/`, `//`, `http://`, vagy `https://`, az alapértelmezett séma nem használatos. Alapértelmezés szerint a `https://` séma szolgál az URL-címeket. Ha azt szeretné, ha testre szeretné szabni az alapértelmezett séma, felülbírálása a konfigurációs jelennek meg:

    window.azureEngagement = {
      ...
      urls: {
        ...      
        scheme: '//'
      }
    };
