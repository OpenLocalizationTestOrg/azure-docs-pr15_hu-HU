<properties
    pageTitle="Felhasználó vagy csoport hozzárendelése az Azure Active Directory előzetes vállalati alkalmazás |} Microsoft Azure"
    description="Vállalati alkalmazás hozzárendelése egy felhasználót vagy csoportot az Azure Active Directory kijelölése"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás Azure Active Directory-ban

Nagyon egyszerűen egy felhasználó vagy csoport hozzárendelése a vállalati alkalmazások Azure Active Directory (Azure Active Directory) előzetes verzióban. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) A vállalati app kezelése a megfelelő engedélyekkel kell rendelkeznie. A jelenlegi előzetes verzióban a könyvtár globális rendszergazdának kell lennie.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Hogyan felhasználói hozzáférés hozzárendelése egy vállalati alkalmazás?

1. Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2. Jelölje ki a **További szolgáltatások**, adja meg az Azure Active Directory a szövegmezőbe, és válassza az **ENTER billentyűt**.

3. Kattintson az * *Azure Active Directory - *könyvtárnév* ** lap (Ez azt jelenti, hogy az Azure Active Directory lap a tartománya könyvtár), válassza a **vállalati alkalmazások **.

    ![Nyitó vállalati alkalmazások](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Válassza a **vállalati alkalmazások** lap az **összes alkalmazás**. Kezelheti az alkalmazások listájának láthatja.

5. A **vállalati alkalmazások – az összes alkalmazás** lap jelölje be az alkalmazás.

6. A ***Alkalmazásnév*** lap (Ez azt jelenti, hogy a lap a kijelölt alkalmazás, a cím a nevet) válassza a **felhasználók és csoportok**lehetőséget.

    ![Jelölje ki az összes alkalmazások parancsot.](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Kattintson a ***Alkalmazásnév*** **-felhasználó és a csoport-hozzárendelés** lap, válassza a **Hozzáadás** parancsot.

8. Válassza a **Hozzárendelés hozzáadása** lap **felhasználók és csoportok ablakot**.

    ![Az alkalmazás a felhasználó vagy csoport hozzárendelése](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. A **felhasználók és csoportok** lap jelölje ki egy vagy több felhasználót vagy csoportot a listában, és válassza a **Kijelölés** gombot a lap alján.

10. Jelölje be a **Hozzárendelés hozzáadása** lap **szerepkör**. A **Szerepkör jelölje be** a lap, kattintson a alkalmazása a kijelölt felhasználóknak vagy csoportoknak, egy szerepkört, és válassza a a lap alján az **OK** gombra.

11. Válassza a **Hozzárendelés hozzáadása** lap a **hozzárendelése** gomb a lap alján. A kijelölt felhasználóknak vagy csoportoknak a vállalati alkalmazás a kijelölt szerepkör által meghatározott engedélyekkel fog rendelkezni.

## <a name="next-steps"></a>Következő lépések

- [Látható a teljes saját csoportok](active-directory-groups-view-azure-portal.md)
- [Vállalati alkalmazás a felhasználó vagy csoport-hozzárendelés eltávolítása](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Felhasználói bejelentkezések-vállalati alkalmazás letiltása](active-directory-coreapps-disable-app-azure-portal.md)
- [A név vagy a vállalati alkalmazás emblémájának módosítása](active-directory-coreapps-change-app-logo-user-azure-portal.md)
