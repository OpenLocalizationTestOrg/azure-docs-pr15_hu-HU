<properties
    pageTitle="Az iOS Mobile-alkalmazások Azure hitelesítési hozzáadása"
    description="Megtudhatja, hogy miként Azure Mobile-alkalmazások használata az iOS-alkalmazás – Identitásszolgáltatók, beleértve a AAD, a Google, a Facebook, a Twitteren és a Microsoft számos felhasználói hitelesítést végezni."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Hitelesítés hozzáadása az iOS-alkalmazás

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Ebben az oktatóanyagban hitelesítési egy támogatott identitásszolgáltató használata az [iOS gyors indítása] projekt hozzáadása. Ebben az oktatóprogramban az [iOS gyors indítása] oktatóprogram, amely el kell végeznie alapul.

##<a name="register"></a>Regisztrálni a hitelesítést az alkalmazást, és az alkalmazás szolgáltatás konfigurálása

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Hitelesített felhasználói engedélyek korlátozása

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode nyomja le a **Futtatás** kattintva elindíthatja az alkalmazást. Kivétel fog kell emelni, mert az alkalmazás nem hitelesített felhasználóként a kódmentes csatlakozna, de _TodoItem_ táblázat most hitelesítést igényel lehetőséget.

##<a name="add-authentication"></a>Hitelesítés felvétele az alkalmazásba

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[rövid útmutató az iOS]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
