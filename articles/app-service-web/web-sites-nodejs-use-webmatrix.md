<properties 
    pageTitle="Építse fel és telepítse a Node.js webalkalmazást az Azure WebMatrix használatával" 
    description="Ez az oktatóanyagot készítsünk Node.js alkalmazást, és üzembe Azure alkalmazás szolgáltatás Web Apps alkalmazások használata a WebMatrix útmutatást ad meg." 
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


# <a name="build-and-deploy-a-nodejs-web-app-to-azure-using-webmatrix"></a>Építse fel és telepítse a Node.js webalkalmazást az Azure WebMatrix használatával

Ebben az oktatóanyagban bemutatja, hogyan WebMatrix használatával készítsünk Node.js alkalmazást, és [Azure alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps alkalmazások telepítheti. WebMatrix, amely tartalmaz minden webhelyén vagy a web app fejlesztési van szüksége a Microsoft ingyenes webes fejlesztői eszköz. WebMatrix szolgáltatásai több könnyen használható Node.js, beleértve a kódot befejezése, a beépített sablonok és a szerkesztő támogatása Jade, kisebb, illetve CoffeeScript. További tudnivalók a [WebMatrix](https://www.microsoft.com/web/webmatrix/).

Ez az útmutató befejeztével Azure alkalmazás szolgáltatást futtató Node.js webalkalmazást lesz.
 
Képernyőkép a kész alkalmazást nem éri el:

![Azure csomópont webhelyén][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="sign-into-azure"></a>Jelentkezzen be az Azure

Kövesse ezeket a lépéseket követve hozzon létre webalkalmazást Azure App szolgáltatásban.

1. Indítsa el a WebMatrix
2. Ha most először használta WebMatrix, felkéri a program jelentkezhet be az Azure.  Egyéb esetben kattintson a **Bejelentkezés** gombra, és válassza a **Fiók hozzáadása**lehetőséget.  Válassza a **Bejelentkezés** a Microsoft Account.

    ![Fiók hozzáadása][addaccount]

3. Ha Ön regisztráltak Azure-fiók, előfordulhat, hogy bejelentkezés a Microsoft Account:

    ![Jelentkezzen be az Azure][signin]  


## <a name="create-a-site-using-a-built-in-template-for-azure"></a>Beépített sablon használatával Azure-webhely létrehozása

1. A kezdőképernyőn kattintson az **Új** gombra, és válassza ki a **Sablonok** az új webhely létrehozása a sablon gyűjteményből:

    ![Új webhely sablongyűjteményéből szeretne][sitefromtemplate]

2. A **sablon egy webhelyről** párbeszédpanelen kattintson a **csomópontra** , és válassza a **Express-webhely**. Végül kattintson a **Tovább**gombra. Ha bármelyik **Express** webhelysablon előfeltételei eltűntek, a rendszer kéri, telepítheti őket.

    ![express-sablon kiválasztása][webmatrix-templates]

3. Ha be van jelentkezve, az Azure, ekkor lehetőség van az alkalmazás szolgáltatás web app a helyi webhely létrehozására.  Válasszon egy egyedi nevet, és válassza ki az adatközpont, ahol meg szeretné létrehozni az alkalmazás szolgáltatás webalkalmazást: 

    ![Azure a webhely létrehozása][nodesitefromtemplateazure]
    
4. WebMatrix az adott helyen összeállítását, és az alkalmazás szolgáltatás webalkalmazás létrehozása befejezése után megjelenik a WebMatrix IDE.

    ![webmatrix ide][webmatrix-ide]

##<a name="publish-your-application-to-azure"></a>Az alkalmazás Azure közzététele

1. WebMatrix, a menüszalagról a **Home** a webhelyen a **Kép közzététele** párbeszédpanel megjelenítéséhez kattintson a **Közzététel** gombra.

    ![kép közzététele][webmatrix-node-publishpreview]

2. Kattintson a **továbbra is**. Közzététel befejeződött, a alkalmazás szolgáltatás web app URL-CÍMÉT az WebMatrix IDE alján jelenik meg

    ![közzététel kész][webmatrix-publish-complete]

3. Kattintson a hivatkozásra kattintva nyissa meg az App szolgáltatás web app a böngészőben.

    ![Express-web App alkalmazásban][webmatrix-node-express-site]

##<a name="modify-and-republish-your-application"></a>Az alkalmazás ismételt közzététele és módosítása

Egyszerűen módosítása és ismételt közzététele az alkalmazás. Ebben az esetben a címsorra a **index.jade** fájlban módosítása, és az alkalmazás ismételt közzététele egy egyszerű láthatóvá válik.

1. WebMatrix jelölje ki a **fájlt**, és majd bontsa ki a **nézetek** mappát. Dupla kattintással nyissa meg a a **index.jade** fájlt.

    ![webmatrix megtekintése index.jade][webmatrix-modify-index]

2. Módosítsa a bekezdés sort a következőket:

        p Welcome to #{title} with WebMatrix on Azure!

3. Mentse a módosításokat, és kattintson a közzététel ikonra. Végül a **Kép közzététele** párbeszédpanelen kattintson a **Tovább** gombra, majd megvárja, amíg a közzéteendő frissítés.

    ![kép közzététele][webmatrix-republish]

4. Amikor befejezte a közzétételt, a visszaadott befejeződésekor a közzétételi folyamat a frissített alkalmazás szolgáltatás web app megtekintéséhez hivatkozásra.

    ![Azure csomópont web App alkalmazásban][webmatrix-node-completed]

##<a name="next-steps"></a>Következő lépések

Többet szeretne tudni az Azure és a használt az alkalmazással verzió megadása Node.js verziója, lásd: [Adja meg az Azure alkalmazásokban Node.js verziót](../nodejs-specify-node-version-azure-apps.md).

Ha után az Azure lett telepítve az alkalmazással problémákat tapasztal, itt tájékozódhat [szeretné hibakeresése Azure App szolgáltatásban Node.js webalkalmazást](web-sites-nodejs-debug.md) információt a probléma azonosításában.

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 