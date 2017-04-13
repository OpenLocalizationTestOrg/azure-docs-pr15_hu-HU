<properties 
    pageTitle="API-kezelés alapfogalmak" 
    description="Tudjon meg többet az API-hoz, termékek, szerepkörök, csoportok és más API-kezelés alapfogalmak." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Hogy miként importálhat egy API-val kapcsolatos műveletek Azure API-kezelés meghatározása

API-kezelése hozhatók létre új API-hoz, és a művelet kézzel hozzáadott és az API importálható a művelet egy lépésben együtt.

API-k és tevékenységeik importálhatók a következő formátumokban használatával.

-   WADL
-   Swagger

Ez az útmutató megtudhatja hogyan hozzon létre egy új API-t, és importálja a művelet egy lépésben. Kézzel létrehozni egy API-val és a műveletek hozzáadása a további tudnivalókért lásd [API-k létrehozása][] , és [hogy miként vehet fel egy API-nak műveletek][].

## <a name="import-api"> </a>Egy API importálása

API-k létrehozása és beállítva a publisher-portálon. A publisher portál eléréséhez kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

![Publisher-portál][api-management-management-console]

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson a **API importálása**gombra.

![Importálás API][api-management-import-apis]

Az **Importálás API** -ablak tartalmaz, amelyek megfelelnek az API-beállítások megadására háromféleképpen három fül.

-   **A vágólapról** teszi lehetővé az API-beállítások beillesztése a kijelölt szöveg mezőbe.
-   **Az importált fájl** segítségével keresse meg és jelölje ki a fájlt, amely tartalmazza az API-beállítások.
-   **URL-ből** lehetővé teszi, hogy adja meg az API előírásának URL-CÍMÉT.

![Importálás API-formátum][api-management-import-api-clipboard]

Után kezeléséről az API-beállítások, a jobb oldalon a választógombok használatával a specifikációja formátum jelzi. A következő formátumok támogatottak.

-   WADL
-   Swagger

Ezután írja be az **URL a webes API -val utótaggal**. Ez a API alkalmazáskezelési szolgáltatás alap URL-címe van ellátva. Az URL-cím alapja közös minden API-API alkalmazáskezelési szolgáltatás minden példányában is. API-kezelés az API-k által a utótag különbözteti meg, és ezért a utótag egy adott API management szolgáltatás példány minden API egyedinek kell lennie.

Miután az összes érték kerülnek, a **Mentés** az API-val és a kapcsolódó műveletek létrehozása gombra. 

>[AZURE.NOTE] Az egyszerű számológép API Swagger formátumban való importálásának oktatóanyagot a [Azure API-kezelés az első API kezelése](api-management-get-started.md)című cikkben talál.

## <a name="export-api"></a> Exportálása egy API

A publisher portálról mellett új API-khoz importálja, exportálhatja az API-khoz meghatározását. Ehhez kattintson a **API exportálása** a **API** **Adatlap fülre** .

![Exportálás API][api-management-export-api]

API-khoz WADL vagy Swagger lehet exportálni. Jelölje ki a kívánt formátumot, kattintson a **Mentés**gombra, és válassza ki a helyet, ahol a fájl mentéséhez.

![Exportálási API-formátum][api-management-export-api-format]

## <a name="next-steps"> </a>Következő lépések

Miután létrehozott egy API-t, és a tevékenységek importált, megtekintheti és konfigurálása az esetleges további beállításokat, az API hozzáadása egy termék, és közzéteheti, hogy a fejlesztők számára érhető el. További tudnivalókért lásd: az alábbi útmutatók.

-   [Hogyan API-beállításainak konfigurálása][]
-   [Hogyan hozhat létre, és egy termék közzététele][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance

[Műveletek hozzáadása egy API]: api-management-howto-add-operations.md
[Hogyan hozhat létre, és egy termék közzététele]: api-management-howto-add-products.md
[API-k létrehozása]: api-management-howto-create-apis.md
[Hogyan API-beállításainak konfigurálása]: api-management-howto-create-apis.md#configure-api-settings
