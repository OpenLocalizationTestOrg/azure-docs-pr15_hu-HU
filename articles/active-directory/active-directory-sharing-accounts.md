<properties
    pageTitle="Megosztott partnerek használatával az Azure Active Directory |}  Microsoft Azure"
    description="Ismerteti, hogyan Azure Active Directory lehetővé teszi a szervezeteknek, amelyekkel biztonságosan megoszthatja a fiókokat helyszíni alkalmazások és a fogyasztói felhőszolgáltatások."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Megosztott partnerek az Azure Active Directory

## <a name="overview"></a>– Áttekintés
Előfordul, hogy szervezetek kell több személynek egyetlen felhasználónevet és jelszót használja. Ez általában két esetekben történik:

- Egy egyedi bejelentkezés, és a jelszót az egyes felhasználókhoz igénylő alkalmazások elérésekor e helyszíni-alkalmazások és a fogyasztói cloud services (például vállalati közösségi hálózati fiókok).
- Több felhasználó környezetekben létrehozásakor. Előfordulhat, hogy egy egyetlen, helyi fiók magasabb szintű jogosultságokkal és alapvető (például a helyi "globális rendszergazdai" fiók az Office 365) vagy a legfelső szintű Salesforce-fiók beállítása, felügyelete és helyreállítási tevékenységek használható.

Régebben ezekben a fiókokban volna megosztott terjesztése a hitelesítő adatok (felhasználónév és jelszó) a megfelelő személyekre korlátozhatja, illetve tárolja őket egy olyan megosztott helyre, ahol több megbízható ügynökök elérhetők legyenek.

A hagyományos megosztási modell több hátrányai foglalja magában:

- Hozzáférés biztosítása új alkalmazások szükséges hitelesítő adatait, és mindenki hozzáférést igénylő terjesztése.
- Minden megosztott alkalmazás szükség lehet a saját egyedi beállítása a felhasználók ne feledje, hogy a hitelesítő adatokat, ha több feltételcsoportnak megkövetelése megosztott hitelesítő adatokat. Ha a felhasználók ne feledje, hogy hány hitelesítő adatokat, a a kockázat, hogy azok fog használhatja kockázatos eljárások megnöveli. (például jelszavak lefelé írása).
- Nem tudja megállapítani, kinek van hozzáférése az alkalmazás.
- Nem tudja megállapítani, kinek van *érhető el* egy alkalmazást.
- Ha szeretné eltávolítani az access, az alkalmazás, frissítse a hitelesítő adatait, és újra mindenkinek, hogy az alkalmazás hozzáférést igénylő terjesztése van.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory fiók megosztása

Azure AD egy új megközelítése megosztott fiókokkal, amely megszünteti a következő hátrányai biztosít.

Az Azure Active Directory-rendszergazda állítja be, hogy mely alkalmazások felhasználó hozzáférhet a Access panelen, és válassza a egyszeri bejelentkezéses legjobb típusú alkalmas-e az alkalmazást. Egy adott típusú, a *jelszó-alapú egyszeri bejelentkezés a*, lehetővé teszi, hogy a használni kívánt egy "ügynök" típusú, az adott alkalmazás bejelentkezési folyamat során Azure AD.

Felhasználók egyszer a szervezeti fiókkal bejelentkezni. Ez a ugyanazzal a fiókkal, amelyeket rendszeresen használnak az Asztalukat és az e-mailek eléréséhez. Ezeket Fedezze fel, és csak a vannak rendelve alkalmazások eléréséhez. A megosztott partnerek e-alkalmazások listájából lehetnek a megosztott hitelesítő adatok tetszőleges számú. A végfelhasználói jegyezze fel az egyes fiókok, lehet, hogy használja, vagy ne feledje, hogy nem kell.

Megosztott partnerek nem csak növelése betekintése és használhatóság javítása, azok is a biztonság fokozása. A hitelesítő adatok jogosultsággal rendelkező felhasználók nem jelenik meg a megosztott jelszót, de inkább első jogosultsággal a jelszó-orchestrated hitelesítési folyamat részeként. További néhány jelszó SSO-alkalmazásokkal, lehetősége van, hogy az Azure Active Directory rendszeres átváltási (frissítés) a jelszót a fiók biztonsági növekvő nagy, összetett jelszavak használatával. A rendszergazda egyszerűen megadása vagy visszavonni a hozzáférést az alkalmazás és is, hogy kinek van hozzáférése a fiókot, és ki nyitotta meg az elmúlt.

Azure Active Directory támogatja a megosztott partnerek az bármely vállalati mobilitás csomagja (EMS), Premium vagy végig a diagramtípusokat egyetlen jelszót egyszerű licenccel rendelkező felhasználók alkalmazások jelentkezzen be. Megoszthatja az összes alkalmazás gyűjteményben lévő előre integrált alkalmazások ezer fiókok, és a saját jelszó hitelesítése alkalmazás [SSO-alkalmazások egyéni](active-directory-sso-integrate-saas-apps.md)adhat hozzá.

Azure Active Directory, amelyekben a fiók megosztási funkciók:

- [Jelszó egyszeri bejelentkezés](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Jelszó egyszeri bejelentkezéses agent
- [Csoport-hozzárendelés](active-directory-accessmanagement-self-service-group-management.md)
- Egyéni jelszó-alkalmazások
- [Alkalmazás használatát irányítópult/jelentések](active-directory-passwords-get-insights.md)
- Végfelhasználói access portálokról
- [Alkalmazás-proxy](active-directory-application-proxy-get-started.md)
- [Az Active Directory piactér](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Fiók megosztása
Azure Active Directory segítségével kell fiók megosztása:

- Alkalmazás [alkalmazásgyűjteményébe](https://azure.microsoft.com/marketplace/active-directory/) vagy [egyéni alkalmazás](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) hozzáadása
- Állítson be a jelszó egyszeri bejelentkezés (SSO)
- Használja a [csoport alapú hozzárendelés](active-directory-accessmanagement-group-saasapps.md) , és jelölje ki a vezérlőt, amellyel adjon meg egy megosztott hitelesítő adatok
- Nem kötelező: az egyes alkalmazások, például a Facebookon, a Twitteren és a LinkedIn, engedélyezheti az [Azure AD automatikus jelszó Filmtekercs fölé](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx) beállítás

Is teheti a megosztott fiók biztosítani a többtényezős hitelesítés (MFA) (További információ [az Azure Active Directory webhelycsoportgazdákra alkalmazások](../multi-factor-authentication/multi-factor-authentication-get-started.md)) és a delegálása kezeléséhez, kinek van hozzáférése az [Azure Active Directory önkiszolgáló](active-directory-accessmanagement-self-service-group-management.md) csoportok kezelése alkalmazást.

## <a name="related-articles"></a>Kapcsolódó cikkek

- [Az alkalmazások kezelése az Azure Active Directory cikk indexben](active-directory-apps-index.md)
- [Feltételes hozzáféréssel rendelkező alkalmazások védelme](active-directory-conditional-access.md)
- [Önkiszolgáló csoport kezelése/SSAA](active-directory-accessmanagement-self-service-group-management.md)
