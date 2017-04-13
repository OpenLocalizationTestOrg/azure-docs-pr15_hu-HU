<properties 
    pageTitle="API-k létrehozása az Azure API kezelése" 
    description="Megtudhatja, hogy miként hozhat létre és állíthat API-khoz Azure API-kezelés." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-apis-in-azure-api-management"></a>API-k létrehozása az Azure API kezelése

Egy API API-kezelés ügyfélalkalmazások hívható műveletek csoportját képviseli. A publisher-portálon új API-hoz készült, és kattintson a kívánt műveleteket hozzáadódnak. A tevékenységek felvétele után az API bekerül a terméket, és a tehetők közzé. Miután egy API közzétette, fizetett elő, és a fejlesztők használja.

Ez az útmutató a folyamat jeleníti meg az első lépés: hogyan hozhat létre, és állítsa be egy új API API-kezelés. Műveletek hozzáadása, és egy termék közzétételéről olvashat, [hogy miként vehet fel egy API-nak műveletek][] és megtekintése [létrehozása és közzététele a termék][]

## <a name="create-new-api"> </a>Hozzon létre egy új API

API-k létrehozása és beállítva a publisher-portálon. A publisher portál eléréséhez kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson a **API hozzáadása**gombra.

![Hozzon létre API][api-management-create-api]

A **Hozzáadás új API** -ablak használatával állítsa be az új API-t.

![Adja hozzá az új API][api-management-add-new-api]

Az alábbi mezők használatával állítsa be az új API-t.

-   **Webes API nevét** az API egyedi, leíró nevet tartalmazza. A Fejlesztőeszközök és a publisher portálokról megjelennek.
-   **Webes szolgáltatás URL-CÍMÉT** a HTTP-szolgáltatás az API végrehajtási hivatkozik. API-kezelés továbbítja kérések erre a címre.
-   Az alap URL-címet az API alkalmazáskezelési szolgáltatás **URL a webes API -val utótaggal** van ellátva. Az URL-cím alapja közös minden API-példány API Management szolgáltatás által üzemeltetett. API-kezelés az API-k által a utótag különbözteti meg, és ezért a utótag minden API-közzétevő megadott egyedinek kell lennie.
-   **Az Office-téma webes API URL-címe** határozza meg, hogy a protokollok az API eléréséhez használható. HTTPs alapértelmezés szerint van megadva.
-   Írhat a új API termékre, kattintson a **termékek (nem kötelező)** legördülő listában, és válassza a terméket. Ebben a lépésben többször a API hozzáadása több termék is kell ismételni.

Miután a kívánt értékeket van beállítva, kattintson a **Mentés**gombra. Amikor az új API létrejött, a összesítő lap az API a publisher portál jelenik meg.

![API összegzése][api-management-api-summary]

## <a name="configure-api-settings"> </a>Konfigurálása API-beállítások

A **Beállítások** lap segítségével ellenőrzése és szerkesztése az adatokat egy API-t. **Webes API nevét**, **webes szolgáltatás URL-címe**és **URL a webes API -val utótaggal** vannak kezdetben a API hoz létre, és módosíthatók Itt adhatja meg. **Leírás** nyújt szerint egy leírást, és a **webes API URL-séma** határozza meg, hogy mely protokollok az API eléréséhez használható.

![API-beállítások][api-management-api-settings]

Konfigurálása az átjáró a végrehajtási az API kódmentes szolgáltatás, jelölje be a **Biztonság** fülre. A **hitelesítő adatokat tartalmazó** legördülő konfigurálása az **egyszerű HTTP** vagy **ügyfél tanúsítványok** használható. Használja a HTTP alapszintű hitelesítés, egyszerűen írja be a kívánt hitelesítő adatokat. Ügyfél tanúsítvány-hitelesítés használatával kapcsolatos további tudnivalókért tájékozódhat [secure háttéradatbázist szolgáltatások ügyfél tanúsítvány hitelesítéssel Azure API-kezelés][].

A **Biztonság** lapon is használható konfigurálása a **felhasználói hitelesítés** OAuth 2.0-s verzióját használja. További információért megtudhatja, [hogy miként engedélyezheti a fejlesztői fiókot OAuth 2.0-s Azure API-kezelés][].

![Alapszintű hitelesítés beállításai][api-management-api-settings-credentials]

Kattintson a **Mentés** az API-beállítások végzett módosítások mentéséhez.

## <a name="next-steps"> </a>Következő lépések

Miután létrehozott egy API-t, és a beállítások, a következő lépésekkel a tevékenységek hozzáadása az API-t, az API hozzáadása egy termék, és a közzétételre, hogy a fejlesztők számára érhető el. További tudnivalókért lásd: az alábbi cikkekben.

-   [Műveletek hozzáadása egy API][]
-   [Hogyan hozhat létre, és egy termék közzététele][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Műveletek hozzáadása egy API]: api-management-howto-add-operations.md
[Hogyan hozhat létre, és egy termék közzététele]: api-management-howto-add-products.md

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Hogyan biztonságos háttéradatbázist szolgáltatások ügyfélprogram API-kezelés Azure hitelesítés]: api-management-howto-mutual-certificates.md
[Hogyan engedélyezheti a fejlesztői fiókot OAuth 2.0-s Azure API kezelése]: api-management-howto-oauth2.md