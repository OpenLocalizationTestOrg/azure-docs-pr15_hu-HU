<properties
   pageTitle="Erőforrás-kezelő sablon forgatókönyv |} Microsoft Azure"
   description="Lépésenkénti útmutató egy erőforrás manager sablon egy egyszerű Azure IaaS architektúra kiépítési."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Erőforrás-kezelő sablon útmutató

A sablon létrehozásakor első kérdések egyike "bemutatja, hogyan nyithat?". Az egyik [sablon létrehozása a cikk](resource-group-authoring-templates.md#template-format)a leírt alapszerkezetét követő üres sablon alapján, és adja hozzá a erőforrások és a megfelelő paramétereket és a változók. Egy megfelelő helyettesítő indítsa el a [quickstart útmutató gyűjtemény](https://github.com/Azure/azure-quickstart-templates) keresztül, és keresse meg a próbál létrehozni egy hasonló helyzetben is. Egyesítés több sablont, vagy szerkesszen egy meglévőt az adott forgatókönyvet az igényeinek. 

Most tekintsünk át egy közös infrastruktúra:

* Két virtuális gépeken futó, hogy ugyanazt a tárterület-fiókot használja, a elérhetősége ugyanazok, valamint a virtuális hálózat ugyanahhoz az alhálózathoz.
* Virtuális gépeken egy hálózati kártya és a virtuális IP-címet.
* Egy terheléselosztó egy terheléskiegyenlítő szabály 80-as port

![architektúra](./media/resource-group-overview/arm_arch.png)

Ez a témakör bemutatja az adott infrastruktúra erőforrás-kezelő sablon létrehozása a lépéseket. A végleges sablont, létrehozhat egy [2 VMs terheléselosztó betöltése és terheléselosztási szabályok](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/)nevű quickstart útmutató-sablonon alapuló.

De, amely sok összeállítása egyszerre, így érdemes először hozzon létre egy tárterület-fiókot, és azokat. Miután a tárterület-fiók létrehozása van lezárt fájlrendszer, a más erőforrások, és újra telepíteni az infrastruktúra elvégzéséhez a sablon.

>[AZURE.NOTE] Szerkesztő bármilyen típusú is használhatja, ha létrehozása a sablon. Visual Studio eszközeivel sablonban fejlesztési egyszerűbb, de nem szükséges az oktatóprogram elvégzéséhez a Visual Studio. A központi Web App- és SQL-adatbázis létrehozása a Visual Studio segítségével oktatóanyag olvassa el a [létrehozása és üzembe helyezése a Visual Studio segítségével Azure az erőforrás-csoportok](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)című témakört. 

## <a name="create-the-resource-manager-template"></a>Az erőforrás-kezelő-sablon létrehozása

A sablon egy JSON-fájlt, amely definiálja az összes telepíti a erőforrás. Lehetővé teszi-paraméterek a telepítés során megadott egyéb értékek és a kifejezések és a telepítést a kimeneti értékeket a helyes összeállítás változók. 

Kezdjük a legegyszerűbb sablon:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Ez a fájl mentése **azuredeploy.json** (figyelje meg, hogy a sablon beállíthatja, hogy minden kívánt nevet, csak az adott informatikai kell lennie a json-fájl).

## <a name="create-a-storage-account"></a>Tárterület-fiók létrehozása
Az **erőforrások** szakaszok objektum, amely meghatározza a tárterület-fiók hozzáadása alább látható módon. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Előfordulhat, hogy kell tudni szeretné, hogy ezek a tulajdonságok és értékek forrását. A Tulajdonságok **típusa**, **neve**, **apiVersion**és **hely** elemei szabványos diagramtípusokat erőforrás elérhető. Megismerheti az [erőforrások](resource-group-authoring-templates.md#resources)a közös elemeket. a paraméter értéke, amely a sikeres a során telepítési és **helyét** a hely az erőforráscsoport által használt **név** értéke. Hogyan meghatározhatja, hogy **típusa** és **apiVersion** az alábbi szakaszok áttekintjük.

A **Tulajdonságok** szakasz összes a Tulajdonságok egyediek adott erőforrástípus tartalmazza. Az értékek pontosan megadhatja a szakasz tartalma egyezik a helyezése művelet a REST API hozhat létre az adott erőforrás típusa. Tárterület fiók létrehozásakor meg kell adnia egy **accountType**. Figyelje meg a [REST API-tároló fiók létrehozása](https://msdn.microsoft.com/library/azure/mt163564.aspx) , a többi művelet tulajdonságok szakaszában is tartalmaz az **accountType** tulajdonságot, és a megengedett értékeket dokumentált. Ebben a példában a fiók típusának beállítása **Standard_LRS**, de sikerült adjon meg más értékek megjelenítésére, illetve engedélyezheti a felhasználóknak, írja be a fiók paraméterként átadni.

Most vegyük ugrani a **Paraméterek** rész, és ellenőrizze, hogyan definiálása a tárterület-fiók nevére. További tudnivalók a [paramétereket](resource-group-authoring-templates.md#parameters)paramétereket e talál. 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Itt megadott paraméter karakterlánc típusú, amely tartalmazni fogja a tárterület-fiók nevére. Ez a paraméter értéke nyújtanak sablon a telepítés során.

## <a name="deploying-the-template"></a>A sablon üzembe helyezése
Van még egy teljes sablont tároló új fiók létrehozása. A visszahívási, mint a sablon **azuredeploy.json** fájlban mentette:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Módon igazán néhány üzembe egy sablont, amint az [erőforrás telepítési cikk](resource-group-template-deploy.md)látható. A sablon Azure PowerShell használatával üzembe helyezéséhez használja:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Vagy a sablon használatával Azure CLI üzembe helyezéséhez használata:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Most már áll a baba tulajdonosa tároló fiók!

A következő lépésekkel lesz, az erőforrások, ebben az oktatóanyagban kezdésének ismertetett architektúrája telepítéséhez szükséges. Ezek az erőforrások ugyanazt a sablont a dolgozunk a ad hozzá.

## <a name="availability-set"></a>Elérhetőség beállítása
A definíciója után a tárterület-fiókom adja hozzá a virtuális gépekhez egy elérhetőségéről beállítása. Ebben az esetben vannak nincs szükség esetén további tulajdonságokat így definíciója meglehetősen egyszerű. Lásd: a [REST API-elérhetősége készlet létrehozása a](https://msdn.microsoft.com/library/azure/mt163607.aspx) teljes körű tulajdonságai csoportban, abban az esetben, ha be szeretné állítani a frissítés tartomány darab és a hiba tartomány száma gombra.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Figyelje meg, hogy a **név** egy változó értékének értéke. Ez a sablon nevét a elérhetőségének beállítása néhány különböző helyeken van szükség. A sablon karbantarthatja definiáló egyszer az adott értéket, és azt több helyen könnyebben.

A megadott **típusú** érték az erőforrás-szolgáltató és az erőforrás típusa is tartalmaz. Elérhetőség készletek az erőforrás-szolgáltató **Microsoft.Compute** pedig az erőforrás típusa **availabilitySets**. A rendelkezésre álló erőforrás szolgáltatók listáját úgy is megnyithatja, hogy futtassa az alábbi PowerShell-parancsot:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Vagy Azure CLI használatakor futtatását is lehetővé teszi a következő parancsot:
```
    azure provider list
```
Mivel a ebben a témakörben hoz létre a tárhely partnerek, a virtuális gépeken futó és a virtuális hálózat, akkor működnek:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Egy adott szolgáltató jelennek meg a erőforrástípus, futtassa az alábbi PowerShell-parancsot:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Másik lehetőségként az Azure CLI, a következő parancsot a rendelkezésre álló típusok térjen JSON formátumban lesz, és mentse a fájlt.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Ekkor megjelennek a **availabilitySets** belül **Microsoft.Compute**típusok közül egyik. A típus teljes neve **Microsoft.Compute/availabilitySets**. Meghatározhatja, hogy az összes sablont, az erőforrások erőforrásnevet.

## <a name="public-ip"></a>Nyilvános IP
Adja meg a nyilvános IP-címet. Ismét tekintse meg a tulajdonságok beállítása a [Nyilvános IP-címek REST API -t](https://msdn.microsoft.com/library/azure/mt163590.aspx) .

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

A felosztás módszer van beállítva, **dinamikus** , de ezt az értéket kell, vagy fogadja el a paraméter értéke állítsa beállítása. A tartomány neve címke értékkel átadni a sablon felhasználóinak engedélyezte.

Most Vegyük szemügyre hogyan meghatározhatja, hogy a **apiVersion**. A megadott értékkel egyszerűen megegyezzen a a REST API-t az erőforrás létrehozásakor használni kívánt. Igen akkor tekintse át, hogy az erőforrás típusa REST API dokumentációját. Másik lehetőségként futtatását is lehetővé teszi az alábbi PowerShell-parancsot egy bizonyos típusú.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Amely adja eredményül az alábbi értékeket:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Az Azure CLI az API-verziók megtekintéséhez a korábban már megjelenített azonos **azure szolgáltató megjelenítése** parancs futtatásával.

Új sablon létrehozása esetén válassza ki a legújabb verzió API-val.

## <a name="virtual-network-and-subnet"></a>Virtuális hálózati és alhálózat
Hozzon létre egy alhálózat virtuális hálózat. A [REST API-virtuális hálózatok](https://msdn.microsoft.com/library/azure/mt163661.aspx) beállítása a Tulajdonságok megtekintése:

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Terheléselosztó
Most egy külső szemben lévő terheléselosztó hoz létre. Mivel ez terheléselosztó a nyilvános IP-címet használja, a nyilvános IP-cím szakaszában **dependsOn** deklarálása függőség. Ez azt jelenti, hogy a terheléselosztó fog nem üzemelnek, amíg a nyilvános IP-cím nem fejeződött üzembe helyezése. Meghatározása a függőség, nélkül hibát jelez, mivel az erőforrás-kezelő megpróbálja telepíteni az erőforrások párhuzamosan, és megpróbálja a nyilvános IP-címet, amely még nem létezik a terheléselosztó beállítása fog kapni. 

A tcp-vizsgálati a 80-as port Ez az erőforrás-definícióban az is kódmentes cím erőforráskészlethez tartozik, pár be a VMs RDP hálózati Címfordítást bejövő szabályok és betöltés terheléselosztó szabály hoz létre. A [REST API-terheléselosztó](https://msdn.microsoft.com/library/azure/mt163574.aspx) kivétel a Tulajdonságok parancsot.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Hálózati kapcsolat
Minden egyes virtuális egy 2 hálózati kapcsolatok hoz létre. Ahelyett, hogy a hálózati kapcsolat esetén ismétlődő elemeket felvenni, használja a [copyIndex() függvény](resource-group-create-multiple.md) Ismétlés (néven nicLoop) másolása le, és a megadott szám hálózati kapcsolatok létrehozása a `numberOfInstances` változók. A hálózati kapcsolat létrehozása a virtuális hálózat és a terheléselosztó függ. A virtuális hálózati létrehozását, és a betöltés terheléselosztó azonosító meghatározott alhálózat állítsa be a betöltés terheléselosztó cím készlet és a bejövő szabályok hálózati Címfordítást használ.
Nézze meg a [Hálózati kapcsolat esetén REST API -t](https://msdn.microsoft.com/library/azure/mt163668.aspx) a a Tulajdonságok parancsot.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtuális gépen
2 virtuális gépeken futó függvénnyel copyIndex(), mint a [hálózati kapcsolatok](#network-interface)létrehozása a létrehoznia.
A virtuális létrehozása függ, hogy a tárterület-fiókot, a hálózati kapcsolat és elérhetőség beállítása. A virtuális létrejön egy piactér kép megadottak a `storageProfile` tulajdonság - `imageReference` adja meg a kép a publisher, ajánlat, termékváltozat és verzióját használják. Végül a diagnosztikai profilok diagnosztika ahhoz, hogy a virtuális van beállítva. 

Az érintett tulajdonságok piactér kép megkereséséhez kövesse a [Linux virtuális gép kép kijelöléséhez](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) , vagy [Válassza ki a Windows virtuális gép képek](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) cikkek.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] A képek **3 külső**gyártók közzé, adjon meg egy másik tulajdonságot nevű kell `plan`. Példa: [ezzel a sablonnal](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) a quickstart útmutató gyűjteményből találhatók. 

A sablon az erőforrások definiáló végzett.

## <a name="parameters"></a>Paraméterek

A paraméterek csoportban adja meg értékeket lehet megadni, ha telepíti a sablont. Csak paraméterek beállítása értékek, amelyek úgy gondolja, hogy változtatható meg a telepítés során. Ha egy nem szerepel a telepítés során használt paraméter egy alapértelmezett értéket lehet nyújtani. A megengedett érték is adhatja meg, ahogy azt a **imageSKU** paraméter.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Változók

Változók szakaszban meghatározhatja, hogy a sablon egynél több helyen használják, vagy értékek, amelyek az egyéb kifejezéseket és a változók épülnek fel. Változók gyakran használják a sablon szintaxisa a következő egyszerűsítése érdekében.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

A sablon befejezése! Összehasonlíthatja a sablon a [quickstart útmutató gyűjtemény](https://github.com/Azure/azure-quickstart-templates) a [2 VMs a terheléselosztó és terheléselosztó szabályok sablon betöltése](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)a teljes sablon alapján. A sablon némileg eltérő lehet a különböző verziójú számok használatának alapján. 

A sablon a tárterület-fiók telepítésekor használt azonos parancsokkal újra telepítheti. Nem kell a tároló fiók törlése a újra üzembe helyezése, mivel az erőforrás-kezelő kihagyja, amely már létezik, és nem változtak újbóli létrehozása erőforrások előtt.

## <a name="next-steps"></a>Következő lépések

- [Azure erőforrás-kezelő sablon Visualizer (ARMViz)](http://armviz.io/#/) ARM sablonok ábrázolásához kiváló eszköz megegyezik túl nagy ahhoz, hogy csak a json-fájl olvasását megértéséhez válhat.
- Sablon felépítésének kapcsolatos további információért lásd: [Azure erőforrás-kezelő létrehozáshoz használható sablonok](resource-group-authoring-templates.md).
- Sablon telepítésével kapcsolatos további tudnivalókért lásd: a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy.md)
