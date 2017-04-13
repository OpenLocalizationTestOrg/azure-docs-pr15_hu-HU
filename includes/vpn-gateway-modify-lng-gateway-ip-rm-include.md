Az átjáró IP-cím módosításához használja a `New-AzureRmVirtualNetworkGatewayConnection` parancsmag. Annyi, hogy a helyi hálózati átjáró neve pontosan ugyanaz, mint a már meglévő nevet, a beállítások felülírja. Ekkor a "Beállítása" parancsmag nem támogatja az átjáró IP-címének módosítása.

### <a name="gwipnoconnection"></a>Az átjáró IP-cím – nincs átjáró kapcsolat módosítása

Frissítse a helyi hálózati átjáró még nincs kapcsolat a gateway IP-címe, használja az alábbi példában. A cím prefixumokban frissítheti egyszerre is. A megadott beállítások felülírja a meglévő beállításokat. Ne felejtse el, a helyi hálózati átjáró meglévő nevével. Ha nem, létre új helyi hálózaton az átjáró nem írja felül a meglévőt.

Használja a következő példában cseréje a saját értékeit.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Az átjáró IP-cím – a meglévő átjáró kapcsolat módosítása

Ha egy átjáró kapcsolat már létezik, először kell a kapcsolat eltávolításához. Ezután az átjáró IP-cím módosítása, és hozza létre az új kapcsolatot. A virtuális Magánhálózati kapcsolat néhány állásidőt eredményezi.


>[AZURE.IMPORTANT] A virtuális Magánhálózati átjáró nem törölheti. Ha így tesz, térjen vissza a lépéseket követve hozza létre újra, valamint az IP-címet, amely jogokat kap az újonnan létrehozott átjáró a helyszíni útválasztó átkonfigurálása keresztül kell.
 

1. A kapcsolat eltávolításához. A kapcsolat nevének megkeresése segítségével a `Get-AzureRmVirtualNetworkGatewayConnection` parancsmag.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName

2. Módosítsa a GatewayIpAddress értékét. Módosíthatja is a cím prefixumokban ebben az esetben, ha szükséges. Megjegyzés: Ez a művelet felülírja a meglévő átjáró helyi hálózati beállítások. A meglévő nevet a helyi hálózaton átjáró melyikkel, hogy felülírja a beállítások módosításával. Ha nem, létre új helyi hálózaton az átjáró nem módosítja a meglévőt.

        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName

3. A kapcsolat létrehozása. Ebben a példában akkor állítja be egy IPsec kapcsolat típusát. Ön hozza létre a kapcsolatot, használatakor a megadott kapcsolat típusát a konfigurációban. További kapcsolatok típusai a [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lapon talál.  Szerezze be a VirtualNetworkGateway nevét, futtassa a `Get-AzureRmVirtualNetworkGateway` parancsmag.

    Állítsa be a változók:

        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName

    A kapcsolat létrehozása:
    
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

