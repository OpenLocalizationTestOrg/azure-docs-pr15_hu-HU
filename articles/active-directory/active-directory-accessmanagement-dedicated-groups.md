<properties
    pageTitle="Csoportok az Azure Active Directory dedikált |} Microsoft Azure"
    description="Hogyan saját csoportok áttekintése az Azure Active Directory és a létrehozott hogyan működnek."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="dedicated-groups-in-azure-active-directory"></a>Az Azure Active Directory saját csoportok

Az Azure Active Directory (Azure Active Directory) a saját csoportok funkció automatikusan hoz létre és az előre definiált Azure Active Directory-csoportok tagságának kitölt. Saját csoportok tagjai nem lehet hozzáadni vagy eltávolítani az Azure klasszikus portál, a Windows PowerShell-parancsmagok használata vagy programozás útján.

>[AZURE.NOTE] Saját csoportok megkövetelése, hogy be van-e rendelve egy Azure Active Directory prémium licenc
>- a rendszergazda, aki a szabály csoport kezelése
>- a felhasználók, akik ki van jelölve a szabály a csoport tagjának kell

**Saját csoportok engedélyezése**

1. Az [Azure klasszikus portálon](https://manage.windowsazure.com)válassza ki **Az Active Directory**, és nyissa meg a szervezet címtárára.

2. Kattintson a **csoportok** fülre, és nyissa meg a szerkeszteni kívánt csoportot.

3. A **beállítás** lapon jelölje ki, és a **Dedikált csoportok engedélyezése** állítsa **Igen**értékre.

Miután a Váltás a saját csoportok engedélyezése **Igen**értékre van állítva, a könyvtár automatikus létrehozása az All Users dedikált csoportban állítsa további engedélyezheti a **engedélyezése a "Minden felhasználónak" csoport** **Igen**váltani. Ezután is szerkesztheti a kijelölt csoport nevét, írja be azt a **megjelenítendő neve "Minden felhasználónak" csoport** mezőben.

Az összes felhasználó csoport ugyanolyan engedélyekkel hozzárendelése a címtárában található összes felhasználót használható. Például adhat az összes felhasználó a szoftver alkalmazáshoz a directory Access alkalmazás az összes felhasználó dedikált csoport hozzáférési hozzárendelésével.

Az All Users dedikált csoportban minden felhasználó a címtárban, beleértve a vendégek és a külső felhasználók is tartalmaz. Ha szüksége van egy csoport, amely nem tartalmazza a külső felhasználók, majd a csoport létrehozása az attribútum alapuló dinamikus szabállyal például a következő szerint elvégezhető:

                (user.userPrincipalName -notContains "#EXT#@")

Egy csoport, hogy kizárja az összes vendégek például a következő szabályok használja:

                (user.userType -ne "Guest")

Arról, hogy miként dinamikus csoporttagság *Speciális* szabályok (szabályokat, amely tartalmazhat több összehasonlítása) létrehozása című témakörben talál [használata attribútumok speciális szabályok létrehozásához](active-directory-accessmanagement-groups-with-advanced-rules.md).


Ezek a cikkek Azure Active Directory további tájékoztatást nyújt.

* [Erőforrások való hozzáférés kezelése az Azure Active Directory-csoportok](active-directory-manage-groups.md)
* [A cikk az Azure Active Directory Alkalmazáskezelés Index](active-directory-apps-index.md)
* [Mi az Azure Active Directory?](active-directory-whatis.md)
* [A helyszíni identitások integrálása az Azure Active Directory](active-directory-aadconnect.md)
