<properties
   pageTitle="Erőforrás-kezelő Azure támogatása terheléselosztó |} Microsoft Azure "
   description="Az Azure erőforrás-kezelővel terheléselosztó powershell használatával. Sablonok használata a terheléselosztó"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure terheléselosztó Azure erőforrás-kezelő támogatási használata

Azure erőforrás-kezelő szolgáltatások Azure-ban használni kívánt felügyeleti kerete. Azure terheléselosztó is kezelhetők a erőforrás-kezelő Azure-alapú API-hoz és eszközök használatával.

## <a name="concepts"></a>Fogalmak

Az erőforrás-kezelő Azure terheléselosztó tartalmazza a gyermek alábbi forrásokat:

- Előtér-IP-konfiguráció – egy terheléselosztó is elhelyezhet egy vagy több előtér IP-címek, más néven egy virtuális IP-címei (VIP). Ezek az IP-címek a forgalmat a bejövő adatok lesz.

- Háttéradatbázis cím készlet – ezek a a virtuális gép hálózati kapcsolat kártya (hálózati kártya), amelyhez a betöltés elosztott kell társított IP-címet.

- Betöltése terheléselosztó szabályok – egy szabály tulajdonsága hozzárendeli egy adott előtér IP- és a port együttes használatával egy vissza vége IP-címek és a port kombinációt. Egy egyetlen terheléselosztó több terheléselosztási szabályok állhat. Minden egyes a szabály egy olyan előtér IP és port és háttéradatbázist IP és VMs társított port.

- Szondákat – a szondákat lehetővé teszi a virtuális példányok állapotának nyomon követheti a. Ha nem sikerül egy állapot vizsgálati, a virtuális példány kell tenni Forgatás ki automatikusan.

- Hálózati Címfordítást a bejövő szabályok – hálózati Címfordítást szabályok – az első lépés a bejövő forgalom definiáló IP befejezéséhez, és vissza vége IP-normális eloszlású.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Quickstart útmutató sablonok

Azure erőforrás-kezelő hozhatók létre az alkalmazások deklaráció sablon használatával teszi lehetővé. Egy sablon azok függőségek és több szolgáltatás telepítheti. Többször telepítése az alkalmazás minden szakaszában, az alkalmazás életciklus használhatja ugyanazt a sablont.

Sablonok virtuális gépeken futó, virtuális hálózatok, elérhetőségének beállítása, hálózati kapcsolatok (NIC), tároló fiókok, terheléselosztókkal, hálózati biztonsági csoportokat és nyilvános IP-címei tartalmazhatnak. A sablonok hozhat létre minden összetett alkalmazáshoz szükséges. A sablonfájlt ellenőrizhető verziókövetés és együttműködési Tartalomkezelés rendszerbe.

[További tudnivalók a sablonok](http://go.microsoft.com/fwlink/?LinkId=544798)

[További tudnivalók a hálózati erőforrások](../virtual-network/resource-groups-networking.md)

Quickstart útmutató sablonok használatával Azure terheléselosztó található egy [GitHub tárházba](https://github.com/Azure/azure-quickstart-templates) szolgáltatója a létrehozott közösségi sablonok készletét.

Példák a sablonok:

- [2 VMs terheléselosztó betöltése és terheléselosztási szabályok](http://go.microsoft.com/fwlink/?LinkId=544799)
- [A belső terheléselosztó és terheléselosztó szabályokkal egy VNET 2 VMs](http://go.microsoft.com/fwlink/?LinkId=544800)
- [Az egy terheléselosztó 2 VMs és a LB hálózati Címfordítást szabályok konfigurálása](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Azure terheléselosztó PowerShell vagy CLI beállítása

Első lépések a Azure erőforrás-kezelő parancsmagok, parancssori eszközök és REST API-hoz

- [Azure hálózati parancsmagok](https://msdn.microsoft.com/library/azure/mt163510.aspx) hozzon létre egy terheléselosztó is használható.
- [Hogyan hozhat létre egy terheléselosztó Azure erőforrás-kezelő használatával](load-balancer-get-started-ilb-arm-ps.md)
- [Azure erőforrás-kezelés az Azure CLI használata](../xplat-cli-azure-resource-manager.md)
- [Terheléselosztó REST API-hoz](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Következő lépések

Is [első lépések megtételében egy internetes terheléselosztó](load-balancer-get-started-internet-arm-ps.md) és [terjesztési mód](load-balancer-distribution-mode.md) egy adott betöltés terheléselosztó hálózati forgalmának engedélyezésére elemeire vonatkozó típusának beállítása.

Megtudhatja, hogy miként [egy terheléselosztó üresjárati TCP időtúllépés beállításainak](load-balancer-tcp-idle-timeout.md)kezelése. Ez fontos, amikor az alkalmazás kell hagyja életben mögött egy terheléselosztó kiszolgálók kapcsolat.
