<properties
    pageTitle="Azure App szolgáltatásban Socket.IO Node.js Csevegés-alkalmazás létrehozása"
    description="Oktatóanyagot, amely bemutatja, hogyan socket.io használata Azure is node.js webalkalmazást."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Azure App szolgáltatásban Socket.IO Node.js Csevegés-alkalmazás létrehozása

Socket.IO biztosít a node.js kiszolgálót és a WebSockets használó ügyfelek közötti valós idejű kommunikációt. Egyéb régebbi böngészők átvitelek (például hosszú lekérdezési) tartalék is támogat. Ebben az oktatóanyagban ismerteti, hogy egy alapú Socket.IO Csevegés alkalmazás meg az Azure web app-szolgáltatója, és bemutatja, hogyan tudja, hogy az alkalmazás használatával [Azure vgx.dll gyorsítótár]méretezni. Socket.IO kapcsolatos további tudnivalókért olvassa el a <http://socket.io/>című témakört.

> [AZURE.NOTE] Ebben a feladatban eljárásokat alkalmazni az [Alkalmazás szolgáltatás Web Apps alkalmazások]; Felhőszolgáltatások [egy egy Azure Felhőbeli szolgáltatásokhoz Socket.IO Node.js Csevegés alkalmazás összeállítása]című témakör tartalmaz.

## <a name="download-the-chat-example"></a>A Csevegés példa letöltése

Ehhez a projekthez fogjuk használni a csevegést példa a [Socket.IO GitHub tárházba]. Hajtsa végre az alábbi lépéseket a példa letöltése, és vegye fel a korábban létrehozott projektet.

1.  Töltse le a egy [ZIP- vagy GZ archivált megjelenési] a Socket.IO projekt (a dokumentum verzió 1.3.5 használt)

1.  Bontsa ki az archiválási és másolása a **Példák\\Csevegés** könyvtár új helyre. Ha például ** \\csomópont\\Csevegés**.

## <a name="modify-appjs-and-install-modules"></a>App.js módosítása és a modul telepítése

1.  Nevezze át a **index.js** fájl **app.js**. Azure észleli, hogy ez legyen-e egy Node.js alkalmazás lehetővé teszi.

1.  Nyissa meg a **app.js** fájlt egy szövegszerkesztőben. Módosítsa a sort tartalmazó `var io = require('../..')(server);` alább látható módon:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Nyissa meg a **package.json** fájlt, és a socket.io mutató hivatkozás hozzáadása `dependencies`, az alább látható módon:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Módosítsa a parancssorból a ** \\csomópont\\Csevegés** könyvtárat és használatának npm a modulokat követel meg az alkalmazás telepítése:

        npm install

    Ez a modulokat telepíti be egy **node_modules**nevű almappában.

## <a name="create-an-azure-web-app"></a>Hozzon létre egy Azure Web App alkalmazásban

Kövesse ezeket a lépéseket követve létrehozása az Azure web app, mely számjegy közzétételi engedélyezése és a web app WebSocket támogatásának engedélyezése a majd.

