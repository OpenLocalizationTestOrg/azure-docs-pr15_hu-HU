<properties
    pageTitle="Felhasználói engedélyek kiosztása adott labor házirendek |} Microsoft Azure"
    description="Megtudhatja, hogy miként felhasználói engedélyek kiosztása a felhasználók igényei alapján DevTest Labs adott labor házirendek"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Felhasználói engedélyek kiosztása adott labor házirendek

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja, hogyan engedélyek adása felhasználóknak az egy adott labor házirendet a PowerShell használatával. Úgy, hogy engedélyeket is alkalmazható a felhasználók igényeinek alapján. Például érdemes lehet adni egy felhasználó az azt jelenti, hogy a virtuális házirend-beállítások, de nem a költség házirend módosítása.

## <a name="policies-as-resources"></a>Erőforrásként házirendek

[Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) című témakörben tárgyalt, RBAC lehetővé teszi, hogy az Azure külön access források kezelése. RBAC használ, feladatokat újból a DevOps munkacsoporton belül, és adjon csak access összegét felhasználóknak munkájuk elvégzéséhez szükségük van.

DevTest Labs egy házirendet, amely lehetővé teszi a RBAC művelet **Microsoft.DevTestLab/labs/policySets/policies/**erőforrástípust. Minden egyes labor házirend írja be a házirend erőforrás erőforrás, és egy RBAC szerepkörhöz keresési tartomány rendelhetők.

Például annak érdekében, hogy adja meg az felhasználók olvasási/írási engedéllyel a **Virtuális méretű engedélyezett** házirendet szeretne létrehoz egy egyéni szerepkört, hogy működik-e a **Microsoft.DevTestLab/labs/policySets/policies/** * művelet, majd a megfelelő felhasználók rendel a egyéni szerepkör hatálya alá * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Ha többet szeretne megtudni a RBAC egyéni szerepkörök, [egyéni szerepkörök hozzáférés-vezérlés](../active-directory/role-based-access-control-custom-roles.md)látható.

##<a name="creating-a-lab-custom-role-using-powershell"></a>A PowerShell használatával labor egyéni szerepkör létrehozása
Első lépésként érdekében olvassa el a következő cikket, amely fog bemutatják, hogy miként telepítheti, állíthatja Azure PowerShell-parancsmagok kell: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Miután beállította az Azure PowerShell-parancsmagok, akkor az alábbi műveleteket végezheti el:

- A lista, a műveletek/műveleteket egy erőforrás-szolgáltató
- Az adott jogosultság Listaműveletek:
- Hozzon létre egy egyéni szerepkört

Az alábbi PowerShell parancsprogramot ezeket a műveleteket példa szemlélteti:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Engedélyek hozzárendelése egy felhasználóhoz, egyéni szerepkörök használata bizonyos házirend
Miután az egyéni szerepkörök egyénileg definiált, felhasználókhoz rendelheti. Annak érdekében, hogy egy egyéni szerepkör hozzárendelése egy felhasználóhoz, be kell szereznie **objektumazonosító** , amely a felhasználó. Ehhez a **Get-AzureRmADUser** parancsmag használja.

A következő példában a **objektumazonosító** *SomeUser* felhasználó 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Ha befejezte a **objektumazonosító** egyéni szerepkör nevét és a felhasználót, hozzárendelheti a szerepkör a **New-AzureRmRoleAssignment** parancsmagot a felhasználó számára:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Az előző példában a **AllowedVmSizesInLab** házirend használják. Használja az alábbi házirendeket:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések

Egyszer Ön már jogosult felhasználó adott labor házirendek, Íme néhány további megfontolandó lépések:

- A [biztonságos módon elérhetők az laboratóriumi](devtest-lab-add-devtest-user.md).

- [Labor házirendek beállítása](devtest-lab-set-lab-policy.md).

- [Labor sablon létrehozása](devtest-lab-create-template.md).

- [Az a VMs létrehozása az egyéni eltéréseket](devtest-lab-artifact-author.md).

- [Hozzáadás egy virtuális laboratóriumi való eltérések rendelkező](devtest-lab-add-vm-with-artifacts.md).
