<properties
    pageTitle="Első lépések a API-alkalmazások és az alkalmazás szolgáltatás ASP.NET |} Microsoft Azure"
    description="Megtudhatja, hogy miként hozhat létre, telepítése, és Azure App szolgáltatásban ASP.NET API alkalmazás felhasználása Visual Studio 2015 használatával."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Első lépések a API-alkalmazások, ASP.NET és Swagger Azure App szolgáltatásban

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Ez a az első, amelyek bemutatják, hogyan Azure alkalmazás szolgáltatás fejlesztését és RESTful API-khoz szolgáltatója hasznosak lehetnek szolgáltatásokat oktatóanyagok sorozata.  Ebben az oktatóanyagban API-metaadatok Swagger formátum támogatása foglalkozik.

Dióhéjban:

* Bemutatja, hogyan hozhat létre, és az [API-alkalmazások](app-service-api-apps-why-best-platform.md) Azure alkalmazás szolgáltatás telepítése a Visual Studio 2015 épített eszközök segítségével.
* Hogyan lehet dinamikusan Swagger API-metaadatok létrehozásához a Swashbuckle NuGet csomag használatával automatizálhatja a API feltárás.
* Hogyan Swagger API-metaadatok használatának előrejelzést ügyfél-kódot az API-alkalmazásokban.

## <a name="sample-application-overview"></a>Minta – áttekintés

Ebben az oktatóanyagban használatakor egy egyszerű feladatlista lista minta alkalmazást. Az alkalmazás az egyoldalas elrendezésű oldalakat alkalmazás (biztonságos jelszó-hitelesítés) előtér-ASP.NET webes API középső és egy ASP.NET webes API-adatok réteg tartalmaz.

