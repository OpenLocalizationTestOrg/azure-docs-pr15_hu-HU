<properties
  pageTitle="Hatékony használata DevOps környezetekben a web App alkalmazásban"
  description="Megtudhatja, hogy miként telepítési helyek beállítása és kezelése az alkalmazás több fejlesztői környezet segítségével"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>A web Apps alkalmazások hatékony DevOps környezetekben használata

Ebből a cikkből megtudhatja, hogy beállítása és kezelése, webhely-alapú telepítés alkalmazás az alkalmazást, például a fejlesztői, kérdések és válaszok, és munkakörnyezeti több változatát. Az alkalmazás minden verzió fejlesztői környezet az adott segítségre van szüksége, a telepítési folyamatot belül tekinthető meg. Például kérdések és válaszok környezet használható a fejlesztők csoport tesztelheti az alkalmazás minőségének, mielőtt a termelési leküldéses, a módosításokat.
Több fejlesztési környezetek beállításáról lehet a nehéz feladat nyomon követése, az erőforrások (számítási, webalkalmazás, adatbázis, gyorsítótár stb.), és helyezhetnek üzembe a kód támogatása környezetekben szükség szerint.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Nem-munkakörnyezetben (szakaszban, a fejlesztői, a kérdések és válaszok) beállítása
Ha befejezte a termelési webalkalmazást felfelé és fut, következő lépésként nem munkakörnyezetben létrehozásához. Használatához a telepítési helyek ellenőrizze, hogy a **normál** vagy a **prémium** alkalmazás szolgáltatás terv módban futtatja. Telepítési helyek ténylegesen élő web Apps alkalmazások, a saját állomásnevekké. Web app tartalom- és konfigurációs elemei között két telepítési helyek, beleértve a termelési tárolóhely is kell cserélni. Az alkalmazást a telepítés tárolóhely üzembe helyezése a következő előnyöket nyújtja:

1. Ellenőrizheti egy átmeneti tárolásra szolgáló telepítési tárolóhely web app változásai lecserélése, és a gyártási tárolóhely előtt.
2. Először webalkalmazást bevezetéshez egy tárolóhely és éles lecserélése azt ellenőrzi, hogy a a tárolóhely összes előfordulását előtt éles cserélni folyamatban van bemelegíteni. Ezzel a legrövidebb leállás elkerülhető, amikor rendszerbe állítják a web App alkalmazásban. A forgalom átirányítása gördülékeny, és nincs kérések szövegdobozokban miatt felcserélése műveletek nem jelennek. A teljes munkafolyamat automatizálható konfigurálása [Automatikus felcserélése](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) előtti felcserélése érvényesítése nem szükségessége esetén.
3. Után egy felcserélése a tárolóhely korábban szakaszos webalkalmazást összegyűjtötte az előző gyártási web App alkalmazásban. Ha nem a várt módon a módosításokat a gyártási tárolóhely be cserélni, végezheti el az azonos felcserélése közvetlenül a "utolsó ismert jó webalkalmazás" megszerezni vissza.

Egy átmeneti tárolásra szolgáló telepítési tárolóhely beállítani, olvassa el a [web Apps alkalmazások Azure App szolgáltatásban környezetek átmeneti beállítása](web-sites-staged-publishing.md)című témakört. Minden környezetben tartalmaznia kell erőforrások, a saját csoportja számára, ha a web app a példában egy adatbázist, majd a gyártási, mind az átmeneti tárolásra szolgáló webalkalmazás kell más adatbázisokat használt. Adja hozzá adatbázis, a tárhely vagy a gyorsítótár az átmeneti tárolásra szolgáló fejlesztői környezet beállításához, például átmeneti tárolásra szolgáló fejlesztői környezet erőforrásokat.

## <a name="examples-of-using-multiple-development-environments"></a>Példák a több fejlesztői környezet segítségével

Olyan projektek egy adatforrás-kód kezelés legalább két környezetben, a fejlesztés és munkakörnyezeti környezet, de amikor Content management rendszerekből, alkalmazás keretek stb azt esetleg fellépő esetleges hibák hol az alkalmazás nem támogatja a beépített ebben az esetben célszerű követnie. Ez a népszerű keretek itt tárgyalt egyes igaz. Kérdések rengeteg állnak rendelkezésre a CMS/keretek például használata

