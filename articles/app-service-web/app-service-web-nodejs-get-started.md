<properties
    pageTitle="Azure App szolgáltatásban Node.js web Apps alkalmazások – első lépések"
    description="Tudnivalók a web App alkalmazás Azure szolgáltatásban egy Node.js alkalmazás telepítéséhez."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Azure App szolgáltatásban Node.js web Apps alkalmazások – első lépések

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ebből az oktatóanyagból megtudhatja, hogy miként hozzon létre egy egyszerű [Node.js] alkalmazást, és a parancssori-környezetből, például cmd.exe [Azure alkalmazás] szolgáltatás üzembe vagy bash. Ebben az oktatóprogramban az utasításokat bármely operációs rendszeren futtatható Node.js követheti.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

<a name="prereq"></a>
## <a name="prerequisites"></a>Előfeltételek

- [NODE.js]
- [Bower]
- [Yeoman]
- [Mely számjegy]
- [Azure CLI]
- A Microsoft Azure-fiókjába. Ha nem rendelkeznek fiókkal, akkor [Jelentkezzen be az ingyenes próbaverzióra] , vagy [a Visual Studio előfizetői előnyeinek aktiválása].

## <a name="create-and-deploy-a-simple-nodejs-web-app"></a>Hozzon létre és telepítsen egy egyszerű Node.js web App alkalmazásban

1. Nyissa meg a lehetőség a parancssori terminált, és [Express Yeoman-nyilvántartás-készítő alkalmazás]telepítéséhez.

        npm install -g generator-express

