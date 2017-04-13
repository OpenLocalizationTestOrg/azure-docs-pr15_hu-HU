
<properties
    pageTitle="Következő lépések az access felügyeleti csoportokkal |} Microsoft Azure"
    description="Hogyan speciális – a hangpostáján a biztonsági csoportokat és ezekhez a csoportokhoz használata egy erőforráshoz való hozzáférés kezelése kezelése."
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
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="managing-owners-for-a-group"></a>A tulajdonosok csoport kezelése
Miután egy erőforrást tulajdonosa access van rendelve egy erőforrás az Azure Active Directory-csoportnak, a csoport tulajdonosának kezeli a csoport tagságát. Az erőforrás tulajdonosa hatékony delegálja jogosult felhasználók hozzárendelése az erőforrás a csoportnak a tulajdonosa.

## <a name="assigning-group-ownership"></a>Tulajdonjogának csoport

**Tulajdonos hozzáadása egy csoporthoz**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és nyissa meg a szervezet címtárára.

2. Kattintson a **csoportok** fülre, és nyissa meg a csoportot, amelyhez fel szeretné venni a tulajdonosok.

3. Jelölje ki a **tulajdonosok hozzáadása**.

4. A **tulajdonosok hozzáadása** lapon válassza azt a felhasználót, hogy vegye fel a csoport tulajdonosát, és győződjön meg arról, hogy ez a név megjelenik a **kijelölt** ablak.


**Tulajdonos eltávolításához csoportból**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és nyissa meg a szervezet címtárára.

2. Kattintson a **csoportok** fülre, és nyissa meg a csoportot, amelyhez el szeretné távolítani a tulajdonos.

4. Kattintson a **tulajdonosok** fülre.

5. Válassza ki a csoportból eltávolítani kívánt tulajdonosát, és válassza a **eltávolítása**.

## <a name="additional-information"></a>További információk

Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagjai csoport beállításainak konfigurálása](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
* [Mi az Azure Active Directory?](active-directory-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
