<properties
   pageTitle="Hogyan egy virtuális címkézéséhez |} Microsoft Azure"
   description="Ismerje meg, az erőforrás-kezelő telepítési modell Azure-ban létrehozott Windows virtuális gép címkézésével kapcsolatban"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-windows-virtual-machine-in-azure"></a>Hogyan címkézéséhez virtuális gép Windows Azure-ban


Ez a cikk ismerteti a változatos módszerekkel, amelyekkel nyomon követése a virtuális gép Windows Azure-ban az erőforrás-kezelő telepítési modell keresztül. Címkék felhasználói kulcs/érték párokká, amely mentesíthetők közvetlenül egy erőforráshoz vagy erőforrás csoport a rendszer. Azure jelenleg támogatott legfeljebb 15 címkék egy erőforrás- és erőforrás-csoportot. Címkék létrehozása idején erőforrás helyezett el vagy hozzáadni a meglévő erőforrás. Felhívjuk a figyelmét arra, hogy csak az erőforrás-kezelő telepítési modell segítségével létrehozott erőforrások címkék támogatott. Ha szeretne egy Linux virtuális gép címkézése, megtudhatja, [hogy miként címkézése Linux virtuális gép Azure-ban](virtual-machines-linux-tag.md).

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-powershell"></a>A PowerShell címkézése

Szeretne létrehozni, hozzáadása és törlése a címkéket, először meg kell állíthatja be a [PowerShell környezetet az Azure erőforrás-kezelő][]Powershellen keresztül. A telepítés befejezése után helyezhetők el címkék számítási, a hálózati és a tárolási erőforrásokat hoz létre, vagy az erőforrás PowerShell létrehozását követően. Ez a cikk a megtekintéséhez vagy szerkesztéséhez címkék virtuális gépeken elhelyezett összpontosít.

Első lépésként nyissa meg a virtuális gépek között a `Get-AzureRmVM` parancsmag.

        PS C:\> Get-AzureRmVM -ResourceGroupName "MyResourceGroup" -Name "MyTestVM"

Ha a virtuális gép már címkéket tartalmazza, az erőforrás majd jelennek meg az összes címkét:

        Tags : {
                "Application": "MyApp1",
                "Created By": "MyName",
                "Department": "MyDepartment",
                "Environment": "Production"
               }

Ha meg szeretné adni a címkék Powershellen keresztül, akkor a `Set-AzureRmResource` parancsot. Megjegyzés: a címkéket, Powershellen keresztül frissítésekor címkék egészének frissülnek. Így egy erőforráshoz címkéket tartalmazó ad hozzá egy címkét, ha szüksége lesz felvenni az összes címkét, az erőforrás elhelyezni kívánt. Az alábbi képen egy példát, hogyan vehet fel további címkéket egy erőforráshoz PowerShell-parancsmagok között.

Ezen első parancsmag állítja be az összes *MyTestVM* a *$tags* változó helyezett el a címkék használata a `Get-AzureRmResource` és `Tags` tulajdonság.

        PS C:\> $tags = (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

A második parancs megjeleníti a címkéket a megadott változó.

        PS C:\> $tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment

A harmadik parancs hozzáadása a *$tags* változó egy további címke. Megjegyzés: a használatát a **+=** az új kulcs/érték pár hozzáfűzése a *$tags* listában.

        PS C:\> $tags += @{Name="Location";Value="MyLocation"}

A negyedik parancs állítja be az összes a címkéket a *$tags* változóban az adott erőforráshoz megadott. Ebben az esetben célszerű MyTestVM.

        PS C:\> Set-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM -ResourceType "Microsoft.Compute/VirtualMachines" -Tag $tags

Az ötödik parancs megjeleníti a címkék található összes az erőforrás. Amint látható, *hely* most definíciója címkeként *MyLocation* az az érték.

        PS C:\> (Get-AzureRmResource -ResourceGroupName MyResourceGroup -Name MyTestVM).Tags

        Name        Value
        ----                           -----
        Value       MyDepartment
        Name        Department
        Value       MyApp1
        Name        Application
        Value       MyName
        Name        Created By
        Value       Production
        Name        Environment
        Value       MyLocation
        Name        Location

További Powershellen keresztül címkézésével kapcsolatban, az [Erőforrás-parancsmagok Azure][]kivétele.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Következő lépések

* Az Azure erőforrások címkézésével kapcsolatban további információért lásd: [Azure erőforrás-kezelő – áttekintés][] és a [Címkék használata az Azure erőforrások rendszerezéséhez][].
* Hogyan lehet címkék segítségével az Azure erőforrások használt kezelése című témakör tartalmazza [Azure számlájának ismertetése][] és [szerezhet be a Microsoft Azure erőforrás-felhasználás az összefüggéseket][].

[A PowerShell környezetet a Azure-kezelő eszközzel]: ../powershell-azure-resource-manager.md
[Azure erőforrás-parancsmagok]: https://msdn.microsoft.com/library/azure/dn757692.aspx
[Azure erőforrás szolgáltatásának áttekintése]: ../azure-resource-manager/resource-group-overview.md
[Az Azure erőforrások rendszerezéséhez-címkék használata]: ../resource-group-using-tags.md
[Azure számlájának ismertetése]: ../billing/billing-understand-your-bill.md
[Nyereség háttérismeretek be a Microsoft Azure erőforrás-felhasználás]: ../billing-usage-rate-card-overview.md
