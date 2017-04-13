<properties
   pageTitle="Végrehajtási egy könnyen hozzáférhető hibrid hálózat architektúrája |} Microsoft Azure"
   description="Hogyan egy Azure virtuális hálózat átnyúlik végrehajtása egy biztonságos webhely hálózat architektúrája, és a virtuális Magánhálózati átjáró feladatátvételi készült ExpressRoute használatával csatlakozik egy helyszíni hálózaton."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Egy könnyen hozzáférhető hibrid hálózat architektúrája végrehajtása

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti a gyakorlati tanácsok a helyszíni hálózaton csatlakoztatása az Azure virtuális hálózatok készült ExpressRoute, a webhely virtuális magánhálózat (VPN) feladatátvevő kapcsolatként használatával. A forgalmat a helyszíni és az Azure virtuális hálózat (VNet) készült ExpressRoute kapcsolaton keresztül között.  Ha a készült ExpressRoute áramkör való csatlakozással mérvű, forgalom áthaladó egy IPSec VPN-csatorna.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A tervrajz használja, erőforrás-kezelő Microsoft új telepítési javasolja.

Ez a felépítés esettel jellemző használata a következők:

- Hibrid alkalmazások hol munkaterhelésekből között egy helyszíni hálózaton és Azure van meghatározva.

- Nagyméretű, kulcsfontosságú munkaterhelésekből méretezhetőség magas fokú igénylő futó alkalmazásokat.

- Nagyméretű biztonsági mentési és visszaállítási létesítményekhez adatokért, amelyeket nem a helyszínen történő kell menteni.

- Nagy adatok munkaterhelésekből kezelése.

- Azure használatával katasztrófaelhárító helyként.

Figyelje meg, hogy a készült ExpressRoute áramkör nem érhető el, ha a virtuális Magánhálózati útvonal csak kezelje magánjellegű peering kapcsolatok. Nyilvános a peering és a kapcsolatokkal peering Microsoft továbbítja az interneten keresztül.

## <a name="architecture-diagram"></a>Architektúra diagramja

>[AZURE.NOTE] Az [Azure virtuális Magánhálózati átjáró szolgáltatás] [ azure-vpn-gateway] végrehajtja a virtuális hálózati átjárók, két típusa Virtuális Magánhálózati virtuális hálózati átjárók és készült ExpressRoute virtuális hálózati átjárók. Ebben a dokumentumban a *Virtuális Magánhálózati átjáró* utal, amelyet az Azure szolgáltatás, a mondatok *VPN virtuális hálózati átjáró* közben és *készült ExpressRoute virtuális hálózati átjáró* kifejezés szolgálnak az átjáró VPN és készült ExpressRoute megvalósítás rendre hivatkozik.

Az alábbi ábra kiemeli a fontos e architektúra összetevőket:

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "hibrid hálózat - ER VPN" lapot.

![[0]][0]

- **Azure virtuális hálózatok (VNets).** Minden egyes VNet egyetlen Azure területen található, és több alkalmazás rétegek üzemeltetheti. Alkalmazás rétegek alhálózat használ az egyes VNet kell szegmentált és/vagy a hálózati biztonsági csoportok (NSGs).

- **A helyszíni vállalati hálózathoz.** Ez a hálózat és eszközfüggetlen módon, a szervezeten belül futó helyi magánhálózaton keresztül csatlakozik.

- **Virtuális Magánhálózati készülék.** Ez az eszköz vagy szolgáltatás, ahol a külső kapcsolat a helyszíni hálózaton. A virtuális Magánhálózati készülék lehet egy hardvereszközt, vagy a Útválasztás és távoli hozzáférés szolgáltatás () a Windows Server 2012 például szoftveres megoldást.

> [AZURE.NOTE] Támogatott VPN készülékek és konfigurálásáról kijelölt VPN készülékek csatlakozhat az Azure virtuális Magánhálózati átjáró listáját, olvassa el a megfelelő eszköz a [virtuális Magánhálózati eszközök támogatják az Azure]útmutatót[vpn-appliance].

- **Virtuális Magánhálózati virtuális hálózati átjárót.** A virtuális Magánhálózati virtuális hálózati átjáró lehetővé teszi, hogy a VNet való csatlakozáshoz a virtuális Magánhálózati készülék a helyszíni hálózaton. A virtuális Magánhálózati virtuális hálózati átjáró a helyszíni hálózaton érkező kérések fogadására csak a virtuális Magánhálózati készülék keresztül van beállítva. További tudnivalókért olvassa el a [Csatlakozás Microsoft Azure virtuális hálózathoz egy helyszíni hálózaton][connect-to-an-Azure-vnet].

- **Készült ExpressRoute virtuális hálózati átjáró.** A virtuális hálózati készült ExpressRoute átjáró lehetővé teszi, hogy a helyszíni hálózati kapcsolat használt készült ExpressRoute áramkör csatlakozni a VNet.

