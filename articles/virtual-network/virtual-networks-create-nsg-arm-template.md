<properties
   pageTitle="NSGs létrehozása sablon használatával ARM módban |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, és a sablon használatával ARM NSGs terjesztése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>Hogyan hozhat létre NSGs sablon használatával

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Is megtekintheti [a klasszikus telepítési modell NSGs létrehozása](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>Egy sablon fájl erőforrásainak NSG

Megjelenítése, és töltse le a [minta sablon](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

Az alábbi szakaszt az előtér NSG, alapján a forgatókönyv a fenti definícióját jeleníti meg.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Az előtér az alhálózathoz NSG társítani, akkor a alhálózat meghatározása a sablon módosítása, és használja az azonosító a NSG.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Figyelje meg, ugyanezt a háttér NSG és a hátsó vége alhálózat a sablonban hajt végre.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM sablon üzembe telepítéséhez kattintson használatával

Az űrlapsablonok érhető el a nyilvános adattárban az alapértelmezett értéket a fent leírt forgatókönyv létrehozásához használt tartalmazó paraméter fájlt használ. Ezzel a sablonnal üzembe, kattintson [erre a hivatkozásra](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG)kattintás használatával üzembe helyezéséhez kattintson a **központi telepítés az Azure**, szükség esetén az alapértelmezett paraméterértékeket cseréje, és kövesse az utasításokat a portálon.

## <a name="deploy-the-arm-template-by-using-powershell"></a>ARM sablon üzembe PowerShell használatával

ARM meg a letöltött sablont PowerShell használatával telepítéséhez kövesse az alábbi lépéseket.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.

3. Futtassa a **`New-AzureRmResourceGroup`** parancsmagot a sablonnal erőforrás csoport létrehozásához.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Várt kimenet:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM sablon telepítése az Azure CLI használatával

ARM sablon az Azure CLI telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa a **`azure config mode`** erőforrás-kezelő módban, az alább látható módon válthat parancsot.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

4. Futtassa a **`azure group deployment create`** az új VNet telepítse a sablon és a paraméter segítségével parancsmag fájlok letöltött és a fenti módosított. A kimenet után megjelenik a paramétereket ismerteti.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Várt kimenet:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **-n (vagy--neve)**. A csoport nevét, az erőforrás létrehozni.
    - **-l (vagy--helyről)**. Azure terület, ahol hozható létre az erőforráscsoport.
    - **az f-(vagy – sablon-fájl)**. Fájl elérési útját a ARM sablont.
    - **-e (vagy – paraméterek-fájl)**. A ARM paraméterek fájl elérési útja.
