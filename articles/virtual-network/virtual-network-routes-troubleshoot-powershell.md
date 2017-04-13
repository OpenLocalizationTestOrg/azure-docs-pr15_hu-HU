<properties 
   pageTitle="Útvonalak - PowerShell elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként útvonalak az Azure erőforrás-kezelő telepítési modell Azure PowerShell használatával kapcsolatos hibák elhárítása."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
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
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Azure PowerShell használatával útvonalak – problémamegoldás

> [AZURE.SELECTOR]
- [Azure portálon](virtual-network-routes-troubleshoot-portal.md)
- [A PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Ha a hálózati kapcsolat problémákat szeretné vagy át az Azure virtuális gép (virtuális) tapasztal, akkor útvonalak érintő a virtuális forgalom. Ez a cikk áttekintést nyújt további hibaelhárítási útvonalak diagnosztika funkciók.

Útvonal alhálózat társított és a hálózati adaptereken (hálózati) az adott alhálózat érvényesek. A következő típusú útvonalak is alkalmazható egyes hálózati kapcsolat:

- **Rendszer útvonalak:** Alapértelmezés szerint minden Azure virtuális hálózatban (VNet) létrehozott alhálózat rendelkezik a rendszer útvonal táblákat, amelyek lehetővé teszik a helyi VNet forgalom, a helyszíni forgalom e VPN átjárók és internetes forgalmat. Rendszer útvonalak is léteznek peered VNets.
- **BGP útvonalak:** Propagált hálózati kapcsolatok készült ExpressRoute vagy a webhely VPN-kapcsolaton keresztül. További tudnivalók a [BGP a virtuális Magánhálózati átjárók](../vpn-gateway/vpn-gateway-bgp-overview.md) és [készült ExpressRoute áttekintése](../expressroute/expressroute-introduction.md) cikkek olvasásával útválasztás BGP.
- **Felhasználói útvonalak (UDR):** Hálózati virtuális készülékek vagy a rendszer használata esetén kényszerített tunneling forgalmat a webhely virtuális Magánhálózaton keresztül a helyszíni hálózaton, előfordulhat, hogy van útvonalak felhasználói (UDRs) a alhálózat útvonal táblázat társított. Ha nem ismeri a UDRs, olvassa el a [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md#user-defined-routes) .

A különböző útvonal, amelyet a hálózati kapcsolat alkalmazhatók Ez lehet nehezen állapíthatók meg, mely összesítő útvonalak érvényesek. Az erőforrás-kezelő Azure telepítési modell problémák megoldására virtuális a hálózati kapcsolat, megtekintheti a hatékony útvonalak hálózati kapcsolat.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Hatékony útvonalak használatáról virtuális forgalom folyamat kapcsolatos hibák elhárítása

Ez a cikk a következő esetben példaként bemutatják, hogyan lehet a hatékony útvonalak hálózati kapcsolat hibaelhárítása használja:

A VNet csatlakozik egy virtuális (*VM1*) (*VNet1*, előtag: 10.9.0.0/16) nem sikerül kapcsolódni a VM(VM3) egy újonnan peered VNet (*VNet3*, 10.10.0.0/16 előtag) szereplő. Vannak olyan UDRs vagy BGP nem irányítja VM1-NIC1 hálózati kapcsolat csatlakozik a virtuális alkalmazott, akkor csak a rendszer útvonalak lesz érvényes.

Ez a cikk ismerteti, hogyan okának a kapcsolati hiba Azure az erőforrás-kezelés telepítési modell használata hatékony útvonalak lehetőséget.
A példában a csak a rendszer utakat, miközben ezeket a lépéseket használható határozza meg a bejövő és kimenő csatlakozási hibák egy útvonal fölé.

>[AZURE.NOTE] Ha a virtuális egynél több hálózati csatlakoztatva van, jelölje be az egyes diagnosztizálása és onnan egy virtuális hálózati csatlakozási problémákat tapasztal a NICS hatékony útvonalak.

### <a name="view-effective-routes-for-a-virtual-machine"></a>A virtuális gép hatékony útvonalak megtekintése

Az összesítő útvonalak egy virtuális alkalmazott megtekintéséhez tegye a következőket:

### <a name="view-effective-routes-for-a-network-interface"></a>A hálózati kapcsolat hatékony útvonalak megtekintése

Az összesítő útvonalak alkalmazott a hálózati kapcsolat megtekintéséhez tegye a következőket:

1.  Indítsa el az Azure PowerShell-munkamenetet, és jelentkezzen be az Azure. Ha nem ismeri a Azure PowerShell, olvassa el a [hogyan telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) .

2.  A következő parancsot a hálózati illesztő *VM1-NIC1* nevű az erőforráscsoport *RG1*alkalmazott összes útvonalak adja eredményül.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Ha nem tudja, hogy a hálózati kapcsolat neve, írja be a következő parancsot a egy erőforrás group.* hálózati adaptereken azoknak a lekérdezni

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    A következő kimenet minden az alhálózathoz a hálózati kártya csatlakoztatva van alkalmazva továbbításához kimenetét hasonlít:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Figyelje meg az eredményben a következőket:
    - **Név**: hatékony útvonal neve üres, lehet, kivéve, ha explicit módon megadva, felhasználó által definiált útvonalak. 
    - **Állapot**: a hatékony útvonal állapotát jelzi. A lehetséges értékek "Aktív" vagy "Érvénytelen":
    - **AddressPrefixes**: Itt adhatja meg a cím előtagját hatékony útvonal CIDR formában. 
    - **nextHopType**: azt jelzi, a következő Ugrás az adott útvonal. Lehetséges értékei *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*vagy *Null*. A *Null* értéket **nextHopType** egy UDR az érvénytelen útvonal jelezheti. Például **nextHopType** *VirtualAppliance* és a hálózat virtuális esetén készülék virtuális nem szerepel egy kiépítéstől/futó állam. **NextHopType** *VPNGateway* , és nincs kiépítve/futtatása a megadott VNet az átjáró nem, az útvonal előfordulhat, hogy érvénytelenné válnak.
    - **NextHopIpAddress**: Adja meg a következő ugrás a hatékony útvonal IP-címét.
    
    A következő parancsot egy könnyebben táblázat megtekintése az útvonalak adja eredményül:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    A következő kimenet néhány a kimenet kapott a korábban leírt eset:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Nem látható a *WestUS-VNet3* VNet (előtag 10.10.0.0/16)* * a *WestUS-VNet1* (előtag 10.9.0.0/16) az eredményben az előző lépésben az útvonal nem. Ahogy az alábbi képen látható, a *WestUS-VNet3* VNet VNet peering csatolásra *kapcsolat* állapotban van.
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    A kétirányú kapcsolat számára a peering megszakad, amely megtudhatja, miért VM1 nem lehet kapcsolódni a *WestUS-VNet3* VNet a VM3. Kétirányú VNet peering hivatkozás ismét a telepítő *WestUS-VNet1* és *WestUS-VNet3* VNets. A kimenet visszaadott után a VNet peering hivatkozás megfelelően folyamatban van a következőképpen:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Miután határozhatja meg a problémát, hozzáadása, eltávolítása, vagy módosíthatja útvonalak és irányítja a táblázatok. Írja be az erre szolgáló parancsok listáját a következő parancsot:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Megfontolandó szempontok

Néhány dolog, amit érdemes észben tartania, amikor a visszaadott útvonalak listájának áttekintése:

- A leghosszabb előtag hol.van (LPM) között UDRs, továbbítás alapul BGP és a rendszer útvonalak. Ha egynél több útvonalat az azonos LPM egyező, akkor útvonal lesz kijelölve alapuló saját származási a következő sorrendben:
    - Felhasználó által definiált továbbítására
    - BGP továbbítására
    - A rendszer (alapértelmezett) továbbítására

    Hatékony útvonalak csak megtekintheti az összes lehetséges útvonal alapján LPM egyező hatékony útvonalak. Hogyan útvonalak ténylegesen kiértékelt, üres egy adott hálózati kártya megjelenítésével így adott utakat, előfordulhat, hogy a virtuális/közötti kapcsolatot kell érintő hibák elhárítása a műveletet egyszerűbbé.

- Ha UDRs és forgalmat a hálózati virtuális a készülék (NVA), a *VirtualAppliance* **nextHopType**, küld, győződjön meg arról, hogy IP-továbbítás engedélyezve van-e a NVA a forgalom fogadása vagy csomagok megszakadnak. 
- Kényszerített tunneling engedélyezve van, ha az összes kimenő internetes forgalmat a helyszíni továbbítja. Ezt a beállítást, attól függően, hogy a forgalmat a helyszíni kezelésének RDP/SSH az internetről szeretne a virtuális nem működik. 
  Kényszerített tunneling engedélyezése:
    - Webhely VPN, használata nextHopType VPN-átjárót, mint a felhasználó által definiált útvonal (UDR) beállításával
    - Ha az alapértelmezett útvonal van közzététel BGP keresztül
- VNet peering forgalmának megfelelően működjön a rendszer útvonal **nextHopType** *VNetPeering* peered VNet előtag tartomány léteznie kell. Ha például egy útvonal nem létezik, és a VNet peering hivatkozás néz ki az OK gombra:
    - Várjon néhány másodpercet, és próbálkozzon újra, ha meg-e egy újonnan létrehozott peering hivatkozást. Időnként hosszabb ideig tart propagálása útvonalak alhálózat a hálózati kapcsolatokat.
    - Hálózati biztonsági csoport (NSG) szabályok a forgalom érintő is lehet. További információt a [Hálózat biztonsági csoportok – Problémamegoldás](virtual-network-nsg-troubleshoot-powershell.md) témakört is.
