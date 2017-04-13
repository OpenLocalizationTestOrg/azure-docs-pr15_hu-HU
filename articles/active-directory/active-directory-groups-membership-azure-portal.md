<properties
    pageTitle="A csoport tagja az Azure Active Directory megtekintése a csoportok kezelése |} Microsoft Azure"
    description="Csoportok az Azure Active Directory más csoportot is tartalmazhat. Az alábbiakban ezeket a csoporttagság kezelése."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>A csoport tagja az Azure Active Directory megtekintése a csoportok kezelése

Csoportok az Azure Active Directory előzetes más csoportok tartalmazhatnak. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md) Az alábbiakban ezeket a csoporttagság kezelése.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Hogyan találhatom meg a csoportok, a csoport tagja?

1.  Jelentkezzen be az [Azure portál](https://portal.azure.com) , amely a könyvtár egy globális rendszergazdai fiókkal.

2.  Jelölje ki a **További szolgáltatások**, adja meg a **felhasználók és csoportok** a szövegmezőbe, és válassza az **ENTER billentyűt**.

  ![Nyitó felhasználók kezelése](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  A **felhasználók és csoportok** lap jelölje be **az összes csoport**.

  ![A csoportok lap megnyitása](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. A **felhasználók és csoportok – az összes** lap válasszon ki egy csoportot.

5. A * *csoport – *csoportnév* ** lap, válassza **a csoport tagságát **.

  ![A csoport tagságát lap megnyitása](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. A csoport hozzáadása egy másik csoportjában kattintson a **csoport – csoporttagság** lap tagjaként válassza a **Hozzáadás** parancsot.

7. A **Csoport kijelölése** lap jelöljön ki egy csoportot, és válassza a **Kijelölés** gombot a lap alján. Csak egy csoportba a csoport vehet egy időben. A **felhasználó** mezőben a megjelenítést, a felhasználó és eszköz nevét bármely részét a bejegyzés egyező alapján szűri. Nincs helyettesítő karakterek használhatók a párbeszédpanelen.

  ![A csoporttagság hozzáadása](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Ha el szeretné távolítani a csoport egy másik csoportjában kattintson a **csoport – csoporttagság** lap tagjaként jelöljön ki egy csoportot.

9. A ***csoportnév*** lap válassza az **Eltávolítás** parancsot, és erősítse meg a parancssorba.

  ![tagsági parancs eltávolítása](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Amikor elkészült, a csoport csoporttagság módosítása, válassza a **Mentés**.


## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [A meglévő csoportok megtekintése](active-directory-groups-view-azure-portal.md)
* [Hozzon létre egy új csoportot, és a tagok hozzáadása](active-directory-groups-create-azure-portal.md)
* [A beállítások csoport kezelése](active-directory-groups-settings-azure-portal.md)
* [Csoport tagjainak kezelése](active-directory-groups-members-azure-portal.md)
* [A csoport felhasználóknak dinamikus szabályok kezelése](active-directory-groups-dynamic-membership-azure-portal.md)
