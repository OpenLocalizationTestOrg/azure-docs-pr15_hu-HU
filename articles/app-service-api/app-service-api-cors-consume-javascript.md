<properties
    pageTitle="Alkalmazás szolgáltatás támogatást CORS |} Microsoft Azure"
    description="Megtudhatja, hogy miként CORS támogatás használata Azure Azure App szolgáltatásban."
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
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>CORS használatáról JavaScript API alkalmazás felhasználása

Alkalmazás-szolgáltatás [Keresztté Origin erőforrások megosztása (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), amely lehetővé teszi, hogy a tartományok közötti hívások API-hoz, amelyekhez az API-alkalmazásokban a JavaScript-ügyfelek beépített támogatása kínál. Alkalmazás szolgáltatás lehetővé teszi a CORS hozzáférést a API konfigurálása a API programozás nélkül.

Ez a cikk két szakaszból áll:

* A [CORS konfigurálása](#corsconfig) szakaszból általános CORS konfigurálása bármely API-alkalmazás, a web app vagy a mobilalkalmazásban. .NET, Node.js és Java-alkalmazás szolgáltatás által támogatott összes keretek egyaránt vonatkozik. 

* A [folyamatos az első lépések .NET oktatóanyagok](#tutorialstart) szakasz kezdve a témakör az oktatóanyagot, mely szemlélteti, hogy CORS támogatja a [az első API-alkalmazásokat az első lépések – oktatóprogram](app-service-api-dotnet-get-started.md)did elkészítéséhez. 

## <a id="corsconfig"></a>CORS Azure alkalmazás szolgáltatás konfigurálása

CORS konfigurálhatja az Azure-portálon vagy [Azure erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) eszközök segítségével.

#### <a name="configure-cors-in-the-azure-portal"></a>CORS konfigurálása az Azure-portálon

8. A böngészőben nyissa meg az [Azure-portálon](https://portal.azure.com/).

2. Kattintson az **Alkalmazás szolgáltatások**, és válassza az API-alkalmazás nevét.

    ![Jelölje ki az API alkalmazást portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. A **Beállítások** lap, amely megnyitja az **API-alkalmazás** lap jobbra keresse meg a **API** című szakaszt, és válassza a **CORS**.

    ![Válassza a beállítások lap CORS](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. A szöveg mezőbe írja be az URL-cím vagy engedélyezni a JavaScript hívások származnak az URL.


    Ha például ha telepítette az todolistangular nevű webalkalmazást JavaScript alkalmazást, írja be az "https://todolistangular.azurewebsites.net". Alternatív megoldásként csillag (*) adja meg, hogy elfogadott tartományok origin adhat meg.


13. Kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Gombra kattintás után **Mentse**az API-alkalmazás akkor fogad el a megadott URL-címek JavaScript hívásait.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Állítsa be a CORS Azure erőforrás-kezelő eszközzel

Beállíthatja úgy is CORS az API-alkalmazásokban parancssori eszközök, például az [Azure PowerShell](../powershell-install-configure.md) és az [Azure CLI](../xplat-cli-install.md) [Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md) használatával. 

Példát állítja be a CORS tulajdonság egy erőforrás-kezelő Azure-sablon nyissa meg a [azuredeploy.json fájlt a tárban tárolnak, ebben az oktatóanyagban minta alkalmazáshoz](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Keresse meg a sablont, amelyet a következő példa a következőképpen néz szakasza:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>A .NET-bevezetés oktatóprogram folytatása

Ha a Node.js vagy Java-bevezetés sorozat az API-alkalmazások követően befejezése a keresztüli lépések sorozat. Ugrás a [következő lépésekkel](#next-steps) szakasz API-alkalmazásokkal kapcsolatos további tanulási javaslatok keresése.

Ez a cikk hátralévő a .NET-bevezetés sorozat folytatása, és azt feltételezi, hogy [az első oktatóprogram](app-service-api-dotnet-get-started.md)sikeresen befejeződött.

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>A ToDoListAngular projekt új webes alkalmazás telepítése

[Az első oktatóprogram](app-service-api-dotnet-get-started.md)létrehozott egy középső API-val és a adatok réteg API-alkalmazást. Ebben az oktatóanyagban hoz létre az egyoldalas elrendezésű oldalakat alkalmazás (biztonságos jelszó-hitelesítés) webalkalmazást, hogy a hívások a középső API alkalmazást. A biztonságos jelszó-hitelesítés működik, akkor az engedélyeznie kell az CORS a középső API alkalmazásba. 

[Listájába minta alkalmazás](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)a ToDoListAngular projekt egy egyszerű AngularJS ügyfél, amely felhívja a középső ToDoListAPI webes API-projektet. A *app/scripts/todoListSvc.js* fájlban JavaScript-kód használatával a AngularJS HTTP-szolgáltató hívja fel az API-t. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Új webes alkalmazás a ToDoListAngular projekt létrehozása

Az új alkalmazás szolgáltatás web App alkalmazásban készíthetnek és helyezhetnek üzembe a projekt rá eljárás hasonlít [létrehozásához](app-service-api-dotnet-get-started.md#createapiapp)és üzembe helyezése a sorozat első oktatóanyagra API-alkalmazás bemutattuk. Az egyetlen különbség, hogy az alkalmazás típus-e a **Web App** **Alkalmazás API**helyett.  A párbeszédpanelek képernyőképek látható 

1. A **Megoldás Explorer**kattintson a jobb gombbal a ToDoListAngular projekt, és ezután kattintson a **Közzététel**gombra.

3.  A **Webhely közzététele** varázsló a **profil** lapon kattintson **A Microsoft Azure alkalmazás szolgáltatás**.

5. **Alkalmazás szolgáltatás** párbeszédpanelen kattintson az **Új**gombra.

3. Az **Alkalmazás szolgáltatás hozzon létre** párbeszédpanel **Hosting** fülre írja be a **Webes alkalmazás neve** egyedi *azurewebsites.net* a tartományban. 

5. Válassza az Azure **előfizetés** , amellyel dolgozni szeretne.

6. Az **Erőforráscsoport** legördülő listában válassza ki a korábban létrehozott azonos erőforráscsoport.

4. Az **Alkalmazás szolgáltatáscsomagja** legördülő listában válassza a korábban létrehozott megegyező csomagot. 

7. Kattintson a **létrehozása**gombra.

    Visual Studio hoz létre a web app, közzététel profil létrehozása és a **kapcsolat** lépés a **Webhely közzététele** varázsló jeleníti meg.

    Még ne kattintson a **Közzététel** . A következő szakaszban az új hívja fel a középső API-alkalmazás alkalmazás szolgáltatást futtató webalkalmazás konfigurálása. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>A középső URL-cím megadása a web app beállításai

1. Az [Azure-portálra](https://portal.azure.com/), és nyissa meg a **Web App** lap az Ön által létrehozott tárolni a TodoListAngular (előtér) a project web App alkalmazásban.

2. Kattintson a **Beállítások > alkalmazás beállításainak**.

3. Az **alkalmazás beállításai** csoportban adja meg a következő kulcsot és érték:

  	|Kulcs|Érték|Példa
  	|---|---|---|
  	|toDoListAPIURL|https://{your középső API-alkalmazás nevét} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Kattintson a **Mentés**gombra.

    A kód futtatásakor Azure-ban a ezt az értéket felülbírálja az localhost URL-címet, amely *a fájlt* . 

    A kód, amely a beállítás értékét kapja *index.cshtml*van:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    A kód *todoListSvc.js* beállítást használja:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>A ToDoListAngular webes projekt az új webhely-alkalmazás telepítése

*  A Visual Studióban a **Webhely közzététele** varázsló **kapcsolat** lépésben kattintson a **Közzététel**gombra.

    Visual Studio üzembe helyezése a ToDoListAngular projekt az új web App alkalmazásban, és megnyílik a böngésző címsorába a web app URL-CÍMÉT. 

### <a name="test-the-application-without-cors-enabled"></a>Az alkalmazás tesztelése engedélyezett CORS nélkül 

2. Kattintson a Fejlesztőeszközök a böngészőben nyissa meg a a konzol ablakot.

3. A felhasználói felület AngularJS jeleníti meg a böngészőablakban **Teendőlista** hivatkozásra.

    A JavaScript-kód próbál hívja fel a középső API-alkalmazást, de a hibát jelez, mivel az előtér működik, mint a háttér egy másik tartományban található. A fejlesztői eszközökkel konzolon böngészőablakban határokon származási hibaüzenet látható.

    ![Idegen származási hibaüzenet jelenik meg](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>A középső API-alkalmazás CORS konfigurálása

Ebben a részben állítsa be a középső ToDoListAPI API-alkalmazás Azure-ban CORS beállítást. Ez a beállítás lehetővé teszi a középső hívásokat a hoz létre a ToDoListAngular a project web App alkalmazásban a JavaScript API-alkalmazás.

8. A böngészőben nyissa meg az [Azure-portálon](https://portal.azure.com/).

2. Kattintson az **Alkalmazás szolgáltatások**, és válassza a ToDoListAPI (középső) API-alkalmazás.

    ![Jelölje ki az API alkalmazást portálon](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. A **Beállítások** lap, amely megnyitja az **API-alkalmazás** lap jobbra keresse meg a **API** című szakaszt, és válassza a **CORS**.

    ![Jelölje ki a CORS portálon](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. A szöveg mezőbe írja be az URL-címet a ToDoListAngular (előtér) webalkalmazás. Például a ToDoListAngular projekt todolistangular0121 nevű webalkalmazást rendszerbe állított, ha lehetővé teszi a hívások URL-ekről `https://todolistangular0121.azurewebsites.net`.

    Alternatív megoldásként csillag (*) adja meg, hogy elfogadott tartományok origin adhat meg.

13. Kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Gombra kattintás után **Mentse**az API-alkalmazás akkor fogad el a megadott URL-cím JavaScript hívásait. A képernyőkép az ToDoListAPI0223 API-alkalmazás akkor fogad el a ToDoListAngular web app JavaScript-ügyfél hívásait.

### <a name="test-the-application-with-cors-enabled"></a>Az alkalmazás tesztelje az engedélyezett CORS

* Nyissa meg a web app HTTPS URL-CÍMÉT a böngésző címsorába. 

    Ez esetben az alkalmazás segítségével megtekintése, hozzáadása, szerkesztése és törlése a teendők. 

    ![Minta app oldalát felsoroló](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Alkalmazás szolgáltatás és a webes API CORS CORS

A webes API projekt megadása a kódot, mely a API akkor fogad el tartományok JavaScript hívja fel a [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet csomag telepítheti.
 
Webes API CORS támogatásának rugalmasabb, mint alkalmazás szolgáltatás CORS támogatási. Például a kód megadhatja különböző elfogadott forrásokból különböző művelet módszerek, miközben alkalmazás szolgáltatás CORS, adja meg az összes az API-alkalmazásokban módszerek elfogadott forrásokból egyik készlete.

> [AZURE.NOTE] Nem próbálja használni a webes API CORS és a alkalmazás szolgáltatás CORS egy API-alkalmazásban. Alkalmazás szolgáltatás CORS elsőbbséget, és a webes API CORS nem lesz hatása. Például ha egy origin tartományt, az alkalmazás szolgáltatás engedélyezése, és engedélyezheti a webes API-kód tartományai origin, az Azure API-alkalmazás csak fogad el az Azure-ban a megadott tartomány hívásait.

### <a name="how-to-enable-cors-in-web-api-code"></a>Hogyan engedélyezhető CORS webes API-kódban

Az alábbi lépéseket foglalja össze a webes API CORS támogatás engedélyezése a folyamat. További tudnivalókért lásd: [Határokon származási kérések engedélyezése ASP.NET webes API 2](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. A webes API projektben a [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet csomag telepítéséhez.

1. Tartalmazza a `config.EnableCors()` **WebApiConfig** osztály, ahogy a következő példában a **regisztrálni** módja a sort. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Adja meg a webes API-vezérlő egy `using` nyilatkozata a `System.Web.Http.Cors` névtér, és vegyen fel a `EnableCors` attribútum a vezérlő osztály vagy egyéni művelet módszereket. A következő példában CORS támogatása a teljes vezérlő vonatkozik.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Azure API-kezelés az API-alkalmazások segítségével

Ha Azure API-kezelés az API alkalmazást használja, konfigurálja CORS a API kezelése az API-alkalmazásban. További információt az alábbi forrásokban talál:

* [Azure API-kezelés – áttekintés (Videó: CORS kezdődik 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Több tartomány házirendek API kezelése](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Hibaelhárítás

Abban az esetben, felmerülhetnek probléma ebben az oktatóanyagban keresztül, az alábbiakban néhány hibaelhárítási ötletek.

* Győződjön meg arról, hogy az [Azure SDK for Visual Studio 2015 .NET](http://go.microsoft.com/fwlink/?linkid=518003)a legújabb verzióját használja.

* Győződjön meg arról, hogy a beírt `https` a CORS beállítással, és győződjön meg arról, hogy beszédeszközt használ `https` az előtér-webalkalmazás futtatásához.

* Győződjön meg arról, hogy a középső API-alkalmazásban, és nem az előtér-web App alkalmazásban a megadott a CORS beállítás.

* CORS alkalmazás kódot és Azure alkalmazás szolgáltatást is használja konfigurálásáról, ne feledje, hogy a az alkalmazás szolgáltatás CORS beállítása érvényteleníti, az alkalmazás kódja csinál függetlenül. 

Ha többet szeretne megtudni a Visual Studio funkciók, amelyek megkönnyítik a hibaelhárítás, című [hibaelhárító Azure alkalmazás szolgáltatás a Visual Studióban](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Következő lépések 

Ebben a cikkben látta alkalmazás szolgáltatás CORS támogatás engedélyezése a, hogy az ügyfél JavaScript-kód egy API-t másik tartományt is felhívhatja. API-alkalmazásokkal kapcsolatos további információért olvassa el a [hitelesítési App szolgáltatásban – bevezetés](../app-service/app-service-authentication-overview.md), és kattintson a [felhasználói hitelesítés az API-alkalmazások](app-service-api-dotnet-user-principal-auth.md) oktatóprogram.
