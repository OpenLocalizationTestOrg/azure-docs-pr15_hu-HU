<properties
    pageTitle="Azure Active Directory tartományi szolgáltatások: Jelszó-szinkronizálás engedélyezése |} Microsoft Azure"
    description="Azure Active Directory tartományi szolgáltatások – első lépések"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure Active Directory tartományi szolgáltatások jelszó-szinkronizálás engedélyezése
Megelőző tevékenységek, az Azure Active Directory tartományi szolgáltatások az Azure Active Directory-bérlői webhelyen engedélyezett. Ahhoz, hogy és Azure Active Directory tartományi szolgáltatások szinkronizálása a NTLM és Kerberos-hitelesítés szükséges hitelesítő adatok tiltva, akkor a következő feladatot. Miután hitelesítőadat-szinkronizálás be van állítva, felhasználók is jelentkezzen be a felügyelt tartomány a vállalati hitelesítő adataival.

Lépései eltérőek alapján, hogy a szervezet van-e a felhőben Azure Active Directory bérlői vagy értéke szinkronizál a rendszer a helyszíni címtárában Azure AD Connect használatával.

<br>

> [AZURE.SELECTOR]
- [Felhőben Azure AD-bérlői](active-directory-ds-getting-started-password-sync.md)
- [Azure AD-bérlő szinkronizált](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Feladat 5: Engedélyezi a jelszó-szinkronizálás AAD tartományi szolgáltatásokhoz való a felhőben Azure AD-bérlő
Azure Active Directory tartományi szolgáltatások van szüksége a hitelesítő adatok NTLM és Kerberos-hitelesítés, a felhasználók a felügyelt tartomány hitelesítése megfelelő formátumban tiltva. Kivéve, ha engedélyezi a bérlő AAD tartományi szolgáltatások, Azure Active Directory nem készítése és tárolni a hitelesítő adatok tiltva az NTLM vagy Kerberos-hitelesítés szükséges formátumban. Egyértelmű biztonsági okokból Azure Active Directory még nem minden hitelesítő adatainak tárolása egyszerű szöveges formátumban. Azure Active Directory emiatt nem rendelkezik ezek a felhasználók meglévő hitelesítő adatai alapján NTLM vagy Kerberos hitelesítő adatok tiltva készítése lehetőséget.

> [AZURE.NOTE] Ha szervezete egy felhőalapú csak Azure AD-bérlői, felhasználóknál, akik az Azure Active Directory tartományi szolgáltatások használatához szükséges módosítania kell a jelszavukat.

A jelszó módosítása folyamat hatására a hitelesítő adatok követel meg az Azure Active Directory tartományi szolgáltatásokban Kerberos és NTLM az Azure Active Directory jöjjön tiltva. Az összes felhasználó számára a bérlőhöz, használja az Azure Active Directory tartományi szolgáltatások, vagy kérje ezeket a felhasználókat, hogy a jelszavukat kell jelszavak járjon vagy le.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>NTLM és Kerberos hitelesítő adatok kivonat létrehozásának engedélyezése a a felhőben Azure AD-bérlő
Az alábbiakban útmutatást, meg kell adnia a felhasználóknak, hogy módosíthatja a jelszavát:

1. Nyissa meg az Azure Active Directory Access Panel lapján [http://myapps.microsoft.com](http://myapps.microsoft.com)a szervezete számára.

2. Jelölje be az ezen a lapon a **profil** fülre.

3. Kattintson a **jelszó módosítása** csempére ezen a lapon.

    ![Hozzon létre egy virtuális hálózati Azure Active Directory tartományi szolgáltatások.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Ha nem látható a **jelszó módosítása** lehetőséget az Access Panel lapján, győződjön meg arról, hogy a szervezete úgy állította be a [jelszó-kezelés az Azure Active Directory](../active-directory/active-directory-passwords-getting-started.md).

4. A **jelszó módosítása** lapon írja be a meglévő (régi) jelszavát, és írjon be egy új jelszót, majd erősítse meg. Kattintson a **Küldés**gombra.

    ![Hozzon létre egy virtuális hálózati Azure Active Directory tartományi szolgáltatások.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Miután módosította a jelszavát, az új jelszót hamarosan Azure Active Directory tartományi szolgáltatások felhasználható. Néhány perc múlva (általában körülbelül 20 perc), jelentkezzen a tartományhoz felügyelt az újonnan módosított jelszót használja-számítógépek.

<br>

## <a name="related-content"></a>Kapcsolódó tartalom

- [A saját jelszó frissítése](../active-directory/active-directory-passwords-update-your-own-password.md)

- A [jelszó-kezelés az Azure Active Directory – első lépések](../active-directory/active-directory-passwords-getting-started.md).

- [Engedélyezése a jelszó-szinkronizálás AAD tartományi szolgáltatásokhoz való egy szinkronizált Azure AD-bérlő](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Az Azure Active Directory tartományi szolgáltatások felügyelt tartomány felügyelete](active-directory-ds-admin-guide-administer-domain.md)

- [A Windows virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-windows-vm.md)

- [Piros kalap vállalati Linux virtuális gép vegye fel az Azure Active Directory tartományi szolgáltatások felügyelt tartományba](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
