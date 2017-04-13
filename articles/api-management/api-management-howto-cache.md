<properties
    pageTitle="Azure API kezelése lapon a teljesítmény javítása érdekében gyorsítótárazás hozzáadása |} Microsoft Azure"
    description="További információ az időtartam, a sávszélesség és a webes szolgáltatás betöltés API Management szolgáltatás hívásokhoz."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Azure API kezelése lapon a teljesítmény javítása érdekében gyorsítótárazás hozzáadása

Műveletek API-kezelés válasz gyorsítótárazás beállíthatók. Válasz gyorsítótárazás jelentősen csökkentheti a API késés, sávszélességet, és a webes szolgáltatás betöltése nem gyakran változó adatok számára.

Ez az útmutató bemutatja, hogyan vehet fel, az API-gyorsítótárazás választ, és a minta halvány API műveletek házirendek beállítása. A művelet majd felhívhatja, ha a Fejlesztőeszközök portálról gyorsítótárba helyezése a művelet megerősítéséhez.

>[AZURE.NOTE] Információt gyorsítótárazási elemeknél billentyű házirend kifejezések használatával című témakörben talál [egyéni gyorsítótárazás Azure API-kezelés](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Előfeltételek

Ebből az útmutatóból lépések végrehajtása előtt kell lennie az API-val API Management szolgáltatás példány és egy termék beállítva. Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

## <a name="configure-caching"> </a>Egy művelet gyorsítótárazás beállításával

Ebben a lépésben be fogja megvizsgálni az **Első erőforrás (gyorsítótárazott)** művelet a minta halvány API gyorsítótárazási beállításaitól.

>[AZURE.NOTE] Minden egyes API Management szolgáltatás példányának megtalálható előre beállított egy API-kezelésről kísérletezhet és használható a visszhang API-val. További tudnivalókért olvassa el a [Azure API-kezelés – első lépések][]című témakört.

Első lépésként kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson a **Visszhang API -val**.

![A visszhang API][api-management-echo-api]

Kattintson a **Műveletek** fülre, és kattintson az **Első erőforrás (gyorsítótárazott)** művelet **Műveletek** listájából.

![A visszhang API-műveletek][api-management-echo-api-operations]

Kattintson a **gyorsítótár** fülre kattintva megjeleníthetők a gyorsítótár beállításai ehhez a művelethez.

![Gyorsítótárazási lap][api-management-caching-tab]

Művelet gyorsítótárazás engedélyezéséhez jelölje be a **engedélyezése** jelölőnégyzetet. Ebben a példában gyorsítótárazás engedélyezve van.

Minden művelethez válaszra beállítható a **lekérdezési karakterlánc paraméterei változó** és a **fejlécek által változó** mezőket az értékek alapján. A lekérdezési karakterlánc paraméterei vagy a fejlécek alapján több válaszokkal gyorsítótár szeretne, is konfigurálása a következő két mezőt.

**Időtartam** lejárati időköze gyorsítótárazott adott válaszok. Ebben a példában az intervallum érték **3600** másodpercre, amely egyenértékű egy órával a.

Ebben a példában a gyorsítótár konfiguráció használ, az **Első erőforrás (gyorsítótárazott)** művelet első kérésére választ ad a kódmentes szolgáltatásból. A válasz fog gyorsítótárba helyezhető, a megadott fejlécek és a lekérdezési karakterlánc paramétereinek által beállítható. A művelet megfelelő paraméterekkel történő további hívások lesz a gyorsítótárban tárolt választ ad vissza, amíg az időtartam időköz lejárt.

## <a name="caching-policies"> </a>Tekintse át a gyorsítótár házirendek

Ebben a lépésben az **Első erőforrás (gyorsítótárazott)** művelet a minta halvány API-gyorsítótár beállításainak áttekintése.

Ha a **gyorsítótár** lapon művelet gyorsítótár beállításai vannak, gyorsítótárazási házirendek kerülnek, a művelet. Ezek a házirendek tekinthetők meg és szerkeszteni a csoportházirend-szerkesztőben.

Kattintson az a bal oldali menüjében **API-kezelési** **házirendek** , és válassza a **halvány API / erőforrás (gyorsítótárazott) beszerzése** a **művelet** legördülő listából.

![Házirend hatálya művelet][api-management-operation-dropdown]

Ez a csoportházirend-szerkesztőben ehhez a művelethez a házirendek jeleníti meg.

![API-kezelés csoportházirend-szerkesztőben][api-management-policy-editor]

Ehhez a művelethez a házirend-definícióhoz házirendek a gyorsítótár konfiguráció meghatározó voltak elemen **gyorsítótárazás** lapján az előző lépésben tartalmazza.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] A csoportházirend-szerkesztőben gyorsítótárazási házirendjét végzett módosítások egy műveletet, és fordítva **gyorsítótárazás** lapján jelennek meg.

## <a name="test-operation"> </a>Hívja fel a műveletet, és a gyorsítótár tesztelése

Szeretné látni, a művelet gyorsítótárazás, a művelet a fejlesztői portálról felhívhatja azt. Kattintson a jobb felső menü **Developer Portal segítségével** .

![Developer Portal segítségével][api-management-developer-portal-menu]

Kattintson a felső menü **API-hoz** , és válassza a **Visszhang API -val**.

![A visszhang API][api-management-apis-echo-api]

>Ha csak egy API konfigurált vagy látható a fiókjához, válassza az API-khoz megnyitja közvetlenül a műveleteket, hogy API-val.

Válassza ki az **Első erőforrás (gyorsítótárazott)** műveletet, és kattintson a **Megnyitás konzolon**.

![Nyissa meg a konzolon][api-management-open-console]

A konzol hívás műveletek közvetlenül a developer Portal segítségével teszi lehetővé.

![Konzol][api-management-console]

Az alapértelmezett értékeket **paraméter1** és **paraméter2**megtartani.

Az **Előfizetés-kulcs** legördülő listából, jelölje ki a kívánt billentyűt. A fiók csak egy előfizetéssel rendelkezik, már kijelölt fel.

A **fejlécek** mezőbe írja be a **sampleheader:value1** .

Kattintson a **HTTP Get** , és jegyezze fel a válasz fejlécek.

A **fejlécek** mezőbe írja be a **sampleheader:value2** , és kattintson a **HTTP Get**.

Ne feledje, hogy **sampleheader** értékének továbbra is **érték1** a válaszban. Próbálja ki az egyes különböző értékeket, és figyelje meg, hogy az első hívás gyorsítótárazott választ ad vissza.

Írja be a **25** a **paraméter2** mezőben, és kattintson a **HTTP Get**.

Ne feledje, hogy **sampleheader** a válaszban értékének most **érték2**. A művelet eredménye a lekérdezési karakterlánc is beállítható, mert nem adta vissza az előző gyorsítótárazott választ.

## <a name="next-steps"> </a>Következő lépések

-   Gyorsítótárazási házirendek kapcsolatos további tudnivalókért olvassa el a [gyorsítótár-házirendek][] az [API adatkezelési házirend hivatkozás][]című témakört.
-   Információt gyorsítótárazási elemen billentyű házirend kifejezések használatával című témakörben talál [egyéni gyorsítótárazás Azure API-kezelés](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API-kezelés – első lépések]: api-management-get-started.md

[API adatkezelési házirend-hivatkozás]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Gyorsítótár-házirendek]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
