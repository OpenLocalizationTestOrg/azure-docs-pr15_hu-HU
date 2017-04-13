<properties
   pageTitle="Hozzon létre a VNet Peering erőforrás-kezelő sablonok használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre virtuális hálózati peering az erőforrás-kezelő a sablonok használatával."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Hozzon létre a VNet Peering erőforrás-kezelő sablonok használata

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Hozzon létre egy erőforrás-kezelő használatával történő peering VNet, kövesse az alábbi lépéseket:

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.

    > [AZURE.NOTE] Modul VNet peering kezelése a PowerShell-parancsmag a [Azure PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. A szöveg, az alábbi VNet2, alapján a forgatókönyv a fenti VNet1 VNet peering csatolás definíciója látható. Másolja az alábbi tartalom, és mentse azt egy VNetPeeringVNet1.json nevű fájlt.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Az alábbi szakaszt VNet1, alapján a forgatókönyv a fenti VNet2 VNet peering csatolás definíciója látható.  Másolja az alábbi tartalom, és mentse azt egy VNetPeeringVNet2.json nevű fájlt.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    A sablonban naplójában létezik néhány konfigurálható tulajdonságai VNet peering:

  	|A beállítás|Leírás|Alapértelmezett|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Engedélyezi vagy tiltja a címterület használatára, egy másik partner VNet megtalálható részeként a virtual_network címke.|igen|
  	|AllowForwardedTraffic|A forgalmat a peered VNet nem származó e elfogadták vagy eltávolítja.|nem|
  	|AllowGatewayTransit|Lehetővé teszi, hogy a partner VNet a VNet átjáró használni.|nem|
  	|UseRemoteGateways|A partner VNet átjáró használata. A partner VNet rendelkeznie kell konfigurálni az átjárók és AllowGatewayTransit kijelölve. Ez a beállítás nem használható, ha van beállítva az átjárók.|nem|

    Minden egyes VNet peering hivatkozásra a fenti tulajdonságok tartozik. Például AllowVirtualNetworkAccess beállítása TRUE VNet peering hivatkozás VNet1 VNet2, és állítsa be a VNet peering hivatkozás más irányban hamis.


4. A sablonfájlt üzembe helyezéséhez futtatását is lehetővé teszi a New-AzureRmResourceGroupDeployment parancsmag létrehozása vagy módosítása a telepítést. Erőforrás-kezelő sablonok használatával kapcsolatos további tudnivalókért olvassa el a [cikk](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Cserélje ki a csoport nevét és a sablonok erőforrásfájl szükség szerint.

    Alább példa alapján az alkalmazási példát:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Kimeneti jeleníti meg:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Kimeneti jeleníti meg:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. A telepítés befejezése után futtatását is lehetővé teszi a a peering állapotának megtekintése az alábbi parancsmagot:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Kimeneti jeleníti meg:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Ebben az esetben folyamatban van a peering után láthatja, bármely virtuális gép mindkét VNets bármely virtuális számítógépről a kapcsolatok létrehozására. Alapértelmezés szerint AllowVirtualNetworkAccess értéke igaz, és VNet peering kiépítése a megfelelő hozzáférés-vezérlési listák engedélyezi a kommunikációt VNets között. Hálózati biztonsági csoport (NSG) szabályok adott alhálózat vagy virtuális gépeken futó Access virtuális hálózatok között finom-szemek irányítását közötti kapcsolatot blokkolása továbbra is alkalmazhat.  További információt a NSG szabályokat hoz létre olvassa el a [cikk](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Hozzon létre egy előfizetésekben peering VNet, kövesse az alábbi lépéseket:

1. Jelentkezzen be az Azure-jogosultsággal rendelkező felhasználó-A előfizetés-A-fiók adatait, és futtassa a következő parancsmagot:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Ez a követelmény, nem peering hozható létre, ha a felhasználóknak egyenként előléptetése peering kérelmek esetében a megfelelő Vnets, mindaddig, amíg a kérések megfelelően. A többi VNet jogosultsággal rendelkező felhasználó hozzáadása a helyi VNet felhasználók megkönnyíti a beállítási lehetőségek.

2. Jelentkezzen be az Azure-fiók jogosultsággal rendelkező felhasználó-B előfizetés-b, és futtassa a következő parancsmagot:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. A felhasználó-A adatait a bejelentkezési folyamat, futtassa a parancsmagot:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Az alábbiakban hogyan a JSON-fájl van megadva.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Felhasználó-B bejelentkezési munkamenetre futtassa a következő parancsmagot:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Az alábbiakban a JSON-fájl definiált hogyan:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Ebben az esetben folyamatban van a peering után láthatja, különböző előfizetésekben kapcsolatainak olyan virtuális gépről bármely virtuális géphez mindkét VNets elindítására.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Ebben az esetben a VNet peering létrehozásához az alábbi űrlapsablonok telepítheti.  A AllowForwardedTraffic tulajdonság értéke igaz, amely lehetővé teszi, hogy a hálózati virtuális készülék küldéséhez és fogadásához a forgalmat a peered VNet kell.

    Az alábbiakban a sablont hozhat létre egy a HubVNet való VNet1 peering VNet. Figyelje meg, hogy AllowForwardedTraffic értéke hamis.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Az alábbiakban a sablont hozhat létre egy a VNet1 való HubVnet peering VNet. Látható, hogy AllowForwardedTraffic értéke igaz.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Miután peering folyamatban van, ez [a cikk](virtual-network-create-udr-arm-ps.md) megadása a felhasználó által definiált routes(UDR) VNet1 forgalom átirányítása egy virtuális készülék képességeit használandó keresztül is hivatkozhat. Útvonalat a következő azonosítható címet ad meg, amikor beállíthatja a virtuális készülék a partner VNet HubVNet az IP-címére.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Különböző telepítési modellekből virtuális hálózatok között peering létrehozásához kövesse az alábbi lépéseket:
1. A szöveg, az alábbi VNET2 ebben az esetben a definícióját VNET1 VNet peering csatolás látható. Azure erőforrás manager virtuális hálózathoz klasszikus virtuális hálózat peer csak egy hivatkozás van szükség.

    Ügyeljen arra, hogy az előfizetés azonosítója, ahol a klasszikus virtuális hálózati vagy VNET2 található elhelyezni, és módosítsa MyResouceGroup az erőforrás megfelelő csoport nevére.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "paraméterek": {}, a "változók": {}, a "erőforrások": [{"apiVersion": "2016-06-01", "típus": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "név": "VNET1/LinkToVNET2", "hely": "[resourceGroup () .location]", "Tulajdonságok": {"allowVirtualNetworkAccess": értéke igaz, "allowForwardedTraffic": értéke igaz, "allowGatewayTransit": értéke igaz, "useRemoteGateways": értéke igaz, "remoteVirtualNetwork": {"azonosító": "[resourceId (" Microsoft.ClassicNetwork/virtualNetworks', "VNET2")] "}}}]}

2. A sablonfájlt üzembe helyezéséhez futtassa a következő parancsmagot létrehozása vagy frissítése a telepítés.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Miután létrejött a telepítés, futtathatja a következő parancsmagot a peering állapot megtekintése:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Miután peering folyamatban van a klasszikus VNet és erőforrás-kezelő VNet között, látnia kell kapcsolatokhoz bármely virtuális számítógépről a VNET1 bármely virtuális géphez VNET2, és ez fordítva is igaz.