1. Hogyan bontsa ki a különböző környezetekben
2. Milyen fájlokat lehet módosíthatja és átméretez keretrendszer verziófrissítések hatással
3. Egy környezet konfigurációja kezelése
4. Modulok és bővítmények verziófrissítések core keretrendszer verziófrissítések kezelése

Számos módon állíthat be a több környezet beállítása a projekthez, és az alábbi példák csak egy ilyen módja a saját alkalmazások.

### <a name="wordpress"></a>WordPress
Ebben a részben megtanulhatja, hogy miként állíthatja be a helyek használatának WordPress telepítési munkafolyamat. Például a legtöbb CMS megoldások WordPress nem támogatja a beépített több fejlesztői környezet használata. Alkalmazás szolgáltatás Web Apps tartalmaz néhány funkciók, amelyek megkönnyítik a konfigurációs beállítások a kód kívüli tárolni.

Mielőtt hoz létre egy átmeneti tárolásra szolgáló tárolóhely, állítsa be az alkalmazás-kódot több környezetek támogatására. Több környezetek támogatására WordPress módosítani szeretne a `wp-config.php` a helyi fejlesztési web App hozzá a következő kódot a fájlt az elején. Ezzel lehetővé teszi, hogy az alkalmazás válassza ki a megfelelő konfigurációja, a kijelölt környezet alapján.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Hozzon létre egy mappát a web app legfelső szintű nevű `config` és vegyen fel egy fájl két: `wp-config.azure.php` és `wp-config.local.php` rendre, amely a azure és a helyi környezetben.

