<properties 
    pageTitle="Hogyan engedélyezheti a fejlesztői fiókot Azure Active Directory Azure API kezelése" 
    description="Megtudhatja, hogy miként engedélyezheti a felhasználók Azure Active Directory API-kezelés." 
    services="api-management" 
    documentationCenter="API Management" 
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

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hogyan engedélyezheti a fejlesztői fiókot Azure Active Directory Azure API kezelése


## <a name="overview"></a>– Áttekintés
Ez az útmutató megtudhatja, hogy hogyan hozzáférést biztosít a developer Portal segítségével a minden felhasználónak egy vagy több Azure Active könyvtárak. Ez az útmutató is bemutatja, hogyan kezelheti a csoportokat az Azure Active Directory-felhasználók, amelyeknél a felhasználók az Azure Active Directory külső csoportok hozzáadásával.

>Hajtsa végre a lépéseket, ez az útmutató az első, amelyben az alkalmazások létrehozása az Azure Active Directory kell rendelkeznie.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Hogyan Fejlesztőeszközök fiókot Azure Active Directory engedélyezése

Első lépésként kattintson a **kezelés** az Azure klasszikus portálon a API-kezelési szolgáltatáshoz elemre. Ekkor megjelenik az API kezelése a publisher-portálra.

![Publisher-portál][api-management-management-console]

>Ha még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

Kattintson a **Biztonság** gombra a bal oldali menüjében **API-kezelés** , és kattintson a **Külső identitások**.

![Külső identitások][api-management-security-external-identities]

Kattintson az **Azure Active Directory**. Jegyezze le a **Átirányítás URL-cím** és a kapcsoló az Azure Active Directory az Azure klasszikus portálon.

![Külső identitások][api-management-security-aad-new]

Kattintson a **Hozzáadás** gombra kattintva hozzon létre egy új Azure Active Directory-alkalmazást, és válassza a **szervezeten elkészítésének az alkalmazás hozzáadása**.

![Új Azure Active Directory-alkalmazás hozzáadása][api-management-new-aad-application-menu]

Írjon be egy nevet az alkalmazáshoz, jelölje be a **webalkalmazás és/vagy a webes API -t**, és kattintson a Tovább gombra.

![Új Azure Active Directory-alkalmazás][api-management-new-aad-application-1]

**Bejelentkezési URL**írja be a bejelentkezési URL-CÍMÉT a developer Portal segítségével. Ebben a példában a **bejelentkezési URL** van `https://aad03.portal.current.int-azure-api.net/signin`. 

Az **Alkalmazás azonosítója URL-címet**az alapértelmezett tartomány vagy a saját tartomány megadása az Azure Active Directory, és egy egyedi karakterlánc hozzáfűzni. Ebben a példában az alapértelmezett tartomány **https://contoso5api.onmicrosoft.com** **/api** megadott utótaggal használja.

![Azure Active Directory-alkalmazás új tulajdonságai][api-management-new-aad-application-2]

A jelölőnégyzet gombra kattintva mentse, és az új alkalmazás létrehozása, és kattintson az új alkalmazás beállítása a **beállítás** fülre.

![Új Azure Active Directory-alkalmazás létrehozása][api-management-new-aad-app-created]

Ehhez az alkalmazáshoz használható több Azure Active könyvtárak tervezi, kattintson az **Igen** gombra az **alkalmazás több bérlői**. Alapértelmezés szerint **nincs**.

![Alkalmazása több bérlői][api-management-aad-app-multi-tenant]

A **Külső identitások** lap a publisher-portálon **Azure Active Directory** szakaszáról **Átirányítás URL-cím** másolása, és illessze be a **Válasz URL-CÍMRE** mezőbe. 

![Válasz URL-címe][api-management-aad-reply-url]

Görgessen a configure lap aljára, a **Szolgáltatásalkalmazás jogosultságainak** legördülő listában válassza ki, és jelölje be a **címtár-adatok olvasása**.

![Szolgáltatásalkalmazás jogosultságainak][api-management-aad-app-permissions]

Jelölje ki a **Hozzáférési jogosultságok** legördülő listában, és jelölje be a **Bejelentkezés engedélyezése, és olvassa el a felhasználói profilok**.

![Delegált engedélyek][api-management-aad-delegated-permissions]

>Többet szeretne tudni az alkalmazás és a meghatalmazott engedélyeit olvassa el a [elérése a Graph API][]című témakört.

Az **Ügyfél-azonosító** másolja a vágólapra.

![Ügyfél-azonosító][api-management-aad-app-client-id]

Lépjen vissza az publisher-portálra, és illessze be az **Ügyfél-azonosító** másolja át az Azure Active Directory-alkalmazás konfigurációjának.

![Ügyfél-azonosító][api-management-client-id]

Lépjen vissza az Azure Active Directory konfigurációs, és kattintson a **Jelölje ki az időtartam** legördülő **billentyűk** szakaszában, és adja meg a elvégezve. Ebben a példában az **1 év** használják.

![Kulcs][api-management-aad-key-before-save]

Mentse a konfigurációt, és a kulcs megjelenítéséhez a **Mentés** gombra. Másolja a vágólapra a billentyűt.

>Jegyezze fel a következő kulcsot. Miután bezárja Azure Active Directory konfigurációs ablakában, a kulcs nem jelenik meg újból.

![Kulcs][api-management-aad-key-after-save]

