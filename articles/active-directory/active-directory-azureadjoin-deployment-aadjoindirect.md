<properties
    pageTitle="Esetek használatát és az Azure Active Directory Bekapcsolódás telepítése során megfontolandó kérdések |} Microsoft Azure"
    description="Ebből a cikkből megtudhatja, hogy a rendszergazdák hozhatnak Azure Active Directory Bekapcsolódás a végfelhasználók (alkalmazottak, diákok számára, más felhasználók). Különböző a való életben alkalmazási eseteit, az Azure Active Directory Bekapcsolódás is ismerteti."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< ms.service= "active Directoryval" ms.workload="identity címkék" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Esetek használatát és az Azure Active Directory Bekapcsolódás telepítése során megfontolandó kérdések

## <a name="usage-scenarios-for-azure-ad-join"></a>Az Azure Active Directory Bekapcsolódás használatát esetek
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Alkalmazási helyzetek 1: Vállalkozások nagymértékben a felhőben

Azure Active Directory Bekapcsolódás (Azure Active Directory csatlakozni) alkalmazhatók, ha jelenleg működtetéséhez és felhasználók kezelése a vállalati a felhőben vagy a felhőbe hamarosan áthelyezni. Azure Active Directory bejelentkezni a Windows 10-ben létrehozott fiók is használhatja. [Első futtatása élmény (FRX)](active-directory-azureadjoin-user-frx.md)folyamatát, vagy összefűzésével Azure Active Directory, [a beállítások](active-directory-azureadjoin-user-upgrade.md)menüben, a felhasználók is csatlakozni tudnak a gép az Azure Active Directory.  A felhasználók is elérhesse egyszeri bejelentkezés (SSO) való hozzáférés felhő erőforrások, mint az Office 365-be, a böngészőben vagy az Office-alkalmazásokban.

### <a name="scenario-2-educational-institutions"></a>2 alkalmazási helyzetek: Oktatási intézmények

Oktatási intézmények általában dokumentumtípusokat két felhasználó: oktatók és diákok számára. Oktatók tagok a szervezet tagjainak hosszú távú számítanak. A helyszíni fiókok létrehozása a számukra célszerű. De a diákok, a szervezet tagjainak shorter-term és fiókjukat is kezelhetők az Azure Active Directory. Ez azt jelenti, hogy a felhőben tárolt helyszíni éppen helyett a címtár-skála kell eltolni. Azt is jelenti, hogy a Windows Azure AD-fiókjukat jelentkezzen be, és az Office-alkalmazásokban hozzáférhet az Office 365-ös forrásokat lesz-e a diákok számára.

### <a name="scenario-3-retail-businesses"></a>Alkalmazási helyzetek 3: Kiskereskedelmi vállalkozások

Kiskereskedelmi vállalkozások szezonális munkatársaival és a hosszú távú alkalmazottak van. Általában a helyszíni fiókok létrehozása és használata a tartományhoz gépek hosszú távú teljes munkaidős alkalmazottak. De szezonális dolgozók a szervezet tagjainak shorter-term, és célszerű kezelése a fiókjuk, ahol a felhasználói licencek könnyebben áthelyezhető körül. A felhasználói fiókjuk létrehozásakor a felhőben az Office 365-licenceket a felhasználóknak beolvasása a előnyökkel jár a bejelentkezés Windows és az Office alkalmazások az Azure Active Directory-fiókkal, miközben a licenccel rendelkező rugalmasabb kezelése, miután elhagyják

### <a name="scenario-4-additional-scenarios"></a>: 4 további forgatókönyvet

Korábban tárgyalt előnyökkel jár, valamint összekapcsolhatók bejelentkezésen, hogy a felhasználók a eszközök csatlakozzon az Azure Active Directory miatt egy egyszerűsített csatlakozó élmény, hatékony mobileszköz-kezelés, automatikus mobileszköz-kezelés regisztrációs és az egyszeri bejelentkezés az Azure Active Directory és a helyszíni erőforrásokat.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Az Azure Active Directory Bekapcsolódás telepítése során megfontolandó kérdések

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Engedélyezése a felhasználóknak, hogy csatlakozzanak a vállalat által birtokolt eszköz közvetlenül az Azure Active Directory


Nagyvállalatoknak partner vállalatok és szervezetek a felhőben fiókok számára. Ezek a partnerek majd könnyen hozzáférhető vállalati alkalmazások és az erőforrások egyszeri bejelentkezést. Ebben az esetben csak a felhasználók, akik elsősorban a felhőben, például az Office 365- vagy szoftver alkalmazások Azure AD-hitelesítés támaszkodó forrásokat alkalmazható.

### <a name="prerequisites"></a>Előfeltételek
**A vállalati szintű (rendszergazda)**

*   Azure Active Directory-Azure előfizetés  

**A felhasználói szintjén**

*   A Windows 10 (Professional és a nagyvállalati verzió)

### <a name="administrator-tasks"></a>Rendszergazdai feladatok
* [Regisztráció eszköz beállítása](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Felhasználó feladatai
* [Az Azure Active Directory, a telepítés során egy új Windows 10-es eszköz beállítása](active-directory-azureadjoin-user-frx.md)
* [Kattintson a beállítások menü Azure AD a Windows 10-es eszköz beállítása](active-directory-azureadjoin-user-upgrade.md)
* [Bekapcsolódás a Windows 10-es eszköz a szervezet számára](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>BYOD engedélyezése a Windows 10 a szervezetben
Beállíthatja, hogy a felhasználók és az alkalmazottak be személyes Windows eszközök (BYOD) vállalati alkalmazások eléréséhez és a források segítségével. A felhasználók az Azure Active Directory-fiókok (munkahelyi vagy iskolai fiókokat) hozzáadhatja a személyes Windows-eszközön biztonságos és megfelelő módon erőforrások eléréséhez.

### <a name="prerequisites"></a>Előfeltételek
**A vállalati szintű (rendszergazda)**

*   Azure Active Directory-előfizetés

**A felhasználói szintjén**

*   A Windows 10 (Professional és a nagyvállalati verzió)


### <a name="administrator-tasks"></a>Rendszergazdai feladatok

* [Regisztráció eszköz beállítása](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Felhasználó feladatai
* [Bekapcsolódás a Windows 10-es eszköz a szervezet számára](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Jelszavak – Microsoft Passport nélkül hitelesítő identitások](active-directory-azureadjoin-passport.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