2. `CD`a munkakönyvtárat, és állítson fel a következő szintaxissal express alkalmazást:

        yo express
        
    Válasszon az alábbi lehetőségek, amikor a rendszer kéri:  

    `? Would you like to create a new directory for your project?`**Igen**  
    `? Enter directory name`**{Alkalmazásnév}**  
    `? Select a version to install:`**MVC**  
    `? Select a view engine to use:`**Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):`**Nincs lehetőség**  
    `? Select a database to use:`**Nincs lehetőség**  
    `? Select a build tool to use:`**Grunt**

3. `CD`a legfelső szintű címtárhoz az új alkalmazást, és indítsa el, hogy azt a fejlesztői környezet fut:

        npm start

    A böngészőben nyissa meg azt a <http://localhost:3000> kattintva győződjön meg arról, hogy megjelenik a Express kezdőlapjára. Miután a alkalmazást futtató megfelelően végzett, `Ctrl-C` leállítása azt.
    
1. ASM módba módosítása, és jelentkezzen be az Azure (szükséges [Azure CLI](#prereq)):

        azure config mode asm
        azure login

    Kövesse a kérdésre, továbbra is a bejelentkezést a böngészőben az Azure előfizetést tartalmazó Microsoft-fiókjával.

2. Ellenőrizze, hogy az alkalmazás a legfelső szintű könyvtár épp, majd az alkalmazás szolgáltatás alkalmazás erőforrás létrehozása egy egyedi alkalmazás neve a következő parancs az Azure-ban. Példa: http://{appname}.azurewebsites.net

        azure site create --git {appname}

    Kövesse az Azure régió szeretne telepíteni, jelölje be a kérdés. Ha soha nem beállította mely számjegy/FTP telepítési hitelesítő adatok Azure-előfizetéséhez, is kérni fogja tabulátorok létrehozására.

3. Nyissa meg a./config/config.js fájlt az alkalmazás gyökerében, és módosítsa a gyártási port `process.env.port`; a `production` tulajdonság a `config` objektum a következő példához hasonlóan kell kinéznie:

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Ezzel a lehetőséggel a válasz, a webes alapértelmezett portja a kérelmeket, hogy iisnode figyeli Node.js alkalmazást.
    
4. Nyissa meg a./package.json, és adja hozzá a `engines` tulajdonságot, [Adja meg a kívánt Node.js verziót](#version).

        "engines": {
            "node": "6.6.0"
        }, 

4. Mentse a módosításokat, majd telepítse az alkalmazást az Azure mely számjegy használatával:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Express nyilvántartás-készítő alkalmazás már tartalmaz egy .gitignore fájlt, így a `git push` nem felhasználása próbál töltse fel a node_modules sávszélesség / könyvtár.

5. Végezetül indítása az élő Azure alkalmazást a böngészőben:

        azure site browse

    Ekkor megjelennek az élő Azure alkalmazás szolgáltatás fut Node.js webalkalmazást.
    
    ![Példa a telepített alkalmazás megnyitásához.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Frissítse a Node.js web App alkalmazásban

A frissítéseket a Node.js web App alkalmazás szolgáltatás fut, csak futtatása `git add`, `git commit`, és `git push` amikor először telepítette a web app megismert.
     
## <a name="how-app-service-deploys-your-nodejs-app"></a>Hogyan alkalmazás szolgáltatás üzembe helyezi a szolgáltatást a Node.js alkalmazás

Azure alkalmazás szolgáltatást használja a [iisnode] Node.js alkalmazások futtatásához. Az Azure CLI és a Kudu motor (mely számjegy-telepítés) együttes ad a egyesíti környezet kialakítása, és a parancssorból Node.js alkalmazások terjesztése. 

- `azure site create --git`a közös Node.js minta server.js vagy app.js felismeri, és létrehoz egy iisnode.yml a legfelső szintű címtárában található. Ez a fájl iisnode testre is használhatja.
- A `git push azure master`, Kudu automatizálja az alábbi telepítési műveleteket:

    - A tárházba legfelső szintű package.json esetén futtassa `npm install --production`.
    - A iisnode, amely a kezdő parancsfájl package.json (pl. server.js vagy app.js) mutat Web.config készítése.
    - Testre szabhatja, hogy készen áll az alkalmazás a csomópont-Felügyelőmodulok hibakeresési Web.config.
    
## <a name="use-a-nodejs-framework"></a>Egy Node.js keretrendszer használata

Ha használhatja a népszerű Node.js keretrendszer, például [Sails.js] [ SAILSJS] vagy [MEAN.js] [ MEANJS] alkalmazásokat fejleszt, hogy azok alkalmazás szolgáltatás telepítheti. Népszerű Node.js keretek kell az adott régi, és azok csomag függőségek megtartása első frissíteni. Alkalmazás szolgáltatás azonban a stdout és stderr naplók érhetők el, úgy, hogy pontosan mi történik az alkalmazást, és végezze el a módosításokat megfelelően. További tudnivalókért olvassa el a [stdout és stderr naplók iisnode az első](#iisnodelog)című témakört.

Az alábbi oktatóanyagok kell követnie az egy adott keretrendszer alkalmazás szolgáltatásban való használatra:

- [Azure alkalmazás szolgáltatás Sails.js webes alkalmazások terjesztése]
- [Azure App szolgáltatásban Socket.IO Node.js Csevegés-alkalmazás létrehozása]
- [Az Azure alkalmazás szolgáltatás Web Apps alkalmazások használata a io.js]

<a name="version"></a>
## <a name="use-a-specific-nodejs-engine"></a>Egy adott Node.js motor használata

A szokásos munkafolyamat megadhatja, hogy egy adott Node.js motor használni, a szokásos módon package.json az alkalmazás szolgáltatás.
Példa:

    "engines": {
        "node": "6.6.0"
    }, 

A Kudu telepítési motor határozza meg, hogy melyik Node.js motor használni a következő sorrendben:

- Először nézzük iisnode.yml annak ellenőrzéséhez, hogy `nodeProcessCommandLine` van megadva. Ha igen, majd használhatja.
- Ezután meg package.json annak ellenőrzéséhez, hogy a `"node": "..."` van megadva a `engines` objektumot. Ha igen, majd használhatja.
- Válassza az alapértelmezett Node.js verzióra alapértelmezés szerint.

>[AZURE.NOTE] Ajánlott a Node.js motor, amelyet kifejezetten megadása. Módosíthatja az alapértelmezett Node.js verzióra, és előfordulhat, hogy el hibák az Azure-webalkalmazásban, mivel az alapértelmezett Node.js verzió nem megfelelő, az alkalmazás.

<a name="iisnodelog"></a>
## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Iisnode stdout és stderr naplók beolvasása

Iisnode naplók olvasása, kövesse az alábbi lépéseket.

> [AZURE.NOTE] Az alábbi lépések elvégzése után naplófájlok nem létezik mindaddig, amíg a hiba történik.

1. Nyissa meg a iisnode.yml fájlt, amely az Azure CLI biztosít.

2. A két következő paraméterek beállítása: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Közös, azok megállapítani, hogy iisnode alkalmazás szolgáltatásban az stdout és stderror eredményt elhelyezni a D:\home\site\wwwroot\**iisnode** címtár.

3. Mentse a módosításokat, majd a módosítások leküldéses az Azure az alábbi mely számjegy parancsok:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
    iisnode most már be van állítva. A következő lépésekkel megtudhatja, hogyan érhető el ezeket a naplókat.
     
4. A böngészőben elérhető az alkalmazás, amely a Kudu hibakeresési konzol:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Ez az URL hozzáadásával eltér a web app URL-címe "*.scm.*" a DNS-nevét. Ha nincs megadva, hogy az URL-címre összeadás, egy 404-es hiba jelenik meg.

5. Nyissa meg azt a D:\home\site\wwwroot\iisnode

    ![Navigálás a iisnode naplófájlok helye.][iislog-kudu-console-find]

6. Kattintson a **Szerkesztés** ikonra a napló el szeretné olvasni. Ha azt szeretné, **Töltse le** és **törlése** is gombra.

    ![Egy iisnode naplófájl megnyitásakor.][iislog-kudu-console-open]

    Ekkor megjelenik a napló segít a alkalmazás szolgáltatás üzembe hibakeresési.
    
    ![Egy iisnode naplófájlt vizsgálata.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Az alkalmazás a csomópont-Felügyelőmodulok hibakeresési

Csomópont-felügyelő szeretné hibakeresése Node.js alkalmazás használatakor az élő számára alkalmazás szolgáltatást használhatja. Csomópont-felügyelő telepítve van az alkalmazás szolgáltatás iisnode telepítését. És ha mely számjegy keresztül telepítette, a Kudu az automatikusan generált Web.config már tartalmaz-e a csomópont-felügyelő engedélyeznie kell az összes konfigurációs.

Ha engedélyezni szeretné a felügyelő csomópontot, kövesse az alábbi lépéseket:

1. Nyissa meg a tárházba gyökerében iisnode.yml, és adja meg a következő paraméterek: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Mentse a módosításokat, majd a módosítások leküldéses az Azure az alábbi mely számjegy parancsok:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Most egyszerűen keresse meg azt az alkalmazás indítása fájl a kezdő parancsfájl a package.json, a megadott URL-címe hozzáadott Debug. Ha például

        http://{appname}.azurewebsites.net/server.js/debug
    
    Másik lehetőségként
    
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>További források

- [Egy Node.js verzió megadása az Azure alkalmazásokban](../nodejs-specify-node-version-azure-apps.md)
- [Ajánlott eljárások és Azure a Node.js alkalmazások hibaelhárítási útmutatója](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Hogyan szeretné hibakeresése Node.js webalkalmazást Azure App szolgáltatásban](web-sites-nodejs-debug.md)
- [Azure alkalmazások Node.js modulok használata](../nodejs-use-node-modules-azure-apps.md)
- [Azure alkalmazás szolgáltatás Web Apps alkalmazások: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [NODE.js Developer Center](/develop/nodejs/)
- [Azure App szolgáltatásban web Apps alkalmazások – első lépések](app-service-web-get-started.md)
- [A kiemelt titkos Kudu hibakeresési konzol felfedezése]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure alkalmazás szolgáltatás]: ../app-service/app-service-value-prop-what-is.md
[a Visual Studio előfizetői juttatások aktiválása]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Azure App szolgáltatásban Socket.IO Node.js Csevegés-alkalmazás létrehozása]: ./web-sites-nodejs-chat-app-socketio.md
[Azure alkalmazás szolgáltatás Sails.js webes alkalmazások terjesztése]: ./app-service-web-nodejs-sails.md
[A kiemelt titkos Kudu hibakeresési konzol felfedezése]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Express Yeoman nyilvántartás-készítő alkalmazás]: https://github.com/petecoop/generator-express
[Mely számjegy]: http://www.git-scm.com/downloads
[Az Azure alkalmazás szolgáltatás Web Apps alkalmazások használata a io.js]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[NODE.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[Regisztráljon az ingyenes próbaverzióra]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png
