<properties
    pageTitle="Egy webalkalmazás létrehozása a Microsoft Azure piactéren lévő |} Microsoft Azure"
    description="Megtudhatja, hogyan hozhat létre egy új WordPress web app a Microsoft Azure piactéren lévő az Azure-portálon."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>Egy webalkalmazás létrehozása az Azure piactérről

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

A Microsoft Azure piactéren elérhetővé teszi a népszerű web Apps alkalmazások Microsoft, külső cégek és Megnyitás szoftver kezdeményezések által fejlesztett széles köre. Ha WordPress, Umbraco CMS, Drupal, például stb. A web Apps alkalmazások számos népszerű keretek, például [PHP] e WordPress példa, a [.NET], [Node.js], [Java]és [Python]néhány nevet a készültek. Egy webalkalmazás létrehozása a Microsoft Azure piactéren lévő csak szoftver van szüksége van, amely az [Azure-portálon]használja a böngészőben.

Ebből az oktatóanyagból megismerheti, hogy miként:

* Keresse meg és webalkalmazás létrehozása az Azure alkalmazás szolgáltatás, amely az Azure Piactérről származó sablonon alapuló.
* Az új webalkalmazás Azure alkalmazás szolgáltatás beállításainak konfigurálása
* Indítsa el, és kezelheti a web App alkalmazásban.

Ebben az oktatóanyagban céljából telepíti a Microsoft Azure piactéren lévő WordPress webnaplót tartalmazó webhelyen. Miután elvégezte a lépéseket, ebben az oktatóanyagban, is be a saját WordPress webhely és futtatása a felhőben.

![Példa WordPress wep alkalmazás irányítópult][WordPressDashboard1]

Ebben az oktatóanyagban fogja rendszerbe WordPress webhely MySQL használja az adatbázist. Ha inkább az adatbázis használata SQL-adatbázis szeretne tulajdonságairól [Projekt Nami], amely is a Microsoft Azure piactéren elérhető.

> [AZURE.NOTE]
> Oktatóprogram elvégzéséhez a Microsoft Azure-fiókra van szüksége. Ha nem rendelkeznek fiókkal, akkor [a Visual Studio előfizetői előnyeinek aktiválása] [ activate] [Regisztráljon az ingyenes próbaverzióra], vagy[free trial].
>
> Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg]. Innen azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban – nem hitelkártya szükség, illetve hogy nincs nyilatkozatát.

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>Keresse meg és webalkalmazást létrehozása az Azure alkalmazás szolgáltatás

1. Jelentkezzen be az [Azure-portálon].

1. Kattintson az **Új**gombra.
    
    ![Hozzon létre egy új Azure erőforrást][MarketplaceStart]
    
1. **WordPress**keresni, és kattintson a **WordPress**. (SQL-adatbázis használata helyett MySQL szeretné, keresse meg **A Project Nami**.)

    ![Kattintson a piactér WordPress keresése][MarketplaceSearch]
    
