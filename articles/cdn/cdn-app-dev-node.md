<properties
    pageTitle="Első lépések az Azure CDN SDK az Node.js |} Microsoft Azure"
    description="Megtudhatja, hogy miként írhat Node.js alkalmazások kezeléséhez Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure CDN fejlesztési – első lépések

> [AZURE.SELECTOR]
- [NODE.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

Az [Azure CDN SDK Node.js](https://www.npmjs.com/package/azure-arm-cdn) automatizálhatja a létrehozásának és kezelésének CDN-profilok és a végpontokat is használhatja.  Az alábbiakban ebben az oktatóprogramban egy egyszerű Node.js konzol alkalmazás, amely bemutatja az elérhető műveletek számos létrehozását.  Ebben az oktatóprogramban az Azure CDN SDK részletekbe menően az Node.js részletesen leírja nem szolgál.

Ebben az oktatóanyagban befejezéséhez meg kell már [Node.js](http://www.nodejs.org) **4.x.x** vagy magasabb telepítette és beállította.  Bármilyen szövegszerkesztővel szeretne létrehozni a Node.js alkalmazás is használhatja.  [Visual Studio-kódot](https://code.visualstudio.com)használt ebben az oktatóanyagban kell írni.  

> [AZURE.TIP] [Ebben az oktatóanyagban a projekt befejezését](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) MSDN letöltésre érhető el.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>A projekt létrehozása és NPM függőségek hozzáadása

Most, hogy már létrehozott erőforráscsoport az a CDN-profil, és engedélyt az Azure Active Directory alkalmazás CDN profilok kezelése és végpontok csoporton belüli, hogy elkezdheti az alkalmazás létrehozása.

Hozzon létre egy mappát az alkalmazás tárolásához.  Az aktuális elérési utat a Node.js eszközeivel konzolról adja meg az aktuális tartózkodási helyén ebbe a mappába, és a projekt inicializálni hajtja végre:
    
    npm init
    
Majd formában jelennek meg a projekt inicializálni kérdések sorozata.  , **Majd a bejegyzés pontra**ebben az oktatóanyagban *app.js*használja.  Megjelenik a következő példában a többi beállítást.

![NPM init kimeneti](./media/cdn-app-dev-node/cdn-npm-init.png)

A project most inicializálni, *packages.json* -fájl segítségével.  A projekt fog néhány Azure tárakkal NPM csomagokban található.  Node.js (azure-arm-cd) használjuk az az Azure-ügyfél futtatókörnyezetének az Node.js (ms-többi-azure) és a Azure CDN-ügyfél tárat.  Helyezzen el azokat a projekthez, függőségek.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Miután végzett a csomagok telepítésével, a *package.json* fájl hasonlóan kell kinéznie ebben a példában (számok eltérhetnek verzió):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Végül a szövegszerkesztővel, hozzon létre egy üres szövegfájlt, és mentse a projekt mappában *app.js*legfelső szintű.  Hogy most már készen áll a kezdje el írni a kódot.

## <a name="requires-constants-authentication-and-structure"></a>Állandók, a hitelesítési és a struktúra igényel

A- *app.js* a szerkesztő megnyitása pedig hozzuk működésbe a program írt alapszerkezetét.

1. Adja meg a "megköveteli a" saját NPM csomagok tetején a következő:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Néhány a módszerhez állandók definiálása szükség.  Adja meg a következőket.  A helyőrzők, beleértve a helyére ne felejtse el a ** &lt;csúcsos zárójelek&gt;**, szükség szerint a saját értékű.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Ezután hogy miként hozható létre az a CDN-kezelés ügyfél, és tegyen saját hitelesítő adatok.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Egyes felhasználói hitelesítés használja, ha két sort némileg eltérő fog kinézni.

    >[AZURE.IMPORTANT] Ha használja a kódot a következő példában, hogy az egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.  Ügyeljen arra, az egyes felhasználói hitelesítő adatokkal és titkos tarthatja őket.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Az elemek helyére ne felejtse el ** &lt;csúcsos zárójelek&gt; ** a megfelelő adatokat.  A `<redirect URI>`, használja az átirányítást az alkalmazás az Azure Active Directory regisztrálásakor megadott URI.
    

4.  A New Node.js parancssori paraméterek lesz.  Vegyük ellenőrzése, hogy legalább egy paramétert.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Amely életre velünk a program, ahol azt elágaztatásnak ki az egyéb funkciók, milyen paramétereket voltak átadott alapján fő része.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  A program a különböző helyeken azt kell ellenőrizze, hogy a megfelelő számú paraméter átadott volt, és néhány Súgó megjelenítése, ha helyesnek nem látszik.  Hozzunk létre, amely függvényt.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Végül az a CDN-kezelés ügyfél fogjuk használni függvényekről is aszinkron, így ha fel szeretne hívni, vissza, ha az azok végzett módszer van szükségük.  Vegyük végezze, amely megjeleníthető a kimenet az ügyféltől CDN management (ha van ilyen), valamint biztonságosan Kilépés a programból.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Most, hogy a program egyszerű felépítésének íródott, akkor a függvény a paraméterek neve alapján létre kell hoznia.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN profilok és a végpontok

Kezdjük a meglévő profilok és a végpontok felsoroló kódot.  A kód megjegyzések a várt szintaxis biztosít, így a tudjuk, ahol mindegyik paraméter kerül.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profilok és végpont létrehozása

Ezután azt fogja írni a függvények profilok és végpont létrehozásához.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Zárólap törlése

Feltételezve, hogy létrejött a végpontot, egy közös feladatot, célszerű lehet végrehajtani a program a végpontot tartalmának van törlése.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profilok és a végpontok törlése

Az utolsó függvénnyel is törli a végpontok és profilok.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>A program futtatása

Azt is most hajtsa végre a Node.js programjára a kedvenc debugger vagy a konzolon.

> [AZURE.TIP] Ha a Visual Studio-kódot az hibakereső használata esetén kell a parancssori paraméterek átadni a környezet beállítása.  Visual Studio Code végzi a **lanuch.json** fájlban.  Keresse meg a **argumentumok** nevű tulajdonság, és adja hozzá a paramétereinek karakterlánc értékek tömbjét, úgy, hogy az alábbihoz hasonló: `"args": ["list", "profiles"]`.

Első lépésként a profilok bejegyzésére.

![Lista profilok](./media/cdn-app-dev-node/cdn-list-profiles.png)

Azt elmehetett üres tömböt.  Profil a saját erőforráscsoport nincs azt, mivel a várt módon, amely.  Most vegyük hozzon létre egy profilt.

![Profil létrehozása](./media/cdn-app-dev-node/cdn-create-profile.png)

Most helyezzen el egy végpontot.

![Végpont létrehozása](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Végezetül nézzük meg profilt törölni.

![Profil törlése](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Következő lépések

Ez a forgatókönyv, [Töltse le a minta](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)a befejezett projekt megjelenítése.

A hivatkozás esetében az Azure CDN SDK Node.js az megnézheti a [hivatkozást](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

További dokumentációjában találhat az Azure SDK Node.js, tekintse meg a [teljes hivatkozást](http://azure.github.io/azure-sdk-for-node/).

Kezelése a [PowerShell](./cdn-manage-powershell.md)CDN erőforrásait.