Másolja a következő `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

A fenti biztonsági kulcsokat beállítás is megakadályozza, hogy a webalkalmazás éppen megtámadott súgó, ezért egyedi értékeket. Ha szeretne létrehozni fent említett biztonsági kulcsok karakterláncát, láthatja el az automatikus nyilvántartás-készítő alkalmazás hozhat létre új kulcsok/értékeket ez [hivatkozás] (https://api.wordpress.org/secret-key/1.1/salt) használata

Másolja a következő kódot a `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Relatív elérési utak használata
Egy utolsó megoldás, ha a WordPress alkalmazást használ a relatív utak konfigurálása. WordPress URL-cím adatokat tárolja az adatbázist. Így áthelyezése a másikra nehezebben, módosítania kell minden alkalommal, amikor, váltani helyi szakaszban, vagy a gyártási környezetekben szakaszban az adatbázis egyik környezetből tartalom. Az adatbázis telepítésével, minden alkalommal, amikor rendszerbe állítják egyik környezetből egy másik használata a [legfelső relatív hivatkozások beépülő modul](https://wordpress.org/plugins/root-relative-urls/) WordPress rendszergazdai irányítópult használata lehet telepíteni vagy kézi letöltése az [alábbi](https://downloads.wordpress.org/plugin/root-relative-urls.zip)okozó problémák a kockázatok csökkentésére.


A következő bejegyzéseket felvenni a `wp-config.php` előtt fájl a `That's all, stop editing!` Megjegyzés:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

A beépülő modul segítségével aktiválása a `Plugins` WordPress rendszergazdai irányítópult menüjére. WordPress app idehivatkozás beállításainak mentéséhez.

#### <a name="the-final-wp-configphp-file"></a>Az utolsó `wp-config.php` fájl
Minden WordPress Core frissítése nem befolyásolja a `wp-config.php`, `wp-config.azure.php` és `wp-config.local.php` fájlokat. A végén ez hogyan `wp-config.php` fájl így jelenik meg.

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
Feltételezve, hogy már rendelkezik az Azure webes, jelentkezzen be [Azure Management előzetes portáljára](http://portal.azure.com) futó WordPress webalkalmazást, és nyissa meg a WordPress web App alkalmazásban. Alkalmazások esetén nem hozhat létre egyet a piactérről. Bővebb, [kattintson ide](web-sites-php-web-site-gallery.md).
Kattintson a beállítások-telepítés > Helyek -> Hozzáadás egy telepítési tárolóhely létrehozását a név szakaszban. A telepítési tárolóhely egy másik, ugyanazokat az erőforrásokat, az előbb létrehozott elsődleges web App megosztás webalkalmazás.

![Szakasz telepítési tárolóhely létrehozása](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Tegyük fel hozzáadása egy másik MySQL-adatbázis `wordpress-stage-db` az erőforrás csoportba `wordpressapp-group`.

 ![Erőforráscsoport MySQL-adatbázis hozzáadása](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

A szakasz telepítési tárolóhely újonnan létrehozott adatbázis, mutasson a kapcsolat karakterláncokat frissítése `wordpress-stage-db`. Jegyzet, hogy a gyártási web app, `wordpressprodapp` és az átmeneti tárolásra szolgáló web app `wordpressprodapp-stage` különböző adatbázisok kell mutatnia.

#### <a name="configure-environment-specific-app-settings"></a>Környezetfüggő alkalmazás beállításainak konfigurálása
A fejlesztők karakterlánc kulcs – érték párokká tárolhatja Azure a konfigurációs adatok nevű alkalmazás beállításainak webalkalmazást társított részeként. Futásidőben a alkalmazás szolgáltatás Web Apps alkalmazások automatikusan olvassa be a következő értékeket meg, és a webalkalmazásban futó kód számára elérhetővé teszi őket. Az értékpapír perspektivikus, amely a szép oldala összekapcsolhatók óta bizalmas információkat, például a csatlakozási_karakterlánc adatbázis jelszóval soha nem jelenik meg a fájl egyszerű szövegként például `wp-config.php`.

Ezt a folyamatot, az alábbiakban meghatározott akkor hasznos, amikor elkészíti a mind fájl és az adatbázis módosítása WordPress alkalmazás tartalmaz:
- WordPress verziófrissítés
- Új hozzáadása, szerkesztése vagy bővítmények frissítése
- Új hozzáadása, szerkesztése vagy témák frissítése

Alkalmazás beállításainak megadása:

- az adatbázis-beállításai
- Kapcsolja be- és kikapcsolása WordPress naplózás
- WordPress biztonsági beállítások

![Wordpress web app alkalmazás beállításainak](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Győződjön meg arról, hogy hozzáadta a termelési web app és a szakasz tárolóhely az a következő alkalmazás beállítások. Figyelje meg, hogy a gyártási web app és a fejlesztői web app használata különböző adatbázisok.
Törölje a jelet WP_ENV kivételével az összes beállítások paraméterekkel **Tárolóhely beállítás** jelölőnégyzetet. Ez lesz felcserélése a webalkalmazás fájl tartalmának és az adatbázis együtt az adatokat. **Tárolóhely beállítás** **bejelölve**, ha a web app alkalmazás beállításainak és a kapcsolati karakterlánc konfigurációs nem helyezi át környezetekben egy FELCSERÉLÉSE művelet során, és így adatbázis módosításokat Ha ez nem megszakítja a termelési web App alkalmazásban.

A helyi fejlesztői környezet web app üzembe szakasz web app és az adatbázis WebMatrix vagy eszközök, például az FTP, mely számjegy vagy PhpMyAdmin lehetőség használatával.

![Webalkalmazás WordPress webes mátrix közzététele párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Tallózással keresse meg, és tesztelje az átmeneti tárolásra szolgáló webalkalmazást. Példa frissíteni kell a web App alkalmazásban a téma esetén, figyelembe véve az alábbiakban az átmeneti tárolásra szolgáló web App alkalmazásban.

![Tallózással keresse meg az átmeneti tárolásra szolgáló webalkalmazás előtt helyek lecserélése](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Ha az összes elégedett az eredménnyel, kattintson a az átmeneti tárolásra szolgáló webalkalmazást a tartalom áthelyezése a termelési környezetén **felcserélése** gombjára. Ebben az esetben, megjelenítését a web app és az adatbázis támogatása környezetekben minden **felcserélése** művelet során.

![Előzetes módosításokat felcserélése WordPress számára](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Ha forgatókönyvön, ahol szükséges csak leküldéses fájlokat (nincs adatbázis frissítések), majd **Jelölje be** a **Tárolóhely beállítás** az összes adatbázis kapcsolódó *alkalmazás beállításainak* és *karakterláncok kapcsolatbeállításokat* a web app beállítása lap belül az Azure előzetes portálon a FELCSERÉLÉSE végrehajtása előtt. Ez esetben DB_NAME DB_HOST, DB_PASSWORD, DB_USER alapértelmezett csatlakozási karakterlánc nem jelenjen meg az előnézeti módosításokat **felcserélése**során. At ezúttal végrehajtása a **felcserélése** művelet a WordPress web app lesz a frissítéseket a fájlok **csak**.

Mielőtt egy FELCSERÉLÉSE módon, az alábbiakban a termelési WordPress web app ![gyártási webalkalmazás előtt helyek lecserélése](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

A csere művelet után a téma frissítette a termelési web App.

![Gyártási webalkalmazás után helyek lecserélése](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

Helyzetben kell **vonhatók vissza**, ha, ugorjon a termelési web app beállításai, és kattintson a **Csere** gombra megjelenítését a web app és az átmeneti tárolásra szolgáló tárolóhely gyártási adatbázist. Ne feledje, hogy egy fontos tudnivaló, hogy az adott időpontban **felcserélése** művelettel szerepelnek a módosításokat, majd a későbbiekben újra az átmeneti tárolásra szolgáló web App alkalmazásban, meg kell telepíteni az adatbázis terjesztése értékűre módosul az aktuális adatbázisban az átmeneti tárolásra szolgáló webalkalmazás, amelyek lehetnek az előző éles adatbázis vagy a szakasz adatbázis.

#### <a name="summary"></a>Összefoglalás
A folyamat bármely alkalmazásból az adatbázis generalize való

1. A helyi környezet alkalmazás telepítése
2. Olyan környezetben adott konfigurációs (helyi és Azure Web App)
3. Állítsa be az alkalmazás szolgáltatás Web Apps alkalmazások – átmeneti gyártási a környezete
4. Ha egy gyártási alkalmazást már Azure, szinkronizálja a helyi és fejlesztői környezet gyártási tartalmak (fájlok/kód + adatbázis).
5. Az alkalmazás a helyi környezet kialakítása
6. A gyártási webalkalmazás karbantartási-zárolt üzemmód és szinkronizálási adatbázis előkészítése és a fejlesztők környezetben gyártási tartalom elhelyezése
7. Fejlesztői környezet és a vizsgálat terjesztése
8. Munkakörnyezetben telepítése
9. Ismételje meg a 4-es és 6

### <a name="umbraco"></a>Umbraco
Ebben a szakaszban megismerheti, hogyan a Umbraco CMS használja a különböző több DevOps környezetben üzembe egy egyéni modulra. Ez a példa egy másik megközelítés több fejlesztői környezet biztosít.

[Umbraco CMS](http://umbraco.com/) az egyik popular.NET CMS megoldásokkal sok fejlesztők által használt nyújt [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulban való telepítéséhez fejlesztési átmeneti gyártási környezetekkel foglalkozik. A helyi fejlesztői környezet egy Umbraco CMS webalkalmazás Visual Studio vagy WebMatrix használatával egyszerűen hozhat létre.

1. Hozzon létre egy Umbraco webalkalmazás Visual Studióban, [kattintson ide](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Az Umbraco web app létrehozását WebMatrix, [kattintson ide](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Ne felejtsen eltávolítása a `install` mappa csoportban az alkalmazást, és soha ne töltse fel szakaszban, vagy gyártási web Apps alkalmazások. Az ebben az oktatóanyagban lehet fogja használni WebMatrix

#### <a name="set-up-a-staging-environment"></a>A fejlesztői környezet beállítása
- Hozzon létre egy telepítési tárolóhely, Umbraco CMS webalkalmazás, feltételezve, hogy a már van egy Umbraco CMS webalkalmazás és futtatása a fent említett. Ha nem hozhat létre egyet a piactérről.

- Frissítse a szakasz telepítési tárolóhely mutasson az újonnan létrehozott adatbázis, **umbraco-szakasz**kapcsolati karakterláncát. A gyártási web app (umbraositecms-1) és az átmeneti tárolásra szolgáló web app (umbracositecms-1-terület) **kell** más adatbázisokat mutasson.

![Kapcsolati karakterlánc web app új átmeneti adatbázis átmeneti tárolására szolgáló frissítése](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Kattintson a telepítés tárolóhely **szakasz** **első Publish settings** . Visual Studio vagy a webes mátrix Azure web App helyi fejlesztési web app alkalmazásból az alkalmazás közzététele szükséges összes információ tárolható közzétételi beállítások fájl letöltése.

 ![Az átmeneti tárolásra szolgáló web App beállítása első közzététele](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Nyissa meg a helyi fejlesztési webalkalmazás **WebMatrix** vagy **Visual Studio**. Ebben az oktatóanyagban webes mátrix használok, és először fel kell a közzététel az átmeneti tárolásra szolgáló webalkalmazás beállításainak-fájl importálása

![Import használata a webes mátrix Umbraco Közzététel beállításai](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- A párbeszédpanel változásainak áttekintéséhez, és a helyi web app telepítése az Azure-webalkalmazást, *umbracositecms-1-szakaszban*. Amikor rendszerbe állítják fájlok közvetlenül az átmeneti tárolásra szolgáló webalkalmazást lévő fájlok kihagy-e a `~/app_data/TEMP/` mappát, ezek fogja hozni a szakasz web app esetén első lépések. Is kell paramétert a `~/app_data/umbraco.config` fájlként, túl, újra kell generálni.

![Közzétételi webhely mátrix változásainak áttekintéséhez](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Után sikeresen közzététele a Umbraco helyi web app webalkalmazás átmeneti tárolására, keresse meg az átmeneti tárolásra szolgáló webalkalmazást, és néhány kizárja az esetleges problémák vizsgálatok futtatása.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 telepítési modul beállítása
[Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul tartalom, a stíluslapok, fejlesztési modulok is leküldéses, és további egy egyszerű, kattintson a jobb gombbal az átmeneti tárolásra szolgáló webalkalmazást gyártási web App alkalmazásban, egy további teljesen ingyenes telepítések és megszakítása a termelési web App alkalmazásban, ha a frissítés telepítése a kockázatok csökkentése.
Licenc beszerzési Courier2 a tartományhoz tartozó `*.azurewebsites.net` és az egyéni tartományát (mondjuk http://abc.com) Miután megvásárolta a licenc, helyezze a letöltött licenc (. LIC-fájl) az az `bin` mappát.

![Húzza a licenc fájlt tároló mappában](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Töltse le a Courier2 csomag [Itt](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Jelentkezzen be a szakasz web App alkalmazásban, http://umbracocms-site-stage.azurewebsites.net/umbraco mondja ki, és kattintson a **Fejlesztőeszközök** menü, valamint a Select **csomagok**. Kattintson a helyi csomag **telepítéséhez**

![A telepítő Umbraco csomag](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Töltse fel a courier2 csomagot használ, a telepítő.

![Courier modulhoz csomag feltöltése](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Konfigurálandó, frissítenie kell courier.config fájlt a web App **Config** mappában.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

A `<repositories>`, írja be a gyártási webhely URL-CÍMEK és a felhasználói adatokat. Alapértelmezett Umbraco tagsági szolgáltató használatakor majd ezt hozzáadhatja a felügyeleti felhasználó azonosítója <user> szakaszban. Ha egyéni Umbraco tagsági szolgáltató használata esetén `<login>`,`<password>` Courier2 modulhoz megtudni, hogy miként csatlakozhat nyomda. További részletekért tekintse át a [dokumentáció](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) Courier modulhoz.

Hasonlóképpen Courier modul telepítése a termelési webhelyen és beállítása szakaszban webalkalmazás mutasson az annak megfelelő courier.config fájlt itt ismertetett módon

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Kattintson a Umbraco CMS web app irányítópult Courier2 fülre, majd válassza a helyek. Meg kell jelennie a tárházba neve említett `courier.config`. Ehhez a gyártási és web Apps alkalmazások átmeneti.

![Nézet céltárházba web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Lehetővé teszi tartalomterjesztés néhány átmeneti tárolásra szolgáló webhelyről gyártási webhelyre. Nyissa meg a tartalmat, és jelölje ki a meglévő lap, vagy új lap létrehozása. Lehet egy meglévő lapon jelölje ki a web App alkalmazásban, ahol a lap címére változik **Első lépések – új** , és kattintson a **Mentés és közzététel gombra**.

![Módosíthatja a lap címét és közzététel](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Most jelölje ki az módosított lapra, és *kattintson a jobb gombbal* kattintva megtekintheti az összes beállításait. Kattintson a **Courier** telepítési párbeszédpanel megtekintéséhez. Kattintson a **Deploy** telepítési kezdeményezése

![Courier modul telepítésének párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Tekintse át a módosításokat, majd kattintson a Folytatás gombra.

![Változások áttekintése Courier-modul és-telepítési párbeszédpanel](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Telepítési naplója azt mutatja meg, ha sikeres volt-e a telepítést.

 ![Telepítési naplók megtekintése Courier modulból](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Tallózással keresse meg a megjelenítéséhez, ha a változtatások megjelenjenek a termelési web App alkalmazásban.

 ![Tallózással keresse meg a gyártási web App alkalmazásban](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Courier használatával kapcsolatos további információért olvassa el a dokumentációt.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Umbraco CMS verziójú frissítés menete

Courier nem segít üzembe helyezéséhez Umbraco cm egy verziót frissít a másikba. Umbraco CMS verzió frissítésekor ki kell választani az kompatibilitási problémák a egyéni modulokat vagy harmadik fél modulok és a Umbraco Core tárak. A legjobb

1. MINDIG készítsen biztonsági másolatot a web app és az adatbázis-frissítés végrehajtása előtt. Azure Web App, állíthat be az automatikus biztonsági mentés-webhelyek, a biztonsági másolat használata Ez a funkció, valamint a webhely visszaállítása Ha szükség használatával visszaállíthatja szolgáltatás. További információra kíváncsi olvassa el a [biztonsági másolatának a web app](web-sites-backup.md) és [a web app visszaállítása](web-sites-restore.md)című témakört.

2. Jelölje be a külső csomagok esetén kompatibilisek-e a verziót frissít. A csomag töltse le a lapot, tekintse át a Project kompatibilitás Umbraco CMS verzióval.

A web app helyi frissítésével kapcsolatos további tájékoztatásért említett [Itt](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general), hajtsa végre az az útmutatásokat.

Miután a helyi fejlesztői webhely frissítését, a módosításainak közzététele átmeneti tárolására web App alkalmazásban. Az alkalmazás tesztelése, és ha a minden elégedett az eredménnyel, használja a **felcserélése** gomb **felcserélése** gyártási web App alkalmazásban a átmeneti tárolásra szolgáló webhely. A **felcserélése** művelet végrehajtásakor az webalkalmazást konfigurációs hatással lehet a módosítások tekinthet meg. Ezzel a **felcserélése** művelettel azt is lecserélése a web Apps alkalmazások és az adatbázisok. Ez azt jelenti, a csere a termelési web app fog mutatni umbraco-szakasz-db adatbázis és átmeneti tárolásra szolgáló webalkalmazás umbraco-termékkatalógus-db adatbázis fog mutatni.

![Felcserélése előzetes Umbraco CMS üzembe helyezése](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

A web App alkalmazásban és az adatbázis lecserélése előnye:
1. Lehetőséget nyújt az alkalmazás hibát tapasztal esetén visszaállíthatja a web app másik **felcserélése** előző verzióját.
2. Frissítés szükséges fájlok és átmeneti tárolására webalkalmazás gyártási web App alkalmazásban és az adatbázis-adatbázis terjesztése. Sok dolgot elromolhatnak a fájlok és az adatbázis telepítésekor. Helyek **felcserélése** funkcióval, hogy csökkentheti az állásidőt a frissítés során, és a módosítások telepítésekor fellépő hibák a kockázat csökkentése.
3. Lehetővé teszi a teendő **A és B vizsgálat** [gyártási tesztelése](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funkció használata

Ebben a példában láthatók a rugalmasság a platform Umbraco Courier modul telepítésének kezelése támogatása környezetekben hasonló egyéni modulok vagy.

## <a name="references"></a>Hivatkozások
[Agilis szoftverfejlesztési Azure alkalmazás szolgáltatással](app-service-agile-software-development.md)

[Állítsa be az Azure-App szolgáltatásban webalkalmazások környezetekben átmeneti tárolására](web-sites-staged-publishing.md)

[Hogyan tiltható le a web access a nem éles üzemi helyek](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
