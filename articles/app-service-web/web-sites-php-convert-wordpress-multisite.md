<properties 
    pageTitle="Azure App szolgáltatásban többhelyes funkció WordPress átalakítása" 
    description="Megtudhatja, hogy miként készíthet egy meglévő WordPress web App alkalmazásban létrehozott keresztül a gyűjtemény Azure-ban, és alakítsa WordPress többhelyes funkció" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>Azure App szolgáltatásban többhelyes funkció WordPress átalakítása

## <a name="overview"></a>– Áttekintés

*[Zsolt Lobaugh]által[ben-lobaugh], [A Microsoft Open technológiák Inc.][ms-open-tech]*

Ebből az oktatóanyagból megtanulhatja, hogyan készíthet egy meglévő WordPress web App alkalmazásban – a gyűjtemény Azure-ban létrehozott, és egy WordPress többhelyes funkció telepítése alakíthatja. Ezenkívül fog megtudhatja, hogy miként társíthat egyéni tartományt minden az alwebhelyek belül a telepítéskor.

Feltételezzük, hogy egy már telepített WordPress. Ha nem, kövesse az útmutatást [a gyűjteményből az Azure WordPress webhely]létrehozása[website-from-gallery].

Egy meglévő WordPress konvertálása többhelyes funkció webhely telepítés általában meglehetősen egyszerű, és a kezdeti lépéseket számos származik, közvetlenül a [Hálózat létrehozása] [ wordpress-codex-create-a-network] a [WordPress Codex](http://codex.wordpress.org)lapján.

Lássunk hozzá.

## <a name="allow-multisite"></a>Többhelyes funkció engedélyezése

Először engedélyeznie kell többhelyes funkció révén a `wp-config.php` fájl a **WP\_engedélyezése\_többhelyes funkció** állandó. A web app-fájlokat szeretne szerkeszteni, két módszer: az egyik FTP- és a második és mely számjegy keresztül. Ha nem ismeri beállítása, kövesse az alábbi módszerek egyikét, olvassa el az alábbi oktatóanyagok:

* [A MySQL- és FTP PHP-webhelyen][website-w-mysql-and-ftp-ftp-setup]

* [A MySQL- és mely számjegy PHP-webhelyen][website-w-mysql-and-git-git-setup]

Nyissa meg a `wp-config.php` a szerkesztő a fájlt, és adja hozzá a következő, a fenti a `/* That's all, stop editing! Happy blogging. */` sor.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Feltétlenül mentse a fájlt, és töltse fel újra a kiszolgáló!

## <a name="network-setup"></a>Hálózat beállítása

Bejelentkezés a webes *wp-rendszergazdai* területére alkalmazást, és láthatók az **eszközök** menü nevű a **Hálózat beállítása**az új elemet. Kattintson a **Hálózat beállítása** gombra, és töltse ki a hálózat részleteit.

![Hálózati beállítási képernyőn][wordpress-network-setup]

Ebben az oktatóprogramban az *almenüből könyvtárak* webhely séma használ, mert kell mindig működnek, és azt szeretné állítani az egyes alwebhely egyéni tartományok belül az oktatóprogram. Jó helyen jár lehetővé válik és altartományt telepítse, ha egy tartományt az [Azure-portál](https://portal.azure.com) és -beállítási helyettesítő DNS keresztül megfelelően megfeleltetése beállítást.

Kapcsolatban további információk a alszint tartomány viewben alkönyvtár mátrixok [többhelyszínű hálózati típusú] [ wordpress-codex-types-of-networks] a WordPress Codex jelentkező.

## <a name="enable-the-network"></a>A hálózati engedélyezése

A hálózat most már be van állítva az adatbázisban, de egy hátralévő lépésben ahhoz, hogy a hálózati funkciókat. Véglegesítése a `wp-config.php` beállítások, valamint `web.config` megfelelően az egyes webhely irányítja.


A *Hálózat beállítása* lapon a **telepítés** gombra kattint, WordPress megpróbálja frissíteni a `wp-config.php` és `web.config` fájlokat. Azonban mindig ellenőrizni kell a fájlokat, annak érdekében, hogy a frissítés sikeres volt. Ha nem, a képernyő jelenítsen meg a szükséges módosításokat az. Szerkesztheti és mentheti a fájlokat.


Elvégzése után az alábbi frissítéseket kell jelentkezzen, majd jelentkezzen be a wp-rendszergazdai irányítópult biztonsági.

Most már kell egy további menüben a rendszergazda címkézett **Saját helyek**sáv. A menü lehetővé teszi, hogy új hálózatához, és a **Hálózati rendszergazda** irányítópult szabályozható.

## <a name="adding-custom-domains"></a>Egyéni tartomány hozzáadása

Az [WordPress MU tartomány Mapping] [ wordpress-plugin-wordpress-mu-domain-mapping] beépülő modul egy egyéni tartományok hozzáadása a hálózat minden olyan webhely Gyerekjáték teszi. Ahhoz, hogy megfelelően működjön a bővítményt végezze el a portálon és a tartományregisztrálónál néhány további beállítási szükséges.

## <a name="enable-domain-mapping-to-the-web-app"></a>A web app tartomány hozzárendelésének engedélyezése

Az **ingyenes** [Alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) terv mód egyéni tartományok hozzáadása Web Apps alkalmazások nem támogatja. Szüksége lesz **megosztva** vagy **normál** módra válthat. Ennek módja:

* Jelentkezzen be az Azure-portálra, és keresse meg a web App alkalmazásban. 
* Kattintson a **Beállítások** **méretezni** lapon.
* Az **Általános**csoportban jelölje be a *megosztott* és a *szokásos*
* Kattintson a **Mentés** gombra.

Előfordulhat, hogy megjelenik egy üzenet kéri, hogy ellenőrizze a módosítása, és igazolja, hogy a webalkalmazás előfordulhat, hogy most költségkezelési, attól függően, hogy a használati és a többi konfigurációs beállíthatja.

Az új beállítások feldolgozása néhány másodpercet vesz igénybe most pedig egy jó a tartomány beállításának megkezdése.

## <a name="verify-your-domain"></a>A tartomány tulajdonjogának igazolása

Mielőtt Azure Web Apps lehetővé teszi, hogy a tartomány feleltesse meg a webhelyet, először kell ellenőrizheti, hogy az engedély hozzárendelése a tartományt. Ehhez hozzá kell adnia egy új CNAME rekordot a DNS-bejegyzés.

* Jelentkezzen be a tartomány DNS-kezelőben
* Hozzon létre egy új CNAME *awverify*
* Mutasson a *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

Eltarthat egy kis időt, a DNS-módosítások teljes életbe, így ha az alábbi lépésekkel nem azonnal, lépjen a tölteni, kávézóból, hogy térjen vissza és, majd próbálkozzon újra.

## <a name="add-the-domain-to-the-web-app"></a>A tartomány felvétele a web App alkalmazásba

Térjen vissza a web App alkalmazásban – az Azure-portálra, kattintson a **Beállítások**gombra, és kattintson az **egyéni tartományok és az SSL**.

Ha az *SSL-beállítások* jelennek meg, hol szeretne hozzárendelni a web App alkalmazásban, amely az összes tartományok fog beviteli mezők megjelenik. Ha tartomány nem szerepel itt, azt nem lesz elérhető a leképezéshez belül WordPress, függetlenül attól, hogy a tartomány DNS beállítása.

![Egyéni tartományok kezelése][wordpress-manage-domains]

Miután a tartományhoz a szövegmezőbe, Azure ellenőrzi a korábban létrehozott CNAME rekordot. Ha a DNS teljesen nem vonatkoznak, egy piros jelölő jeleníti meg. Ha sikeres volt, látni fogja a zöld pipa jelzi. 

Ajánljuk figyelmébe az IP-cím szerepel a listában, kattintson a párbeszédpanel alján. Ez az A rekordot a tartomány beállítása szüksége lesz.

## <a name="setup-the-domain-a-record"></a>A tartomány rekord beállítása

Sikeres volt a lépéseket, ha előfordulhat, hogy most rendel a tartomány DNS A rekord keresztül az Azure webalkalmazást. 

Fontos megjegyzés: Itt Azure web Apps alkalmazások fogadja el CNAME, mind A rekordok, azonban be *kell* használni az A rekord ahhoz, hogy megfelelő tartomány megfeleltetés. A CNAME nem kell juttatni egy másik CNAME, amely olyan, milyen Azure által létrehozott, a YOUR_DOMAIN.azurewebsites.net.

Az előző lépésben az IP-cím használata esetén a DNS-kezelőben vissza, és az A rekordot, mutasson az adott IP beállítása.


## <a name="install-and-setup-the-plugin"></a>Telepítse és állítsa be a beépülő modul

WordPress többhelyes funkció jelenleg nem rendelkezik beépített módszert a tartományait. Azonban van, akkor a beépülő modul nevű [WordPress MU tartomány Mapping] [ wordpress-plugin-wordpress-mu-domain-mapping] , amely hozzáad a használható funkciók körét. Jelentkezzen be a webhely hálózati rendszergazda részét, és telepítse a **WordPress MU tartomány leképezése** bővítményt.

Miután telepítésével és aktiválásával a beépülő modul, látogasson el a **Beállítások** > **Tartomány leképezése** konfigurálása a bővítményt. Az első mezőbe a *Kiszolgáló IP-címét*, adja meg az IP-címet használta az A rekordot a tartomány beállítása. Kívánt *Tartomány beállítások* (az alapértelmezett mérete gyakran finom), és kattintson a **Mentés**gombra.

## <a name="map-the-domain"></a>Feleltesse meg a tartomány

Keresse fel az **Irányítópult** -szeretne hozzárendelni kívánt webhely. Kattintson az **eszközök** > **Tartomány megfeleltetése** és az új tartomány írja be a szövegmezőbe, és kattintson a **Hozzáadás**gombra.

Alapértelmezés szerint az új tartomány az automatikusan létrehozott webhely tartománynak kell írni. Ha azt szeretné, hogy minden forgalom az új tartomány küldött, jelölje be *a bloghoz elsődleges tartomány* mentés előtt. A tartományok tetszőleges számú hozzáadása a webhelyhez, de csak egy elsődleges lehet.

## <a name="do-it-again"></a>Végezze el ismét

Azure Web Apps lehetővé teszi a tartományok korlátlan szám hozzáadása a webalkalmazást. További tartomány felvétele szüksége lesz az **a tartomány ellenőrzése** és **a tartományhoz egy rekord beállítása** szakaszok minden egyes tartományhoz végrehajtása.  

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók kezdéshez Azure alkalmazás szolgáltatással, nyissa meg a [Próbálja alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Útmutató a módosítása a webhelyekre alkalmazás szolgáltatás című: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