> [AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért lásd: <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure ingyenes próbaverziót</a>.

1. Telepítse az Azure parancssori kezelőfelületről Azure, és csatlakozzon az Azure előfizetését. [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md)látható.

1. Ha egy tárban tárolnak Azure-ban a első alkalommal beállítandó, a bejelentkezési adatok létrehozásához szükséges. Az Azure CLI az adja meg a következő parancsot:

        azure site deployment user set [username] [password]

1. Módosítsa a ** \\node\chat** könyvtárat és a következő parancsot a hozzon létre egy új Azure használata a webes alkalmazás és a helyi mely számjegy tárházba. Ez a parancs létrehoz egy mely számjegy távoli named "azure" is.

        azure site create mysitename --git

    Egy egyedi nevet a web App alkalmazásban ezt kell lecserélnie "mysitename".

1. A meglévő fájlokat véglegesítse a helyi tárházba használatával az alábbi parancsokat:

        git add .
        git commit -m "Initial commit"

1. A fájlok leküldéses az alábbi paranccsal Azure Web Apps alkalmazások tárházba:

        git push azure master

    Amikor a rendszer kéri, írja be hitelesítő adatait a lépés: 2. Állapotüzenetek kap, modulok importálása a kiszolgálón. Miután befejezte a ezt a folyamatot, az alkalmazás az Azure-webalkalmazást a tárolni.

    > [AZURE.NOTE] Modul telepítése során hiba Észreveheti, hogy "... az importált projekt nem található". Ezek nyugodtan figyelmen kívül hagyható.

1. Socket.IO WebSockets, amely a Azure alapértelmezés szerint nincs engedélyezve vannak használja. Ahhoz, hogy a webes sockets, használja az alábbi parancsot:

        azure site set -w

    Ha a rendszer kéri, írja be a webes alkalmazás nevére.

    >[AZURE.NOTE]
    >A "webhely azure set -w" parancs fog csak olyan verziójú 0.7.4 munkahelyi vagy újabb Azure parancssor. Az [Azure-portálon](https://portal.azure.com)WebSocket támogatás is engedélyezheti.
    >
    >Az Azure-portálon WebSockets engedélyezéséhez kattintson a Web Apps alkalmazások lap a web app, kattintson a **beállítások minden** > **alkalmazás beállításait**. A **Webes Sockets**kattintson **a**. Ezután kattintson a **Mentés**gombra.

1. Azure a web app megtekinteni, a következő paranccsal elindíthatja a webböngészőt, és nyissa meg azt a szolgáltatott web app:

        azure site browse

Az alkalmazás letöltése Azure fut, és Socket.IO használatával különböző ügyfelek közötti Csevegés üzeneteket továbbíthat.

## <a name="scale-out"></a>Méretezése

Socket.IO alkalmazások terjesztheti az üzenetek és események több szolgáltatásalkalmazás-példányok között egy __kártya__ használatával is átméretezi. Több adaptereken áll rendelkezésre, amíg a [socket.io vgx.dll] kártya egyszerűen kínál az Azure vgx.dll gyorsítótár-szolgáltatás.

> [AZURE.NOTE] Egy további követelményt méretezés Socket.IO megoldást ki-e támogatás a Beragadó folyamatokhoz. Öntapadós munkamenetek alapértelmezés szerint engedélyezve vannak a Azure Web Apps alkalmazások Azure kérése útválasztás keresztül. További tudnivalókért olvassa el a [Példány affinitása Azure webhelyek]című témakört.

### <a name="create-a-redis-cache"></a>Hozzon létre egy vgx.dll gyorsítótár

Hozzon létre egy új gyorsítótár [létrehozása az Azure vgx.dll gyorsítótárban gyorsítótárat] a lépések végrehajtása

> [AZURE.NOTE] A Mentés a __Host name__ és az __elsődleges kulcs__ a gyorsítótár, ezek lesz szükség a következő lépésekkel.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Adja hozzá a vgx.dll és socket.io vgx.dll modulok

1. Módosítsa a parancssorból a __ \\csomópont\\Csevegés__ könyvtárat és a következő parancsot használja.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] A megadott Ez a parancs csak ez a cikk tesztelésekor használt verziókról.

1. Módosítsa az __app.js__ fájlt adja hozzá a következő közvetlenül azután, hogy vonalak`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Az állomásnév és a vgx.dll gyorsítótár kulcsa __redishostname__ és __rediskey__ cserélje ki.

    Ez hozzon létre egy közzétételt, és a korábban létrehozott vgx.dll gyorsítótárba ügyfél előfizetés. Az ügyfelek majd használják a kártya Socket.IO használni a vgx.dll gyorsítótár üzenetek és események áthaladó közötti üzenetváltások az alkalmazás beállítása

    > [AZURE.NOTE] A __socket.io vgx.dll__ kártya közvetlenül az vgx.dll tudnak kommunikálni, amíg az aktuális verziója nem támogatja a hitelesítés szükséges Azure vgx.dll gyorsítótár. Ezért a kezdeti kapcsolat jön létre a __Vgx.dll__ modulról, majd az ügyfelet a __socket.io vgx.dll__ kártya átadott.
    >
    > Azure vgx.dll gyorsítótár támogatja a biztonságos kapcsolat port 6380 használatával, miközben az ebben a példában használt modulok nem támogatják a biztonságos kapcsolat 7/14/2014-es kezdve. A fenti kódot használ, az alapértelmezett, nem biztonságos portja 6379.

1. A módosított __app.js__ mentése

### <a name="commit-changes-and-redeploy"></a>A módosítások véglegesítése és telepítsen újra

A parancssorból a a __ \\csomópont\\Csevegés__ címtár, használja az alábbi parancsokat a módosítások véglegesítése és telepítsen újra az alkalmazást.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Után a változtatások van már tolni a kiszolgálóra, a következő parancs használatával a webhely méretezheti át több példányon.

    azure site scale instances --instances #

Ha __#__ létrehozása példányok száma.

A web app csatlakozhat több böngészők vagy számítógépek ellenőrzéséhez üzenetek megfelelően küldött összes ügyfeleknek.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="connection-limits"></a>Kapcsolat korlátai

Azure Web Apps alkalmazások több termékváltozatban, amelyek meghatározzák, a webhely számára elérhető erőforrások érhető el. Ide tartoznak a megengedett WebSocket kapcsolatok száma. További tudnivalókért lásd: a [Web Apps alkalmazások árak lapot].

### <a name="messages-arent-being-sent-using-websockets"></a>Üzenetek nem küldött WebSockets használatával

Ha az ügyfél böngészők megtartása nézi hosszú WebSockets használata helyett végző mintavételi, lehet, hogy a következők valamelyike miatt.

* **Próbálkozzon az átvitel csak WebSockets korlátozása**

    Ahhoz, hogy az üzenetek átviteli fogja használni a WebSockets Socket.IO a kiszolgáló és az ügyfél támogatnia WebSockets. Ha az egyiket nem, Socket.IO egyezteti egy másik átviteli, például hosszú lekérdezési. Alapértelmezett Socket.IO által használt átvitelek van ` websocket, htmlfile, xhr-polling, jsonp-polling`. Kényszerítheti csak a következő kód hozzáadása a **app.js** fájl után a következő műveletet tartalmazó sor használandó WebSockets `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Látható, hogy a régebbi WebSockets nem támogató böngészőkben nem tud kapcsolódni a webhelyhez, amíg a fenti kóddal működik, kommunikáció WebSockets csak korlátozza.

* **Az SSL használata**

    WebSockets egyes kisebb használt HTTP fejlécek, például a **frissítés** fejléc támaszkodik. Néhány köztes hálózati eszközök, például a webes proxyk, előfordulhat, hogy távolítsa el ezeket a fejléceket. Ez a probléma elkerülése érdekében a WebSocket kapcsolatot hozhat létre SSL.

    Ehhez legegyszerűbben a Socket.IO konfigurálása, `match origin protocol`. Utasítja, hogy a biztonságos WebSockets kommunikációs ugyanaz, mint az weblapon származó HTTP-/ HTTPS-vonatkozó Socket.IO. Ha a böngészőben keresse fel a webhely URL-címének HTTPS használ, további WebSocket kommunikáció Socket.IO keresztül SSL fog vonatkozni.

    Ez a példa ahhoz, hogy ez a beállítás módosításához hozzá a következő kódot a **app.js** fájlhoz után a következő műveletet tartalmazó sor `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Web.config beállításainak ellenőrzése**

    Azure web Apps alkalmazások Node.js alkalmazásokat **fájl** használatával irányítja Node.js alkalmazása a bejövő felkérést. WebSockets függvénynek megfelelően Node.js alkalmazásokkal a **web.config** tartalmaznia kell a következő bejegyzést.

        <webSocket enabled="false"/>

    Ez a művelet a IIS WebSockets modul, amely magában foglalja a saját végrehajtása WebSockets és Node.js adott WebSocket modulok például Socket.IO ütközik letiltja. Ha nem szerepel ebben a sorban, vagy értéke `true`, ez lehet az oka, hogy az alkalmazás nem működik a WebSocket átviteli.

    Általában Node.js alkalmazások ne kerüljön bele **customErrors,** így az Azure webhelyek automatikusan létrehozza egyik Node.js alkalmazások, amikor a központi telepítés. Mivel ez a fájl automatikusan létrejön a kiszolgálón, vagy kell használnia az FTP FTPS URL-CÍMÉT a webhely megtekintése a fájlt. Talál az FTP és FTPS URL-ek a webhelyen a klasszikus portálon a web app, majd az **Irányítópult** hivatkozásra kattintva. Az URL-címek a **fontos** szakasz jelenik meg.

    > [AZURE.NOTE] **A fájlt** Azure webhelyek csak jön létre, ha az alkalmazás nyújt tájékoztatást. Ha az alkalmazás projekt legfelső szintű **web.config** fájl ad meg, akkor Azure Webappok által lesz.

    Ha nem szerepel a bejegyzést, vagy a értékre van állítva `true`, akkor érdemes hozzon létre egy **web.config** Node.js alkalmazás legfelső szintű, és adja meg az értéket `false`.  A hivatkozás az alábbi van az alkalmazás, a bejegyzés helyként **app.js** használó alapértelmezett **web.config** .

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Ha az alkalmazás nem **app.js**belépési pontjához, ezt kell lecserélnie a megfelelő belépési pontjához **app.js** minden előfordulását. Ha például lecserélve **app.js** **server.js**.

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás], ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan hozhat létre egy csevegés alkalmazást az Azure webalkalmazásban is. Ez az alkalmazás-Azure felhőalapú szolgáltatást üzemeltetheti is. Hogyan ehhez, című [egy Socket.IO egy Azure Felhőbeli szolgáltatásokhoz Node.js Csevegés alkalmazás összeállítása].

További tudnivalókért lásd még: a [Node.js Developer Center].

## <a name="whats-changed"></a>Mi változott

* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a meglévő Azure-szolgáltatások gyakorolt hatását].

<!-- URL List -->

[Azure vgx.dll gyorsítótár]: /documentation/services/redis-cache/
[Alkalmazás szolgáltatás Web Apps alkalmazások]: http://go.microsoft.com/fwlink/?LinkId=529714
[Alkalmazások árak weblap]: http://go.microsoft.com/fwlink/?LinkId=511643
[Egy Node.js Csevegés alkalmazást Socket.IO épülnek az Azure felhőalapú szolgáltatás]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure alkalmazás szolgáltatás és hatása a meglévő Azure-szolgáltatások]: http://go.microsoft.com/fwlink/?LinkId=529714
[NODE.js Developer Center]: /develop/nodejs/
[Próbálja meg az App szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=523751
[Azure webhelyek példány rokonsága]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[A gyorsítótár létrehozása az Azure vgx.dll gyorsítótárban]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO vgx.dll]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub tárházba]: https://github.com/socketio/socket.io
[ZIP- vagy GZ archivált megjelenés]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
