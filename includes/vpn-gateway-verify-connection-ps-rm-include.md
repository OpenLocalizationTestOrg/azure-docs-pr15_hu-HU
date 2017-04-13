Ellenőrizheti, hogy a kapcsolat használatával sikeresen befejeződött a `Get-AzureRmVirtualNetworkGatewayConnection` parancsmag, vagy azok nélkül `-Debug`. 

1. Használja a következő parancsmagot példában konfigurálása a megfelelő saját értékeket. Ha a rendszer kéri, válassza a "A" futtatásához a Mindet". A példában szereplő `-Name` létrehozott, és meg szeretné vizsgálni a kapcsolat neve hivatkozik.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. A parancsmaggal után megtekintése az értékeket. Az alábbi példában a kapcsolati állapot látható az "Connected", és láthatja, hogy be- és kilépési bájt.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }