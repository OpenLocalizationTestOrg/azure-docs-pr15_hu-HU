<properties
    pageTitle="Azure alkalmazás szolgáltatása a vállalati szintű WordPress |} Microsoft Azure"
    description="Megtudhatja, hogy miként egy vállalati szintű WordPress webhelyet Azure alkalmazás szolgáltatása"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Vállalati szintű WordPress Azure alkalmazás szolgáltatása

Azure alkalmazás szolgáltatás misszió kritikus, nagyméretű skála [WordPress] méretezhető, biztonságos és könnyen használható környezetet biztosít[ wordpress] webhelyek. Microsoft magát fut nagyvállalati webhelyeken, például az [Office] [ officeblog] és a [Bing] [ bingblog] blogok. A dokumentum megtudhatja, hogyan használhatja Azure alkalmazás szolgáltatás Web Apps alkalmazások létrehozása és fenntartása egy vállalati szintű, felhőalapú WordPress webhely látogatói nagy mennyiségű kezelhető.

## <a name="architecture-and-planning"></a>Architektúra és megtervezése

Egy egyszerű WordPress telepítési csak két követelményei vannak.

* **MySQL-adatbázis** - [ClearDB a az Azure piactéren]elérhető[cdbnstore], és kezelheti a saját MySQL-telepítés a Azure virtuális gépeken futó vagy [Windows] használata[ mysqlwindows] vagy [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB több MySQL-konfigurációk esetén az egyes kereséskonfigurációs teljesítmény különböző tulajdonságokkal tartalmaz. Lásd: az [Azure áruházból] [ cdbnstore] szeretne rendelni, az Azure üzletben vagy közvetlenül a [ClearDB webhelyen](http://www.cleardb.com/pricing.view)látható módon a megadott információkat.

* **PHP 5.2.4-s vagy újabb** -Azure alkalmazás jelenleg szolgáltatói [PHP-verziók 5.4-es, 5,5 és 5.6][phpwebsite].

    > [AZURE.NOTE] Azt javasoljuk, hogy biztosan a legújabb biztonsági javításokat PHP legújabb verziója mindig.

### <a name="basic-deployment"></a>Egyszerű telepítési

Az alapvető követelmények használ, úgy lehetett létrehozni az Azure régión belüli egyszerű megoldást.

![egyetlen Azure régióban üzemeltetett az Azure web app és a MySQL-adatbázishoz][basic-diagram]

Ez lehetővé teszi skála meg az alkalmazást a webhely Web Apps-példányai létrehozásával, a szolgáltatás üzemelteti belül az egy adott földrajzi régióban adatközpontokban. Ezen a területen kívül a látogatók látható válaszidejű a webhely használata, ha a adatközpontokban ezen a területen nyissa le, így nem az alkalmazás.


### <a name="multi-region-deployment"></a>Több elem régió telepítési

Azure [Forgalom Manager]programmal[trafficmanager], lehetséges, ha át kívánja méretezni a a WordPress webhelyen keresztül több földrajzi régiók során csak egy URL-cím kezeléséről a látogatók. Az összes látogatók beépített részei, és adatforgalom-kezelővel, és kattintson a betöltés terheléselosztó konfigurációjának területére továbbítása.

![az Azure web app, üzemeltetett több régiókban MySQL irányítja a különböző régiók CDBR magas elérhetősége útválasztó használatával][multi-region-diagram]

Egyes régiókra belül szeretne több Web Apps-példányok között továbbra is átméretezi a WordPress webhelyet, de a méretezés régió adott; nagy forgalmat régiók megadhat alacsony forgalom melyeket másképp.

Replikációs és a továbbítás több MySQL-adatbázishoz történik ClearDB's [CDBR magas elérhetősége útválasztó] használatával[ cleardbscale] (balra látható) vagy a [MySQL-fürt CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Multimédia-tárhely és a gyorsítótár több területre telepítési
Ha a webhely feltöltések és host multimédiafájlokat elfogadja, használja az Azure Blob-tárolóhoz. Ha gyorsítótárazás van szüksége, fontolja meg a [gyorsítótár vgx.dll][rediscache], [Memcache felhőalapú](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)vagy az [Azure-tár](http://azure.microsoft.com/gallery/store/)más gyorsítótárazási ajánlataiban közül.

![az Azure web app, üzemeltetett több régiókban MySQL felügyelt gyorsítótár, Blob-tárolóhoz és CDN CDBR magas elérhetősége útválasztó használata][performance-diagram]

BLOB-tárolóhoz azért, hogy geo-elosztva régiók alapértelmezés szerint, nem kell aggódnia amiatt, hogy az összes webhelyek közötti fájlreplikálás. Az Azure [Tartalom terjesztési hálózati (CDN)] is engedélyezheti[ cdn] Blob-tárolóhoz, amely elosztja fájlok csomópontok közelebbi látogatóinak befejezéséhez.

### <a name="planning"></a>Megtervezése

#### <a name="additional-requirements"></a>További követelmények

Ehhez... | Ezzel a...
------------------------|-----------
**Fel- vagy a nagyméretű fájlok tárolása** | [WordPress beépülő modul használatával Blob-tárolóhoz][storageplugin]
**E-mail küldése** | [SendGrid] [ storesendgrid] és a [WordPress beépülő modul használatával SendGrid][sendgridplugin]
**Egyéni tartomány neve** | [Azure alkalmazás szolgáltatás a saját tartománynév beállítása][customdomain]
**HTTPS** | [Azure App szolgáltatásban webalkalmazást HTTPS engedélyezése][httpscustomdomain]
**Előzetes gyártási érvényesítése** | [Állítsa be az Azure-App szolgáltatásban webalkalmazások környezetekben átmeneti tárolására][staging] <p>Váltás a termelési is átmeneti webalkalmazást helyezi át a WordPress konfiguráció. Bizonyosodjon meg, hogy minden elérhető beállítás frissülnek a termelési alkalmazás követelményei váltása a szakaszos alkalmazás éles előtt.</p>
**Figyelés és hibaelhárítás** | [Web Apps alkalmazások Azure App szolgáltatásban a diagnosztikai naplózás engedélyezése] [ log] és [Azure App szolgáltatásban webes alkalmazásainak figyelése][monitor]
**A webhely terjesztése** | [Azure App szolgáltatásban webes alkalmazások terjesztése][deploy]

#### <a name="availability-and-disaster-recovery"></a>Elérhetőség és katasztrófa helyreállítás

Ehhez... | Ezzel a...
------------------------|-----------
**Betöltési egyensúly webhelyek** vagy **geo-webhelyek terjesztése** | [Azure forgalom Manager útvonal forgalom][trafficmanager]
**Biztonsági mentése és visszaállítása** | [Készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást] [ backup] és [Azure App szolgáltatásban webalkalmazást visszaállítása][restore]

#### <a name="performance"></a>Teljesítmény

A felhőben teljesítmény érik el elsősorban gyorsítótárba helyezése és a méretezés-ki; azonban a memória, a sávszélesség és más attribútumait Web Apps alkalmazások szolgáltatója kell tekinteni.

Ehhez... | Ezzel a...
------------------------|-----------
**Alkalmazás szolgáltatás példány lehetőségek megértése** | [Árak adatait, beleértve a szolgáltatási alkalmazás rétegek funkcióinak][websitepricing]
**Gyorsítótár-erőforrások** | [Gyorsítótár vgx.dll][rediscache], [Memcache felhőalapú](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)vagy az [Azure-tár](/gallery/store/) más gyorsítótárazási ajánlataiban közül
**Az alkalmazás méretezése** | [Azure alkalmazás szolgáltatás webalkalmazást méretezése] [ websitescale] és [ClearDB magas elérhetősége útválasztás][cleardbscale]. Ha úgy dönt, hogy a szolgáltató, és kezelheti a saját MySQL-telepítés, fontolja meg [MySQL fürt CGE] [ cge] skála-fájllal

#### <a name="migration"></a>Áttelepítési

Kétféleképp egy meglévő WordPress webhelyen áttelepítésének Azure alkalmazás szolgáltatásba.

* ** [WordPress exportálása] [ export] ** -exportálja a blogra, majd importálhatók az Azure alkalmazás szolgáltatása a [WordPress importáló beépülő modul]használatával új WordPress webhely tartalmának[import].

    > [AZURE.NOTE] Bár ez az eljárás használatával áttelepítése a tartalom, a nem áttelepíteni bármely bővítmények, témák és további változtatásokat. Ezek telepítve kell lennie kézzel újra.

* **Kézi áttelepítését** - [készítsen biztonsági másolatot a webhely] [ wordpressbackup] és az [adatbázis][wordpressdbbackup], manuálisan visszaállítja az Azure alkalmazás szolgáltatás webalkalmazást, és kapcsolódó MySQL-adatbázis áttelepítése erősen testreszabott webhelyek, és a manuális telepítésének bővítmények, témák és egyéb testreszabásokat tedium elkerüléséhez.

## <a name="step-by-step-instructions"></a>Lépésenként

### <a name="create-a-wordpress-site"></a>WordPress webhely létrehozása

1. Használja a [Microsoft Azure piactéren] [ cdbnstore] [architektúra és tervezés](#planning) szakaszában fog tárolni a webhely régió azonosítani a méret MySQL-adatbázis létrehozása.

2. Kövesse a [létrehozása az Azure alkalmazás szolgáltatás WordPress webalkalmazást] [ createwordpress] WordPress webes alkalmazás létrehozása céljából. A web app létrehozásakor válassza **Egy meglévő MySQL-adatbázishoz** , és jelölje ki az adatbázist, az 1.

Ha egy meglévő WordPress webhelyen, lásd: [Azure meglévő WordPress webhely áttelepítése](#Migrate-an-existing-WordPress-site-to-Azure) után az új webalkalmazás létrehozása.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Egy meglévő WordPress webhely áttelepítése az Azure

Az említett [architektúrája és tervezés](#planning) szakaszában, kétféleképpen WordPress webhely áttelepítése.

* **exportálása és importálása** - webhely nélkül mekkora testreszabás, vagy csak ahová tartalmát át szeretné helyezni.

* **biztonsági mentési és visszaállítási** - webhelyek testreszabása sok, amelyhez áthelyezése minden.

Az alábbi szakaszok használatával a webhely áttelepítése.

#### <a name="the-export-and-import-method"></a>A exportálása és importálása módszer

1. Használja a [WordPress exportálása] [ export] exportálása a meglévő webhelyen.

2. Hozzon létre a [WordPress webhely létrehozása](#Create-a-new-WordPress-site) szakaszban ismertetett lépések webalkalmazást.

3. Jelentkezzen be az a Web Apps alkalmazások WordPress webhelyet, és kattintson a **bővítmények** -> **Új hozzáadása**. Keresse meg, és telepítse, a **WordPress importáló** bővítményt.

4. A importáló beépülő modul telepítése után kattintson az **eszközök** -> **Importálás**, és jelölje be a **WordPress** WordPress importáló a beépülő modul használata.

5. Az **Importálás WordPress** lapon kattintson a **Fájl kiválasztása**. Tallózással keresse meg az exportált a meglévő webhelyről WordPress WXR fájlt, és válassza a **fájl feltöltése és importálni**.

6. Kattintson a **Küldés**gombra. A rendszer kéri, hogy az importálás sikeres volt.

8. Miután elvégezte ezeket a lépéseket, indítsa újra a web app lap a webhelyről az [Azure portál][mgmtportal].

Importálja a webhelyet, után szükség lehet a következő lépésekkel ahhoz, hogy az importálási fájl nem található beállítások.

Ha ez... | Művelet
------------------ | ----------
**Idehivatkozások** | Az új webhely WordPress irányítópult lapon kattintson a **Beállítások** -> **idehivatkozások** és majd frissítés idehivatkozások szerkezetét
**kép/media hivatkozások** | A hivatkozások frissítésének új helyét, használja a [bársony kékek frissítése URL-címeit modul][velvet], a Keresés és csere eszközben, vagy manuálisan az adatbázis
**Témák** | Nyissa meg a **Megjelenés** -> **téma** és frissítése a webhely témáját, szükség szerint
**Menük** | Ha a témát támogatja a menük, hivatkozások és a Kezdőlap lap továbbra is előfordulhat, hogy a régi alszint könyvtár beágyazva őket. Nyissa meg a **Megjelenés** -> **menük** és frissítheti az adataikat

#### <a name="the-backup-and-restore-method"></a>A biztonsági mentés és visszaállítás módszer

1. Készítsen biztonsági másolatot a meglévő WordPress információk felhasználásával [WordPress biztonsági másolatok]a webhely[wordpressbackup].

2. Készítsen biztonsági másolatot a meglévő adatbázist, [az adatbázis biztonsági másolatának]információk felhasználásával[wordpressdbbackup].

3. Hozzon létre egy adatbázist, és a biztonsági mentés visszaállítása.

    1. Új adatbázis vásárolja meg a [Microsoft Azure piactéren][cdbnstore], vagy [Windows] rendszerű MySQL-adatbázishoz beállítása[ mysqlwindows] vagy [Linux] [ mysqllinux] virtuális.

    2. MySQL ügyfélprogram, például a [MySQL-munkaterület][workbench], az új adatbázis csatlakoztatása a és a WordPress adatbázis importálása.

    3. Az adatbázis módosítása a tartomány bejegyzések új Azure alkalmazás szolgáltatás tartománya frissítéséhez. Ha például mywordpress.azurewebsites.net. A [Keresés és csere WordPress adatbázisok parancsfájl] [ searchandreplace] biztonságosan módosítása az összes előfordulását.

4. Egy webalkalmazás létrehozása az Azure-portálon, és tegye közzé a WordPress biztonsági másolatot.

    1. Egy webalkalmazás létrehozása az [Azure portál] [ mgmtportal] használatával **Új**adatbázist tartalmazó -> **webes + Mobile** -> **Azure piactéren elérhető** -> **Web Apps alkalmazások** -> **Web app + SQL** (vagy **webalkalmazás + MySQL**) -> **Létrehozás**. Állítsa be a szükséges beállításokat hozzon létre egy üres web App alkalmazásban.

    2. A biztonságimásolat-WordPress keresse meg a **wp-config.php** fájlt, és nyissa meg a-szerkesztőben. A következő bejegyzéseket cserélje ki az adatokat az új MySQL-adatbázis.

        * **DB_NAME** – a felhasználó nevét, az adatbázis

        * **DB_USER** – a felhasználó nevét, használja a hozzáférést az adatbázishoz

        * **DB_PASSWORD** – a felhasználó jelszavát

        Után módosítani szeretné ezeket a bejegyzéseket, mentse és zárja be a **wp-config.php** fájlt.

    3. Használja a [Deploy Azure App szolgáltatásban webalkalmazást] [ deploy] információk ahhoz, hogy a telepítési mód ki szeretne használni, és telepítheti a WordPress biztonsági másolatot a web App alkalmazás Azure szolgáltatásban.

5. Miután a WordPress webhely lett telepítve, látnia kell eléréséhez, az új webhely (meg az App szolgáltatás web app) használata a *. a webhely azurewebsite.net URL-CÍMÉT.

### <a name="configure-your-site"></a>A webhely beállítása

Miután a WordPress webhelyen létrehozott vagy áttelepített, használja az alábbi információkat teljesítmény javítása, illetve további funkciók engedélyezése.

Ehhez... | Ezzel a...
------------- | -----------
**Alkalmazás szolgáltatás terv mód, méretét és engedélyezése méretezési beállítása** | [Azure alkalmazás szolgáltatás webalkalmazást méretezése][websitescale]
**Állandó az adatbázis-kapcsolatokat engedélyezése** | Alapértelmezés szerint az WordPress nem használja az állandó adatbázis-kapcsolatot okozhat a kapcsolat az adatbázishoz való több kapcsolat után válnak szabályozott. Ahhoz, hogy az állandó kapcsolatok, telepítse a (állandó kapcsolatok kártya bővítményt) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**Teljesítmény javítása** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">Tiltsa le a ARR cookie</a> - javíthatja a teljesítményt több Web Apps-példányok WordPress forgalmi</p></li><li><p>Gyorsítótárazása. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Gyorsítótár vgx.dll</a> (előzetes verzió) kínál az <a href="https://wordpress.org/plugins/redis-object-cache/">objektum-gyorsítótár WordPress beépülő modul vgx.dll</a>, vagy az <a href="/gallery/store/">Azure-tár</a> más gyorsítótárazási ajánlataiban közül</p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Hogyan lehet WordPress gyorsabb a Wincache</a> - Wincache alapértelmezés szerint engedélyezve van Web Apps alkalmazások</p></li><li><p><a href="../web-sites-scale/">Azure App szolgáltatásban webalkalmazást skála</a> és <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB magas elérhetősége útválasztás</a> vagy <a href="http://www.mysql.com/products/cluster/">MySQL fürt CGE</a> használata</p></li></ul>
**Tárterület BLOB használata** | <ol><li><p><a href="../storage-create-storage-account/">Azure tároló fiók létrehozása</a></p></li><li><p>További tudnivalók <a href="../cdn-how-to-use/">A tartalom terjesztési hálózati (CDN)</a> használatát mikéntjéről geo-BLOB tárolt adatok terjesztése.</p></li><li><p>Telepítése és konfigurálása az <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure tároló WordPress bővítmény</a>.</p><p>Részletes telepítési és konfigurációs adatait a beépülő modul a <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">felhasználói útmutatóban</a>című témakörben.</p> </li></ol>
**E-mailek engedélyezése** | Engedélyezze az Azure-tárolót használó <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> . A <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid beépülő modul</a> telepítése WordPress.
**A saját tartománynév beállítása** | [Azure alkalmazás szolgáltatás a saját tartománynév beállítása][customdomain]
**Egyéni tartománynév HTTPS engedélyezése** | [Azure App szolgáltatásban webalkalmazást HTTPS engedélyezése][httpscustomdomain]
**Egyensúly vagy geo betöltése-webhelyéhez terjesztése** | [Irányítja a forgalmat Azure forgalom Manager][trafficmanager]. Ha egy egyéni tartományt használ, olvassa el az [Azure-App szolgáltatásban a saját tartománynév beállítása] [ customdomain] információkat forgalom segítségével egyéni tartománynevek használata
**Az automatikus biztonsági mentés engedélyezése** | [Készítsen biztonsági másolatot az Azure alkalmazás szolgáltatás webalkalmazást][backup]
**Diagnosztikai naplózás engedélyezése** | [Web Apps alkalmazások Azure App szolgáltatásban a diagnosztikai naplózás engedélyezése][log]

## <a name="next-steps"></a>Következő lépések

* [WordPress optimalizálása](http://codex.wordpress.org/WordPress_Optimization)

* [Azure App szolgáltatásban többhelyes funkció WordPress átalakítása](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB Azure varázsló frissítése](http://www.cleardb.com/store/azure/upgrade)

* [Az Azure-App szolgáltatásban a web App almappában üzemeltetési WordPress](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Lépésenkénti útmutató: Azure használatával WordPress webhely létrehozása](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [A meglévő WordPress blogjának Azure a szolgáltató](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [A WordPress közérthető idehivatkozások engedélyezése](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Áttelepítése és futtatása a WordPress blogjának Azure alkalmazás szolgáltatása](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Az ingyenes az Azure alkalmazás szolgáltatása WordPress futtatása](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [A két percen belül vagy annál kisebb Azure WordPress](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [WordPress blog Azure - 1 rész áthelyezése: Azure WordPress blog létrehozása](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Azure - 2 rész áthelyezése WordPress blog: a tartalom átvitele](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Azure - 3 rész áthelyezése WordPress blog: az egyéni tartomány beállítása](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [WordPress blog áthelyezése az Azure - kijelző 4: közérthető idehivatkozások és az URL-cím Átírásának szabályok](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Azure - 5 rész áthelyezése WordPress blog: áthelyezése almappába a legfelső szintű](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Hogyan WordPress webalkalmazást az Azure-fiók beállítása](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [A Azure WordPress felfelé propping](http://www.johnpapa.net/wordpress-on-azure/)

* [Tippek a Azure WordPress](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
