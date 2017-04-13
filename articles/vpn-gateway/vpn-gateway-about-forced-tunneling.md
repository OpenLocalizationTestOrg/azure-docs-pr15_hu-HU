<properties 
   pageTitle="Állítsa be a webhely hibák a klasszikus telepítési modell kényszerített tunneling |} Microsoft Azure"
   description="Hogyan szeretné irányítani, illetve "kényszerítése" összes internetes kötött forgalmat a helyszíni helyére."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a>Klasszikus telepítési modellt használja a kényszerített tunneling konfigurálása

> [AZURE.SELECTOR]
- [A PowerShell - klasszikus](vpn-gateway-about-forced-tunneling.md)
- [A PowerShell - erőforrás-kezelő](vpn-gateway-forced-tunneling-rm.md)

Kényszerített tunneling meghatározhatja, átirányítás vagy a "kötelező" összes internetes kötött forgalom biztonsági másolatot a helyszíni helyre a webhely VPN alagutas vizsgálati és naplózási keresztül. Ez a legtöbb nagyvállalati informatikai kritikus biztonsági követelmény házirendeket. 

Kényszerített tunneling nélkül Internet kötött-forgalmat a VMs Azure-ban a fog mindig bejárása az Azure hálózati infrastruktúrát közvetlenül ki az internethez, a vezérlőt, amellyel lehetővé teszi, hogy vizsgálja meg, vagy a forgalmat a naplózandó nélkül. Ezzel az illetéktelen Internet-hozzáférés esetleg vezethet illetéktelen vagy más típusú biztonsági szabályok megsértésével.

Ez a cikk azt ismerteti, hogy konfigurálása, kényszerített tunneling a klasszikus telepítési modell alapján létre virtuális hálózatok között. 

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Telepítési modellek és a kényszerített bújtatásra eszközök**

Kényszerített közötti kapcsolatot a klasszikus telepítési modell és az erőforrás-kezelő telepítési modell beállíthatók. További információt az alábbi táblázatban látható. Új cikkek, új telepítési modellek és további eszközök ebben a konfigurációban elérhetővé válnak, frissítjük a táblázatban. Egy cikk érhető el, ha azt csatolása közvetlenül az értékeket.

