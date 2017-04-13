<properties
    pageTitle="Azure Active Directory 2.0-s verzió NodeJS webes API |} Microsoft Azure"
    description="Hogyan hozhat létre egy NodeJS webes API fogadja el a tokenek mindkét személyes Microsoft-Account és a munkahelyi vagy iskolai fiók."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="secure-a-web-api-using-nodejs"></a>A webes API segítségével node.js biztonságos

> [AZURE.NOTE]
    Nem minden Azure Active Directory esetek és szolgáltatások 2.0-s verzió végpont által támogatott.  Annak megállapításához, ha a 2.0-s verzió végpont kell használnia, [2.0-s verzió korlátozások](active-directory-v2-limitations.md)olvashat.

Az Azure Active Directory a 2.0-s verzió végpont megvédheti webes API segítségével [OAuth 2.0-s](active-directory-v2-protocols.md#oauth2-authorization-code-flow) hozzáférési jogkivonat, engedélyezése a felhasználók személyes Microsoft-fiók és munkahelyi vagy iskolai fiók az biztonságos hozzáférés a webes API-val.

**A Passport** hitelesítési köztes Node.js számára is. Rendkívül rugalmas és elemes, Passport unobtrusively húzható minden Express alapú vagy webalkalmazás Resitify. Stratégiák a felhasználónevet és jelszót, Facebook, Twitteren és az egyéb használatával hitelesítés támogatása a teljes készletével. Microsoft Azure Active Directory stratégia kialakítása azt. Azt a modul telepítése és vegye fel a Microsoft Azure Active Directory `passport-azure-ad` beépülő modult.

## <a name="download"></a>Letöltés
Ebben az oktatóanyagban kódját [a GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs)karbantartása.  Követésre, [Töltse le az alkalmazás skeleton, mint egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip) is vagy a skeleton klónozhatja:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

A kész alkalmazást is ez az oktatóanyag végén megadva.


## <a name="1-register-an-app"></a>1. a regisztráció alkalmazás
Új alkalmazás létrehozása a [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [lépések részletes leírását](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

- Másolás lefelé **Azonosítója** rendelt az alkalmazást, szüksége van rá hamarosan.
- A **mobil** platform az alkalmazás hozzáadása
- Másolja az **Átirányítás URI** lefelé a portálon. Az alapértelmezett értéket kell használnia `urn:ietf:wg:oauth:2.0:oob`.


## <a name="2-download-nodejs-for-your-platform"></a>2: node.js a platform letöltése
Ez a példa sikeresen használatához Node.js működő telepítéssel kell rendelkeznie.

Telepítse Node.js [http://nodejs.org](http://nodejs.org).

## <a name="3-install-mongodb-on-to-your-platform"></a>3: telepítés MongoDB megjelenítheti a platform

Ez a példa sikeresen használatához MongoDB működő telepítéssel kell rendelkeznie. MongoDB, hogy a REST API-persistant server-példányok között fogjuk használni.

Telepítse MongoDB [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Az útmutató feltételezi, hogy az alapértelmezett telepítési és kiszolgáló végpontok-et MongoDB, amely az írás időpontjában: mongodb://localhost

## <a name="4-install-the-restify-modules-in-to-your-web-api"></a>4: telepítse a Restify modulokat a webes API-hoz

Azt fogja használni Resitfy össze a REST API-t. Restify a minimális és rugalmas Node.js alkalmazás keretet származik, amelynek szolgáltatások fölött csatlakozás REST API-khoz készítéséhez robusztus halmazának Express.

### <a name="install-restify"></a>Telepítés Restify

A parancssorból váltson könyvtárak azuread. Ha a **azuread** könyvtár nem létezik, hozza létre.

`cd azuread`– vagy –`mkdir azuread;`

Írja be a következő parancsot:

`npm install restify`

Ez a parancs Restify telepíti.

#### <a name="did-you-get-an-error"></a>Hibaüzenet jelent meg egy?

Amikor npm bizonyos operációs rendszeren, előfordulhat, hogy hibaüzenet hiba: EPERM, chmod "/ usr/helyi/bin / …" és futtassa a fiók rendszergazdaként kérést. Ez akkor fordulhat elő, ha a sudo paranccsal a magasabb szintű jogosultsággal npm futtatásához.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Hibaüzenet jelent meg egy DTrace kapcsolatban?

Láthat jelennek meg Restify telepítésekor:

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


Restify lehetővé teszi a hatékony DTrace többi hívások nyomon követéséhez. Számos operációs rendszer azonban nem rendelkezik a DTrace érhető el. A hibák nyugodtan figyelmen kívül hagyható.


Ez a parancs jelenjen meg az alábbihoz hasonló:


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
    └── bunyan@0.22.0(mv@0.0.5)


## <a name="5-install-passportjs-into-your-web-api"></a>5: Passport.js telepíthető át a webes API

[A Passport](http://passportjs.org/) hitelesítési köztes Node.js számára is. Rendkívül rugalmas és elemes, Passport unobtrusively húzható minden Express alapú vagy webalkalmazás Resitify. Stratégiák a felhasználónevet és jelszót, Facebook, Twitteren és az egyéb használatával hitelesítés támogatása a teljes készletével. Azure Active Directory stratégia kialakítása azt. Hogy a modul telepítése, és vegye fel az Azure Active Directory stratégia beépülő modult.

A parancssorból váltson könyvtárak azuread.

Írja be a következő parancsot passport.js telepítése

`npm install passport`

A parancs jelenjen meg az alábbihoz hasonló:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="6-add-passport-azure-ad-to-your-web-api"></a>6: ADFS-Azure-Passport hozzáadása a webes API

Ezután hozzáadunk az OAuth stratégia passport-azuread, a stratégiák csatlakozó Passport Azure Active Directory-öt használ. Azt fogja használni a stratégia Bearer tokenek a Rest API-t a következő példában.

> [AZURE.NOTE] Bár oauth2 hitelesítési mód, amelyben minden olyan ismert jogkivonat állítható ki keretet biztosít, csak bizonyos jogkivonat típusok széles-várható használata szerzett. Végpontok, amely nem kapcsolta ki kell Bearer tokenek védelmet. Bearer tokenek, hogy a legtöbb körben kiállított token az oauth2 hitelesítési mód típusú, és sok megvalósítás feltételezik, hogy bearer tokenek kiadott jogkivonat csak típusát.

A parancssorból a azuread könyvtár könyvtárak módosítása

Írja be a következő parancs Passport.js passport-azure-Active Directory modul telepítése:

`npm install passport-azure-ad`

A parancs jelenjen meg az alábbihoz hasonló:

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

## <a name="7-add-mongodb-modules-to-your-web-api"></a>7: MongoDB modulok hozzáadása a webes API-hoz

Azt fogja használni MongoDB a adattárhoz, ezért, mind a elterjedt telepítéséhez szükséges beépülő modul modellek és sémák kezelésének hívott mongúz, valamint az adatbázis illesztőprogramját MongoDB, más néven MongoDB.


* `npm install mongoose`
* `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: további modul telepítése

Ezután telepítse fogja azt a többi szükséges modulokat.


A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`


Írja be a következő modulokat telepítéséhez node_modules címtárában az alábbi parancsokat:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## <a name="9-create-a-serverjs-with-your-dependencies"></a>9: hozzon létre egy server.js a függőségek

A server.js fájl fog nyújt a használható funkciók körét a legtöbb a webes API-kiszolgálóhoz. Hogy hozzáadja a kódot a legtöbb ehhez a fájlhoz. Termelési célra, a funkciók a kisebb fájlok, például külön útvonalak és vezérlők volna refactor. Ez a bemutató érdekében azt server.js ezt a funkciót fogja használni.

A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

Hozzon létre egy `server.js` a kedvenc szerkesztőben fájlt, és adja meg a következő adatokat:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
```

Mentse a fájlt. Azt fogja visszatérhet hozzá hamarosan.

## <a name="10-create-a-config-file-to-store-your-azure-ad-settings"></a>10: tárolja az Azure Active Directory-beállítások konfigurációs fájl létrehozása

Ez a kód fájl átadja konfigurációs paraméterét az Azure Active Directory portál Passport.js. A webes API hozzáadva az útmutató az első része a portáljának létrehozott konfigurációs ezeket az értékeket. Azt ismertetik mi helyezze az alábbi paramétereknek értékeket, a kód másolása után.


A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

Hozzon létre egy `config.js` a kedvenc szerkesztőben fájlt, és adja meg a következő adatokat:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
issuer: 'https://sts.windows.net/**<your application id>**/',
audience: '<your redirect URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For using Microsoft you should never need to change this.
};

```



### <a name="required-values"></a>Szükséges értékek

*IdentityMetadata*: Ez a hol fog kinézni az ADFS-azure-passport a konfigurációs adatok a IdP, valamint a billentyűket a JWT tokenek érvényesítéséhez. Valószínűleg nem kívánt módosítsa, ha az Azure Active Directory használatával.

*célközönség*: az átirányítási URI a portálon.

> [AZURE.NOTE]
A billentyűparancsok akkor forgassa a rendszeres időközönként. Győződjön meg arról, hogy Ön mindig "openid_keys" URL-ekről vannak adatok és az alkalmazás elérhető-e az internethez.


## <a name="11-add-configuration-to-your-serverjs-file"></a>11: konfigurációs hozzáadása a server.js fájlhoz

Ezeket az értékeket a konfigurációs fájl olvasásakor végig az alkalmazás éppen létrehozott szükséges. Ehhez azt egyszerűen hozzáadhatja a .config fájl szükséges erőforrásként az alkalmazásban, és adja meg a globális változók azoknak a config.js dokumentumban

A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

Nyissa meg a `server.js` a kedvenc szerkesztőben fájlt, és adja meg a következő adatokat:

```Javascript
var config = require('./config');
```
Ezután vegyen fel egy új szakaszt szeretne `server.js` kódot a következő:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
identityMetadata: config.creds.identityMetadata,
issuer: config.creds.issuer,
audience: config.creds.audience
};
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="step-12-add-the-mongodb-model-and-schema-information-using-moongoose"></a>Lépésben 12: A MongoDB az adatmodelleket és Moongoose séma adatainak hozzáadása

Ez a előkészítése most fizetésre, ahogy azt szél ezek a fájlok együttes a REST API szolgáltatáshoz indításához fog.

Ez a forgatókönyv azt fogja használni MongoDB tárolni a saját feladatok ***lépés: 4***ismertetett módon.

A config.js fájlból lépés 11 létrehozott visszahívja, ha azt a neve az adatbázis *tasklist*hasonlóan, hogy milyen azt helyezni mogoose_auth_local csatlakozási URL végén. Nem kell hozzá előzetesen létrehozása az adatbázis MongoDB, hoz létre a nekünk a első futtatásakor a kiszolgáló-alkalmazás (feltételezve, hogy még nem létezik).

Most, hogy azt már említettük a kiszolgáló milyen MongoDB adatbázis azt szeretné használni, a kiszolgáló tevékenységek a modell és -sémafájlok létrehozásához néhány további kódírás szükség.

#### <a name="discussion-of-the-model"></a>A modell vitafórum

A séma modell rendkívül egyszerű, és szükség szerint kibontásához.

NÉV - ki a tevékenységhez rendelt nevét. Egy ***karakterlánc***

TEVÉKENYSÉG - a tevékenység magát. Egy ***karakterlánc***

DÁTUM - a tevékenység adott onnan. Egy ***DATETIME***

KÉSZ – Ha a tevékenység befejeződött-e. ***Logikai***

#### <a name="creating-the-schema-in-the-code"></a>A séma létrehozását a kód


A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

Nyissa meg a `server.js` a kedvenc szerkesztőben fájlt, és adja meg a következő adatokat a konfigurációs bejegyzés alatti:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Ez a MongoDB Serverhez való csatlakozás és kioszthatja vissza egy séma objektum us.

#### <a name="using-the-schema-create-our-model-in-the-code"></a>A séma használata esetén a modell a kód létrehozása

A kód írt fölött, alatt adja meg a következő kódot:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
A kód kiderítheti, mint azt a séma, és hozza létre az a modell objektum fogjuk használni az adatokat a kód egész tárolni, amikor azt meg, hogy a ***leveleket***.

## <a name="step-13-add-our-routes-for-our-task-rest-api-server"></a>Lépés 13: Adja hozzá a leveleket a tevékenység REST API-kiszolgáló

Most, hogy elkészült a végezhető adatbázismodell, helyezzen el a útvonalak fogjuk használni a REST API-kiszolgálóhoz.

### <a name="about-routes-in-restify"></a>Tudnivalók a leveleket a Restify

Útvonalak munka Restify pontos ugyanúgy működnek, a Express protokoll használatával. A várt hívja fel az ügyfélalkalmazások URI használatával útvonalak megadása Általában adhatja meg a útvonalak egy külön fájlba. Mostani célok szempontjából a server.js fájlban adunk a leveleket. Azt javasoljuk, hogy Ön tényező ezeket saját fájl használható.

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


Ez egy, a mintázat legalapvetőbb szintre. Resitfy (és Express) adja meg a sok mélyebb functionaltiy például alkalmazás típusok meghatározása és módon összetett útválasztás különböző végpontokon keresztül. Mostani célok szempontjából azt megtartja a útvonalak nagyon egyszerűen.

#### <a name="add-default-routes-to-our-server"></a>A kiszolgáló alapértelmezett útvonalak hozzáadása

Azt fogja most hozzáadása az egyszerű CRUD útvonalak lekérés, jelentések létrehozása, frissítése és törlése.

A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

Nyissa meg a `server.js` a kedvenc szerkesztőben fájlt, és adja meg a következő adatokat a fent elvégzett adatbáziselemek alatt:

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
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
next(new MissingTaskError());
return;
}
_task.owner = owner;
_task.task = req.params.task;
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
// Delete a task by name
function removeTask(req, res, next) {
Task.remove({
task: req.params.task,
owner: owner
}, function(err) {
if (err) {
req.log.warn(err,
'removeTask: unable to delete %s',
req.params.task);
next(err);
} else {
log.info('Deleted task:', req.params.task);
res.send(204);
next();
}
});
}
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
function getTask(req, res, next) {
log.info('getTask was called for: ', owner);
Task.find({
owner: owner
}, function(err, data) {
if (err) {
req.log.warn(err, 'get: unable to read %s', owner);
next(err);
return;
}
res.json(data);
});
return next();
}
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

### <a name="add-some-error-handling-for-the-routes"></a>Néhány hibakezelés a útvonalak hozzáadása

Célszerű hozzáadása valamilyen hiba kezelése, hogy kommunikálhatnak visszatérhet az ügyfél a problémát észlelt oly módon, azt meg érthető.

Adja meg a fenti megírt kód alatt a következő kódot:

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


## <a name="step-14-create-your-server"></a>14 lépés: A kiszolgálót hozhat létre!

Az adatbázis definiálva van, azt saját útvonalak már a helyükön, és a teendő, utolsó lépésként hozzáadása a kiszolgálói példány, hogy a hívások fogja kezelni.

Restify (és Express) végezheti el a REST API-kiszolgáló-mély testreszabás sok van, de ismét fogjuk használni a legalapvetőbb beállítási mostani célok szempontjából.

```Javascript
/**
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
}));
```
## <a name="15-adding-the-routes-without-authentication-for-now"></a>15: a útvonalak (nélkül hitelesítés most) hozzáadása

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```
## <a name="16-before-we-add-oauth-support-lets-run-the-server"></a>16: OAuth támogatási hozzáadásához nézzük futtassa a kiszolgálóra.

Tesztelje a munkafüzetben a kiszolgáló, mielőtt hozzáadunk hitelesítés

A művelet legegyszerűbben curl használatával a parancssorban. Előtte, egy egyszerű segédprogram, amely lehetővé teszi, hogy us JSON, kimeneti elemzésére szükséges. Ehhez a json eszköz telepítése mivel minden, az alábbi példákat, amely használ.

`$npm install -g jsontool`

Ezzel a JSON eszköz globálisan telepíti. Most, hogy azt, hogy – már elvégezni lejátszása a kiszolgálóval:

Először győződjön meg arról, hogy fut-e a monogoDB isntance.

`$sudo mongod`

Ezután a címtárhoz módosítása, és sütővasak indítása.

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 2.0OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Ezután azt a tevékenységet hozzáadhatja ily módon:

`$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

A kérdésre adott választ kell:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
És azt is a lista feladatok Brandon ily módon:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Az összes működik, ha azt adja hozzá a REST API-kiszolgálóhoz OAuth készen áll.

**MongoDB REST API-kiszolgálóval van!**

## <a name="17-add-authentication-to-our-rest-api-server"></a>17: a REST API-kiszolgálói hitelesítési hozzáadása

Most, hogy elkészült a futó REST API-t (congrats, btw!) pedig hozzuk működésbe, hogy olyan hasznos Azure Active Directory szemben.

A parancssorból módosítása könyvtárak azt a **azuread** mappát, ha még nem létezik:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: a mellékelt ADFS-azure-passport oidcbearerstrategy használata

Az eddigi úgy van alakítottuk ki egy tipikus többi teendő kiszolgáló bármilyen típusú hitelesítés nélkül. Ez a, ahol azt indítása kiépítésekor, amely.

Első lépésként szükség jelzi, hogy szeretne-e Passport használni. A kiszolgáló beállításait után szeretné elhelyezni a jobbra:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
API-khoz írásakor meg kell mindig az adatok csatolása valamit egyedi a jogkivonat, hogy a felhasználó nem tud meghamisíthatja a. Ha a kiszolgáló tárolja az tennivaló elemeket, tárolja őket az előfizetés azonosítója, a felhasználó (más néven token.sub keresztül) jogkivonat, amely azt helyezi el a "tulajdonosa" mező alapján. Ez biztosítja, hogy csak az adott felhasználó hozzáférhet a teendők és más férhet hozzá a megadott teendők. Van nem kapta az API "tulajdonosának" külső felhasználó a teendők kérhet, ha a hitelesítése, így.

Ezután a megnyitott azonosító csatlakozás Bearer stratégia, hogy az az ADFS-azure-passport használata. Csak tekintse meg a kód egyelőre, fog elmagyarázom, hamarosan. Után szeretné elhelyezni a Mit pated fent:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/
var findById = function(id, fn) {
for (var i = 0, len = users.length; i < len; i++) {
var user = users[i];
if (user.sub === id) {
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
log.info('User was added automatically as they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

Passport hasonló mintát használ, az összes stratégiák (Twitter, Facebook stb.), amely az összes stratégia szövegírói igazodjon. Megjeleníti a stratégia látni azt adja át egy function() jogkivonat, és a paraméterek kész. A stratégia fog megfelelő gondossággal térjen vissza az us Ha így tesz, az összes meg adatait a munkát. Ha így tesz szeretnénk fog tárolni a felhasználó és a token Panelcsoport, így nem kell a kérdezzen rá.

> [AZURE.IMPORTANT]
A fenti kódot, amely a kiszolgáló hitelesítést végezni történik bármelyik felhasználó megnyitja. Automatikus regisztrációs nevezik. A gyártási kiszolgálók nem kívánt engedélyez nélkül első őket a regisztrációs folyamat döntse el, hogy feldolgozzuk. Ez általában az a mintázat látható fogyasztói alkalmazások, akiknek engedélyezi a Facebook regisztrálása, de majd kérjük, hogy adja meg a további információt. Ha ez nem lett parancssori program, azt is van csak kiolvasott az e-mailt az jogkivonat objektumra, majd ismételt őket töltheti ki további információt és vissza. Mivel ez a próba-kiszolgáló azt egyszerűen felveheti a memóriában adatbázist.

### <a name="2-finally-protect-some-endpoints"></a>2. végül az egyes végpontok védelme

A passport.authenticate() hívás megadásával a használni kívánt protokollal történő védelme a végpontok.

A kiszolgáló-kódot szeretne elhelyezni érdekesebb az útvonal szerkesztése:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again-and-ensure-it-rejects-you"></a>18: futtassa újra a kiszolgáló alkalmazást, és győződjön meg arról, akkor utasítja el

Használata `curl` ismét a azt most már rendelkezik-e oauth2 hitelesítési mód védelmet a végpontok szemben. Azt fogja végezze el a előtt fut. az ügyfél SDK szemben a végponttal. A fejlécek vissza kell lennie ahhoz, hogy mondja le a megfelelő elérési vagyunk.

Először győződjön meg arról, hogy fut-e a monogoDB isntance.

    $sudo mongod

Ezután a címtárhoz módosítása, és sütővasak indítása.

    $ cd azuread
    $ node server.js

Próbálja ki egy egyszerű BEJEGYZÉSBEN:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Egy 401 a keresett Itt választ megegyezik, amely jelzi, hogy a Passport réteg partnere kapcsolatba próbál átirányítása az engedélyezés végpontot, amely olyan, pontosan milyen szeretne.


## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Gratulálok! A REST API-val szolgáltatás oauth2 hitelesítési mód van!

Már váltott amennyire nélkül, az oauth2 hitelesítési mód kompatibilis ügyfél ezen a kiszolgálón is. Szüksége lesz egy további forgatókönyv folyamatát.

Csak keresett olvashat arról, hogyan tudnak megvalósítani a REST API Restify és az oauth2 hitelesítési mód, esetén több, mint elég megtartása a szolgáltatás fejlesztése, és hogyan hozhat létre, ez a példa a tanulási kódot.

## <a name="next-steps"></a>Következő lépések

Hivatkozások [formájában érhető el az alábbi egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip)kész példa (nélkül a konfigurációs értékek), illetve klónozhatja, a GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Most már áthelyezheti Speciális témakörök alakzatot.  Előfordulhat, hogy szeretne kipróbálni:

[Használja a 2.0-s verzió végpont Node.js webalkalmazást biztonságos >>](active-directory-v2-devquickstarts-node-web.md)

További források olvassa el:
- [A 2.0-s verzió developer útmutató >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active Directoryval" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések beszerzése termékei

Javasoljuk, hogy mikor védelmi események előfordulásainak megtalálhatók az [erre a lapra](https://technet.microsoft.com/security/dd252948) , és a feliratkozás biztonsági figyelmeztető riasztás, értesítést kap.
