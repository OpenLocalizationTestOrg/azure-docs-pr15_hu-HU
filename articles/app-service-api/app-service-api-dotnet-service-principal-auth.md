<properties
    pageTitle="Szolgáltatás fő hitelesítési az Azure-App szolgáltatásban API-alkalmazások |} Microsoft Azure"
    description="Megtudhatja, hogy miként védelme a szolgáltatás felhasználási területei Azure App szolgáltatásban API alkalmazás."
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

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Szolgáltatás fő hitelesítési az Azure-App szolgáltatásban API-alkalmazások

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan alkalmazás szolgáltatás hitelesítési használható a *belső* hozzáférhet az API-alkalmazásokhoz. Egy belső forgatókönyv, ahol van, amely csak a saját alkalmazások kóddal felhasználható felvenni kívánt API alkalmazás. Az ajánlott módszereket, ebben az esetben az alkalmazás szolgáltatás végrehajtásához környezetbe Azure AD a hívott API-alkalmazás védelmét. A védett API-alkalmazás egy bearer jogkivonathoz, amely Azure AD, mert az alkalmazás azonosító (tőke szolgáltatás) hitelesítő adatait el a hívható meg. Azure Active Directory használatával kiváltása című **szolgáltatás hitelesítés** az [Azure alkalmazás szolgáltatás hitelesítési áttekintése](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

Ebben a cikkben megismerheti:

* Hogyan lehet védelme az API-alkalmazásokban nem hitelesített access-fájlból az Azure Active Directory (Azure Active Directory) segítségével.
* Hogyan lehet a védett API-at API-alkalmazást, webalkalmazás, vagy mobilalkalmazásban felhasználása az Azure Active Directory szolgáltatás fő (alkalmazás azonosító) hitelesítő adataival. Logika alkalmazásból felhasználása olvashat lásd: [az egyéni API is logika alkalmazással alkalmazás szolgáltatás használata](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Hogyan lehet győződjön meg arról, hogy a védett API-alkalmazás nem hívható böngészőben bejelentkezett felhasználó.
* Hogyan kattintva győződjön meg arról, hogy a védett API-app csak hívható által egy adott Azure Active Directory szolgáltatás fő.

A cikk két szakaszból áll:

* A [szolgáltatás fő hitelesítési Azure alkalmazás szolgáltatás konfigurálása](#authconfig) a szakaszból általános bármely API-alkalmazásokban hitelesítés konfigurálása és a felhasználni a védett API-alkalmazást. Ez a szakasz egyaránt alkalmazás szolgáltatás, beleértve a .NET, Node.js és Java által támogatott összes keretek érinti.

* A [folyamatos az első lépések .NET oktatóanyagok](#tutorialstart) szakasz kezdve, az oktatóprogram végigvezeti Önt beállítása egy "belső access" forgatókönyv alkalmazás szolgáltatást futtató .NET minta alkalmazáshoz. 

## <a id="authconfig"></a>Egyszerű hitelesítési szolgáltatás konfigurálása a Azure alkalmazás szolgáltatás

Ez a témakör az általános útmutató lehetőséget, amely bármely API-alkalmazásra vonatkozik. A lépések adott kattintva tegye lista .NET minta alkalmazásával lépjen a [Folytatás a .NET API-alkalmazások oktatóprogram-sorozat](#tutorialstart).

1. Az [Azure portálon](https://portal.azure.com/)nyissa meg az API-alkalmazás, amelyet védeni, és keresse meg a **szolgáltatások** szakaszt vagy kattintson a **Beállítások** lap **hitelesítési / engedélyezési**.

    ![Hitelesítés és engedélyezési az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Az a **hitelesítési / engedélyezési** lap, kattintson **a**.

4. A **kérés nem hitelesítése esetén végrehajtandó művelet** legördülő listában válassza a **Jelentkezzen be az Azure Active Directory** .

5. **Hitelesítésszolgáltatók**csoportban jelölje be az **Azure Active Directory**.

    ![Hitelesítés és engedélyezési lap az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Állítsa be az **Azure Active Directory-beállítások** lap, hozzon létre egy új Azure AD-alkalmazást, vagy egy meglévő Azure Active Directory alkalmazást, ha már van, amely a használni kívánt kell használnia.

    Belső esetek általában az API-alkalmazásokban hívásához API alkalmazás foglalnak magukban. Használhatja a külön Azure Active Directory-alkalmazások minden API-alkalmazásokban vagy csak egy Azure AD-alkalmazás.

    Részletes Ez a lap, tanulmányozza [az alkalmazás szolgáltatásalkalmazás használatához az Azure Active Directory bejelentkezési konfigurálásáról](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Amikor elkészült a hitelesítési szolgáltató konfigurációs lap, kattintson az **OK gombra**.

7. Az a **hitelesítési / engedélyezési** lap, kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Ez történik, amikor alkalmazás szolgáltatás csak lehetővé teszi, hogy kérelmek a hívók a beállított az Azure AD-bérlő. A védett API alkalmazásban nincs hitelesítő kód szükséges. Az bearer jogkivonathoz átadott az API-alkalmazás együtt gyakran használt követelések a HTTP-fejlécek, és ezeket az információkat, a kód ellenőrzéséhez kérések az egy adott hívó, például egy egyszerű erről.

Ez a hitelesítési szolgáltatás alkalmazás szolgáltatás által támogatott, beleértve a .NET, Node.js és Java az összes nyelvhez ugyanígy működik. 

#### <a name="how-to-consume-the-protected-api-app"></a>Hogyan kell a védett API-alkalmazás felhasználása

A hívó biztosítania kell az Azure Active Directory bearer jogkivonathoz az API-hívások. A hívó lépni egy bearer jogkivonathoz szolgáltatás fő hitelesítő adataival, használja az Active Directory Authentication Library ( [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)vagy [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)ADAL). Úgy juthat az jogkivonat, a kód, amely felhívja ADAL az ADAL az alábbi információk találhatók:

* A Azure AD-bérlő neve.
* Az ügyfél-azonosító és a ügyfél titkos (alkalmazás kulcs) az Azure Active Directory-alkalmazás társított kíván a hívó féllel.
* A védett API-alkalmazással társított az Azure Active Directory-alkalmazás ügyfél-azonosító. (Csak egy Azure AD-alkalmazást használja, ha ez a kíván a hívó féllel mint a azonos ügyfél-azonosító.)

Ezeket az értékeket az Azure Active Directory lapján az [Azure klasszikus portál](https://manage.windowsazure.com/)érhetők el.

Miután beszerzett a jogkivonat, a hívó tartalmazza az engedélyezés fejlécében HTTP kéréseivel.  Alkalmazás szolgáltatás ellenőrzi a jogkivonat, és lehetővé teszi, hogy a kérelmek elérje a védett API-alkalmazást.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Az API-alkalmazás védelmét ugyanahhoz a bérlőhöz felhasználók által az access-fájlból

Felhasználók ugyanahhoz a bérlőhöz bearer tokenek érvényes, a védett API-alkalmazás számítanak.  Ha szeretné biztosítani, hogy csak egy egyszerű felhívhatja, ha a védett API-alkalmazást, adja meg a védett API-alkalmazás a következő jogcímalapú a token az ellenőrizendő kódot:

* `appid`az ügyfél-azonosító az Azure Active Directory-alkalmazás a hívó társított kell lennie. 
* `oid`(`objectidentifier`) kell lennie a hívó azonosítója szolgáltatás fő. 

Alkalmazás szolgáltatás is biztosít a `objectidentifier` formál az X-MS-ügyfél-egyszerű-ID fejlécében.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Az API-alkalmazás védelmét a elérés webböngészővel

Ha nem érvényesítés követelések a kódot a védett API-alkalmazásban, és ha külön Azure AD-alkalmazás a védett API-alkalmazásokból, győződjön meg róla, hogy az Azure Active Directory-alkalmazás válasz URL-címe nem ugyanaz, mint az API-alkalmazás alap URL-címe. A válasz URL-cím közvetlenül a védett API-alkalmazás mutat, ha egy felhasználó ugyanahhoz a bérlőhöz Azure Active Directory sikerült tallózással keresse meg az API-alkalmazás, jelentkezzen be, és sikeresen hívja fel az API-t.

## <a id="tutorialstart"></a>A .NET API-alkalmazások oktatóprogram-sorozat folytatása

Ha az API-alkalmazások Node.js vagy Java oktatóanyag sorozat követik, a [következő lépésekkel](#next-steps) szakasz szakaszhoz. 

Ez a cikk hátralévő továbbra is a .NET API-alkalmazások oktatóprogram-sorozat és feltételezi, hogy a [felhasználói hitelesítés oktatóprogram](app-service-api-dotnet-user-principal-auth.md) kitöltötte és Azure-ban engedélyezett felhasználói hitelesítéssel a minta alkalmazást.

## <a name="set-up-authentication-in-azure"></a>Azure-ban hitelesítés beállítása

Ebben a részben alkalmazás szolgáltatás konfigurálása, hogy a csak a HTTP-kérések lehetővé teszi az adatok réteg API alkalmazás eléréséhez lépnek, amelyek érvényes Azure Active Directory bearer tokenek. 

A következő szakaszban beállíthatja a középső API-alkalmazás alkalmazás hitelesítő adatokat küldeni az Azure Active Directory, visszatérhet a bearer jogkivonathoz és küldje el az bearer jogkivonathoz az adatok réteg API-alkalmazást. Ez a folyamat menetét az a diagramot.

![Szolgáltatás-hitelesítés diagram](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Ha problémákat tapasztal utasításokat az oktatóprogram során, az oktatóanyag végén a [hibaelhárítási](#troubleshooting) témakörben talál. 

1. Az [Azure portálon](https://portal.azure.com/)nyissa meg a **Beállítások** lap az Ön által létrehozott API-alkalmazás API alkalmazás ToDoListDataAPI (adatok réteg), és válassza a **Beállítások**.

2. A **Beállítások** lap, keresse meg a **szolgáltatások** szakaszt, és kattintson **hitelesítési / engedélyezési**.

    ![Hitelesítés és engedélyezési az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. Az a **hitelesítési / engedélyezési** lap, kattintson **a**.

4. A **kérés nem hitelesítése esetén végrehajtandó művelet** legördülő listában válassza a **Jelentkezzen be az Azure Active Directory**.

    Ez a beállítás hatására az alkalmazás a szolgáltatást, győződjön meg arról, hogy csak hitelesített kérések vannak az API-alkalmazást. Érvényes bearer tokenek rendelkező kérelmekhez alkalmazás szolgáltatás a tokenek mentén továbbítja az API-alkalmazást, és feltölti a gyakran használt jogcímalapú ezeket az információkat a kódot is könnyen elérhetővé tétele a HTTP-fejlécek.

5. **Hitelesítésszolgáltatók**csoportban kattintson az **Azure Active Directory**.

    ![Hitelesítés és engedélyezési lap az Azure-portálon](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Kattintson az **Azure Active Directory-beállítások** lap **Express**.

    **Express** beállítás Azure automatikusan létrehozhat egy AAD alkalmazást a Azure AD- [bérlő](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nem kell hozzon létre egy bérlői, mivel minden Azure-fiók automatikusan rendelkezik.

7. **Kezelési módot**kattintson az **Új Active Directory-alkalmazás létrehozása** , ha még nincs bejelölve.

    A portál csatlakozik a **Alkalmazás létrehozása** beviteli mezőben alapértelmezett értékű. Alapértelmezés szerint az Azure Active Directory-alkalmazás neve megegyezik az API-alkalmazást. Ha inkább, egy másik nevet adhat meg.
    
    ![Azure Active Directory-beállítások](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Megjegyzés**: alternatívájaként használhatja is egy egyetlen Azure Active Directory mind a hívó API-val és a védett API alkalmazást az alkalmazást. Ha úgy döntött, hogy alternatív, nem kellene itt az **Új Active Directory-alkalmazás létrehozása** beállítás, mert már létrehozott egy Azure AD-alkalmazást a felhasználói hitelesítés oktatóprogram során. Az ebben az oktatóanyagban kell megadnia a hívó API-val és a védett API-alkalmazást az Azure Active Directory-alkalmazások külön.

8. Jegyezze le a típusú érték, amely a **Alkalmazás létrehozása** beviteli mezőben; AAD alkalmazás az Azure klasszikus portálon később fogja keresése.

7. Kattintson az **OK gombra**.

10. Az a **hitelesítési / engedélyezési** lap, kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    Alkalmazás szolgáltatás az Azure Active Directory alkalmazás **bejelentkezési URL-** címmel hoz létre, és automatikusan állítsa **Válasz URL-CÍMÉT** az API-alkalmazás URL-CÍMÉT. Az utóbbi érték lehetővé teszi a felhasználóknak a AAD bérlői webhelyén jelentkezzen be, és az API-alkalmazás eléréséhez.

### <a name="verify-that-the-api-app-is-protected"></a>Ellenőrizze, hogy az API-alkalmazás védett

1. A böngészőben nyissa meg az URL-címet az API-alkalmazás: az **API-alkalmazás** fel az Azure-portálra, kattintson a hivatkozás **URL-címe**alatt. 

    Megnyílik a egy bejelentkezési képernyője, mert nem hitelesített kérelmeket elérje az API-alkalmazás nem engedélyezettek. 

    A böngészőben lépjen a Swagger felhasználói felület, ha a böngésző már kerülhet naplózásra tovább--ebben az esetben nyisson meg egy InPrivate- vagy Incognito ablakot, és nyissa meg a Swagger felhasználói felület URL-CÍMÉT.

18. Jelentkezzen be a AAD bérlő a felhasználó hitelesítő adatait.

    Ha be van jelentkezve, megjelenik a "sikeres létrehozott" lap a böngészőben.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>A ToDoListAPI projekt szerezheti be, és küldje el az Azure Active Directory jogkivonathoz konfigurálása

Ebben a szakaszban válasszon az alábbi műveleteket:

* Kód hozzáadása a középső API-alkalmazásban, amely a hitelesítő adatokkal Azure Active Directory alkalmazás szerez egy jogkivonat, és küldje el az HTTP kéréseivel az adatok réteg API alkalmazásba.
* Szükséges hitelesítő adatok lekérése az Azure Active Directory.
* Írja be az Azure alkalmazás szolgáltatás futtatókörnyezet környezeti beállítások a középső API-alkalmazás a hitelesítő adatait. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>A ToDoListAPI projekt szerezheti be, és küldje el az Azure Active Directory jogkivonathoz konfigurálása

A következő módosításokat az ToDoListAPI projektben, a Visual Studióban.

1. Vegye ki a megjegyzésjeleket összes a *ServicePrincipal.cs* fájlban a kódot.

    Ez a, amely a .NET rendszerhez ADAL használja az Azure Active Directory bearer jogkivonathoz szerezheti be a kódot.  Több konfigurációs értékek beállított lesz az Azure futtatókörnyezet később használ. A következő kódot: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Megjegyzés:** Ez a kód a ADAL .NET NuGet csomag (Microsoft.IdentityModel.Clients.ActiveDirectory), amely már telepítve van a projekt van szükség. Ehhez a projekthez nulláról hozott létre, ha azt szeretné, hogy a csomag telepítéséhez. A csomag nem automatikusan telepítődik az API alkalmazássablon új projektet.

2. A *Vezérlők/ToDoListController*, vegye ki a megjegyzésjeleket a kódot a `NewDataAPIClient` módszer a token ellátó HTTP kéri a engedélyezési fejlécében.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Telepítse az ToDoListAPI-projektet. (Kattintson a jobb gombbal a projektet, majd kattintson a **Közzététel > Közzététel**.)

    Visual Studio üzembe helyezése a projektet, és megnyílik a böngésző címsorába a web app alap URL-címe. Ez egy 403 hiba lapot, amely normál kísérlet nyissa meg a webes API alap URL-címe a böngészőben az jelennek meg.

4. A böngésző bezárásával.

### <a name="get-azure-ad-configuration-values"></a>Ismerkedés az Azure Active Directory konfigurációs értékek

11. Az [Azure klasszikus portálon](https://manage.windowsazure.com/)lépjen az **Azure Active**Directory.

12. A **címtár** lapon kattintson az AAD bérlői webhelyen.

14. Kattintson a **alkalmazások > alkalmazások vállalaton tulajdonosa**, és kattintson a pipára.

15. Az alkalmazások listájában kattintson az adott Azure-készült, ha engedélyezte a ToDoListDataAPI (adatok réteg) API alkalmazáshoz hitelesítést nevére.

16. Kattintson a **beállítás** fülre.

5. Másolja az **Ügyfél-azonosító** értéket, és mentse valahol később juthatok hozzá a. 

8. Az Azure klasszikus portálon térjen vissza az alkalmazáslistából **vállalaton tulajdonában van**, és kattintson az Ön által létrehozott ToDoListAPI API-alkalmazás (az adott az előző oktatóanyagban hoztunk, nem az adott ebben az oktatóanyagban hoztunk) a középső AAD alkalmazásra.

16. Kattintson a **beállítás** fülre.

5. Másolja az **Ügyfél-azonosító** értéket, és mentse valahol később juthatok hozzá a.

6. A **billentyűparancsok**jelöljön ki **1 év** az **időtartam kiválasztása** legördülő listában.

6. Kattintson a **Mentés**gombra.

    ![Alkalmazás kulcs generálása](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. A kulcs értékét másolja a vágólapra, és mentse valahol később juthatok hozzá a.

    ![Új alkalmazás kulcs másolása](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>A középső API-alkalmazás futtatókörnyezet Azure AD-beállításainak konfigurálása

1. Az [Azure-portálra](https://portal.azure.com/), és nyissa meg az **API-alkalmazás** lap, amelyen a TodoListAPI (középső) projekt API-alkalmazásokban.

2. Kattintson a **Beállítások > alkalmazás beállításainak**.

3. Az **alkalmazás beállításai** csoportban adja meg a következő kulcsok és értékeket:

  	| **Kulcs** | IDA: hitelesítésszolgáltató |
  	|---|---|
  	| **Érték** | {az Azure Active Directory bérlői neve} https://Login.microsoftonline.com/ |
  	| **Példa** | https://Login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Kulcs** | IDA: ClientId |
  	|---|---|
  	| **Érték** | Ügyfél-azonosító a hívó alkalmazás (középső - ToDoListAPI) |
  	| **Példa** | 960adec2-b74a-484A-960adec2-b74a-484A |

  	| **Kulcs** | IDA: ClientSecret |
  	|---|---|
  	| **Érték** | Alkalmazás billentyűt a hívó alkalmazás (középső - ToDoListAPI) |
  	| **Példa** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Kulcs** | az erőforrás IDA: |
  	|---|---|
  	| **Érték** | A hívott alkalmazás az ügyfél-azonosító (adatok réteg - ToDoListDataAPI) |
  	| **Példa** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Megjegyzés**: A `ida:Resource`, győződjön meg arról, hogy a hívott alkalmazás **ügyfél-azonosító** , és nem az **Alkalmazás azonosítója URI**használja.

    `ida:ClientId`és `ida:Resource` különböző értékeket Ez az oktatóanyag mert használja külön az Azure Active Directory applicaations a középső és az adatok réteg. Ha egyetlen Azure AD-alkalmazás számára a hívó API-val és a védett API alkalmazást, használja az azonos értékkel mind `ida:ClientId` és `ida:Resource`.

    A kód beolvasása ezeket az értékeket, így azok a fájlt a projekt vagy az Azure futtatókörnyezet tárolhatók ConfigurationManager használja. ASP.NET-alkalmazások Azure alkalmazás szolgáltatás futtatásakor a környezeti beállítások automatikusan felülbírálása Web.config beállításait. Környezeti beállítások használata általában egy [egy fájlt összehasonlítva bizalmas adatokat tároló biztonságosabb módja](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Az alkalmazás tesztelése

1. A böngészőben nyissa meg a AngularJS előtér web app HTTPS URL-CÍMÉT.

2. Kattintson a **Teendőlista** fülre, majd jelentkezzen be a Azure AD-bérlő a felhasználó hitelesítő adatait. 

4. Adja hozzá a teendők ellenőrizze, hogy működik-e az alkalmazást.

    ![A lista, oldal](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Ha az alkalmazás nem működik a várt módon működik, győződjön meg róla minden az Azure-portálon megadott beállítás. Minden beállítás helyes jelenik meg, ha a [Hibaelhárítás](#troubleshooting) szakasz című részben ebben az oktatóanyagban.

## <a name="protect-the-api-app-from-browser-access"></a>Elérés webböngészővel védelme az API-alkalmazás

Ebben az oktatóanyagban létrehozott egy külön Azure AD-alkalmazás (adatok réteg) ToDoListDataAPI API-alkalmazásokban. Láthatta, amikor az alkalmazás szolgáltatás létrehoz egy AAD alkalmazás, mint oly módon, amely lehetővé teszi, hogy egy felhasználó, nyissa meg a böngészőben az API-alkalmazás URL-CÍMÉT, és jelentkezzen be konfigurálja az AAD alkalmazást. Ez azt jelenti, lehetséges, hogy a Azure AD-bérlő, nem csak egy egyszerű, szolgáltatás az API eléréséhez a végfelhasználó. 

Megakadályozhatja, hogy a böngészőben az access a védett API alkalmazásban programozás nélkül szeretné, ha a **Válasz URL-CÍMÉT** az AAD alkalmazásban módosíthatja, így különbözik a API-alkalmazás alap URL-címe. 

### <a name="disable-browser-access"></a>Elérés webböngészővel letiltása

1. A klasszikus portál **beállítása** lapon az TodoListService létrehozott AAD alkalmazásához módosíthatja a **Válasz URL-címe** mező értékét, hogy érvényes URL-t, de nem az API-app URL-címe.
 
2. Kattintson a **Mentés**gombra.

### <a name="verify-browser-access-no-longer-works"></a>Ellenőrizze, hogy már nem működik a elérés webböngészővel

Korábbi ellenőrizve, hogy láthatja el az API-app URL-címe böngészőben egyes felhasználói hitelesítő adatokkal való bejelentkezés. Ebben a részben, ellenőrizze, hogy ez már nem lehetséges. 

1. Egy új böngészőablakban nyissa meg az URL-címet az API-alkalmazás újra.

2. Amikor a rendszer erre kéri, jelentkezzen be.

3. Bejelentkezés sikerül, de hibalap vezet.

    Beállította az AAD alkalmazást, hogy a felhasználók AAD bérlőhöz nem jelentkezzen be, és az API elérése a böngészőből. Szolgáltatás fő token, ellenőrizheti a web app URL-címre, és további teendők felvétele segítségével az API-alkalmazás továbbra is elérheti.

## <a name="restrict-access-to-a-particular-service-principal"></a>Egy adott szolgáltatás egyszerű való hozzáférés korlátozása  

Most, hogy a felhasználó szerezhet be a token bármelyik hívó jobbra, vagy az Azure Active Directory-ös bérlőben szolgáltatás egyszerű felhívhatja a TodoListDataAPI (adatok réteg) API-alkalmazást. Érdemes lehet győződjön meg arról, hogy az adatok réteg API alkalmazás csak elfogadja a TodoListAPI (középső) API alkalmazásból, és csak egy adott szolgáltatás egyszerű érkező hívások. 

A korlátozások érvényesítéséhez statisztikai kód hozzáadásával is hozzáadhat a `appid` és `objectidentifier` követelések a bejövő hívásokat.

Ez az oktatóanyag-a kódot, amely ellenőrzi az alkalmazás és szolgáltatás fő azonosító közvetlenül a vezérlő végrehajtott műveletek a helyezi el.  Alternatívák állnak használni egy egyéni `Authorize` attribútum vagy kell elvégeznie az ellenőrzés az indítási sequences (pl. OWIN köztes). Példa az utóbbi olvassa el a [minta alkalmazás](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs)című témakört. 

A következő módosításokat az TodoListDataAPI projekt.

2. Nyissa meg a *Controllers/TodoListController.cs* fájlt.

3. Vegye ki a megjegyzésjeleket a sorokat, amelyeket `trustedCallerClientId` és `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Vegye ki a megjegyzésjeleket a CheckCallerId módszer a kódot. Ez a módszer az összes művelet módszer a vezérlő az elején nevezik. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Telepítsen újra a ToDoListDataAPI projekt Azure alkalmazás szolgáltatásba.

6. A böngészőben nyissa meg a AngularJS előtér web app HTTPS URL-CÍMÉT, és kattintson a Kezdőlap lap **Teendőlista** fülére.

    Az alkalmazás nem működik, mert a hívások átirányítása a háttér nem működnek. Az új kódot tényleges appid és objectidentifier ellenőrzi, de még nincsenek ellenőrizze ellen őket a megfelelő értékeket. A böngésző fejlesztői eszközökkel konzol azt jelenti, hogy a kiszolgáló HTTP 401 hibát ad vissza.

    ![Hiba a fejlesztői eszközökkel konzol](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    Az alábbi lépésekkel beállítja a várt értékek.

8. Azure Active Directory PowerShell használatával, a szolgáltatás egyszerű értékének beszerzése az Azure Active Directory-alkalmazás a TodoListWebApp projekt létrehozásához.

    egy. Azure PowerShell telepítése és csatlakoztatása az előfizetés, tanulmányozza [Az Azure erőforrás-kezelő Azure a PowerShell használatával](../powershell-azure-resource-manager.md).

    b. Lista beszerzése az szolgáltatás rendszerbiztonsági, hajtsa végre a `Login-AzureRmAccount` parancs és a `Get-AzureRmADServicePrincipal` parancsot.

    c billentyűkombinációt. Objektumazonosító keresse meg a szolgáltatás egyszerű TodoListAPI alkalmazását, és mentse a másolhatja később helyen.

7. Az Azure-portálon nyissa meg az API alkalmazás lap a ToDoListDataAPI projekt telepített API-alkalmazásokban.

9. Kattintson a **Beállítások > alkalmazás beállításainak**.

3. Az **alkalmazás beállításai** csoportban adja meg a következő kulcsok és értékeket:

  	| **Kulcs** | ToDo:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Érték** | Szolgáltatás fő azonosítója alkalmazás hívása |
  	| **Példa** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Kulcs** | ToDo:TrustedCallerClientId |
  	|---|---|
  	| **Érték** | Az alkalmazás - másolja át az Azure Active Directory TodoListAPI alkalmazás hívása ügyfél-azonosító |
  	| **Példa** | 960adec2-b74a-484A-960adec2-b74a-484A |

6. Kattintson a **Mentés**gombra.

    ![Kattintson a Mentés gombra.](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. A böngészőben lépjen vissza a web app URL-címre, és a Kezdőlap lapon kattintson a **Teendőlista** fülre.

    Ez esetben az alkalmazás a várt módon működik, mert a megbízható hívó alkalmazást, és szolgáltatás fő azonosító is a várt értékek.

    ![A lista, oldal](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>A projektek nulláról létrehozása

A két webes API-projektet az **Azure API alkalmazás** project-sablon használatával, és az alapértelmezett értékek vezérlő cseréje egy listájába vezérlővel hozta létre. Az Azure Active Directory szolgáltatás fő tokenek a ToDoListAPI projekt beszerzésekor, az [Active Directory Authentication tár (ADAL) a .NET rendszerhez](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet csomag lett telepítve.
 
Információ arról, hogy miként egy webes API vissza vége ToDoListAngular például egy AngularJS egyoldalas-alkalmazás létrehozása [kéz a labor: az ASP.NET webes API-val és a Angular.js egyetlen lap alkalmazás (egészében) összeállítása](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Azure AD-hitelesítés kód olvashat című [biztonságossá tétele AngularJS egyetlen lap az Azure Active Directory](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Hibaelhárítás

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Győződjön meg arról, hogy ne tévessze össze ToDoListAPI (középső) és ToDoListDataAPI (adatok réteg). Például az oktatóprogram hozzáadása hitelesítési a réteg API alkalmazásba **, de az alkalmazás billentyűt az Azure Active Directory-alkalmazás, a középső API alkalmazáshoz létrehozott származhatnak**.

## <a name="next-steps"></a>Következő lépések

Az API-alkalmazások adatsor az utolsó oktatóprogram: az. 

Többet szeretne tudni az Azure Active Directory a következő forrásokban talál.

* [Azure Active Directory fejlesztői útmutatója](http://aka.ms/aaddev)
* [Azure Active Directory-esetek](http://aka.ms/aadscenarios)
* [Azure Active Directory-minta](http://aka.ms/aadsamples)

    A [Webalkalmazás-WebAPI-oauth2 hitelesítési mód-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) minta hasonlít az oktatóprogram, de az alkalmazás szolgáltatás hitelesítés nélkül látható.

Megtudhatja, [hogy miként az Azure alkalmazás szolgáltatás alkalmazások terjesztése](../app-service-web/web-sites-deploy.md)más módokon való telepítéséhez Visual Studio projektek API-alkalmazásokat, vagy az [adatforrás-vezérlő rendszer](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), a [telepítési automatizálása](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) Visual Studio használatával kapcsolatos tudnivalókat.
