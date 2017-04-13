<properties
    pageTitle="Építse és üzembe microservices előre Azure-ban"
    description="Megtudhatja, hogy miként tevődik össze microservices egységként Azure App szolgáltatásban és a JSON erőforrás Felhasználócsoport-sablonok és PowerShell-parancsfájlokat kiszámíthatóan módon alkalmazások telepítése."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Építse és üzembe microservices előre Azure-ban #

Ebből az oktatóanyagból megtudhatja, hogy miként kiépítése és tevődik össze [microservices](https://en.wikipedia.org/wiki/Microservices) [Azure alkalmazás szolgáltatás](/services/app-service/) egységként és JSON erőforrás csoport sablonok és PowerShell-parancsfájlokat kiszámíthatóan módon alkalmazások telepítése. 

Amikor a kiépítési, és tevődnek össze leválasztott nagyon magas szintű-alkalmazások telepítése microservices, ismételhetősége és előreláthatóság roppant fontos sikeres. [Azure alkalmazás szolgáltatás](/services/app-service/) lehetővé teszi, hogy hozzon létre microservices web Apps alkalmazások, a mobilalkalmazások, a API-alkalmazások és az alkalmazások logika tartalmazó. [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) lehetővé teszi, hogy minden microservices kezelheti egy egységként, az erőforrás-függőségek, például adatbázisból együtt, és a forrás-vezérlők beállításainak. Most például az alkalmazás JSON-sablonok és egyszerű PowerShell-parancsfájlokat is telepítheti. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Mit érhetek ##

Az oktatóprogram során telepíti az alkalmazást, amely tartalmazza:

-   Két web apps (azaz a két microservices)
-   Egy kódmentes SQL-adatbázis
-   Alkalmazás beállításainak, a kapcsolati karakterláncot és az adatforrás-vezérlő
-   Alkalmazás hírcsatornájában, értesítések, autoscaling beállításai

## <a name="tools-you-will-use"></a>Használandó eszközök ##

Ebben az oktatóanyagban de a következő eszközök kell használni. Mivel nem teljes vitafórum az eszközök, fogom a végpontok közötti forgatókönyv szigorúan, és csak ad egy rövid bevezető minden, és ha talál bővebb felvilágosítást. 

### <a name="azure-resource-manager-templates-json"></a>Azure erőforrás-kezelő sablonok (JSON) ###
 
Minden alkalommal, amikor az Azure alkalmazás szolgáltatás webalkalmazást hoz létre, például Azure erőforrás-kezelő JSON sablont használ az erőforrás teljes csoport létrehozásához a összetevő erőforrásokkal. A [Microsoft Azure piactéren](/marketplace) a, mint a [Méretezhető WordPress](/marketplace/partners/wordpress/scalablewordpress/) alkalmazást is elhelyezhet a MySQL-adatbázishoz, tároló fiókok, az alkalmazás szolgáltatáscsomagja, a saját webalkalmazás összetett sablon riasztási szabályok, alkalmazás beállításainak, Automatikus méretezéssel beállítások és a sablonok és az összes további találhatók Powershellen keresztül. Töltse le és a sablonok című témakörben olvashat [Azure PowerShell használatá Azure erőforrás-kezelővel](../powershell-azure-resource-manager.md)olvashat.

Az erőforrás-kezelő Azure sablonok kapcsolatos további tudnivalókért olvassa el a [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md) című témakört.

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 for Visual Studio ###

A legújabb SDK csomagjában talál az erőforrás-kezelő sablon támogatja a JSON-szerkesztőben javított tartalmazza. Ezzel a paranccsal gyorsan egy erőforrás csoport sablon létrehozása üres lapból, vagy nyissa meg a meglévő JSON sablon (például egy letöltött gyűjtemény sablon) módosításra, a paraméterek fájl feltöltése és még a az erőforráscsoport közvetlenül egy erőforráscsoport Azure megoldást üzembe.

További tudnivalókért lásd: [Azure SDK 2.6 for Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 vagy újabb verzió ###

Verzió 0.8.0 kezdve, az Azure PowerShell telepítése az Azure erőforrás-kezelő modul mellett az Azure modul tartalmazza. Ez a modul lehetővé teszi, hogy az erőforrás csoportok telepítését parancsfájl.

További tudnivalókért lásd: [Azure PowerShell használatá a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure erőforrás Explorer ###

Az [Előnézet eszköz](https://resources.azure.com) lehetővé teszi, hogy az előfizetése, és az egyes erőforrások az erőforrás-csoportok a JSON-definíciók feltárása. A eszközben a JSON-definíciók egy erőforrás szerkesztése, törlése az erőforrások teljes hierarchia, és hozzon létre új erőforrásokat.  Könnyen elérhető az eszközben látható nagyon hasznos sablon létrehozása, mert azt jelzi, hogy mely tulajdonságokat kell beállítania egy bizonyos típusú erőforrás, a megfelelő értékek, stb. Az erőforrás csoport létrehozása az [Azure-portálon](https://portal.azure.com/), majd nézze meg a JSON-definíciók segít az erőforráscsoport templatize explorer eszközében is.

### <a name="deploy-to-azure-button"></a>Telepítse az Azure gomb ###

Adatforrás-vezérlő GitHub használ, ha be a fontos elhelyezheti a [Deploy Azure gombra](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) . MD, amely lehetővé teszi az UI Azure bekapcsolása billentyűs telepítés. Ezt megteheti a minden olyan egyszerű webalkalmazás, miközben bővítése engedélyezheti egy teljes erőforráscsoport üzembe úgy, hogy helyezi a tárházba legfelső szintű azuredeploy.json fájlként. A JSON-fájllal, ez az erőforrás csoport sablont tartalmaz, az erőforráscsoport létrehozása fog használható a Deploy Azure gombra. Egy példa című témakörben a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) minta használandó ebben az oktatóanyagban.

## <a name="get-the-sample-resource-group-template"></a>A minta erőforrás csoport sablon beszerzése ##

Most pedig hozzuk működésbe a megfelelő rá.

1.  Nyissa meg a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) alkalmazás szolgáltatás minta.

2.   Readme.md kattintson **az Azure Deploy**.
 
3.  Éppen írt [üzembe-a-azure](https://deploy.azure.com) -webhelyre, és kéri, hogy a bemeneti paramétereket telepítési. Figyelje meg, hogy a mezőket a legtöbb van feltöltve tárházba nevét és az egyes véletlen karakterláncok meg. Az összes mező módosíthatja, ha azt szeretné, de a csak dolog, amit meg kell adnia, hogy az SQL Server felügyeleti bejelentkezés, és a jelszót, majd kattintson a **Tovább**gombra.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Ezután kattintson a **központi telepítés** a telepítés megkezdéséhez. Miután a folyamat befejezése fut, kattintson a http://todoapp*XXXX*. azurewebsites.net hivatkozásra kattintva keresse meg a telepített alkalmazások. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    A felhasználói felület volna lassúnak kissé során, először tallózással keresse meg, mert az alkalmazások csak indítását, de vennie saját magát, hogy a teljes funkcionalitású alkalmazások.

5.  Vissza a központi telepítés lapon kattintson a **kezelése** hivatkozásra kattintva megtekintheti az új alkalmazás az Azure-portálon.

6.  A **Essentials** tartalmazó legördülő listára kattintson az erőforrás csoport hivatkozásra. Is vegye figyelembe, hogy a web app már csatlakoztatva van a **Külső projekt**GitHub tárházba. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Az erőforrás csoport lap a látható, hogy már két web Apps alkalmazások és az erőforráscsoport egy SQL-adatbázishoz.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
A szolgáltatás csak látott rövid néhány perc van egy teljesen telepített két – microservice alkalmazás az összes összetevők, függőségek, beállítások, adatbázist, és folytonos közzététel, állítsa be az automatikus üzletifolyamat-tervező az Azure erőforrás-kezelő. Mindezt két dolgot által végrehajtott:

-   A központi telepítés Azure gomb
-   a legfelső szintű repó azuredeploy.JSON

Ez az alkalmazás terjeszthet tízesre, több száz vagy időpontokat ezer és pontos konfigurációja azonos minden alkalommal. A megismételhetőség és eljárás a kiszámítható lehetővé teszi, hogy a magas szintű alkalmazások terjesztése könnyű és megbízhatóság.

## <a name="examine-or-edit-azuredeployjson"></a>Vizsgálja meg (vagy szerkesztése) AZUREDEPLOY. JSON ##

Most Vegyük szemügyre milyen lett beállítva a GitHub tárat. Lesz a JSON-szerkesztőben az Azure .NET SDK, ha még nem telepítette az [Azure .NET SDK 2.6](/downloads/), tegye a következőt.

1.  A [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tár a Kedvencek mely számjegy eszközzel klónozhatja. Az alábbi képernyőképen lehet vagyok ezzel a csapat Intéző a Visual Studio 2013-ban.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Nyissa meg a tárházba legfelső szintű azuredeploy.json a Visual Studióban. Ha a JSON Vázlat ablaktábla nem látható, akkor telepítenie kell Azure .NET SDK csomagjában talál.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Nem fogom ismertetik a JSON formátum minden részlet, de a [További erőforrások](#resources) szakaszban az használatának elsajátításához az erőforrás csoport sablon nyelvi hivatkozásokat tartalmaz. Ebben az esetben csak most mutatja, hogy a érdekes funkciók, amelyek segíthetnek tétele az alkalmazások telepítésének saját egyéni sablon használatának megkezdéséhez.

### <a name="parameters"></a>Paraméterek ###

Nézze meg a paraméterek csoportban láthatja, hogy ezek a paraméterek táblázatparancsok nagy része Mi az **Azure a központi telepítés** gomb kéri, hogy a szövegbeviteli. A webhelyen a **Deploy az Azure** gomb mögött feltölti a bemeneti a azuredeploy.json megadott paramétereket alkalmaz a felhasználói felület. Ezek a paraméterek előfordulnak az erőforrás-definíciókat, például az erőforrások nevei, tulajdonságértékeket stb.

### <a name="resources"></a>Erőforrások ###

Az erőforrások csomópontra akkor láthatja, hogy a 4 legfelső szintű erőforrások meg vannak adva, például SQL Server-példányt, az alkalmazás szolgáltatáscsomagja és két web Apps alkalmazások. 

#### <a name="app-service-plan"></a>Alkalmazás szolgáltatáscsomagja ####

Kezdjük a JSON egyszerű gyökérszintű az erőforráshoz. A JSON vázlatot kattintson az alkalmazás szolgáltatáscsomagja **[hostingPlanName]** nevű jelölje ki a megfelelő JSON-kódot. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Megjegyzendő, hogy a `type` elem azt adja meg a karakterlánc (azt hívták kiszolgálófarm korábbi hosszú, hosszú idő) alkalmazás szolgáltatás verzió esetében más elemek és a Tulajdonságok ki vannak töltve a JSON-fájlban definiált paraméterekkel és az erőforráshoz nincs olyan beágyazott erőforrásokat.

>[AZURE.NOTE] Érdemes megjegyezni is, hogy az értéke `apiVersion` Ez az Azure használni a JSON erőforrás meghatározása a REST API-t, és melyik verziója befolyásolhatják, milyen az erőforrás belül formázása kell-e a `{}`. 

#### <a name="sql-server"></a>Az SQL Server ####

Ezután kattintson az SQL Server-erőforrásnév **SQL Server** a JSON tagolás.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Megjegyzés: a kiemelt JSON-kódot az alábbiakat:

-   Paraméterek használata biztosítja, hogy a létrehozott források nevű, beállítva, ha követik egymást készíti őket.
-   Az SQL Server erőforrásnak megvan-e két beágyazott erőforrások, minden más értékkel rendelkezik `type`.
-   A beágyazott erőforrások belül `“resources”: […]`, ahol az adatbázist, és a tűzfalszabályokat meg van határozva, van egy `dependsOn` elem, amely meghatározza az erőforrás-azonosító a legfelső szintű SQL Server erőforrás. Ez azt jelenti Azure erőforrás-kezelő, az "Ez az erőforrás, amely már léteznie kell, más erőforrásra; létrehozása előtt és ha a sablont, hogy az erőforrás van definiálva, hozza létre, hogy az egyik előbb".

    >[AZURE.NOTE] Részletes információkat használatáról a `resourceId()` fog működni, lásd: [Azure erőforrás-kezelő sablon függvények](../resource-group-template-functions.md).

-   Hatása a `dependsOn` elem, hogy Azure erőforrás-kezelő tudja, milyen erőforrások párhuzamosan hozhatja létre, és mely erőforrások egymás után kell létrehozni. 

#### <a name="web-app"></a>Web App alkalmazásban ####

Most vegyük lépjen tovább a tényleges web Apps alkalmazások maguk, hogy melyik bonyolultabb. Kattintson a [variables('apiSiteName')] web app a JSON tagolás, jelölje ki a JSON-kódot. Észre fogja venni, hogy dolog, amit első sokkal több érdekes. Erre a célra lehet szót nyújtott lehetőségekről a funkciók egyesével:

##### <a name="root-resource"></a>Legfelső szintű erőforrás #####

A web app attól függ, hogy két különböző erőforrásokat. Ez azt jelenti, hogy csak az alkalmazás szolgáltatáscsomagja és az SQL Server-példányt is létrehozása után Azure erőforrás-kezelő hoz létre a web App alkalmazásban.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Alkalmazás beállításainak #####

Az alkalmazás beállításait is meg van határozva, beágyazott erőforrásként.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

Az a `properties` elemet `config/appsettings`, két alkalmazás beállításainak formátumban van `“<name>” : “<value>”`.

-   `PROJECT`az [KUDU beállítást](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , amely arra utasítja az Azure telepítés mely projekt használata a Visual Studio több projekt megoldást. E kell követnie később adatforrás-vezérlő beállításaitól, de mivel a ToDoApp kódot több projekt Visual Studio megoldást, ez a beállítás szükséges.
-   `clientUrl`az egyszerűen beállítás, amely az alkalmazás kódja alkalmazást használ.

##### <a name="connection-strings"></a>Kapcsolati karakterlánc #####

A kapcsolati karakterláncot, beágyazott erőforrásként is meg van határozva.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

Az a `properties` elemet `config/connectionstrings`, minden kapcsolati karakterlánc is definíciója név: érték pár, az adott formátumának `“<name>” : {“value”: “…”, “type”: “…”}`. Az a `type` elem lehetséges értékei `MySql`, `SQLServer`, `SQLAzure`, és `Custom`.

>[AZURE.TIP] A kapcsolati karakterlánc típusú végleges listáját, a következő parancsot a Azure PowerShellben: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Adatforrás-vezérlő #####

Az adatforrás-vezérlő beállítások is meg van határozva, beágyazott erőforrásként. Azure erőforrás-kezelő használja az erőforrás folyamatos közzétételének konfigurálása (bővítve lássanak `IsManualIntegration` később) és is, hogy az alkalmazás kódja a JSON-fájl feldolgozása közben automatikusan telepítését elindításához.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`és `branch` meglehetősen intuitív kell lennie, és a mely számjegy tárat, és szeretné közzétenni a fiók nevét kell mutatnia. Ismét ezek határozza meg a bemeneti paramétereket. 

A Megjegyzés: a `dependsOn` elem mellett a web app erőforrás magát, hogy `sourcecontrols/web` is függ, hogy `config/appsettings` és `config/connectionstrings`. Ennek oka, hogy miután `sourcecontrols/web` van konfigurálva, a Azure telepítési folyamatot automatikusan megpróbálja telepíthető, összeállítása és indítsa el az alkalmazás kódja. Emiatt a függőség beszúrása segít győződjön meg arról, hogy az alkalmazás hozzáférjen a szükséges alkalmazás beállításainak és a kapcsolati karakterláncot az alkalmazás kódja futása előtt. 

>[AZURE.NOTE] Érdemes megjegyezni is, hogy `IsManualIntegration` értéke `true`. Ezt a tulajdonságot oka az, ebben az oktatóanyagban szükség ténylegesen nem rendelkezik a GitHub tárat, és így nem ténylegesen engedélyt Azure [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) a folyamatos közzétételének (azaz konfigurálása leküldéses a frissítések automatikus tárházba való Azure). Használhatja az alapértelmezett érték `false` a megadott tárhoz, csak akkor, ha a tulajdonos GitHub hitelesítő adatok beállította az [Azure portál](https://portal.azure.com/) előtt. Ez azt jelenti állított be minden alkalmazás az [Azure-portálon](https://portal.azure.com/) GitHub vagy BitBucket adatforrás-vezérlő korábban, ha a felhasználó hitelesítő adataival Azure fog ne feledje, hogy a hitelesítő adatait, majd felhasználhassa őket bármely GitHub vagy BitBucket alkalmazás a jövőben rendszerbe. Jó helyen jár Ha még nem tette meg, JSON sablon telepítési sikertelen lesz, ha Azure erőforrás-kezelő próbál konfigurálni az adatforrás-vezérlő beállítások a web app, mert azt nem lehet bejelentkezni a GitHub vagy BitBucket tárházba tulajdonosa hitelesítő adatokkal.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>A telepített erőforráscsoport JSON-sablonnal összehasonlítása ##

Ebben az esetben a web app minden pengéit az [Azure-portálon](https://portal.azure.com/)keresztül láthatja el, de másik eszközt, amely csak, akkor hasznos, ha nem szeretne. Nyissa meg az [Azure erőforrás Explorer](https://resources.azure.com) előzetes eszközt, amely lehetővé teszi az erőforrás-csoportokat JSON ábrázolása a foglalt ténylegesen az Azure kódmentes létezik. Az erőforráscsoport JSON-hierarchia az Azure hogyan felel meg a sablonfájl létrehozásához használt a hierarchiában is megjelenik.

Például az [Azure erőforrás Explorer](https://resources.azure.com) eszköz megnyitásához, és bontsa ki a csomópontokat az Explorer ugyanitt láthatók az erőforráscsoport és a megfelelő erőforrástípus alapján begyűjtési gyökérszintű erőforrásokat.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Akkor hatoljon le a megfelelő web App alkalmazásban, ha látnia kell web app konfigurációs részletek hasonlóan, mint a képernyőfelvétel alatt:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Ismét a beágyazott erőforrások van, hogy nagyon hasonlít a JSON sablonfájl a hierarchiát, és meg kell jelennie a alkalmazás beállításainak, a kapcsolati karakterláncot, a megfelelően tükröződik a JSON ablaktábla stb. Az itt látható beállítások hiányában jelezheti a problémát a JSON-fájllal, és segíthet a JSON sablonfájl elhárításában.

## <a name="deploy-the-resource-group-template-yourself"></a>Az erőforrás csoport sablon saját maga terjesztése ##

A **központi telepítés az Azure** gomb kiválóan, de lehetővé teszi, hogy az erőforrás-csoport sablon azuredeploy.json üzembe, csak akkor, ha már tolni azuredeploy.json GitHub. Az Azure .NET SDK is eszközeivel telepíthet bármely JSON sablonfájl közvetlenül a helyi számítógépre. Ehhez hajtsa végre az alábbi lépéseket:

1.  A Visual Studióban, kattintson a **fájl** > **Új** > **Projekt**.

2.  Kattintson a **Visual C#** > **felhő** > **Azure erőforráscsoport**, kattintson **az OK**gombra.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  **Azure-sablon kiválasztása**jelölje ki az **Üres sablon** , és kattintson az **OK gombra**.

4.  Azuredeploy.json húzza az új projekt a **sablonok** mappába.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Nyissa meg a másolt azuredeploy.json megoldás Explorerben.

6.  Csak az annak szemléltetése, nézzük néhány szabványos alkalmazás betekintést erőforrások hozzáadása a JSON-fájl **Erőforrás hozzáadása**gombra kattintva. Ha érdekli csak az üzembe helyezése a JSON-fájlt, ugorjon a központi telepítés lépéseket.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Jelölje ki az **Alkalmazást az összefüggéseket Web Apps alkalmazások**, majd ellenőrizze, hogy egy meglévő alkalmazás szolgáltatást tervezése és webalkalmazás választógombot, és kattintson a **Hozzáadás**gombra.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Most is több új forrásokban talál, amely attól függően, hogy az erőforrás, és mit jelent, az alkalmazás szolgáltatáscsomagja vagy a web app táblává. Ezek az erőforrások meglévő kategóriákról szerint nincs engedélyezve, és szeretné módosítani, amely.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Kattintson a JSON vázlat **appInsights Automatikus méretezéssel** jelölje ki a JSON-kódot. Ez az alkalmazás szolgáltatás csomagja a méretezési beállítása.

9.  A kiemelt JSON kódot, keresse meg a `location` és `enabled` tulajdonságokat és az alább látható módon állíthatja be.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. Kattintson a JSON vázlat **CPUHigh appInsights** jelölje ki a JSON-kódot. Ez az értesítés.

11. Keresse meg a `location` és `isEnabled` tulajdonságokat és az alább látható módon beállítani őket. Tegye ugyanezt a többi három értesítések (lila hagymák).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Most már készen áll a üzembe. Kattintson a jobb gombbal a projektet, és válassza ki a **központi telepítés** > **Új telepítési**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Jelentkezzen be az Azure-fiókjába, ha még nem tette.

14. Meglévő erőforráscsoport válassza az előfizetés vagy hozzon létre egy újat, jelölje be a **azuredeploy.json**, és kattintson a **Szerkesztése paramétereinek**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Most is szerkesztheti a sablonfájlhoz szép táblázat összes paramétert. Alapértelmezett meghatározó paramétereket már az alapértelmezett értékeket, és a paraméterek engedélyezett értéklista meghatározó jelennek meg a legördülő lista.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Töltse ki az üres Parameters, és használja a [ToDoApp GitHub repó címét](https://github.com/azure-appservice-samples/ToDoApp.git) **repoUrl**. Ezután kattintson a **Mentés**gombra.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Autoscaling egy **szabványos** réteg vagy magasabb ajánlja fel a szolgáltatást, és terv szintű riasztások szolgáltatások felajánlott **egyszerű** réteg vagy újabb verziójával, a **termékváltozat** paraméter értéke **Standard** vagy **Premium** ahhoz, hogy megjelenjen az összes új alkalmazás meglátásait erőforrások bekapcsolható kell.
    
16. Kattintson a **üzembe**. Ha bejelölte a **jelszavak mentése**, a jelszót a paraméter fájl **egyszerű szöveges formátumban**menthető. Ellenkező esetben meg kell adnia a bemeneti az adatbázis jelszavát a telepítés során.

Az egész! Most az új riasztását látni az [Azure-portál](https://portal.azure.com/) és az [Azure erőforrás Explorer](https://resources.azure.com) tool csatlakoznia kell, és a JSON hozzáadott Automatikus méretezéssel beállítások alkalmazása rendszerbe.

A szakasz lépései a főként hajtható végre a következőket:

1.  A sablonfájlt készített
2.  Válassza a sablonfájlt paraméter fájl létrehozása
3.  A paraméter fájllal a sablonfájlt rendszerbe

Az utolsó lépésben egy PowerShell-parancsmag egyszerűen végzi. Mi a Visual Studio az, ha azt rendszerbe, hogy az alkalmazás did megtekintéséhez nyissa meg a Scripts\Deploy-AzureResourceGroup.ps1. Kód nincs sok, de csak most jelölje ki a lényegesek kódot kell üzembe a sablonfájlt paraméter fájlt.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Az utolsó parancsmag `New-AzureResourceGroup`, ténylegesen a művelet végrehajtása egyetlen. Mindez kell bemutatják, hogy, szerszámok segítségével, hogy az a felhő alkalmazás előre telepítéséhez meglehetősen egyszerű. Minden alkalommal futtatja a parancsmag ugyanazt a sablont a paraméter ugyanannak a fájlnak, fogjuk ugyanazt az eredményt kapja.

## <a name="summary"></a>Összefoglalás ##

DevOps ismételhetőség és előreláthatóság is tevődik össze microservices magas szintű alkalmazás bármely sikeres telepítési billentyűkombinációt. Ebben az oktatóanyagban telepítette az erőforrás-kezelő Azure-sablon segítségével egyetlen erőforrás csoportként Azure egy két – microservice alkalmazást. Remélhetőleg akkor meghatalmazta a sablon átalakítása az Azure-alkalmazás elindítása és is kiépítése és üzembe előre szükséges ismereteket. 

## <a name="next-steps"></a>Következő lépések ##

Megtudhatja, hogy miként [Agilis módszertan alkalmazása és folyamatosan tegye közzé a könnyű kezelés microservices alkalmazással](app-service-agile-software-development.md) és a speciális telepítési technikák, például [flighting telepítés](app-service-web-test-in-production-controlled-test-flight.md) egyszerűen.

<a name="resources"></a>
## <a name="more-resources"></a>További források ##

-   [Azure erőforrás-kezelő sablon nyelve](../resource-group-authoring-templates.md)
-   [Azure erőforrás-kezelő sablonok létrehozása](../resource-group-authoring-templates.md)
-   [Azure erőforrás-kezelő sablon függvények](../resource-group-template-functions.md)
-   [Erőforrás-kezelő Azure sablonnal alkalmazások telepítése](../resource-group-template-deploy.md)
-   [Azure PowerShell használatával a Azure-kezelő eszközzel](../powershell-azure-resource-manager.md)
-   [Hibaelhárítási erőforráscsoport telepítések Azure-ban](../resource-manager-troubleshoot-deployments-portal.md)




 
