<properties 
   pageTitle="Virtuális géphez - PowerShell hálózati gyorsított |} Microsoft Azure"
   description="Megtudhatja, hogy miként gyorsított hálózat beállítása az Azure virtuális gép PowerShell használatával."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Virtuális gép gyorsított hálózatok

> [AZURE.SELECTOR]
- [Azure portálon](virtual-network-accelerated-networking-portal.md)
- [A PowerShell](virtual-network-accelerated-networking-powershell.md)

Gyorsított hálózat lehetővé teszi, hogy egyetlen legfelső szintű I/O virtualizációs (SR-IOV) virtuális géphez (virtuális), a hálózati teljesítmény nagyban javítására. A nagy teljesítményű elérési út figyelmen kívül hagyja a késés, vibrálás és a legtöbb nagy hálózati a támogatott virtuális típusok terhelések kezelésére használható Processzor kihasználtsági csökkentése datapath a tároló. Ez a cikk ismerteti, hogyan Azure PowerShell használata a gyorsabb hálózat beállítása az Azure erőforrás-kezelő telepítési modell. A virtuális a gyorsított hálózat az Azure-portálon is létrehozhat. Megtudhatja, hogyan, kattintson a tetején látható ez a cikk az Azure-portálon mezőbe.

Az alábbi képen látható két virtuális gépeken futó (virtuális) és hálózati gyorsított nélkül közötti kommunikáció:

![Összehasonlítás](./media/virtual-network-accelerated-networking-powershell/image1.png)

