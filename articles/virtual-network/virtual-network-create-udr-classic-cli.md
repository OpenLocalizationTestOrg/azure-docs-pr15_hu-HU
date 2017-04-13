<properties 
   pageTitle="Továbbítás határozzák meg, és használja az Azure CLI használata a klasszikus telepítési modell virtuális készülékek |} Microsoft Azure"
   description="Megtudhatja, hogy miként szabályozhatja az Azure CLI használata a klasszikus telepítési modell VNets köröztetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Továbbítás határozzák meg, és használja az Azure CLI használatáról virtuális készülékek (klasszikus)

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Is [vezérlő útválasztás, és használja az erőforrás-kezelő telepítési modell virtuális készülékek](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

A minta Azure CLI parancsok az alábbi fejlemények egy egyszerű környezet már létrehozott alapján a forgatókönyv a fenti. Ha szeretne a dokumentumban megjelenített parancsokat, hozzon létre a környezet, [Hozzon létre egy (klasszikus) használ az Azure CLI VNet](virtual-networks-create-vnet-classic-cli.md)látható.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Az előtér-alhálózat a UDR létrehozása
Az útvonal tábla és a alapján a forgatókönyv a fenti az előtér-alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

1. Futtassa a **`azure config mode`** klasszikus mód.

        azure config mode asm

    Kimenet:

        info:    New mode is asm

3. Futtassa a **`azure network route-table create`** az előtér-alhálózat útvonal tábla létrehozása parancsot.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Kimenet:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Paraméterek:
    - **-l (vagy--helyről)**. Azure terület, ahol az új NSG létrejön. A példában *westus*.
    - **-n (vagy--neve)**. Az új NSG nevét. A példában *NSG-FrontEnd*.

4. Futtassa a **`azure network route-table route set`** az útvonal táblázat összes forgalmat a hátsó vége alhálózat (192.168.2.0/24) **FW1** virtuális (192.168.0.4) szánt az előbb létrehozott útvonal létrehozása parancsot.

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Kimenet:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Paraméterek:
    - **-r (vagy--útvonal táblanév)**. Hol lehet hozzáadni az útvonal útvonal tábla nevét. A példában *UDR-FrontEnd*.
    - **-(vagy – cím-előtag)**. Hol csomagok vannak szánt alhálózat előtag címet. A példában *192.168.2.0/24*.
    - **-t (vagy--tovább jellegű azonosítható)**. Az objektum adatforgalom típusa küld. Lehetséges értékei *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *internetes*és *nincs*.
    - **-p (vagy--tovább-azonosítható ip-cím**). Következő ugrás IP-címét. A példában *192.168.0.4*.

5. Futtassa a **`azure network vnet subnet route-table add`** társíthatja az útvonal táblázat a **FrontEnd** alhálózat az előbb létrehozott parancsot.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Kimenet:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Paraméterek:
    - **-t (vagy--vnet neve)**. A VNet, ahol az alhálózathoz található neve. A példában *TestVNet*.
    - **- n (vagy--alhálózat neve**. Az alhálózathoz, hozzáadódik az útvonal tábla neve. A példában *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>A visszahívási vége alhálózat a UDR létrehozása
Az útvonal táblázat és az alkalmazási példát alapján vissza vége alhálózat szükséges útvonal létrehozásához kövesse az alábbi lépéseket.

3. Futtassa a **`azure network route-table create`** vissza vége alhálózat útvonal tábla létrehozása parancsot.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Futtassa a **`azure network route-table route set`** az útvonal táblázat összes adatforgalmat az előtér alhálózathoz (192.168.1.0/24) **FW1** virtuális (192.168.0.4) küldése az előbb létrehozott útvonal létrehozása parancsot.

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Futtassa a **`azure network vnet subnet route-table add`** társíthatja az útvonal táblázat a **Kódmentes** alhálózat az előbb létrehozott parancsot.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

