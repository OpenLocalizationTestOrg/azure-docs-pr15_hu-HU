<properties
    pageTitle="Azure Active Directory és az alkalmazások: a fejlesztők irányadó |} Microsoft Azure"
    description="Informatikai szakembereknek írt, ez a cikk nyújt útmutatást Azure alkalmazások integrálása az Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure Active Directory és az alkalmazások: a vállalati verziós alkalmazások fejlesztése

Ez az útmutató a vállalati verzió (üzleti) alkalmazások az Azure Active Directory (AD) fejlesztésével áttekintést nyújt. A célközönség az Active Directory és az Office 365 globális rendszergazdái számára.

## <a name="overview"></a>– Áttekintés

Azure AD az integrált alkalmazások létrehozásába elemre koppintva felhasználók a szervezet egyszeri bejelentkezés az Office 365-tel. Az alkalmazás tárolásának vezérlésére a hitelesítési házirendet, az alkalmazás az Azure Active Directory biztosít lehetőséget. További tudnivalók a feltételes az access és a többtényezős hitelesítés (MFA) alkalmazások védelmét lásd: [konfigurálása hozzáférési szabályokat](active-directory-conditional-access-azuread-connected-apps.md).

Regisztráljon az alkalmazást, hogy az Azure Active Directory. Az alkalmazás regisztrálása, az azt jelenti, hogy a fejlesztők segítségével Azure Active Directory hitelesítést végezni a felhasználók és felhasználói erőforrások, például az e-mailek, a naptár és a dokumentumok hozzáférést kérni.

A címtárban (nem vendégek) bármely tagja rögzítheti egy alkalmazást, más néven *alkalmazás objektum létrehozását*.

Alkalmazás rögzítése lehetővé teszi, hogy minden felhasználó tegye a következőket:

- Az Azure Active Directory felismer alkalmazás identitás beszerzése
- Egy vagy több titkos kulcsok/billentyűket, amelyek az alkalmazás használatával hitelesítse magát az Active Directory beszerzése
- Márkajegyeket helyezhet el az alkalmazás az Azure portálon, kijelölt egy egyéni név, embléma stb.
- Azure Active Directory engedélyezési lehetőségek vonatkoznak az alkalmazásra, beleértve a:
  - Szerepköralapú hozzáférés-szerepalapú
  - Azure Active Directory oAuth engedélyezési kiszolgálójával (biztonságos egy API-t, az alkalmazás által elérhetővé tett)

- Szükséges engedélyekkel függvény az alkalmazáshoz szükséges deklarálhatnak megfelelően működjön, például:-Alkalmazásengedélyek (csak a globális rendszergazdák). Példa: egy másik Azure Active Directory alkalmazás vagy szerepkör tagság az Azure erőforrás, erőforráscsoport, vagy -előfizetésre - meghatalmazott engedélyeit (felhasználói) viszonyított szerepkör tagsága. Példa: Azure AD, bejelentkezés, és olvasási profil


> [AZURE.NOTE]Alapértelmezés szerint bármely tagja regisztrálhatja az alkalmazás. Megtudhatja, hogy miként szeretné korlátozni a Regisztrálás az adott tag az alkalmazások engedélyeit, olvassa el a [hogyan vehetők fel az alkalmazást az Azure Active Directory](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance)című témakört.

A következő, a globális rendszergazdának kell tenni a fejlesztők, hogy készen áll a gyártás alkalmazásuk:

- Állítsa be a hozzáférési szabályok (hozzáférési házirend/MFA)
- Állítsa be az alkalmazás csak a felhasználó hozzárendelés és adhatnak a felhasználóknak
- Az alapértelmezett kezelőfelület hozzájárulása mellőzése

## <a name="configure-access-rules"></a>Hozzáférési szabályok beállítása

Állítsa be az alkalmazás hozzáférési szabályokat, a szoftver-alkalmazás között. Például MFA megkövetelése, vagy csak engedélyezése a felhasználók való hozzáférés megbízható hálózatokon. Az Ez az adatait a dokumentumban [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md)érhetők el.

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Állítsa be az alkalmazás csak a felhasználó hozzárendelés és adhatnak a felhasználóknak

Alapértelmezés szerint felhasználók hozzá tudnak férni alkalmazások hozzárendeli nélkül. Ha az alkalmazás közzététele szerepkörök, vagy ha azt szeretné, hogy az alkalmazás jelenjen meg a felhasználó hozzáférést panel, felhasználói kiosztása kell szükség.

[Felhasználói kiosztása megkövetelése](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Ha az Azure Active Directory Premium vagy vállalati mobilitás csomagja (EMS) előfizetői, javasoljuk csoportok használatával. Csoportok hozzárendelése az alkalmazás lehetővé teszi annak a csoportnak a tulajdonosa folyamatos hozzáféréshez kezelése meghatalmazás. Hozza létre a csoportot, vagy kérje meg a felelős fél szeretné, hogy a csoport kezelése funkcióval a csoport létrehozása.

[Az alkalmazás a felhasználók hozzárendelése](active-directory-applications-guiding-developers-assigning-users.md)  
[Az alkalmazás csoportok hozzárendelése](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Felhasználó hozzájárulása mellőzése

Alapértelmezés szerint minden felhasználóhoz kerül, keresztül jelentkezzen be a felhasználó hozzájárul ahhoz funkcióit. Lehet, hogy a felhasználók, akik nem ismerik ilyen döntések disconcerting a felhasználó hozzájárul ahhoz változat, helyezni a felhasználóknak, az alkalmazás engedélyeket szeretne adni.

Megbízható alkalmazások a felhasználói felület egyszerűsíthető hozzájárul ahhoz, hogy az alkalmazás nevében a szervezet által.

Többet szeretne tudni a felhasználó hozzájárulása és a felhasználó hozzájárul ahhoz felület Azure-ban olvassa el az [Alkalmazások integrálása az Azure Active Directory](active-directory-integrating-applications.md)című témakört.

##<a name="related-articles"></a>Kapcsolódó cikkek

- [A helyszíni alkalmazásokat az Azure Active Directory szolgáltatásalkalmazás-Proxy biztonságos távoli hozzáférés engedélyezése](active-directory-application-proxy-get-started.md)
- [Azure feltételes Access előzetes szoftver alkalmazások](active-directory-conditional-access-azuread-connected-apps.md)
- [Az Azure Active Directory alkalmazások való hozzáférés kezelése](active-directory-managing-access-to-apps.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
