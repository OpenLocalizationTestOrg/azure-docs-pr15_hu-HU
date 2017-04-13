<properties
    pageTitle="Azure Active Directory jelszólejárati házirendekről beállítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként jelölje be a jelszólejárati házirendekről, és módosítsa a felhasználó jelszólejárati egyenként vagy tömegesen, az Azure Active directory-jelszavak"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Az Azure Active Directory jelszólejárati házirendekről jelszó megadása

> [AZURE.IMPORTANT] **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).

Globális rendszergazdaként egy Microsoft felhőalapú szolgáltatás a Microsoft Azure Active Directory modul Windows Powershellhez felhasználói jelszavak beállítása, hogy ne járjon le is használhatja. Ha el szeretné távolítani a Windows PowerShell-parancsmagok is használhatja a nem-jár le konfigurációs vagy megtekintéséhez a felhasználói jelszavak beállítása nem járjon le. Ez a cikk segítséget nyújt a felhőalapú szolgáltatások, például Microsoft Intune és az Office 365-be, amelyek a Microsoft Azure Active Directory az identitás- és -szolgáltatások használja arra.

  > [AZURE.NOTE] Csak olyan felhasználói fiókok nem szinkronizálódnak címtár-szinkronizálás keretében jelszavak beállítható, hogy ne járjon le. Címtár-szinkronizálás kapcsolatos további tudnivalókért tekintse át a [címtár-szinkronizálás menete](https://msdn.microsoft.com/library/azure/hh967642.aspx)témakörei listáját.

A Windows PowerShell-parancsmagok használatához előbb telepítenie kell őket.

## <a name="what-do-you-want-to-do"></a>Milyen feladatot szeretne tenni?

- [A jelszó jelszólejárati házirendjének ellenőrzése](#how-to-check-expiration-policy-for-a-password)

- [Jelszó beállítása, hogy lejárjon](#set-a-password-to-expire)

- [Jelszó beállítása, hogy nem jár](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>A jelszó jelszólejárati házirendjének ellenőrzése

1.  Csatlakozás Windows PowerShell a vállalati rendszergazdai hitelesítő adataival.

2.  Tegye a következők valamelyikét:

    - Szeretné látni, hogy egy felhasználó jelszavát legyen-e beállítva, hogy sohase járjon le, futtassa a következő parancsmagot a felhasználó egyszerű felhasználónév (UDN) használatával (például aprilr@contoso.onmicrosoft.com) vagy annak a felhasználónak meg szeretné tekinteni a felhasználói Azonosítójával:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Ha látni szeretné a minden felhasználónak "Jelszó sohasem jár le" beállítását, futtassa a következő parancsmagot:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Jelszó beállítása, hogy lejárjon

1.  Csatlakozás Windows PowerShell a vállalati rendszergazdai hitelesítő adataival.

2.  Tegye a következők valamelyikét:

    - Egy felhasználó jelszavát úgy beállítani, a jelszó lejáratának, futtassa a következő parancsmagot a felhasználó egyszerű felhasználónév (UDN) használatával vagy a felhasználói Azonosítójával:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Az összes felhasználó jelszavakat a szervezeten belül úgy beállítani, azok is lejár, használja az alábbi parancsmagot:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Jelszó beállítása, hogy sohase járjon le

1. Csatlakozás Windows PowerShell a vállalati rendszergazdai hitelesítő adataival.

2.  Tegye a következők valamelyikét:

    - Adja meg a jelszót, ha egy felhasználó, hogy sohase járjon le, futtassa a következő parancsmagot a felhasználó egyszerű felhasználónév (UDN) használatával vagy a felhasználói Azonosítójával:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - A jelszavát szeretné beállítani az összes felhasználó szervezeti, hogy sohase járjon le, futtassa a következő parancsmagot:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Következő lépések

* **Itt azért, mert netán problémákat tapasztal bejelentkezéskor?** Ha igen, [az alábbiakban hogyan módosíthatja, és a saját jelszó alaphelyzetbe állítása](active-directory-passwords-update-your-own-password.md).
