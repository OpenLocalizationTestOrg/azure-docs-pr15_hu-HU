<properties
    pageTitle="Azure Active Directory-ban a rendszergazdai szerepkörök hozzárendelése egy felhasználói |} Microsoft Azure"
    description="Megtudhatja, hogyan módosíthatja a felhasználói rendszergazdai adatok az Azure Active Directory"
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
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>A felhasználó Azure Active Directory-ban a rendszergazdai szerepkörök hozzárendelése

Ez a cikk ismerteti, hogyan-rendszergazdai szerepkör hozzárendelése egy felhasználóhoz az Azure Active Directory (Azure Active Directory) előzetes verzió. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Új felhasználók hozzáadása a szervezet tudni olvassa el az [Új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md)című témakört. Felvett felhasználók alapértelmezés szerint nem rendelkezik rendszergazdai jogosultságokkal, de a szerepköröket rendelhet hozzájuk bármikor.

## <a name="assign-a-role-to-a-user"></a>A szerepkör hozzárendelése egy felhasználóhoz

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  A **felhasználók és csoportok** lap válassza a **minden felhasználónak**.

    ![Az összes felhasználó lap megnyitása](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. A **felhasználók és csoportok – az összes felhasználó** lap válassza ki azt a felhasználót a listából.

5. A kijelölt felhasználó lap jelölje ki **a címtár szerepkört**, és hozzárendelheti a felhasználó a szerepkör a **címtár-szerepkör** listából. Felhasználó és a rendszergazdai szerepkörök kapcsolatos további tudnivalókért olvassa el a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md)című témakört.

      ![A felhasználó számára oszt ki szerepkör](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Válassza a **Mentés**.


## <a name="whats-next"></a>Következő lépések

- [Felhasználó hozzáadása](active-directory-users-create-azure-portal.md)
- [Az új Azure portálon egy felhasználói jelszó alaphelyzetbe állítása](active-directory-users-reset-password-azure-portal.md)
- [A felhasználó adatainak módosítása](active-directory-users-work-info-azure-portal.md)
- [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
- [Felhasználó törlése a Azure Active Directory](active-directory-users-delete-user-azure-portal.md)
