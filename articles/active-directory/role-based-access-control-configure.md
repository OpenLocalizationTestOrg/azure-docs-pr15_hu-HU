<properties
    pageTitle="Szerepköralapú hozzáférés-vezérlés használata az Azure-portálon |} Microsoft Azure"
    description="Első lépések az access-kezelés szerepköralapú hozzáférés-vezérlés az Azure-portálon. Szerepkör-hozzárendelés segítségével az erőforrások engedélyeket társíthat."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Szerepkör-hozzárendelések az Azure előfizetés erőforrások való hozzáférés kezelése

> [AZURE.SELECTOR]
- [Access-felhasználó vagy csoport kezelése](role-based-access-control-manage-assignments.md)
- [Erőforrás szerint hozzáférés kezelése](role-based-access-control-configure.md)

Azure szerepköralapú hozzáférés vezérlő (RBAC) lehetővé teszi, hogy az Azure külön hozzáférés kezelése. RBAC használ, adhat csak access összegét, hogy a felhasználók munkájuk elvégzéséhez szükséges. Ez a cikk segít a kezdeti lépéseket az RBAC az Azure-portálon. Ha többet szeretne tudni, hogy miként RBAC segít hozzáférés kezelése, ismerje meg, [a szerepköralapú hozzáférés-vezérlés az](role-based-access-control-what-is.md).

## <a name="view-access"></a>Access nézet
Láthatja, hogy ki fér hozzá egy erőforrás, erőforráscsoport, vagy -előfizetésre a saját fő lap az [Azure-portálon](https://portal.azure.com). Ha például azt szeretné, hogy ki fér hozzá az erőforráscsoport egyikét:

1. Jelölje ki **az erőforrás csoportok** a bal oldali navigációs sávon.  
    ![Az erőforrás-csoportok - ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Válassza ki az erőforráscsoport a nevét az **erőforrás csoportoknak** lap.
3. A bal oldali menüben válassza a **hozzáférés-vezérlés (IAM)** .  
4. Az Access a vezérlő lap összes felhasználók, csoportok és rendelkezik hozzáféréssel a erőforrás csoportba alkalmazások listája.  

    ![Felhasználók lap - örökölt viewben rendelt access képernyőképe](./media/role-based-access-control-configure/view-access.png)

Értesítés, hogy az egyes felhasználók voltak **hozzárendelt** hozzáférhessenek míg mások **öröklődés** . Az Access az erőforráscsoport rendelt vagy hozzárendelés öröklődnek a szülő-előfizetéséhez.

> [AZURE.NOTE] Klasszikus előfizetés rendszergazdáknak és a további-rendszergazdák az előfizetést, az új RBAC modell tulajdonosainak számítanak.


## <a name="add-access"></a>Az Access hozzáadása
Az erőforrás, erőforráscsoport vagy előfizetést, terjed ki a szerepkör-hozzárendelés hozzáférést adja meg.

1. Válassza a **Hozzáadás** a az Access a vezérlő lap.  
2. Jelölje ki a szerepkör hozzárendelése a **Válassza ki azt a szerepkört** lap a használni kívánt.
3. Jelölje ki a felhasználó, csoport vagy alkalmazás, amelyet szeretne hozzáférést biztosítani a címtárban. A megjelenítendő nevét, e-mail címét és objektumok azonosítói címtárban is kereshet.  

    ![Adja hozzá a felhasználókat a lap - kép keresése](./media/role-based-access-control-configure/grant-access2.png)

4. Válassza az **OK gombra** a hozzárendelés létrehozásához. A **felhasználó hozzáadása** előugró haladás nyomon követi.  
    ![Felhasználói folyamatjelző - kép hozzáadása](./media/role-based-access-control-configure/addinguser_popup.png)

Miután sikeresen megadta a szerepkör-hozzárendelés, kattintson a **felhasználók** lap fog megjelenni.

## <a name="remove-access"></a>A hozzáférés-eltávolítása

1. Jelölje ki az Access a vezérlő lap a szerepkör-hozzárendelés.
2. **Eltávolításához** válassza a hozzárendelés a Részletek lap.  
3. Válassza az **Igen** eltávolításának megerősítéséhez.  
    ![A felhasználók lap - szerepkör kép eltávolítása](./media/role-based-access-control-configure/remove-access1.png)

Örökölt hozzárendelések nem lehet eltávolítani. Az alábbi képen látható, hogy az Eltávolítás gomb szürkén jelenik meg. Nézze meg, helyette a **Felelős a** részleteket. Nyissa meg az erőforrásra nem szerepel a szerepkör-hozzárendelés eltávolítása.

![Felhasználók lap - örökölt hozzáférés letiltása eltávolítása gomb képe](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Egyéb eszközök való hozzáférés kezelése
Szerepköröket rendelhet hozzájuk, és Azure RBAC parancsok eszközök kívül az Azure-portálra a hozzáférés kezelése.  Kövesse a hivatkozásokra kattintva többet szeretne tudni a Előfeltételek és első lépések az Azure RBAC parancsokat.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure parancssor](role-based-access-control-manage-access-azure-cli.md)
- [REST API-VAL](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Következő lépések
- [Access módosítási előzmények jelentés létrehozása](role-based-access-control-access-change-history-report.md)
- Lásd: a [RBAC beépített szerepkörök](role-based-access-built-in-roles.md)
- A saját [egyéni szerepkörök Azure RBAC](role-based-access-control-custom-roles.md) meghatározása