Hálózati gyorsított nélkül teljes hálózatokból forgalom és kijelentkezés a virtuális kell bejárása az állomás és a virtuális kapcsolót. A virtuális Váltás az összes házirend végrehajtás, például hálózati biztonsági csoportokat, hozzáférés-listákat, elkülönítési és más hálózati forgalmat a hálózati virtualizálva szolgáltatások biztosít. További tudnivalókért olvassa el a [Hyper-V hálózati virtualizációs és a virtuális váltás](https://technet.microsoft.com/library/jj945275.aspx) cikket.

A hálózat gyorsított hálózati forgalmat a hálózati kártya megérkezik, és továbbítja a virtuális. Az összes hálózati házirendek nélkül gyorsított hálózati alkalmazott a virtuális váltás kiürített, és alkalmazza a hardver. A hardver házirend alkalmazása lehetővé teszi, hogy közvetlenül a virtuális kihagyásával az állomás és a virtuális kapcsoló akkor alkalmazza a fogadó összes házirend megőrzésével az előre hálózati forgalmat a hálózati.

A hálózat gyorsított előnyei csak a virtuális, hogy engedélyezve van a vonatkoznak. A legjobb eredményt akkor hasznos, ahhoz, hogy kapcsolódik az azonos VNet legalább két VMs ezt a szolgáltatást.  Kommunikáció több VNets és összekötő a helyszíni, ha ez a funkció a teljes időtartam a minimális hatása van.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Legfontosabb előny

- **Alsó késés / egy újabb csomagok második (pps):** A virtuális váltás eltávolítása a datapath eltávolítja a csomagok töltött idő a fogadó házirend feldolgozásra, és megnövelheti a csomagot, amely a virtuális belül feldolgozhatók számát.
- **Vibrálás csökkentett:** Virtuális kapcsoló feldolgozás vonatkozni kell, hogy a házirend összegét és a Processzor, amely a feldolgozás tesz a terhelését függ. A szabály végrehajtására a hardver mert megszüntetéséhez adott adattartományról csomagok kézbesítése közvetlenül a virtuális az állomás virtuális kommunikáció és az összes szoftver megszakítás és a helyi kapcsolók eltávolítása.
- **Csökkent Processzor kihasználtsági:** A virtuális Váltás a fogadó kihagyásával vezet kevesebb Processzor kihasználtsági feldolgozásához hálózati forgalmának engedélyezésére.

## <a name="limitations"></a>Korlátozások

Az alábbi korlátozások vonatkoznak a használja ezt a lehetőséget, ha létezik:
 
- **Hálózati kapcsolat létrehozása:** Gyorsított hálózati csak engedélyezhető az új hálózati kapcsolat.  Egy meglévő hálózati kapcsolaton nem engedélyezett.
- **Virtuális létrehozása:** A hálózati illesztő engedélyezett gyorsított hálózattal csak kapcsolható egy virtuális a virtuális létrehozásakor. A hálózati kapcsolaton nem lehet egy meglévő virtuális csatolt.
- **Régiók:** Csak a nyugati központi US és a nyugati Europe Azure régió kínált. A régiók készlete kibonthatja a jövőben.
- **Támogatott operációs rendszer:** Microsoft Windows Server 2012 R2 és a Windows Server 2016 technikai előzetes verzió 5. Linux és a Windows Server 2012-támogatási, amint hozzáadódik.
- **Virtuális méret:** Standard_D15_v2 és Standard_DS15_v2 az egyetlen támogatott virtuális példány méretét. További információ a [Windows virtuális méretű](../virtual-machines/virtual-machines-windows-sizes.md) témakört is. A támogatott virtuális példány méretű megadása kibonthatja a jövőben.

Ezek a korlátozások módosításai keresztül az [Azure virtuális hálózati frissítések](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lap jelenti be a rendszer.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Virtuális Windows gyorsított hálózattal létrehozása

1. Nyisson meg egy PowerShell parancssort, és töltse ki a ebben a szakaszban a hátralévő lépéseket belül egy PowerShell-munkamenetet. Ha még nincs telepítve és beállítva PowerShell, hajtsa végre a [miként telepítheti és Azure PowerShell beállítása](../powershell-install-configure.md) című témakörben.
2. A betekintő rögzítése, küldjön e-mailt [Hálózati előfizetések gyorsabb,](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) és az előfizetés azonosítója és szánt használatát. A hátralévő lépéseket, amíg nem teljes, miután az, hogy Ön már elfogadták az Előnézet be értesítő e-mailt kap.
3. Az előfizetés az alábbi parancsok beírásával a képesség regisztrálása:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Cserélje le a *westcentralus* támogatják ezt a lehetőséget (ha szükséges) Ez a cikk a [vonatkozó korlátozások](#limitations) című szakaszát szerepel egy másik helyre a nevet. Írja be a következő parancsot a változó hely beállítása:

        $locName = "westcentralus"

5. *RG1* cserélje ki az erőforráscsoport, amely tartalmazni fogja az új hálózati kapcsolat létrehozásához az alábbi parancsok adja meg egy nevet:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Váltson *VM1-NIC1* kapcsolódó nevezze el a hálózati kapcsolaton, és írja be a következő parancsot:

        $NICName = "VM1-NIC1"

7. A hálózati kapcsolaton kell csatlakoznia egy meglévő Azure virtuális hálózati (VNet) belüli alhálózat az helyét és az [előfizetés](../azure-glossary-cloud-terminology.md#subscription) a hálózati kapcsolat. További tudnivalók a VNets a [virtuális hálózati áttekintés](virtual-networks-overview.md) cikk olvasásával, ha nem ismeri a őket. Egy VNet létrehozásához hajtsa végre [a VNet létrehozása](virtual-networks-create-vnet-arm-ps.md) című témakörben. Módosítsa a következő $Variables "értékek" adja meg a használni kívánt a hálózati felületet VNet és a nevét.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Ha nem tudja, hogy egy meglévő VNet 3 lépésben kiválasztott hely nevét, írja be az alábbi parancsokat:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Ha a lista vissza üres, akkor kell hozzon létre egy VNet helyen. Egy VNet létrehozásához hajtsa végre a [virtuális hálózat létrehozása](virtual-networks-create-vnet-arm-ps.md) című témakörben.

    Az alhálózathoz belül a VNet nevének, írja be a következő parancsokat, és *Alhalozat_1* feletti cserélje alhálózat neve:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Írja be a következő parancsok végrehajtása az VNet és alhálózat olvashat be és változók osztani.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Azonosítsa a meglévő nyilvános IP-cím erőforrás rendelhető a hálózati felületére úgy csatlakozhat azt az interneten keresztül. Ha nem szeretné, az interneten keresztül a a hálózati kapcsolaton, virtuális eléréséhez, kihagyhatja ezt a lépést. Anélkül, hogy egy nyilvános IP-címet csatlakoznia kell a virtuális az azonos VNet csatlakozik egy másik virtuális a. 

    Módosítsa a *PIP1* egy meglévő nyilvános IP-cím erőforrás, amely a kapcsolat készít, és, amely nem egy másik hálózati kapcsolat jelenleg társított hely létezik nevére. Ha szükséges, váltson $rgName a csoport nevét, az erőforrás a nyilvános IP-cím erőforrás szerepel, és írja be a következő parancsot:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Ha nem tudja, hogy egy meglévő nyilvános IP-cím erőforrás nevét, írja be az alábbi parancsokat:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    A **IPConfiguration** oszlop nem tartalmaz értéket az eredményt ad vissza, ha a nyilvános IP-cím erőforrás program nem társítja meglévő hálózat felületet, és használhatók. Ha a lista nem üres, vagy nem érhető el, nyilvános IP-cím erőforrások, létrehozhat egyet a New-AzureRmPublicIPAddress paranccsal.

    >[AZURE.NOTE] Nyilvános IP-címek a névleges díjat kell. További tudnivalók az [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses) oldal olvasásával árak.
10. Ha nem a kapcsolat egy nyilvános IP-cím erőforrás hozzáadása, eltávolítása *- PublicIPAddress $PIP1* végén található a következő parancsot. A hálózati kapcsolat létrehozása gyorsított hálózattal beírásával a következő parancsot:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. A hálózati kapcsolaton hozzárendelése egy virtuális létrehozása a virtuális lépéseket (3) és 6 [létrehozása egy virtuális](../virtual-machines/virtual-machines-windows-ps-create.md) cikk utasításait követve. A 6-2 cserélje *Standard_A1* a [korlátozások](#limitations) szakaszban, a jelen cikk virtuális méretű közül.

    >[AZURE.NOTE] Ha megváltoztatta a *nevét* a $locName, $rgName, vagy $nic változók ebben a cikkben, lépés 6 egy virtuális cikk létrehozása sikertelen lesz. Jó helyen jár, akkor módosítsa az *értékeket* a változók.

12. Amikor létrejött a virtuális, töltse le a [hálózati gyorsított illesztőprogram](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), a virtuális csatlakozás, és futtassa a illesztőprogram telepítőjét belül a virtuális.

13. Kattintson a jobb gombbal a Windows-gombot, és kattintson az **Eszközkezelő**elemre. Ellenőrizze, hogy a **Mellanox ConnectX-3-as virtuális függvény Ethernet kártya** jelenik meg a **hálózati** megadására, ha ki van bontva, az alábbi képen látható módon:

    ![Az Eszközkezelő](./media/virtual-network-accelerated-networking-powershell/image2.png)