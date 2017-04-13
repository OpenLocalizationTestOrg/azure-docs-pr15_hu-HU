<properties
    pageTitle="Azure egymást fedő (szolgáltatás rendszergazdai és bérlői) felhasználónként erőforrások az engedélyek kezelése |} Microsoft Azure"
    description="Szolgáltatás-rendszergazda vagy bérlői megtudhatja, hogy miként felhasználónként forrá jogosultságok kezelése."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Felhasználói engedélyek kezelése

A felhasználó Azure egymást fedő lehet egy olvasó, a tulajdonos vagy a közös munka egy előfizetés, erőforráscsoport vagy szolgáltatás minden példányában. Például "a" felhasználó előfordulhat, hogy előfizetés 1 olvasó engedélyekkel rendelkezni, de virtuális gép 7-tulajdonosi engedélyekkel rendelkezik.

-   Olvasó: Felhasználói megtekinthetők mindent, de nem tudja módosítani.

-   Közös munka: Felhasználói kezelheti erőforrásokhoz való hozzáférés kivételével minden.

-   Tulajdonosa: Felhasználói kezelhetik a szolgáltatás, beleértve az erőforrásokhoz.


## <a name="set-access-permissions-for-a-user"></a>A felhasználó a hozzáférési engedélyek beállítása

1.  Jelentkezzen be a kezelni kívánt erőforrás-tulajdonosi engedélyekkel rendelkező fiókkal.

2.  Az erőforrás lap, kattintson az **Access** ikonjára ![](media/azure-stack-manage-permissions/image1.png).

3.  Kattintson a **felhasználók** lap a **szerepköröket**.

4.  Kattintson a **szerepkörök** , a lap **hozzáadása** a felhasználó engedélyeinek hozzáadni.

## <a name="next-steps"></a>Következő lépések

[Az Azure Papírhalom bérlői hozzáadása](azure-stack-add-new-user-aad.md)
