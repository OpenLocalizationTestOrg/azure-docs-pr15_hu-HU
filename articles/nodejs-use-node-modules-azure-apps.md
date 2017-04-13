<properties
    pageTitle="Node.js modulok használata"
    description="Megtudhatja, hogy miként dolgozhat az Azure alkalmazás szolgáltatás vagy Cloud Services használata során Node.js modulokat."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Azure alkalmazások Node.js modulok használata

A dokumentum nyújt útmutatást Node.js modulok használatával is Azure-alkalmazásokkal. Biztosítva, hogy az alkalmazás egy adott verzióját használja, a modul, valamint a natív modulok használata Azure nyújt útmutatást.

Ha már jól ismert Node.js modulokat, **package.json** és **npm-shrinkwrap.json** fájlok, a következő használata a jelen cikkben tárgyalt mi rövid összefoglalása:

* Azure alkalmazás szolgáltatás **package.json** és **npm-shrinkwrap.json** fájlok értelmez, és modulok bejegyzések ezeket a fájlokat az alapján telepítheti.
* Azure Cloud Services várt minden modul telepítve legyen azon a fejlesztői környezet és a **csomópont\_modulok** Directoryval, hogy a telepítőcsomag része lesz. Telepítése modulok **package.json** vagy **npm-shrinkwrap.json** fájlok Cloud Services, a használatát, azonban ehhez a Felhőbeli szolgáltatástól projektek által használt alapértelmezett parancsfájlok testreszabási támogatásának engedélyezése lehetőség. Példa Ehhez olvassa el a [Azure indítási feladat futtatása npm telepítés elkerülése érdekében a csomópont modul telepítése](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown) című témakört.

> [AZURE.NOTE] Azure virtuális gépeken futó nem tárgyalt ebben a cikkben, mert egy virtuális a telepítési működését lesz az operációs rendszer a virtuális gép által üzemeltetett függ.

##<a name="nodejs-modules"></a>NODE.js modulok

Modulok bizonyos funkciókat nyújt az alkalmazás terhelésátcsoportosító JavaScript-csomagokat. A modul általában telepítve van a **npm** parancssori eszközzel, azonban néhány (például a HTTP-modul) is közöljük az alapvető Node.js csomag részeként.

Modul telepítésekor mappába kerülnek a **csomópont\_modulok** könyvtár a legfelső szintű az alkalmazás a címtár-struktúra. Belül minden modulban a **csomópont\_modulok** címtár akiknek saját **csomópont\_modulok** , amelytől függ, és ez a minden modul teljes mértékben a függőség lánc le ismét ismételt modulokat tartalmazó könyvtár. Minden telepítve van a saját verziókövetelményének modulokat Ez attól függ, azonban okozhat struktúrában igen nagy directory modul lehetővé teszi.

Telepítésekor a **csomópont\_modulok** az alkalmazás részét képező címtár, azt fog méretének növelése a példányban **package.json** vagy **npm-shrinkwrap.json** fájllal; és összehasonlítása azonban garantálja, hogy a modulokat előállítására használt verzióját ugyanazok, mint egy fejlesztés alatt álló használtakkal.

###<a name="native-modules"></a>Natív modulok

A legtöbb modul egyszerűen egyszerű szöveges JavaScript-fájlok, bizonyos modulok platform-specifikus bináris képeket is. Ezeket a modulokat összeállítása telepítés időben általában Python és csomópont-gyp használatával. Mivel az Azure Cloud Services támaszkodhat a **csomópont\_modulok** üzembe helyezéséhez az alkalmazás, bármilyen natív modul a telepített modulok részét képező részeként mappát kell dolgoznia egy felhőalapú szolgáltatásba, amíg meg lett telepítve, és lefordított fejlesztési Windows rendszeren.

Azure alkalmazás szolgáltatás nem támogatja az összes natív modult, és előfordulhat, hogy nem rendelkezők nagyon konkrét Előfeltételek összeállítása a. Néhány népszerű modulokat, például MongoDB választható natív függőségek, és nélkülük csak finom munka van, miközben a két megoldások igazolni sikeres kapható szinte minden natív modul:

