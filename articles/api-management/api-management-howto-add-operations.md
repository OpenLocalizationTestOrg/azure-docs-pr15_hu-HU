<properties 
    pageTitle="Műveletek hozzáadása egy API Azure API-kezelés |} Microsoft Azure" 
    description="Megtudhatja, hogy miként műveletek hozzáadása egy API Azure API-kezelés." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Műveletek hozzáadása egy API Azure API kezelése

Mielőtt egy API API-kezelés is használható, műveletek hozzá kell adnia. Ez az útmutató hozzáadása és konfigurálása a különböző típusú műveletek az API-nak API-kezelés mutatja.

## <a name="add-operation"> </a>Vegyen fel egy operátort

Műveletek hozzáadása, és úgy beállítva, hogy egy API-t, a publisher-portálon. A publisher portál eléréséhez kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

Jelölje ki a kívánt API a publisher-portálon, és válassza a **Műveletek** fülre. 

![Műveletek][api-management-operations]

Kattintson a **Művelet hozzáadása** egy új művelet hozzáadása gombra. Jelenik meg az **új műveletet** , és az **aláírása** lap alapértelmezés szerint lesz kijelölve.

![Művelet hozzáadása][api-management-add-operation]

Adja meg a **HTTP-művelet** kiválasztása a legördülő listából.

![HTTP-metódus][api-management-http-method]

<a name="url-template"></a>

Az URL-cím sablon írjon be egy egy vagy több URL-cím elérési út szegmensek és nulla vagy több lekérdezési karakterlánc paraméterei álló URL-cím kódrészletet. Az URL-cím sablont, az alap URL-címet az API fűzött egyetlen HTTP-művelet azonosítja. Tartalmazhat egy vagy több nevű kapcsos zárójelek azonosítjuk változó részből. Változó részei sablon paraméterek neve és dinamikusan oszthatók ki az értékeket a kérelem URL-címe kiolvasott, amikor a kérelmet a API-kezelés platform feldolgozása.

![A sablon URL-címe][api-management-url-template]

<a name="rewrite-url-template"></a>

Tetszés szerint adja meg a **sablon Átírásának URL-CÍMÉT**. Ez lehetővé teszi, hogy az előtér-a bejövő felkérést feldolgozása közben hívja fel a háttéradatbázist keresztül konvertált az URL-CÍMEK átírása sablon szerint a szabványos URL-cím sablon használata. Sablon paramétereket sablon URL-CÍMÉT a átírása sablonban kell használni. A következő példa bemutatja, hogyan tartalomtípust a webszolgáltatás az előző példában az elérési út szakaszában az API közzétett keresztül az API-kezelés platform az URL-cím sablonok használata a lekérdezési paraméterként meghatározott kódolt.

![URL-cím sablon átírása][api-management-url-template-rewrite]

A művelet hívók fogja használni a formátum `/customers?customerid=ALFKI` , és ez lesz rendelve `/Customers('ALFKI')` a háttéradatbázist szolgáltatás meghívott mikor.


**Megjelenítendő** név és **Leírás** : a művelet leírását, és ahhoz, hogy dokumentáció Ez az API segítségével a developer Portal segítségével a fejlesztői szolgálnak.

![Leírás][api-management-description]

A művelet leírását, egyszerű szöveg vagy HTML, a **Leírás** mezőben adható meg.

## <a name="operation-caching"> </a>Művelet gyorsítótárazás

Válasz gyorsítótárazás csökkenti a késés API vonzóbbak lehetnek, sávszélességet csökkenti és a terhelést a HTTP webes szolgáltatás végrehajtása az API csökkenések észlelt. 

Könnyen és gyorsan gyorsítótárazása a művelet, jelölje be a **gyorsítótár** fülre, és a **engedélyezése** jelölőnégyzetet.

![Gyorsítótár][api-management-caching-tab]

**Időtartam** Megadja, hogy az időtartamot, amely alatt a művelet válasz a gyorsítótárban marad. Az alapértelmezett érték 3600 másodpercet vagy 1 órát.

