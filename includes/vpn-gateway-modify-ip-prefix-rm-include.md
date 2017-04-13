### <a name="noconnection"></a>Hogyan lehet hozzáadni vagy eltávolítani a prefixumokban - nincs átjáró kapcsolat

- **Hozzáadása** további cím prefixumokban a helyi hálózati átjáró, létrehozott, de nem, amely még internetkapcsolata átjárót, az alábbi példa. Győződjön meg arról, hogy módosítsa az értékeket a saját.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- **Ha el szeretne távolítani** egy cím előtag helyi hálózati két átjáró, amely nincs a virtuális Magánhálózati kapcsolat használja az alábbi példában. Hagyja meg a prefixumokban, amely már nincs szüksége. Ebben a példában azt már nem szükséges előtag 20.0.0.0/24 (a az előző példában), így frissíteni fogjuk a helyi hálózati átjáró, és zárja ki, hogy előtagot.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Hogyan lehet hozzáadni vagy eltávolítani a prefixumokban - átjáró kapcsolat meglévő

Ha az átjáró kapcsolatot hozott létre, és szeretne hozzáadni vagy eltávolítani a IP cím prefixumokban a helyi hálózati átjáró található, kell hajtsa végre sorrendben az alábbi lépéseket. A virtuális Magánhálózati kapcsolat néhány állásidőt eredményezi. A prefixumokban frissítésekor, fog először törölje a kapcsolatot, módosítsa a prefixumokban, és majd hozzon létre egy új kapcsolatot. Az alábbi példában ügyeljen arra, hogy módosítsa az értékeket a saját.

>[AZURE.IMPORTANT] A virtuális Magánhálózati átjáró nem törölheti. Ha így tesz, akkor be kell térjen vissza a lépéseket követve hozza létre újra, valamint a helyszíni útválasztó, az új beállítások átkonfigurálása keresztül.
 
1. A kapcsolat eltávolításához.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. A cím prefixumokban a helyi hálózaton átjáró módosítása.

    A változó beállítása a LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Módosítsa a prefixumokban.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. A kapcsolat létrehozása. Ebben a példában akkor állítja be egy IPsec kapcsolat típusát. Ön hozza létre a kapcsolatot, használatakor a megadott kapcsolat típusát a konfigurációban. További kapcsolatok típusai a [PowerShell-parancsmag](https://msdn.microsoft.com/library/mt603611.aspx) lapon talál.

    A változó beállítása a VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    A kapcsolat létrehozása. Ne feledje, hogy ez a példa a változó $local az előző lépésben beállított használja-e.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'