* Futtassa a **npm telepítése** Windows gépen, amelyen a natív modul összes Előfeltételek telepítve. Telepítheti a létrehozott **csomópont\_modulok** az alkalmazás Azure alkalmazás szolgáltatás részét képező mappát.
* Azure alkalmazás szolgáltatás beállítható úgy, hogy hajtsa végre az egyéni bash vagy rendszerhéj-parancsfájlokat a telepítés során biztosítva a Futtatás egyéni parancsok végrehajtása, és pontosan a **npm telepítése** módon beállítása lehetőséget. Ehhez, akkor olvassa el az [Egyéni webhely üzembe parancsfájlok Kudu]ábrázoló videók.

###<a name="using-a-packagejson-file"></a>Package.json fájl használatával

**Package.json** fájl, ha szeretne, adja meg a legfelső szintű függőségeket az alkalmazáshoz szükséges, hogy a használat platform telepítheti a függőségeket, és nem igénylő, amelyet fel szeretne venni a **csomópont\_csomagok** mappában a telepítés részeként. Az alkalmazás telepítését követően a **npm telepítése** parancs a **package.json** fájl elemezni, és telepítse a felsorolt összes függőségek szolgál.

Fejlesztés, során használható a **– mentése**, **– Mentés-fejlesztők**, vagy **– Mentés választható** paraméterek modulokat **package.json** fájljait a modul szöveg automatikus beszúrása telepítésekor. További tudnivalókért olvassa el a [npm-telepítés](https://docs.npmjs.com/cli/install)című témakört.

A **package.json** fájlt egy potenciális problémája, hogy csak adja meg a legfelső szintű függőségek verzió. Minden modul telepítve van is, vagy nem adja meg a modulokat, amelytől függ verzióját, és úgy is lehet, hogy, végül a különböző típusú függés lánc, mint egy fejlesztés használt.

> [AZURE.NOTE]
> Telepítésekor Azure alkalmazás szolgáltatást, ha a <b>package.json</b> fájl hivatkozik natív modul jelenik a következőhöz hasonló hiba az alkalmazást, mely számjegy közzététele:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Npm-shrinkwrap.json fájl használatával

**Npm-shrinkwrap.json** fájl kísérlet a **package.json** fájl modul verziószámozás korlátozások címet. A **package.json** fájl csak a legfelső szintű modulok verzióinak tartalmazza, miközben a **npm-shrinkwrap.json** fájl tartalmaz a teljes modul függőség lánc verziókövetelményének.

Ha készen áll a termelési az alkalmazást, akkor zárolhatja-verzióra vonatkozó követelmények lefelé, és egy **npm-shrinkwrap.json** fájl létrehozása a **npm shrinkwrap** parancs használatával. A jelenleg telepített verzióinak használata a **csomópont\_modulok** mappát, és rögzítése, ezek a **npm-shrinkwrap.json** fájlt. Az alkalmazás telepítését a üzemeltetési környezet, követően a **npm telepítése** parancs segítségével elemezni az **npm-shrinkwrap.json** fájlt, és telepítse a felsorolt összes függőségek. További tudnivalókért olvassa el a [npm-shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap)című témakört.

> [AZURE.NOTE]
>Telepítésekor Azure alkalmazás szolgáltatást, ha a <b>npm-shrinkwrap.json</b> fájl hivatkozik natív modul jelenik a következőhöz hasonló hiba az alkalmazást, mely számjegy közzététele:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Következő lépések

Most, hogy megismeri Node.js modulok használata Azure, megtudhatja, miként [Adja meg a Node.js verziót], [létre és helyezhetnek üzembe a Node.js webalkalmazást], és [a Mac- és Linux Azure parancssori felületének használata].

További tudnivalókért lásd: a [Node.js Developer Center](/develop/nodejs/).

[Adja meg a Node.js verziója]: nodejs-specify-node-version-azure-apps.md
[Azure parancssort használatáról a Mac- és Linux]: xplat-cli-install.md
[építse fel és telepítse a megfelelő Node.js web App alkalmazásban]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Egyéni webhely üzembe parancsfájlok Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
