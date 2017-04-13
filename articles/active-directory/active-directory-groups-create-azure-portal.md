<properties
    pageTitle="Az új csoport létrehozása az Azure Active Directory-ban |} Microsoft Azure"
    description="Csoport létrehozása az Azure Active Directory és a felhasználók (tagok) hozzáadása a csoporthoz"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Az új csoport létrehozása az Azure Active Directory-ban

> [AZURE.SELECTOR]
- [Azure portál](active-directory-groups-create-azure-portal.md)
- [Azure klasszikus portál](active-directory-accessmanagement-manage-groups.md)
- [A PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Ez a cikk ismerteti, hogyan hozhat létre és feltöltése egy új csoportot az Azure Active Directory (Azure Active Directory) előzetes verzióban. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Egy csoport segítségével felügyeleti feladatokat, például a licencek vagy az engedélyek felhasználók vagy eszközök számú hozzárendelése egyszerre.

## <a name="how-do-i-create-a-group"></a>Hogyan hozhatok létre csoportot?

1. Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2. Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

  ![Nyitó felhasználók kezelése](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. A **felhasználók és csoportok** lap jelölje be **az összes csoport**.

  ![A csoportok lap megnyitása](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. A **felhasználók és csoportok – az összes** lap válassza a **Hozzáadás** parancsot.

  ![Az Add parancs kiválasztása](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Adja meg a **csoport** lap nevét és leírását a csoport szimbólumára.

6. Jelölje be a tagok hozzáadása a csoporthoz, a **Tagság típusa** mezőben válassza ki a **hozzárendelt** , és válassza a **tagok**. Dinamikusan kezelése csoport tagságát kapcsolatos további tudnivalókért olvassa el a [használata attribútumok csoporttagság speciális szabályok létrehozása](active-directory-groups-dynamic-membership-azure-portal.md)című témakört.

  ![Jelölje ki a tagok hozzáadása](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Válassza a **tagok** lap egy vagy több felhasználót vagy eszközök vegye fel a csoportba, és válassza a **Kijelölés** gombot a lap alján kattintva vegye fel őt a csoportba. A **felhasználó** mezőben a megjelenítést, a felhasználó és eszköz nevét bármely részét a bejegyzés egyező alapján szűri. Nincs helyettesítő karakterek használhatók a párbeszédpanelen.

6. Ha befejezte a tagok hozzáadása csoporthoz, kattintson a **Létrehozás** a **csoport** lap.    

  ![Megerősítő csoport létrehozása](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [A meglévő csoportok megtekintése](active-directory-groups-view-azure-portal.md)
* [A beállítások csoport kezelése](active-directory-groups-settings-azure-portal.md)
* [Csoport tagjainak kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport felhasználóknak dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
