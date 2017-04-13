<properties
    pageTitle="Az alkalmazás Services alkalmazás Twitter hitelesítés konfigurálása"
    description="Megtudhatja, hogy miként Twitter hitelesítés az alkalmazás Services alkalmazás."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Az alkalmazás szolgáltatásalkalmazás használatához a Twitteren bejelentkezési konfigurálása

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan használhatja a Twitter-hitelesítés konferenciaszolgáltatóként Azure-alkalmazás szolgáltatás konfigurálása.

Ez a témakör a művelethez a egy igazolt e-mail cím és telefonszám megadása tartalmazó Twitter fiókkal kell rendelkeznie. Hozzon létre egy új Twitter-fiókot, keresse fel a <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Az alkalmazás regisztrálása a Twitteren


1. Jelentkezzen be az [Azure-portálra], és nyissa meg az alkalmazást. Másolja a vágólapra az **URL-CÍMÉT**. A Twitter app konfigurálása használandó.

2. Keresse meg a [Fejlesztők Twitter] webhelyet, jelentkezzen be a Twitteren fiók hitelesítő adatait, és kattintson az **Új alkalmazás létrehozása**.

3. Írja be a **nevét** és **leírását** az új alkalmazás. Illessze be az értéket a **webhely** **URL-CÍMÉT** az alkalmazást. A **Visszahívási URL-CÍMÉT**, ezután másolja az előbb másolt **Visszahívási URL-CÍMÉT** . Ez a mobilalkalmazás kezdőlapja az elérési út _/.auth/login/twitter/callback_fűzött. Ha például `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Győződjön meg arról, hogy a HTTPS sorrendben kell használnia.

3.  A képernyő alján az oldal olvassa el és fogadja el a feltételeket. Kattintson a **Létrehozás a Twitteren alkalmazása**gombra. Ez regisztrál az alkalmazás jelenít meg az alkalmazás részleteit.

4. Kattintson a **Beállítások** fülre, jelölje be a **engedélyezni kívánja a Twitter jelentkezzen alkalmazás**, majd kattintson a **Frissítési beállítások**parancsra.

5. Jelölje ki a **kulcsok és a hozzáférési jogkivonat** fülre. Jegyezze fel a **Fogyasztói kulcs (API -val)** és a **fogyasztói titkos kulcs (API titkos)**értékeit.

    > [AZURE.NOTE] A fogyasztói titkos kulcs egy fontos biztonsági hitelesítő adatait. Ne ossza meg a titkos mással és terjesztése azt az alkalmazást.


## <a name="secrets"> </a>Az alkalmazás hozzáadása Twitter információk

13. Vissza az [Azure portálon]nyissa meg az alkalmazást. Kattintson a **Beállítások**, majd **hitelesítési / engedélyezési**.

14. Ha a hitelesítés / engedélyezési funkció nincs engedélyezve, kapcsolja be a Váltás **a**.

15. Kattintson a **Twitter**. Illessze be az alkalmazás azonosítója és az alkalmazás titkos értékeket, amelyeket korábban szerezte be. Kattintson **az OK**gombra.

    ![][1]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

17. (Nem kötelező) Elérésének korlátozására csak azok a felhasználók Twitter hitelesíteni a webhelyen, állítsa a **műveleteket, ha nem lehet kérelem hitelesíteni** **Twitter**. Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a Twitteren hitelesítéshez.

17. Kattintson a **Mentés**gombra.

Most már készen áll az alkalmazásban hitelesítéshez használt a Twitteren.

## <a name="related-content"> </a>Kapcsolódó tartalom

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[A fejlesztők Twitter]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portál]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
