<properties 
    pageTitle="Hogyan hozhat létre, és tegye közzé a termék Azure API kezelése" 
    description="Megtudhatja, hogy miként hozhat létre, és tegye közzé a termékek Azure API-kezelés." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Hogyan hozhat létre, és tegye közzé a termék Azure API kezelése

Azure API-kezelése a termék tartalmaz egy vagy több API-hoz, valamint a használati kvótája és a használati feltételei Miután egy termék közzétette, a fejlesztők fizessen elő a termék és a termék API-khoz használatának megkezdéséhez. A témakör áttekintést nyújt a termék létrehozása, hozzáadása egy API-val és a fejlesztők közzétéve útmutató.

## <a name="create-product"> </a>Termék létrehozása

Műveletek hozzáadása, és úgy beállítva, hogy egy API-t, a publisher-portálon. A publisher portál eléréséhez kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

A **termékek** lap megjelenítéséhez a bal oldali menüben kattintson a **termékek** , és kattintson a **Termék hozzáadása**gombra.

![Termékek][api-management-products]

![Új termék][api-management-add-new-product]

Írja be a termék leíró nevét a **név** mezőben, és a **Leírás** mezőben a termék leírását.

**Nyissa meg** és a **védett**, a termékek API-kezelés is lehet. Védett termékek elő kell fizetnie az felhasználhatók, miközben Megnyitás előtt termékek előfizetés nélkül is használható. Jelölje be a **előfizetésre van szükség** , amely az előfizetéshez védett termék létrehozásához. Ez az alapértelmezett.

Ha azt szeretné, hogy a változtatások elfogadása vagy elvetése a termék előfizetést próbál a rendszergazda, ellenőrizze a **előfizetés jóváhagyást igényel** . Ha a mező nincs bejelölve, előfizetés kísérletek lesz automatikus által jóváhagyott. Előfizetések kapcsolatos további tudnivalókért olvassa el a [termék előfizetőinek megtekintése][]című témakört.

Fejlesztőeszközök fiókok lehetőséget a termék többször előfizetés engedélyezéséhez jelölje be a **több előfizetéssel engedélyezése** jelölőnégyzetet. Ez a jelölőnégyzet nincs bejelölve, ha minden Fejlesztőeszközök fiók feliratkozhat a termék csak egyetlen idő.

![Korlátlan több előfizetéssel][api-management-unlimited-multiple-subscriptions]

Több egyidejű előfizetés számának korlátozása, jelölje be a **egyidejű előfizetéseket, számának korlátozása** jelölőnégyzetet, és írja be a előfizetés korlátot. A következő példában egyidejű előfizetések korlátozódik négy Fejlesztőeszközök számla száma.

![Négy több előfizetéssel][api-management-four-multiple-subscriptions]

Miután az összes új termék beállítás be van állítva, a **Mentés** az új termék létrehozása gombra.

![Termékek][api-management-products-page]

>Alapértelmezés szerint új termékek közzé nem tett, és csak a **rendszergazdák** csoport számára láthatóak legyenek.

Adja meg a terméket, kattintson a termék neve a **termékek** lapon.

## <a name="add-apis"> </a>Termék API hozzáadása

A **termékek** lap konfiguráció négy hivatkozásokat tartalmaz: **összegzése**, **Beállítások**, **a láthatóság**és **előfizetők számára**. Az **Adatlap** fülre, ahol is hozzáadhat API-k és közzététele vagy egy termék közzétételének megszüntetése.

![Összefoglalás][api-management-new-product-summary]

A termék közzététele előtt szeretne adni egy vagy több API-khoz. Ehhez kattintson a **Termék API hozzáadása**gombra.

![Adja hozzá az API-hoz][api-management-add-apis-to-product]

Jelölje ki a kívánt API-k, és kattintson a **Mentés**gombra.

## <a name="add-description"> </a>Magyarázó információkat helyezhet termékre

A **Beállítások** lapon adhatja meg, például célját, az API-hoz való hozzáférést biztosít és egyéb hasznos információkat a termékkel kapcsolatos részletes adatokat. A tartalom a fejlesztők az API hív, és az egyszerű szöveg vagy HTML-kód írható alkalmazásindítójukban.

![Termék-beállítások][api-management-product-settings]

Ellenőrizze a **előfizetésre van szükség** , amely az előfizetéshez használandó védett termék létrehozásához, vagy törölje a jelet a jelölőnégyzetből, hozhat létre egy megnyitott terméket, amely hívható előfizetés nélkül.

Ha szeretne manuálisan az összes termék előfizetés kérelem jóváhagyása, válassza a **előfizetés jóváhagyáshoz** . Alapértelmezés szerint a minden termék előfizetés jogosultsággal automatikusan.

Fejlesztőeszközök fiókok lehetőséget a termék többször előfizetés engedélyezéséhez jelölje be a **több előfizetéssel engedélyezése** jelölőnégyzetet, és tetszés szerint adja meg a korlátozott. Ez a jelölőnégyzet nincs bejelölve, ha minden developer fiók feliratkozhat a termék csak egyetlen idő.

Tetszés szerint adja meg a **használati feltételek** mező azt mutatja be, a termék mely előfizetők el kell fogadnia a lyncet szeretné használni, termék feltételei.

## <a name="publish-product"> </a>Termék közzététele

Egy termék az API-k hívható, mielőtt a termék közzé kell tenni. A termék **összegzése** lapján kattintson a **Közzététel**gombra, és kattintson az **Igen, a projekt közzététele a** megerősítéséhez. Egy korábban közzétett termék magánjellegűvé tétele, kattintson az **e verzió közzétételének megszüntetése**.

![Termék közzététele][api-management-publish-product]

## <a name="make-visible"> </a>Termék láthatóvá szeretné tenni a fejlesztők számára

A **láthatóság** lapon válassza ki, mely szerepkörök is tudják a termék látható a developer Portal segítségével, és a termék előfizetés teszi lehetővé.

![Termék láthatósága][api-management-product-visiblity]

Engedélyezése vagy letiltása a termék láthatóság csoport a fejlesztők számára, jelölje be, vagy törölje a jelet a jelölőnégyzetből a csoport mellett, és kattintson a **Mentés**gombra.

>További információért elolvashatja, [hogyan hozhat létre és használhat a csoportok, amelyeknek Fejlesztőeszközök-fiókokat az Azure API-kezelés][].

## <a name="view-subscribers"> </a>Termék előfizetőinek megtekintése

Az **előfizetői** lapon a fejlesztőknek előfizetett a termék sorolja fel. A részletek és az egyes Fejlesztőeszközök megtekintheti a fejlesztői nevére kattintva. Ebben a példában nem fejlesztők előfizetett még a terméket.

![A fejlesztők][api-management-developer-list]

## <a name="next-steps"> </a>Következő lépések

Miután a kívánt API-k kerülnek, és a termék közzé, fejlesztők fizessen elő a terméket, és kezdje el, hívja fel az API-khoz. Oktatóanyagot, mely szemlélteti, ezek a cikkek, valamint a termék speciális konfigurációs olvassa el a [hogyan létrehozása és Azure API-kezelés termék speciális beállításainak megadása][]című témakört.

További információt a termékek használata az alábbi videóban talál.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Egy termék előfizetőinek megtekintése]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Hogyan hozhat létre és csoportok segítségével a fejlesztői-fiókokat az Azure API-kezelés]: api-management-howto-create-groups.md
[Hogyan létrehozása és Azure API kezelése lapon a speciális termék beállításainak konfigurálása]: api-management-howto-product-with-rules.md 