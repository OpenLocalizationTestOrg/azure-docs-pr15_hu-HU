<properties
    pageTitle="Az automatikus eszköz regisztráció Windows 8.1 tartományhoz csatlakoztatott eszközök beállítása |} Microsoft Azure"
    description=" Állítsa be a Windows 8.1 tartományhoz csatlakoztatott eszközök: Azure AD automatikus regisztrálása csoportházirend lépéseket. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Az automatikus eszköz regisztráció Windows 8.1 tartományhoz csatlakoztatott eszközök beállítása

Az Active Directory csoportházirend használatával állítsa be a Windows 8.1 tartományhoz csatlakoztatott eszközök: Azure AD automatikus regisztrálása. A csoportházirend beállításához legalább egy tartomány illesztett Windows Server 2012 R2 vagy Windows 8.1 gépi kell rendelkeznie a Csoportházirend kezelése szolgáltatással telepítette. Ennek a Csoportházirend-engedélyezve van a tartomány, miután jelentkezik be a számítógép tartományhoz bárki lesz automatikusan és csendes regisztrálva eszköz objektummal Azure AD. A fizikai eszközök regisztrált minden felhasználójának Azure AD egy eszköz objektum lesz. Győződjön meg arról, olvassa el a, és fejezze be az automatikus eszköz vételét Azure Active Directory for Windows Domain-Joined eszközökkel a előfeltételek.

>[AZURE.NOTE]
 Legújabb hogy miként állíthatja be az automatikus eszköz regisztrációs témaköröket olvassa el, [Automatikus regisztráció Windows-tartomány beállítása az Azure Active Directory eszközökhöz illesztés](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Állítsa be a csoportházirend-Windows 8.1 tartományhoz csatlakoztatott mobileszközökön

1. Nyissa meg a Kiszolgálókezelő, és kattintson az **eszközök** > **A Csoportházirend kezelése**.
2. A Csoportházirend kezelése nyissa meg a tartomány csomópontot, amely megfelel a tartományba, amelyben a mezőkódokkal **Munkahelye automatikus csatlakozás**engedélyezése.
3. Kattintson a jobb gombbal a **Csoportházirend-objektumok** , és válassza az **Új**. Nevezze el a csoportházirend-objektum, például a **Munkahelyi automatikus csatlakozás**. Kattintson az **OK gombra**.
4. Kattintson a jobb gombbal az új csoportházirend-objektumot, és kattintson a **Szerkesztés**gombra.
5. Nyissa meg azt a **Számítógép konfigurációja** > **házirendek** > **Felügyeleti sablonok** > **Windows-összetevők** > **munkahelye csatlakozni**.
6. Kattintson a jobb gombbal automatikusan munkahelye illesztés ügyfélszámítógépek, és válassza a **Szerkesztés**.
7. Jelölje be a engedélyezve választógombot, és kattintson az Alkalmaz gombra. Kattintson az **OK gombra**.
8. Előfordulhat, hogy most csatol a csoportházirend-objektumot a kívánt helyre. Ha engedélyezni szeretné a Windows 8.1 tartományhoz csatlakoztatott eszközök a szervezete összes házirend, hivatkozás a csoportházirend a tartomány.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>A Windows 8.1 tartomány regisztrációjának csatlakozott eszközök

Megadhatja, hogy a tartományhoz csatlakoztatott Windows 8.1 eszközök unregister a következő módon: az előző részben létrehozott csoportházirend munkahelye Bekapcsolódás beállításainak módosítása. Állítsa a automatikusan munkahelye illesztés számítógépek ügyfélházirend letiltja. Ez megakadályozza, hogy az új eszközök a munkahely automatikusan csatlakozását.

A meglévő tartományhoz csatlakoztatott Windows 8.1 gépek unregister szerint az alábbi két lehetőség egyikét:

* 1 beállítás: **a Windows 8.1 Unregister tartomány csatlakozott eszköz használatával a gépház**
  1. Windows 8.1 az eszközre, nyissa meg azt a **Gépház** > **hálózati** > **munkahelye**
  2. Válassza a **Kilépés**.
Ez a folyamat minden tartomány minden olyan felhasználóhoz, a gép az aláírta, és automatikusan már csatlakozott munkahelye meg kell ismételni.

* 2 beállítás: A Windows 8.1 tartomány illesztett eszköz parancsfájl használatával unregister parancsot.
    1. Nyisson meg egy parancssort, a Windows 8.1 számítógépen, és hajtsa végre az alábbi parancsot:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Ez a parancs a gép az aláírt tartomány felhasználónként környezetében kell futtatni.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Az Eseménynapló és hibák a Windows 8.1 tartományhoz csatlakoztatott eszközökön

A Windows eseménynaplójának Windows 8.1 gépre eszköz regisztrációs kapcsolódó üzeneteket jeleníti meg. Üzenetek sikeres és sikertelen események találja. 

Az Eseménynapló megtalálható az Eseménynapló az alkalmazások és szolgáltatások **Naplók**Viewer > **Microsoft** > **Windows > Csatlakozás munkahelye**.

##<a name="additional-details"></a>További részletek

A csoportházirend lehetővé teszi, hogy a rendszer, amely a felhasználó környezetében fut, és a felhasználónak bejelentkezés induljanak ütemezett tevékenység. A tevékenység csendes regisztrálja a felhasználó és eszköz az Azure Active Directory, a bejelentkezés befejeződése után. Az ütemezett tevékenység megtalálható a Feladatütemező könyvtár a **Microsoft**Windows 8.1-eszközök > **Windows** > **Munkahelye csatlakozni**. A feladat futtatása, és a minden Active Directory-felhasználók regisztrálhatja adott jelentkezzen be. 

## <a name="additional-topics"></a>További témakörök
- [Azure Active Directory eszköz regisztráció áttekintése](active-directory-conditional-access-device-registration-overview.md)
- [Az automatikus eszköz regisztrációs az Azure Active Directory a Windows 10-tartományhoz csatlakoztatott eszközök](active-directory-conditional-access-automatic-device-registration.md)
- [Az automatikus eszköz regisztráció Windows 7 tartományhoz csatlakoztatott eszközök beállítása](active-directory-conditional-access-automatic-device-registration-windows7.md)

