<properties
    pageTitle="Az első API Azure API-kezelés kezelése |} Microsoft Azure"
    description="Megtudhatja, hogy miként API-k létrehozása, műveletek hozzáadása és API-kezelés – első lépések."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Az első API Azure API-kezelés kezelése

## <a name="overview"> </a>– Áttekintés

Ez az útmutató bemutatja, hogyan gyorsan Azure API-kezelés használatának első lépései és az első API-hívás.

## <a name="concepts"> </a>Mi az Azure API szolgáltatás?

Azure API-kezelés segítségével bármely kódmentes készíthet, és indítsa el a teljes értékű API-program alapuló.

Gyakori alkalmazási területek a következők:

* **Mobil infrastruktúra biztonságossá tétele** átjáró access API-kulcsokkal DOS megakadályozása támadásokat szabályozásának vagy speciális biztonsági házirendek például JWT jogkivonat érvényességi segítségével használatával.
* **Engedélyezése a külső partner ökológiai** kínáló gyors partner bevezetési az developer portal segítségével és az API homlokzati épület szeretne a belső megvalósítása, amelyek nem érett partner fogyasztási decouple.
* **Belső API programot** a szervezet számára egy központi helyet biztosításával kommunikáció az elérhetőség és a legújabb változásairól API-khoz, átjáró alapuló szervezeti fiókok, az access összes alapján az API átjáró és a kódmentes közötti biztonságos csatornát.


A rendszer az alábbi összetevőket tevődik össze:

* Az **átjáró API-t** , a végpontot, amely:
  * Fogadja el a hívások API-val, és továbbítja azokat a háttérkiszolgálókon.
  * Ellenőrzi, API kulcsok, JWT tokenek, tanúsítványok és más hitelesítő adatokat.
  * Használati kvótájának kényszeríti és korlátok ráta.
  * A kód módosítások nélkül menet API alakítja.
  * A gyorsítótárban kódmentes válaszokat, ahol beállítása.
  * Naplók hívja fel a metaadat-alapú analytics céljából.

* A **publisher-portált** a hol beállíthatja az API-alkalmazás felügyeleti felületén. Használja azt:
    * Határozza meg, vagy API-séma importálása.
    * Csomag API-khoz termékek.
    * Kvóták és az API-hoz a átalakítások például házirendeket beállítani.
    * Megnyithatja az összefüggéseket analytics.
    * Felhasználók kezelése.

* A **developer portal** szolgál a fő webes jelenlét a fejlesztők számára hol lehet:
    * Olvasás API dokumentációt.
    * Próbálja ki egy API-t az interaktív console keresztül.
    * Hozzon létre egy fiókot, és az API-kulcsok első előfizetés.
    * Az Access analytics a saját használatát.


## <a name="create-service-instance"> </a>Hozza létre az API-kezelés

>[AZURE.NOTE] Ebben az oktatóanyagban befejezéséhez Azure-fiók szükséges. Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes fiók. A részletekért lásd: [Azure ingyenes próbaverziót][].

Az első API-kezelés használata lépésként hozza létre a szolgáltatás. Jelentkezzen be az [Azure klasszikus portál][] , és kattintson a **Új**, az **Alkalmazás szolgáltatásokat**, a **API-kezelés**, a **Létrehozás**.

![Új példányát API-kezelés][api-management-create-instance-menu]

**URL-CÍMÉT**adjon meg egy egyedi alszint tartománynevet használni a szolgáltatás URL.

Válassza ki a kívánt **előfizetést** , és **régió** a szolgáltatás példány. A beállítások elvégzése után kattintson a **Tovább** gombra.

![Új API alkalmazáskezelési szolgáltatás][api-management-create-instance-step1]

Írja be a **Contoso Ltd.** a **Szervezet neve**és a **Rendszergazda e-mail címe** mezőben adja meg az e-mail címét.

>[AZURE.NOTE] Ezt az e-mail címét az API a Projektirányítási rendszerek az értesítések szolgál. További információért megtudhatja, [hogy miként értesítések és Azure API-kezelés e-mail sablonok konfigurálása][].

![Új API alkalmazáskezelési szolgáltatás][api-management-create-instance-step2]

API Management szolgáltatás példányainak érhetők el a három réteg: fejlesztőeszközök, normál és Premium. A Fejlesztőeszközök réteg alapértelmezés szerint új API Management szolgáltatás példányok jönnek létre. Jelölje ki a normál vagy prémium réteg, jelölje be a **Speciális beállítások** jelölőnégyzetet, és válassza ki a kívánt réteg a következő képernyőn.

