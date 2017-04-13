<properties
    pageTitle="Azure alkalmazás szolgáltatás Sails.js webes alkalmazások terjesztése"
    description="Megtudhatja, hogy miként egy Azure alkalmazás szolgáltatás Node.js alkalmazás telepítéséhez. Ebből az oktatóanyagból megtudhatja, hogy hogyan Sails.js webes alkalmazások terjesztése."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="cephalin"/>

# <a name="deploy-a-sailsjs-web-app-to-azure-app-service"></a>Azure alkalmazás szolgáltatás Sails.js webes alkalmazások terjesztése

Ebből az oktatóanyagból megtudhatja, hogy hogyan Sails.js alkalmazások terjesztése Azure alkalmazás szolgáltatásba. A folyamat akkor is glean néhány általános ismeretek, hogyan kell beállítani a Node.js alkalmazás futtatásához App szolgáltatásban. 

Sails.js munka ismeretekkel kell rendelkeznie. Ebben az oktatóanyagban nem célja segítséget nyújt az általános futó Sail.js kapcsolatos problémákat.


## <a name="prerequisites"></a>Előfeltételek

- [NODE.js](https://nodejs.org/)
- [Sails.js](http://sailsjs.org/get-started)
- [Mely számjegy](http://www.git-scm.com/downloads)
- [Azure CLI](../xplat-cli-install.md)
- A Microsoft Azure-fiókjába. Ha nem rendelkeznek fiókkal, akkor [Jelentkezzen be az ingyenes próbaverzióra](/pricing/free-trial/?WT.mc_id=A261C142F) , vagy [a Visual Studio előfizetői előnyeinek aktiválása](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Mielőtt feliratkozna az Azure-fiók működésben Azure alkalmazás szolgáltatás megjelenítéséhez nyissa [alkalmazás](http://go.microsoft.com/fwlink/?LinkId=523751)szolgáltatás próbálja meg. Itt azonnal létrehozhat egy rövid életű starter alkalmazásban az alkalmazás szolgáltatás – nem kötelező hitelkártya, nincs nyilatkozatát.

## <a name="step-1-create-a-sailsjs-app-locally"></a>Lépés: 1: A helyben Sails.js alkalmazás létrehozása

Első lépésként gyorsan létre alapértelmezett Sails.js a fejlesztői környezet a következő lépésekkel:

1. Nyissa meg a parancssori terminált tetszés szerinti és `CD` munkakönyvtárat szeretne.

2. Sails.js alkalmazás létrehozása, és indítsa el:

        sails new <appname>
        cd <appname>
        sails lift

    Győződjön meg arról, hogy meg tudja nyitni az alapértelmezett kezdőlapja a http://localhost:1377.

## <a name="step-2-create-the-azure-app-resource"></a>Lépés: 2: Létrehozása az Azure alkalmazás erőforrás

Ezután hozzon létre az alkalmazás szolgáltatás erőforrás Azure-ban. Telepítse az Sails.js-alkalmazást, hogy később fogja.

1. Jelentkezzen be az Azure hasonló így:
1. Az azonos terminálablakba ASM módba módosítása, és jelentkezzen be az Azure:

        azure config mode asm
        azure login

    Kövesse a kérdésre, továbbra is a bejelentkezést a böngészőben az Azure előfizetést tartalmazó Microsoft-fiókjával.

2. Győződjön meg arról, hogy a legfelső szintű könyvtár a Sails.js projekt épp. Az alkalmazás szolgáltatás alkalmazás erőforrás létrehozása egy egyedi alkalmazás neve a következő parancs az Azure-ban. A web app URL: http://&lt;alkalmazásnév >. azurewebsites.net.

        azure site create --git <appname>

    Kövesse az Azure régió szeretne telepíteni, jelölje be a kérdés. Ha soha nem beállította mely számjegy/FTP telepítési hitelesítő adatok Azure-előfizetéséhez, is kérni fogja tabulátorok létrehozására.

    Ha az alkalmazás szolgáltatás alkalmazás erőforrás létrehozása:

    - Sails.js app, hogy mely számjegy inicializált
    - A mely számjegy inicializált tárházba csatlakozik az új alkalmazás szolgáltatási alkalmazást, egy távoli, mely számjegy szolgáltatási a "azure" nevű és
    - És a legfelső szintű címtárban iisnode.yml fájl jön létre. Ez a fájl segítségével [iisnode](https://github.com/tjanczuk/iisnode), amely Node.js alkalmazások futtatásához használ szolgáltatási alkalmazás beállítása.

## <a name="step-3-configure-and-deploy-your-sailsjs-app"></a>3 lépés: Állítsa be, és a Sails.js alkalmazások terjesztése

 Egy Sails.js alkalmazást az App szolgáltatás használata három fő lépésből áll:

 - Állítsa be az alkalmazás futtatásához App szolgáltatásban
 - Alkalmazás szolgáltatás üzembe
 - Olvasási stderr és stdout naplók telepítési problémák megoldásához

Kövesse az alábbi lépéseket:

1. Nyissa meg az új iisnode.yml fájl a legfelső szintű címtárban, és a következő két sor hozzáadása:

        loggingEnabled: true
        logDirectory: iisnode

    Naplózás engedélyezve van a iisnode. Hogyan működik a további tudnivalókért olvassa el a  [stdout és stderr naplók iisnode az első](app-service-web-nodejs-get-started.md#iisnodelog)című témakört.

2. Nyissa meg a config/env/production.js konfigurálása az üzemi környezetben, és meghatározhat `port` és `hookTimeout`:

        module.exports = {

            // Use process.env.port to handle web requests to the default HTTP port
            port: process.env.port,
            // Increase hooks timout to 30 seconds
            // This avoids the Sails.js error documented at https://github.com/balderdashy/sails/issues/2691
            hookTimeout: 30000,

            ...
        };

    Dokumentációjában találhat a konfigurációs beállítások  [Sails.js dokumentációjában](http://sailsjs.org/documentation/reference/configuration/sails-config)találhat.

    Ezután akkor győződjön meg róla, hogy [Grunt](https://www.npmjs.com/package/grunt) Azure cég hálózati meghajtók kompatibilis. 1.0.0-nél kisebb grunt verziók használja az elavult [glob](https://www.npmjs.com/package/glob) csomagok (kisebb, mint 5.0.14), amely nem támogatja a hálózati meghajtók. 

3. Nyissa meg a package.json, és módosítsa a `grunt` verzió `1.0.0` és az összes eltávolítása `grunt-*` csomagok. A `dependencies` tulajdonság így néz ki:

        "dependencies": {
            "ejs": "<leave-as-is>",
            "grunt": "1.0.0",
            "include-all": "<leave-as-is>",
            "rc": "<leave-as-is>",
            "sails": "<leave-as-is>",
            "sails-disk": "<leave-as-is>",
            "sails-sqlserver": "<leave-as-is>"
        },

3. Az package.json, adja meg a következő `engines` tulajdonságot állítsa Node.js verziót, amely szeretnénk.

        "engines": {
            "node": "6.6.0"
        },

6. A módosítások mentéséhez, és győződjön meg arról, hogy az alkalmazás továbbra is fut helyi meghajtóra, hogy a módosítások ellenőrzéséhez. Ehhez törlése a `node_modules` mappát, és kattintson a Futtatás:

        npm install
        sails lift

4. Most mely számjegy használatával telepítse az alkalmazást az Azure:

        git add .
        git commit -m "<your commit message>"
        git push azure master

5. Végül egyszerűen indítsa el az élő Azure alkalmazást a böngészőben:

        azure site browse

    Most már látnia kell a Sails.js azonos kezdőlapjára.
    
    ![](./media/app-service-web-nodejs-sails/sails-in-azure.png)

## <a name="troubleshoot-your-deployment"></a>A telepítő hibaelhárítása

Ha Sails.js alkalmazás nem sikerül egy alkalmazás szolgáltatásban valamilyen okból, keresse meg a stderr naplók hibaelhárítási azt.
További tudnivalókért olvassa el a [stdout és stderr naplók iisnode az első](app-service-web-nodejs-sails.md#iisnodelog)című témakört.
Azt sikeresen elindult, ha a stdout napló jelenjen meg a már jól ismert üzenetet:

                .-..-.

    Sails              <|    .-..-.
    v0.12.4             |\
                        /|.\
                        / || \
                    ,'  |'  \
                    .-'.-==|/_--'
                    `--'-------' 
    __---___--___---___--___---___--___
    ____---___--___---___--___---___--___-__

    Server lifted in `D:\home\site\wwwroot`
    To see your app, visit http://localhost:\\.\pipe\c775303c-0ebc-4854-8ddd-2e280aabccac
    To shut down Sails, press <CTRL> + C at any time.

Megadhatja, hogy a stdout naplók a [config/log.js](http://sailsjs.org/#!/documentation/concepts/Logging) fájlban granularitása. 

## <a name="connect-to-a-database-in-azure"></a>Csatlakozás az Azure-adatbázishoz

Adatbázishoz szeretne kapcsolódni egy Azure-ban, lehetőség az adatbázis létrehozása az Azure, például Azure SQL-adatbázissal, MySQL, MongoDB, (vgx.dll) Azure gyorsítótár, stb, és a megfelelő [adattárhoz kártya](https://github.com/balderdashy/sails#compatibility) használatához a csatlakozáshoz. Ez a szakasz lépéseit Azure-ban MySQL-adatbázishoz való csatlakozás módját mutatják.

1. Kövesse az oktatóanyag [az alábbi](../store-php-create-mysql-database.md) MySQL-adatbázis létrehozása az Azure-ban.

2. A parancssori terminálablakba a telepítse a MySQL-adaptert:

        npm install sails-mysql --save

3. Nyissa meg a config/connections.js, és a kapcsolat következő objektum hozzáadása a listához: 

        mySql: {
            adapter: 'sails-mysql',
            user: process.env.dbuser,
            password: process.env.dbpassword,
            host: process.env.dbhost, 
            database: process.env.dbname,
            options: {
                encrypt: true
            }
        },

4. Minden egyes környezeti változóba (`process.env.*`), állítsa be azt az alkalmazás szolgáltatás kell. Ehhez a terminálablakba a következő parancsokat kezdődik. Minden kapcsolat szükséges adata az Azure-portálon (lásd: [a MySQL-adatbázishoz való kapcsolódás](../store-php-create-mysql-database.md#connect)).

        azure site appsetting add dbuser="<database user>"
        azure site appsetting add dbpassword="<database password>"
        azure site appsetting add dbhost="<database hostname>"
        azure site appsetting add dbname="<database name>"
        
    A beállítások illusztráló Azure alkalmazás beállításainak továbbra is ki a verziókövetés (mely számjegy) bizalmas adatokat. Ezután fog állítsa be a fejlesztői környezet használata ugyanabban a kapcsolat adatait.

4. Nyissa meg a config/local.js, és adja hozzá a következő kapcsolatok objektumot:

        connections: {
            mySql: {
                user: "<database user>",
                password: "<database password>",
                host: "<database hostname>", 
                database: "<database name>",
            },
        },
    
    Ebben a konfigurációban felülírja a helyi környezetben config/connections.js fájljait a beállításokat. Ez a fájl nem szerepel a projekt, az alapértelmezett .gitignore szerint úgy nem fogja mely számjegy tárolható. Most Ön tud csatlakozni a MySQL-adatbázishoz, mind az Azure webalkalmazásból, és a helyi fejlesztői környezet.

4. Nyissa meg a config/env/production.js konfigurálása az üzemi környezetben, és adja hozzá a következő `models` objektum:

        models: {
            connection: 'mySql',
            migrate: 'safe'
        },

4. Nyissa meg a config/env/development.js a fejlesztői környezet beállításához, és adja hozzá a következő `models` objektum:

        models: {
            connection: 'mySql',
            migrate: 'alter'
        },

    `migrate: 'alter'`adatbázis áttelepítése szolgáltatások hozhatja létre, és egyszerűen frissítse a MySQL az adatbázistáblákban teszi lehetővé. Azonban `migrate: 'safe'` használatos (gyártás) Azure környezetben, mert Sails.js nem teszi lehetővé használandó `migrate: 'alter'` munkakörnyezetben (lásd:  [Sails.js dokumentáció](http://sailsjs.org/documentation/concepts/models-and-orm/model-settings)).

4. A terminált, a [készítése](http://sailsjs.org/documentation/reference/command-line-interface/sails-generate) Sails.js [tervrajz API](http://sailsjs.org/documentation/concepts/blueprints) Önhöz hasonló módon, majd működhetnek `sails lift` Sails.js az adatbázis áttelepítése az adatbázis létrehozásához. Példa:

         sails generate api mywidget
         sails lift

    A `mywidget` létrehozott, ezzel a paranccsal modell üres, de azt is használható, hogy van-e adatbázis-összekapcsolás megjelenítéséhez.
    Futtatásakor `sails lift`, az alkalmazás létrehozza a modellek, a hiányzó táblát használ.

6. Az imént létrehozott a böngészőben API tervrajz elérheti. Példa:

        http://localhost:1337/mywidget/create
    
    Az API térjen vissza a létrehozott bejegyzés vissza, a böngészőablakban, ami azt jelenti, hogy az adatbázis elkészült.

        {"id":1,"createdAt":"2016-09-23T13:32:00.000Z","updatedAt":"2016-09-23T13:32:00.000Z"}

5. Most Azure küldeni a a módosításokat, és tallózással keresse meg az alkalmazást, hogy továbbra is működik.

        git add .
        git commit -m "<your commit message>"
        git push azure master
        azure site browse

6. Access, az Azure-webalkalmazást a API tervrajz. Példa:

        http://<appname>.azurewebsites.net/mywidget/create

    Ha az API-t ad vissza egy másik új bejegyzést, majd az Azure webalkalmazást folytat beszélgetést a MySQL-adatbázishoz.

## <a name="more-resources"></a>További források

- [Azure App szolgáltatásban Node.js web Apps alkalmazások – első lépések](app-service-web-nodejs-get-started.md)
- [Azure alkalmazások Node.js modulok használata](../nodejs-use-node-modules-azure-apps.md)
