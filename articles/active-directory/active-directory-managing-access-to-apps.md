<properties
  pageTitle="Access-alkalmazás segítségével az Azure Active Directory kezelése |}  Microsoft Azure"
  description="Ismerteti, hogyan Azure Active Directory lehetővé teszi, hogy adja meg az alkalmazásokat, amelyhez a minden felhasználónak van hozzáférési szervezetek."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Alkalmazások való hozzáférés kezelése

Folyamatban lévő access-kezelés, használatát kiértékelési és jelentéskészítési továbbra is a kérdés után a szervezet identitás rendszerbe integrált alkalmazás. Sok esetben rendszergazdák vagy az IT-részleghez végre kell hajtania az alkalmazások való hozzáférés kezelése a folyamatban lévő aktív szerepet. Időnként előfordulhat a hozzárendelés általános vagy megosztott csapatnak történik. Gyakran, a hozzárendelés döntés delegálni kell az üzleti döntés maker előtt informatikai megkönnyíti a jóváhagyást célja a hozzárendelés.  Más szervezetek egy meglévő automatikus identitás- és hozzáférési rendszer, például a szerepköralapú hozzáférés szerepalapú vagy attribútum-alapú hozzáférés szabályozása (ABAC) integrációja fektetni. A integráció és a szabály fejlesztési általában speciális és drága. Figyelés, vagy akár adatkezelési megközelítésre jelentési a saját külön költséges és összetett befektetés.

## <a name="how-does-azure-active-directory-help"></a>Hogyan segíti az Azure Active Directory?

 Azure Active Directory támogatja teljes körű hozzáférés-kezelés beállított alkalmazásokat, egyszerűen elérése a megfelelő hozzáférési házirendek automatikus, attribútum hozzárendelés (ABAC vagy RBAC esetek) kezdve a meghatalmazás és kezelése a rendszergazdai szervezetek engedélyezése. Az Azure Active Directory egyszerűen érhet el összetett házirendek kombinálásával több management modellek egyetlen alkalmazáshoz, és újra programmal is kezelési szabályok alkalmazások ugyanazt a célközönségek között.

 - [Hozzáadás új vagy meglévő alkalmazások](active-directory-sso-integrate-saas-apps.md)


 Azure AD-alkalmazás hozzárendelés kétféleképpen elsődleges hozzárendelés koncentrál:

- **Adott hozzárendelés** Az informatikai rendszergazda címtár globális rendszergazdai engedélyekkel rendelkező választhat egyéni felhasználói fiókok és hozzáférést őket az alkalmazás.
- **A csoport alapú hozzárendelés (csak az Azure AD kifizetett)** Az informatikai rendszergazda címtár globális rendszergazdai engedélyekkel rendelkező az alkalmazás oszthatnak ki egy csoportot. Adott felhasználók hozzáférésének e a csoportnak a tagjai megpróbálnak hozzáférni az alkalmazás időben határozza meg. Más szóval rendszergazda hatékony hozhat létre egy hozzárendelés szabály arról, hogy "bármely aktuális tagja a kijelölt csoport fér hozzá az alkalmazás". A hozzárendelés beállítás használatával a rendszergazdák bármilyen Azure Active Directory csoport adatkezelési beállítások, többek között az [attribútum alapuló dinamikus csoportok](active-directory-accessmanagement-manage-groups.md), a külső rendszerhez csoportok (például a helyszíni Active Directory vagy a kalk.munkanap) vagy a rendszergazda által kezelt vagy automatikus-üzemeltetési felügyelt csoportok élvezheti. Egyetlen csoport egyszerűen rendelhetők több alkalmazásokat, hogy annak biztosítása, hogy a hozzárendelés affinitás alkalmazások hozzárendelés szabályokat, megoszthatja a teljes kezelésének bonyolultsága csökkentése. Felhívjuk a figyelmét arra, hogy tagságot csoport alapú hozzárendelés alkalmazások jelenleg nem támogatottak, beágyazott csoport.

Használja a következő két hozzárendelés üzemmódok, rendszergazdák érhet el bármely célszerű hozzárendelés management megközelítés.

Azure Active Directory, a használatát és a hozzárendelés jelentéskészítés teljes mértékben integrálva, egyszerűen jelentheti a hozzárendelés állapotát, a hozzárendelés hibák és a páros használatát rendszergazdák engedélyezése.

## <a name="complex-application-assignment-with-azure-ad"></a>Összetett alkalmazás hozzárendelés az Azure Active Directory

