<properties
    pageTitle="Az alkalmazás Services alkalmazás Facebook-hitelesítés konfigurálása"
    description="Megtudhatja, hogy miként állítsa be az alkalmazás Services alkalmazás Facebook-hitelesítést."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>A Facebook bejelentkezési használandó alkalmazás szolgáltatásalkalmazás konfigurálása

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan használhatja a Facebook-hitelesítés konferenciaszolgáltatóként Azure-alkalmazás szolgáltatás konfigurálása.

Ez a témakör a művelethez rendelkeznie a Facebook-fiókját, amelynek a igazolt e-mail címre és egy telefonszámot. Hozzon létre egy új Facebook-fiókját, lépjen [Facebook.com weboldalt].

## <a name="register"> </a>Az alkalmazás regisztrálhatja a Facebookkal

1. Jelentkezzen be az [Azure-portálra], és nyissa meg az alkalmazást. Másolja a vágólapra az **URL-CÍMÉT**. A Facebook-alkalmazás konfigurálása használandó.

2. Egy másik böngészőablakban keresse fel a [Facebook-fejlesztők] webhelyet, és jelentkezzen be a Facebook-fiókja hitelesítő adatait.

3. (Nem kötelező) Ha már nem regisztrálta, kattintson az **alkalmazások** > **fejlesztői regisztrálni**, majd fogadja el a házirendet, és kövesse a regisztráció.

4. Kattintson a **saját alkalmazások** > **adja hozzá az új alkalmazás** > **webhely** > **átugrása, és hozza létre az alkalmazás azonosítója**. 

5. A **Megjelenítendő név**mezőbe írjon be egy egyedi nevet az alkalmazás, írja be a **Partner E-mail**, válasszon egy **kategóriát** az alkalmazás, majd kattintson az **Alkalmazás-azonosító létrehozása** és fejezze be a biztonsági jelölőnégyzetet. Ekkor megjelenik az új Facebook-alkalmazás a fejlesztői irányítópult.

6. "Facebook-bejelentkezés," csoportban kattintson az **Első lépések**. Vegye fel az alkalmazás **Átirányítás URI** **érvényes OAuth átirányítás URL-címe**, majd a **Módosítások mentése**gombra. 

    > [AZURE.NOTE] Az átirányítási URI az alkalmazás, az elérési út _/.auth/login/facebook/callback_fűzött URL-CÍMÉT. Ha például `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Győződjön meg arról, hogy a HTTPS sorrendben kell használnia.

6. A bal oldali navigációs sávon kattintson a **Beállítások**gombra. Az **Alkalmazás titkos** mezőben kattintson a **Megjelenítés**gombra, adja meg a jelszót, ha szükséges, majd jegyezze fel az **Alkalmazás azonosítója** és az **Alkalmazás titkos kulcs**értékeit. Akkor ezek későbbi felhasználás konfigurálhatja az alkalmazást az Azure-ban.

    > [AZURE.IMPORTANT] Az alkalmazás titkos kulcs egy fontos biztonsági hitelesítő adatait. Ne ossza meg a titkos mással és terjesztése azt egy ügyfélalkalmazásban.

7. A Facebook-fiókját az alkalmazás regisztrálása használt rendszergazdája-e az alkalmazást. Ezen a ponton csak a rendszergazdák is jelentkezzen be az Ez az alkalmazás. Hitelesíteni egyik Facebook-fiókból, kattintson a **Véleményezés alkalmazást** , és engedélyezze a **< - alkalmazás – név > Közzététel** általános nyilvános elérésének Facebook-hitelesítés használatával.

## <a name="secrets"> </a>Az alkalmazás hozzáadása a Facebook-információk

1. Vissza az [Azure portálon]nyissa meg az alkalmazást. Kattintson a **Beállítások** > **hitelesítési / engedélyezési**, és győződjön meg arról, hogy **Alkalmazás szolgáltatás hitelesítési** -e **kapcsolva**.

2. Kattintson a **Facebook**, illessze be az alkalmazás azonosítója és az alkalmazás titkos időértékek, amelyek korábban szerezte be, tetszés szerint engedélyezése bármelyik hatókörök szükség szerint az alkalmazás, majd kattintson az **OK gombra**.

    ![][0]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

3. (Nem kötelező) Elérésének korlátozására csak azok a felhasználók Facebook hitelesíteni a webhelyen, állítsa a **műveleteket, ha nem lehet kérelem hitelesíteni** **Facebook**. Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a Facebook-hitelesítést.

4. Ha elkészült a konfigurálás hitelesítés, kattintson a **Mentés**gombra.

Most már készen áll az alkalmazásban hitelesítéshez használt a Facebook.

## <a name="related-content"> </a>Kapcsolódó tartalom

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-fejlesztők számára]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com weboldalt]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portál]: https://portal.azure.com/
