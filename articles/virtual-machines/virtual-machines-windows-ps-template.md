<properties
    pageTitle="Hozzon létre egy virtuális az erőforrás-kezelő sablonnal |} Microsoft Azure"
    description="Egy erőforrás-kezelő sablon és a PowerShell használatával egyszerűen készíthet egy új Windows virtuális számítógépre."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Az erőforrás-kezelő sablonnal Windows virtuális gép létrehozása

Ez a cikk bemutatja az erőforrás-kezelő Azure-sablon, és megtudhatja, hogy miként telepítheti a PowerShell használatával. A sablon egy új virtuális hálózatban az egyetlen alhálózat Windows Server operációs rendszert futtató egyetlen virtuális gép üzembe helyezése.

A jelen cikkben ismertetett lépések végrehajtásához körülbelül 20 perc kell vennie.

> [AZURE.IMPORTANT] Ha azt szeretné, hogy a virtuális egy elérhetőségének beállítása részét, felveheti a beállítás a virtuális létrehozásakor. Jelenleg nem kínál egy virtuális egy elérhetővé után lett létrehozva.

## <a name="step-1-create-the-template-file"></a>Lépés: 1: A sablon fájl létrehozása

Létrehozhat saját sablon használatával [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)található információkat. [Azure QuickStarts csomagban](https://azure.microsoft.com/documentation/templates/)sablonok-készült sablonok is telepítheti.

1. Nyissa meg a kedvenc szövegszerkesztőben, és a szükséges sémának elem és a szükséges contentVersion elem hozzáadása:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Paraméterek](../resource-group-authoring-templates.md#parameters) nem mindig kötelező, de azok kínál bemeneti értékek, ha telepíti a sablont. A paraméterek elemet, valamint az annak alárendelt elemeinek felvétele a contentVersion elem után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Változók](../resource-group-authoring-templates.md#variables) értékek, amelyek gyakran módosíthatja és értékek, amelyek létre kívánja hozni az adatok kombinációjának paraméter megadása sablon használható. A változók elem hozzáadása a paraméterek szakasz után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. A sablon ezután definiálva [erőforrások](../resource-group-authoring-templates.md#resources) , például a virtuális gép, a virtuális hálózat és a tárterület-fiókot. Az erőforrások szakasz hozzáadása a változók szakasz után:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Ez a cikk a Windows Server operációs rendszer verziója fut virtuális gép hoz létre. Más képet kiválasztásával kapcsolatos további tudnivalókért lásd: a [Navigálás és a Windows PowerShell és az Azure CLI választó Azure virtuális gép képekkel](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Mentse a sablont fájlt *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Lépés: 2: A paraméterek fájl létrehozása

Az erőforrás-paraméterek a sablonban definiált értékei megadásához használatosak, amikor a sablon telepítik értékeket tartalmazó paraméterek fájl létrehozása.

1. A szövegszerkesztőben a JSON-tartalom másolása *Parameters.json*nevű új fájlra mutató:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Lásd: További tudnivalók a [felhasználónév és jelszó követelményeknek](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. A paraméterek fájl mentése

## <a name="step-3-install-azure-powershell"></a>Lépés 3: Azure PowerShell telepítése

Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) információt az Azure PowerShell legújabb verzióját, jelölje ki az előfizetés, és bejelentkezés az a fiókjába.

## <a name="step-4-create-a-resource-group"></a>Lépés: 4: Az erőforrás csoport létrehozása

Az összes erőforrások [erőforráscsoport](../azure-resource-manager/resource-group-overview.md)kell telepíthető.

1. Lista beszerzése az elérhető helyek, ahol erőforrások hozhatók létre.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. **$LocName** értékének cserélje a listából, például a **Központi USA -beli**hely. Hozzon létre a változó.

        $locName = "location name"
        
3. **$RgName** értékének cserélje az új erőforráscsoport a nevét. Hozzon létre a változó és az erőforráscsoport.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Ez a példa hasonló kell megjelennie:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>5 lépés: Az erőforrások létrehozása a sablon és a paraméterek

**$TemplateFile** értékének cserélje az elérési utat és a sablonfájl nevét. **$ParameterFile** értékének cserélje az elérési utat és a paraméterek fájl nevét. A változók létrehozása és telepítheti a sablont. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Sablonok és a paraméterek Azure tárterület-fiókból is telepítheti. További tudnivalókért lásd: [Azure PowerShell használatá Azure adathordozós](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Következő lépések

- A telepítési problémák volt, a következő lépésben akkor is megtekintheti a [Hibaelhárítás erőforrás csoport telepítések Azure Portal segítségével](../resource-manager-troubleshoot-deployments-portal.md)
- Megtudhatja, hogy miként kezelheti a virtuális gép megtekintésével [kezelése virtuális gépeken futó Azure erőforrás-kezelő és a PowerShell használatával](virtual-machines-windows-ps-manage.md)létrehozott.