>[AZURE.NOTE] A Fejlesztőeszközök réteg fejlesztése, tesztelése és kísérleti API-programokat, ha magas elérhetősége nem egy problémát szolgál. A szokásos és prémium rétegek úgy méretezheti a fenntartott egység száma lehetőséget választva további forgalom kezelésére. A szokásos és prémium rétegek adja meg a API alkalmazáskezelési szolgáltatás együtt a legtöbbet feldolgozás és teljesítmény. Ebben az oktatóanyagban bármelyik rétegben segítségével hajthatja végre. API-kezelés rétegek kapcsolatos további tudnivalókért olvassa el az [API-kezelés árak][]című témakört.

Kattintson a jelölőnégyzet bejelölésével hozza létre a szolgáltatás.

![Új API alkalmazáskezelési szolgáltatás][api-management-instance-created]

Amikor létrejött a szolgáltatás példánya, következő lépésként létrehozására vagy importálására egy API-t.

## <a name="create-api"> </a>Egy API importálása

Egy API-t, ahol a műveleteket, amelyek az ügyfélalkalmazás hívható csoportja. API-műveletek proxy meglévő webes szolgáltatásokhoz.

API-khoz hozhatók létre (és a műveletek manuálisan is hozzáadhatók), manuálisan vagy lehet importálni. Ebben az oktatóprogramban az API-t egy minta Számológép webes szolgáltatás, a Microsoft által nyújtott és Azure is fogjuk importálni.

>[AZURE.NOTE] Egy API létrehozásáról és a manuális felvétele a műveletek, olvassa el az [API-k létrehozása](api-management-howto-create-apis.md) , és [hogy miként vehet fel egy API-nak műveletek](api-management-howto-add-operations.md).

API-khoz úgy van beállítva, a publisher portálon az Azure klasszikus portálon keresztül érhető el. A publisher portál elérjen, kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre.

![Publisher-portál][api-management-management-console]

A Számológép API importálásához **API-khoz** kattintson a bal oldali menüjében **API-kezelés** , és kattintson az **Importálás API -val**.

![Importálás API gomb][api-management-import-api]

Végezze el a Számológép API konfigurálásához az alábbi lépéseket:

1. Kattintson az **URL-ből**, a **specifikációja dokumentum URL-címe** mezőbe írja be a **http://calcapi.cloudapp.net/calcapi.json** , és kattintson a **Swagger** választógombra.
2. **Calc** mezőbe írja be a **webes API URL-címe utótag** szöveget.
3. Kattintson a **termékek (nem kötelező)** mezőbe, és válassza az **alapszintű**.
4. Az API importálása a **Mentés** gombra.

![Adja hozzá az új API][api-management-import-new-api]

