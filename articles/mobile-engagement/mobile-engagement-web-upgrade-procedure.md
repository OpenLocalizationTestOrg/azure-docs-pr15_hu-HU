<properties
    pageTitle="Azure Mobile tetszés szerint elmélyedhet Web SDK frissítési eljárások |} Microsoft Azure"
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
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure Mobile tetszés szerint elmélyedhet Web SDK frissítési eljárások

Ha van már integrált a webes alkalmazásba az Azure Mobile tetszés szerint elmélyedhet Web SDK korábbi verziójában, vegye figyelembe az alábbiakat, ha frissít a SDK szeretne.

Ha a Mobile tetszés szerint elmélyedhet Web SDK több verziójának kihagyott, szükség lehet a frissítési folyamat során több eljárás befejezéséhez. Ha például 1.4.0 1.6.0 áttelepítése akkor, ha először hajtsa végre 1.5.0 1.4.0 frissíthet. Ezután kövesse az eljárások 1.6.0 1.5.0 frissíthet.

Bármelyik verziót frissít, a fájl azure engagement.js minden korábbi verziójának cserélje ki a fájlt a legújabb verzióját.

## <a name="upgrade-from-121-to-200"></a>2.0.0 1.2.1 frissítése

Ez a szakasz ismerteti, hogy miként telepítheti át a Mobile tetszés szerint elmélyedhet Web SDK integráció az Azure Mobile tetszés szerint elmélyedhet alkalmazás Capptain Társítások, által kínált, Capptain szolgáltatás. Ha egy korábbi verziójáról, olvassa el az első áttelepítése 1.2.1 Capptain webhely, és alkalmazza az alábbi eljárások valamelyikét.

A mobil tetszés szerint elmélyedhet Web SDK verziójában Samsung intelligens TV, Opera TV, webOS vagy a elérés funkció nem támogatja.

>[AZURE.IMPORTANT] Capptain és Azure Mobile tetszés szerint elmélyedhet sem ugyanazt a szolgáltatást. Az alábbi eljárást csak hogy miként telepítheti át az ügyfél alkalmazás kijelöli. A mobil tetszés szerint elmélyedhet Web SDK a alkalmazásban áttelepítése nem áttelepíti az adatok Capptain kiszolgálóról Mobile tetszés szerint elmélyedhet-kiszolgálón.

### <a name="javascript-files"></a>JavaScript-fájlok

A fájl capptain sdk.js cserélje ki a fájl azure engagement.js, és a parancsprogram-import árnyalatait.

### <a name="remove-capptain-reach"></a>Capptain vannak eltávolítása

Ez a Mobile tetszés szerint elmélyedhet Web SDK verziója nem támogatja az elérés funkció. Ha Capptain vannak beépül az alkalmazást, akkor kell távolítsa el azt.

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

### <a name="remove-deprecated-apis"></a>Elavult API-khoz eltávolítása

A mobil tetszés szerint elmélyedhet Web SDK már nem támogatja a Capptain egyes API-k.

Távolítsa el a következő API-k bármely hívásainak: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.

A következő visszahívást példányait eltávolítása a Capptain konfigurációs: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguráció

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

### <a name="javascript-apis"></a>A JavaScript API-hoz

A globális JavaScript-objektum `window.capptain` átnevezése `window.azureEngagement` is használhatja, de a `window.engagement` alias API-hívások. Az alias nem használható a SDK konfiguráció megadása.

Ha például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`és így tovább.
