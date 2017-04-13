<properties
    pageTitle="Azure mobil tetszés szerint elmélyedhet webes SDK áttekintése |} Microsoft Azure"
    description="A legújabb frissítések és Azure Mobile tetszés szerint elmélyedhet a webes SDK eljárások"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure mobil tetszés szerint elmélyedhet webes SDK

Kezdje itt a integrálása az Azure Mobile tetszés szerint elmélyedhet a webalkalmazást hogyan olvashat. Ha azt szeretné, hogy tegyen egy próbát, mielőtt a saját webalkalmazás – első lépések, olvassa el a a [15 perces oktatóprogram](mobile-engagement-web-app-get-started.md)című témakört.

## <a name="integration-procedures"></a>Integráció eljárások
1. Tudnivalók [a Mobile tetszés szerint elmélyedhet a webalkalmazásban](mobile-engagement-web-integrate-engagement.md).

2. Címke terv végrehajtásához miként [használhatja a speciális Mobile tetszés szerint elmélyedhet a webalkalmazásban API címkézés](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="202-10182016"></a>2.0.2 (10/18 vagy 2016)

-   Rögzített összeomlik a privát böngészési (Safari).
-   Rögzített összeomlik a cookie-k letiltása böngészőkben.

Az összes verzió olvassa el a [teljes kibocsátási megjegyzések](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Frissítési eljárások

### <a name="upgrade-from-121-to-200"></a>2.0.0 1.2.1 frissítése

Az alábbi szakaszok ismertetik, hogyan telepítheti át egy mobil tetszés szerint elmélyedhet Web SDK integrálása az Azure Mobile tetszés szerint elmélyedhet alkalmazás Capptain Társítások, által kínált, Capptain szolgáltatás. Ha verziójú verziónál 1.2.1, olvassa el az első 1.2.1 áttelepítése a Capptain webhely, és majd alkalmazza az alábbi eljárások valamelyikét.

A mobil tetszés szerint elmélyedhet Web SDK verziójában Samsung intelligens TV, Opera TV, webOS vagy a elérés funkció nem támogatja.

>[AZURE.IMPORTANT] Capptain és Azure Mobile tetszés szerint elmélyedhet nem ugyanazt a szolgáltatást, és az alábbi eljárások kiemelése csak hogy miként telepítheti át az ügyfél alkalmazást. A mobil tetszés szerint elmélyedhet Web SDK a alkalmazásban áttelepítése nem áttelepíti az adatok Capptain kiszolgálóról Mobile tetszés szerint elmélyedhet-kiszolgálón.

#### <a name="javascript-files"></a>JavaScript-fájlok

A fájl capptain sdk.js cserélje ki a fájl azure engagement.js, és a parancsprogram-import árnyalatait.

#### <a name="remove-capptain-reach"></a>Capptain vannak eltávolítása

Ez a Mobile tetszés szerint elmélyedhet Web SDK verziója nem támogatja az elérés funkció. Ha van Capptain vannak beépül az alkalmazást, meg kell távolítsa el azt.

A elérje a CSS-fájlok importálása eltávolítása a lapon, és törölje a kapcsolódó CSS-fájlt (capptain-reach.css, alapértelmezés szerint).

A következő vannak erőforrások törlése: a közeli képet (capptain-close.png, alapértelmezés szerint) és a arculatának ikonra (capptain-értesítés-ikon, alapértelmezés szerint).

Távolítsa el az alkalmazást az értesítésekhez elérje a felhasználói felület. Az alapértelmezett elrendezés néz ki:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Távolítsa el a szöveg és a webhely hirdetmények és szavazásokra elérje a felhasználói felület. Az alapértelmezett elrendezés néz ki:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Távolítsa el a `reach` objektum az Ön konfigurációjának, ha van ilyen. Hogy néz ki:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Távolítsa el a bármely más vannak testreszabás, például a kategóriákat.

#### <a name="remove-deprecated-apis"></a>Elavult API-khoz eltávolítása

A mobil tetszés szerint elmélyedhet Web SDK már nem támogatja a Capptain egyes API-k.

Távolítsa el a következő API-k bármely hívásainak: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.

A következő visszahívást bármelyikét eltávolítása a Capptain konfigurációs: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguráció

Mobil tetszés szerint elmélyedhet a kapcsolati karakterlánc SDK azonosítók, például az alkalmazás azonosítója konfigurálása használja.

A kapcsolati karakterláncot cserélje le az alkalmazás azonosítója. Megjegyzendő, hogy módosítja a globális objektum SDK konfiguráció `capptain` való `azureEngagement`.

Áttelepítés: előtt

    window.capptain = {
      appId: ...,
      [...]
    };

Miután az áttelepítés:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

A kapcsolati karakterláncot, az alkalmazás megjelenik az Azure-portálon.

#### <a name="javascript-apis"></a>A JavaScript API-hoz

A globális JavaScript-objektum `window.capptain` átnevezése `window.azureEngagement`, de a a `window.engagement` alias API-hívások. Az alias nem használható a SDK konfiguráció megadása.

Ha például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`és így tovább.

Ha van már integrált az alkalmazásba az Azure Mobile tetszés szerint elmélyedhet Web SDK korábbi verziójában, kérjük, további információ: [eljárások frissítése](mobile-engagement-web-upgrade-procedure.md).
