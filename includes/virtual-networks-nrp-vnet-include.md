## <a name="virtual-network"></a>Virtuális hálózati
Virtuális hálózatok (VNET) és alhálózat forrásokkal az Azure-ban futó feladatok biztonsági oszlopazonosító jobboldali határozza meg. Egy VNet jellemzi cím szóközök, jelenti CIDR blokkok gyűjteménye. 

>[AZURE.NOTE] Hálózati rendszergazda jól ismert CIDR alakban. Ha nem ismeri a CIDR, [kaphat részletesebb tájékoztatást](http://whatismyipaddress.com/cidr).

![A több alhálózat VNet](./media/resource-groups-networking/Figure4.png)

VNets a következő tulajdonságokat tartalmazza.

|A tulajdonság|Leírás|Minta értékek|
|---|---|---|
|**addressSpace**|Cím prefixumokban a VNet CIDR jelöléssel alkotó gyűjteménye|192.168.0.0/16|
|**alhálózat**|A VNet alkotó alhálózat gyűjteménye|az alábbi [alhálózat](#Subnets) témakörben található.|
|**IP-cím**|Az objektum hozzárendelt IP-címet. Az írásvédett tulajdonságot.|104.42.233.77|

### <a name="subnets"></a>Alhálózat
Alhálózat egy VNet gyermek erőforrást, és segít definiálása szegmensek cím szóközök belül egy CIDR szövegrészt, IP-cím prefixumokban használatával. Hálózati kártyák alhálózat hozzáadva, és kapcsolatot biztosító különböző munkaterhelésekből VMs csatlakozik.

Alhálózat a következő tulajdonságokat tartalmazza. 

|A tulajdonság|Leírás|Minta értékek|
|---|---|---|
|**addressPrefix**|A alhálózat jelölés CIDR alkotó egyetlen cím előtag|192.168.1.0/24|
|**networkSecurityGroup**|Az alhálózathoz alkalmazott NSG|Lásd: [NSGs](#Network-Security-Group)|
|**routeTable**|Az alhálózathoz alkalmazott útvonal táblázat|Lásd: [UDR](#Route-table)|
|**ipConfigurations**|Az alhálózathoz csatlakoztatott NIC által használt IP-konfigurációs objektumok gyűjteménye|Lásd: [UDR](#Route-table)|


Minta VNet JSON formátumban:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>További források

- [VNet](../articles/virtual-network/virtual-networks-overview.md)további információhoz juthat.
- Olvassa el a [REST API-hivatkozás dokumentáció](https://msdn.microsoft.com/library/azure/mt163650.aspx) VNets számára.
- Az alhálózathoz, olvassa el a [REST API-hivatkozás dokumentációt](https://msdn.microsoft.com/library/azure/mt163618.aspx) .