[AZURE.INCLUDE [vpn-gateway-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="requirements-and-considerations"></a>Követelmények és kapcsolatos szempontok

Kényszerített tunneling Azure virtuális hálózati a felhasználó által definiált útvonalak (UDR) keresztül van beállítva. Forgalom átirányítása a helyszíni webhely alapértelmezett útvonalat az Azure virtuális Magánhálózati átjáró kifejezve. A következő szakaszt az aktuális útválasztási táblázat és korlátozása útvonalak Azure virtuális hálózat sorolja fel:


-  Minden egyes virtuális hálózati alhálózat rendszer beépített, útválasztási táblázat tartalmaz. A rendszer útválasztási táblázat tartalmaz, a következő három csoport útvonalak:

    - **Helyi VNet irányítja:** Közvetlenül az a cél VMs virtuális ugyanabba a hálózatba
    
    - **Helyileg útvonalon:** Az Azure virtuális Magánhálózati átjáró
    
    - **Alapértelmezett útvonal:** Közvetlenül az internetkapcsolat. A saját IP-címek nem vonatkozik az előző két útvonalak szánt csomagokat, azokat a program eltávolítja.


-  Az verziójának megjelenése után a felhasználó által definiált utakat hozzon létre egy alapértelmezett út hozzáadása útválasztási táblát, és majd társítani ahhoz, hogy ezek alhálózat kényszerített tunneling a VNet subnet(s) az útválasztási táblát.

- Kell beállítania egy "alapértelmezett hely" az idegen helyszíni helyi webhelyek között a virtuális hálózathoz csatlakoztatva.

- Kényszerített tunneling kell társítani egy VNet, amelynek a dinamikus útválasztási VPN átjáró (nem egy statikus).
 
- Kényszerített tunneling készült ExpressRoute van beállítva, ez az eljárás keresztül, de ehelyett szerint engedélyezve van egy alapértelmezett útvonalat a készült ExpressRoute BGP peering munkamenetek keresztül hirdetési. Talál további információt a [Készült ExpressRoute dokumentációt](https://azure.microsoft.com/documentation/services/expressroute/) .



## <a name="configuration-overview"></a>Beállítások áttekintése

A következő példában az alhálózathoz nem kényszerített. Frontend bújtatott. A feladatok a Frontend alhálózat az elfogadás és ügyfél kérelmekre válaszolni az internetről közvetlenül is. A réteg közép- és Kódmentes alhálózat tudnak bújtatott. Minden kimenő kapcsolatok, a következő két alhálózat az internethez kényszerített vagy egy helyszíni webhelyen keresztül az S2S VPN alagutakat átirányítva lesz.

Lehetővé teszi, hogy korlátozza, és nézze meg a virtuális gépeken futó Internet hozzáférést vagy felhőszolgáltatásokba az Azure, miközben a több szálon szolgáltatás architektúra szükséges ahhoz, hogy továbbra. Is alkalmazhat kényszerített tunneling a teljes virtuális hálózatokhoz ha vannak olyan nincs internetes munkaterhelésekből a virtuális hálózatok.


![Kényszerített Tunneling](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)



## <a name="before-you-begin"></a>Első lépések

Ellenőrizze, hogy rendelkezik-e az alábbi elemek konfigurációs megkezdése előtt.

- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

- A beállított virtuális hálózati. 

- Az Azure PowerShell-parancsmagok legújabb verzióját. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.


## <a name="configure-forced-tunneling"></a>Kényszerített tunneling konfigurálása

Az alábbi lépésekkel megadhatja, hogy egy virtuális hálózati kényszerített tunneling nyújt segítséget. Beállítási lépések megegyeznek a VNet hálózati konfigurációs fájl.



    <VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>

Ebben a példában a virtuális "MultiTier-VNet" hálózatnak három alhálózathoz: *Frontend* *Midtier*és *Kódmentes* alhálózat, négy közötti helyileg kapcsolatok: *DefaultSiteHQ*, és a három *használja*. 

A lépéseket a *DefaultSiteHQ* legyen az alapértelmezett webhely kapcsolat a kényszerített tunneling, és állítsa be a Midtier és Kódmentes alhálózat használandó kényszerített tunneling.


1. Hozzon létre egy útválasztási táblázatot. Használja a következő parancsmagot a útvonal táblázat létrehozásához.

        New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"

2. Az alapértelmezett útvonal elhelyezése a útválasztási táblázatban. 

    Az alábbi példa összeadja az alapértelmezett útvonalat az útválasztási táblához, az 1. Megjegyzés: a csak útvonal támogatott "0.0.0.0/0" a "VPNGateway" NextHop cél előtagját.
 
        Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway

3. Rendelje hozzá az útválasztási táblázat az alhálózathoz. 

    Miután egy útválasztási táblát hoz létre, és egy útvonalat, használatával az alábbi példa hozzáadása és hozzárendelése az útvonal tábla VNet alhálózat. A példában az útvonal "MyRouteTable" táblázat hozzáadása a Midtier és Kódmentes alhálózat VNet MultiTier-VNet.

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"

        Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"

4. Rendeljen egy alapértelmezett hely, a kényszerített tunneling. 

    Az előző lépésben a minta parancsmag parancsfájlok hozta létre a útválasztási táblázatot, és két a VNet alhálózat az útvonal tábla kapcsolódó. A hátralévő lépésként helyi webhely között a virtuális hálózati kapcsolatainak több webhelyen válassza az alapértelmezett hely vagy alagutas.

        $DefaultSite = @("DefaultSiteHQ")
        Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"

## <a name="additional-powershell-cmdlets"></a>További PowerShell-parancsmagok

### <a name="to-delete-a-route-table"></a>Az útvonal táblázat törlése

    Remove-AzureRouteTable -Name <routeTableName>

### <a name="to-list-a-route-table"></a>Útvonal táblázat listához

    Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]

### <a name="to-delete-a-route-from-a-route-table"></a>Egy útvonal útvonal táblázat törlése

    Remove-AzureRouteTable –Name <routeTableName>

### <a name="to-remove-a-route-from-a-subnet"></a>Ha el szeretne távolítani egy útvonal alhálózat

    Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-list-the-route-table-associated-with-a-subnet"></a>Az útvonal táblázat alhálózat társított listázása
    
    Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a>Ha el szeretne távolítani egy alapértelmezett hely VNet VPN az átjárók

    Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>