1. A leírás WordPress alkalmazás elolvasása, után kattintson a **Létrehozás**gombra.

    ![WordPress webalkalmazás létrehozása][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>Az új webalkalmazást Azure alkalmazás szolgáltatás beállításainak konfigurálása

1. Miután létrehozott egy új web App alkalmazásban, a WordPress a beállítások lap jelenik meg, milyen, hajtsa végre az alábbi lépéseket:

    ![WordPress web app beállításainak konfigurálása][ConfigStart]

1. Írja be egy nevet a web app a **Web app** mezőbe.

    Ezt a nevet a azurewebsites.net tartományban egyedinek kell lennie, mert a web app URL-címe lesz *{nevű}*. azurewebsites.net. Ha a név nem egyedi, piros felkiáltójel jelenik meg, a szövegmezőbe.

    ![Állítsa be a WordPress web app neve][ConfigAppName]

1. Ha egynél több előfizetése van, válassza a használni kívántat. 

    ![Az előfizetés a webalkalmazás konfigurálása][ConfigSubscription]

1. Jelöljön ki egy **Erőforráscsoport** , vagy hozzon létre egy újat.

    További információt a erőforrás csoportok [áttekintése]című témakörben találhat Azure erőforrás-kezelő[ResourceGroups].

    ![Az erőforráscsoport a webalkalmazás konfigurálása][ConfigResourceGroup]

1. Jelölje be az **Alkalmazás szolgáltatás terv/helyét** , vagy hozzon létre egy újat.

    További információt a App milyen szolgáltatáscsomagok [áttekintése]című témakörben találhat Azure alkalmazás szolgáltatás csomagok[AzureAppServicePlans]. 

    ![A szolgáltatás csomagot a webalkalmazás konfigurálása][ConfigServicePlan]

1. Kattintson az **adatbázist**, és kattintson az **Új MySQL-adatbázis** lap az adja meg a szükséges értékeket a MySQL-adatbázis konfigurációs.

    egy. Írjon be egy új nevet, vagy hagyja az alapértelmezett nevet.

    b. Hagyja meg a **megosztott** **Adatbázis típusát** .

    c billentyűkombinációt. Válassza az ezen a helyen, mint amit a webalkalmazásban.

    d. Válasszon egy árak réteg. **Higany** – vagyis a minimális kapcsolatok és a szabad lemezterület ingyenes – nem kell aggódnia az ebben az oktatóanyagban.

    e. Az **Új MySQL-adatbázis** lap fogadja el a jogi szerződést, és kattintson **az OK**gombra. 

    ![Az adatbázis a web app beállításainak konfigurálása][ConfigDatabase]

1. A **WordPress** lap fogadja el a jogi szerződést, és kattintson a **Létrehozás**gombra. 

    ![Fejezze be a web app beállításai, és kattintson az OK gombra][ConfigFinished]

    Azure alkalmazás szolgáltatás hoz létre a web app általában egy perc alatt. A portáloldalon tetején harang ikonra kattintva megtekinthet végrehajtását.

    ![Folyamatjelző][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Indítsa el, és kezelheti a WordPress web App alkalmazásban
    
1. A webhely-alkalmazás létrehozása befejeződése után nyissa meg az erőforrás-csoportba, amelyben az alkalmazás létrehozása az Azure-portálon, és megjelenik a web app és az adatbázis.

    A további erőforrások az lámpa ikon az [Alkalmazás az összefüggéseket][ApplicationInsights], amely a webalkalmazás felügyeleti szolgáltatásokat nyújt.

1. Az **erőforráscsoport** lap kattintson a web app sor.

    ![Jelölje ki a WordPress web App alkalmazásban][WordPressSelect]

1. A Web app lap, a **Tallózás**gombra.

    ![Tallózással keresse meg a WordPress web App alkalmazásban][WordPressBrowse]

1. Ha a program kéri, válassza ki a nyelvet a WordPress blogokban, válassza ki a kívánt nyelvet, és kattintson a **Folytatás**gombra.

    ![A nyelvet a WordPress webalkalmazás konfigurálása][WordPressLanguage]

1. WordPress **Üdvözöljük** lapon adja meg a konfiguráció szükséges WordPress, és válassza a **Telepítés WordPress**.

    ![A WordPress webalkalmazás beállításainak konfigurálása][WordPressConfigure]

1. Jelentkezzen be a hitelesítő adatok hozta létre az **Üdvözöljük** lapon.  

1. A webhely Irányítópultlap nyithatók meg és a megadott információk jeleníthetők meg.    

    ![A WordPress irányítópult megtekintése][WordPressDashboard2]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban láthatta, hogyan hozhat létre, és a Microsoft Azure piactéren lévő Példa webes alkalmazás telepítéséhez.

Alkalmazás szolgáltatás Web Apps alkalmazások használata a további tudnivalókért lásd: a hivatkozások (széles böngészőablakot) lap bal oldalán vagy (keskeny böngészőablakot) lap tetején.

A Azure WordPress web Apps alkalmazások fejlesztésével kapcsolatos további tudnivalókért lásd: [Azure alkalmazás szolgáltatás fejlesztése WordPress][WordPressOnAzure]. 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[NODE.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[Próbálja meg az App szolgáltatás]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure portál]: https://portal.azure.com/
[Projekt Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
