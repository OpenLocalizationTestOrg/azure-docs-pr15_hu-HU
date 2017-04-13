Létre kell hoznia egy VNet és az átjáró alhálózat először előtt a következő tevékenységek kezelése. A témakör [a klasszikus portálon virtuális hálózat konfigurálása](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) további információt.   

## <a name="add-a-gateway"></a>Egy átjáró felvétele

A paranccsal az alábbi átjáró létrehozása. Ügyeljen arra, hogy helyett a saját értékeket.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Ellenőrizze az átjáró hozták létre

A paranccsal az alábbi ellenőrizze, hogy az átjáró létrehoztak. Ez a parancs is valós az átjáró-azonosító egyéb műveleteket van szüksége.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Az átjárók átméretezése

Számos [Átjáró termékváltozatok](../articles/expressroute/expressroute-about-virtual-network-gateways.md). A következő paranccsal az átjáró Termékváltozat bármikor módosíthatja.

>[AZURE.IMPORTANT] Ez a parancs UltraPerformance átjáró nem működik. Az átjáró-UltraPerformance átjáró módosításához először távolítsa el a meglévő készült ExpressRoute átjáró, és hozza létre az új átjáró UltraPerformance. Vissza a az átjáró-UltraPerformance átjáró léptetheti, először távolítsa el az UltraPerformance átjáró, és kattintson az új átjáró létrehozása. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Az átjárók eltávolítása

A paranccsal az alábbi az átjárók eltávolítása

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>