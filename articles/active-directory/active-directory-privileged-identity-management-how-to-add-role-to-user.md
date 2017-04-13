<properties
   pageTitle="Hogyan lehet hozzáadni vagy eltávolítani a felhasználói szerepkörön |} Microsoft Azure"
   description="Megtudhatja, hogy miként szerepkörök hozzáadása a Yammerhez azonosítási az Azure Active Directory Yammerhez Identitáskezelés alkalmazással."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure Active Directory Yammerhez Identitáskezelés: Hogyan lehet hozzáadni vagy eltávolítani egy felhasználói szerepkör

Az Azure Active Directory (AD), a globális rendszergazdai (vagy vállalati rendszergazdai) frissítheti mely felhasználók **véglegesen** az Azure Active Directory szerepkörökhöz rendelt. Ez a lépés PowerShell-parancsmagok például `Add-MsolRoleMember` és `Remove-MsolRoleMember`. Vagy az Azure klasszikus portal segítségével a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md)leírt módon.

Az Azure Active Directory Yammerhez Identitáskezelés alkalmazás rendszergazdák Yammerhez szerepkört, állandó szerepkör-hozzárendelés, valamint hogy. Ezenkívül Yammerhez szerepkör rendszergazdák felhasználók **jogosult** rendszergazdai szerepkörök teheti. Jogosult rendszergazda is aktiválhatja a szerepkört, amikor rá szükség, és ezután engedélyeiket járjon le, amint az általuk végzett.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Az Azure-portálra a személyes Információkezelő szerepkörök kezeléséhez

A szervezet rendelhet felhasználók eltérő rendszergazdai szerepkörök az Azure Active Directory, az Office 365 és más Microsoft szolgáltatások és alkalmazások.  A rendelkezésre álló szerepkörök rendszeren [az Azure Active Directory személyes Információkezelő szerepkörök](active-directory-privileged-identity-management-roles.md)találhatók.

Felvétele, illetve eltávolít egy felhasználót egy szerepkörben jogosultsággal rendelkező Identitáskezelés használatával jelenítse meg a személyes Információkezelő irányítópult. Kattintson a **felhasználók a rendszergazdai szerepkörök** gombra, vagy jelölje ki az adott szerepkörhöz (például a globális rendszergazda) a szerepkörök táblában.

> [AZURE.NOTE] Ha személyes Információkezelő egyelőre még nem engedélyezte az Azure-portálon, lépjen az [első lépések az Azure Active Directory személyes Információkezelő](active-directory-privileged-identity-management-getting-started.md) további információt.

Szeretne egy másik felhasználó hozzáférést adni a személyes Információkezelő magát, a szerepkörök, amely a személyes Információkezelő igényel, hogy a felhasználó esetén a, [hogy miként adhat hozzáférést a személyes Információkezelő](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)további ismerteti.

## <a name="add-a-user-to-a-role"></a>Felhasználó felvétele szerepkör

1. Az [Azure-portálon](https://portal.azure.com/)válassza az **Azure Active Directory jogosultsággal rendelkező Identitáskezelés** az irányítópulton lévő csempén.
2. Jelölje ki a **kiemelt szerepkörök kezeléséhez**.
3. Az **összefoglaló szerepkör** táblázatban jelölje ki a szerepkört, kezelni szeretné.
4. A szerepkörkezelés lap válassza a **Hozzáadás**elemre.
5. **Jelölje ki a felhasználókat** és a felhasználó a **Válassza a felhasználók** lap a Keresés gombra.  
6. Válassza ki a felhasználó a keresési eredmények listájában, és kattintson a **kész**gombra.
4. Kattintson **az OK gombra** a módosítások mentéséhez. A kijelölt felhasználó jogosult a szerepkör a listában fog megjelenni.

> [AZURE.NOTE]
>Új felhasználók egy szerepkörben alapértelmezés szerint a szerepkör csak jogosultak. Ha azt szeretné, hogy a szerepkör állandó, kattintson a felhasználó a listában. A felhasználói adatok egy új lap jelenik meg. Jelölje ki a **perm ellenőrizze** a felhasználói adatok menüben.  
>Ha a felhasználó nem tudja regisztrálni Azure többtényezős hitelesítés (MFA), vagy egy Microsoft-fiókot használ (általában @outlook.com), van szüksége, hogy az összes szerepkörének állandó. Jogosult rendszergazdák MFA regisztrálni az aktiválás során a rendszer kéri.

Most, hogy a felhasználó jogosult szerepkör szerepel, érdemes tudatnia ezt vele, hogy azok is aktiválhatja a [hogyan aktiválhatja és inaktiválhatja szerepkörbe](active-directory-privileged-identity-management-how-to-activate-role.md)útmutatása szerint.

## <a name="remove-a-user-from-a-role"></a>Felhasználó törlése a szerepkör

Felhasználók eltávolítása jogosult szerepkör-hozzárendelés, de győződjön meg arról, hogy mindig legyen legalább egy felhasználó, aki állandó globális rendszergazdának.

Kövesse ezeket a lépéseket, ha el szeretne távolítani egy adott felhasználói szerepkör:

1. Nyissa meg azt a szerepkört, a szerepkör listában jelöljön ki egy szerepkör az Azure Active Directory személyes Információkezelő irányítópult vagy a **felhasználók a rendszergazdai szerepkörök** gombra kattintva.
2. Kattintson a felhasználó a felhasználók listáját.
3. Kattintson az **Eltávolítás**gombra. Üzenet megkérdezi, hogy erősítse meg.
4. Kattintson az **Igen gombra** a szerepkör eltávolítása a felhasználó.

Ha nem biztos abban, hogy mely felhasználók van szüksége a szerepkör-hozzárendelés, majd is [egy access-ellenőrzés a szerep indítása](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
