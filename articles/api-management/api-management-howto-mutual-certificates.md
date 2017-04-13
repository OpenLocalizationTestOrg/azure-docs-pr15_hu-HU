<properties 
    pageTitle="Hogyan biztonságos háttéradatbázis szolgáltatások ügyfélprogram API-kezelés Azure hitelesítés" 
    description="Megtudhatja, hogy miként biztonságos háttéradatbázist szolgáltatások ügyfél tanúsítvány hitelesítéssel Azure API-kezelés." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Hogyan biztonságos háttéradatbázist szolgáltatások ügyfélprogram API-kezelés Azure hitelesítés

API-kezelés lehetővé teszi az API a háttéradatbázist szolgáltatáshoz való hozzáférés biztonságossá tétele ügyfél tanúsítványok használatával. Ez az útmutató a tanúsítványok az API publisher portál kezelése, és konfigurálása a háttéradatbázist szolgáltatás eléréséhez egy tanúsítványt használni egy API jeleníti meg.

A API Management REST API tanúsítványok kezelésével kapcsolatos további tudnivalókért lásd [Azure API Management REST API-tanúsítvány entitás][].

## <a name="prerequisites"> </a>Vonatkozó követelmények

Ez az útmutató megtudhatja, hogy miként konfigurálható az API Management szolgáltatás példány elérni a háttéradatbázist szolgáltatást az API-ügyfél tanúsítvány-hitelesítés használatával. Ez a témakör a lépések végrehajtása előtt meg kell járassa a háttéradatbázist ügyfél tanúsítvány-hitelesítés ([konfigurálása az Azure webhelyek hitelesítés olvassa el ebben a cikkben][]) be van állítva, és rendelkezik hozzáféréssel a tanúsítvány és a jelszót az API kezelése a publisher-portálon feltöltése a tanúsítvány.

## <a name="step1"> </a>Töltse fel az ügyfél-tanúsítvány

Első lépésként kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![API a Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

A bal oldalon a **API-kezelés** menüben kattintson a **Biztonság** gombra, majd kattintson az **ügyfél tanúsítványok**.

![Ügyfél-tanúsítványok][api-management-security-client-certificates]

Új tanúsítvány feltölteni, kattintson a **tanúsítvány feltöltése**elemre.

![Töltse fel a tanúsítvány][api-management-upload-certificate]

Tallózással keresse meg azt a tanúsítványt, és írja be a jelszót a tanúsítvány.

>A tanúsítvány **.pfx** formátumban kell lennie. Önaláírt tanúsítványok engedélyezett.

![Töltse fel a tanúsítvány][api-management-upload-certificate-form]

Kattintson a **Töltse fel** a tanúsítvány feltöltése elemre.

>A tanúsítvány jelszót adott időben érvényességét. Ha még nem a megfelelő hibaüzenet jelenik meg.

![A tanúsítvány feltöltve.][api-management-certificate-uploaded]

Miután a tanúsítvány feltölteni, az **ügyfél tanúsítványok** lapon megjelenik. Ha több tanúsítvány közül, győződjön meg a tárgy megjegyzés, vagy a ujjlenyomatot, utolsó négy karaktert a tartalomkeresési-tanúsítványok, API hatálya alá, a következő szakaszban [konfigurálása az API átjáró hitelesítéshez ügyfél-tanúsítványt használni][] , jelölje be a tanúsítvány használt.

>Kikapcsolásához tanúsítvány-lánc érvényesítése használatakor, például önaláírt tanúsítvány – gyakori kérdések az [elem](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end)leírt lépésekkel.

## <a name="step1a"> </a>Ügyfél tanúsítvány törlése

Ha törölni szeretne egy tanúsítványt, kattintson a **Törlés** mellett a kívánt tanúsítványt.

![Tanúsítvány törlése][api-management-certificate-delete]

Kattintson a **Igen, törölheti** a megerősítéséhez.

![Törlés jóváhagyása][api-management-confirm-delete]

Ha a tanúsítványt használja-e egy API-t, egy figyelmeztetés képernyő jelenik meg. A tanúsítvány törlése a tanúsítvány első távolítsa el az használatához konfigurált bármely API-khoz.

![Törlés jóváhagyása][api-management-confirm-delete-policy]

## <a name="step2"> </a>Átjáró hitelesítéssel ügyfél-tanúsítványt használni egy API konfigurálása

**API-khoz** kattintson a bal oldali menüjében **API-kezelés** , kattintson a kívánt API nevére, és kattintson a **Biztonság** fülre.

![Biztonsági API][api-management-api-security]

A **hitelesítő adatokat tartalmazó** legördülő listában jelölje ki az **ügyfél tanúsítványok** .

![Ügyfél-tanúsítványok][api-management-mutual-certificates]

Jelölje ki a kívánt tanúsítványt az **ügyfél-tanúsítvány** legördülő listából. Ha több tanúsítvány közül megnézheti a tárgy vagy az utolsó négy karaktere az ujjlenyomatot, amint az előző szakaszban határozza meg a megfelelő tanúsítvány.

![Jelölje be a tanúsítvány][api-management-select-certificate]

A konfigurációs módosítást menti az API-nak a **mentése** gombra.

>Ez a változás azonnal érvénybe, és az adott API műveletek hívásainak fogja használni a tanúsítvány a háttéradatbázist kiszolgáló hitelesítést végezni.

![API módosításainak mentése][api-management-save-api]

>Ha egy tanúsítványt egy API-t az átjáró a háttéradatbázist szolgáltatás hitelesítési van megadva, a házirend részévé válik, hogy API, és megtekintheti a csoportházirend-szerkesztőben az.

![Tanúsítvány-házirend][api-management-certificate-policy]

## <a name="next-steps"></a>Következő lépések

Más módszerek a kódmentes szolgáltatásban, például a HTTP-egyszerű vagy megosztott titkos hitelesítés, biztonságos további információt az alábbi videó megtekintése

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure API-kezelés – első lépések]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance

[Azure API Management REST API-tanúsítvány entitás]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Állítsa be az Azure webhelyek hitelesítés olvassa el ez a cikk]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Egy átjáró hitelesítés ügyfél-tanúsítványt használatához API konfigurálása]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