>[AZURE.NOTE] **API-kezelés** jelenleg támogatott Swagger dokumentum 1.2-es és a 2.0-s verzióját az importáláshoz. Győződjön meg arról, hogy annak ellenére, hogy [Swagger 2.0-s specifikációja](http://swagger.io/specification) , amelyek deklarálása `host`, `basePath`, és `schemes` tulajdonságok nem kötelező, **kell** Swagger 2.0-s dokumentum tartalmaz-e tulajdonságok; egyéb, nem importálva. 

Az API importálása után az API Összegzés lapon megjelenik a publisher-portálon.

![API összegzése][api-management-imported-api-summary]

A API című szakaszt tartalmaz, több lapot. Az **Összegzés** lapon az egyszerű mértékek és az API-val kapcsolatos információk jeleníti meg. A [Beállítások](api-management-howto-create-apis.md#configure-api-settings) lap megtekintése és szerkesztése az adatokat egy API-t használják. A [Műveletek](api-management-howto-add-operations.md) fülre a API-műveletek kezelésére szolgál. A **Biztonság** lapon is használható, konfigurálása az átjáró a kódmentes kiszolgáló alapszintű hitelesítés vagy [kölcsönös hitelesítés](api-management-howto-mutual-certificates.md)használatával, és állítsa be a [felhasználói hitelesítés OAuth 2.0 használatával](api-management-howto-oauth2.md).  A **problémák** fülre kattintva tekintheti meg a fejlesztők számára, akik az API-hoz a által jelzett problémákat használják. A **termékek** lapon állítsa be az Ez az API tartalmazó termékek szolgál.

Alapértelmezés szerint minden API-kezelés példány két minta termékek nyújtja:

-   **Alapszintű**
-   **Korlátlan**

Ebben az oktatóprogramban az egyszerű számológép API hozzá lett adva a Starter termékhez az API importálásakor.

Egy API-hoz a hívások kezdeményezéséhez fejlesztők kell először elő kell fizetnie egy olyan terméket, elérheti azokat meg. A fejlesztők feliratkozhat a developer Portal segítségével-terméket, vagy rendszergazdák feliratkozhat a fejlesztők számára a termékek a publisher-portálon. Ön rendszergazda, mivel az API-kezelés példányt az előző lépések során a oktatóanyagban hoztunk, így a már előfizetett minden termékének alapértelmezés szerint.

## <a name="call-operation"> </a>a developer Portal segítségével hívhat fel egy művelet

Műveletek közvetlenül a developer Portal segítségével, amely magában foglalja a megtekintése, és a műveleteket egy API teszteléséhez kényelmesen hívható. Az oktatóprogram ezt a lépést hívja az egyszerű számológép API **hozzáadása két egész számok** művelet. **Developer Portal segítségével** kattintson a menü felső sarkában látható a publisher-portálon.

![Developer Portal segítségével][api-management-developer-portal-menu]

**API** a felső parancsot, és kattintson a **Egyszerű számológép** az elérhető műveletek megtekintéséhez.

![Developer Portal segítségével][api-management-developer-portal-calc-api]

Megjegyzés: a minta leírásokat és a paraméterek együtt az API-val és a műveleteket, dokumentumokat eljuttatja a fejlesztők, amely használja ezt a műveletet az importált. Leírások tevékenységek felvétele után manuálisan is hozzáadhatók.

Ha fel szeretne hívni a **Hozzáadás két egész számok** műveletet, kattintson a **próbálja ki**.

![További információk][api-management-developer-portal-calc-api-console]

Adja meg a paraméterek egyes értékeit vagy tartani az alapértelmezett beállításokat, és kattintson a **Küldés**.

![HTTP Get][api-management-invoke-get]

Művelet hivatkoznak, miután a developer Portal segítségével **válaszállapota**, a **Válasz fejlécek**és **Válasz tartalmat**jeleníti meg.

![Válasz][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Statisztikák megtekintése

Egyszerű számológép analitika megtekintése, váltson vissza a publisher-portálon **kezelése** választhatja ki a menü felső sarkában látható a developer Portal segítségével.

![Kezelése][api-management-manage-menu]

Az alapértelmezett nézet, a publisher portál az **Irányítópult**, amelynek áttekintést nyújt az API-kezelés példány.

![Irányítópult][api-management-dashboard]

Mutasson a egérrel a diagram **Egyszerű számológép** megjelenítéséhez a adott mérési módja miatt a használatát a API-t egy adott időszakra.

>[AZURE.NOTE] Ha bármely sor nem látható a diagramon, térjen vissza a developer Portal segítségével a és az API be néhány hívásokat, várjon néhány percet és majd térjen vissza az irányítópult.

Kattintson a **Részletek** megtekintése a összefoglaló lapon az API, beleértve a megjelenített mérési módja miatt nagyobb változatát.

![Elemző][api-management-mouse-over]

![Összefoglalás][api-management-api-summary-metrics]

Részletes mértékek és a jelentések kattintson a bal oldali menüjében **API-kezelés** **Analytics** .

![– Áttekintés][api-management-analytics-overview]

A **Analytics** szakaszt a következő négy lapokon foglalja magában:

-   **Áttekintése** általános használati és mértékek, valamint a felső fejlesztők, és biztosít a legjobb termékek, felső API-khoz felső műveletek.
-   **Használatát** a API-hívások és sávszélességet, beleértve a földrajzi grafikusan is meg szeretné vizsgálni működésébe biztosít.
-   **Állapot** fókusz állapot kódokat a gyorsítótár-sikerességéről, a válasz időpontok és az API és válaszidő szolgáltatás.
-   **Tevékenység** -jelentések Lehatolás fejlesztőeszközök, termék, API-val és a művelet az adott tevékenység biztosít.

## <a name="next-steps"> </a>Következő lépések

- Megtudhatja, hogyan [védelme a API a ráta korlátai](api-management-howto-product-with-rules.md).

[Azure ingyenes próbaverziója]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Értesítések és az e-mail sablonok konfigurálása Azure API kezelése]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Árak API-kezelés]: http://azure.microsoft.com/pricing/details/api-management/

[Azure klasszikus portál]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