Lépjen vissza a publisher-portálra, és a kulcs illessze be az **Ügyfél titkos** szöveg mezőbe.

![Ügyfél titkos kulcs][api-management-client-secret]

**Engedélyezett bérlők** Itt adhatja meg, mely könyvtárak van hozzáférése az API Management szolgáltatás példányának az API-khoz. Adja meg a tartományokat az Azure Active Directory-példány, amelyhez hozzáférést szeretne. Több tartomány ágyazódjanak, szóközzel vagy vesszővel is külön.

![Engedélyezett bérlők][api-management-client-allowed-tenants]

Több tartományt is megadható a **Bérlők engedélyezett** szakaszban. Mielőtt bármelyik felhasználó is jelentkezik be az eredeti tartomány, ahol az alkalmazás lett regisztrálva eltérő tartományban, különböző tartomány globális rendszergazdának engedélyt kell beállítania a címtár-adatok access alkalmazás. Engedélyek biztosítása globális rendszergazdának kell jelentkezzen be az alkalmazás és az **Elfogadás**gombra. A következő példában `miaoaad.onmicrosoft.com` **Bérlők engedélyezett** és a globális lett hozzáadva azt a tartományt a rendszergazda naplózás az első alkalommal.

![Engedélyek][api-management-permissions-form]

>Bejelentkezés után által egy globális rendszergazdai engedélyekkel nem globális rendszergazdának kísérel meg, ha a bejelentkezési kísérlet sikertelen lesz, és egy hiba képernyő jelenik meg.

Miután a kívánt konfiguráció van megadva, akkor kattintson a **Mentés**gombra.

![Mentés][api-management-client-allowed-tenants-save]

Miután menti a módosításokat, a megadott Azure Active Directory felhasználóinak is jelentkezzen be a Developer Portal segítségével [Jelentkezzen be az Azure Active Directory-fiókkal Developer portal][]lépéseit követve.

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Hogyan lehet hozzáadni egy külső Azure Active Directory-csoportnak

Az Azure Active Directory felhasználók hozzáférésének engedélyezése, után, Azure Active Directory-csoportok hozzáadhat API-kezelés csoportjában a fejlesztők társítás a kívánt termékek könnyebben kezelheti.

> Adja meg egy külső Azure Active Directory-csoportot, hogy az Azure Active Directory először kell konfigurálni az azonosítók lapján az előző szakaszban az eljárást követve. 

Külső Azure Active Directory-csoportok a **láthatóság** lapon, a termék, amelynek hozzáférést a csoportba szeretné kerülnek. Kattintson a **Products**elemre, és válassza ki a kívánt termék nevét.

![Termék konfigurálása][api-management-configure-product]

Kattintson a **láthatóság** fülre, majd kattintson a **Csoportok hozzáadása az Azure Active Directory**.

![Csoportok hozzáadása][api-management-add-groups]

Jelölje ki az **Azure Active Directory bérlői** a legördülő listából, és írja be a kívánt csoport nevére a **csoportok** hozzá kell adni a szövegdobozt.

![Jelölje be a csoport][api-management-select-group]

A csoport neve található a **csoportok** listában az Azure Active Directory, a következő példában látható módon.

![Azure Active Directory-csoportok listája][api-management-aad-groups-list]

Kattintson a **Hozzáadás** ellenőrzése a csoport nevét, és adja hozzá a csoportot. Ebben a példában a **Contoso 5 fejlesztők** külső csoport megjelenik. 

![Csoport hozzáadása][api-management-aad-group-added]

Az új csoport kijelölés mentése a **Mentés** gombra.

Miután az Azure Azure Active Directory-csoportot egy termék konfigurált, érhető el a **láthatóság** lapon az API Management szolgáltatás példány termékek ellenőrizni kívánt.

Tekintse át, és állítsa be a külső csoport tulajdonságait, miután ez megtörtént, kattintson a **csoportok** lapon a csoport nevét.

![A csoportok kezelése][api-management-groups]

További lehetőségek szerkesztheti, hogy a **név** és a csoport **leírása** .

![Csoport szerkesztése][api-management-edit-group]

A felhasználók az beállított Azure Active Directory jelentkezzen be a Fejlesztőeszközök portál és -nézet és előfizetés azokat a csoportokat, amelynek a következő részben leírt utasításokat követve rendelkeznek láthatóságát.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Hogyan jelentkezhet be az Azure Active Directory-fiókkal, a Developer Portal segítségével

Jelentkezzen be a Developer Portal segítségével az előző szakaszokban konfigurált Azure Active Directory-fiókot használ, nyissa meg a egy új böngészőablakban, az Active Directory-alkalmazás konfigurációjának a **bejelentkezési URL** használatával, és kattintson az **Azure Active Directory**.

![Developer Portal segítségével][api-management-dev-portal-signin]

Írja be a hitelesítő adatokat, az egyik azoknak a felhasználóknak az Azure Active Directory, és kattintson a **Bejelentkezés**gombra.

![bejelentkezés][api-management-aad-signin]

Kérheti olyan regisztrációs űrlap, ha minden további információkra szükség. Töltse ki a regisztrációs űrlap, és kattintson a **regisztráció**gombra.

![Regisztráció][api-management-complete-registration]

A felhasználó most már be van jelentkezve a developer Portal segítségével a API Management szolgáltatás példány be.

![Regisztráció kész][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

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
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[A diagram API elérése]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Bejelentkezés az Azure Active Directory-fiókkal, a Developer Portal segítségével]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

