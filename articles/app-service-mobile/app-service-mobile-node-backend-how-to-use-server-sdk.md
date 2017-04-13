<properties
    pageTitle="Hogyan használható a Node.js kódmentes kiszolgálóval SDK, a Mobile-alkalmazások |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként működik a Node.js kódmentes kiszolgálóval SDK Azure alkalmazás szolgáltatás Mobile-alkalmazások."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Az Azure Mobile alkalmazások Node.js SDK használata

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ebben a cikkben részletes tudnivalókat és egy Node.js kódmentes az Azure alkalmazás szolgáltatás Mobile-alkalmazások használata a bemutató példákat.

## <a name="Introduction"></a>– Bevezetés

Azure Service Mobile Alkalmazásindítónalkalmazások mobile optimalizált adathozzáférés webes API hozzáadása egy webalkalmazáshoz lehetővé teszi.  Az Azure alkalmazás szolgáltatás Mobile alkalmazások SDK ASP.NET és Node.js webalkalmazások megadva.  A SDK lehetővé teszi, hogy a következő műveleteket:

- Az adatokhoz való hozzáférés (olvasható, Beszúrás, frissítés, Törlés) táblázat műveletek
- Egyéni API-műveletek

Mindkét műveletek hitelesítéshez Azure alkalmazás szolgáltatás, beleértve a vállalati identitáshoz például Facebook, Twitteren, a Google és a Microsoft, valamint Azure Active Directory közösségi Identitásszolgáltatók meghatároz összes Identitásszolgáltatók révén.

A [minták könyvtárában GitHub]minden használatieset minták talál.

## <a name="supported-platforms"></a>Támogatott platformok

Az Azure Mobile alkalmazások csomópont SDK támogatja a LTS kiadásban csomópontot, és újabb verzióiban.  Írás, kezdve a legújabb LTS verziója csomópont v4.5.0.  Csomópont más verzióit előfordulhat, hogy működik, de nem támogatottak.

Az Azure Mobile alkalmazások csomópont SDK támogatja a két adatbázis-illesztőprogramokat – a csomópont-mssql illesztőprogram támogatja az SQL Azure- és a helyi SQL Server-példány.  A sqlite3 illesztőprogramot SQLite adatbázisok csak egyetlen példány használatát támogatja.

### <a name="howto-cmdline-basicapp"></a>Útmutató: hozzon létre egy egyszerű Node.js kódmentes a parancssor használatával

Minden Azure alkalmazás szolgáltatás Mobile alkalmazást Node.js kódmentes ExpressJS alkalmazásként kezdődik.  ExpressJS a leggyakrabban használt webes szolgáltatás keretrendszer Node.js érhető el.  Létrehozhat egy basic [Express] alkalmazást az alábbiak szerint:

1. Egy parancs vagy PowerShell ablakában a projekthez könyvtár létrehozása.

        mkdir basicapp

2. A csomag szerkezet inicializálni npm init futtatni.

        cd basicapp
        npm init

    A npm init parancs megkérdezi, hogy a projekt inicializálni kérdésekre.  A példa kimenet jelenik meg:

    ![A npm init eredménye][0]

3. Telepítse a sürgős és azure-mobile-alkalmazások tárakat a npm tárat.

        npm install --save express azure-mobile-apps

4. Hozzon létre egy app.js fájlt, az egyszerű mobil kiszolgáló végrehajtásához.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Ez az alkalmazás a mobile optimalizált WebAPI létrehoz egy végpontot (`/tables/TodoItem`), az alapul szolgáló SQL adattár egy dinamikus séma használata nem hitelesített hozzáférést biztosít.  Alkalmas ügyfél tár rövid elindul a következő:

- [Android ügyfél quickstart útmutató]
- [Apache Cordova ügyfél quickstart útmutató]
- [iOS ügyfél quickstart útmutató]
- [A Windows áruházból ügyfél quickstart útmutató]
- [Xamarin.iOS ügyfél quickstart útmutató]
- [Xamarin.Android ügyfél quickstart útmutató]
- [Xamarin.Forms ügyfél quickstart útmutató]

Ehhez az alkalmazáshoz egyszerű a [minta basicapp GitHub]megtalálhatja a kódot.

### <a name="howto-vs2015-basicapp"></a>Útmutató: hozzon létre egy csomópont kódmentes Visual Studio 2015

Visual Studio 2015 belül az IDE Node.js alkalmazások fejlesztéséhez egy bővítmény szükséges.  Szeretné kezdeni, telepítse a [Node.js eszközök 1.1 for Visual Studio].  Miután a Node.js Tools for Visual Studio telepítve van, Express 4.x alkalmazás létrehozása:

1. Nyissa meg az **Új projekt** párbeszédpanelt ( **fájlból** > **Új** > **Project...**).

2. Bontsa ki a **sablonok** > **JavaScript** > **Node.js**.

3. Jelölje ki az **egyszerű Azure Node.js Express 4-es alkalmazás**.

4. Adja meg a projekt nevét.  Kattintson az *OK gombra*.

    ![Új projekt Visual Studio 2015][1]

5. Kattintson a jobb gombbal a **npm** csomópontot, és válassza a **telepítés új npm csomagok...**.

6. Előfordulhat, hogy a npm katalógus létrehozása az első Node.js alkalmazás frissítéséhez.  Ha szükséges, kattintson a **frissítés** parancsra.

7. A Keresés mezőbe írja be az _azure-mobile-alkalmazások_ .  Kattintson az **azure-mobile-alkalmazások 2.0.0** csomagra, majd kattintson a **Telepítés csomagot**.

    ![Új npm csomagok telepítése][2]

8. Kattintson a **Bezárás**gombra.