Fontolja meg egy alkalmazást, például a Salesforce. Sok szervezetek Salesforce elsősorban a marketing- és értékesítési szervezetek. Gyakran a marketingcsoport tagjai van kiemelt jogosultságokkal rendelkezik Salesforce, hogy az access az értékesítési csoport tagjai korlátozott hozzáférés közben. Sok esetben az infomunkások széles sokaság korlátozta való hozzáférést az alkalmazást. Szabályok alóli kivételek ügyekben bonyolult lesz. A marketing- vagy Értékesítési vezetés a csoportoknak a felhasználó hozzáférést, és módosíthatja a szerepkörök függetlenül általános szabályok megállapítása legtöbbször.

Azure Active Directory, a alkalmazásokban, például a Salesforce előre beállított egyszeri bejelentkezés (SSO) és az automatikus kiépítési lehet. Ha az alkalmazás be van állítva, a rendszergazda létrehozása és hozzárendelése a megfelelő csoportok, a egyszeri művelet vehet igénybe. Ebben a példában a rendszergazdának az alábbi hozzárendeléseket támadó:

- [Dinamikus csoportok](active-directory-accessmanagement-manage-groups.md) összes tagja a marketing- és értékesítési csapatok, például részlege vagy szerepkör attribútumok használatával automatikusan ábrázolásához határozható meg:

    - Marketingtevékenység csoport összes tagja a "marketing" szerepkör a Salesforce rendelt volna

    - Csoportok volna hozzárendelve a "értékesítés" szerepkör a Salesforce értékesítési csoport összes tagja. A további pontosítás több csoportok regionális értékesítési csoportokhoz különböző Salesforce szerepkörökhöz rendelt megjelenítő használhatja.

- Ahhoz, hogy a kivétel mechanizmusa, önkiszolgáló csoport lehetett létrehozni, minden szerepkörhöz. Ha például a "Salesforce marketing kivétel" csoport önkiszolgáló csoportként hozható létre. A csoport rendelhet a Salesforce-marketingtevékenység szerepkört, és vezetés marketingcsoport tulajdonosok tehetők. Vezetés marketingcsoport sikerült hozzáadása vagy távolítsa el a felhasználók, egy illesztés házirend beállítása vagy akár jóváhagyása vagy tagok megtagadja az egyes felhasználók kérések csatlakozni szeretne. Ez támogatott információk dolgozó megfelelő eszköz, amely az esetében nincs szükség speciális oktatás tulajdonosok és tagok keresztül.

Ebben az esetben az összes hozzárendelt felhasználók szeretné automatikusan kiépítése Salesforce, hogy azok szerepkör-hozzárendelés Salesforce frissíteni kell különböző csoportokhoz hozzáadásukkor. Felhasználók Fedezze fel, és a Microsoft alkalmazás hozzáférést panelen, az Office webes ügyfelek, vagy akár navigálással a szervezeti Salesforce bejelentkezési lapját férhetnek hozzá a Salesforce lenne. A rendszergazdák volna tudja egyszerűen megtekintheti az Azure Active Directory jelentési használatát és a hozzárendelés állapotát.

A rendszergazdák az [Azure Active Directory feltételes access](active-directory-conditional-access.md) szerepköreinek az access-házirendek beállítása is alkalmazhat. Ezek a házirendek is elhelyezhet, hogy az access engedélyezve van a vállalati környezetben és még többtényezős hitelesítés vagy eszköz követelmények egyes esetekben az access eléréséhez kívüli.

## <a name="how-can-i-get-started"></a>Hogyan tudom elkezdeni használni?

Először is, ha még nem keresztül Azure Active Directory és az informatikai rendszergazda:

 - [Próbálja ki!](https://azure.microsoft.com/trial/get-started-active-directory/) -a 30 napig ingyenes próbaverziója ma regisztrálhat, és az első felhő megoldást üzembe helyez a használja ezt a hivatkozást az 5 perc múlva

Azure Active Directory, amelyekben a fiók megosztási funkciók:

- [Csoport-hozzárendelés](active-directory-accessmanagement-self-service-group-management.md)
- Azure AD az alkalmazások hozzáadásával
- Hozzárendelés – első lépések
- Alkalmazás-hozzárendelés – gyakori kérdések
- [Alkalmazás használatát irányítópult/jelentések](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Hol találok további információkat?

- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
- [Feltételes hozzáféréssel rendelkező alkalmazások védelme](active-directory-conditional-access.md)
- [Önkiszolgáló csoport kezelése/SSAA](active-directory-accessmanagement-self-service-group-management.md)
