<properties
   pageTitle="Hogy miként adhat hozzáférést a személyes Információkezelő |} Microsoft Azure"
   description="Megtudhatja, hogy miként adhat hozzá az Azure Active Directory Yammerhez Identitáskezelés kiterjesztéssel rendelkező felhasználók szerepkört, azok is kezelheti a személyes Információkezelő."
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Hogyan kell adni a hozzáférést az Identitáskezelés Azure Active Directory Yammerhez kezelése

Automatikusan lehetővé teszi, hogy egy szervezet Azure Active Directory jogosultsággal rendelkező Identitáskezelés (személyes Információkezelő) globális rendszergazdának szerepkör-hozzárendelések és a személyes Információkezelő való hozzáférést kap. Más kap írási alapértelmezés szerint azonban, beleértve a más globális rendszergazda. Más globális rendszergazdák, a biztonsági rendszergazdák és a biztonsági olvasók hozzáférést csak olvasható Azure Active Directory személyes Információkezelő. Személyes Információkezelő adhat hozzáférést, az első felhasználó oszthatnak mások a **Yammerhez szerepkör rendszergazdai** szerepkört. A hozzárendelés belül kell elvégezni a személyes Információkezelő magát, és nem módosíthatók PowerShell vagy más portálokról keresztül.

> [AZURE.NOTE] Azure MFA Azure Active Directory személyes Információkezelő kezeléséhez szükséges. Microsoft-fiókok nem tudja regisztrálni Azure többtényezős, mivel a felhasználó, aki be egy Microsoft-fiókjával bejelentkezik az Azure Active Directory személyes Információkezelő nem tud hozzáférni.

Győződjön meg arról, hogy mindig legalább két felhasználókat a Yammerhez szerepkör rendszergazdai szerepkör abban az esetben, ha egy felhasználó zárolva van, vagy a fiók törlődik.

## <a name="give-another-user-access-to-manage-pim"></a>Adjon meg egy másik felhasználó hozzáférését kezelheti a személyes Információkezelő

1. Jelentkezzen be az [Azure-portálra](https://portal.azure.com/) , és válassza ki az **Azure Active Directory jogosultsággal rendelkező Identitáskezelés** alkalmazást az irányítópulton.
2. Jelölje ki a **kiemelt szerepkörök kezeléséhez** > **rendszergazda jogosultsággal rendelkező szerepkör** > **Hozzáadás gombra**.

    ![Adja hozzá a Yammerhez szerepkör rendszergazdák - képernyőképe][1]

4. Kattintson a Hozzáadás a felügyelt felhasználók lap lépés: 1 már befejeződött. Jelölje ki a lépés: 2, **Jelölje ki a felhasználókat** és a hozzáadni kívánt felhasználó keresése.

    ![Válassza a felhasználók – kép][2]

6. Jelölje ki a felhasználót a találatok között, és kattintson a **kész**gombra.
7. Kattintson **az OK gombra** a módosítások mentéséhez. A kijelölt felhasználó megjelennek a Yammerhez szerepkör rendszergazda listáját.

    - Amikor valaki kioszt egy új szerepkört, automatikusan létrehozásuk jogosult, a szerepkör aktiválásához. Ha azt szeretné, hogy a szerepkör a állandó, kattintson a felhasználó a listában. Jelölje ki a **perm ellenőrizze** a felhasználói adatok menüben.

8. A felhasználó mutató hivatkozás küldése [az Azure Active Directory Yammerhez Identitáskezelés az első lépések](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Egy másik felhasználó jogokkal a személyes Információkezelő kezelésére szolgáló eltávolítása

Mielőtt eltávolít valakit a Yammerhez szerepkör rendszergazdai szerepkörből, ügyeljen arra, hogy továbbra sem lesznek rendelt két felhasználót.

1. A személyes Információkezelő irányítópult kattintson a **védett szerepkör rendszergazdai**szerepkört.  A felhasználók az adott szerepkörhöz aktuálisan listáját jelennek meg.
2. Kattintson a felhasználó a felhasználók listáját.
3. Kattintson a **eltávolítása**.  Egy megerősítést kérő üzenet jelenik meg.
4. Kattintson az **Igen gombra** a szerepkör a felhasználó eltávolítása.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
