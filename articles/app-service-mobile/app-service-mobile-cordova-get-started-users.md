<properties
    pageTitle="Hitelesítés hozzáadása Apache Cordova a Mobile-alkalmazások |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként Azure alkalmazás szolgáltatás Mobile-alkalmazások használatával hitelesíti a felhasználókat a Apache Cordova alkalmazás Identitásszolgáltatók, beleértve a Google, a Facebook, a Twitteren és a Microsoft számos keresztül."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Hitelesítés felvétele a Apache Cordova alkalmazásba

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Összefoglalás

Ebben az oktatóanyagban vesz fel hitelesítési egy támogatott identitásszolgáltató használata Apache Cordova quickstart útmutató projekt listájába. Ebben az oktatóprogramban az [Mobile-alkalmazások – első lépések] oktatóprogram, amely el kell végeznie alapul.

##<a name="register"></a>Regisztrálni a hitelesítést az alkalmazást, és az alkalmazás szolgáltatás konfigurálása

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Hasonló lépésekkel megjelenítő videó megtekintése](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Most ellenőrizheti, hogy a kódmentes névtelen hozzáférés le van tiltva. A Visual Studióban nyissa meg a projektet, Ön által létrehozott [Mobile-alkalmazások – első lépések]az oktatóprogram befejeződésekor, ezután az alkalmazást a **Google Android irányító** futtatni, és ellenőrizze, hogy az alkalmazás indítása után jelenik-e a kapcsolat váratlan hiba történt.

Ezután a felhasználók hitelesítését előtt erőforrások kér a mobilalkalmazás kódmentes alkalmazás frissíti.

##<a name="add-authentication"></a>Hitelesítés felvétele az alkalmazásba

1. Nyissa meg a projekt a **Visual Studióban**, majd nyissa meg a `www/index.html` fájlt szerkesztésre.

2. Keresse meg a `Content-Security-Policy` metacímke központi szakaszában.  Szüksége lesz az engedélyezett adatforrások listáját OAuth állomás hozzáadása.

  	| Szolgáltató               | SDK szolgáltató neve | A Host OAuth                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | aad               | https://Login.Windows.NET   |
  	| Facebook               | Facebook          | https://www.Facebook.com    |
  	| A Google                 | a Google            | https://Accounts.google.com |
  	| A Microsoft              | microsoftaccountja  | https://Login.live.com      |
  	| Twitter                | Twitter           | https://API.Twitter.com     |

    Példa (az Azure Active Directory végrehajtott) tartalma-biztonsági-házirend értéke az alábbi képlettel történik:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Ezt kell lecserélnie `https://login.windows.net` a OAuth állomáswebhelyén a táblából.  További információt a metacímke, olvassa el a [tartalom-biztonsági-házirend dokumentációt] .

    Megjegyzés: egyes hitelesítésszolgáltatók nincs szükség tartalom-biztonsági-házirend megváltozik, amikor a megfelelő mobileszközön használja.  Ha például tartalom-biztonsági-házirend-módosítások szükségesek, ha a Google-hitelesítés használatával Android-eszközön.

3. Nyissa meg a `www/js/index.js` szerkesztésre a fájl, keresse meg a `onDeviceReady()` módszer, és az ügyfél létrehozása a kód vegye fel a következő:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Megjegyzés: Ez a meglévő kód csere kód, amely a táblázat hivatkozást hoz létre, és a felhasználói felület frissítése.

    A login() módszer a szolgáltató hitelesítési kezdődik. A login() egy JavaScript cikk megvásárlását fel egy aszinkron függvény található.  A többi a inicializálni belül a cikk megvásárlását fel válasz kerül, hogy nem végre, amíg befejeződik a login() módszert.

4. Cserélje le a kódot közvetlenül a felvétele után, `SDK_Provider_Name` szolgáltatójától jelentkezzen be a nevet. Ha például az Azure Active Directory, az `client.login('aad')`.

4. Futtassa a projektben.  Befejeződése a projekt inicializálása, akkor az alkalmazás a választott hitelesítési szolgáltató OAuth bejelentkezési lapját jeleníti meg.

##<a name="next-steps"></a>Következő lépések

* Ismerje meg a további [Tudnivalók a hitelesítési] Azure alkalmazás szolgáltatással.
* Az oktatóprogram folytassa a Apache Cordova alkalmazásba a [Leküldéses értesítések] hozzáadásával.

Megtudhatja, hogy miként használhatja a SDK.

* [Apache Cordova SDK]
* [ASP.NET-kiszolgáló SDK]
* [NODE.js Server SDK]

<!-- URLs. -->
[Mobile-alkalmazások – első lépések]: app-service-mobile-cordova-get-started.md
[Tartalom-biztonsági-házirend dokumentáció]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Leküldéses értesítések]: app-service-mobile-cordova-get-started-push.md
[Tudnivalók a hitelesítés]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[ASP.NET-kiszolgáló SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[NODE.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
