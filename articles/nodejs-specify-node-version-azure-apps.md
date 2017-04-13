<properties
    pageTitle="Egy Node.js verzió megadása"
    description="Megtudhatja, hogy miként Azure-webhelyek és a Cloud Services által használt Node.js verziójának megadása"
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

# <a name="specifying-a-nodejs-version-in-an-azure-application"></a>Egy Node.js verzió megadása az Azure alkalmazásokban

Ha egy Node.js alkalmazást üzemeltető, érdemes győződjön meg arról, hogy az alkalmazás Node.js adott verzióját használja. Többféleképpen is Azure-alkalmazásokhoz ehhez.

##<a name="default-versions"></a>Alapértelmezett verziójában

Az Azure által biztosított Node.js verziók folyamatosan frissülnek. Hiányában, az alapértelmezett verzióra megadott a `WEBSITE_NODE_DEFAULT_VERSION` környezeti szolgálnak. Ez az alapértelmezett érték felülbírálásához kövesse a következő szakaszokban a jelen cikk

> [AZURE.NOTE] Ha az alkalmazás-Azure felhőszolgáltatásában (webes vagy dolgozó szerepkör) üzemeltet, és amikor először telepítette az alkalmazást, Azure próbálja használni a Node.js a megfelelő verziójú, telepítette a fejlesztői környezet egyező az alapértelmezett verziók Azure közül.

##<a name="versioning-with-packagejson"></a>Verziószámozás a package.json

Megadhatja, hogy a **package.json** fájlhoz a következő hozzáadásával használandó Node.js verziója:

    "engines":{"node":version}

*Ha verziója használata bizonyos verziószámát.* Is megadhatja összetettebb feltételeket verzióhoz, például:

    "engines":{"node": "0.6.22 || 0.8.x"}

Mivel 0.6.22 nem érhető el az üzemeltetési környezet verziók egyikét, a sorozat elérhető lesz 0.8 legnagyobb verzióját használja - 0.8.4.

##<a name="versioning-websites-with-app-settings"></a>A verziószámozás webhelyek alkalmazás beállításokkal
Ha az alkalmazás egy webhelyen üzemeltet, beállíthatja, hogy a környezeti változó **WEBSITE_NODE_DEFAULT_VERSION** a kívánt verzióra. 

##<a name="versioning-cloud-services-with-powershell"></a>A verziószámozás Cloud Services szolgáltatást a PowerShell használatával

Ha üzemeltet, az alkalmazás egy felhőalapú szolgáltatásba, és az alkalmazás Azure PowerShell használatával telepít, az alapértelmezett Node.js verzió felülbírálhatja a **Set-AzureServiceProjectRole** PowerShell-parancsmag használatával. Példa:

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

Megjegyzés: a paramétereket a fenti utasításban-és nagybetűk.  Ellenőrizheti, hogy a megfelelő verziója Node.js, jelölje be a **motorok** tulajdonság a szerepkör **package.json**ki van jelölve.

A **Get-AzureServiceProjectRoleRuntime** segítségével Node.js verziók egy felhőalapú szolgáltatásba, üzemeltetett alkalmazások listáját.  Mindig ellenőrizze a függ, hogy a projekt Node.js verziója az ebben a listában.

##<a name="using-a-custom-version-with-azure-websites"></a>Egyéni verziójával Azure-webhely

Miközben Azure biztosít alapértelmezett különböző verzióiban Node.js, érdemes verziójával rendelkezik, amely nem szerepel a alapértelmezés szerint. Ha az alkalmazás-Azure webhelyként üzemelteti, ez elvégezhető a **iisnode.yml** fájl használatával. Az alábbi lépéseket azon a folyamaton, egyéni az verziójával Node.Js az Azure webhely ismerteti:

1. Hozzon létre egy új könyvtárat, és hozza létre a címtáron belül **server.js** fájlt. A **server.js** fájl tartalmaznia kell a következő:

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    Ez a webhely böngészés közben használt Node.js verziója fog megjelenni.

2. Új webhely létrehozása, és jegyezze fel a webhelyet. Ha például az alábbi használja az [Azure parancssori eszközök] Azure **segítségével**nevű új webhely létrehozása, és engedélyezheti a webhely mely számjegy összegyűjti.

        azure site create mywebsite --git

3. Hozzon létre egy új könyvtárat nevű **mappa** a **server.js** fájlt tartalmazó könyvtár stílusnak.

4. Töltse le az adott verziójának **node.exe** (a Windows-verzió), amelyet szeretne használni az alkalmazást. Például az alábbi használ **curl** verzió 0.8.1 letöltéséhez:

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    Mentse a **node.exe** fájlt **tároló** mappába a korábban létrehozott.

5. **Iisnode.yml** fájl létrehozása a **server.js** fájl ugyanabban a mappában, és kattintson a következő tartalom hozzáadása a **iisnode.yml** fájlt:

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    Ez az elérési út, ahol a projektben a **node.exe** fájl kerül, amikor az alkalmazás az Azure webhelyre közzétett.

6. Az alkalmazás a Közzététel gombra. Például lehet egy új webhely – mely számjegy paraméterrel a korábban létrehozott, mivel a következő parancsokat hozzáadja az alkalmazás fájljait a helyi mely számjegy tárházba, és kattintson a webhely tárházba leküldéses őket:

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    Az alkalmazás közzétételét, nyissa meg a webhelyet a böngészőben. Meg kell jelennie egy üzenet arról "Hello Azure futó csomópont verzióról: v0.8.1".

##<a name="next-steps"></a>Következő lépések

Most, hogy megismeri adja meg az alkalmazás által használt Node.js verzióját, és megtudhatja, hogy miként [modulok dolgozni], [létre és helyezhetnek üzembe a Node.js webhelyen], és [a Mac- és Linux Azure parancssori eszközeivel miként].

További tudnivalókért lásd: a [Node.js Developer Center](/develop/nodejs/).

[Az Azure parancssori eszközök használatáról a Mac- és Linux]: xplat-cli-install.md
[Azure parancssori eszközök]: xplat-cli-install.md
[modulokat használata]: nodejs-use-node-modules-azure-apps.md
[építse fel és telepítse a Node.js webhely]: web-sites-nodejs-develop-deploy-mac.md
