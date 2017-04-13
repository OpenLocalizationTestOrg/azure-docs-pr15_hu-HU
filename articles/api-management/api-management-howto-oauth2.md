<properties 
    pageTitle="Hogyan engedélyezheti a fejlesztői fiókot OAuth 2.0-s Azure API kezelése" 
    description="Megtudhatja, hogy miként engedélyezheti a felhasználók OAuth 2.0 API-kezelés." 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hogyan engedélyezheti a fejlesztői fiókot OAuth 2.0-s Azure API kezelése

Sok API-khoz [OAuth 2.0-s](http://oauth.net/2/) biztonságos az API-t, és győződjön meg arról, hogy csak érvényes felhasználók férnek hozzá, és csak elérhetik, amelyre jogosult esetén erőforrások támogatja. Annak érdekében, hogy ilyen API-khoz Azure API's interaktív Developer kezelőkonzol használatához a szolgáltatás lehetővé teszi, hogy állíthatja be a szolgáltatás példánya 2.0-s engedélyezett API a OAuth végezhető.

## <a name="prerequisites"> </a>Vonatkozó követelmények

Ez az útmutató megtudhatja, hogy miként az API-kezelés szolgáltatás példánya OAuth 2.0-s engedélyezési használandó Fejlesztőeszközök fiókok konfigurálása, de Ön nem látható egy OAuth 2.0-szolgáltatóval konfigurálásáról. A konfiguráció minden OAuth 2.0-szolgáltató eltér, annak ellenére a lépések hasonlóak, és a szükséges információkat vehet OAuth 2.0-s konfigurálása a API Management szolgáltatás példány használt megegyeznek. Ez a témakör bemutatja az Azure Active Directory-OAuth 2.0-konferenciaszolgáltatóként használja példákat.

>[AZURE.NOTE] OAuth 2.0-s verzióját használja az Azure Active Directory konfigurálásával kapcsolatos további tudnivalókért lásd: a [Webalkalmazás-GraphAPI-DotNet][] minta.

## <a name="step1"> </a>API-kezelés OAuth 2.0-s engedélyezési kiszolgáló beállítása

Első lépésként kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

>[AZURE.NOTE] Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

Kattintson a **Biztonság** gombra a bal oldalon a **API-kezelés** menüben kattintson a **OAuth 2.0-s**, és kattintson a **Hozzáadás engedélyezési kiszolgáló**.

![OAuth 2.0-s verziója][api-management-oauth2]

**Engedély kiszolgáló hozzáadása**gombra kattint, az új engedélyezési kiszolgáló űrlap jelenik meg.

![Új kiszolgáló][api-management-oauth2-server-1]

Írja be a nevét és leírását a **név** és **Leírás** mezőkbe. 

>[AZURE.NOTE] Ezeket a mezőket az OAuth 2.0-s engedélyezési kiszolgáló belül az aktuális API Management-példány azonosítására használt, és az értékeket nem a OAuth 2.0-kiszolgálóról származnak.

Írja be az **ügyfél regisztrációs lap URL-CÍMÉT**. Ez a lap, ahol felhasználók létrehozása és azok a fiókok kezelése és használt OAuth 2.0-szolgáltató függően változik. Az **ügyfél regisztrációs lap URL-címe** mutat, a lapot, amelyet a felhasználók létrehozása és a saját fiók konfigurálása a felhasználói fiókok kezelése támogató szolgáltatók OAuth 2.0-s segítségével. Egyes szervezetek beállítása vagy nem használja ezt a funkciót, még akkor is, ha az OAuth 2.0-s szolgáltató támogatja. Ha a OAuth 2.0-s szolgáltató nincs felhasználókezelés konfigurált fiókok, írja be az egy helyőrző URL-CÍMÉT, például a vállalat URL-CÍMÉT, vagy egy URL-CÍMÉT például `https://placeholder.contoso.com`.

A következő szakaszban az űrlap tartalmazza a **engedélyezési kód megadása típusok**, **engedélyezési végpont URL-CÍMÉT**, és **engedélyt kérelmek módja** beállításait.

![Új kiszolgáló][api-management-oauth2-server-2]

Adja meg a **engedélyezési kód megadása típusok** , jelölje be a kívánt típusú. **Engedélyezési kód** alapértelmezés szerint van megadva.

Írja be az **Engedélyezés végpont URL-CÍMÉT**. Azure Active Directory URL-címe lesz hasonlóan, mint az alábbi URL-címet, ahol `<client_id>` az ügyfél-azonosító, amely azonosítja az alkalmazást az OAuth 2.0 kiszolgáló helyére.

    https://login.windows.net/<client_id>/oauth2/authorize

Az **engedély kérelmek módja** Itt adhatja meg, hogyan küldi el az engedélyezési kérelmet OAuth 2.0-kiszolgálóhoz. Alapértelmezés szerint **első** van jelölve.

A következő szakaszban, ahol a **Token végpont URL-CÍMÉT**, **ügyfél hitelesítési módszereket**, **módszer küldése jogkivonat**, és **alapértelmezett hatókör** meg vannak adva.

![Új kiszolgáló][api-management-oauth2-server-3]

Azure Active Directory OAuth 2.0 kiszolgáló esetén **Token végpont URL-címe** lesz a következő formátumban, ahol `<APPID>` formátumának van `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Az alapértelmezett beállítás, az **ügyfél hitelesítési módszereket** : **egyszerű**, és **hozzáférési jogkivonat módszer küldése** **engedélyezési fejléc**. Ezeket az értékeket Ez a szakasz az űrlap **alapértelmezett hatókör**együtt kell beállítani.

Az **ügyfél hitelesítő adatok** szakasz tartalmazza az **Ügyfél-azonosító** és a **titkos ügyfél**, előállított OAuth 2.0-s kiszolgálója létrehozása és konfigurálása során. Az **Ügyfél-azonosító** , és az **ügyfél titkos** megadása után a **redirect_uri** a **engedélyezési kód** jön létre. A URI szolgál a válasz URL-cím beállítása a OAuth 2.0-s kiszolgáló beállításait.

![Új kiszolgáló][api-management-oauth2-server-4]

Ha **engedélyezési kód megadása típusú** **erőforrás-tulajdonosi jelszó**van beállítva, az **erőforrás tulajdonosa jelszó hitelesítő adatok** szakasz használatával adja meg a hitelesítő adatokat; egyéb esetben üresen azt.

![Új kiszolgáló][api-management-oauth2-server-5]

Az űrlap elkészülte mentéséhez kattintson **mentése** a API Management OAuth 2.0-s engedélyezési beállítása. Miután mentette a kiszolgáló beállításait, beállíthatja API-khoz ebben a konfigurációban használni, a következő szakaszban látható módon.

## <a name="step2"> </a>Egy API használata OAuth 2.0 felhasználói hitelesítés konfigurálása

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , kattintással jelölje ki a kívánt API, kattintson a **Biztonság**gombra, és jelölje be a jelölőnégyzetet az **OAuth 2.0**.

![Felhasználói engedély][api-management-user-authorization]

Jelölje ki a kívánt **engedélyt kiszolgáló** a legördülő listából, és kattintson a **Mentés**gombra.

![Felhasználói engedély][api-management-user-authorization-save]

## <a name="step3"> </a>A Developer Portal segítségével az OAuth 2.0 felhasználói engedély ellenőrzése

Miután beállította a OAuth 2.0 engedélyezési kiszolgáló és a API-t használja a kiszolgálón beállított, a Developer Portal segítségével és az API hívása tesztelheti.  Kattintson a jobb felső menü **Developer Portal segítségével** .

![Developer Portal segítségével][api-management-developer-portal-menu]

Kattintson a felső menü **API-hoz** , és válassza a **Visszhang API -val**.

![A visszhang API][api-management-apis-echo-api]

>[AZURE.NOTE] Ha csak egy API konfigurált vagy látható a fiókjához, válassza az API-khoz megnyitja közvetlenül a műveleteket, hogy API-val.

Jelölje ki az **Első erőforrás** -művelet, kattintson a **Megnyitás konzol**és **engedélyezési kód** válassza a legördülő listában.

![Megnyitott konzol][api-management-open-console]

**Engedélyezési kód** kijelölésekor előugró ablak az OAuth 2.0-szolgáltató bejelentkezési képernyő jelenik meg. Ebben a példában a bejelentkezési képernyőn Azure Active Directory által biztosított.

>[AZURE.NOTE] Letiltott előugró Ha kérni fogja engedélyezi őket a böngészőben. Miután engedélyezte őket, jelölje be **engedélyezési kód** újra és a bejelentkezési képernyőn jelenik meg.

![bejelentkezés][api-management-oauth2-signin]

Ha be van jelentkezve a, a **kérelem élőfejek** feltöltve egy `Authorization : Bearer` fejléc, amely engedélyezi a kérést.

![Élőfej jogkivonat kérése][api-management-request-header-token]

Ezen a ponton a kívánt értékeket a többi paraméterek beállítása, valamint a kérelmet. 

## <a name="next-steps"></a>Következő lépések

OAuth 2.0-s és az API-kezelés használatával kapcsolatos további tudnivalókért lásd: az alábbi videó és a mellékelt [cikk](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API-kezelés – első lépések]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[Webalkalmazás-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

