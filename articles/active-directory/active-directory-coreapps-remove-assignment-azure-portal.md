<properties
    pageTitle="Vállalati alkalmazás Azure Active Directory-ban egy felhasználó vagy csoport hozzárendelés eltávolítása |} Microsoft Azure"
    description="Az access-hozzárendelés egy felhasználó vagy csoport eltávolítása az Azure Active Directory vállalati alkalmazás"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Vállalati alkalmazás Azure Active Directory-ban egy felhasználó vagy csoport-hozzárendelés eltávolítása

Nagyon egyszerűen, ha el szeretne távolítani egy felhasználó vagy csoport hozzáférési hozzárendeli az Azure Active Directory (Azure Active Directory) előzetes verzióban a vállalati alkalmazások egyik. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) A vállalati app kezelése a megfelelő engedélyekkel kell rendelkeznie. A jelenlegi előzetes verzióban a könyvtár globális rendszergazdának kell lennie.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hogyan távolíthatom el a felhasználó vagy csoport hozzárendelés?

1. Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2. Jelölje ki a **További szolgáltatások**, adja meg az **Azure Active Directory** a szövegmezőbe, és válassza az **ENTER billentyűt**.

3. Kattintson az * *Azure Active Directory - *könyvtárnév* ** lap (Ez azt jelenti, hogy az Azure Active Directory lap a tartománya könyvtár), válassza a **vállalati alkalmazások **.

    ![Nyitó vállalati alkalmazások](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Válassza a **vállalati alkalmazások** lap az **összes alkalmazás**. Kezelheti az alkalmazások listájának láthatja.

5. A **vállalati alkalmazások – az összes alkalmazás** lap jelölje be az alkalmazás.

6. A ***Alkalmazásnév*** lap (Ez azt jelenti, hogy a lap a kijelölt alkalmazás, a cím a nevet) válassza a **felhasználók és csoportok**lehetőséget.

    ![Felhasználók vagy csoportok kiválasztása](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Kattintson a ***Alkalmazásnév*** **-felhasználó és a csoport-hozzárendelés** lap, jelöljön ki egy további felhasználók vagy csoportok, és válassza az **Eltávolítás** parancsot. A erősítse meg a parancssorba.

    ![Válassza az Eltávolítás parancs](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Következő lépések

- [Látható a teljes saját csoportok](active-directory-groups-view-azure-portal.md)
- [Felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](active-directory-coreapps-assign-user-azure-portal.md)
- [Felhasználói bejelentkezések-vállalati alkalmazás letiltása](active-directory-coreapps-disable-app-azure-portal.md)
- [A név vagy a vállalati alkalmazás emblémájának módosítása](active-directory-coreapps-change-app-logo-user-azure-portal.md)
