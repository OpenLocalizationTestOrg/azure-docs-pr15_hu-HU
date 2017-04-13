<properties
    pageTitle="Hogyan szeretné hibakeresése Node.js webalkalmazást Azure App szolgáltatásban"
    description="Megtudhatja, hogyan szeretné hibakeresése Azure App szolgáltatásban Node.js webalkalmazást."
    tags="azure-portal"
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

# <a name="how-to-debug-a-nodejs-web-app-in-azure-app-service"></a>Hogyan szeretné hibakeresése Node.js webalkalmazást Azure App szolgáltatásban

Azure hibakeresése során az [Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps alkalmazásainak Node.js megkönnyítése beépített diagnosztika biztosít. Ebben a cikkben megismerheti, hogyan stdout és stderr naplózásának engedélyezése, hibaüzenet adatainak megjelenítése a böngészőben, és hogyan töltheti le, és a naplófájlok megtekintése.

Azure is Node.js alkalmazások diagnosztika [IISNode]által biztosított. Ez a cikk azt ismerteti, hogy a leggyakrabban használt beállítások a diagnosztikai adatok gyűjtése, amíg meg nem nyújt teljes hivatkozást IISNode a. További információt a IISNode használata nézze meg a [IISNode – fontos fájl] GitHub.

<a id="enablelogging"></a>
## <a name="enable-logging"></a>A naplózás engedélyezése

Alapértelmezés szerint az alkalmazás-szolgáltatás web app csak rögzíti a diagnosztikai információ megjelenítése telepítésekről, például amikor rendszerbe mely számjegy használatával webalkalmazást. Ez az információ akkor lehet hasznos, ha problémák merülnek fel, például egy **package.json**, a hivatkozott modul telepítésekor a telepítés során, vagy ha egy egyéni telepítési parancsfájlt használja.

A stdout és stderr adatfolyam a naplózás engedélyezése a Node.js alkalmazás gyökérszintjén hozzon létre egy **IISNode.yml** fájlt, és adja meg a következőket:

    loggingEnabled: true

A naplózás stderr és a Node.js levelezőprogramból stdout szolgáltatás lehetővé teszi.

A **IISNode.yml** fájlt is használható vezérlő e rövid hibák vagy Fejlesztőeszközök hibák kerülnek vissza a böngészőben Ha hiba történik. Ahhoz, hogy a fejlesztői hibákat, adja hozzá a következő sort a **IISNode.yml** fájlt:

    devErrorsEnabled: true

Ha ez a beállítás engedélyezve van, a IISNode küldött ezzel helyett egy könnyen megjegyezhető hiba, például "belső kiszolgáló hiba történt" adatok utolsó 64 ezer ad vissza.

> [AZURE.NOTE] A felhasználóknak küldött fejlesztési hibák devErrorsEnabled akkor hasznos, ha a problémáinak diagnosztizálása fejlesztés során, miközben engedélyezés munkakörnyezetben vonhat.

Ha a **IISNode.yml** fájl már nem létezik a alkalmazáson belül, a frissített alkalmazás közzététele után indítsa újra a web app. Egyszerűen egy meglévő **IISNode.yml** fájl, amely a korábban közzétett beállításait módosítani szeretné, ha az újraindítás nélküli szükség.

> [AZURE.NOTE] Ha a web App alkalmazásban készült Azure parancssori eszközök vagy Azure PowerShell-parancsmagok, automatikusan létrejön egy alapértelmezett **IISNode.yml** fájlt.

Indítsa újra a web App alkalmazásban, jelölje be a web app az [Azure-portálra](https://portal.azure.com), és kattintson az **Újraindítás** gombra:

![Indítsa újra a gomb][restart-button]

Ha az Azure parancssori eszközök a fejlesztői környezetben van telepítve, a következő parancs használatával indítsa újra a web app:

    azure site restart [sitename]

> [AZURE.NOTE] Habár loggingEnabled és devErrorsEnabled csak a leggyakrabban használt IISNode.yml konfigurációs beállítások a diagnosztikai adatok, IISNode.yml konfigurálása a üzemeltetési környezet beállításainak megadására számos használható. A beállítási lehetőségek teljes listáját a [iisnode_schema.xml](https://github.com/tjanczuk/iisnode/blob/master/src/config/iisnode_schema.xml) -fájl megtekintéséhez.

<a id="viewlogs"></a>
## <a name="accessing-logs"></a>Naplók elérése

A diagnosztikai naplók háromféleképpen; is elérhető A fájl Transfer Protocol (FTP segítségével), a Zip-archívumban letöltése vagy másként egy élő frissített adatfolyam a napló (más néven képviselőknek). A naplófájlok Zip-archívumban le, vagy az élő adatfolyam megtekintése szükség az Azure parancssori eszközök. A következő parancs használatával is telepíthetők:

    npm install azure-cli -g

Telepítése után az eszközök segítségével érhetők el a "azure" parancsot. A parancssori eszközök először kell beállítania, hogy az Azure előfizetés használja. A feladat elvégzéséhez tudnivalókért lásd a **letöltéséről importálása közzétételi beállításokat, és** szakaszában a [használja az Azure parancssori eszközök módjáról](../xplat-cli-connect.md) a cikk.

###<a name="ftp"></a>FTP

A diagnosztikai adatok eléréséhez FTP keresztül, keresse fel az [Azure-portálon](https://portal.azure.com), jelölje be a web App alkalmazásban, és válassza a az **IRÁNYÍTÓPULT**parancsra. A **tartalom** csoportban az **FTP a diagnosztikai NAPLÓKBAN** és a **Diagnosztikai naplók FTPS** hivatkozásokra kattintva a naplókat, az FTP protokollon keresztül hozzáférés.

> [AZURE.NOTE] Nem korábban állította be a felhasználónevét és jelszavát az FTP vagy telepítési, akkor teheti a **quickstart útmutató** kezelése lapon a **telepítési hitelesítő adatok**kijelölés.

Az irányítópult visszaadott FTP URL-CÍMÉT a **naplófájlok** könyvtár, amely tartalmazza a következő alszint könyvtárak van:

* [Telepítési mód](web-sites-deploy.md) – Ha a telepítési mód, például mely számjegy használja, az azonos nevű könyvtárában létrejön, és telepítések kapcsolódó adatokat tartalmazza.

* nodejs - Stdout és stderr információt az alkalmazás az összes előfordulását rögzített (teljesülésekor loggingEnabled.)

###<a name="zip-archive"></a>Zip-archiválása

Töltse le a diagnosztikai naplók Zip-archívumban, használja a Azure parancssori csoportban a következő parancsot:

    azure site log download [sitename]

Az aktuális mappában egy **diagnostics.zip** letöltése. Ez az archiválás a következő címtár szerkezetet tartalmazza:

* telepítések – az alkalmazás telepítések kapcsolatos információk naplózása

* Naplófájlok

    * [Telepítési mód](web-sites-deploy.md) – Ha a telepítési mód, például mely számjegy használja, az azonos nevű könyvtárában létrejön, és telepítések kapcsolódó adatokat tartalmazza.

    * nodejs - Stdout és stderr információt az alkalmazás az összes előfordulását rögzített (teljesülésekor loggingEnabled.)

###<a name="live-stream-tail"></a>Élő adatfolyam (képviselőknek)

Diagnosztikai adatok élő adatfolyam megtekintéséhez a Azure parancssori csoportban a következő parancsot használja:

    azure site log tail [sitename]

Ez ad vissza, amely a kiszolgálón megjelenésükkor frissülnek naplóbeli események adatfolyam. Követő visszaadja telepítési információ, valamint a stdout és stderr információk (teljesülésekor loggingEnabled.)

<a id="nextsteps"></a>
## <a name="next-steps"></a>Következő lépések

Ebben a cikkben megtanulta, hogy miként engedélyezheti és diagnosztika hozzáférhet az Azure. Amíg ez az információ akkor hasznos, az alkalmazás előforduló problémákat ismertetése, lépett fel egy modult használja, vagy Node.js alkalmazás szolgáltatás Webappok által használt verziója a telepítési környezet használt eltérő lehet, hogy mutat.

Információt a Azure modulok használata című témakörben talál [Node.js modulok használatával Azure-alkalmazásokkal](../nodejs-use-node-modules-azure-apps.md).

Az alkalmazás Node.js verziót megadásával kapcsolatban további tudnivalókért lásd [az Azure alkalmazásokban Node.js verziót megadása].

További tudnivalókért lásd még: a [Node.js Developer Center](/develop/nodejs/).

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

[IISNode]: https://github.com/tjanczuk/iisnode
[IISNode – fontos fájl]: https://github.com/tjanczuk/iisnode#readme
[How to Use The Azure Command-Line Interface]: ../xplat-cli-install.md
[Using Node.js Modules with Azure Applications]: ../nodejs-use-node-modules-azure-apps.md
[Egy Node.js verzió megadása az Azure alkalmazásokban]: ../nodejs-specify-node-version-azure-apps.md

[restart-button]: ./media/web-sites-nodejs-debug/restartbutton.png
 
