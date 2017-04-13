<properties
    pageTitle="Virtuális gép skála csoportosító létrehozása az Azure portálon |} Microsoft Azure"
    description="Telepítse az Azure portálon skála beállítása."
    keywords="virtuális gép skála beállítása" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Virtuális gép skála csoportosító létrehozása az Azure portál használatával

Ebből az oktatóanyagból megtudhatja, hogy milyen könnyen hozhat létre virtuális gép skála megadása csak néhány perc alatt az Azure portál használatával. Ha nem rendelkezik az Azure előfizetéssel, [ingyenes fiók](https://azure.microsoft.com/free/) létrehozása, mielőtt elkezdené.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Jelölje ki a virtuális képet a piactérről

A portálon CentOS, CoreOS, Debian, Megnyitás Suse, vörös kalap vállalati Linux, SUSE Linux vállalati kiszolgáló, Ubuntu Server vagy Windows Server képek az skálával könnyen telepíthető.

Első lépésként nyissa meg az [Azure portal](https://portal.azure.com) egy webböngészőben. Kattintson a `New`, kereshet `scale set`, és válassza a `Virtual machine scale set` bejegyzés:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>A Linux virtuális gép létrehozása

Most már használja az alapértelmezett beállításokat, és gyorsan létre virtuális gépen.

* Kattintson a `Basics` lap, írjon be egy nevet az skála számára. Ez a név válik a terheléselosztó elé skála megadása a teljesen minősített tartománynév alapjának, ezért ügyeljen a név összes Azure egyediségét.

* Jelölje ki a kívánt OS írja be, írja be a kívánt tartozó felhasználónevét, és válassza ki a hitelesítési írja be a szívesebben. Ha úgy dönt, hogy a jelszó, akkor kell lennie legalább 12 karakter hosszú, és teljesíti három kívül az alábbi négy összetettsége követelmények: egy kisbetű karakter, egy nagybetű karakter, egy szám vagy egy speciális karaktert. Lásd: További tudnivalók a [felhasználónév és jelszó követelményeknek](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Ha úgy dönt, `SSH public key`, Beillesztés meg róla, hogy csak a nyilvános kulcs, a titkos kulcs:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Adja meg a kívánt erőforráscsoport nevét és helyét, és kattintson a `OK`.

* Kattintson a `Virtual machine scale set service settings` lap: Adja meg a kívánt tartomány nevét a címke (a terheléselosztó elé skála megadása a teljesen minősített tartománynév az alap). Ez a címke összes Azure egyedinek kell lennie.

* Válassza ki a kívánt operációs rendszer lemez képe, példányok száma és gépi méretét.

* Engedélyezheti vagy letiltásával, és adja meg, ha engedélyezve van:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Kattintson a `Summary` lap, adatérvényesítési befejezése után kattintson a `OK`.

* Végül kattintson a `Purchase` lap, kattintson a `Purchase` indítsa el a skála megadása a telepítés.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Csatlakozás egy virtuális skála megadása

Miután a skála megadása telepíti, nyissa meg azt a `Inbound NAT Rules` -skála megadása a terheléselosztó lapon:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

A skála megadása a hálózati Címfordítást szabályok használatával az egyes virtuális csatlakozhat. Például a Windows méretarányra beállított esetén a hálózati Címfordítást szabályok bejövő port 50000, ha sikerült szeretne kapcsolódni, hogy a gép keresztül RDP `<load-balancer-ip-address>:50000`. A Linux méretarányra beállított volna csatlakoztatja az paranccsal `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Következő lépések

Dokumentáció skála beállítja a CLI a telepítéséről [Ez](./virtual-machine-scale-sets-cli-quick-create.md)dokumentációjában olvasható.

Dokumentáció skála beállítja a PowerShell telepítéséről [Ez](./virtual-machine-scale-sets-windows-create.md)dokumentációjában olvasható.

Dokumentáció skála beállítja a Visual Studio telepítéséről [Ez](./virtual-machine-scale-sets-vs-create.md)dokumentációjában olvasható.

Általános dokumentációt tanulmányozza a [dokumentáció áttekintése lapon a skála állítja be](./virtual-machine-scale-sets-overview.md).

Általános információt jelölje ki a [Méretezés eredménye a fő céloldal](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