Gyorsítótár billentyűk segítségével válaszok különbözteti meg, hogy a választ, minden más gyorsítótár kulcsa megfelelő saját külön gyorsítótárazott értéket kapja. Tetszés szerint adja meg az adott lekérdezési karakterlánc paraméterei és/vagy a HTTP-fejlécek használható számítások rendre a gyorsítótár kulcs értékeit a **lekérdezési karakterlánc paraméterei változó** és a **fejlécek által változó** szöveg mezőbe. Ha egyik sem a megadott, teljes kérelem URL-címe és a következő HTTP élőfej értékek használt gyorsítótár kulcs generálása: **Elfogadás** és az **Elfogadás-karakterkészlet**.

>További információt a gyorsítótár és a gyorsítótár-házirendek megtudhatja, [hogy miként gyorsítótár művelet eredménye Azure API-kezelés][].


## <a name="request-parameters"> </a>Kérelem paraméterei

A művelet paraméterek kezelhetők a Paraméterek lapon. A **Sablon URL-CÍMÉT** az **aláírása** lap megadott paramétereket a rendszer automatikusan hozzáadja, és csak az URL-cím sablon szerkesztésével módosíthatja. További paramétereket manuálisan kell megadni.

Új lekérdezési paraméter hozzáadásának, kattintson a **Lekérdezési paraméter felvétele** , és írja be az alábbi adatokat:

-   **Név** - paraméter neve.
-   **Leírás** – rövid leírását (nem kötelező) a paramétert.
-   **Típus** - paraméter típusa, lefelé a legördülő kijelölt.
-   **Értékek** - értékeket Ez a paraméter rendelt. Az értékek közül jelölhető alapértelmezett (nem kötelező).
-   **Szükséges** - a paraméter kötelezővé tétele a jelölőnégyzet bejelölésével. 

![Paraméter kérése][api-management-request-parameters]

## <a name="request-body"> </a>Szervezet kérése

Ha lehetővé teszi, hogy a művelet (például helyezése, POST), és a szervezet lehetővé teheti, hogy az összes támogatott ábrázolása formátumban (például json, XML) Példa igényel. 

>A kérelem törzsén csak a dokumentáció célokra szolgál, és a érvényesítése nem volt.

A kérelem szervezet megadásához kattintson a **szervezet** fülre.

**Ábrázolása hozzáadása**gombra, kezdje el beírni a kívánt tartalomtípus nevét (például alkalmazás/json), a legördülő listában jelölje ki és illessze be a kívánt összehívás törzsébe példa a megadott formátumban a szövegmezőbe. 

![Szervezet kérése][api-management-request-body]

A megadott, további is megadhatja egy választható leírását a **Leírás** mezőben.

## <a name="responses"> </a>Válaszok

Tanácsos alakulhat ki a műveletet az összes állapot kódokat a válaszokat példákon mutatják. Előfordulhat, hogy az egyes állapotkód egynél több válasz törzsén példában egy, a támogatott tartalomtípusok mindegyikére. 

Visszajelzés hozzáadásához kattintson a **Hozzáadás** gombra, és kezdje el beírni a kívánt állapotkód. Ebben a példában a állapotkód van **200 OK**. A kódot a legördülő lista jelenik meg, miután jelölje ki azt, és a válasz kód létrejön, és a művelet adott hozzá.

![Válasz kódot.][api-management-response-code]

Kattintson a **Ábrázolása hozzáadása**, kezdje el beírni a kívánt tartalomtípus nevére (például alkalmazás/json), és majd jelölje ki a legördülő lefelé.

![Szervezet webhelytartalom-típus][api-management-response-body-content-type]

Illessze be a választ szervezet példa kiválasztott formátumban a szövegmezőbe. 

![Válasz törzsében][api-management-response-body]

Ha szükséges, adja hozzá a szerint egy leírást a **Leírás** mezőbe.

Miután a művelet van beállítva, kattintson a **Mentés**gombra.


## <a name="next-steps"> </a>Következő lépések

Miután a műveletek bekerülnek az API-hoz, következő lépésként az API társítása egy termék és a közzétételre, hogy a fejlesztők felhívhatja a műveletek.

-   [Hogyan hozhat létre, és egy termék közzététele][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Hogyan hozhat létre, és egy termék közzététele]: api-management-howto-add-products.md
[Hogyan gyorsítótár művelet eredménye Azure API kezelése]: api-management-howto-cache.md