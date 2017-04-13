<properties
    pageTitle="Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése |} Microsoft Azure"
    description="Hogyan használhat a Windows 10-eszközökön Azure Active Directory csatlakozzon Azure Active Directory regisztrálva részletes áttekintést nyújt."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése

## <a name="what-is-azure-active-directory-join"></a>Mi az Azure Active Directory csatlakozni?
Azure Active Directory Bekapcsolódás (Azure Active Directory csatlakozni), akkor a vállalat által birtokolt eszköz regisztrál az Azure Active Directory ahhoz, hogy az eszköz a központi felügyeleti funkciókat. Lehetővé teszi a felhasználóknak, alkalmazottak és diákok csatlakozni a vállalati felhő Azure Active Directory – például. Ezzel az egyszerűsített Windows telepítések és bármilyen Windows eszközről a szervezeti-alkalmazások és az erőforrások hozzáférést, vállalati tulajdonú mind személyazonosításra tulajdonú (BYOD).

Azure Active Directory illesztés nagyvállalatoknak, amelyek a felhőben első/cloud-csak – általában kis - és középvállalatok, amely nem rendelkezik a helyszíni Windows Server Active Directory infrastruktúrát íródott. Nagy szervezetekben képes, hajtsa végre a tartomány hagyományos illesztés (mobil eszközök, például), vagy a felhasználók, akik célozza kell tudni férnie az Office 365-ös vagy más Azure Active Directory-szoftver alkalmazások eszközön, hogy az említett, Azure Active Directory illesztés is, valamint azt, hogy fogja használni.

Bár a hagyományos tartomány join is kínál a legjobb helyszíni tapasztal, amelyek képesek tartomány csatlakozással eszközön, Azure Active Directory illesztés alkalmas eszközök, amelyek nem tudja a tartományhoz történő csatlakozást. Azure Active Directory illesztés alkalmas is kezelése a felhasználók a felhőben. Igen, nem a mobileszköz-kezelési lehetőségek segítségével hagyományos tartomány kezelő eszközök, például a csoportházirend- és a System Center Configuration Manager (SCCM) használatával.