- **Átjáró alhálózat.** A virtuális hálózati átjárók tartják ugyanahhoz az alhálózathoz.

- **Virtuális Magánhálózati kapcsolatot.** A kapcsolat tulajdonságai, adja meg a kapcsolat típusát (IPSec) és a megosztott a helyszíni VPN készülék kulcs forgalom titkosítása tartalmaz.

- **Készült ExpressRoute áramkör.** Ez az egy réteg 2, vagy az adatkapcsolat szolgáltató által biztosított layer 3 áramkör, hogy a helyszíni illesztések-hálózaton keresztül él útválasztó Azure. A kapcsolat a hardver infrastruktúra a csatlakozási szolgáltató által kezelt használja.

- **N szintű felhő alkalmazást.** Ez a az Azure-ban tárolt alkalmazást. Többszintű, az Azure terheléselosztókkal keresztül csatlakoztatott több alhálózat tartalmazhat. A forgalmat a minden alhálózathoz tartozhatnak [Azure hálózati biztonsági csoportok]használatával definiált szabályok[azure-network-security-group](NSGs). További tudnivalókért lásd: [Microsoft Azure biztonsági – első lépések][getting-started-with-azure-security].

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat.

### <a name="vnet-and-gatewaysubnet"></a>VNet és GatewaySubnet

Az azonos VNet virtuális hálózati készült ExpressRoute átjáró és a virtuális Magánhálózati virtuális hálózati átjáró létrehozása Ez azt jelenti, hogy azok érdemes megosztania ugyanahhoz az alhálózathoz **GatewaySubnet** nevű

Ha a VNet már van egy **GatewaySubnet**nevű alhálózat, gondoskodjon arról, hogy azt egy /27 vagy nagyobb címterület. Ha a meglévő alhálózat túl kicsi, távolítsa el az alábbi képlettel történik, és hozzon létre egy újat, a következő listajelet látható módon:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Ha a VNet **GatewaySubnet**nevű alhálózathoz nem tartalmaz, majd hozzon létre egy újat az alábbi képlettel történik:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>Virtuális Magánhálózat és készült ExpressRoute átjáró

Ellenőrizze, hogy a szervezet megfelel-e a [készült ExpressRoute kapcsolatban előzetesen szükséges követelményeknek] [ expressroute-prereq] Azure csatlakozhat.

Ha már van egy virtuális Magánhálózati virtuális hálózati átjárót az Azure VNet, távolítsa el azt, alább látható módon.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Kövesse a [végrehajtási egy hibrid hálózat architektúrája és a készült Azure ExpressRoute] [ implementing-expressroute] a készült ExpressRoute kapcsolatot létesíteni.

Kövesse a [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN] [ implementing-vpn] a virtuális Magánhálózati virtuális hálózati átjáró kapcsolatot létesíteni.

A virtuális hálózati átjáró kapcsolatokat hozott létre, miután tesztkörnyezetben a következőképpen:

1. Győződjön meg arról, hogy a helyszíni hálózaton lehet csatlakozni az Azure VNet.

2. Forduljon a szolgáltatójához le szeretné állítani a teszteléshez készült ExpressRoute kapcsolatot.

3. Győződjön meg arról, hogy a továbbra is csatlakozhat a helyszíni hálózaton az Azure VNet virtuális hálózati átjáró virtuális Magánhálózati kapcsolat.

4. Lépjen kapcsolatba a reestabilish készült ExpressRoute kapcsolódási szolgáltatójánál.

## <a name="considerations"></a>Megfontolandó szempontok

Az készült ExpressRoute kapcsolatos szempontok, lásd: a [végrehajtási egy hibrid hálózat architektúrája és a készült Azure ExpressRoute] [ guidance-expressroute] útmutatást.

Webhely VPN szempontok, talál [egy hibrid hálózat architektúrája Azure és a helyszíni VPN végrehajtási] [ guidance-vpn] útmutatást.

Az általános Azure biztonsági megfontolások, olvassa el [a Microsoft cloud services és]hálózati biztonsági[best-practices-security].

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

Ha egy meglévő helyszíni infrastruktúra már be van állítva egy megfelelő hálózati készülék, ezeket a lépéseket követve telepítheti a hivatkozás architektúra:

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Megvárja, amíg az Azure-portálon nyissa meg a hivatkozásra, majd tegye a következőket: 
  - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-hybrid-vpn-er-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Várja meg a hivatkozásra kattintva nyissa meg az Azure-portálon, majd adja meg, majd tegye a következőket: 
  - Jelölje ki a **meglévő használata** **erőforráscsoport** szakaszában, és adja meg `ra-hybrid-vpn-er-rg` a szövegmezőbe.
  - A **hely** legördülő listából válassza ki a régió.
  - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
  - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
  - Kattintson a **vásárlás** gombra.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Egy könnyen hozzáférhető hibrid hálózat architektúrája használatával és a virtuális Magánhálózati készült ExpressRoute átjáró architektúráját"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
