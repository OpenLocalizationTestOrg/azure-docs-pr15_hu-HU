<Properties
    pageTitle="Azure Active Directory feltételes access |} Microsoft Azure"  
    description="Használni feltételes hozzáférés-vezérlés az Azure Active Directory a megadott feltételek esetén az access-alkalmazások hitelesítése."  
    services="active-directory"
    keywords="feltételes access-alkalmazások, a feltételes hozzáférés Azure Active Directory, a vállalati erőforrások, feltételes hozzáférési biztonságos hozzáférés"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Az Azure Active Directory feltételes hozzáférés   

A vezérlő funkciók az Azure Active Directory (Azure Active Directory) feltételes access biztonságos erőforrások az a felhő és a helyszíni egyszerű módszert kínál. Többtényezős hitelesítés kiküszöbölheti a kockázata, mint az access feltételes házirendek ellopott és phished hitelesítő adatait. Más feltételes hozzáférési szabályok segítségével, hogy a szervezet adatok biztonsága. Például nemcsak a hitelesítő adatok igénylő, lehet, hogy egy házirendet, hogy csak egy mobileszköz-kezelési rendszerben vannak tehát, például a Microsoft Intune férhet hozzá a szervezet bizalmas szolgáltatások eszközök.


## <a name="prerequisites"></a>Előfeltételek

