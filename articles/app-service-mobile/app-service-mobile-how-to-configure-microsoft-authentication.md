<properties
    pageTitle="Hitelesítés Microsoft Account az alkalmazás Services alkalmazás beállítása"
    description="Hitelesítés Microsoft Account az alkalmazás Services alkalmazás konfigurálásának ismertetése."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Az alkalmazás szolgáltatásalkalmazás használatához Microsoft Account bejelentkezni konfigurálása

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan Azure alkalmazás szolgáltatás használatához Microsoft Account hitelesítési szolgáltatóként beállításához. 

## <a name="register-microsoft-account"> </a>Az alkalmazás regisztrálása a Microsoft-fiók

1. Jelentkezzen be az [Azure-portálra], és nyissa meg az alkalmazást. Másolja a vágólapra az **URL-CÍMÉT**, amelyekkel újabb az alkalmazás konfigurálása a Microsoft-Account.

2. Nyissa meg a Microsoft Account Developer Center a [Saját alkalmazások] lapot, és jelentkezzen be Microsoft-fiókjával, ha szükséges.

3. Kattintson **az alkalmazás hozzáadása**, majd írjon be egy alkalmazásnevet, és kattintson az **alkalmazás létrehozása**.

4. Jegyezze le az **Alkalmazás azonosítója**, később lesz szükség szerint. 

5. "Platformok," csoportban kattintson a **Platform hozzáadása** , és válassza a "Webhely".

6. "Átirányítás URL-címe" csoportban adja meg az alkalmazás az végpontot, majd kattintson a **Mentés**gombra. 
 
    >[AZURE.NOTE]Az átirányítási URI az alkalmazás, az elérési út _/.auth/login/microsoftaccount/callback_fűzött URL-CÍMÉT. Ha például `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Győződjön meg arról, hogy a HTTPS sorrendben kell használnia.

7. "Alkalmazás titkos kulcsok" csoportban kattintson az **Új jelszó létrehozása**. Jegyezze fel a megjelenő értéket. Miután elhagyja a lapon, akkor nem jelennek meg újra.


    > [AZURE.IMPORTANT] A jelszó nem egy fontos biztonsági hitelesítő adatait. Ne ossza meg a jelszót mással és terjesztése azt egy ügyfélalkalmazásban.

## <a name="secrets"> </a>Az alkalmazás szolgáltatásalkalmazás információt a Microsoft-fiók felvétele

1. Vissza az [Azure portálon]nyissa meg az alkalmazást, kattintson a **Beállítások** > **hitelesítési / engedélyezési**.

2. Ha a hitelesítés / engedélyezési funkció nincs engedélyezve, kapcsolja **be**.

3. Kattintson a **Microsoft-fiókjával**. Illessze be az alkalmazás azonosítója és jelszava időértékek, amelyek korábban szerezte be, és tetszés szerint engedélyezése bármelyik hatókörök az alkalmazáshoz szükséges. Kattintson **az OK**gombra.

    ![][1]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

4. (Nem kötelező) Elérésének korlátozására csak a Microsoft-fiók által hitelesített felhasználók a webhelyen, állítsa **műveleteket, ha nem lehet kérelem hitelesíteni** **Microsoft-fiókjával**. Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a hitelesítés Microsoft-fiókjával.

5. Kattintson a **Mentés**gombra.

Most már készen áll az alkalmazás a hitelesítés Microsoft-Account használatához.

## <a name="related-content"> </a>Kapcsolódó tartalom

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Saját alkalmazások]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portál]: https://portal.azure.com/
