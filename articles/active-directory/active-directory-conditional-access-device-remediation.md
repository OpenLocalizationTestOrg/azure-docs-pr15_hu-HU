<properties
    pageTitle="Azure Active Directory access problémáinak elhárítása |} Microsoft Azure"
    description="Ismerje meg, hogy a szervezet online források az access problémák megoldására tehet."
    services="active-directory"
    keywords="eszköz-alapú feltételes access eszköz regisztráció, eszköz regisztrációs, eszköz regisztrációs és MDM engedélyezése"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Azure Active Directory access kapcsolatos problémák elhárítása

Próbálja meg, ha a szervezet SharePoint Online intranet elérését, és a "hozzáférés megtagadva" hibaüzenet jelenik meg. mivel foglalkozol?

Ez a cikk bemutatja a remediation lépések, amelyek segítséget nyújtanak a szervezet online források az access problémák megoldásához.

Azure Active Directory (Azure Active Directory) segítségre elérése a problémákat, ugorjon a cikk az eszköz platform eltakaró című:

-   Windows-eszközön
-   iOS-eszközön (ellenőrzése vissza, amint az iPhone és iPad eszközök segítségért.)
-   Android-készülék (ellenőrizni hamarosan segítség az Android-telefonokon és táblaszámítógépeken.)

## <a name="access-from-a-windows-device"></a>Hozzáférés a Windows-eszközön

Ha az eszköz futtatása a következő platformokon közül, nézze meg a következő szakaszok a következő hibaüzenettel eléréséhez, az adott alkalmazás vagy szolgáltatás látható:

- Windows 10-ben
- Windows 8.1
- Windows 8-ban
- Windows 7 rendszerben
- A Windows Server 2016-ban
- A Windows Server 2012 R2
- A Windows Server 2012
- A Windows Server 2008 R2 rendszerben

### <a name="device-is-not-registered"></a>Nem regisztrált eszköz

Ha az eszköz nem regisztrált Azure Active Directory és eszköz-alapú házirend védi az alkalmazás, a lapot, amely tartalmazza a következő hibaüzenetek egyike jelenhet meg:

!["Nem sikerül van innen" üzenetet nem regisztrált eszközök] (./media/active-directory-conditional-access-device-remediation/01.png "Eset")

Ha az eszköz a tartományhoz az Active Directory a szervezet, próbálkozzon a következővel:

1.  Győződjön meg arról, hogy, jelentkezzen be Windows munkahelyi fiókját (az Active Directory-fiókját) használatával.
2.  Csatlakozás a vállalati hálózathoz, virtuális magánhálózaton (VPN) vagy a DirectAccess.
3.  Miután csatlakozott, nyomja le a Windows billentyű + zárolásához a Windows-munkamenet az L billentyűt.
4.  Írja be a munkahelyi fiók hitelesítő adatait a Windowsból zárolásának feloldásához.
5.  Várja meg a percet, és próbálkozzon újra az alkalmazás vagy szolgáltatás eléréséhez.
6.  Ha ugyanazon a lapon látható, kattintson a **További részleteket** hivatkozásra, és forduljon a rendszergazdához az adataival.

Ha az eszköz nem tartományhoz és a Windows 10 fut, két lehetőség van:

- Azure Active Directory illesztés futtatása
- A munkahelyi vagy iskolai fiók hozzáadása a Windows

Miben különböznek a beállításokkal kapcsolatos további tudnivalókért lásd: [használatával a Windows 10-es eszköz a munkahelyi](active-directory-azureadjoin-windows10-devices.md).

Azure Active Directory Bekapcsolódás futtatásához tegye az alábbi lépéseket a platform fut az eszközön. (Azure Active Directory illesztés nem áll rendelkezésre a Windows rendszerű telefonok.)

**Frissítés a Windows 10 évforduló**

1.  Nyissa meg a **Beállítások** alkalmazást.
2.  Kattintson a **fiókok** > **Access munkahelyi vagy iskolai**.
3.  Kattintson a **Csatlakozás**gombra.
4.  Kattintson a **Bekapcsolódás az Azure Active Directory eszköz**.
5.  A szervezet hitelesíteni, többtényezős hitelesítés adja meg, amikor a program kéri, majd kövesse a megjelenő.
6.  Jelentkezzen ki, majd jelentkezzen be a munkahelyi fiókjával.
7.  Az alkalmazás eléréséhez, próbálkozzon újra.


**A Windows 10 November 2015 frissítése**

1.  Nyissa meg a **Beállítások** alkalmazást.
2.  Kattintson a **rendszer** > **kapcsolatban**.
3.  Kattintson a **Bekapcsolódás az Azure Active Directory**.
4.  A szervezet hitelesíteni, többtényezős hitelesítés adja meg, amikor a program kéri, majd kövesse a megjelenő.
5.  Jelentkezzen ki, és jelentkezzen be a munkahelyi fiókjával (az Azure Active Directory-fiók).
6.  Az alkalmazás eléréséhez, próbálkozzon újra.

A munkahelyi vagy iskolai fiók felvételéhez hajtsa végre az alábbi lépéseket:

**Frissítés a Windows 10 évforduló**

1.  Nyissa meg a **Beállítások** alkalmazást.
2.  Kattintson a **fiókok** > **Access munkahelyi vagy iskolai**.
3.  Kattintson a **Csatlakozás**gombra.
4.  A szervezet hitelesíteni, többtényezős hitelesítés adja meg, amikor a program kéri, majd kövesse a megjelenő.
5.  Az alkalmazás eléréséhez, próbálkozzon újra.


**A Windows 10 November 2015 frissítése**

1.  Nyissa meg a **Beállítások** alkalmazást.
2.  Kattintson a **fiókok** > **a fiókok**.
3.  Kattintson a **vegye munkahelyi vagy iskolai fiókjával**.
4.  A szervezet hitelesíteni, többtényezős hitelesítés adja meg, amikor a program kéri, majd kövesse a megjelenő.
5.  Az alkalmazás eléréséhez, próbálkozzon újra.

Ha az eszköz nem tartományhoz és futtatja a Windows 8.1, csatlakozás a munkahelyi és jelentkezhet a Microsoft Intune, hajtsa végre az alábbi lépéseket:

1.  Nyissa meg a **Gépház parancsra**.
2.  Kattintson a **hálózat** > **munkahelye**.
3.  Kattintson a **Csatlakozás**gombra.
4.  A szervezet hitelesíteni, többtényezős hitelesítés adja meg, amikor a program kéri, majd kövesse a megjelenő.
5.  Kattintson a **bekapcsolása**.
6.  Az alkalmazás eléréséhez, próbálkozzon újra.


### <a name="browser-is-not-supported"></a>Nem támogatott böngésző

Akkor lehet, hogy megtagadja a hozzáférést Ha használatával történő elérésére egy alkalmazás vagy szolgáltatás az alábbi böngészők egyikét:

- A Chrome, Firefox vagy bármely más böngészőben nem Microsoft Edge vagy a Microsoft Internet Explorer, a Windows 10-es vagy Windows Server 2016-ban
- A Firefox Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 vagy Windows Server 2008 R2 rendszerben

Megjelenik egy hiba lap, így néz ki:

!["Nem sikerül van innen" üzenetet nem támogatott böngészők] (./media/active-directory-conditional-access-device-remediation/02.png "Eset")

A csak remediation, hogy az alkalmazás az eszköz platform támogató böngészőkben használható.

## <a name="next-steps"></a>Következő lépések

[Azure Active Directory feltételes hozzáférés](active-directory-conditional-access.md)
