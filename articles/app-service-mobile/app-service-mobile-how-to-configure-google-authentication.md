<properties
    pageTitle="Az alkalmazás Services alkalmazás a Google-hitelesítés konfigurálása"
    description="Megtudhatja, hogy miként állítsa be a Google-hitelesítést az alkalmazás Services alkalmazás."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Az alkalmazás használata Google bejelentkezési szolgáltatásalkalmazás konfigurálása

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Ez a témakör bemutatja, hogyan használhatja a Google-hitelesítés konferenciaszolgáltatóként Azure-alkalmazás szolgáltatás konfigurálása.

Ebben a témakörben művelethez rendelkeznie egy igazolt e-mail címe Google-fiók. Hozzon létre egy új Google-fiókot, keresse fel a [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Regisztrálja az alkalmazást a Google

1. Jelentkezzen be az [Azure-portálra], és nyissa meg az alkalmazást. Másolja a vágólapra az **URL-CÍMÉT**, mellyel később a Google-app konfigurálása.

2. Keresse meg a [Google API](http://go.microsoft.com/fwlink/p/?LinkId=268303) -webhelyet, jelentkezzen be a Google-fiók hitelesítő adatait, kattintson a **Projekt létrehozása**adja meg a **projekt nevét**, majd kattintson a **Létrehozás**gombra.

3. **Közösségi API -k** csoportban kattintson a **Google + API** , majd **engedélyezze**.

4. A bal oldali navigációs sávban a **hitelesítő adatok** > **OAuth beleegyezés képernyőre**, majd jelölje be az **E-mail címét**, írja be a **Termék neve**, és kattintson a **Mentés**gombra.

5. A **hitelesítő adatait** lapon kattintson a **Létrehozás hitelesítő adatok** > **OAuth ügyfél-azonosító**, majd válassza a **webalkalmazásban**.

6. Illessze be az előbb másolt **Engedélyezni a JavaScript forrásokból**alkalmazás szolgáltatás **URL-CÍMÉT** , majd illessze be az átirányítási URI **Átirányítás URI engedélyezett**. Az átirányítás a URI az alkalmazás, az elérési út _/.auth/login/google/callback_fűzött URL-CÍMÉT. Ha például `https://contoso.azurewebsites.net/.auth/login/google/callback`. Győződjön meg arról, hogy a HTTPS sorrendben kell használnia. Kattintson a **Létrehozás**gombra.

7. A következő képernyőn jegyezze fel az ügyfél-azonosító és az ügyfél titkos kulcs értékeit.


    > [AZURE.IMPORTANT]
    Az ügyfél titkos kulcs egy fontos biztonsági hitelesítő adatait. Ne ossza meg a titkos mással és terjesztése azt egy ügyfélalkalmazásban.


## <a name="secrets"> </a>Az alkalmazás hozzáadása a Google-információ

8. Vissza az [Azure portálon]nyissa meg az alkalmazást. Kattintson a **Beállítások**, majd **hitelesítési / engedélyezési**.

9. Ha a hitelesítés / engedélyezési funkció nincs engedélyezve, kapcsolja be a Váltás **a**.

10. Kattintson a **Google**. Illessze be az alkalmazás azonosítója és az alkalmazás titkos időértékek, amelyek korábban szerezte be, és tetszés szerint engedélyezése bármelyik hatókörök az alkalmazáshoz szükséges. Kattintson **az OK**gombra.

    ![][1]

    Alapértelmezés szerint az alkalmazás szolgáltatás hitelesítésről, de nem engedélyezett hozzáférés korlátozása a webhelytartalom és az API-hoz. Alkalmazás kódban engedélyeznie kell a felhasználókat.

17. (Nem kötelező) Elérésének korlátozására csak azok a felhasználók a Google hitelesíteni a webhelyen, állítsa **a Google** **kérelem nem hitelesítése esetén végrehajtandó műveletet** . Ehhez az összes kérelmet hitelesíteni, és az összes hitelesített kérés megnyílik a Google-hitelesítést.

12. Kattintson a **Mentés**gombra.

Most már készen áll a Google az alkalmazásban hitelesítéshez használt.

## <a name="related-content"> </a>Kapcsolódó tartalom

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portál]: https://portal.azure.com/

