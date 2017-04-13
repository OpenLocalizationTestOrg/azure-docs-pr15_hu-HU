<properties
    pageTitle="Hitelesítés hozzáadása a Mobile-alkalmazások Android-eszközön |} Azure alkalmazás szolgáltatás"
    description="Megtudhatja, hogy miként Azure App szolgáltatásban Mobile-alkalmazások használata az Android-alkalmazás keresztül sokféle Identitásszolgáltatók, beleértve a Google, a Facebook, a Twitteren és a Microsoft felhasználói hitelesítő."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Az Android-alkalmazás hitelesítési hozzáadása

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Összefoglalás

Ebben az oktatóanyagban vesz fel hitelesítési egy támogatott identitásszolgáltató használata az Android listájába quickstart útmutató projekt. Ebben az oktatóprogramban az [Mobile-alkalmazások – első lépések] oktatóprogram, amely el kell végeznie alapul.

##<a name="register"></a>Regisztrálni a hitelesítést az alkalmazást, és az alkalmazás szolgáltatás konfigurálása

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Az Android Studio nyissa meg a projekt a tervezett kitöltötte az oktatóprogram [Mobile-alkalmazások – első lépések]. Kattintson a **Futtatás** menü **alkalmazás futtatásához** kattintson, és ellenőrizze, hogy az alkalmazás indítása után következik (nem engedélyezett) 401 állapotkódot a esetén nem kezelt kivételt.

     Ez a kivétel oka az, hogy az alkalmazás nem hitelesített felhasználóként a kódmentes csatlakozna, de a _TodoItem_ táblázat most hitelesítést igényel lehetőséget.

Ezt követően a felhasználók hitelesítését előtt erőforrások kér a mobilalkalmazás kódmentes alkalmazás frissítenie.

## <a name="add-authentication-to-the-app"></a>Hitelesítés felvétele az alkalmazásba

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Gyorsítótár hitelesítési tokenek az ügyfélszámítógépen

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Következő lépések

Alapszintű hitelesítés oktatóprogram befejezte, érdemes megfontolni a Folytatás megjelenítheti az alábbi oktatóanyagok egyikét:

+ [Az Android-alkalmazás hozzáadása leküldéses értesítések](app-service-mobile-android-get-started-push.md) További információ a mobilalkalmazás kódmentes Azure értesítés hubok segítségével küldje el a leküldéses értesítések konfigurálása.

+ [Kapcsolat nélküli szinkronizálás az Android-alkalmazás engedélyezése](app-service-mobile-android-get-started-offline-data.md) Megtudhatja, hogy miként vehet fel az offline munka támogatása a alkalmazást egy mobilalkalmazás kódmentes. Kapcsolat nélküli szinkronizálás lehetővé teszi a felhasználóknak használata mobilalkalmazásban&mdash;megtekintése, hozzáadása vagy módosítása, adatok&mdash;akkor is, ha nincs hálózati kapcsolat.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobile-alkalmazások – első lépések]: app-service-mobile-android-get-started.md
