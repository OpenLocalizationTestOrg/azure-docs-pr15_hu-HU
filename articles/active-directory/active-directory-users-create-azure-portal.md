<properties
    pageTitle="Új felhasználók felvétele az Azure Active Directory előzetes verzió |} Microsoft Azure"
    description="Új felhasználók hozzáadása vagy módosítása a felhasználói adatok az Azure Active Directory ismerteti."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Új felhasználók felvétele az Azure Active Directory előzetes verzió

> [AZURE.SELECTOR]
- [Azure portál](active-directory-users-create-azure-portal.md)
- [Azure klasszikus portál](active-directory-create-users.md)

Ez a cikk ismerteti, hogyan az új felhasználók a szervezet az Azure Active Direstory (Azure Active Directory) előzetes verzióban. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  A **felhasználók és csoportok** lap jelölje ki **mindegyik felhasználót**, és válassza a **Hozzáadás**gombra.

    ![Az Add parancs kiválasztása](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  A felhasználó számára, például a **név** és a **felhasználó nevét**adja meg adatait. A felhasználó nevét a tartomány részét kell lennie, az Outlook eredeti alapértelmezett tartomány neve "foo.onmicrosoft.com" tartománynevet, vagy egy igazolt, nem szövetséges tartománynevet (például "contoso.com)."

5. Másolja vagy egyéb esetben a létrehozott felhasználó jelszavát figyelje meg, hogy a is azt a felhasználót a folyamat befejezése után.

6. Másik lehetőségként nyissa meg, és töltse ki az információkat a **profil** lap, a **csoportok** a lap vagy a felhasználó a **címtárban szerepkör** lap. Felhasználó és a rendszergazdai szerepkörök kapcsolatos további tudnivalókért olvassa el a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md)című témakört.

7.  Jelölje be a **felhasználó** lap **létrehozása**.

8. A létrehozott jelszót az új felhasználó biztonságosan terjesztése, hogy a felhasználó jelentkezhetnek be.

## <a name="whats-next"></a>Következő lépések

- [Külső felhasználók felvétele](active-directory-users-create-external-azure-portal.md)
- [Az új Azure portálon egy felhasználói jelszó alaphelyzetbe állítása](active-directory-users-reset-password-azure-portal.md)
- [A felhasználó adatainak módosítása](active-directory-users-work-info-azure-portal.md)
- [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
- [Felhasználó törlése a Azure Active Directory](active-directory-users-delete-user-azure-portal.md)
- [Felhasználó a szerepkör hozzárendelése az Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
