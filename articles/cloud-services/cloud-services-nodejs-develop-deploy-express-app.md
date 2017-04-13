<properties 
    pageTitle="Webes Express (Node.js) az alkalmazás |} Microsoft Azure" 
    description="A felhőalapú szolgáltatás oktatóprogram épül, és bemutatja, hogyan kell használni a Express modul oktatóanyagot." 
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






# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Express-Azure felhőalapú szolgáltatás használata Node.js webalkalmazás összeállítása

Az alapvető futtatókörnyezet funkciók minimális számú node.js tartalmazza.
A fejlesztők gyakran segítségével 3 fél modulok nyújt további szolgáltatásokat, amikor a Node.js-alkalmazások fejlesztése. Ebben az oktatóanyagban létrehoz egy új alkalmazást a [Express][] modul, amely előírja egy MVC keretrendszer Node.js webalkalmazások létrehozása.

Képernyőkép a kész alkalmazást nem éri el:

![Üdvözli az Azure Express megjelenítése webböngészőben](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##<a name="create-a-cloud-service-project"></a>Felhőalapú szolgáltatás projekt létrehozása

A következő lépésekkel "expressapp" nevű új felhőalapú szolgáltatás projekt létrehozása:

1. A **Start menüből** vagy **Kezdőképernyőjén**keressen rá **A Windows PowerShell**. Végül kattintson a jobb gombbal a **Windows PowerShell** , és válassza a **Futtatás rendszergazdaként**.

    ![Azure PowerShell ikon](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. Könyvtárak módosítása a **c:\\csomópont** címtár, és írja be az alábbi parancsok **expressapp** nevű új megoldást és a webes szerepkör **WebRole1**nevű létrehozásához:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] Alapértelmezés szerint az **Add-AzureNodeWebRole** Node.js régebbi verzióját használja. A fenti **Set-AzureServiceProjectRole** utasítás arra utasítja az Azure csomópont v0.10.21 használni.  Megjegyzés: a paraméterek-és nagybetűk.  Ellenőrizheti, hogy a megfelelő verziója Node.js ki van jelölve, jelölje be a **WebRole1\package.json** **motorok** tulajdonságot.

##<a name="install-express"></a>Express telepítése

1. Express nyilvántartás-készítő alkalmazás telepítése a következő parancsot:

        PS C:\node\expressapp> npm install express-generator -g

    A npm parancs az eredmény az alábbi hasonlóan kell kinéznie. 

    ![A kimenet: a npm megjelenítése a Windows PowerShell telepítése express parancsot.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. Váltson az könyvtárak **WebRole1** , és használja a sürgős parancsot új alkalmazás létrehozása:

        PS C:\node\expressapp\WebRole1> express

    A rendszer kéri írja felül a korábbi alkalmazást. Adja meg az **y** vagy az **Igen gombra** a folytatáshoz. Express hoz létre, a app.js fájl, és a mappastruktúra az alkalmazás készítéséhez.

    ![A kimenet a sürgős gomb](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  Telepítse a package.json fájlban definiált további függőségek, írja be a következő parancsot:

        PS C:\node\expressapp\WebRole1> npm install

    ![A kimenet: a npm telepítési parancs](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  A következő parancs használatával másolhatja a **bin és www** **server.js**. Ez a helyzet a felhőbeli szolgáltatástól megtalálhatja a belépési pontjához ehhez az alkalmazáshoz.

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    Ez a parancs befejeződése után rendelkeznie kell a WebRole1 címtárban **server.js** fájl.

7.  Módosítsa a **server.js** egyik eltávolításához a "." a következő sor karaktereit.

        var app = require('../app');

    Ez a módosítás elvégzése után a sor jelenjen meg az alábbi képlettel történik.

        var app = require('./app');

    Ez a változás szükség, mert azt át a fájlt (korábbi nevén **bin/www**) a azonos címtárhoz szükséges alkalmazás-fájlként. A módosítások végrehajtása után mentse a **server.js** fájlt.

8.  A következő parancsot használja az alkalmazás futtatásához kattintson az Azure irányító:

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Üdvözli a express tartalmazó weblapot.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>A nézet módosítása

Most módosítsa a nézetet az "Üdvözöljük való Express az Azure" üzenet megjelenítéséhez.

1.  Írja be a index.jade fájl megnyitásakor a következő parancsot:

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![A index.jade fájl tartalmát.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade az alapértelmezett nézet motor Express alkalmazások által használt. A nézet Jade motor kapcsolatos további tudnivalókért olvassa el a [http://jade-lang.com][]című témakört.

2.  Módosíthatja a szöveg utolsó sor hozzáfűzése **Azure-ban**.

    ![A index.jade fájlt az utolsó sor felolvassa: p Üdvözöljük \#{title} Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  Mentse a fájlt, és lépjen ki a Jegyzettömbből.

4.  Frissítse a böngészőt, és látni fogja a módosításokat.

    ![A böngésző ablakában, a lap tartalmazza üdvözli a Express Azure-ban](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Az alkalmazás tesztelés után a **Leállítás-AzureEmulator** parancsmag használatával a irányító leállítása.

##<a name="publishing-the-application-to-azure"></a>Az alkalmazás Azure közzététele

Az Azure PowerShell ablakában a **Közzététel-AzureServiceProject** parancsmag használatával telepíteni az alkalmazást, ha egy felhőalapú szolgáltatásba

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Ha a telepítési művelet befejeződött, a böngészőben nyissa meg, és az weblapon megjelenítéséhez.

![Egy webböngészőben, a Express lap megjelenítéséhez. Az URL-cím azt jelzi, hogy most Azure is.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [Node.js Developer Center](/develop/nodejs/).

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://Jade-Lang.com]: http://jade-lang.com

 
