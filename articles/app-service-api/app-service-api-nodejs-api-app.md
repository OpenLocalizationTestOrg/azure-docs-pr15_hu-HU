<properties
    pageTitle="Azure App szolgáltatásban node.js API-alkalmazás |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre Node.js RESTful API, és az API-alkalmazásokban Azure App szolgáltatásban kell üzembe."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="rachelap"/>

# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Node.js RESTful API összeállítása és beállítaná őket az API-alkalmazásokban Azure-ban

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Ebből az oktatóanyagból megtudhatja, hogy miként hozzon létre egy egyszerű [Node.js](http://nodejs.org) API-t, és [mely számjegy](http://git-scm.com)használatával egy [API alkalmazást](app-service-api-apps-why-best-platform.md) az [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md) üzembe. Olyan operációs rendszerekhez, futtatását is lehetővé teszi, hogy Node.js is használhatja, és fogja végezze el az összes munkáját parancssori eszközökkel, például cmd.exe vagy bash.

## <a name="prerequisites"></a>Előfeltételek

1. Microsoft Azure-fiók ([ingyenes fiók megnyitása](https://azure.microsoft.com/pricing/free-trial/))
1. [Node.js](http://nodejs.org) telepítette (Ez a példa feltételezi, hogy rendelkezik-e Node.js 4.2.2 verzió)
2. [Mely számjegy](https://git-scm.com/) telepítve
1. [GitHub](https://github.com/) fiók

Alkalmazás szolgáltatás lehetővé teszi, hogy hányféleképpen API-alkalmazás telepítéshez használni a kód, miközben a ebben az oktatóanyagban jeleníti meg a mely számjegy módszer, és feltételezi, hogy miként dolgozhat az mely számjegy alapismeretei. Egyéb telepítési módjairól további tudnivalókért lásd [Deploy Azure alkalmazás szolgáltatás az alkalmazást](../app-service-web/web-sites-deploy.md).

## <a name="get-the-sample-code"></a>A minta kódszámú

1. Nyissa meg a parancssor, amelyek Node.js és mely számjegy parancsai futtathatók.

1. Nyissa meg a helyi mely számjegy tárházba, és a [minta kódját tartalmazó GitHub tárházba](https://github.com/Azure-Samples/app-service-api-node-contact-list)adatfeliratsor használható mappára.

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    A minta API biztosít a végpontokat: Get kérés `/contacts` neveket és e-mail címeket listáját adja eredményül JSON formátumban, miközben `/contacts/{id}` csak a kijelölt partner adja eredményül.

## <a name="scaffold-auto-generate-nodejs-code-based-on-swagger-metadata"></a>Scaffold (automatikus létrehozása) Node.js kód Swagger metaadatok alapján

Metaadat-alapú RESTful API leíró fájlformátum [swagger](http://swagger.io/) . Azure alkalmazás szolgáltatás [beépített támogatása Swagger metaadatokat](app-service-api-metadata.md)tartalmaz. Ez a szakasz az oktatóprogram modellek egy API fejlesztési munkafolyamatot, amelyben Swagger metaadatok először létre, és annak scaffold használatával (automatikus létrehozása) az API kiszolgáló kódját. 

>[AZURE.NOTE] Ez a szakasz kihagyhatja, ha nem szeretné megtudhatja, hogy miként scaffold Node.js kód Swagger metaadat-fájlból. Ha azt szeretné, csak példakódot új API-alkalmazás telepítéshez használni, ugorjon közvetlenül [az Azure-ban API-alkalmazás létrehozása](#createapiapp) című.

### <a name="install-and-execute-swaggerize"></a>Telepítse és Swaggerize végrehajtása

1. Hajtsa végre a következő parancsok végrehajtása az **yo** és **nyilvántartás-készítő alkalmazás swaggerize** NPM modulok globálisan telepítéséhez.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize eszköz, amely a kiszolgáló hozzárendel egy API leírt Swagger metaadat-fájlt. Szeretné használni a Swagger fájl neve *api.json* , és a tárat, klónozva *indítása* mappában található.

2. Keresse meg a *Kezdés* mappát, és ezután hajtsa végre a `yo swaggerize` parancsot. Fog swaggerize kérje meg a kérdések sorozata.  **Mi a hívás ehhez a projekthez**írja be a "ContactList", az **elérési útját a dokumentum swagger**, adja meg a "api.json" és **Express Boldog, vagy Restify**, írja be a "express".

        yo swaggerize

    ![Parancssori swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Megjegyzés**: Ha ezt a lépést a hibát, a következő lépésre megtudhatja, hogyan megoldással.

    Swaggerize alkalmazás mappát, scaffolds kezelők és konfigurációs fájlokat hoz létre, és létrehoz egy **package.json** fájlt. A sürgős nézet motor létrehozásához a Swagger Súgó lap használatos.  

3. Ha a `swaggerize` parancs nem sikerült a "nem várt token" vagy "érvénytelen escape-karakterlánc" hibaüzenet, a probléma okát, a hiba javítása a létrehozott *package.json* fájl szerkesztésével. Az a `regenerate` sor alatt `scripts`, módosítsa a fordított perjel *api.json* perjellel, hogy a program úgy, hogy a sor néz ki a következő példa:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Nyissa meg azt a mappát, amely tartalmazza a scaffolded kódját (ebben az esetben az */start/ContactList* almappát).

1. Futtatása `npm install`.
    
        npm install
        
2. A **jsonpath** NPM modul telepítése. 

        npm install --save jsonpath
        
    ![Jsonpath telepítése](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. A **swaggerize felhasználói felület** NPM modul telepítése. 

        npm install --save swaggerize-ui
        
    ![Felhasználói felület telepítés swaggerize](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### <a name="customize-the-scaffolded-code"></a>A scaffolded kódot testreszabása

1. Másolja a vágólapra a **tár** mappában **Indítsa el** a mappából az scaffolder által létrehozott **ContactList** mappába. 

1. A kódot a **handlers/contacts.js** fájlban cserélje le a következő kódot. 

    Ez a kód **lib/contactRepository.js**által szolgáltatott **lib/contacts.json** -fájlban tárolt adatok JSON használja. Az új contacts.js kódot válaszol a HTTP-kérések felvenni az összes névjegy és JSON hasznos adja vissza őket. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. A kódot a **handlers/contacts/{id}.js** fájlban cserélje le a fofllowing kódot. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. A kód **server.js** cserélje le a következő kódot. 

    A server.js fájl módosításainak kiemelt naptárikonnal megjegyzések használatával, így láthatja, hogy a változtatások. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### <a name="test-with-the-api-running-locally"></a>A helyben fut API-val tesztelése

1. Aktiválja a kiszolgálón a Node.js parancssori végrehajtható fájl használatával. 

        node server.js

1. **Http://localhost:8000/partnerek**választékát át, amikor megjelenik a névjegylistában JSON kimenetét (vagy kéri töltse le, attól függően, hogy a böngésző). 

    ![Az összes partnerek Api-hívást.](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Böngészés közben ****http://localhost:8000/partnerek/2, látni fogja a névjegyet, hogy azonosító értékét jelöli.

    ![Adott partner Api-hívást](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. A Swagger JSON adatok van felszolgált **a/swagger** végpont keresztül:

    ![Névjegy-Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. A Swagger felhasználói felületének van felszolgált keresztül az **/docs** végpontot. Swagger felhasználói felület a sokoldalú ügyfél HTML-szolgáltatásokhoz, az API teszteléséhez is használhatja.

    ![Felhasználói felület swagger](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a>Új API-alkalmazás létrehozása

Ebben a részben, az Azure portal segítségével egy új API-alkalmazás létrehozása az Azure-ban. Az API-alkalmazás jelöli a számítási erőforrások Azure ad a kód futtatásához. Későbbi szakaszában a kód rendszerbe fogja az új API-alkalmazást.

1. Tallózással keresse meg az [Azure-portálon](https://portal.azure.com/). 

1. Kattintson a **Új > webes + mobil > API-alkalmazás**. 

    ![A portál új API-alkalmazás](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Írja be az **alkalmazás neve** egyedi *azurewebsites.net* tartományban, például NodejsAPIApp és a számot, hogy egyedi legyen. 

    Ha például a név `NodejsAPIApp`, az URL-címe lesz `nodejsapiapp.azurewebsites.net`.

    Ha beírta a nevét, hogy valaki más már használatban van, jobbra piros felkiáltójel látható.

6. Az **Erőforráscsoport** legördülő kattintson az **Új**gombra, és **Új erőforrásnevet csoportban** adja meg "NodejsAPIAppGroup" vagy más néven Ha inkább. 

    [Erőforráscsoport](../azure-resource-manager/resource-group-overview.md) Azure erőforrások, például a API alkalmazások, adatbázisok és VMs gyűjteménye. Ebben az oktatóanyagban célszerű hozzon létre új erőforráscsoport, mivel, amely megkönnyíti az Azure erőforrások az oktatóprogram létrehozott egy lépésben törlése.

4. Kattintson az **Alkalmazás terv/SRV**, és kattintson az **Új létrehozása**.

    ![Alkalmazás szolgáltatás terv létrehozása](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    Az alábbi lépésekkel hozza létre az új erőforráscsoport-alkalmazás szolgáltatás megtervezése. Az alkalmazás szolgáltatáscsomagja adja meg a számítási erőforrások az API-alkalmazást futtató. Például ha úgy dönt, hogy a szabad réteg, az API-alkalmazás futtat a megosztott VMs futása az egyes fizetett rétegek a dedikált VMs. App milyen szolgáltatáscsomagok információkért [alkalmazás szolgáltatás csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)című témakörben találhat.

5. Adja meg az **Alkalmazás szolgáltatás megtervezése** lap "NodejsAPIAppPlan" vagy más néven Ha inkább.

5. A **hely** legördülő listában válassza a helyre, ahol a legközelebb.

    Ezzel a beállítással megadhatja, melyik Azure adatközponthoz az alkalmazás fog futni. Ebben az oktatóanyagban bármely régió kijelölhet és nem létrehozni, akkor a észrevehető különbség. De egy gyártási alkalmazás a kiszolgáló kell a lehető legközelebb férnek hozzá, [a késleltetés](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)csökkentése érdekében, hogy az ügyfelek számára.

5. Kattintson a **árak réteg > az összes megjelenítése > F1 ingyenes**.

    Az ebben az oktatóanyagban a szabad árak réteg nyújt elegendő teljesítményét.

    ![Válassza a réteg árak](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. Az **Alkalmazás szolgáltatás megtervezése** lap kattintson az **OK gombra**.

7. A lap az **API-alkalmazást** kattintson a **Létrehozás**gombra.

## <a name="set-up-your-new-api-app-for-git-deployment"></a>Állítsa be az új API alkalmazást a telepítés mely számjegy

Azure App szolgáltatásban mely számjegy-tárházba véglegesítése közvetítheti a kód fogja üzembe az API-alkalmazásba. Az oktatóprogram ebben a részben hoz létre a hitelesítő adatokat, és mely számjegy tárházba Azure, amely a telepítéshez használni.  

1. Kattintson az API-alkalmazás létrehozása után **alkalmazás szolgáltatások > {az API-alkalmazás}** portál kezdőlapján. 

    A portál **API-alkalmazás** és a **Beállítások** rögzítéséhez jeleníti meg.

    ![Portál API-alkalmazás és a beállítások lap](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. A **Beállítások** lap görgessen le a **közzététele** csoportban, és válassza a **telepítés hitelesítő adatait**.
 
3. **Telepítési hitelesítő adatainak beállítása** lap az adja meg a felhasználónevét és jelszavát, és kattintson a **Mentés**gombra.

    A Node.js kód közzétételi az API-alkalmazásba kell megadnia a hitelesítő adatokat. 

    ![Telepítési hitelesítő adatok](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. Kattintson a **Beállítások** lap **telepítési forrás > forrás válassza a > helyi mely számjegy tárházba**, majd kattintson az **OK gombra**.

    ![Mely számjegy repó létrehozása](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Miután a mely számjegy tárházba elkészült a lap módosításokat mutatja, hogy az aktív telepítések. Mivel a tár új, ha nem aktív telepítések listában. 

    ![Nem aktív telepítések](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Mely számjegy tárházba URL másolása másokkal. Ehhez az új API-alkalmazás keresse meg a lap, és tekintse meg a lap **Alapverzió** szakaszát. Figyelje meg, **mely számjegy adatfeliratsor URL-cím** **Essentials** szakaszában. Ez az URL mutat, jelenik egy ikonra mutat, amely az URL-címet másolja a vágólapra a jobb oldalon. Kattintson erre az ikonra kattintva másolja a vágólapra az URL-címet.

    ![Mely számjegy URL-címe kinyerése a portálon](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Megjegyzés**: az URL-címet a következő szakaszban ügyeljen arra, hogy mentse valahol a pillanatra, hogy mely számjegy adatfeliratsor szüksége lesz.

Most, hogy az API-alkalmazásokban a biztonsági másolat készítése mely számjegy összegyűjti, történő tárházba az API-alkalmazás telepítéshez használni a kód segítségével tudja telepíteni a kódot. 

## <a name="deploy-your-api-code-to-azure"></a>Azure a API-kód telepítése

Ebben a részben egy helyi mely számjegy tárházba, amely tartalmazza a kiszolgáló kódot az API-hoz létre, és ezután, leküldéses a kód az adott tárházba, amely a korábban létrehozott Azure-ban tárházba.

1. Másolás a `ContactList` mappa olyan helyre, ahol egy új helyi mely számjegy tárházba is használhatja. Ha Ön nem vette fel a oktatóanyag első része, másolja a vágólapra `ContactList` a a `start` mappát. egyéb esetben másolása `ContactList` a a `end` mappát.

1. A parancssor eszköz keresse meg az új mappát, majd hajtsa végre a következő hozzon létre egy új helyi mely számjegy tárházba parancsot. 

        git init

     ![Új helyi mely számjegy repó](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Hajtsa végre a következő parancsot a távoli az API-alkalmazás tárhoz mely számjegy hozzáadása. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Megjegyzés**: a "YOUR_GIT_CLONE_URL_HERE" karakterláncot cserélje ki a saját mely számjegy adatfeliratsor URL-címet, előbb másolt. 

1. A jóváhagyás, amely tartalmaz minden ezzel a kód létrehozása a következő parancsok végrehajtása. 

        git add .
        git commit -m "initial revision"

    ![Mely számjegy jóváhagyás kimeneti](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. A kód nyomni az Azure parancsának végrehajtása. Amikor a rendszer kéri a jelszót, adja meg a egy Ön által létrehozott korábbi az Azure-portálon.

        git push azure master

    Ez elindítja az API-alkalmazást a telepítés.  

1. A böngészőben lépjen vissza az API-alkalmazás a **telepítés** lap, és láthatja, hogy mi a telepítés. 

    ![Telepítési történik](media/app-service-api-nodejs-api-app/deployment-happening.png)

    A parancssor ablakokban, a telepítés állapotának tükrözi, közben. 

    ![Csomópont Js telepítési probléma](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    A telepítésének befejeződése után a **telepítés** lap tükrözi, a kód változások az API-alkalmazás sikeres telepítését. 

## <a name="test-with-the-api-running-in-azure"></a>Tesztelje a Azure-ban futó API-val
 
3. Másolja a vágólapra az **URL-CÍMÉT** az API-alkalmazás lap **Essentials** szakaszában. 

    ![A telepítés befejeződött](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. REST API ügyfélprogram Postman vagy Fiddler (vagy a webböngészőben), például adja meg a partnereket az URL-cím API-hívást, amely a `/contacts` végpontját az API-alkalmazást. Az URL-címe lesz.`https://{your API app name}.azurewebsites.net/contacts`

    GET kérésekben a végpontra küldésekor az API-alkalmazás JSON kimenetét kap.

    ![Szerezze meg Api postman](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. A böngészőben nyissa meg a `/docs` végpontot, próbálja ki az Swagger a felhasználói felület, az Azure futása közben.

Most, hogy be vezetékes folyamatos kézbesítési, kód módosításokat, és egyszerűen közvetítheti véglegesítése az Azure mely számjegy tárházba Azure telepítse őket.

## <a name="next-steps"></a>Következő lépések

Ezen a ponton sikeresen elkészítette az API-alkalmazásokban és rendszerbe állított Node.js API-kódot. A következő oktatóanyag mutatja, hogy hogyan [API-alkalmazások CORS használatáról JavaScript-ügyfelek](app-service-api-cors-consume-javascript.md)használhatnak.