Azure Active Directory feltételes access lehetővé teszi az [Azure Active Directory prémium verzióban](http://www.microsoft.com/identity). Minden felhasználó, aki hozzáfér egy alkalmazást, amelynek alkalmazott feltételes access házirendek az Azure Active Directory prémium licencet kell rendelkeznie. További információ a [licenccel nem rendelkező felhasználók](https://aka.ms/utc5ix)jelentésben licenc követelmények talál.


## <a name="how-is-conditional-access-control-enforced"></a>Hogyan vannak érvényben feltételes hozzáférés-vezérlés?  

Feltételes hozzáférés-vezérlés helyen az Azure Active Directory keressen az egyedi feltételeket, adja meg a felhasználó alkalmazás eléréséhez. Miután hozzáférési követelmények teljesülése esetén a felhasználó hitelesített, és férhet hozzá az alkalmazás.  

![Feltételes hozzáférés – áttekintés](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Feltételek

Feltételek szerint is elhelyezhet egy feltételes hozzáférési házirendet a következők:

- **A csoporttagság**. A csoport tagsága alapján egy felhasználói hozzáférés szabályozása.

- **Helyét**. Hol található a felhasználót, hogy az eseményindító többtényezős hitelesítés használja, és a felhasználó nem szerepel a megbízható hálózati blokk-vezérlők használatával.

- **Eszköz platform**. Az eszköz platform iOS, az Android, a Windows Mobile és a Windows, például egy feltétel feltételes formázási szabály kiszolgálónévként használja.

- Az **eszköz engedélyezve van**. Az eszköz állapotát, engedélyezhető vagy tiltható le, hogy érvényesítése eszköz házirend értékelés során. Ha letiltja a címtárban az elveszett vagy ellopott eszköz, azt is már nem megfelelnek a házirend.

- **Bejelentkezés, és a felhasználó a kockázat**. Feltételes hozzáférés kockázata házirendek az [Azure Active Directory-identitás védelem](active-directory-identityprotection.md) használata. Feltételes hozzáférés kockázata házirendek súgójában, a szervezet előzetes védelme a kockázat események és szokatlan bejelentkezési tevékenységek alapján.


## <a name="controls"></a>Vezérlők

Ezek a vezérlők feltételes hozzáférési házirend hivatkozási használható:

- **Többtényezős hitelesítés**. Többtényezős hitelesítés keresztül erős hitelesítés kérheti. Többtényezős hitelesítés is használhatja, Azure többtényezős hitelesítés vagy egy helyszíni többtényezős hitelesítés-szolgáltatóval, kombinálva az Active Directory összevonási szolgáltatások (AD FS) használatával. Többtényezős hitelesítés használatával segít az erőforrások az lehet, hogy szerzett érvényes felhasználói hitelesítő adatok eléréséhez jogosulatlan felhasználók által használt.

- **Továbbfejlesztett fájlblokkolás**. Alkalmazhat feltételeket, például a felhasználói hely felhasználói hozzáférés blokkolása. Például az access letiltani, ha egy felhasználó nem megbízható hálózaton.

- **Kompatibilis eszközök**. Beállíthatja, hogy feltételes hozzáférési szintre eszközt. A házirend beállítása előfordulhat, hogy csak a számítógépek, amelyek a tartományhoz tartozó vagy mobileszköz-kezelés alkalmazást, tehát vannak mobileszközök érheti el a szervezet erőforrásait. Például Intune használja annak ellenőrzésére, eszközt, és ezután jelentse Azure AD-kényszerítés alkalommal, amikor a felhasználó az alkalmazások eléréséhez. Részletes Intune-alkalmazások és az adatok védelme használatáról, olvassa el [védelme-alkalmazások és a Microsoft Intune adataival](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Is használhatja Intune adatvédelmi az elveszett vagy ellopott eszközök hivatkozási. További információért súgója [a teljes és szelektív törlést, mely a Microsoft Intune használata az adatok védelme](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Alkalmazások

Alkalmazhat egy feltételes hozzáférési házirendet, az alkalmazás szintjén. A helyszíni vagy felhőalapú hozzáférési szintek alkalmazások és szolgáltatások beállítása A házirend közvetlenül a webhely vagy a szolgáltatás érvényes. A házirend eléréséhez a böngészőben és a szolgáltatás hozzáférő alkalmazások lép érvénybe.


## <a name="device-based-conditional-access"></a>Eszköz alapuló feltételes hozzáférés

Az access alkalmazások korlátozhatják a eszközökről regisztrált, amely az Azure Active Directory, és amelyek megfelelnek a megadott feltételeknek. Feltételes access eszköz-alapú megakadályozza a felhasználók, akik próbálnak hozzáférni a erőforrásait egy szervezet erőforrások:

- Ismeretlen vagy nem felügyelt eszközök.
- Eszközök, amelyek nem felelnek meg a biztonsági házirendeket szervezete beállítása.

Beállíthatja, hogy ezeknek a követelményeknek alapján házirendek:

- **Eszközök a tartományhoz**. Állított be egy házirendet, amely tartományhoz egy helyszíni Active Directory és, amely szintén regisztrált az Azure Active Directory eszközök elérésének korlátozására. A házirend vonatkozik a Windows asztali számítógépek, a hordozható vagy a vállalati táblagépen.
Olvashat arról, hogy miként állíthatja be a tartományhoz eszközökhöz Azure AD automatikus regisztrálása olvassa el a [beállítása automatikus regisztráció Windows Azure Active Directory eszközökhöz tartományhoz](active-directory-conditional-access-automatic-device-registration-setup.md)című témakört.

- **Kompatibilis eszközök**. A **kompatibilis** a kezelés rendszerkönyvtárán a megjelölt eszközökre való hozzáférés korlátozása házirendjének beállítása. Ezzel a házirenddel biztosítja, hogy csak a megfelelő biztonsági házirendeket, például az eszközön fájl titkosítás kényszerítése eszközök engedélyezettek access. A házirend segítségével a következő eszközökön való hozzáférés korlátozása:

    - **A Windows tartományhoz csatlakoztatott eszközökön**. Felügyelt által System Center Configuration Manager (a az aktuális ág) hibrid konfigurációban.
    - A **Windows 10 Mobile munkahelyi vagy személyes eszközök**. Felügyelt Intune, illetve egy támogatott külső mobileszközén a projektirányítási rendszerek.
    - **az iOS és Android-eszközön**. Intune kezeli.


Azok a felhasználók, akik eszköz alapú eső alkalmazások elérését, hitelesítő hitelesítésszolgáltató házirend kell elérhetik az alkalmazást, amely megfelel a házirend készülékről. Hozzáférés megtagadva, ha megpróbálkozik vele a rendszer egy eszközön, amely nem felel meg a házirend követelményeinek.

Információt arról, hogy miként az Azure Active Directory hitelesítő hitelesítésszolgáltató eszköz alapú házirend konfigurálása című témakörben talál [az Azure Active Directory-kompatibilis alkalmazásokat eszköz-alapú feltételes hozzáférési házirend beállítása](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Erőforrások

Lásd az alábbi erőforrás kategóriák és cikkek, ha többet szeretne tudni a szervezet feltételes hozzáférés beállítása.


### <a name="multi-factor-authentication-and-location-policies"></a>Többtényezős hitelesítés és helyének házirendek

- [Azure Active Directory-kompatibilis alkalmazások csoport, helyét és többtényezős hitelesítés házirendek alapján feltételes access – első lépések](active-directory-conditional-access-azuread-connected-apps.md)
- [Alkalmazások által támogatott](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Eszköz alapuló feltételes hozzáférés

- [Hozzáférés-vezérlés az Azure Active Directory-kompatibilis alkalmazásokat feltételes access eszköz alapuló szabály beállítása](active-directory-conditional-access-policy-connected-applications.md)
- [Automatikus regisztráció Windows Azure Active Directory eszközökhöz tartományhoz tartozó beállítása](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Azure Active Directory access kapcsolatos problémák elhárítása](active-directory-conditional-access-device-remediation.md)
- [Az elveszett vagy ellopott eszközön adatok védelme a Microsoft Intune segítségével](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>A bejelentkezési kockázat alapján erőforrások védelme

-   [Azure Active Directory identitás-védelem](active-directory-identityprotection.md)

### <a name="next-steps"></a>Következő lépések

- [Feltételes access gyakori kérdések](active-directory-conditional-faqs.md)
- [Technikai útmutató](active-directory-conditional-access-technical-reference.md)
