<properties
    pageTitle="Felhasználói hitelesítés az Azure-App szolgáltatásban API-alkalmazások |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure alkalmazás szolgáltatás API-alkalmazás védelme azáltal, hogy csak hitelesített felhasználó férhet hozzá."
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
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Az Azure-App szolgáltatásban API-alkalmazások a felhasználói hitelesítés

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan védelme az Azure API-alkalmazásokban, úgy, hogy csak a hitelesített felhasználók is hívja meg. A cikk tartalma feltételezi, hogy elolvasta az [Azure alkalmazás szolgáltatás hitelesítési – áttekintés](../app-service/app-service-authentication-overview.md).

Dióhéjban:

* Hogyan lehet egy hitelesítési szolgáltató konfigurálása az Azure Active Directory (Azure Active Directory) adataival.
* Hogyan lehet a védett API-at felhasználása az [Active Directory Authentication tár (ADAL) JavaScript-](https://github.com/AzureAD/azure-activedirectory-library-for-js)használatával.

A cikk két szakaszból áll:

* A [felhasználói hitelesítés Azure alkalmazás szolgáltatás konfigurálása](#authconfig) című szakaszt általános megtudhatja, hogyan bármely API-alkalmazásokban a felhasználói hitelesítés konfigurálása és alkalmazás szolgáltatás, beleértve a .NET, Node.js és Java által támogatott összes keretek egyaránt vonatkozik.

* A [.NET API-alkalmazások oktatóanyagok Folytatás](#tutorialstart) csoportban a cikk útmutatók kezdődő végig a minta alkalmazások konfigurálása és a .NET vége készíteni, és egy AngularJS elülső befejezése, hogy az Azure Active Directory számára a felhasználói hitelesítés használ. 

## <a id="authconfig"></a>Felhasználói hitelesítés konfigurálása a Azure alkalmazás szolgáltatás

Ez a témakör az általános útmutató lehetőséget, amely bármely API-alkalmazásra vonatkozik. A lépések adott kattintva tegye lista .NET minta alkalmazásával lépjen [az első lépések .NET oktatóanyagok a Folytatás](#tutorialstart).

1. Az [Azure portálon](https://portal.azure.com/)nyissa meg az API-alkalmazás kívánt védelme, keresse meg a **szolgáltatások** szakaszt, és kattintson a **Beállítások** lap **hitelesítési / engedélyezési**.

    ![Azure portál hitelesítési/engedély](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Az a **hitelesítési / engedélyezési** lap, kattintson **a**.

4. Válasszon a lehetőségek közül a **műveleteket, ha nem lehet kérelem hitelesíteni** legördülő listából.

    * Ha azt szeretné, hogy csak hitelesített hívások elérje az API-alkalmazást, válassza a **... Jelentkezzen be a** lehetőségek közül. Ez a beállítás lehetővé teszi az API-alkalmazás védelme akkor futtató kódírás nélkül.

    * Ha azt szeretné, hogy elérje az API-alkalmazás az összes hívást, válassza a **(semmi) kérés engedélyezése**. Ez a beállítás nem hitelesített hívók egy hitelesítésszolgáltatók megválasztása irányítsa át is használhatja. Ezt a beállítást választja, kell kódírás engedélyezési kezelheti.

    További tudnivalókért lásd: a [hitelesítési és az API-alkalmazások Azure alkalmazás szolgáltatás engedélyezési](app-service-api-authentication.md#multiple-protection-options).

5. Jelölje ki a **Hitelesítésszolgáltatók**közül.

    A képen látható, választási lehetőségek, hogy az Azure Active Directory hitelesítse összes hívóknak.
 
    ![Azure portál hitelesítési/engedélyezési lap](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Ha úgy dönt, hogy egy hitelesítési-szolgáltatóval, a portálon a konfigurációs a lap adott szolgáltató jeleníti meg. 

    Részletes útmutató mutatják be, hogy miként konfigurálható hitelesítési szolgáltató konfigurációs rögzítéséhez megtudhatja, [hogy miként konfigurálása, az alkalmazás Azure Active Directory bejelentkezési használni](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (A hivatkozás ugrik egy cikk Azure Active Directory kapcsolatban, de magát a cikk tartalmaz, amely a többi hitelesítésszolgáltatók for cikkekre mutató hivatkozás lapok.)

7. Amikor elkészült a hitelesítési szolgáltató konfigurációs lap, kattintson az **OK gombra**.

7. Az a **hitelesítési / engedélyezési** lap, kattintson a **Mentés**gombra.

Ez történik, ha az alkalmazás szolgáltatás hitelesíti API-hívások elérése előtti az API-alkalmazás. A hitelesítési szolgáltatások alkalmazás szolgáltatás által támogatott, beleértve a .NET, Node.js és Java az összes nyelvhez ugyanúgy működik. 

Hitelesített API hívásokat, a hívó tartalmaz a hitelesítési szolgáltató 2.0-s OAuth bearer jogkivonathoz HTTP-kérések engedélyezése fejlécében. A token szerezhető a hitelesítési szolgáltató SDK használatával.

## <a id="tutorialstart"></a>A .NET API-alkalmazások oktatóanyagok folytatása

Ha a Node.js vagy Java oktatóanyagok az API-alkalmazások követik, ugorja át a következő cikket, [API-appok fő hitelesítési szolgáltatás](app-service-api-dotnet-service-principal-auth.md). 

Ha követik a .NET oktatóanyag sorozat-API-alkalmazást, és már telepítette a minta alkalmazást, az [első](app-service-api-dotnet-get-started.md) és [második](app-service-api-cors-consume-javascript.md) oktatóanyag utasításai, ugorjon a [állítsa be az alkalmazás szolgáltatás és az Azure Active Directory authentication](#azureauth) szakaszban.

Ha meg szeretné kövesse az ebben az oktatóanyagban anélkül, hogy az első és második oktatóanyagok keresztül, végezze el a következő lépések, amelyek bemutatják, hogyan veheti használatba az automatizált eljárással segítségével a minta alkalmazás telepítéséhez.

>[AZURE.NOTE] Az alábbi lépésekkel eléréséhez, azonos kiindulási pontként, mintha az elvégzett az első két oktatóanyag, egy kivétellel: a Visual Studio már nem tudja, melyik web appot vagy az egyes projektek kap telepített API-alkalmazás. Ez azt jelenti, hogy ne kelljen a pontos utasításokat az oktatóprogram ismertetik a megfelelő célok telepítéshez használni. Ha nem tudja, hogy miként végezze el a telepítési lépéseket az önálló gondot, érdemes az oktatóprogram-sorozat végezze el az [első oktatóprogram](app-service-api-dotnet-get-started.md) mint indítsa el a automatikus telepítési folyamatot.

1. Győződjön meg arról, hogy az összes a szerepel a felsorolásban, az [első oktatóprogram](app-service-api-dotnet-get-started.md)előfeltételek. Mellett a Előfeltételek szerepel a felsorolásban hitelesítési oktatóanyagokban feltételezik, hogy az alkalmazás szolgáltatás web Apps alkalmazások és a Visual Studio és az Azure portál API-alkalmazások módosítottuk.

2. A **központi telepítés az Azure** gombra a [fontos tárházba tegye lista mintafájl](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) az API-alkalmazások és a web app telepítését. Jegyezze fel az Azure erőforráscsoport kap létrehozni, mert ezzel újabb lépések elvégzésével keresheti meg a web app és az API-alkalmazás nevét.
 
3. Töltse le vagy a [Teendőlista minta tárházba](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) fogja használatakor helyileg a Visual Studióban kódszámú klónozhatja.

## <a id="azureauth"></a>Az alkalmazás szolgáltatás és az Azure Active Directory authentication beállítása

Ekkor a anélkül, hogy a felhasználók sikerült hitelesíteni Azure App szolgáltatásban alkalmazást. Ebben a szakaszban hozzáadja hitelesítési hajtsa végre az alábbi műveleteket:

* Azure Active Directory (Azure Active Directory) hitelesítést igényel a hívásához a középső API-alkalmazás alkalmazás szolgáltatás konfigurálása.
* Azure AD-alkalmazás létrehozása.
* Konfigurálja az Azure AD-alkalmazást, az bearer jogkivonathoz AngularJS előtér bejelentkezés után küldhet. 

Ha problémákat tapasztal utasításokat az oktatóprogram során, az oktatóanyag végén a [hibaelhárítási](#troubleshooting) témakörben talál. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>A középső API-alkalmazás hitelesítés konfigurálása

1. Az [Azure portálon](https://portal.azure.com/)nyissa meg a **Beállítások** lap az Ön által létrehozott API-alkalmazás a ToDoListAPI projekt, keresse meg a **szolgáltatások** szakaszt, és kattintson **hitelesítési / engedélyezési**.

    ![Azure portál hitelesítési/engedély](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Az a **hitelesítési / engedélyezési** lap, kattintson **a**.

4. A **kérés nem hitelesítése esetén végrehajtandó művelet** legördülő listában válassza a **Jelentkezzen be az Azure Active Directory**.

    Ezt a beállítást biztosítja, hogy nincs unauathenticated kéréseket elérje az API-alkalmazást. 

5. **Hitelesítésszolgáltatók**csoportban kattintson az **Azure Active Directory**.

    ![Azure portál hitelesítési/engedélyezési lap](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Az **Azure Active Directory-beállítások** lap válassza az **Express**

    ![Azure portál hitelesítési/engedélyezési Express beállítás](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    A **Express** lehetőség alkalmazás szolgáltatás automatikusan létrehozhat az Azure Active Directory-alkalmazások az Azure AD- [bérlő](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nem kell hozzon létre egy bérlői, mivel minden Azure-fiók automatikusan rendelkezik.

7. A **felügyeleti mód**területen kattintson az **Új Active Directory-alkalmazás létrehozása** , ha még nincs bejelölve, és figyelje meg az érték, amely a **Alkalmazás létrehozása** mezőben; AAD alkalmazás az Azure klasszikus portálon később fogja keresése.

    ![Azure portál Azure AD-beállítások](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure az Azure Active Directory-alkalmazások automatikusan létrehozza a Azure AD-bérlő jelzett nevű. Alapértelmezés szerint az Azure Active Directory-alkalmazás neve megegyezik az API-alkalmazást. Ha inkább, egy másik nevet adhat meg.
 
7. Kattintson az **OK gombra**.

7. Az a **hitelesítési / engedélyezési** lap, kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Most már csak azok a felhasználók az Azure Active Directory-ös bérlőben az API-alkalmazás is felhívhatja.

### <a name="optional-test-the-api-app"></a>Nem kötelező: Tesztelje az API-alkalmazás

1. A böngészőben nyissa meg az URL-címet az API-alkalmazás: az **API-alkalmazás** fel az Azure-portálra, kattintson a hivatkozás **URL-címe**alatt.  

    Megnyílik a egy bejelentkezési képernyője, mert nem hitelesített kérelmeket elérje az API-alkalmazás nem engedélyezettek.

    Ha a böngészőben a "sikeres létrehozott" lapra ugrik, a böngészőben előfordulhat, hogy már naplózza a--ebben az esetben nyisson meg egy InPrivate- vagy Incognito ablakot, és nyissa meg az API-app URL-címe.

2. Jelentkezzen be a Azure AD-bérlő a felhasználó hitelesítő adataival.

    Ha be van jelentkezve, megjelenik a "sikeres létrehozott" lap a böngészőben.

9. A böngésző bezárásával.

### <a name="configure-the-azure-ad-application"></a>Az Azure Active Directory-alkalmazás beállítása

Azure Active Directory authentication konfigurálásakor alkalmazás szolgáltatás létrehoz egy Azure AD-alkalmazást. Az új Azure Active Directory alapértelmezés szerint az alkalmazás ahhoz, hogy az bearer jogkivonathoz az API-app URL-címe lett beállítva. Ebben a szakaszban adja meg az bearer jogkivonathoz AngularJS első végére helyett közvetlenül a középső API-alkalmazást az Azure Active Directory alkalmazása konfigurálása. AngularJS előtér a token küld az API-alkalmazásba, amikor, hívja az API-alkalmazást.

>[AZURE.NOTE] A folyamat tartani a lehető egyszerű, egyetlen Azure Active Directory ezen oktatóprogram használó az előtér és a középső alkalmazás első csoportba tartozó API-alkalmazást. Egy másik, hogy két Azure AD-alkalmazások használatát. Ebben az esetben azt szeretné, hogy az előtér Azure Active Directory alkalmazás szóló engedély megadása a középső Azure AD-alkalmazás hívja. A több alkalmazást módon szeretné ad engedéllyel minden réteg pontosabban szabályozható.

11. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)lépjen az **Azure Active**Directory.

    Meg kell használni a klasszikus portált, mert bizonyos Azure Active Directory-beállításait hozzáféréssel kell rendelkeznie, amely még nem érhetők el az aktuális Azure-portálon.

12. A **könyvtár** lapon kattintson a AAD bérlő.

    ![Azure Active Directory klasszikus portálon](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Kattintson a **alkalmazások > alkalmazások vállalaton tulajdonosa**, és kattintson a pipára.

    Lehet, is, ha frissíti a lapot, hogy az új alkalmazás.

15. Kattintson az alkalmazások listájában, amely az Azure jött létre, ha engedélyezte a hitelesítést az API-alkalmazás nevét.

    ![Azure Active Directory-alkalmazások lapján](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Kattintson a **konfigurálása**.

    ![Azure Active Directory konfigurálása lapon](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. **Bejelentkezési URL** állítsa a AngularJS web App alkalmazásban nem ferde URL-CÍMÉT.

    Példa: https://todolistangular.azurewebsites.net

    ![Bejelentkezés URL-címe](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. **Válasz URL-CÍMÉT** az URL-címet a web App, nem ferde beállítása

    Példa: https://todolistsangular.azurewebsites.net

16. Kattintson a **Mentés**gombra.

    ![Válasz URL-címe](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. A lap alján kattintson a **kezelése jegyzék > Letöltés jegyzék**.

    ![Jegyzék letöltése](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Töltse le a helyet, ahol szerkeszteni tudja azt a fájlt.

16. A letöltött nyilvánvalóan fájl keressen rá a `oauth2AllowImplicitFlow` tulajdonság. Ez a tulajdonság értékét módosítása `false` való `true`, majd mentse a fájlt.

    Ez a beállítás szükség az access JavaScript-egyoldalas alkalmazásból. Lehetővé teszi a 2.0-s Oauth bearer jogkivonathoz az URL-cím fragment vissza.

16. Kattintson a **kezelése jegyzék > feltöltés jegyzék**, és töltse fel a fájlt, az előző lépésben módosított.

    ![Töltse fel a jegyzék](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Másolja az **Ügyfél-azonosító** értéket, és mentse azon elemére kattint, az újabb.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Hitelesítés használandó ToDoListAngular projekt konfigurálása

Ebben a szakaszban módosíthatja a AngularJS előtér, hogy az Active Directory Authentication tár (ADAL) JS-szerezheti be a bejelentkezett felhasználó Azure AD egy bearer jogkivonathoz használ. A kód tartalmazni fogja a token HTTP összehívások, a középső, a következő ábrán látható módon. 

![Diagram a felhasználói hitelesítés](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

A következő módosításokat az ToDoListAngular projekt fájlok.

1. Nyissa meg a *index.html* fájlt.

2. Vegye ki a megjegyzésjeleket a sorokat, amelyek az Active Directory hitelesítési tár (ADAL) JS parancsfájlok hivatkoznak.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Nyissa meg a *app/scripts/app.js* fájlt.

2. Megjegyzések hozzáfűzése lévő időtartomány hitelesítéshez"nem" megjelölt kódot, és vegye ki a megjegyzésjeleket hitelesítéshez"a" megjelölt kód lévő időtartomány.

    Ez a változás az ADAL JS hitelesítési szolgáltató hivatkozik, és a konfigurációs értékeket ad. Az alábbi lépésekkel beállíthatja, a API-alkalmazást, és Azure Active Directory-alkalmazás konfigurációs értékeket.

8. A kód, amely akkor állítja be a `endpoints` változó, API URL-CÍMÉT az URL-címe értékűre az API-alkalmazás, a ToDoListAPI project által létrehozott, és állítsa az ügyfél-azonosító az Azure klasszikus portálról kimásolt az Azure Active Directory azonosítója.

    Ettől a következőhöz hasonló kódot.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. A hívás `adalProvider.init`, állítsa be `tenant` meg a bérlői webhely nevét és `clientId` használta az előző lépésben azonos értéket.

    Ettől a következőhöz hasonló kódot.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    A módosítások, hogy `app.js` adja meg, hogy a hívó kódot és a hívott API ugyanabban az Azure Active Directory-alkalmazásban.

1. Nyissa meg a *app/scripts/homeCtrl.js* fájlt.

2. Megjegyzések hozzáfűzése lévő időtartomány hitelesítéshez"nem" megjelölt kódot, és vegye ki a megjegyzésjeleket hitelesítéshez"a" megjelölt kód lévő időtartomány.

1. Nyissa meg a *app/scripts/indexCtrl.js* fájlt.

2. Megjegyzések hozzáfűzése lévő időtartomány hitelesítéshez"nem" megjelölt kódot, és vegye ki a megjegyzésjeleket hitelesítéshez"a" megjelölt kód lévő időtartomány.

### <a name="deploy-the-todolistangular-project-to-azure"></a>Telepítse az ToDoListAngular projektet az Azure

8. A **Megoldás Explorer**kattintson a jobb gombbal a ToDoListAngular projekt, és ezután kattintson a **Közzététel**gombra.

9. Kattintson a **Közzététel**gombra.

    Visual Studio üzembe helyezése a projektet, és megnyílik a böngésző címsorába a web app alap URL-címe. Ez egy 403 hiba lapot, amely normál kísérlet nyissa meg a webes API alap URL-címe a böngészőben az jelennek meg.

    Továbbra is lehet módosítani a középső API-alkalmazás előtt ellenőrizheti, hogy az alkalmazás.

10. A böngésző bezárásával.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Hitelesítés használandó ToDoListAPI projekt konfigurálása

Jelenleg a ToDoListAPI projekt küldése "*", a `owner` ToDoListDataAPI értéket. Ebben a szakaszban módosítsa a kódot, és küldje el a bejelentkezett felhasználó azonosítója.

A következő módosításokat az ToDoListAPI projektben.

1. Nyissa meg a *Controllers/ToDoListController.cs* fájlt, és vegye ki a megjegyzésjeleket minden egyes művelet módszer, amely akkor állítja be a sor `owner` az Azure ad `NameIdentifier` formál értéket. Példa:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Fontos**: ne vegye ki a megjegyzésjeleket a kódot a `ToDoListDataAPI` módszer; fogja megteheti, hogy később a a szolgáltatás fő hitelesítési tanulásra.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Telepítse az ToDoListAPI projektet az Azure

8. A **Megoldás Explorer**kattintson a jobb gombbal a ToDoListAPI projekt, és kattintson a **Közzététel**gombra.

9. Kattintson a **Közzététel**gombra.

    Visual Studio üzembe helyezése a projektet, és megnyílik a böngésző címsorába az API-alkalmazás alap URL-címe.

10. A böngésző bezárásával.

### <a name="test-the-application"></a>Az alkalmazás tesztelése

9. Nyissa meg a web app, **a HTTPS nem HTTP**URL-CÍMÉT.

8. Kattintson a **Teendőlista** fülre.

    Jelentkezzen be az kéri.

9. Jelentkezzen be a AAD bérlő a felhasználó hitelesítő adatait.

10. A **Teendőlista** lap jelenik meg.

    ![A lista, oldal](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Nincs teendők jelennek meg, mert az eddig összes voltak tulajdonos "*". Most már a középső kér a bejelentkezett felhasználó elemet, és nincs még készült.

11. Adja hozzá az új Teendők, ellenőrizze, hogy működik-e az alkalmazást.

12. Egy másik böngészőablakban, nyissa meg a Swagger felhasználói felület URL-CÍMÉT a ToDoListDataAPI API-alkalmazást, és kattintson a **listájába > első**. Írja be egy csillag karaktert a `owner` paraméter, és kattintson a **próbálja ki**.

    A válasz azt mutatja, hogy az új Teendők a tényleges Azure Active Directory felhasználói azonosító mező tulajdonságlapján a tulajdonosa.

    ![JSON válaszként tulajdonosához azonosító](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>A projektek nulláról létrehozása

A két webes API-projektet az **Azure API alkalmazás** project-sablon használatával, és az alapértelmezett értékek vezérlő cseréje egy listájába vezérlővel hozta létre. 

Információ arról, hogy miként egy AngularJS egyoldalas-alkalmazás létrehozása a webes API 2 vissza vége [kéz a labor: az ASP.NET webes API-val és a Angular.js egyetlen lap alkalmazás (egészében) összeállítása](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Azure AD-hitelesítés kód olvashat az alábbi forrásokban talál:

* [Biztonságossá tétele AngularJS egyetlen lap alkalmazások az Azure AD](../active-directory/active-directory-devquickstarts-angular.md)-e.
* [ADAL JS v1 bemutatása](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Hibaelhárítás

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy ne tévessze össze ToDoListAPI (középső) és ToDoListDataAPI (adatok réteg). Például győződjön meg arról, hogy hozzáadta a középső API-alkalmazásba, nem az adatok réteg hitelesítési. 
* Győződjön meg arról, hogy a AngularJS forráskód hivatkozik, a középső API app URL-címe (ToDoListAPI, nem ToDoListDataAPI) és a megfelelő Azure Active Directory ügyfél-azonosítóval. 

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban szüksége a tanultakhoz alkalmazás szolgáltatás hitelesítési használatáról az API-alkalmazásokban és az API-alkalmazás hívás a ADAL JS tár használatával. A következő oktatóanyagból megismerheti, hogyan [az API-alkalmazást, a szolgáltatás felhasználási területei való hozzáférés biztonságossá tétele](app-service-api-dotnet-service-principal-auth.md)érdekében.

