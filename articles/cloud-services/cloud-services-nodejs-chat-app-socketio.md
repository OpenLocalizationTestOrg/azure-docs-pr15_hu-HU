<properties 
    pageTitle="NODE.js alkalmazást Socket.io |} Microsoft Azure" 
    description="Megtudhatja, hogy miként socket.io használata egy Azure is node.js alkalmazásra." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Egy Node.js Csevegés alkalmazást Socket.IO épülnek az Azure felhőalapú szolgáltatás

Socket.IO biztosít a valós idejű között a node.js kiszolgáló és az ügyfelek közötti kommunikációt. Ebben az oktatóanyagban végigvezeti szoftvercsatornát szolgáltatója. IO Azure Csevegés alkalmazás alapján. Socket.IO kapcsolatos további tudnivalókért olvassa el a <http://socket.io/>című témakört.

Képernyőkép a kész alkalmazást nem éri el:

![A szolgáltatás Azure is megjelenítése böngészőablakban][completed-app]  

## <a name="prerequisites"></a>Előfeltételek

Győződjön meg arról, hogy az alábbi termékek és -verziók telepítve sikeres befejezéséhez az ebben a cikkben a példában:

* [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) telepítése
* [Node.js](https://nodejs.org/download/) telepítése
* [Python 2.7.10 verzió](https://www.python.org/) telepítése

## <a name="create-a-cloud-service-project"></a>Felhőalapú szolgáltatás projekt létrehozása

Az alábbi lépéseket a felhőalapú szolgáltatás project ugyanis az Socket.IO alkalmazás létrehozása

1. A **Start menüből** vagy **Kezdőképernyőjén**keressen rá **A Windows PowerShell**. Végül kattintson a jobb gombbal a **Windows PowerShell** , és válassza a **Futtatás rendszergazdaként**.

    ![Azure PowerShell ikon][powershell-menu]

2. Hozzon létre egy úgynevezett könyvtárat **c:\\csomópont**. 
 
        PS C:\> md node

3. Könyvtárak módosítása a **c:\\csomópont** címtár
 
        PS C:\> cd node

4. Írja be az alábbi parancsok hozhat létre egy új megoldás **chatapp** nevű és **WorkerRole1**nevű dolgozó szerepkörbe:

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    A következő válasz jelennek meg:

    ![A kimenet az új azureservice és azurenodeworkerrolecmdlets hozzáadása](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>A Csevegés példa letöltése

Ehhez a projekthez fogjuk használni a csevegést példa a [Socket.IO GitHub tárházba]. Hajtsa végre az alábbi lépéseket a példa letöltése, és vegye fel a korábban létrehozott projektet.

1.  Hozzon létre egy helyi példányt a tárházba **Adatfeliratsor** gomb segítségével. A **ZIP-** gombra kattintva töltse le a projekt is használhatja.

    ![A ZIP letöltési ikon ki van emelve a https://github.com/LearnBoost/socket.io/tree/master/examples/chat, megtekintése böngészőablakban][chat-example-view]

3.  Nyissa meg a helyi tárházba könyvtárat felépítésének, mindaddig, amíg odaér ehhez a **Példák\\Csevegés** könyvtárat. A könyvtár tartalmát másolhatja a **C:\\csomópont\\chatapp\\WorkerRole1** korábban létrehozott címtár.

    ![Explorer, a példák tartalmának megjelenítése\\az archiválás kiolvasott Csevegés címtár][chat-contents]

    A fenti képernyőképet kiemelt cikkeket a fájlokat, másolja át a **Példák\\Csevegés** címtár

4.  Az a **C:\\csomópont\\chatapp\\WorkerRole1** címtár, törölje a **server.js** fájlt, és majd nevezze át a **app.js** fájlt **server.js**. Ez az alapértelmezett **server.js** fájl **Hozzáadása-AzureNodeWorkerRole** parancsmag által korábban létrehozott eltávolítja, és lecseréli az alkalmazás fájl a Csevegés példából.

### <a name="modify-serverjs-and-install-modules"></a>Módosítsa a Server.js és modul telepítése

Az alkalmazás az Azure irányító tesztelése, mielőtt néhány kisebb módosításokat kell azt. Hajtsa végre az alábbi lépéseket a server.js fájlt:

1.  Visual Studio vagy bármilyen szövegszerkesztővel, nyissa meg a **server.js** fájlt.

2.  Server.js **modul függőségek** szakaszának az elejére, és módosítsa a sort tartalmazó **sio = require('.. //.. lib//Socket.IO ")** való **sio = require('socket.io')** alább látható módon:

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  Annak érdekében, hogy a megfelelő port figyel az alkalmazás, nyissa meg server.js Jegyzettömb vagy a kedvenc szerkesztő, és módosítsa a következő sort **3000** helyettesítve **process.env.port** alább látható módon:

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });

Után a változtatások mentése **server.js**, kövesse az alábbi lépéseket a szükséges modulokat telepítése, és kattintson az alkalmazás tesztelheti az Azure irányító:

1.  Könyvtárak **Azure PowerShell**, használatával módosíthatja a **C:\\csomópont\\chatapp\\WorkerRole1** könyvtárat és használja a következő parancsot a modulokat követel meg az alkalmazás telepítése:

        PS C:\node\chatapp\WorkerRole1> npm install

    Ez a package.json fájlban felsorolt modulok telepíti. A parancs befejeződése után meg kell jelennie a kimeneti az alábbihoz hasonló:

    ![A kimenet: a npm telepítési parancs][The-output-of-the-npm-install-command]

4.  Mivel ez a példa eredetileg a Socket.IO GitHub tárat egy részét, és közvetlenül hivatkoznak az Socket.IO tár relatív elérési utat, Socket.IO lett fájl nem hivatkozik a package.json, így akkor is telepítse a következő parancsot:

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Tesztelje és terjesztése

1.  Indítsa el a irányító a következő parancsot:

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  Nyissa meg a böngészőt, és kattintson a **http://127.0.0.1**.

3.  Amikor megnyílik a böngésző ablakában, írja be a becenév, majd találatot.
    Ennek hatására az összes, hogy az üzenetet tesz közzé, mint egy adott becenév. Több felhasználó működésének teszteléséhez nyissa meg a megegyező URL-CÍMMEL használata más böngészőablakot, és írja be a másik becenevei.

    ![Két böngészőablakot felhasználó1 és Felhasználó2 üzeneteit megjelenítése](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  Az alkalmazás tesztelés után megakadályozhatja, hogy a irányító a következő parancsot:

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  A **Közzététel-AzureServiceProject** parancsmag használatával az Azure alkalmazás telepítéséhez. Példa:

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] Ügyeljen arra, hogy egyedi nevet, egyéb esetben a közzétételi folyamat sikertelen lesz. A telepítés befejezése után a böngészőben nyissa meg, és keresse meg a telepített szolgáltatást.
    > 
    > Ha egy hibaüzenet arról, hogy az adott előfizetés nevét az importált közzététel profil nem létezik, töltse le, és a közzétételi profil importálása az Azure mielőtt az előfizetés. [Építse fel és telepítse az Azure Felhőszolgáltatásában egy Node.js alkalmazás](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/) **telepítése az alkalmazás Azure** szakaszában talál

    ![A szolgáltatás Azure is megjelenítése böngészőablakban][completed-app]

    > [AZURE.NOTE] Ha egy hibaüzenet arról, hogy az adott előfizetés nevét az importált közzététel profil nem létezik, töltse le, és a közzétételi profil importálása az Azure mielőtt az előfizetés. [Építse fel és telepítse az Azure Felhőszolgáltatásában egy Node.js alkalmazás](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/) **telepítése az alkalmazás Azure** szakaszában talál

Az alkalmazás letöltése Azure fut, és Socket.IO használatával különböző ügyfelek közötti Csevegés üzeneteket továbbíthat.

> [AZURE.NOTE] Az egyszerűség kedvéért a következő példában az Csevegés között a példányt csatlakozó felhasználók korlátozott. Ez azt jelenti, hogy a felhőbeli szolgáltatástól két dolgozó szerepkör példánya hoz létre, ha felhasználók csak akkor tud Csevegés csatlakozik a dolgozó szerepkör példányt másokkal. Az alkalmazás több szerepkör példányon dolgozhat méretezni, például szolgáltatás Bus technológia segítségével megoszthatja a Socket.IO tároló állam példányok. Példák című témakörben talál a súgótémaköröket, és a szolgáltatás Bus sorban várakozó használatát minták az [Azure SDK Node.js GitHub tárhoz](https://github.com/WindowsAzure/azure-sdk-for-node).

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan hozhat létre egy egyszerű Csevegés alkalmazást az Azure felhőszolgáltatásában is. Ez az alkalmazás-Azure webhely üzemeltetése című témakörben talál [egy Socket.IO az Azure webhely Node.js Csevegés alkalmazás összeállítása][chatwebsite].

További tudnivalókért lásd még: a [Node.js Developer Center](/develop/nodejs/).

  [chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/

  [Azure SLA]: http://www.windowsazure.com/support/sla/
  [Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
  [completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
  [Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
  [Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Socket.IO GitHub tárházba]: https://github.com/LearnBoost/socket.io/tree/0.9.14
  [Azure Considerations]: #windowsazureconsiderations
  [Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
  [Summary and Next Steps]: #summary
  [powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

  [chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
  [chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png
  
  
  [chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
  [The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
  [The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png
  
 
