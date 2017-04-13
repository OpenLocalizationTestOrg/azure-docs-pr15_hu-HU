<properties
    pageTitle="Kiemelt hozzáférés biztosítása érdekében az Azure Active Directory |} Microsoft Azure"
    description="Ez a témakör ismerteti az eljárások Azure, az Azure Active Directory és a Microsoft Online Services jogosultsággal rendelkező hozzáférés biztosítása érdekében."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Azure AD a Yammerhez hozzáférés biztosítása érdekében

Kiemelt hozzáférés biztonságossá tétele kritikus első lépés a modern szervezet üzleti eszközök védelme érdekében. Kiemelt partnerek azok felügyelete és informatikai rendszerek kezelése. A támadók számítógépes Megcélozhat ezekben a fiókokban eléréséhez az adott szervezet adatok és a rendszer. Annak érdekében, hogy a biztonságos hozzáférés jogosultsággal rendelkező, a fiókok és rendszerek, hogy egy felhasználóhoz, a kártékony kockázatból kell elkülöníteni.

Kiemelt férhet cloud services révén a több felhasználó indítása Ez az Office 365 globális rendszergazdái, az Azure előfizetés rendszergazdák és a VMs vagy szoftver alkalmazások rendszergazdai hozzáféréssel rendelkező felhasználók is előfordulhatnak.

A Microsoft javasolja, kövesse az ebben az útmutató a [Yammerhez hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx).

Az Azure Active Directory használó Azure, az Office 365-be, vagy más Microsoft-szolgáltatások és alkalmazások hozzáférésének kezelése ügyfeleknek ezek az elvek e felhasználói fiókok kezelését, és az Active Directory vagy az Azure Active Directory hitelesített vonatkoznak. A következő szakaszokban további információt a Azure AD-szolgáltatások támogatása a Yammerhez hozzáférés biztosítása érdekében.

## <a name="multi-factor-authentication"></a>Többtényezős hitelesítés

Ha növelni szeretné a biztonsági rendszergazda hitelesítés, szüksége van többtényezős hitelesítés (MFA) jogosultságokkal megadása előtt kell. MFA ellenőrzésére ki, hogy Ön a módszer használatához nem egyszerűen csak felhasználónevet és jelszót. Felhasználói bejelentkezések, és a tranzakciók biztonsági második réteg biztosít.

Azure többtényezős hitelesítés segít védelmét hozzáférés az adatokhoz és alkalmazások felhasználói igény szerint egy egyszerű bejelentkezési folyamat az értekezlet közben. Egy egyszerű ellenőrzési beállítások, például telefonhívások, a szöveges üzenetek, a mobilalkalmazásban értesítést, vagy a ellenőrzőkódot és a 3 fél elfogadható tokenek cellatartomány útján erős hitelesítéshez kézbesíti.

Hogyan működik a Azure többtényezős hitelesítés áttekintése lásd az alábbi videóban.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

További részletekért olvassa el [az Office 365 és az Azure MFA MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Idő kötött jogosultságok

Egyes szervezetek tapasztalhatja, hogy rendelkezik-e túl sok felhasználó kiemelt jogosultságokkal rendelkező szerepkörök. A felhasználó előfordulhat, hogy felvette a szerepkörhöz egy adott tevékenységhez, például feliratkozhat a szolgáltatásra, de nem testreszabásokat gyakran használt ezeket az engedélyeket.

Alsó jogosultságokkal kapta időpontját és használatuk be a láthatóság fokozása, korlátozza a felhasználóknak, hogy csak a jogosultságaik csupán az idő (igény szerinti), ha egy feladat elvégzéséhez szükségük. Azure Active Directory és a Microsoft Online Services, az [Azure Active Directory Yammerhez Identitáskezelés (személyes Információkezelő)](http://aka.ms/AzurePIM)is használhatja.


![Személyes Információkezelő irányítópult][2]


## <a name="attack-detection"></a>Támadások észlelése

[Azure Active Directory identitás védelem](../active-directory-identityprotection.md) kockázat események és a szervezet identitások érintő potenciális biztonsági összevont betekintést biztosít. A kockázat események alapján, a identitás védelem minden olyan felhasználóhoz, úgy automatikusan védő a a szervezet kockázat-alapú házirendek beállítása egy felhasználó kockázat szint számítja ki. Ezek a házirendek más Azure Active Directory és EMS, által biztosított vezérlőelemek feltételes elérése a felhasználónak letiltja az automatikusan, vagy javaslatot, amely tartalmazza a jelszó alaphelyzetbe állítása és többtényezős hitelesítés kényszerítési.

![Azure Active Directory-identitás-védelem][3]

## <a name="conditional-access"></a>Feltételes hozzáférés

Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi, kiválaszthatja, hogy amikor hitelesítése a felhasználó, mielőtt az alkalmazás hozzáférést a megadott feltételekkel. Ha ezen feltételek teljesülése esetén a felhasználó a hitelesített és az alkalmazás férhet hozzá.


![A MFA feltételes access szabályok beállítása][4]


## <a name="related-articles"></a>Kapcsolódó cikkek

- [Azure többtényezős hitelesítés](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) engedélyezése
- [Azure Active Directory jogosultsággal rendelkező Identitáskezelés](../active-directory-privileged-identity-management-configure.md) engedélyezése
- [Azure Active Directory-identitás](../active-directory-identityprotection.md) -védelem
- [Vezérlők feltételes hozzáférés](../active-directory-conditional-access.md) engedélyezése


"További tájékoztatást a teljes biztonsági tudnivalókat foglaltuk össze felépíteni a ügyfél felelősségi köre és ütemterv" szakaszában olvashat a [Microsoft Cloud biztonsági vállalati építészek a](http://aka.ms/securecustomer) dokumentumot. További információt a Microsoft szolgáltatásokat az alábbi témakörök egyikét segítsék érdekes lépjen kapcsolatba a Microsoft képviselőjével a követelmények, vagy keresse fel a [Cybersecurity megoldások lapon](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
