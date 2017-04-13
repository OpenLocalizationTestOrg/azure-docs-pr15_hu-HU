<properties 
    pageTitle="Az Azure alkalmazás szolgáltatás Web Apps alkalmazások használata a io.js" 
    description="Megtudhatja, hogy miként webalkalmazást io.js Azure alkalmazás szolgáltatás használata." 
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
    ms.author="robmcm" />

# <a name="how-to-use-iojs-with-azure-app-service-web-apps"></a>Az Azure alkalmazás szolgáltatás Web Apps alkalmazások használata a io.js

A népszerű csomópont elágazás [io.js] Joyent's Node.js projektbe, például több nyissa meg az irányítási modell, egy gyorsabb kiadási ciklus és egy új és kísérleti JavaScript-szolgáltatások gyorsabb elfogadása különböző különbségek szolgáltatásait.

Míg a [Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps alkalmazások előtelepített sok Node.js verziói, is lehetővé teszi a felhasználó által megadott Node.js binárisra. Ez a cikk azt ismerteti, hogy a két módszer io.js a alkalmazás szolgáltatás Web Apps használatának engedélyezése: egy bővített telepítési parancsfájlt, amely automatikusan konfigurálja a legújabb elérhető io.js, valamint a kézi feltöltése egy bináris io.js használandó Azure használatát. 

<a id="deploymentscript"></a>
## <a name="using-a-deployment-script"></a>Olyan telepítési parancsfájlt használata

Egy Node.js alkalmazás telepítéshez, alkalmazás szolgáltatás Web Apps alkalmazások fut, győződjön meg arról, hogy megfelelően van-e konfigurálva a környezet kis parancsok szám. Olyan telepítési parancsfájlt használ, ez az eljárás testre szabható a letöltés és io.js konfigurálása.

A [telepítési parancsfájlt io.js](https://github.com/felixrieseberg/iojs-azure) GitHub érhető el. Ha engedélyezni szeretné a web App io.js, egyszerűen másolja át az alkalmazás mappa gyökerébe **.deployment**, **deploy.cmd** és **IISNode.yml** , és telepítse az Web Apps alkalmazások.  

Az első fájl **.deployment**, arra utasítja Web Apps alkalmazások futtatásához **deploy.cmd** a telepítés után. A parancsfájl a szokásos lépéseket Node.js-alkalmazás elindul, de is letölti a io.js legújabb verzióját. Végezetül **IISNode.yml** Web Apps alkalmazások használata csak a letöltött io.js bináris helyett egy előre telepített Node.js binárisra állítja be.

> [AZURE.NOTE] A parancsprogram szeretné módosítani szeretné a használt io.js binárisra, csak telepítsen újra az alkalmazás - letölti io.js új verziója egyetlen mindig az alkalmazás telepítve van.

<a id="manualinstallation"></a>
## <a name="using-manual-installation"></a>Manuális telepítés használata

A manuális telepítés egy egyéni io.js verzió csak két lépést tartalmazza. Először töltse le a **win-x64** bináris közvetlenül a [io.js terjesztési]. Szükség van - **iojs.exe** és **iojs.lib**két fájlok. Mentheti a fájlokat egy mappát a web App alkalmazásban, például a **bin és iojs**belül.

Webalkalmazások helyett egy előre csomópont telepített **iojs.exe** használandó konfigurálásához **IISNode.yml** fájl létrehozása a legfelső szintű az alkalmazást, és vegye fel a következő sort.

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## <a name="next-steps"></a>Következő lépések

Ez a cikk szüksége a tanultakhoz használatáról az alkalmazás szolgáltatás Web Apps alkalmazásokkal, mindkét io.js adni telepítési parancsfájlok, valamint a manuális telepítés. 

> [AZURE.NOTE] IO.js nehéz fejlesztés és Node.js gyakrabban frissíteni. Számos Node.js modult nem működik együtt io.js - kérjük consult [io.js GitHub meg] az alábbiakkal.

## <a name="whats-changed"></a>Mi változott
* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás című: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

[IO.js]: https://iojs.org
[IO.js ki.]: https://iojs.org/dist/
[a GitHub IO.js]: https://github.com/iojs/io.js
[io.js Deployment Script]: https://github.com/felixrieseberg/iojs-azure
 