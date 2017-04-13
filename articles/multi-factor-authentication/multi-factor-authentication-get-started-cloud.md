<properties
    pageTitle="Első lépések az Azure MFA a felhőben |} Microsoft Azure"
    description="Ez a a Microsoft Azure többtényezős hitelesítést lapot, amely bemutatja, hogy miként veheti használatba az Azure MFA a felhőben."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Első lépések – Azure többtényezős hitelesítés a felhőben
Hogyan kezdjek hozzá az Azure többtényezős hitelesítés a felhőben az alábbiakban ismertetjük.

> [AZURE.NOTE]  A következő dokumentáció engedélyezése a felhasználóknak az **Azure klasszikus portál**ismertetése. Ha a keresett olvashat Azure többtényezős hitelesítés beállítása az Office 365-felhasználóknak, olvassa el a [az Office 365 többtényezős hitelesítés beállítása.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA a felhőben](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Előfeltételek
Az alábbi előfeltételek szükség, mielőtt Azure többtényezős hitelesítés engedélyezése a felhasználóknak.


1. [Feliratkozás egy olyan Azure-előfizetésre](https://azure.microsoft.com/pricing/free-trial/) – Ha még nem rendelkezik az Azure előfizetéssel, kell előfizetési egyik. Ha csak kezdi az egyesítést, és azt használja az Azure MFA használhatja a próba-előfizetés
2. [Többtényezős hitelesítés szolgáltató létrehozása](multi-factor-authentication-get-started-auth-provider.md) és hozzárendelése a directory vagy a [felhasználóknak a licencek hozzárendelése](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licenceket a felhasználóknak, akik Azure MFA, Azure Active Directory Premium vagy vállalati mobilitás csomagja (EMS) érhetők el.  MFA Azure Active Directory prémium verzió és az EMS is tartalmazni fogja. Ha elég, nem szükséges hozzon létre egy Auth szolgáltatóval.


## <a name="turn-on-two-step-verification-for-users"></a>Kapcsolja be a felhasználók két lépésből álló ellenőrzése
Két indítási ellenőrzési megkövetelése a felhasználó elindításához a felhasználó állapotának módosítása letiltva értékről engedélyezve  Felhasználói államok kapcsolatos további tudnivalókért olvassa el a [Felhasználó államok Azure többtényezős hitelesítés](multi-factor-authentication-get-started-user-states.md) című témakört.

Az alábbi eljárással MFA engedélyezése a felhasználók számára.

### <a name="to-turn-on-multi-factor-authentication"></a>Többtényezős hitelesítés bekapcsolása

1.  Jelentkezzen be rendszergazdaként az [Azure klasszikus portálon](https://manage.windowsazure.com) .
2.  A bal oldalon kattintson **Az Active Directory**.
3.  Címtár csoportban jelölje ki az engedélyezni kívánt felhasználó a címtárban.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  A képernyő tetején kattintson a **felhasználók**elemre.
5.  A lap alján kattintson a **Többtényezős hitelesítés kezelése**gombra. Ekkor megnyílik egy új böngészőlapon.
![Kattintson a címtár](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Keresse meg a felhasználót, hogy a két lépésből álló tartomány ellenőrzéséhez engedélyezni kívánt. Előfordulhat, hogy a nézet tetején módosítása. Győződjön meg arról, hogy az Állapot oszlop értéke **Letiltva.** 
 ![Engedélyezése a felhasználó](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Ellenőrizze,** jelölje be a nevük mellett.
7.  A jobb oldalon kattintson az **Engedélyezés**parancsra.
![Felhasználó engedélyezése](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Kattintson az **Engedélyezés többtényezős hitelesítés**parancsra.
![Felhasználó engedélyezése](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Figyelje meg, hogy a felhasználó állapot megváltozott a **letiltott** **engedélyezve van**.
![Engedélyezése a felhasználóknak](./media/multi-factor-authentication-get-started-cloud/user.png)

Miután engedélyezte a felhasználók számára, akkor tájékoztassa azokat mailben. A következő alkalommal, amikor megpróbál bejelentkezni, azok kéri a fiók használatához kétlépcsős hitelesítési beléptetéséhez. Miután használatbavételét kétlépcsős hitelesítési, azok is kell alkalmazás jelszavak beállítása, ki nem böngésző alkalmazások zárolva elkerülése érdekében.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>A PowerShell használatával automatizálhatja a kétlépcsős hitelesítési bekapcsolása

[Azure Active Directory PowerShell](../powershell-install-configure.md)használatával az [állapot](multi-factor-authentication-whats-next.md) módosításához az alábbi is használhatja.  Módosíthatja `$st.State` egyenlő, az alábbi állapotok egyikét:

- Engedélyezve van
- Vannak érvényben
- Letiltott  

> [AZURE.IMPORTANT]  Azt kívánó felhasználókat ellen áthelyezése a felhasználók közvetlenül a letiltott állapotát érvénybe léptetve állapotát. Nem-böngészőalapú alkalmazásokat fog működni, mert a felhasználó nem MFA regisztrációs között megszűnt, valamint az [alkalmazás jelszó](multi-factor-authentication-whats-next.md#app-passwords)kapott. Ha nem-böngészőalapú alkalmazásokat, és alkalmazás jelszó megadását kéri, azt javasoljuk, hogy megnyitja a letiltott állapotba kerül engedélyezve. Így a felhasználók regisztrálhat, és szerezze be az alkalmazás jelszavukat. Ezt követően áthelyezheti őket érvénybe léptetve.

A PowerShell használatával lenne engedélyezése a felhasználók tömeges beállítást. Jelenleg nincs tömeges enable funkció van az Azure-portálra, és válassza a minden felhasználó külön-külön kell. Ez lehet igazán egy tevékenység, sok felhasználó. Egy PowerShell létrehozásával parancsfájl használatával az alábbi, ismétlése a felhasználók listáját, és engedélyezi őket.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Lássunk egy példát:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


További tudnivalókért lásd: [felhasználó államok Azure többtényezős hitelesítés](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Következő lépések
Most, hogy állított be Azure többtényezős hitelesítés a felhőben, állítsa be, és állítsa be a üzembe. További részletekért lásd: az [Azure többtényezős hitelesítés beállítása](multi-factor-authentication-whats-next.md) .
