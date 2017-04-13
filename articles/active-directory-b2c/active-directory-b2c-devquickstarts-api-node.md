<properties
    pageTitle="Azure Active Directory B2C: A webes API biztonságos Node.js használatával |} Microsoft Azure"
    description="Hogyan hozhat létre Node.js webes API-val, hogy elfogadja a token származhat B2C bérlői webhelyre"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure Active Directory B2C: A webes API biztonságos Node.js használatával

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Az Azure Active Directory (Azure Active Directory) B2C biztonságossá tétele a webes API OAuth 2.0-s hozzáférési jogkivonat alkalmazásával. Tokenek lehetővé teszi, hogy az ügyfél-alkalmazások által használt Azure Active Directory B2C az API-nak hitelesítést végezni. Ez a cikk bemutatja, hogyan hozhat létre, amely lehetővé teszi, hogy a felhasználók hozzáadása, és a feladatok lista a "Feladatlista" API. A webes API-val védett Azure Active Directory B2C használ, és csak lehetővé teszi a felhasználóknak, hogy a feladatlista egyszerűbben kezelhető hitelesített.

> [AZURE.NOTE]  Ez a példa készült kapcsolódik az [iOS B2C minta alkalmazás](active-directory-b2c-devquickstarts-ios.md)használatával. Az aktuális segédlet először elvégzendő, és kövesse a minta együtt.

**A Passport** hitelesítési köztes Node.js számára is. Rugalmas és elemes, Passport unobtrusively telepített bármelyik Express-alapú vagy webalkalmazás Restify. Stratégiák a teljes készletével hitelesítési támogatja a felhasználónév és jelszó, Facebook, Twitteren és további használatával. Azure Active Directory (Azure Active Directory) stratégia kialakítása azt. Ez a modul telepítése, és vegye fel az Azure Active Directory `passport-azure-ad` beépülő modult.

Ez a példa elvégzendő kell:

1. Az alkalmazások regisztrálása Azure AD.
2. Állítsa be az alkalmazás használatához útlevelét `azure-ad-passport` beépülő modult.
3. Állítsa be, hívja fel a "Feladatlista" webes API ügyfélalkalmazás.


## <a name="get-an-azure-ad-b2c-directory"></a>Ismerkedés az Azure Active Directory B2C címtár

Azure Active Directory B2C használata előtt hozzon létre egy könyvtárat, vagy az bérlői.  A könyvtár (az összes felhasználó, alkalmazások, csoportok és az egyéb tároló).  Ha nincs egy már, [B2C könyvtár létrehozása](active-directory-b2c-get-started.md) előtt a Folytatás gombra.

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ezután kell a B2C könyvtárában, amellyel, hogy szüksége van az alkalmazás biztonságos kommunikáció információk az Azure Active Directory-alkalmazás létrehozása. Ebben az esetben az ügyfél alkalmazás és a webes API jeleníti meg egyetlen **Azonosítója**, mert egy logikai alkalmazás tartalmazzák. Az alkalmazás létrehozásához kövesse az [alábbi utasításokat](active-directory-b2c-app-registration.md). Győződjön meg arról, hogy:

