
<properties
    pageTitle="Azure Active Directory feltételes Access technikai útmutató |} Microsoft Azure"
    description="Feltételes hozzáférés-vezérlés az Azure Active Directory ellenőrzi az adott feltételeknek választhat, amikor a felhasználó hitelesítése és előtt, hogy az alkalmazás hozzáférést. Ha ezen feltételek teljesülése esetén a felhasználó a hitelesített és az alkalmazás férhet hozzá."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure Active Directory feltételes Access technikai útmutató

## <a name="services-enabled-with-conditional-access"></a>Feltételes hozzáféréssel rendelkező engedélyezett szolgáltatások
Feltételes hozzáférési szabályok keresztül Azure AD-alkalmazás különféle támogatottak. A lista tartalmazza:

- A szövetséges alkalmazásokat az Azure Active Directory-alkalmazás gyűjteményből
- Jelszó SSO alkalmazásokat az Azure Active Directory-alkalmazás gyűjteményből
- Alkalmazások regisztrált a Azure szolgáltatásalkalmazás-Proxy
- Az üzleti és Azure Active Directory regisztrált több bérlői alkalmazások fejlesztett sor
- Visual Studio Online
- Azure Remote alkalmazáshoz
-   Dynamics CRM-hez
- A Microsoft Office 365 Yammerbe
- A Microsoft Office 365 Exchange Online
- A Microsoft Office 365 SharePoint Online (beleértve a OneDrive vállalati verzió)


## <a name="enable-access-rules"></a>Hozzáférési szabályok engedélyezése

Szabályok engedélyezhető vagy tiltható le a egy per alkalmazás alapjait. Ha szabályok **be** őket engedélyezve lesz, és vannak érvényben az alkalmazás elérő felhasználók számára. Amikor **ki** nem fogja használni, és nem befolyásolja a felhasználó bejelentkezési élmény.

## <a name="applying-rules-to-specific-users"></a>Szabályok alkalmazása az egyes felhasználók
Szabályok **Alkalmazása**történő biztonsági csoport alapján meghatározott felhasználócsoportok alkalmazhatók. **Érvényes** beállítható, hogy **Az összes felhasználók** vagy **csoportok**. Ha az **Összes felhasználó** szabályok vonatkoznak bármely felhasználó az alkalmazás hozzáférést. A **csoportok** beállítás lehetővé teszi, hogy adott biztonsági és terjesztési csoportok kiválasztását, szabályok csak szabályon ezekhez a csoportokhoz.

Telepít egy szabályt, amikor célszerű először alkalmazza korlátozott beállítása a felhasználók, amelyek egy piloting csoportok tagjainak közös. Miután befejeződött a szabály az **Összes felhasználó**alkalmazhatók. Ennek hatására a szabályt a a szervezet összes felhasználójának érvényesíthetők.

Válassza a csoportok is **kivételével** parancs házirend alól. Bármely ezekhez a csoportokhoz tagjai fog alól, még akkor is, ha egy része csoportban jelennek meg.

## <a name="at-work-networks"></a>"A munkahelyén" hálózatok


A megbízható IP-címtartományai Azure Active Directory, a beállított feltételes hozzáférési szabályokat, amelyek egy "munkahelyi" hálózat használja arra, vagy a "belül corpnet" állítás Active Directory összevonási szolgáltatások használatára. Többek között az alábbiakat:

- Ha nem a munkahelyén többtényezős hitelesítést igényel
- Ha nem a munkahelyén hozzáférésének letiltása

LDAP beállításainak "munkahelyi" hálózatok

1. [Többtényezős hitelesítés beállítása lapon](../multi-factor-authentication/multi-factor-authentication-whats-next.md)állítsa be a megbízható IP-címtartományok. Feltételes hozzáférési házirend használja a beállított tartományok minden hitelesítési kérelem és jogkivonat kiállítási szabályok ki szeretné számítani. 
2. Állítsa be belsejében felhasználása corpnet formál, ezt a lehetőséget kínál a szövetséges könyvtárak az AD FS Összetevőt használja. [Többet szeretne tudni a belső coronet követelések](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Állítsa be nyilvános IP-címtartományok. A beállítás lapon a könyvtár, beállíthatja, hogy nyilvános IP-címeket. Feltételes Access fogja használni ezek "munkahelyi" IP-címek, és lehetővé teszi a további tartomány felett az 50 IP cím korlátozást megvalósul a MFA beállítás lap konfigurálása.



## <a name="rules-based-on-application-sensitivity"></a>Szabályok alkalmazása magánjellegű alapján

Szabályok egy alkalmazás lehetővé teszi a magas érték szolgáltatások biztonságát érintő egyéb szolgáltatásokhoz való hozzáférés nélkül úgy vannak beállítva. Feltételes hozzáférési szabályokat a **beállítás** lapon, az alkalmazás beállítható. 

Szabályok, amely jelenleg ingyenesen áll:

- **Többtényezős hitelesítést igényel**
 - Minden felhasználónak, ezzel a házirenddel alkalmazott kell legalább egyszer többtényezős hitelesítés hitelesíthet.
 
- **Ha nem a munkahelyén többtényezős hitelesítést igényel**
 - Alkalmazza a házirendet, ha minden felhasználónak kell legalább egyszer többtényezős hitelesítés elvégeztetni, ha nem munka távolról hozzáférésük van a szolgáltatás. Ha az azok áthelyezése egy munkából távoli helyre, azok kötelező többtényezős hitelesítést végrehajtani, amikor a szolgáltatás.
 
- **Ha nem a munkahelyén hozzáférésének letiltása** 
 - Amikor a felhasználó egy távoli helyre viszi munkából, azok, azzal blokkolhatja a "Ha nem a munkahelyén hozzáférésének letiltása az" házirend alkalmazása esetén őket.  Őket újra engedélyezett egy helyét a hozzáférést.


## <a name="related-topics"></a>Kapcsolódó témakörök

- [Azure Active Directory csatlakozik az Office 365-ben és más alkalmazások való hozzáférés biztonságossá tétele](active-directory-conditional-access.md)
- [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
