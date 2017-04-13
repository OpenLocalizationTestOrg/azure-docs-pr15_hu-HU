<properties
    pageTitle="Az Azure Active Directory-ban egy csoport tagjainak kezelése |} Microsoft Azure"
    description="Felhasználók és eszközök, amelyek az Azure Active Directory csoport tagjainak"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Az Azure Active Directory-ban egy csoport tagjainak kezelése

Ez a cikk ismerteti, hogyan kezelheti a tagok csoport Azure Active Directory (Azure Active Directory) előzetes verzióban. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Hogyan keresheti meg a tagokat és kezelheti azokat?

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

  ![Nyitó felhasználók kezelése](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  A **felhasználók és csoportok** lap jelölje be **az összes csoport**.

  ![A csoportok lap megnyitása](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. A **felhasználók és csoportok – az összes** lap válasszon ki egy csoportot.

5. Kattintson a * *csoport - *csoportnév* ** lap, válassza **a tagok **.

  ![A tagok lap megnyitása](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Tagok hozzáadása csoportjában kattintson a **csoport – a tagok** a lap, válassza a **Tagok hozzáadása**lehetőséget.

  ![A tagok parancs hozzáadása](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Válassza a **tagok** lap egy vagy több felhasználót vagy eszközök vegye fel a csoportba, és válassza a **Kijelölés** gombot a lap alján kattintva vegye fel őt a csoportba. A **felhasználó** mezőben a megjelenítést, a felhasználó és eszköz nevét bármely részét a bejegyzés egyező alapján szűri. Nincs helyettesítő karakterek használhatók a párbeszédpanelen.

8. Ha szeretne tagokat eltávolítani a csoportból, kattintson a **csoport – a tagok** lap jelöljön ki egy tagot.

9. A ***tagnév*** lap válassza az **Eltávolítás** parancsot, és erősítse meg a parancssorba.

  ![a tagok parancs eltávolítása](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Amikor befejezte a tagok csoport módosítását, válassza a **Mentés**.


## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [A meglévő csoportok megtekintése](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot, és a tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [A beállítások csoport kezelése](active-directory-groups-settings-azure-portal.md)
* [A csoport tagságát kezelése](active-directory-groups-membership-azure-portal.md)
* [A csoport felhasználóknak dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
