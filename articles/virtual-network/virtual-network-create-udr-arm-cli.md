<properties 
   pageTitle="Továbbítás határozzák meg, és használja a virtuális készülékek az erőforrás-kezelő használatával az Azure CLI |} Microsoft Azure"
   description="Megtudhatja, hogy miként szabályozhatja a továbbítás, és az Azure CLI használatáról virtuális készülékek használata"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Létrehozása az Azure CLI felhasználói útvonalak (UDR)

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Is megtekintheti [a klasszikus telepítési modell UDRs létrehozása](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

A minta Azure CLI parancsok az alábbi fejlemények egy egyszerű környezet már létrehozott alapján a forgatókönyv a fenti. Ha szeretne a dokumentumban megjelenített parancsokat, először hozza létre a tesztkörnyezetben úgy, hogy [Ez a sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before)üzembe helyezése, kattintson a **központi telepítés az Azure**, szükség esetén az alapértelmezett paraméterértékeket cseréje és kövesse az utasításokat a portálon.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Az előtér-alhálózat a UDR létrehozása
Az útvonal tábla és a alapján a forgatókönyv a fenti az előtér-alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a **`azure network route-table create`** az előtér-alhálózat útvonal tábla létrehozása parancsot.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Kimenet:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Paraméterek:
    - **-g (vagy – erőforrás-csoport)**. Az erőforráscsoport, ahol a UDR létrejön neve. A példában *TestRG*.
    - **-l (vagy--helyről)**. Azure terület, ahol az új UDR létrejön. A példában *westus*.
    - **-n (vagy--neve)**. Az új UDR nevét. A példában *UDR-FrontEnd*.

4. Futtassa a **`azure network route-table route create`** az útvonal táblázat összes forgalmat a hátsó vége alhálózat (192.168.2.0/24) **FW1** virtuális (192.168.0.4) szánt az előbb létrehozott útvonal létrehozása parancsot.

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Kimenet:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Paraméterek:
    - **-r (vagy--útvonal táblanév)**. Hol lehet hozzáadni az útvonal útvonal tábla nevét. A példában *UDR-FrontEnd*.
    - **-(vagy – cím-előtag)**. Hol csomagok vannak szánt alhálózat előtag címet. A példában *192.168.2.0/24*.
    - **-y (vagy--tovább jellegű azonosítható)**. Az objektum adatforgalom típusa küld. Lehetséges értékei *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *internetes*és *nincs*.
    - **-p (vagy--tovább-azonosítható ip-cím**). Következő ugrás IP-címét. A példában *192.168.0.4*.

5. Futtassa a **`azure network vnet subnet set`** társíthatja az útvonal táblázat a **FrontEnd** alhálózat az előbb létrehozott parancsot.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Kimenet:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Paraméterek:
    - **-e (vagy--vnet neve)**. A VNet, ahol az alhálózathoz található neve. A példában *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>A visszahívási vége alhálózat a UDR létrehozása
Az útvonal táblázat és az alkalmazási példát alapján vissza vége alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

1. Futtassa a **`azure network route-table create`** vissza vége alhálózat útvonal tábla létrehozása parancsot.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Futtassa a **`azure network route-table route create`** az útvonal táblázat összes adatforgalmat az előtér alhálózathoz (192.168.1.0/24) **FW1** virtuális (192.168.0.4) küldése az előbb létrehozott útvonal létrehozása parancsot.

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Futtassa a **`azure network vnet subnet set`** társíthatja az útvonal táblázat a **Kódmentes** alhálózat az előbb létrehozott parancsot.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>IP-továbbítás FW1 engedélyezése
Ha engedélyezni szeretné a hívásátirányítás a hálózati kártya **FW1**által használt IP, kövesse az alábbi lépéseket.

1. Futtassa a **`azure network nic show`** parancsot, és figyelje meg az érték **Engedélyezése IP**-továbbításához. *Hamis értékre*kell állítható.

        azure network nic show -g TestRG -n NICFW1

    Kimenet:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Futtassa a **`azure network nic set`** IP-továbbítás engedélyezése parancsot.

        azure network nic set -g TestRG -n NICFW1 -f true

    Kimenet:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Paraméterek:

    - **az f-(vagy – IP--továbbítás engedélyezése)**. *Igaz* vagy *hamis*.
