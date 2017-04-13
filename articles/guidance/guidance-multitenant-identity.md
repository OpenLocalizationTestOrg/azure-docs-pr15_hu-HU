<properties
   pageTitle="Az Identitáskezelés Multitenant alkalmazások |} Microsoft Azure"
   description="Gyakorlati tanácsok a hitelesítés, engedélyezési és identitás kezelése multitenant-alkalmazásokban."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>A Microsoft Azure multitenant alkalmazások Identitáskezelés

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Sorozat gyakorlati tanácsok a hitelesítésről és az identitás kezelése az Azure Active Directory használatakor multitenancy, ismerteti.

Egy multitenant alkalmazást szeretne összeállítani, amikor közül az első kihívásokkal kapcsolatban van kezelése felhasználói identitások, mivel most már minden felhasználónak a bérlői webhelyre tartozik. Példa:

- Felhasználók a szervezeti hitelesítő adataival jelentkezzen be.
- Felhasználók kell hozzáférést szervezetük adatokat, de nem tartozik, más bérlők adatokat.
- Egy szervezet az alkalmazás regisztrálhat, és kattintson az alkalmazás szerepkörök hozzárendelése a tagok.

Azure Active Directory (Azure Active Directory) tartalmaz néhány nagyszerű szolgáltatással, amely támogatja az összes forgatókönyvekben.

Ez a cikksorozat fűzni, hogy is létrehozott a kész, [Végpontok közötti végrehajtása] [ tailspin] egy multitenant alkalmazás. A cikkek megfelelően, hogy milyen azt tanultakhoz te000129565 összeállítását, az alkalmazás. Első lépések az alkalmazást, olvassa el [a felmérések alkalmazást futtató](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

A következő cikkek a sorozat listája:

- [Az Identitáskezelés multitenant alkalmazások – bevezetés](guidance-multitenant-identity-intro.md)
- [Dejójáték Kft felmérések alkalmazással kapcsolatos](guidance-multitenant-identity-tailspin.md)
- [Azure Active Directory hitelesítéssel és OpenID csatlakoztatása](guidance-multitenant-identity-authenticate.md)
- [Állítást-alapú identitások használata](guidance-multitenant-identity-claims.md)
- [Előfizetési és bevezetési bérlői](guidance-multitenant-identity-signup.md)
- [Alkalmazás-szerepkörök](guidance-multitenant-identity-app-roles.md)
- [Szerepkör- és erőforrás-alapú hitelesítés](guidance-multitenant-identity-authorize.md)
- [Kódmentes webes API biztonságossá tétele](guidance-multitenant-identity-web-api.md)
- [Gyorsítótárazási oauth2 hitelesítési mód hozzáférési jogkivonat](guidance-multitenant-identity-token-cache.md)
- [Az AD FS ügyfél Azure multitenant alkalmazásait összevonása](guidance-multitenant-identity-adfs.md)
- [Hozzáférési jogkivonat lekérése az Azure Active Directory ügyfél állítás használatával](guidance-multitenant-identity-client-assertion.md)
- [Kulcs tárolóra használatával alkalmazás titkos kulcsok védelme](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