![API-alkalmazások minta alkalmazás diagram](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Az alábbiakban képernyőképe az [AngularJS](https://angularjs.org/) előtérrészét.

![Példa a API-alkalmazások alkalmazás teendőlista](./media/app-service-api-dotnet-get-started/todospa.png)

A Visual Studio megoldás három projektet tartalmaz:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - előtér: egy AngularJS biztonságos jelszó-hitelesítés, amely felhívja a középső.

* **ToDoListAPI** – a középső: egy ASP.NET webes API projekten, amely felhívja a adatok réteg teendők CRUD műveletek elvégzéséhez.

* **ToDoListDataAPI** – az adatok réteg: az ASP.NET webes API projektnél teendők CRUD műveleteket hajt végre.

A harmadik szintű architektúrájának az sok architektúrákban, amelyek segítségével miként állíthat API-alkalmazások használatával, és itt csak a bemutató célokat szolgál egyik. A kód minden réteg legegyszerűbb a lehető bemutatják API-alkalmazások szolgáltatások; például az adatok réteg kiszolgáló memória helyett használja egy adatbázis saját adatmegőrzési mechanizmusként.

Ebben az oktatóanyagban befejezése, a is a két webes API-projekt mentése és futtatása a felhőben, az alkalmazás szolgáltatáshoz tartozó API-alkalmazásokban.

A következő oktatóanyagra a sorozat a felhőbe üzembe helyezése a biztonságos jelszó-hitelesítés előtér.

## <a name="prerequisites"></a>Előfeltételek

* ASP.NET webes API - oktatóanyag utasításokat tegyük fel, hogyan dolgozhat ASP.NET [webes API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) a Visual Studióban alapvető ismeretek állnak rendelkezésre.

* Azure-fiók - lehetőség van [az ingyenes nyissa meg az Azure-fiók](/pricing/free-trial/?WT.mc_id=A261C142F) vagy [Visual Studio aktiválása előfizetői előnyeit](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Ha szeretné az első lépések Azure alkalmazás szolgáltatás, mielőtt regisztrál az Azure-fiók, nyissa meg a [Alkalmazás szolgáltatás próbálja meg](http://go.microsoft.com/fwlink/?LinkId=523751). Itt azonnal létrehozhat egy rövid életű starter alkalmazásban az alkalmazás szolgáltatás – **nem hitelkártya kötelező**, és nincs nyilatkozatát.

* Az [Azure SDK a .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - a SDK a Visual Studio 2015 automatikusan telepíti a Visual Studio 2015, ha még nincs.

    * A Visual Studióban, kattintson a Súgó kapcsolatban a Microsoft Visual Studio ->, és győződjön meg arról, hogy telepítve van a "Azure alkalmazás szolgáltatás eszközök v2.9.1" vagy magasabb.

    ![Azure alkalmazás eszközök vesion](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] Attól függően, hogy hány SDK függőség már van a számítógépen a SDK telepítése eltarthat néhány percet fél órán vagy több hosszú ideig.

## <a name="download-the-sample-application"></a>A minta alkalmazás letöltése

1. Töltse le a [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tárat.

    Kattintson a **Letöltés ZIP-** gombra, vagy a tár másolása a helyi számítógépre.

2. Nyissa meg azt a listájába megoldást Visual Studio 2015 vagy 2013 alkalmazásban.
   1. Meg kell minden megoldást a megbízható gombra.
        ![Biztonsági figyelmeztetés](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. A megoldás (CTRL + SHIFT + B) az NuGet csomagok visszaállítása összeállítása.

    Ha azt szeretné, hogy az alkalmazás, a művelet mielőtt beállítaná őket, akkor helyileg futtathatók. Győződjön meg róla, hogy ToDoListDataAPI a indítási projekt, és futtassa a megoldást. Meg kell várt a HTTP 403 hibára a böngészőben.

## <a name="use-swagger-api-metadata-and-ui"></a>Swagger API-metaadatok és a felhasználói felület használata

Azure alkalmazás üzembe API-metaadatok [Swagger](http://swagger.io/) 2.0 támogatási épül fel. Minden egyes API-alkalmazást egy URL-cím végpontot, amely visszaadja a metaadat-alapú az API Swagger JSON formátumban is megadhat. A metaadatok végpontra – által eredményül adott ügyfél-kód létrehozása használható.

ASP.NET webes API projektben dinamikusan Swagger metaadatok hozhat létre az [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet csomag használatával. A Swashbuckle NuGet csomag már telepítve van a letöltött ToDoListDataAPI és ToDoListAPI projektekben.

Az oktatóprogram ebben a részben, tekintse meg a létrehozott Swagger 2.0-s metaadatokat, és próbálkozzon a felhasználói felület Swagger metaadatok alapuló próba.

1. (**Nem** a ToDoListAPI projekt) a ToDoListDataAPI projekt beállítása az indítási projekt.

    ![Projekt indítása ToDoDataAPI beállítása](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Nyomja le az F5 billentyűt, vagy kattintson a **hibakeresési > indítása hibakeresési** hibakeresési módban a projekt futtatásához.

    A böngészőben megnyílik, és a HTTP 403 hibalap jeleníti meg.

3. Adja meg a böngésző címsorába `swagger/docs/v1` a sort, és nyomja le az visszatérési végére. (Az URL-cím `http://localhost:45914/swagger/docs/v1`.)

    Ez a 2.0-s JSON Swagger metaadatok térni az API Swashbuckle által használt alapértelmezett URL-CÍMÉT.

    Az Internet Explorer használata esetén a böngésző kéri, hogy letölti-e egy *v1.json* fájlt.

    ![Az Internet Explorer JSON metaadat-alapú letöltése](./media/app-service-api-dotnet-get-started/iev1json.png)

    Ha a Chrome, Firefox vagy él használata esetén a böngészőben a JSON a böngészőablakban jeleníti meg. Különböző böngészők JSON máshogyan kell kezelni, és a böngésző ablakának esetleg nem pontosan az történni.

    ![A Chrome-ban JSON-metaadat](./media/app-service-api-dotnet-get-started/chromev1json.png)

    A következő példában az első rész a Swagger metaadatok az API együtt a definícióját a Get-metódus. A metaadatok mi meghajtók Swagger a felhasználói felület, amelyek használatával az alábbi lépéseket, és használhatja azt az oktatóprogram későbbi szakaszában ügyfél-kód automatikus létrehozása.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Zárja be a böngészőt, és leállítja a Visual Studio hibakeresési.

5. **Megoldás Explorer**ToDoListDataAPI projektben nyissa meg a *App_Start\SwaggerConfig.cs* fájlt, majd görgessen le 174 sor, és vegye ki a megjegyzésjeleket a következő kódot.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    A *SwaggerConfig.cs* fájlt a Swashbuckle csomag projekt telepítése során jön létre. A fájl többféle módon Swashbuckle konfigurálása.

    A korábban a uncommented kódot lehetővé teszi, hogy a Swagger felhasználói felületének, amelyek használatával az alábbi lépéseket. A webes API-projekt API alkalmazás project-sablon használatával létrehozásakor kód van megjegyzéseket fűzhet alapértelmezett biztonsági okokból.

6. Indítsa újra a projektet.

7. Adja meg a böngésző címsorába `swagger` a sort, és nyomja le az visszatérési végére. (Az URL-cím `http://localhost:45914/swagger`.)

8. A Swagger felhasználói felület lap megjelenésekor kattintson a **listájába** , a rendelkezésre álló módszerekkel megjelenítéséhez.

    ![Felhasználói felület rendelkezésre álló módszerek swagger](./media/app-service-api-dotnet-get-started/methods.png)

9. Kattintson a lista első **beolvasása** gombra.

10. **Paraméterek** csoportban adja meg a csillaggal a program a `owner` paraméter, és kattintson a **próbálja ki**.

    Hitelesítés újabb oktatóanyag hozzáadásakor a középső nyújtanak az adatok réteg a tényleges felhasználói Azonosítóval. Most minden tevékenység lesz csillag a tulajdonos ID az alkalmazás engedélyezett hitelesítés nélkül futása közben.

    ![Felhasználói felület swagger próbálja ki](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    A Swagger felhasználói felületének a hívások listájába kérése módszer, és megjeleníti, a válasz kód és a JSON eredménye.

    ![Felhasználói felület swagger próbálja ki az eredmények](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Kattintson a **Küldés**elemre, és válassza a **Modell séma**a mezőben.

    Kattintás a modell séma prefills a beviteli mezőben megadhatja a bejegyzés módszer a paraméter értéke. (Ha ez nem működik az Internet Explorerben, más böngészőt használ, vagy írja be manuálisan a paraméter értéke a következő lépés.)  

    ![Felhasználói felület swagger próbálja ki a bejegyzésben](./media/app-service-api-dotnet-get-started/post.png)

12. A JSON az módosítása a `todo` paramétert a szövegbeviteli mezőbe, hogy a következő példa lát, vagy helyett a saját szöveget:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Kattintson a **próbálja ki**.

    A listájába API-HTTP 204 válasz kódot, amely jelzi a sikeres adja eredményül.

14. Kattintson az első **beszerzése** gombra, és a lap azon a részén kattintson a **próbálja ki** gombra.

    A Get-módszer válasz most már magában foglalja az új elemet.

15. Nem kötelező: Próbálja meg is a helyezése, törlése, és azonosító módszerekkel el.

16. Zárja be a böngészőt, és leállítja a Visual Studio hibakeresési.

Swashbuckle működik-e minden ASP.NET webes API-projektet. Ha azt szeretné, Swagger metaadatok létrehozása egy meglévő projekten szeretne hozzáadni, csak a Swashbuckle csomag telepítéséhez.

>[AZURE.NOTE] Metaadat-alapú swagger az egyes API műveletekhez egyedi Azonosítót tartalmaz. Alapértelmezés szerint a Swashbuckle miatt előfordulhat, hogy ismétlődő Swagger művelet azonosítók a webes API-vezérlő módokat. Ez történik, ha a vezérlő például van terhelve HTTP módszerek, `Get()` és `Get(id)`. Információ arról, hogy miként kezelje a túlterhelések, akkor olvassa el a [testreszabása Swashbuckle által generált API-definíciók](app-service-api-dotnet-swashbuckle-customize.md). Ha egy webes API-projektet hoz létre a Visual Studióban az Azure API-alkalmazássablon használatával, által generált egyedi művelet azonosítók vonalkód automatikusan felkerül a *SwaggerConfig.cs* fájlt.  

## <a id="createapiapp"></a>API-alkalmazás létrehozása az Azure-ban és az azt telepítése kódot.

Ebben a részben Azure eszközök, amelyek a Visual Studio **Webhely közzététele** varázsló új API-alkalmazás létrehozása az Azure közös használata. Ezután telepítse az ToDoListDataAPI-projektet az új API-alkalmazásba, és hívja fel az API Swagger a felhasználói felület futtatásával.

1. A **Megoldás Explorer**kattintson a jobb gombbal a ToDoListDataAPI projekt, és kattintson a **Közzététel**gombra.

    ![Kattintson a Közzététel gombra a Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  Kattintson a **Közzététel** varázsló **profil** lépésében **A Microsoft Azure alkalmazás szolgáltatás**.

    ![Kattintson az Azure alkalmazás szolgáltatást a webhely közzététele](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Ha, jelentkezzen be az Azure-fiókjába, még nem tette, vagy a hitelesítő adatok frissítése, ha éppen a lejárt.

4. Alkalmazás szolgáltatás párbeszédpanelen válassza ki a használni kívánt Azure **előfizetés** , és kattintson az **Új**gombra.

    ![Kattintson az új alkalmazás szolgáltatás párbeszédpanel](./media/app-service-api-dotnet-get-started/clicknew.png)

    Az **Alkalmazás szolgáltatás hozzon létre** párbeszédpanel **Hosting** lapja jelenik meg.

    Swashbuckle telepítve van egy webes API projekt éppen telepít, mert a Visual Studio feltételezi, hogy szeretne-e létrehozni az API-alkalmazásokban. Ez az **API-alkalmazás nevét** és címével arra, hogy a **Típus módosítása** legördülő lista értéke **API-alkalmazás**jelzi.

    ![Az alkalmazás szolgáltatási párbeszédpanel alkalmazás típusa](./media/app-service-api-dotnet-get-started/apptype.png)

5. Írjon be egy egyedi *azurewebsites.net* tartományban van **API-alkalmazás nevét** . Elfogadhatja az alapértelmezett nevet, amely a Visual Studio javasolja.

    Ha beírta a nevét, hogy valaki más már használatban van, jobbra piros felkiáltójel látható.

    Az API-alkalmazás URL-címe lesz `{API app name}.azurewebsites.net`.

6. Az **Erőforráscsoport** legördülő kattintson az **Új**gombra, és ha inkább írja be "ToDoListGroup" vagy egy másik nevet.

    Erőforráscsoport gyűjteménye Azure erőforrások, például az API-alkalmazások, adatbázisok, VMs és így tovább. Ebben az oktatóanyagban célszerű hozzon létre új erőforráscsoport, mivel, amely megkönnyíti az Azure erőforrások az oktatóprogram létrehozott egy lépésben törlése.

    Ebben a mezőben lehetővé teszi, hogy, jelölje be a meglévő [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) , vagy hozzon létre egy újat, írjon be egy nevet, amely eltér az előfizetés a meglévő erőforrás csoportokhoz.

7. Az **Alkalmazás szolgáltatás megtervezése** legördülő lista mellett lévő **Új** gombra.

    A képernyőkép minta értékeit mutatja az **API-alkalmazás nevét**, az **előfizetés**és a **Erőforráscsoport** – az értékek különböző lesz.

    ![Hozzon létre alkalmazás szolgáltatási párbeszédpanel](./media/app-service-api-dotnet-get-started/createas.png)

    Az alábbi lépésekkel hozza létre az új erőforráscsoport-alkalmazás szolgáltatás megtervezése. Az alkalmazás szolgáltatáscsomagja adja meg a számítási erőforrások az API-alkalmazást futtató. Például ha úgy dönt, hogy a szabad réteg, az API-alkalmazás futtat a megosztott VMs futása az egyes fizetett rétegek a dedikált VMs. App milyen szolgáltatáscsomagok információkért [alkalmazás szolgáltatás csomagok áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)című témakörben találhat.

8. A **Konfigurálása alkalmazás szolgáltatás tervezése** párbeszédpanelen adja meg "ToDoListPlan" vagy más néven Ha inkább.

9. A **hely** legördülő listában válassza a helyre, ahol a legközelebb.

    Ezzel a beállítással megadhatja, melyik Azure adatközponthoz az alkalmazás fog futni. Válasszon egy helyet közelébe, [a késleltetés](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)csökkentése érdekében.

10. A **méret** legördülő listában kattintson a **Free**gombra.

    Az ebben az oktatóanyagban a szabad árak réteg nyújt elegendő teljesítményét.

11. **Konfigurálása alkalmazás szolgáltatás tervezése** párbeszédpanelen kattintson **az OK gombra**.

    ![Kattintson az OK gombra a megadása alkalmazás szolgáltatáscsomagja](./media/app-service-api-dotnet-get-started/configasp.png)

12. **Alkalmazás szolgáltatás hozzon létre** párbeszédpanelen kattintson a **Létrehozás**gombra.

    ![Kattintson a Létrehozás gombra az alkalmazás szolgáltatási létrehozása párbeszédpanel](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Visual Studio hoz létre az API-alkalmazás és a közzététel profilok, amely tartalmaz minden szükséges beállítás a API-alkalmazásokban. Kattintson a **Webhely közzététele** varázslót, amely a telepíthető a projekt kell megadnia nyílik meg.

    A **Közzététel** varázsló megnyílik a **kapcsolat** lapon (lásd alább).

    A **kapcsolat** lapon a **kiszolgáló** és a **webhely neve** beállításai mutasson az API-alkalmazást. A **felhasználónév** és **jelszó** Azure létrehoz, telepítési hitelesítő adatokat. A telepítés után a Visual Studio megnyílik a böngésző címsorába a **Célhely URL-CÍMÉT** (csak szolgál a **Célhely URL-CÍMÉT**).  

13. Kattintson a **Tovább**gombra.

    ![Kattintson a Tovább gombra a kapcsolat lapon a webhely közzététele](./media/app-service-api-dotnet-get-started/connnext.png)

    A következő lap jelenik meg a **Beállítások** lapon (lásd alább). Itt módosíthatja a [távoli](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)hibakereséshez hibakeresési építés üzembe build beállítás lapon. A lap is kínál **Beállításokat közzététele**:

    * A cél további fájlok eltávolítása
    * Közzététel alatt precompile
    * Fájlok kihagyása a App_Data mappából

    Az ebben az oktatóanyagban már nincs szükség az alábbiak. Keresett részletes ismertetése ugyanúgy működnek, lásd: [hogyan: a webes projekt segítségével egyetlen kattintással közzététele a Visual Studióban üzembe](https://msdn.microsoft.com/library/dd465337.aspx).

14. Kattintson a **Tovább**gombra.

    ![Kattintson a Tovább gombra a beállítások lapon a webhely közzététele](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Ezután van a **kép** lap (lásd alább), mely ad egy lehetőséget, ha milyen fájlokat szeretné látni fogja a projekt másolja át az API-alkalmazást. Ha már telepítette a korábbi API alkalmazás projekt telepítését, csak a módosított fájlok kerülnek. Ha szeretne másolni mi listájának megtekintése, rákattintva az **Előnézet indítása** gombra.

15. Kattintson a **Közzététel**gombra.

    ![Kattintson a Közzététel gombra a kép lap közzététele webhely](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio az új API-alkalmazásba üzembe helyezése a ToDoListDataAPI projekt. A **kimeneti** ablakban sikeres környezetben jelentkezik, és a böngészőablakban az URL-címet az API-alkalmazás megnyitásakor megjelenik a "sikeres létrehozott" lap.

    ![Kimeneti ablakban sikeres telepítési](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Új API sikeresen létre lapja](./media/app-service-api-dotnet-get-started/appcreated.png)

16. "Swagger" felvétele a böngésző címsorában lévő URL-CÍMÉT, és nyomja le az ENTER billentyűt. (Az URL-cím `http://{apiappname}.azurewebsites.net/swagger`.)

    A böngészőben jeleníti meg, amely a korábban, de most már fut a felhőben Swagger UI. Próbálja ki a Get-metódus, és meg, hogy Ön-e vissza az alapértelmezett 2 teendők. A korábban elvégzett módosításokat a memóriában a helyi számítógép zónában mentette.

17. Nyissa meg az [Azure-portálon](https://portal.azure.com/).

    Az Azure portál egy webes felület Azure erőforrások, például API-alkalmazások kezeléséhez.

18. Kattintson a **További szolgáltatások > szolgáltatások alkalmazás**.

    ![Tallózással keresse meg az App szolgáltatások](./media/app-service-api-dotnet-get-started/browseas.png)

19. Az **Alkalmazás szolgáltatások** lap keresése, és kattintson az új API-alkalmazást. (Az Azure-portálon nyissa meg a jobb oldalon a windows úgynevezett *pengéit*.)

    ![Alkalmazás a szolgáltatások lap](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Nyissa meg a két pengéit. Egy lap van áttekintése az API-alkalmazást, és egy tartalmaz hosszú beállításainak megtekintése és módosítása.

20. A **Beállítások** lap a keresse meg a **API** című szakaszt, és kattintson a **Definíció API**.

    ![A beállítások lap API meghatározása](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Az **API-definíciós** lap az URL-címet, amely visszaadja a metaadat-alapú Swagger 2.0-s JSON formátumban megadását teszi lehetővé. Ha Visual Studio hoz létre az API-alkalmazást, értékre állítja a API definition URL-CÍMÉT az alapértelmezett érték a Swashbuckle által generált metaadatok, amely a korábban, amely az API-alkalmazás adatait a viszonyítási URL-cím plusz `/swagger/docs/v1`.

    ![API-definíciós URL-címe](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Ügyfél-kód létrehozása a hozzá tartozó API alkalmazás lehetőséget választja, a Visual Studio Ez az URL a metaadatok átveszi.

## <a id="codegen"></a>Az adatok réteg ügyfél-kód létrehozása

Az az előnye Swagger integrálása az Azure API-alkalmazások egyik kód automatikus generálása. Létrehozott ügyfél osztályok megkönnyítik a kódírás felhívja az API-alkalmazásokban.

A ToDoListAPI a projekt már létrehozott ügyfél kódot, de a következőképpen fog törölheti és újragenerálása, hogy megtudhatja, hogy miként végezze el a kód létrehozása.

1. A Visual Studio **Megoldás Explorer**, a ToDoListAPI projekt törlése a *ToDoListDataAPI* mappát. **Figyelmeztetés: Csak a mappa törléséhez nem a ToDoListDataAPI projekt.**

    ![A létrehozott ügyfél kód törlése](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Ez a mappa létrehozásának a kód létrehozási folyamat, amely az Ön készül, hogy feldolgozzuk használatával.

2. Kattintson a jobb gombbal a ToDoListAPI projekt, és kattintson a **hozzáadása > REST API-ügyfél**.

    ![A Visual Studióban REST API-ügyfél hozzáadása](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. **REST API ügyfél hozzáadása** párbeszédpanelen kattintson a **Swagger URL-CÍMÉT**, és válassza az **Azure eszköz kiválasztása**.

    ![Jelölje ki az Azure eszköz](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. **Alkalmazás szolgáltatás** párbeszédpanelen bontsa ki az erőforráscsoport ebben az oktatóanyagban átvitelére használt, és válassza ki az API-alkalmazást, és kattintson **az OK**gombra.

    ![Jelölje ki az API alkalmazást kód létrehozása céljából](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Figyelje meg, hogy amikor visszatér a **REST API ügyfél hozzáadása** párbeszédpanelen, a szövegdoboz van töltve a API-definíciót be-beli korábbi a portál URL-cím értéket.

    ![API-definíciós URL-címe](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Egy alternatív szeretne letölteni a metaadat-alapú kód generálása módja az URL-cím közvetlenül, ahelyett hogy a Tallózás párbeszédpanelen adja meg. Vagy szeretné az ügyfél-kód létrehozása az Azure telepítése előtt, ha sikerült a webes API-projekt helyben futtatni, lépjen az URL-címet, ahol a Swagger JSON-fájlt, mentse a fájlt, és adja meg a **Válassza ki a meglévő Swagger metaadatok fájl** beállítást.

5. A **REST API ügyfél hozzáadása** párbeszédpanelen kattintson **az OK gombra**.

    Visual Studio létrehoz egy után az API-alkalmazás nevű mappát, és hozza létre az ügyfél osztályok.

    ![Kód fájlok létrehozott ügyfél](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. Nyissa meg a ToDoListAPI projekt *Controllers\ToDoListController.cs* 40 vonal, amely az API hív meg által generált ügyfelet használ, a kód megtekintéséhez.

    A következő kódtöredékének jeleníti meg, hogyan a kód elindítja az ügyfél-objektumra, és hívja a Get-metódus.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    A konstruktor paraméter kapja a végpont URL-címet a `toDoListDataAPIURL` app beállítása. A fájlt az adott értéke helyi IIS Express URL-CÍMÉT az API-projektet, hogy a helyi meghajtóra meg tudja nyitni az alkalmazás. Ha a konstruktor paramétert elhagyja, az alapértelmezett végpont URL-címet jelenti a létrehozott a kódot.

7. Az ügyfél osztályához jön létre, más néven alapján az API-alkalmazás neve; hogy a típus neve megegyezik a Mi az a projekt jött létre, módosíthatja a *Controllers\ToDoListController.cs* a kódot. Ha például az API-alkalmazás ToDoListDataAPI071316 elnevezett, ha változna kód:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

Ez a:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>A középső tárolni-API-alkalmazás létrehozása

Korábbi, [az adatok réteg API-alkalmazás létrehozása, és azt kód rendszerbe](#createapiapp).  Most már hajtsa végre ugyanezt az eljárást, a középső API-alkalmazásokban.

1. A **Megoldás Explorer**, kattintson a jobb gombbal a középső ToDoListAPI project (nem az adatok réteg ToDoListDataAPI), és kattintson a **Közzététel**gombra.

    ![Kattintson a Közzététel gombra a Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  A **Webhely közzététele** varázsló a **profil** lapon kattintson **A Microsoft Azure alkalmazás szolgáltatás**.

3. **Alkalmazás szolgáltatás** párbeszédpanelen kattintson az **Új**gombra.

4. Az **Alkalmazás szolgáltatás hozzon létre** párbeszédpanel **Hosting** lapon fogadja el az alapértelmezett **API-alkalmazás nevét** , vagy írjon be egy nevet, amely egyedi *azurewebsites.net* a tartományban.

5. Válassza az Azure **előfizetés** használta.

6. Az **Erőforráscsoport** legördülő listában válassza ki a korábban létrehozott azonos erőforráscsoport.

7. Az **Alkalmazás szolgáltatáscsomagja** legördülő listában válassza ki a korábban létrehozott megegyező csomagot. Ez az érték fog alapértelmezett.

8. Kattintson a **létrehozása**gombra.

    Visual Studio az API-alkalmazás létrehozása, közzététel profil létrehozása és a **kapcsolat** lépés a **Webhely közzététele** varázsló jeleníti meg.

9.  A **Webhely közzététele** varázslót **kapcsolat** lépésben kattintson a **Közzététel**gombra.

    Visual Studio üzembe helyezése a ToDoListAPI projekt az új API-alkalmazásba, és az API-alkalmazás URL-címében a böngészőben megnyílik. Megjelenik a "sikeres létrehozott" lap.

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>A középső, hívja fel az adatok szint konfigurálása

Most már hívta a középső API-alkalmazást, ha megpróbál hívja fel a adatok réteg visszaállítása a fájlt localhost URL-cím használatával. Ebben a szakaszban adja meg az adatok réteg API app URL-cím be egy környezet beállítása a középső API-alkalmazásban. A kód, a középső API alkalmazásban beolvassa az adatokat réteg URL-cím beállítása, ha a környezet beállítása érvényteleníti meg, mi látható a fájlt.

1. Az [Azure-portálra](https://portal.azure.com/), és nyissa meg az **API-alkalmazás** lap az Ön által létrehozott tárolni a TodoListAPI (középső) projekt API alkalmazáshoz.

2. A **Beállítások** lap az API-alkalmazást kattintson az **alkalmazás beállításai**parancsra.

3. Az **Alkalmazás beállításai** lap az API-alkalmazást görgessen le az **alkalmazás beállításai** csoportban, és adja hozzá a következő kulcsot és értéket. Az érték az első API-alkalmazást, ebben az oktatóanyagban közzétett URL-címe lesz.

  	| **Kulcs** | toDoListDataAPIURL |
  	|---|---|
  	| **Érték** | https://{your adatok réteg API alkalmazásnév} .azurewebsites .net |
  	| **Példa** | https://todolistdataapi.azurewebsites.NET |

4. Kattintson a **Mentés**gombra.

    ![Kattintson a Mentés az alkalmazás beállításai](./media/app-service-api-dotnet-get-started/asinportal.png)

    A kód futtatásakor Azure-ban ezt az értéket most felülírja az localhost URL-címet, amely a fájlt.

## <a name="test"></a>Teszt

1. A böngészőablakban keresse meg az új középső API-alkalmazás éppen létrehozott ToDoListAPI az URL-CÍMÉT. Elérheti van az API-alkalmazás fő fel a portálon található az URL-CÍMÉRE kattintva.

2. "Swagger" felvétele a böngésző címsorában lévő URL-CÍMÉT, és nyomja le az ENTER billentyűt. (Az URL-cím `http://{apiappname}.azurewebsites.net/swagger`.)

    A böngészőben jeleníti meg a látott korábbi az ToDoListDataAPI, de most Swagger Felhasználóifelület `owner` függvény nem kötelező megadni a művelet, mivel a középső API-alkalmazás küldő ezt az értéket az adatok réteg API-alkalmazás meg. (A hitelesítési oktatóanyagok ekkor a középső tényleges felhasználói azonosítók küld a `owner` paraméter; a most már meg van kemény kódolása csillag.)

3. Próbálja ki a Get-metódus és a más módszereket, amelyekkel ellenőrzése, hogy a középső API-alkalmazás sikeresen hívásához az adatok réteg API-alkalmazást.

    ![Felhasználói felület első módszer swagger](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Hibaelhárítás

Abban az esetben, ha probléma – Ez az oktatóanyag az alábbi menet közben problémába vannak hibaelhárítási ötletek:

* Győződjön meg arról, hogy az [Azure SDK a .NET rendszerhez](http://go.microsoft.com/fwlink/?linkid=518003), a legújabb verzióját használja.

* A projekt nevét a kettő hasonló (ToDoListAPI, ToDoListDataAPI). Ha dolog, amit nem jelenik meg, amikor egy projekt dolgozik, a képernyőn megjelenő utasításokat ismertetett módon, feltétlenül nyitotta meg a megfelelő projekthez.

* Ha a vállalati hálózaton és Azure alkalmazás szolgáltatás tűzfalon keresztül telepíteni, ellenőrizze, hogy 8172 és a 443-as port nyitva webes telepítése. Ha nem tud megnyitni azokat a portokat, más telepítési módok is használhatja.  Lásd: [telepítse az Azure alkalmazás szolgáltatás alkalmazást](../app-service-web/web-sites-deploy.md).

* "Útvonal nevének egyedinek kell lennie" hibák – sikerült kap e ha véletlenül a megfelelő projekt API alkalmazás telepítése, és később telepíteni szeretné, hogy a megfelelő fiókot. A probléma elhárításához újratelepítés a megfelelő projekthez az API-alkalmazást, és a **Webhely közzététele** varázsló a **Beállítások** lapon jelölje be **célhelyen további fájlok eltávolítása**.

Után az Azure alkalmazás szolgáltatást futtató ASP.NET API-alkalmazást, érdemes, ha többet szeretne tudni a Visual Studio funkciók, amelyek megkönnyítik a hibaelhárítás. Naplózás információt távoli hibakeresés és az egyéb, című [hibaelhárító Azure alkalmazás szolgáltatás a Visual Studióban](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Következő lépések

Láthatta, API-alkalmazás telepítése a meglévő webes API projektek, API-alkalmazások ügyfél-kód létrehozása és felhasználása a .NET-ügyfelek API-alkalmazások hogyan. A következő oktatóanyagra a sorozat látható [CORS használhatnak JavaScript-ügyfelek API-alkalmazások](app-service-api-cors-consume-javascript.md)használatát.

További információt az ügyfél-kód létrehozása nézze meg az [Azure/AutoRest](https://github.com/azure/autorest) tárházba GitHub.com. Problémákkal kapcsolatos segítségért a létrehozott ügyfélprogramban nyissa meg a [problémát a AutoRest tárat](https://github.com/azure/autorest/issues).

Ha API-alkalmazás új projekt létrehozása előzmények nélkül szeretne, használja az **Azure API** -alkalmazássablon.

![A Visual Studióban API alkalmazássablon](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

**Azure API-App** a project-sablon függvény egyenértékű a választás az **üres** ASP.NET 4.5.2 sablont, a jelölőnégyzet bejelölésével adja hozzá a webes API-támogatás parancsra, és a Swashbuckle NuGet csomag telepítéséhez. Ezenkívül a sablon hozzáadása néhány Swashbuckle fiókkonfigurálási kód célja, hogy az ismétlődő Swagger művelet azonosítók létrehozásának megakadályozása. Miután létrehozta a projektben API-alkalmazást, telepítheti azt az API-alkalmazásokban ebben az oktatóanyagban-beli ugyanúgy.
