<properties
    pageTitle="Felhasználók felvétele az egyéb könyvtárak vagy partner cégek Azure Active Directory-ban |} Microsoft Azure"
    description="Megtudhatja, hogyan vehet fel felhasználókat, és módosíthatja a felhasználói adatok az Azure Active Directory, ideértve a Vendég és a külső felhasználókat."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Felhasználók felvétele az egyéb könyvtárak vagy partner cégek Azure Active Directory-ban

> [AZURE.SELECTOR]
- [Azure portál](active-directory-users-create-external-azure-portal.md)
- [Azure klasszikus portál](active-directory-create-users-external.md)

Ez a cikk ismerteti, hogyan vehet fel felhasználókat az Azure Active Directory (Azure Active Directory) előzetes verzió többi könyvtárak vagy partner cégek által készített. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Új felhasználók hozzáadása a szervezet, és fel a Microsoft-fiókkal rendelkező felhasználók kapcsolatos tudnivalókért lásd: [Új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md). Felvett felhasználók alapértelmezés szerint nem rendelkezik rendszergazdai jogosultságokkal, de a szerepköröket rendelhet hozzájuk bármikor.

## <a name="add-a-user"></a>Felhasználó hozzáadása

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

    ![Nyitó felhasználók kezelése](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  A **felhasználók és csoportok** lap jelölje ki a **felhasználókat**, és válassza a **Hozzáadás**gombra.

    ![Az Add parancs kiválasztása](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. A **felhasználó** a lap adja meg egy **nevet** a megjelenítendő név és a felhasználó bejelentkezési nevét a **név**.

5. Másolja vagy más módon, hogy a is azt a felhasználót a folyamat befejezése után jegyezze fel a létrehozott felhasználó jelszavát.

6. Tetszés szerint válassza a **profil** előbb adja hozzá a felhasználókat, és a Vezetéknév, beosztását, egy részleg nevét.
    
    ![A felhasználói profil megnyitása](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Jelölje ki a **csoportokat** a felhasználó hozzáadása egy vagy több csoportokhoz.

        ![Felhasználó felvétele a csoportokba](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Jelölje ki a **szervezeti szerepkör** a felhasználó hozzárendelése egy szerepkört a **szerepkörök** listából. Felhasználó és a rendszergazdai szerepkörök kapcsolatos további tudnivalókért olvassa el a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md)című témakört.

        ![A felhasználó számára oszt ki szerepkör](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Jelölje ki a **létrehozása**.

8. A létrehozott jelszót az új felhasználó biztonságosan terjesztése, hogy a felhasználó jelentkezhetnek be.

> [AZURE.IMPORTANT] Ha a szervezet egynél több tartományt is használ, kapcsolatos tudnivalók az alábbi szempontokat, amikor a felhasználói fiók felvétele:
>
> - Az ugyanaz az egyszerű felhasználóneve (UPN) a felhasználói fiókok tartományok **első** felvenni, például geoffgrisso@contoso.onmicrosoft.com, **követ** geoffgrisso@contoso.com.
> - **Ne** legyen geoffgrisso@contoso.com felvétele előtt geoffgrisso@contoso.onmicrosoft.com. Ebben a sorrendben, és lehet lehető visszavonásához.

Ha módosítja egy felhasználóhoz, akinek identitás szinkronizálja a rendszer a helyszíni Active Directory-szolgáltatás az adatokat, nem módosítható a felhasználói adatok az Azure klasszikus portálon. A felhasználói adatok módosításához használja a helyszíni Active Directory-kezelő eszközök.


## <a name="whats-next"></a>Következő lépések

- [Felhasználó hozzáadása](active-directory-users-create-azure-portal.md)
- [Az új Azure portálon egy felhasználói jelszó alaphelyzetbe állítása](active-directory-users-reset-password-azure-portal.md)
- [Felhasználó a szerepkör hozzárendelése az Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
- [A felhasználó adatainak módosítása](active-directory-users-work-info-azure-portal.md)
- [Felhasználói profilok kezelése](active-directory-users-profile-azure-portal.md)
- [Felhasználó törlése a Azure Active Directory](active-directory-users-delete-user-azure-portal.md)