![Vállalati eszközök és a személyes eszközök a helyszíni Active Directory és Azure Active Directory – áttekintés](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Miért kell nagyvállalatoknak elfogadják Azure Active Directory csatlakozni?

* **A vállalkozások, amelyek a felhőben nagymértékben**: Ha át lett helyezve, vagy a modell, amelyben a helyszíni helyigénye csökkentheti, és a felhőben működik szeretné áthelyezni, Azure Active Directory Bekapcsolódás kihasználhatók, akkor. Azure Active Directory-fiókok esetleg létrehozott manuálisan vagy a helyszíni Active Directory szinkronizáló keresztül. Mindkét esetben fiókja van, az Azure Active Directory, és jelentkezzen be a Windows 10-ben használható. A felhasználók is csatlakozni tudnak a számítógépükre az Azure Active Directory vagy a kimenő kész felület (OOBE) vagy a beállítások menü keresztül. Csatlakozás, miután felhasználók élvezzenek egyszeri bejelentkezés (SSO) való hozzáférés felhő erőforrások, mint az Office 365-be, a böngészőben vagy az Office-alkalmazásokban.

* **Oktatási intézmények**: a azt szóló értesítésekre gyakran esetek egyike, oktatási intézmények kétféle felhasználó rendelkezik-e: oktatók és diákok számára. Oktatók tagok, célszerű a helyszíni fiókok létrehozása az őket a szervezet tagjainak hosszú távú számít. De diákok a szervezet tagjainak shorter-term, és így az Azure Active Directory kezelhető. Ez azt jelenti, hogy a felhőben tárolt helyszíni helyett a címtár-skála kell eltolni. Azt is jelenti, hogy a diákok jelentkezzen be Windows Azure AD-fiókjukat, és hozzáférhet az Office 365-forrásokat a böngészőben vagy az Office-alkalmazásokban.

* **Kiskereskedelmi vállalkozások**: az ügyfelekkel kapcsolatos leggyakoribb egy másik példa, hogy könnyebben kezelheti szezonális dolgozók.  Hosszú távú, teljes munkaidős alkalmazottak fiókokat ismét általában létrehozott, a helyszíni fiókok tartományhoz gépeken. De szezonális dolgozók tartoznak shorter-term a szervezeti, így célszerű kezelése, ahol a felhasználói licencek könnyebben áthelyezhető körül őket. A felhasználói fiókok létrehozása a felhőben az Office 365-licencek lehetővé teszi, hogy a felhasználók számára beolvasása a Windows és az Office-alkalmazások Azure AD-fiókkal történő bejelentkezés előnyeit. Eközben a licenccel rendelkező rugalmasabb karbantartása, elhagyják után.
* **Más vállalatok**: annak ellenére, hogy megmaradjanak a felhasználókat a helyszíni Active Directoryban, továbbra is hasznosak kell az Azure Active Directory csatlakozott felhasználók bejelentkezésen. Ennek az oka az Azure Active Directory egyszerűsített csatlakozó élmény, hatékony mobileszköz-kezelés, automatikus mobileszköz-kezelés regisztrációs és egyszeri bejelentkezéses lehetőséget kínál a az Azure Active Directory és a helyszíni erőforrásokat.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Milyen funkciókat tartalmaz Azure Active Directory Bekapcsolódás kínálnak?
Azure Active Directory csatlakozik az alábbi kap:

* **Vállalati tulajdonú eszközök önálló kiépítése**: a Windows 10, a felhasználók konfigurálhat egy teljesen új, légmentes fóliacsomagolású eszközt a kimenő kész felület informatikai bevonása nélkül.


* **Modern űrlap tényezők támogatása**: Azure Active Directory illesztés, amelyeken nincs telepítve a hagyományos tartomány Bekapcsolódás lehetőségeit eszközökön működik.  


* **Meglévő szervezeti fiókok támogatása**: felhasználók létrehozása és kezelése nincs szüksége egy elvégezve a legjobb élmény vállalat által kibocsátott eszközök, mint a Windows 8 rendszerben egy személyes Microsoft-fiókjával. Azok is használja a meglévő munka fiókjuk az Azure Active Directory. A sok szervezetek ez alapvetően azt jelenti, hogy a felhasználók beállítása, és jelentkezzen be Windows ugyanazt az Office 365 eléréséhez használt hitelesítő adataival.


* **Automatikus mobileszköz-kezelés regisztrációs**: eszközök automatikusan ebben a mobileszköz-kezelés Azure Active Directory csatlakozáskor. Ez a folyamat működik-e a Microsoft Intune és a partnerek mobileszköz management megoldások. Mobileszköz-kezelés az Intune befejezése után rendszergazdák is monitor/kezelheti Azure Active Directory-tartományhoz tartományhoz csatlakoztatott eszközökön a SCCM management console mellett.


* **Egyszeri bejelentkezés a vállalati erőforrások**: felhasználók egyszeri bejelentkezés a Windows asztal az alkalmazások és a felhőben, például az Office 365- és Azure Active Directory authentication és [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md)az támaszkodó üzleti alkalmazások ezer erőforrások fogják túlságosan nagyra értékelni. Az eszköz esetén a vállalati hálózathoz, és bárhonnan ezek az erőforrások az [Azure Active Directory szolgáltatásalkalmazás-Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx)keresztül elérhető Azure Active Directory tartományhoz vállalati tulajdonú eszközök is élvezzenek SSO helyszíni erőforrásokhoz.


* **Operációs rendszer állapota barangolás**: kisegítő lehetőségek, a webhelyek, a Wi-Fi jelszavakat és az egyéb beállítások vannak szinkronizálva minden eszközön vállalati tulajdonában anélkül, hogy egy személyes Microsoft-fiókjával.


* **Vállalati kész a Windows áruházból**: A Windows áruházból alkalmazás beszerzése és Azure Active Directory-fiókok licencelési támogat. Szervezetek is mennyiségi licencet alkalmazások elérhetővé tétele a felhasználók a szervezet és.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hogyan működnek a különböző eszközök az Azure Active Directory csatlakozni?

| Vállalati eszköz (tartományhoz a helyszíni)                                                                                                                                                                                         | Vállalati eszköz (a felhőbe csatlakozott)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Eszköz                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Felhasználók is jelentkezzen be Windows munka hitelesítő adatokkal (mint ma).                                                                                                                                                                        | Felhasználók is jelentkezzen be Windows Azure Active Directory kezelése munka hitelesítő adatokkal. Ez a vállalati eszközök három esetben fontos: 1) a szervezet Active Directory nem rendelkezik helyszíni (például a kisvállalati verzió esetén). 2) a szervezet nem minden felhasználói fiók létrehozása az Active Directoryban (például a diákok, a tanácsadóknak vagy a szezonális dolgozók fiókok program nem hoz létre az Active Directory). 3) a szervezetnek vállalati eszközök, nem kell tartományhoz egy (helyszíni), például a telefonra vagy táblagépre futó Mobile Termékváltozatot (például egy másodlagos eszköz hozni egy gyár/kiskereskedelmi padló). Azure Active Directory illesztés támogatja a csatlakozás a vállalati eszközök felügyelt és a szövetséges szervezeteket. | Felhasználók jelentkezzen be Windows a személyes Microsoft-fiók hitelesítő adatait (itt nincs változás).                                                |
| A felhasználók rendelkeznek központi beállítások eléréséhez és a nagyvállalati verzió a Windows áruházból. Az alábbi szolgáltatások munka működik, és nincs szükség a személyes Microsoft-fiókjával. Ehhez a szervezetek Azure AD a helyszíni Active Directory csatlakozni.                                        | Felhasználók önkiszolgáló telepítés hajthat végre. Azok is hajtsa végre az első indításkori (FRX) keresztül a munka-fiókjukkal ahelyett, hogy problémákat informatikai rendelkezést eszközök, Habár mindkét módszer használható.                                                                                                                                                                                                                                                                                                                                                                             | Felhasználói egyszerűen vehetnek fel a munka-fiókot, amelyet kezelik az Active Directory vagy Azure AD.                                                      |
| A felhasználók egyszeri bejelentkezés képes az asztalról alkalmazások, a webhelyek és a források – beleértve a helyszíni erőforrások és a felhő alkalmazások Azure Active Directory authentication használó rendelkeznek.                                                                                                            | Eszközök automatikusan regisztrált a vállalati címtárban (Azure Active Directory), és a mobileszköz-kezelés automatikusan tehát. (Azure Active Directory prémium funkció).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Felhasználók egyszeri bejelentkezés lehetősége van, használt alkalmazások és a webhelyek vagy a munkahelyi fiókjával.                                              |
| Felhasználói vehetnek fel saját személyes Microsoft-fiók nélkül vállalati adatok érintő saját személyes képek és fájlok eléréséhez. (Barangolás beállításainak is használhassa a munka-fiókjuk.) A Microsoft-fiók lehetővé teszi, hogy egyszeri bejelentkezés, és már nem a beállítások megadása központi meghajtók.  | Felhasználók hajthat végre egy önkiszolgáló jelszó-visszaállítás a winlogon, ami azt jelenti, elfelejtett jelszó alaphelyzetbe állíthassák. (Azure Active Directory prémium funkció).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Felhasználó fér hozzá a vállalati a Windows áruházból, hogy szerezheti be, és a vállalati verziós alkalmazások használata a személyes eszközök. |                                                               |


## <a name="additional-information"></a>További információk
* [A Windows 10, a nagyvállalati: eszközök használata munkahelyi módjai](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú lehetőségeket a Windows 10-es eszközökön keresztül Azure Active Directory Bekapcsolódás bővítése](active-directory-azureadjoin-user-upgrade.md)
* [Jelszavak – Microsoft Passport nélkül hitelesítő identitások](active-directory-azureadjoin-passport.md)
* [Használatát esetek is találhat az Azure Active Directory csatlakozás](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás tartományhoz csatlakoztatott eszközök Azure AD a Windows 10-élmény](active-directory-azureadjoin-devices-group-policy.md)
* [Azure Active Directory Bekapcsolódás beállítása](active-directory-azureadjoin-setup.md)
