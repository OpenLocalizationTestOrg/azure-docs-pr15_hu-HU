<properties
    pageTitle="Tiltsa le a felhasználói bejelentkezések előnézetben Azure Active Directory-vállalati alkalmazáshoz |} Microsoft Azure"
    description="Vállalati alkalmazás letiltása, így a felhasználók nem előfordulhat, hogy jelentkezzen be azt az Azure Active Directory"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Tiltsa le a felhasználói bejelentkezések előnézetben Azure Active Directory-vállalati alkalmazáshoz

Akkor is, hogy a felhasználók nem előfordulhat, hogy jelentkezzen be azt az Azure Active Directory (Azure Active Directory) előzetes verzió letiltása a vállalati alkalmazás könnyen. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) A vállalati app kezelése a megfelelő engedélyekkel kell rendelkeznie. A jelenlegi előzetes verzióban a könyvtár globális rendszergazdának kell lennie.

## <a name="how-do-i-disable-user-sign-ins"></a>Hogyan tilthatom le a felhasználó bejelentkezési bővítmények?

1. Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2. Jelölje ki a **További szolgáltatások**, adja meg az **Azure Active Directory** a szövegmezőbe, és válassza az **ENTER billentyűt**.

3. Kattintson az * *Azure Active Directory - *könyvtárnév* ** lap (Ez azt jelenti, hogy az Azure Active Directory lap a tartománya könyvtár), válassza a **vállalati alkalmazások **.

    ![Nyitó vállalati alkalmazások](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Válassza a **vállalati alkalmazások** lap az **összes alkalmazás**. Kezelheti az alkalmazások listájának megtekintése

5. A **vállalati alkalmazások – az összes alkalmazás** lap jelölje be az alkalmazás.

6. A ***Alkalmazásnév*** lap (Ez azt jelenti, hogy a lap a kijelölt alkalmazás, a cím a nevet) válassza a **Tulajdonságok parancsot**.

    ![Jelölje ki az összes alkalmazások parancsot.](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Kattintson a ***Alkalmazásnév*** **-Tulajdonságok** lap, jelölje be **nem** az **engedélyezése a felhasználóknak, hogy jelentkezzen be?**.

8. Válassza a **Mentés** parancsot.

## <a name="next-steps"></a>Következő lépések

- [Az összes csoport megjelenítéséhez](active-directory-groups-view-azure-portal.md)
- [Felhasználó vagy csoport hozzárendelése egy vállalati alkalmazás](active-directory-coreapps-assign-user-azure-portal.md)
- [Vállalati alkalmazás a felhasználó vagy csoport-hozzárendelés eltávolítása](active-directory-coreapps-remove-assignment-azure-portal.md)
- [A név vagy a vállalati alkalmazás emblémájának módosítása](active-directory-coreapps-change-app-logo-user-azure-portal.md)
