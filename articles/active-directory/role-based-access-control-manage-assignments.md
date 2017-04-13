<properties
    pageTitle="Az access Azure erőforrás-hozzárendelések megtekintése |} Microsoft Azure"
    description="Megtekintheti és kezelheti a bármelyik felhasználó vagy csoport az Azure-portálon szerepköralapú hozzáférés-vezérlés-hozzárendelésekhez"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>A felhasználók és csoportok az Azure-portálon – nyilvános kép az access-hozzárendelések megtekintése

> [AZURE.SELECTOR]
- [Access-felhasználó vagy csoport kezelése](role-based-access-control-manage-assignments.md)
- [Erőforrás szerint hozzáférés kezelése](role-based-access-control-configure.md)

A szerepköralapú hozzáférés vezérlő (RBAC) az Azure Active Directory-ban, az access kezelheti az Azure erőforrásoknak. [Mi az a előzetes verzióban?](active-directory-preview-explainer.md)

Access RBAC adataiban oka az, külön kétféle módon korlátozhatják a engedélyeket:

- **Hatókör:** RBAC szerepkör-hozzárendelések az egy adott előfizetés, erőforráscsoport vagy erőforrás korlátozódik. Az access egy erőforrás adott felhasználó nem tud hozzáférni az azonos előfizetés más erőforrások.
- **Szerepkör:** A hozzárendelésben szereplő hatálya alá maradt van az access által a szerepkör hozzárendelése akár további. Lehet, hogy szerepkörök magas szintű, például a tulajdonos vagy adott, például a virtuális gép olvasó.

Szerepkörök csak rendelhetők a az előfizetést, az erőforráscsoport vagy az erőforrás, hogy a hatókör a hozzárendeléshez tartozó belül. De tekinthet meg az adott felhasználó vagy csoport access-hozzárendelések egy helyen.

További információk arról, hogy miként [szerepkör-hozzárendelések az Azure előfizetés erőforrások való hozzáférés kezelése](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Access-hozzárendelések megtekintése

Az access-hozzárendelések egyetlen felhasználó vagy csoport kikeresni, indítsa el az Azure Active Directory az [Azure-portálon](http://portal.azure.com).

1. Jelölje ki az **Azure Active Directory**. Ha ez a beállítás nem látható a navigációs lista, válassza a **További szolgáltatások** , és görgetéssel keresse meg **Azure Active Directory**.
2. Válassza a **felhasználók és csoportok**, majd vagy **minden felhasználónak** vagy az **összes csoport**. Ez a példa azt kiemelése felhasználónként.
    ![Felhasználók és csoportok az Azure Active Directory - képernyőképe kezelése](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Keresse meg a felhasználó nevét vagy felhasználónevét.
4. Jelölje be a felhasználó lap **Azure erőforrások** . Az adott felhasználó access-hozzárendelések jelennek meg.

### <a name="read-permissions-to-view-assignments"></a>Olvasás-hozzárendelések megtekintése

Ezen az oldalon csak az access-hozzárendelések olvasási engedéllyel rendelkező jeleníti meg. Például olvasási hozzáférést A előfizetése van, és nyissa meg az Azure erőforrások lap felhasználó hozzárendelések ellenőrzése. A hozzáférés-hozzárendelések előfizetés A is látható, de nem fogja látni, hogy kikkel is rendelkezik a hozzáférés-hozzárendelések a b előfizetés

## <a name="delete-access-assignments"></a>Access-hozzárendelések törlése

Ez a lap az access-hozzárendeléseket, közvetlenül a felhasználó vagy csoport rendelt törölheti. Ha az access-hozzárendelés a szülőcsoportjában öröklődik, nyissa meg az erőforrás-előfizetést, és kezelheti a hozzárendelés van szüksége.

1. A felhasználó vagy csoport az access-hozzárendelések listában jelölje ki a törölni kívánt.
2. Válassza az **Eltávolítás** , majd **Igen gombra** kattintva erősítse meg.
    ![Távolítsa el az access-hozzárendelés – kép](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Kapcsolódó témakörök

- Szerepköralapú hozzáférés-vezérlés [szerepkör-hozzárendelések az Azure előfizetés erőforrások való hozzáférés kezelése](role-based-access-control-configure.md) az első lépések
- Lásd: a [RBAC beépített szerepkörök](role-based-access-built-in-roles.md)