9. Nyissa meg az Azure Mobile alkalmazások SDK támogatásának hozzáadása a _app.js_ fájlt.  A sor alsó részén a tár 6 at kimutatások szükség, és adja meg a következő kódot:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Az add körülbelül sor 27 után a app.use kimutatásokban, a következő kódot:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Mentse a fájlt.

10. Futtassa az alkalmazást helyileg (a http://localhost:3000 az API felszolgált), vagy a közzététel az Azure.

### <a name="create-node-backend-portal"></a>Útmutató: hozzon létre egy Node.js kódmentes az Azure portál használatával

Az [Azure portál]hozhat létre egy mobilalkalmazás kódmentes jobbra. Hajtsa végre az alábbi lépéseket, vagy hozzon létre egy ügyfél- és kiszolgálóoldali együtt a [mobilalkalmazásban létrehozása](app-service-mobile-ios-get-started.md) oktatóprogram. Az oktatóprogram tartalmazza ezeket az utasításokat egyszerűsített verziója, és fogalmat projektek igazolása leginkább.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Vissza az _első lépések_ lap **API a tábla létrehozása**válassza a **Node.js** a **Kódmentes nyelvet**. Jelölje be "**e igazolja, hogy ez a művelet felülírja az összes webhely tartalma.**", majd kattintson a **tábla létrehozása TodoItem**.

### <a name="download-quickstart"></a>Útmutató: Töltse le a Node.js kódmentes quickstart útmutató kód projekt mely számjegy használatával

Az **első lépések** lap portal segítségével egy Node.js mobilalkalmazás kódmentes létrehozásakor Node.js projekt jött létre, és a webhelyre telepíti. Táblázatok és az API-khoz hozzáadása, és szerkesztheti a Node.js kódmentes a portálon kód-fájlokat. Különböző telepítési eszközök segítségével is, hogy a hozzáadni vagy módosítani a táblázatok és az API-hoz, majd közzé a projektet, töltse le a kódmentes projekt. További tudnivalókért lásd: az [Azure alkalmazás szolgáltatás telepítési útmutatóját]. az alábbi eljárás mely számjegy összegyűjti segítségével töltse le a quickstart útmutató projekt kódot.

1. Mely számjegy, telepítése, ha még nem tette. Mely számjegy telepítéséhez szükséges lépéseket operációs rendszerek eltérnek. [Telepítés mely számjegy](http://git-scm.com/book/en/Getting-Started-Installing-Git) operációs rendszer-specifikus felosztott és a telepítési útmutatást talál.

2. Kövesse a [engedélyezése az alkalmazás szolgáltatás alkalmazás tárházba](../app-service-web/app-service-deploy-local-git.md#Step3) ahhoz, hogy a kódmentes webhelyéhez, végrehajtása telepítésének felhasználónév és jelszó a Megjegyzés mely számjegy tárháza.

3. Az a mobilalkalmazás kódmentes lap, a jegyezze le a **mely számjegy adatfeliratsor URL-cím** beállítást.

4. Hajtsa végre a `git clone` paranccsal és a mely számjegy klónozhatja URL-CÍMÉT, ha szükséges, ahogy a következő példában a jelszó beírása:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Keresse meg a helyi könyvtár, amely az előző példában /todolist, és figyelje meg, hogy projektfájlok letöltött. Keresse meg a `todoitem.json` a fájl a `/tables` címtár.  A táblázat engedélyek határozza meg, ezt a fájlt.  Is megtalálhatók a `todoitem.js` fájl ugyanabban a mappában, amely meghatározza, hogy CRUD művelet parancsfájlok el a táblázatot.

6. Miután módosította projektfájlokat, hajtsa végre az alábbi parancsok felvétele, véglegesítése, és töltse fel a módosításokat a webhelyre:

        $ git commit -m "updated the table script"
        $ git push origin master

    Új fájlok hozzáadása a projekthez, amikor először kell végrehajtani a `git add .` parancsot.

A webhely minden alkalommal, amikor új halmazának véglegesítése tolódik a webhely ismételt közzététele.

### <a name="howto-publish-to-azure"></a>Útmutató: a Node.js kódmentes Azure közzététele

Microsoft Azure sok mechanizmusok a Azure alkalmazás szolgáltatás Mobile alkalmazások Node.js kódmentes közzétételi az Azure szolgáltatás biztosít.  Ide tartoznak a beépül a Visual Studio telepítési eszközök, a parancssori eszközök és az adatforrás-vezérlő alapján folyamatos telepítési lehetőségek felhasználásával.  A témával kapcsolatos további tudnivalókért lásd: az [Azure alkalmazás szolgáltatás telepítési útmutatóját].

Azure alkalmazás szolgáltatás Node.js alkalmazás telepítése előtt tekintse át az adott helyen foglalja magában:

- [A csomópont-verzió] megadása
- [Csomópont modulok] használata

### <a name="howto-enable-homepage"></a>Útmutató: az alkalmazás kezdőlap engedélyezése

Sok alkalmazások webes és mobilalkalmazások és ExpressJS keretében lehetővé teszi a két metszettel kombinálása.  Néha azonban érdemes csak egy mobil felületen végrehajtásához.  Célszerű rendelkeznek a céloldal ahhoz, hogy az alkalmazás szolgáltatás lépéseket.  Adja meg a saját kezdőlapja, vagy átmeneti kezdőlap engedélyezése.  Ahhoz, hogy egy ideiglenes kezdőlap, használja az alábbi elindítását Azure Mobile-alkalmazások:

    var mobile = azureMobileApps({ homePage: true });

Ha csak ez a beállítás elérhető helyileg fejlesztésekor, ezt a beállítást is hozzáadhat a `azureMobile.js` fájlt.

## <a name="TableOperations"></a>Táblázat műveletek 

Az azure-mobile-alkalmazások Node.js Server SDK Ide kattintva jelenítse meg az adattáblák, mint egy WebAPI Azure SQL-adatbázisban tárolt eljárások.  Öt műveleti állnak rendelkezésre.

| Művelet | Leírás |
| --------- | ----------- |
| _Táblanév_ /tables/ beszerzése | Összes rekord beolvasása a táblázatban |
| /Tables/_táblanév_/:id beszerzése | Egy adott rekord beolvasása a táblázatban |
| BEJEGYZÉS /tables/_táblanév_ | Hozzon létre egy rekordot a táblázatban |
| A javítás /tables/_táblanév_/:id | A tábla egy rekord frissítése |
| /Tables/_táblanév_/:id törlése | A táblázat rekord törlése |

Ez a WebAPI [OData] támogatja, és kibővíti a táblázatot séma a [kapcsolat nélküli szinkronizálás]támogatási.

### <a name="howto-dynamicschema"></a>Útmutató: táblázatok használata a dinamikus séma meghatározása

Táblázat is használható, előtt meg kell adni.  Táblázatok (ahol a fejlesztői határozza meg a séma oszlopainak) statikus séma vagy dinamikusan definiálható (ahol a SDK szabályozza a séma alapján, bejövő felkérést). Ezenkívül a fejlesztői szabályozhatja bizonyos részei a WebAPI Javascript-kód hozzáadása a definíció.

Legjobb módszer meg kell meghatározása táblázat a táblázatok címtárban Javascript-fájlt, majd a tables.import() módszerrel importálja a táblákat.  Bővítése a basic alkalmazást, a app.js fájlt szeretne módosítani:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Határozza meg a táblát. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Táblázatok használata a dinamikus séma alapértelmezés szerint.  Ha ki szeretné kapcsolni dinamikus séma globálisan, állítsa az App beállítása **MS_DynamicSchema** hamis belül az Azure-portálra.

Egy kész példa a [Teendők minta GitHub]talál.

### <a name="howto-staticschema"></a>Útmutató: definiálása táblák egy statikus séma használata

Explicit módon határozhatja meg az oszlopok kattintva jelenítse meg a WebAPI keresztül.  Az azure-mobile-alkalmazások Node.js SDK automatikusan hozzáadja a listához, Ön által kapcsolat nélküli szinkronizálás működéséhez szükséges további oszlopokat.  Ha például a quickstart útmutató ügyfélalkalmazásokban megkövetelése a két oszlopot tartalmazó táblázat: szövegtartalma (karakterlánca), és fejezze be (logikai).  
A táblázat definiálható a táblázat definition JavaScript-fájlt (a táblázatok könyvtárában található) az alábbi képlettel történik:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Ha statikus táblákat, majd meg kell is hívni a tables.initialize() módszer az adatbázissémát induláskor létrehozott.  A tables.initialize() metódus ad vissza egy [cikk megvásárlását fel] , hogy a webszolgáltatás nem szolgálják az adatbázisban, alatt álló inicializált előtt kérések.

### <a name="howto-sqlexpress-setup"></a>Útmutató: használata SQL Express egy fejlesztés adatokat tároló a helyi számítógépre

Az Azure Mobile alkalmazások a AzureMobile alkalmazások csomópont SDK adatok felszolgálásához beépített három lehetőségeket is biztosít: SDK lehetővé teszi az adatok felszolgálásához beépített három lehetőség közül választhat:

- A **memória** -illesztőprogram használatával adja meg a nem állandó például üzlet
- A szükséges egy SQL Express adattár fejlesztési a **mssql** illesztőprogramot használatával
- A **mssql** illesztőprogramot használja egy SQL Azure-adatbázis adatokat tároló nyújtsanak előállítása

Az Azure Mobile alkalmazások Node.js SDK a [mssql Node.js csomagot] használ, létrehozásához és a kapcsolat SQL Express és SQL-adatbázis használata.  A csomag igényel, hogy engedélyezze az SQL Express példányon TCP-kapcsolatokat.

> [AZURE.TIP]A memória-illesztőprogram nem nyújt a tesztelésre létesítményekhez teljes sorozatát.  A kódmentes helyileg tesztelni szeretné, ha egy SQL Express adattár és a mssql illesztőprogram használatát javasoljuk.

1. Töltse le és telepítse a [Microsoft SQL Server 2014-es Express].  Gondoskodjon arról, hogy telepíteni az SQL Server 2014-es Express eszközök edition.  Hacsak nincs 64 bites verziók kifejezetten szüksége a 32 bites fogyaszt kevesebb memória futtatásakor.

2. Futtassa az SQL Server 2014-es Konfigurációkezelőjével.

  1. Bontsa ki a bal oldali fastruktúrájú menü az **SQL Server Network Configuration** csomópontot.
  2. Kattintson a **protokollok SQLEXPRESS**.
  3. Kattintson a jobb gombbal a **TCP/IP elemre** , és jelölje be a **engedélyezése**.  Az előugró panelen kattintson az **OK gombra** .
  4. Kattintson a jobb gombbal a **TCP/IP elemre** , és válassza a **Tulajdonságok parancsot**.
  5. Kattintson az **IP-címek** fülre.
  6. Keresse meg a **IPAll** csomópontot.  A **TCP-Port** mezőbe írja be a **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Kattintson az **OK gombra**.  Az előugró panelen kattintson az **OK gombra** .
  8. **SQL Server Services** elemre a bal oldali fastruktúrájú menüben.
  9. Kattintson a jobb gombbal az **SQL Server (SQLEXPRESS)** , és jelölje ki az **Újraindítás**
  10. Zárja be az SQL Server 2014-es Konfigurációkezelőjével.

3. Az SQL Server 2014-es Management Studio futtatása és az SQL Express példányához

  1. Kattintson a jobb gombbal a példány az objektum-kezelő ablakban, majd válassza a **Tulajdonságok**
  2. Jelölje ki a **Biztonság** lapon.
  3. Ellenőrizze az **SQL Server és a Windows-hitelesítési módban** kiválasztva
  4. Kattintson az **OK gombra**

        ![Express SQL-hitelesítés konfigurálása][4]

  5. Bontsa ki a **biztonsági** > **bejelentkezések** az objektum Explorer
  6. Kattintson a jobb gombbal a **bejelentkezések** , és válassza az **Új bejelentkezési...**
  7. Írja be bejelentkezési nevét.  Jelölje ki az **SQL Server-hitelesítés**.  Írjon be egy jelszót, majd adja meg a ugyanazt a jelszót a **jelszó megerősítése**.  A jelszó kell felelnie a Windows összetettsége.
  8. Kattintson az **OK gombra**

        ![Az SQL Express új felhasználó hozzáadása][5]

  9. Kattintson a jobb gombbal az új bejelentkezési adatait, és válassza a **Tulajdonságok parancsot**
  10. A **Kiszolgálói szerepkörök** lap kijelölése
  11. Jelölje be a jelölőnégyzetet a **dbcreator** kiszolgálói szerepkör
  12. Kattintson az **OK gombra**
  13. Zárja be az SQL Server 2015 Management Studio

Győződjön meg róla, rögzítheti a felhasználónév és jelszó választotta-e.  Előfordulhat, hogy rendeljen további kiszolgálói szerepkörök és engedélyek attól függően, hogy az adott adatbázis igényeknek megfelelően alakíthatja.

A Node.js alkalmazás beolvassa a **SQLCONNSTR_MS_TableConnectionString** környezet változó a kapcsolati karakterláncot az adatbázishoz.  Beállíthatja, hogy ez a változó a környezetben.  Például a PowerShell használatával a környezet-változó beállítása:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Az adatbázis elérése a TCP/IP kapcsolaton keresztül, és a kapcsolatot a felhasználónév és jelszó szükséges.

### <a name="howto-config-localdev"></a>Útmutató: a projekt helyi fejlesztési konfigurálása

Azure Mobile-alkalmazások beolvassa a helyi fájlrendszerben _azureMobile.js_ hívását JavaScript-fájlt.  Ne használja ezt a fájlt az Azure Mobile alkalmazások SDK konfigurálása a termelési - alkalmazás beállításainak belül az [Azure portálon] használja helyette.  A _azureMobile.js_ fájl kell konfigurációs-objektum exportálása egy.  A leggyakoribb beállítások az alábbiak:

- Adatbázis-beállításai
- Diagnosztikai naplózás beállításai
- Alternatív CORS beállításai

Egy példa _azureMobile.js_ fájl végrehajtása az előző adatbázis beállításai a következőképpen:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Azt javasoljuk, _azureMobile.js_ hozzáadása a _.gitignore_ fájlt (vagy más verziókövetési szolgáltatása figyelmen kívül hagyása fájl) megakadályozhatja, hogy a jelszó nem tárolhatók a felhőben.  Gyártási beállításainak mindig belül az [Azure portál]alkalmazás beállításainak konfigurálása.

### <a name="howto-appsettings"></a>Útmutató: A mobilalkalmazásban alkalmazás beállításainak konfigurálása

A _azureMobile.js_ fájlban legtöbb beállítás van-e az [Azure portál]egy ezzel egyenértékű App beállítása.  Az alábbi lista használatával állítsa be az alkalmazás az alkalmazás beállításai:

| App beállítása                 | _azureMobile.js_ beállítás  | Leírás                               | Az érvényes értékek                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | név                      | Az alkalmazás neve                       | karakterlánc                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | A naplózandó üzenetek minimális napló szintje      | Hiba, figyelmeztetés, részletes adatai hibakeresési silly |
| **MS_DebugMode**            | hibakeresési                     | Engedélyezés vagy letiltás hibakeresési mód              | IGAZ, hamis                                 |
| **MS_TableSchema**          | Data.Schema               | SQL-táblák alapértelmezett séma neve        | karakterlánc (alapértelmezett: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Engedélyezés vagy letiltás hibakeresési mód              | IGAZ, hamis                                 |
| **MS_DisableVersionHeader** | verzióját (beállítva, nem meghatározott)| Az X-ZUMO-kiszolgáló verzióját élőfej letiltása | IGAZ, hamis                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Letiltja az ügyfélprogram API-verziójának ellenőrzése     | IGAZ, hamis                                 |

Az alkalmazás beállítás megadásához:

1. Jelentkezzen be az [Azure-portálon].
2. Jelölje ki **az összes erőforrás** vagy **Alkalmazás szolgáltatások** , majd kattintson a mobil alkalmazás nevére.
3. Alapértelmezés szerint a beállítások lap nyílik meg. Ha nem, kattintson a **Beállítások**gombra.
4. Az általános menüjében kattintson az **alkalmazás beállításai** parancsra.
5. Görgessen le az alkalmazás beállításai szakaszban.
6. Ha az alkalmazás beállítása a már létezik, kattintson az alkalmazás beállítás értékét a szerkesztendő értékét.
7. Ha az alkalmazás beállítása nem létezik, írja be az alkalmazás beállítása a kulcs mezőbe, és a értéket az érték mezőben.
8. Miután elkészült, kattintson a **Mentés**gombra.

A legtöbb alkalmazás beállításainak módosításával szolgáltatás újraindításra lehet szükség.

### <a name="howto-use-sqlazure"></a>Útmutató: a termelési adatokat tároló SQL-adatbázis használata

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure SQL-adatbázis használata egy adattár megegyezik Azure alkalmazás szolgáltatási kérelem diagramtípusokat keresztül. Ha még nem tette meg, kövesse ezeket a lépéseket követve hozzon létre egy mobilalkalmazás kódmentes.

1. Jelentkezzen be az [Azure-portálon].

2. Felső balra az ablak, kattintson a **+ Új** gombra > **webes + Mobile** > **Mobile alkalmazást**, majd nevezze el a mobilalkalmazás kódmentes.

3. Az **Erőforráscsoport** mezőben adja meg az App ugyanazt a nevet.

4. Az alapértelmezett alkalmazás szolgáltatáscsomagja be van jelölve.  Ha kívánja, az alkalmazás szolgáltatás csomag módosítása, ehhez az alkalmazás szolgáltatáscsomagja kattintva > **+ Új létrehozása**.  Adjon nevet az új alkalmazás szolgáltatás terv, és válassza ki a megfelelő helyre.  Kattintson a árak réteg, és válassza ki a megfelelő árak réteg a szolgáltatás. Jelölje ki a **nézet összes** árak további lehetőségek, például az **ingyenes** , **megosztott**megtekintése.  Miután kijelölte a árak réteg, kattintson a **Kijelölés** gombra.  Az **alkalmazás szolgáltatáscsomagja** lap kattintson **az OK gombra**.

5. Kattintson a **létrehozása**gombra. Kiépítési egy mobilalkalmazás kódmentes néhány percet is igénybe vehet.  Miután a mobilalkalmazás kódmentes már kiépítve, a portálon megnyitása a mobilalkalmazás kódmentes számára a **Beállítások** lap

A mobilalkalmazás kódmentes létrehozását követően megadhatja egy meglévő SQL-adatbázis csatlakoztatása a mobilalkalmazás kódmentes, vagy hozzon létre egy új SQL-adatbázishoz.  Ebben a részben hozzunk létre egy SQL-adatbázisban.

> [AZURE.NOTE]Ha már van egy adatbázist a mobilalkalmazásban kódmentes ugyanazon a helyen, ehelyett a **meglévő adatbázis használata** , és válassza ki az adatbázist. Más helyen adatbázis használata nem ajánlott miatt késések magasabb.

6. Kattintson az új mobilalkalmazás kódmentes **Beállítások** > **Mobilalkalmazás** > **adatok** > **+ Hozzáadás gombra**.

7. Kattintson az **adatbázisból származó adatok hozzáadása** lap **SQL-adatbázis - szükséges beállításainak konfigurálása** > **Új adatbázis létrehozása**.  Adja meg az új adatbázis nevét a **név** mezőbe.

8. Kattintson a **kiszolgálóra**.  Az **Új kiszolgáló** lap a **kiszolgáló neve** mezőjében adjon meg egy egyedi kiszolgáló neve, és adjon meg egy megfelelő **Server felügyeleti bejelentkezés** , és a **jelszó**.  Győződjön meg róla, **kiszolgáló elérésére engedélyezése azure szolgáltatás** be van jelölve.  Kattintson az **OK gombra**.

    ![Azure SQL-adatbázis létrehozása][6]

9. Az **Új adatbázis** lap kattintson **az OK gombra**.

10. Jelentkezzen be az **adatok kapcsolat hozzáadása** a lap jelölje be a **kapcsolati karakterlánc**, írja be a bejelentkezési és az adatbázis létrehozásakor a megadott jelszót.  Meglévő adatbázis használata esetén adja meg a bejelentkezési adatok arra az adatbázisra.  Ha meg van adva, kattintson az **OK gombra**.

11. Újra az **adatbázisból származó adatok hozzáadása** lap be, és kattintson az **OK gombra** az adatbázis létrehozása.

<!--- END OF ALTERNATE INCLUDE -->

Az adatbázis létrehozása néhány percet is igénybe vehet.  Az **értesítési** területen segítségével a telepítés állapotának nyomon követéséhez.  Nem végrehajtási mindaddig, amíg az adatbázis rendszerbe sikeresen.  Miután sikeresen rendszerbe, az SQL-adatbázis-példány a mobil kódmentes alkalmazás beállításainak a kapcsolati karakterlánc jön létre.  Megtekintheti a **Beállítások**alkalmazást beállítás > **Alkalmazásbeállítások** > **kapcsolati karakterláncot**.

### <a name="howto-tables-auth"></a>Útmutató: táblázatok eléréséhez használatbavétele

Ha ki szeretne alkalmazás szolgáltatás hitelesítési használata a táblázatok végpontot, meg kell adnia alkalmazás szolgáltatás hitelesítési az [Azure portál] először.  Hitelesítés konfigurálása az Azure-alkalmazás szolgáltatásainak olvashat a használni kívánt identitásszolgáltató használatához olvassa el a konfigurációs Útmutató:

- [Azure Active Directory-hitelesítés konfigurálása]
- [Facebook-hitelesítés konfigurálása]
- [Google-hitelesítés konfigurálása]
- [A Microsoft Authentication konfigurálása]
- [Twitter hitelesítés konfigurálása]

Mindegyik táblázatban szerepel egy access-tulajdonság való hozzáférés korlátozása a táblázat használható.  A következő példában statikus definiált táblázat a hitelesítés szükséges.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Az access tulajdonság is igénybe vehet, értékek egyike

  - *Névtelen* jelzi, hogy az ügyfélalkalmazás engedélyezve van-e olvasható az adatok hitelesítés nélkül
  - *hitelesített* azt jelzi, hogy az ügyfélalkalmazás kell küldenie a kérést egy érvényes hitelesítési jogkivonat
  - *letiltott* azt jelzi, hogy az alábbi táblázat le van tiltva

Ha az access tulajdonság nem meghatározott, az access nem hitelesített engedélyezett.

### <a name="howto-tables-getidentity"></a>Útmutató: a táblázatok Jogcímalapú hitelesítés használata

Beállíthatja a különböző Jogcímalapú hitelesítés beállításakor kérő.  Ezeket a követelések nem érhetők el a szokásos módon keresztül a `context.user` objektumot.  Jó helyen jár, hogy tudja visszaszerezni használata a `context.user.getIdentity()` módot.  A `getIdentity()` módszer egy cikk megvásárlását fel egy objektum feloldó adja eredményül.  Az objektum van beállítható (facebook, a google, a twitteren, microsoftaccountja vagy aad) hitelesítési módszer szerint.

Ha például beállítása hitelesítés Microsoft Account és kérelem azoknak az e-mail címét, ha vehet az e-mail címet a rekord az alábbi táblázat vezérlővel:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Szeretné látni, hogy milyen követelések érhetők el, használata a böngészőben megtekintéséhez a `/.auth/me` a webhely végpontot.

### <a name="howto-tables-disabled"></a>Útmutató: táblázat műveletek elérésének letiltása

Mellett jelenjen meg a táblát, az access tulajdonság kattintva állítsa be az egyes műveletek használható.  Vannak négy műveletek:

  - *olvassa el* az a RESTful első művelet, a táblázatban
  - a RESTful utáni műveletet a táblázat *beszúrása* .
  - *frissíteni* az a RESTful javítás művelet, a táblázatban
  - *törölje* az a RESTful törlése művelet, a táblázatban

Ha például merülnek fel egy olyan csak olvasható nem hitelesített táblázat:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Útmutató: módosítsa a lekérdezést, amely a táblázat műveletek

Táblázat műveletekhez közös követelmény is biztosít az adatok korlátozott nézet.  Például egy olyan táblát, hogy csak olvasható vagy a saját rekordok frissítése a hitelesített felhasználói azonosítóval címkézett, előfordulhat, hogy adja meg.  A következő táblázat definíciót ezt a funkciót kínál:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Amely általában a lekérdezés végrehajtása műveletnek egy lekérdezés tulajdonság, beállíthatja, hogy a where záradék. A lekérdezés tulajdonság értéke egy [QueryJS] -objektum, amely az OData-lekérdezés átalakítása, amit az adatok kódmentes dolgozhat.  Egyszerű egyenlő (olyanra, mint az előző) esetben egy térképen is használható. Felveheti az SQL-záradékok is:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Útmutató: finom törlés konfigurálása táblázatra

Finom törlés valójában nem törli a rekordokat.  Ehelyett kész megjelöléssel látja el őket a törölt oszlop igaz értékre állításával az adatbázisban lévő törölt.  Az Azure Mobile alkalmazások SDK kivéve, ha a mobil ügyfélprogram SDK használja IncludeDeleted() automatikusan törli törölt rekordok közül.  Adja meg a táblát, finom törlés, állítsa be a `softDelete` tulajdonság a tábla-definíciós fájl:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

A végleges törlése a bejegyzések – akár egy ügyfélalkalmazás olyan WebJob Azure függvény e- és egy egyéni API a kisalkalmazások létre kell hoznia.

### <a name="howto-tables-seeding"></a>Útmutató: rendezze az adatokat tartalmazó adatbázis

Amikor egy új alkalmazás létrehozása, előfordulhat, hogy kívánt rendezze az adatokat tartalmazó táblázat.  Ezt megteheti a táblázat definition JavaScript-fájlt belül az alábbi képlettel történik:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Az adatok rendezi csak befejeződött a táblázatot az Azure Mobile alkalmazások SDK által létrehozásakor.  Ha a táblázat már létezik az adatbázisban, a program a táblázatba beékelt nincsenek adatok.  Ha dinamikus séma be van kapcsolva, majd a séma az osztott adatok következtetett a függvény.

Azt javasoljuk, hogy kifejezetten hívja a `tables.initialize()` módszer a szolgáltatás megkezdi a táblázat létrehozásához.

### <a name="Swagger"></a>Útmutató: Swagger támogatásának engedélyezése

Azure Service Mobile Alkalmazásindítónalkalmazások megtalálható beépített [Swagger] támogatás.  Ahhoz, hogy a Swagger támogatási, előbb telepítenie swagger – a felhasználói felület függőség:

    npm install --save swagger-ui

Telepítése után engedélyezheti az Azure Mobile-alkalmazások konstruktort Swagger támogatása:

    var mobile = azureMobileApps({ swagger: true });

Valószínűleg csak engedélyezni szeretné Swagger támogatási fejlesztési kiadásokban.  Ehhez felhasználásával a `NODE_ENV` app beállítása:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

A swagger végpont a http://_yoursite_.azurewebsites.net/swagger helyezkedik el.  A Swagger felhasználói felületének keresztül érheti el a `/swagger/ui` végpontot.  Ha úgy dönt, hogy a teljes alkalmazás keresztül hitelesítést igényel, a Swagger hibát eredményez.  A legjobb, válassza az Azure alkalmazás szolgáltatás hitelesítési keresztül nem hitelesített kérelmeket engedélyezni / engedély-beállításokat, majd szabályozhatja hitelesítés használatával a `table.access` tulajdonság.

Swagger lehetősége is hozzáadhat a `azureMobile.js` fájl, ha csak Swagger támogatási helyileg fejlesztésekor.

## <a name="a-namepushpush-notifications"></a><a name="push">Leküldéses értesítések

Mobile-alkalmazások integrálódik Azure értesítés hubok ahhoz, hogy minden fő platformon eszközök millióit lezárási leküldéses értesítéseket küldeni. Értesítés hubok használatával küldhet leküldéses értesítéseket IOS-en, Androidon és Windows-eszközökön. Ha többet szeretne tudni, hogy Ön a értesítés hubok is [Értesítés hubok áttekintése](../notification-hubs/notification-hubs-push-notification-overview.md)című témakörben találhat.

### </a><a name="send-push"></a>Útmutató: küldés leküldéses értesítések

A következő kódot közvetítési leküldéses értesítést küld nyilvántartott az iOS-eszközök a leküldéses objektum segítségével mutatja be:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Az ügyféltől a sablon leküldéses regisztrálását létrehozásával inkább küldhet sablon leküldéses üzenet összes támogatott platformok eszközökön. A következő kódot sablon értesítés küldése mutatja be:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Útmutató: küldés leküldéses értesítéseket szeretne egy hitelesített felhasználó-címkék használata

Ha egy hitelesített felhasználó regisztrálja a leküldéses értesítéseket, felhasználói azonosító címke automatikusan felkerül a regisztráció. Címke használatával küldhet leküldéses értesítéseket regisztrált egy adott felhasználó összes eszközt. A következő kódot a kérés felhasználó biztonsági azonosítója kap, és a sablon leküldéses értesítést küld minden eszköz regisztrációs az illető felhasználónak:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Leküldéses értesítések egy hitelesített ügyfélprogramból regisztrálásakor győződjön meg róla, hogy a hitelesítés teljes regisztrációs előtt.

## <a name="CustomAPI"></a>Egyéni API-hoz

###  <a name="howto-customapi-basic"></a>Útmutató: egyéni API-val meghatározása


Az adatok hozzáférésének API az /tables végpontot, kívül Azure Mobile-alkalmazások is biztosít egyéni API-val.  Egyéni API-khoz határozzák meg a táblázat meghatározásaival hasonló módon, és a azonos létesítményekhez, beleértve a hitelesítési hozzáférhetnek.

Ha ki szeretne alkalmazás szolgáltatás hitelesítést használ egy egyéni API-val, meg kell adnia alkalmazás szolgáltatás hitelesítési az [Azure portál] először.  Hitelesítés konfigurálása az Azure-alkalmazás szolgáltatásainak olvashat a használni kívánt identitásszolgáltató használatához olvassa el a konfigurációs Útmutató:

- [Azure Active Directory-hitelesítés konfigurálása]
- [Facebook-hitelesítés konfigurálása]
- [Google-hitelesítés konfigurálása]
- [A Microsoft Authentication konfigurálása]
- [Twitter hitelesítés konfigurálása]

Egyéni API-khoz sokkal ugyanúgy, mint a tábla API definiálva.

1. Hozzon létre egy **api** -címtár
2. Hozzon létre egy API definition JavaScript-fájlt az **api** -címtár.
3. Az importálási módszer segítségével az **api** -címtár importálhatja.

Az alábbiakban a korábban használt basic-app statisztikai sokaság mintájából prototípusának api meghatározása.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Nézzük végig a példát API-val, amely a _Date.now()_ módszerrel kiszolgáló dátumát adja vissza.  Az alábbiakban a api/date.js fájlt:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Az egyes paraméterek a szabványos RESTful műveleteken - kérése, a bejegyzés, a javítás vagy a törlés egyike.  A módszer az szabványos [ExpressJS köztes] függvény, amely a szükséges kimeneti küld.

### <a name="howto-customapi-auth"></a>Útmutató: hitelesítés szükséges egy egyéni API eléréséhez

Azure Mobile alkalmazások SDK hitelesítési a táblázatok végpont és az egyéni API-hoz hasonló módon hajtja végre.  Hitelesítés az előző szakaszban fejlesztett API felvenni, **access** tulajdonság:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Hitelesítés is műveleteket adhat meg:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Az azonos jogkivonat, amellyel a táblázatok végpont egyéni API-khoz hitelesítést igénylő kell használni.

### <a name="howto-customapi-auth"></a>Útmutató: nagy fájlfeltöltések kezelése

Azure Mobile alkalmazások SDK a [szervezet-elemző köztes](https://github.com/expressjs/body-parser) elfogadásához és a beküldött a Laptörzs tartalmának dekódolását használja.  Előtti törzs-elemző fogadja el a nagyobb fájlfeltöltések adhatja meg:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Fájl továbbítása előtt kódolva alap 64.  Esetében a tényleges feltöltés méretének növelése (és ezért méretének figyelembe kell venni).

### <a name="howto-customapi-sql"></a>Útmutató: egyéni SQL-utasítások végrehajtása

Az Azure Mobile alkalmazások SDK lehetővé teszi, hogy az egész környezetben keresztül a request objektum lehetővé teszi, hogy könnyen végrehajtása a definiált adatszolgáltató paraméteres SQL-utasítások való hozzáférést:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Hibakeresése során, egyszerűen táblázatokat és könnyen API-hoz

### <a name="howto-diagnostic-logs"></a>Útmutató: hibakeresési diagnosztizálása és Azure Mobile appjai – problémamegoldás

Az Azure alkalmazás szolgáltatás több hibakeresési és hibaelhárítási Node.js alkalmazások technikák biztosít.
Első lépések a Node.js Mobile kódmentes javítást az alábbi cikkekben talál:

- [Az alkalmazás Azure szolgáltatás figyelése]
- [Azure alkalmazás szolgáltatás a diagnosztikai naplózás engedélyezéséhez]
- [A Visual Studióban az Azure alkalmazás szolgáltatásainak – problémamegoldás]

NODE.js alkalmazások hozzáférést a diagnosztikai naplókban eszközök széles köre.  Belső az Azure Mobile alkalmazások Node.js SDK [Winston] használja a diagnosztikai naplózás.  Naplózás hibakeresési mód engedélyezése vagy az **MS_DebugMode** alkalmazás beállítása az [Azure portál]igaz beállításával automatikusan engedélyezett. A diagnosztikai naplók az [Azure portál]létrehozott naplók jelennek meg.

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Útmutató: az Azure-portálon egyszerűen táblázatok használata

A portálon egyszerűen táblázatokat látványosan létrehozása és használata a táblázatok jobbra a portálon. Az alkalmazás szolgáltatás szerkesztővel táblázat műveletek még akkor is szerkesztheti.

Ha **egyszerűen táblázatokat** a kódmentes webhelybeállítások gombra kattint, hozzáadása, módosítása vagy táblázat törlése. A táblázatban lévő adatok is láthatja.

![Egyszerű táblázatok használata](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Az alábbi parancsok állnak rendelkezésre a parancssávon tábla:

+ **Engedélyek módosítása** - olvasható engedélyeinek módosítása, beszúrása, frissíthet és törölhet műveletek a táblázatban. 
  Beállítások közül választhat hitelesítést igényel, vagy a műveletet az összes elérésének letiltása a névtelen hozzáférés engedélyezése. 
+ **Parancsfájl szerkesztése** – a parancsprogram a tábla meg van nyitva a alkalmazás szolgáltatás szerkesztőben.
+ **Séma kezelése** – hozzáadása vagy oszlopok törlése, vagy módosítsa a táblázat index.
+ **Táblázat törlése** - csonkít meglévő táblához kell az összes adat sorok törlése, de a séma elhagyása változatlan marad.
+ **Sorok törlése** - egyes sorok törlése.
+ **A folyamatos átvitelű naplók megtekintése** – csatlakozik a továbbított naplózási szolgáltatás a webhelyen.

###<a name="work-easy-apis"></a>Útmutató: az Azure-portálon egyszerűen API-k használata

Egyszerű API-hoz a portálon látványosan létrehozása és használata az egyéni API-khoz jobbra a portálon. Módosíthatja a alkalmazás szolgáltatás szerkesztőben API-parancsfájlokat.

**Egyszerű API-hoz** a kódmentes webhely beállításai parancsra kattint, hozzáadása, módosítása vagy törlése egy egyéni API-végpontot.

![Egyszerű API-k használata](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

A portálon a hozzáférési engedélyek módosítása az egy adott HTTP-művelet, az alkalmazás szolgáltatás szerkesztő API-parancsprogram szerkesztése vagy a továbbított naplók megtekintése.

###<a name="online-editor"></a>Útmutató: kódot az alkalmazás szolgáltatás szerkesztő szerkesztése

Az Azure portal lehetővé teszi a szerkesztheti a Node.js kódmentes parancsfájl-fájlokat az alkalmazás szolgáltatás szerkesztő anélkül, hogy a projekt letöltése a helyi számítógépre. Fájlokat szeretne szerkeszteni, script Editor online programban:

1. Kattintson a mobilalkalmazás kódmentes lap az **összes beállítások** > **egyszerű táblák** vagy **Egyszerűen API-hoz**, kattintson a táblázat vagy API, majd **parancsfájl szerkesztése**. A parancsprogram a alkalmazás szolgáltatás Editor alkalmazásban nyitja meg.

    ![Alkalmazás szolgáltatás szerkesztő](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Végezze el a módosításokat a kód fájlt az online szerkesztőben. A változások mentése automatikusan beírás közben.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android ügyfél quickstart útmutató]: app-service-mobile-android-get-started.md
[Apache Cordova ügyfél quickstart útmutató]: app-service-mobile-cordova-get-started.md
[iOS ügyfél quickstart útmutató]: app-service-mobile-ios-get-started.md
[Xamarin.iOS ügyfél quickstart útmutató]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android ügyfél quickstart útmutató]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms ügyfél quickstart útmutató]: app-service-mobile-xamarin-forms-get-started.md
[A Windows áruházból ügyfél quickstart útmutató]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[kapcsolat nélküli szinkronizálás]: app-service-mobile-offline-data-sync.md
[Azure Active Directory-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google-hitelesítés konfigurálása]: app-service-mobile-how-to-configure-google-authentication.md
[A Microsoft Authentication konfigurálása]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter hitelesítés konfigurálása]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure alkalmazás szolgáltatás telepítési útmutató]: ../app-service-web/web-sites-deploy.md
[Az alkalmazás Azure szolgáltatás figyelése]: ../app-service-web/web-sites-monitor.md
[Azure alkalmazás szolgáltatás a diagnosztikai naplózás engedélyezéséhez]: ../app-service-web/web-sites-enable-diagnostic-log.md
[A Visual Studióban az Azure alkalmazás szolgáltatásainak – problémamegoldás]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Adja meg a csomópont-verzió]: ../nodejs-specify-node-version-azure-apps.md
[Csomópont modulok használata]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portál]: https://portal.azure.com/
[OData]: http://www.odata.org
[Cikk megvásárlását fel]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[a GitHub basicapp minta]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[a GitHub teendő minta]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[a minták könyvtárában GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[1.1 node.js Tools for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js csomag]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014-es Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[Köztes ExpressJS]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
