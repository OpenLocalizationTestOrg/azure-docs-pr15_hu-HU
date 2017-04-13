<properties
   pageTitle="Egy statikus, nyilvános IP-, erőforrás-kezelő sablon használatával egy virtuális telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként üzembe VMs egy statikus, nyilvános IP-, erőforrás-kezelő sablon használatával"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Az egy statikus nyilvános IP-cím sablon használatával egy virtuális terjesztése

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klasszikus telepítési modell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>A sablonfájlt nyilvános IP-erőforrások

Megjelenítése, és töltse le a [minta sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Az alábbi szakaszt nyilvános IP-erőforrás, alapján a forgatókönyv a fenti definícióját jeleníti meg.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Figyelje meg a **publicIPAllocationMethod** tulajdonság értéke *statikus*. Lehet, hogy ez a tulajdonság *dinamikus* (alapértelmezett érték-) vagy *statikus*. Beállítást, amely a nyilvános IP-cím soha nem változik, statikus garanciákkal.

Az alábbi szakaszt hálózati kezelőfelületen jelennek meg a nyilvános IP-címének a társítás jeleníti meg.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Figyelje meg a **publicIPAddress** tulajdonság **variables('webVMSetting').pipName**nevű erőforrás **azonosító** mutatnak. Ez a fent látható nyilvános IP-erőforrás nevét.

Végül a fenti hálózati kapcsolaton jelenik meg a létrehozandó virtuális **networkProfile** tulajdonsága.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>A sablon üzembe telepítéséhez kattintson használatával

Az űrlapsablonok érhető el a nyilvános adattárban az alapértelmezett értéket a fent leírt forgatókönyv létrehozásához használt tartalmazó paraméter fájlt használ. Ez a sablon használatával kattintson üzembe helyezéséhez kattintson **az Azure Deploy** a [virtuális statikus MEZŐPONT a](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) sablon Readme.md fájlban. Az alapértelmezett paraméter értékek cseréje, ha azt szeretné, és adja meg az értékeket az üres paraméterek.  Az utasításokat követve a portálon hozhat létre virtuális gép nyilvános statikus IP-cím.

## <a name="deploy-the-template-by-using-powershell"></a>A sablon üzembe PowerShell használatával

A PowerShell használatával letöltött sablont telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../powershell-install-configure.md) , és kövesse a lépéseket 1 – 3.

2. Egy PowerShell konzolban futtassa **Új-AzureRmResourceGroup** hozhat létre egy új erőforráscsoport, ha szükséges. Ha már létrehozott egy erőforrás csoport, ugorjon a 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Várt kimenet:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Egy PowerShell konzolban futtassa **Új-AzureRmResourceGroupDeployment** bevezetését tervezi a sablont.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Várt kimenet:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>A sablon telepítése az Azure CLI használatával

A sablon az Azure CLI telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, hajtsa végre a [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) cikk lépéseinek, majd a lépéseket a CLI csatlakozhat az [Azure-előfizetésbe a az Azure parancssori kezelőfelületről Azure csatlakozás](../xplat-cli-connect.md) cikk "Segítségével a azure bejelentkezési hitelesítő interaktív" szakaszban az előfizetés.
2. Futtassa az erőforrás-kezelő mód, váltson a **azure konfiguráció mód** parancsot, alább látható módon.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Nyissa meg a [Paraméterek fájlt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), jelölje ki a tartalmát, és mentse a fájlt a számítógépen. Ebben a példában a paraméterek *parameters.json*nevű fájlt menti. Módosítsa a paraméterértékeket, a fájlban, ha azt szeretné, de legalább azt ajánljuk, hogy egy egyedi, összetett jelszó módosítása a adminPassword paraméter értéke.

4. Futtassa **telepítési azure csoport létrehozása** az új VNet telepítse a sablon és a paraméterek fájlokat töltötte le, és a fenti módosított segítségével. Az alábbi parancsot, a csere <path> útvonala meg a fájlt mentette. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Várt eredményt ad (használt paraméterértékeket listákkal):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
