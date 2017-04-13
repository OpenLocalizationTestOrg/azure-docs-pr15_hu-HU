<properties
    pageTitle="# Az automatikus eszköz regisztráció Windows 7 tartományhoz csatlakoztatott eszközök beállítása |} Microsoft Azure"
    description="Az illesztés eszközök: Azure AD automatikus regisztrálása lépéseket követve állítsa be a Windows 7-ben tartományt. és a lépéseket a Windows 7-ben tartományt szeretne telepíteni, az eszköz regisztrációs szoftvercsomag csatlakozott eszközök, például a System Center Configuration Manager szoftver terjesztési rendszert használ."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Az automatikus eszköz regisztráció Windows 7 tartományhoz csatlakoztatott eszközök beállítása

Az informatikai rendszergazda, mint a Windows 7 tartományhoz csatlakoztatott eszközök: Azure AD automatikus regisztrálása is beállíthatja. Ehhez az eszköz regisztrációs szoftvercsomag telepítenie kell a Windows 7-ben tartományt szeretne csatlakozott az eszközök, például a System Center Configuration Manager szoftver terjesztési rendszert használ. Győződjön meg arról, olvassa el a, és fejezze be az automatikus eszköz regisztrációs az Azure Active Directory-Windows tartományhoz eszközök felsorolt előfeltételek.

>[AZURE.NOTE]
 Legújabb hogy miként állíthatja be az automatikus eszköz regisztrációs témaköröket olvassa el, [Automatikus regisztráció Windows-tartomány beállítása az Azure Active Directory eszközökhöz illesztés](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Az eszköz regisztrációs szoftvercsomag telepítése a Windows 7 tartomány csatlakozott eszközök

Windows 7-alapú eszköz regisztrációs [letölthető MSI-csomag](https://connect.microsoft.com/site1164)érhető el. A csomag telepíteni kell az Active Directory-tartomány is csatlakozik a Windows 7 gépeken. Kell telepítenie a csomagot, például a System Center Configuration Manager szoftver terjesztési rendszert használ. Az MSI-csomag támogatja a normál Csendes telepítési beállítások, használja a/quiet paramétert.
A [Microsoft Connect webhely](https://connect.microsoft.com/site1164)letöltés szoftvercsomag érhető el. Itt választhatja majd töltse le a munkahelye Bekapcsolódás a Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Az Azure Active Directory munkahelye illesztés
Windows 7 tartományhoz csatlakoztatott eszközök eszköz regisztráció szükséges vagy nem tartozik felhasználói felület. Miután telepített a számítógépen, jelentkezik be a számítógép tartományhoz bárki lesz automatikusan és csendes regisztrálva eszköz objektummal Azure AD. A fizikai eszközök regisztrált minden felhasználójának Azure AD egy eszköz objektum lesz.

A telepítő ütemezett tevékenység a rendszer, amely a felhasználó környezetében fut, és a felhasználónak bejelentkezés induljanak hoz létre. A tevékenység csendes regisztrálja a felhasználót, és a befejeződése után a felhasználó jelek az Azure Active Directory eszközt.
Az ütemezett tevékenység megtalálható a **Microsoft**a Feladatütemező könyvtár > **Munkahelye csatlakozni**.
A feladat futtatása, és a bejelentkezés a számítógépen, amely minden Active Directory-felhasználók regisztrálni.
Az alábbi ábrán a lépésenkénti automatikus eszköz regisztrációs folyamat sorolja fel.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Egy felhasználó (információk dolgozó) jelentkezik be a Windows 7 ügyfélszámítógép, az Active Directory tartományi hitelesítő adataival.
1. A munkahelyi csatlakozás ütemezett tevékenység végrehajtása.
1. A felhasználó csendes hitelesítése az AD FS Windows-hitelesítés használatával.
1. A Windows 7 rendszerben az Azure Active Directory regisztrálva van a felhasználó számára.
1. Azure AD egy eszköz objektummal és a tanúsítvány jön létre. Az objektum jelöli a user@device.
1. A csatlakozás a munkahelyi tanúsítvány a számítógépen van tárolva.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>A Windows 7 tartomány regisztrációjának csatlakozott eszközök

Megadhatja, hogy a tartományhoz csatlakoztatott Windows 7 eszközök unregister a következő módon: a munkahelyi Bekapcsolódás eltávolítása a Windows 7 tartományból szoftvercsomag csatlakozott eszközök, például a System Center Configuration Manager szoftver terjesztési rendszert használ.

Ezután nyissa meg a parancssort, a Windows 7-gépen, és hajtsa végre a következő az eszköz unregister parancsot:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Ez a parancs a gép az aláírt tartomány felhasználónként környezetében kell futtatni.
Az Eseménynapló és hibák a Windows 7 tartományhoz csatlakoztatott eszközökön.

A Windows eseménynaplójának a Windows 7-gépen munkahelye Bekapcsolódás kapcsolódó üzeneteket jeleníti meg. Üzenetek sikeres és sikertelen munkahelye Bekapcsolódás eseményekhez talál. Az Eseménynapló megtalálható az Eseménynapló az alkalmazások és szolgáltatásnaplók Viewer > Microsoft-munkahelye csatlakozni.

## <a name="additional-topics"></a>További témakörök

- [Azure Active Directory eszköz regisztráció áttekintése](active-directory-conditional-access-device-registration-overview.md)
- [Az automatikus eszköz regisztrációs Azure Active Directory for Windows Domain-Joined eszközzel](active-directory-conditional-access-automatic-device-registration.md)
- [Az automatikus eszköz regisztráció Windows 8.1 tartományhoz csatlakoztatott eszközök beállítása](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Az automatikus eszköz regisztrációs az Azure Active Directory a Windows 10-tartományhoz csatlakoztatott eszközök](active-directory-azureadjoin-devices-group-policy.md)
