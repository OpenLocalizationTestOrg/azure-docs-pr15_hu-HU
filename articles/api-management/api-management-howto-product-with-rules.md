<properties
    pageTitle="Az Azure API-ös API védelme |} Microsoft Azure"
    description="Megtudhatja, hogy miként védelme a API kvóták és szabályozásának házirendek (ráta korlátozása)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>A API védelme az Azure API-kezelés segítségével ráta korlátai

Ez az útmutató megtudhatja, hogy milyen könnyen célszerű ráta korlát és kvóta házirendek az Azure API-szolgáltatások konfigurálásával a kódmentes API-védelem.

Ebben az oktatóprogramban egy "Ingyenes próbaverziót" API-terméket, amely lehetővé teszi, hogy a fejlesztők hívásokhoz legfeljebb 10 percenkénti és legfeljebb 200 hívások heti az API-hoz a [korlát hívás ráta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és a [Set-használati kvóta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendek hoz létre. Majd az API közzéteszi és tesztelje a ráta korlát házirendet.

Speciális szabályozási esetben a [ráta-korlát – által-billentyűt](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) , és [a billentyű-kvóta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) házirendek használata [Speciális kérelem szabályozásának az Azure API kezelése](api-management-sample-flexible-throttling.md)című témakör tartalmaz.

## <a name="create-product"> </a>Termék létrehozásához

Ebben a lépésben létrehoz egy ingyenes próbaverziót termék előfizetés jóváhagyási nem igénylő.

>[AZURE.NOTE] Ha már rendelkezik egy termék beállítva, és szeretne használni, ebben az oktatóanyagban, ugorhat, [hívás ráta korlát és kvóta házirendek beállítása][] , és végezze el az oktatóprogram van az ingyenes próbaverziót termék helyett a terméket használni.

Első lépésként kattintson a **kezelés** az API-kezelési szolgáltatáshoz Azure Classic elemre. Ekkor megjelenik a API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés az első API kezelése][] oktatóprogram során.

Kattintson a **termékek** **API-kezelés** menü a bal oldali **termékek** lap megjelenítéséhez.

![Termék hozzáadása][api-management-add-product]

Kattintson a **Hozzáadás termék** az **új termék hozzáadása** párbeszédpanel megjelenítéséhez.

![Új termék hozzáadása][api-management-new-product-window]

A **cím** mezőbe írja be az **Ingyenes próbaverziót**.

A **Leírás** mezőben írja be a következő szöveggel:  **előfizetőinek tudják 200 hívások/hét utáni amelynek hozzáférés megtagadva legfeljebb 10 hívások/perc futtatásához.**

Termék API-kezelés védhetők, és nyissa meg. Védett termékek elő kell fizetnie az előtt is használhatók. Nyissa meg a termékek előfizetés nélkül is használható. Győződjön meg arról, hogy **előfizetésre van szükség** van jelölve, amely az előfizetéshez védett termék létrehozásához. Ez az alapértelmezett.

Ha azt szeretné, hogy a változtatások elfogadása vagy elvetése a termék előfizetést próbál a rendszergazda, jelölje ki az **előfizetés jóváhagyást igényel**. A jelölőnégyzet nincs bejelölve, ha előfizetés próbálkozás lesz automatikusan engedélyezett. Ebben a példában az előfizetések automatikusan engedélyezett, akkor jelölje be a mezőbe.

Ahhoz, hogy a fejlesztői fiókok többször az új termék előfizetéséhez, jelölje be a **több egyidejű előfizetés engedélyezése** jelölőnégyzetet. Ebben az oktatóanyagban nem több egyidejű előfizetés csatlakozást, így hagyja bejelölve.

Miután az összes érték kerülnek, a **mentése** a termék létrehozása gombra.

![Adott termék][api-management-product-added]

Alapértelmezés szerint új termékek jelennek meg a **rendszergazda** csoportból a felhasználók számára. Adja hozzá a **fejlesztők** csoportot fogjuk. Kattintson az **Ingyenes próbaverziót**, és kattintson a **láthatóság** fülre.

>API-kezelése a csoportok kezelése a termékekkel, és a fejlesztők olvashatóságát szolgálnak. Termékek Láthatóság megadása csoportok, és a fejlesztők megtekintheti és látható a csoportokba tartoznak, ahol a termékek előfizetés. További információért elolvashatja, [hogyan hozhat létre és használhat az Azure API felügyeleti csoportok][].

![A fejlesztők csoport hozzáadása][api-management-add-developers-group]

Jelölje be a **fejlesztők számára** jelölőnégyzetet, és kattintson a **Mentés**gombra.

## <a name="add-api"> </a>Egy API hozzáadása a termék

Ebben a lépésben az oktatóprogram azt a visszhang API hozzáadja az új ingyenes próbaverziót terméket.

>Minden egyes API Management szolgáltatás példányának megtalálható előre beállított egy használható is kipróbálhat és az API-kezelésével kapcsolatos további tudnivalók a visszhang API-val. További információ a [Azure API-kezelés az első API kezelése][]című cikkben talál.

Kattintson a **Products** elemre a bal oldali menüjében **API-kezelés** , és kattintson a **Próba-előfizetésre** konfigurálása a terméket.

![Termék konfigurálása][api-management-configure-product]

Kattintson a **Termék API hozzáadása**gombra.

![Termék API hozzáadása][api-management-add-api]

Jelölje ki a **Visszhang API**, és kattintson a **Mentés**gombra.

![Adja hozzá a visszhang API][api-management-add-echo-api]

## <a name="policies"> </a>Hívás ráta korlát és kvóta házirendek beállítása

Ráta korlátai és kvóták van konfigurálva a csoportházirend-szerkesztőben. Kattintson a **házirendek** az **API-kezelés** a bal oldali menüjében. A **termék** listában kattintson az **Ingyenes próbaverziót**.

![Termék házirend][api-management-product-policy]

Importálja a házirend-sablont, és kezdje el, az árfolyam korlát és kvóta házirendek létrehozása **Házirend hozzáadása** gombra.

![Házirend beállítása][api-management-add-policy]

Házirendek beszúrásához vigye a kurzort a vonatkozó házirendsablonok vagy a **bejövő** és **kimenő** szakaszba. Ráta korlát és kvóta házirendek a bejövő szabályok, így helyezze a kurzort a bejövő elemet.

![Csoportházirend-szerkesztőben][api-management-policy-editor-inbound]

Ebben az oktatóanyagban hozzáadni, hogy két házirend-beállítások a [korlát hívás ráta előfizetésenként](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) és [előfizetésenként használati kvóta beállítása](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) házirendeket.

![Házirend-kimutatásokban][api-management-limit-policies]

A kurzor a **bejövő** házirend-elem, miután kattintson az **előfizetés korlát hívás díjat** szeretne beszúrni a vonatkozó házirendsablonok melletti nyílra.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Korlát hívás ráta előfizetésenként** termék szintre is használható, és az API-val és az egyéni művelet nevét szinten is használható. Ebben az oktatóanyagban csak a termék szintű házirendeket használja, így **API-val** és a **művelet** elemek a **ráta-korlát** elem törlése, hogy csak a külső **ráta-korlát** elem marad, az alábbi példában látható módon.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

A próba-előfizetésre termék az maximális engedélyezett hívás ráta percenkénti 10 hívások, így a **hívások** attribútum értéke, valamint a **megújítás időszak** attribútum **60** írja be a **10** .

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Konfigurálja a **előfizetésenként használati kvóta beállítása** a házirendet, vigye a kurzort közvetlenül a **bejövő** elemben az újonnan hozzáadott **ráta-korlát** elem alatt, és kattintson a nyílra a balra lévő **előfizetésenként használati kvóta beállítása**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Ezzel a házirenddel is célja, hogy a termék szinten, mert törlése az **API-val** és a **művelet** neve elemeket az alábbi példában látható módon.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kvóták alapulhatnak időköz vagy sávszélesség eső hívások száma. Ebben az oktatóanyagban alapján sávszélesség szabályozása nem azt is, így törlése a **sávszélesség** attribútum.

    <quota calls="number" renewal-period="seconds">
    </quota>

A próba-előfizetésre termék kvóta heti 200 hívások. Adja meg, a **hívások** attribútum dátumaként **200** , és adja meg a **megújítás időszak** attribútum dátumaként **604800** .

    <quota calls="200" renewal-period="604800">
    </quota>

>Házirend intervallumok másodperc vannak megadva. Szeretné kiszámítani az intervallum egy hetet, a munkaórák száma a nap (24) szerint hány perc a egy óra (60), hogy hány másodperc egy perc (60) számú is szorzása napokat (7): 7 *24* 60 * 60 = 604800.

Ha befejezte a házirend beállítása, azt meg kell egyeznie a következő példa.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Miután a kívánt házirend konfigurálva van, kattintson a **Mentés**gombra.

![Házirend mentése][api-management-policy-save]

## <a name="publish-product"></a> a termék közzététele

Most, hogy a bekerülnek az API-hoz és a házirend konfigurálva van, a termék közzé kell tennie, hogy a fejlesztők számára használható. Kattintson a **Products** elemre a bal oldali menüjében **API-kezelés** , és kattintson a **Próba-előfizetésre** konfigurálása a terméket.

![Termék konfigurálása][api-management-configure-product]

Kattintson a **Közzététel**gombra, és kattintson az **Igen, a projekt közzététele a** megerősítéséhez.

![Termék közzététele][api-management-publish-product]

## <a name="subscribe-account"> </a>Ha fel szeretne iratkozni egy termék Fejlesztőeszközök fiókkal.

Most, hogy a termék van közzétéve, akkor érhető el fizetett elő, és a fejlesztők használják.

>A rendszergazdák az API-kezelési-példányok automatikusan iratkozott fel minden termékének a. Az oktatóprogram ezt a lépést azt fog fizessen elő a rendszergazdai jogokkal nem rendelkező Fejlesztőeszközök fiókokra az ingyenes próbaverziót termékhez. Ha a Fejlesztőeszközök fiók része a rendszergazdák szerepkört, majd, kövesse együtt ebben a lépésben akkor is, ha már előfizetett.

**Felhasználók** a **API-kezelés** menüben válassza a bal oldalon, és kattintson a Fejlesztőeszközök fiók nevére. Ebben a példában a **Clayton Gragg** Fejlesztőeszközök fiókot használ azt.

![Állítsa be a Fejlesztőeszközök][api-management-configure-developer]

Kattintson az **Előfizetés hozzáadása**gombra.

![Előfizetés hozzáadása][api-management-add-subscription-menu]

Jelölje ki az **Ingyenes próbaverziót**, és válassza az **előfizetés**.

![Előfizetés hozzáadása][api-management-add-subscription]

>[AZURE.NOTE] Ebben az oktatóanyagban több egyidejű előfizetések nem érhetők el az ingyenes próbaverziót termék. Azokat, mintha, szeretné kérni nevet az előfizetést, az alábbi példában látható módon.

![Előfizetés hozzáadása][api-management-add-subscription-multiple]

**Az előfizetés**gombra kattint, a termék jelenik meg, az **előfizetés** listájában a felhasználó számára.

![Hozzáadott előfizetések][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Művelet felhívását, és tesztelje a ráta korlát

Most, hogy az ingyenes próbaverziót termék van beállítva, és közzé, hogy hívja fel az egyes műveletek, és tesztelje a ráta korlát házirendet.
Váltson a developer Portal segítségével **Developer portal** kattintson a jobb felső sarkában menüben.

![Developer Portal segítségével][api-management-developer-portal-menu]

Kattintson a felső menü **API-hoz** , és kattintson a **Visszhang API -val**.

![Developer Portal segítségével][api-management-developer-portal-api-menu]

Kattintson az **Első erőforrás**, és kattintson a **próbálja ki**.

![Megnyitott konzol][api-management-open-console]

Hagyja meg az alapértelmezett paraméterértékeket, és válassza az Előfizetés kulcs az ingyenes próbaverziót termékhez.

![Előfizetés kulcs][api-management-select-key]

>[AZURE.NOTE] Ha több előfizetéssel rendelkezik, ne felejtse el, válassza ki a kulcsot, **Ingyenes**próbaverzióra, különben a házirendek az előző lépésekben beállított gyakorlatilag nem lesz.

Kattintson a **Küldés**gombra, és tekintse meg a választ. Megjegyzés: a **válaszállapota** **200 OK**.

![A művelet eredménye][api-management-http-get-results]

Nagyobb, mint a ráta korlát házirend percenkénti 10 hívások sebessége kattintson a **Küldés** gombra. A ráta korlát házirend túllépik, miután **Egy válasz 429-es jelű túl sok kérelmek** állapotát adja vissza.

![A művelet eredménye][api-management-http-get-429]

A **Válasz tartalom** a hátralévő időköz jelzi, mielőtt újrapróbálkozások sikeres lesz.

A ráta korlát házirend percenkénti 10 hívások gyakorlatilag esetén további hívások meghiúsul, amíg 60 másodperc telt el az első előtt a ráta korlát túllépte a termék 10 sikeres hívásainak. Ebben a példában a hátralévő intervallum: 54 másodperc.

## <a name="next-steps"> </a>Következő lépések

-   A ráta korlátai és kvóták beállítása az alábbi videóban a bemutatóból.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Az első API Azure API-kezelés kezelése]: api-management-get-started.md
[Hogyan csoportok létrehozása és használata a Azure API-kezelés]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Hívás ráta korlát és kvóta házirendek beállítása]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
