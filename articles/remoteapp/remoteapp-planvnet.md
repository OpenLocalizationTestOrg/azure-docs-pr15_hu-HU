<properties
    pageTitle="Hogyan kell a virtuális hálózattervezés az Azure RemoteApp gyűjtemény |} Microsoft Azure"
    description="További információ a virtuális hálózattervezés az Azure RemoteApp a webhelycsoporthoz."
    services="remoteapp"
    documentationCenter="" 
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>A virtuális hálózat megtervezése Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

A dokumentum ismerteti, hogyan állíthat be az Azure virtuális hálózati (VNET) és a alhálózat az Azure RemoteApp. Ha nem ismeri a Azure virtuális hálózatok, az egy lehetőség, amellyel különböző helyekre történő virtualizálása a hálózati infrastruktúrára a felhőbe, és Azure és a helyszíni erőforrások hibrid megoldások létrehozása. Erről további vele [Itt](../virtual-network/virtual-networks-overview.md).

Eszközbiztonsági házirendek forgalom (a bejövő és kimenő) definiálhat a hol telepít Azure RemoteApp virtuális hálózatához tetszés kifejezetten ajánljuk külön alhálózathoz létrehozása az Azure RemoteApp a telepítések az Azure virtuális hálózat többi. Eszközbiztonsági házirendek definiálásáról az Azure virtuális hálózati alhálózat a további tudnivalókért olvassa el az [egy hálózati biztonsági csoport (NSG) tartalma?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Azure virtuális hálózatokkal Azure RemoteApp gyűjtemények

Az alábbi ábrán a két másik fizetési lehetőség virtuális hálózatot használni kívánt megjelenítése.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp VNET felhő gyűjtemény

 ![Azure RemoteApp – egy VNET felhő gyűjtemény](./media/remoteapp-planvpn/ra-cloudvpn.png)

Ez jelenti, hogy az Azure RemoteApp webhelycsoport, ahol a RemoteApp munkamenet hosts való hozzáférést igénylő erőforrások Azure-ban van telepítve. Lehetnek, mint a RemoteApp VNET az azonos VNET vagy egy másik VNET Azure-ban.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp VNET hibrid gyűjtemény

![Azure RemoteApp – egy VNET hibrid gyűjtemény](./media/remoteapp-planvpn/ra-hybridvpn.png)

Egy Azure RemoteApp webhelycsoport, ahol a RemoteApp munkamenet hosts való hozzáférést igénylő erőforrások telepítve a helyszíni jelöl ki. A RemoteApp VNET van csatolva a helyszíni hálózaton, például a webhely VPN vagy Express útvonal Azure hibrid technológiákkal.


## <a name="how-the-system-works"></a>A rendszer működése

A fedőlapok csoportban Azure RemoteApp Azure virtuális gépeken futó (képpel a feltöltött), amelyhez során kiépítési virtuális alhálózathoz üzembe helyezése. Ha azt választotta, a hibrid gyűjtemény, azt próbálja megoldani a tartományvezérlőnek a DNS-kiszolgálóval, a virtuális hálózat megadott megadott kiépítési-munkafolyamaton belül a teljes Tartománynevét.  
Ha egy meglévő virtuális hálózathoz csatlakozik, feltétlenül kattintva jelenítse meg a szükséges portok az Azure RemoteApp alhálózathoz, a hálózati biztonsági csoportokat. 

Azt javasoljuk, hogy [az Azure RemoteApp elég nagy alhálózat](remoteapp-vnetsizing.md)használja. A legnagyobb Azure virtuális hálózati által támogatott /8 (CIDR alhálózat definíciók használata). Az alhálózathoz elég nagy az Azure RemoteApp VMs skála telefonos során, ha több felhasználó fér hozzá az alkalmazások kell lennie. 

Az alábbiakban a következőkre lesz szüksége lesz ahhoz, hogy a virtuális hálózati alhálózat: 

2.  Az alhálózathoz kimenő forgalom használatával kommunikál a belső Azure RemoteApp szolgáltatások közül a porttartományt 10101-10175 kell tenni.
3.  Kimenő forgalmának kell csatlakozhat az alhálózathoz a 443-as port Azure-tárolóhoz
4.  Ha üzemeltetett az Azure Active Directory, legyen az Azure RemoteApp a virtuális hálózati alhálózat belül minden olyan virtuális tud csatlakozni a tartomány. A virtuális hálózati DNS-Rekordokat kell tudja oldani a tartományvezérlőnek a teljes Tartománynevét.


## <a name="virtual-network-with-forced-tunneling"></a>Kényszerített tunneling virtuális hálózaton

Minden új Azure RemoteApp gyűjtemény most támogatott [kikényszerített tunneling](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) . Jelenleg nem támogatott kikényszerített tunneling támogatásához egy meglévő webhelycsoport az áttelepítés.  Be kell a meglévő gyűjtemény használatával a csatolni kívánt Azure RemoteApp VNET törlése, és hozzon létre egy újat az első kényszerített tunneling a webhelycsoportok engedélyezve van. 