- Tartalmazza a **web app/webes api** alkalmazásban
- Adja meg `http://localhost/TodoListService` **Válasz URL**-címként. Célszerű a kódot a következő példában az alapértelmezett URL-CÍMÉT.
- Hozzon létre egy **alkalmazás titkos** az alkalmazás, és másolja a vágólapra. Az adatok később szükség lenne. Figyelje meg, hogy ezt az értéket kell lennie, [XML-escape](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használat előtt.
- Másolja az alkalmazás rendelt **Azonosítója** . Az adatok később szükség lenne.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>A házirendek létrehozása

Azure Active Directory B2C minden felhasználói felület [házirend](active-directory-b2c-reference-policies.md)van megadva. Az alkalmazás tartalmaz két identitás változat: jelentkezzen, és jelentkezzen be. Kell minden típusú több házirend létrehozása a [házirend összefoglaló cikkben](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)leírt módon.  Amikor a három házirendek hoz létre, ne felejtse el:

- A **megjelenítendő név** és egyéb előfizetési attribútumai válassza az előfizetés házirend.
- Válassza a **megjelenítendő név** és a **Objektumazonosító** alkalmazás jogcímalapú minden házirend.  Megadhatja, hogy más igények is.
- Másolja le a **nevet** az egyes házirendek létrehozásuk után. Az előtag rendelkeznie kell `b2c_1_`.  Házirend neveket később szükség lenne.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a három házirendek, készen áll az alkalmazás összeállítása.

Hogyan működnek a házirendek az Azure Active Directory B2C kapcsolatos további tudnivalókért kezdje az [oktatóprogram .NET web app az első lépések](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Töltse le a kódot.

Ezen oktatóprogram [a GitHub karbantartása](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS)kódját. A minta készítéséhez menet közben is [.zip fájlként skeleton projekt letöltése](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Átmásolhatja a skeleton is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

A kész alkalmazás [.zip fájlként](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) is érhető el vagy a `complete` ugyanazt a tárházba részlege.

## <a name="download-nodejs-for-your-platform"></a>Töltse le a Node.js a Platform

Ez a példa sikeresen használatához szükséges Node.js működő telepítéssel. 

Telepítse Node.js [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>A platform MongoDB telepítése

Ez a példa sikeresen használatához szükséges MongoDB működő telepítéssel. MongoDB használjuk, hogy a REST API állandó server-példányok között.

Telepítse MongoDB [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] A segédlet feltételezi, hogy az alapértelmezett telepítési és kiszolgáló végpontok-et MongoDB, amely az írás időpontjában `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Telepítse a Restify modulokat a webes API

Restify segítségével összeállítása a REST API-t. Restify minimális és rugalmas Node.js alkalmazás keretrendszer származik Express. Csatlakozás fölött REST API-khoz készítéséhez szolgáltatások robusztus halmazának van.

### <a name="install-restify"></a>Telepítés Restify

A parancssorból módosítása a könyvtár `azuread`. Ha a `azuread` directory nem létezik, hozza létre.

`cd azuread`vagy`mkdir azuread;`

Írja be a következő parancsot:

`npm install restify`

Ez a parancs Restify telepíti.

#### <a name="did-you-get-an-error"></a>Hibaüzenet jelent meg egy?

Az egyes operációs rendszerek, ha `npm`, a hiba jelenhet meg `Error: EPERM, chmod '/usr/local/bin/..'` és egy kérelmet, hogy a fiók futtatása rendszergazdaként. Ha a probléma jelentkezett, használja a `sudo` parancs futtatása `npm` a magasabb szintű jogosultsággal.

#### <a name="did-you-get-a-dtrace-error"></a>Hibaüzenet jelent meg egy DTrace?

Ez a szöveg hasonló megjelenhet Restify telepítésekor:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify biztosít a hatékony kisalkalmazások többi nyomkövetés meghívja DTrace használatával. Számos operációs rendszer azonban nem rendelkezik a DTrace érhető el. A hibák nyugodtan figyelmen kívül hagyható.

A parancs jelenjen meg a szöveg hasonló:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>A webes API Passport telepítése

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott.

Telepítse a Passport a következő parancsot:

`npm install passport`

A parancs a következőképpen fog kinézni a szöveg:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>A webes API passport-azuread hozzáadása

Ezután adja hozzá az OAuth stratégia használatával `passport-azuread`, a stratégiák csatlakozó Passport Azure AD-öt. Ez a beállítás használata bearer tokenek a REST API-minta.

> [AZURE.NOTE] Bár az oauth2 hitelesítési mód, amelyben minden olyan ismert jogkivonat állítható ki keretet biztosít, csak bizonyos jogkivonat típusok széles körű használata szerzett. A végpontok védelmet tokenek bearer tokenek. Az ilyen típusú tokenek a legtöbb körben oauth2 hitelesítési mód az elküldött. Sok megvalósítás feltételezik, hogy bearer tokenek kiadott jogkivonat csak típusát.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott.

Telepítse az esetben a `passport-azure-ad` modul használja a következő parancsot:

`npm install passport-azure-ad`

A parancs a következőképpen fog kinézni a szöveg:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>A webes API MongoDB modulok hozzáadása

Ez a példa a adattár MongoDB használja. Az, hogy telepíteni egy elterjedt mongúz modellek és sémák kezelésére szolgáló beépülő modult.

* `npm install mongoose`

## <a name="install-additional-modules"></a>További modul telepítése

Ezután telepítse a többi szükséges modulokat.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Telepítse a modulokat a `node_modules` címtár:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Hozzon létre egy server.js fájlt a függőségek

A `server.js` fájlt a legtöbb funkció biztosít a webes API-kiszolgálóhoz. 

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Hozzon létre egy `server.js` fájl-szerkesztőben. Adja meg a következő adatokat:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Mentse a fájlt. Állítsa vissza, később.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Hozzon létre egy config.js fájlt tárolja az Azure Active Directory-beállítások

Ez a kód fájl konfigurációs paraméterek továbbítja az Azure Active Directory portálja a `Passport.js` fájlt. A webes API hozzáadva az első része a segédlet portált létrehozott ezeket a konfigurációs értékeket. Mi az értékeket a következő paraméterek után, másolja a kódot elhelyezni azt ismertetik.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Hozzon létre egy `config.js` fájl-szerkesztőben. Adja meg a következő adatokat:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Szükséges értékek

`clientID`: A webes API-alkalmazás a ügyfél azonosítója.

`IdentityMetadata`: Ez az, hogy hol `passport-azure-ad` a konfigurációs adatok keres az identitásszolgáltató. A JSON webes tokenek érvényesítése billentyűk is keresi. 

`audience`: Az egységes erőforrás-azonosító (URI) a portálról, amely azonosítja a hívó alkalmazás. 

`tenantName`: A bérlői nevét (például **contoso.onmicrosoft.com**).

`policyName`: A házirendet, amely a tokenek, a kiszolgáló érkező érvényesíteni szeretné. Ezzel a házirenddel az ügyfélalkalmazás a az használt bejelentkezési ugyanaz a házirend kell lennie.

> [AZURE.NOTE] A B2C előzetes házirendekkel az azonos ügyfél- és a telepítő keresztül. Ha már egy segédlet befejeződött, és ezek a házirendek létrehozott, akkor nem kell újra a műveletet. Mivel a segédlet végeztével kerülni a Webhelyfiókok kell ügyfél útmutatók az új házirendek beállítása a webhelyen.

## <a name="add-configuration-to-your-serverjs-file"></a>Konfigurációs hozzáadása a server.js fájlhoz

Értékeit olvasható a `config.js` létrehozott fájl, és adja meg a `.config` szükséges erőforrásként az alkalmazásban a fájl, és a globális változók állítsa a a `config.js` dokumentumot.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg a `server.js` fájl-szerkesztőben. Adja meg a következő adatokat:

```Javascript
var config = require('./config');
```
Az új szakasz hozzáadása `server.js` , amely tartalmazza a következő kódot:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Ezután a felhasználókhoz azt a hívó alkalmazások fogadjanak néhány helyőrzők hozzáadása.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Lépjen tovább, és hozza létre a saját naplózó is.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Adja meg a MongoDB modell és -sémafájlok adatokat mongúz használatával

A korábbi előkészítése fizet, ezek a fájlok együttes REST API szolgáltatásban hozása.

Ez a segédlet az használva MongoDB a feladatok, korábbiakban tárgyalt.

Az a `config.js` az adatbázis **tasklist**nevű fájlt. A név lett is helyezi a végén a `mongoose_auth_local` kapcsolat URL-címe. Nem kell hozzá előzetesen létrehozása az adatbázis MongoDB. Azt hoz létre az adatbázis meg a kiszolgáló alkalmazás első futtatása.

Megadhatja, hogy a kiszolgáló mely MongoDB adatbázist, miután kell néhány további kódírás hozhat létre a modell és -sémafájlok a kiszolgáló tevékenységekhez.

### <a name="expand-the-model"></a>Bontsa ki a modell

Ez a séma modell található egyszerű. Szükség szerint bővítheti.

`owner`: A tevékenységhez ki van rendelve. Az objektum egy **karakterlánc**.  

`Text`: A tevékenység magát. Az objektum egy **karakterlánc**.

`date`: A tevékenység esedékes a dátumot. Az objektum a **dátum és idő**.

`completed`Ha a tevékenység befejeződött. Az objektum olyan **logikai**.

### <a name="create-the-schema-in-the-code"></a>A séma a kód létrehozása

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg a `server.js` fájl-szerkesztőben. Adja hozzá a konfigurációs bejegyzés alatt a következő adatokat:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
A séma először hozzon létre, és kattintson a kód egész az adatok tárolására, a **útvonalak**megadásakor használt modell objektumot hozunk létre.

## <a name="add-routes-for-your-rest-api-task-server"></a>A REST API-tevékenység Server útvonalak hozzáadása

Most, hogy van egy adatbázismodell végezhető, adja hozzá a leveleket a REST API-kiszolgálót használ.

### <a name="about-routes-in-restify"></a>Tudnivalók a Restify útvonalak

Útvonalak munka Restify módon működnek-e a Express Papírhalom használata. A várt hívja fel a ügyfélalkalmazásokban URI használatával meghatározhatja útvonalak. 

Egy tipikus minta Restify útvonal a következő:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify és Express biztosíthat sok mélyebb szolgáltatások, például alkalmazás típusok meghatározása és módon összetett útválasztás különböző végpontokon keresztül. Ebben az oktatóanyagban alkalmazásában akkor tartsa szem előtt ezek az útvonalak egyszerű.

#### <a name="add-default-routes-to-your-server"></a>A kiszolgáló alapértelmezett útvonalak hozzáadása

Most már hozzáadhat **létrehozása** és a **lista** egyszerű CRUD útvonalak a REST API. Más útvonalak megtalálható a `complete` a minta ág.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg a `server.js` fájl-szerkesztőben. Az alábbi a fent elvégzett adatbáziselemek adja meg a következő adatokat:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Hiba történt a útvonalak kezelése hozzáadása

Adja meg, bizonyos hiba kezelése, hogy a hang nélküli kommunikáció bármilyen problémát tapasztal az ügyfélhez oly módon, hogy azt követően deríthető ki.

Adja hozzá a következő kódot:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>A kiszolgáló létrehozása

Most az adatbázis megadott, és helyezze a útvonalak helyen. Az, hogy végezze el az utolsó lépésként is vegye fel a server-példányt, hogy a hívások kezelése.

Restify és Express adja meg a REST API-kiszolgáló-mély testreszabási, de a legtöbb szokásos telepítés itt használjuk. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Adja hozzá a leveleket a kiszolgálóhoz (nélkül hitelesítés)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Hitelesítés hozzáadása a REST API-kiszolgálóhoz

Most, hogy egy futó REST API-kiszolgálóval rendelkezik, elérhetővé teheti hasznos Azure Active Directory szemben.

A parancssorból módosítása a könyvtár `azuread`, ha még nem szerepel ott:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>A OIDCBearerStrategy, amely szerepel az ADFS-azure-passport használata


> [AZURE.TIP]
API-khoz írásakor meg kell mindig az adatok csatolása valamit egyedi a jogkivonat, hogy a felhasználó nem tud meghamisíthatja a. A kiszolgáló tárolja az elemeket teendő, ha tartalmaz, így alapján **objektumazonosító** annak a felhasználónak a token (más néven token.oid keresztül), amely a "tulajdonos" mezőben. Ez az érték biztosítja, hogy csak az adott felhasználó hozzáférhet a saját tennivaló elemeket. Van nem kapta tulajdonosának"," az API-t egy külső felhasználónak kérhet mások teendők elemeket akkor is, ha a hitelesítése, így.

Ezután használja a bearer stratégia épített `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Annak a stratégiák azonos típusúak Passport használja. Adja meg azt a `function()` , amelynek `token` és `done` paraméterként. A stratégia vissza megtalálható Önnek, miután az összes munkáját befejeződik. Meg kell majd tárolni a felhasználó, és mentse a jogkivonat, hogy nem kell kérdezzen rá.

> [AZURE.IMPORTANT]
A fenti kód megnyitja bárki, aki a kiszolgáló hitelesítést végezni történik. Ez a folyamat autoregistration nevezik. Gyártási Servers nem lehetővé teszik a felhasználó hozzáférést az API nélkül első őket a regisztrációs folyamat folyamatát. Ez a folyamat az általában a mintázatot, amelyek lehetővé teszik Facebook segítségével regisztrálhatja, de majd kérjük, hogy adja meg a kiegészítő információk fogyasztói alkalmazások látható. A program nem lett parancssori programot, ha azt is van kiolvasott az e-mailt a token objektum, amely adja vissza, és töltse ki a további információt a felhasználók majd kéri. Mivel ez a példa, hogy felveheti a memóriában adatbázissá.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>A kiszolgáló alkalmazásnak a futtatására ellenőrizze, hogy elutasítja azt

Használható `curl` megjelenítéséhez, ha a most már oauth2 hitelesítési mód védelem szemben a végpontok. A fejlécek vissza kell lennie ahhoz, hogy mondani, hogy a megfelelő helyen vannak.

Győződjön meg arról, hogy a MongoDB példánya fut:

    $sudo mongodb

Váltson át a könyvtárra, és futtassa a kiszolgáló:

    $ cd azuread
    $ node server.js

Új Terminálszolgáltatások ablakban futtatása`curl`

Próbálja ki egy egyszerű BEJEGYZÉSBEN:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Egy 401 hiba a kívánt választ. Azt jelzi, hogy a Passport réteg partnere kapcsolatba próbál irányítsa át az engedélyezés végpontot.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Ekkor megjelenik egy REST API-szolgáltatás, használja az oauth2 hitelesítési mód

A REST API Restify és OAuth végrehajtotta! Ekkor elegendő kódot, hogy továbbra is a szolgáltatás fejlesztését, és ebben a példában épülnek. Akkor telt amennyire nélkül, az oauth2 hitelesítési mód-kompatibilis ügyfél ezen a kiszolgálón is. A következő lépésben használata a [webes API segítségével B2C az iOS csatlakozás](active-directory-b2c-devquickstarts-ios.md) útmutató – például egy további segédlet.


## <a name="next-steps"></a>Következő lépések

Speciális témaköreit, például: most áthelyezése:

[Csatlakozás egy webes API-B2C iOS használatával](active-directory-b2c-devquickstarts-ios.md)