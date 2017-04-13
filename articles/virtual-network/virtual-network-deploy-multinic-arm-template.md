<properties
   pageTitle="Erőforrás-kezelő sablon használatával több hálózati kártya VMs telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként több hálózati kártya VMs sablon használatával az erőforrás-kezelő terjesztése"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Sablon használatával több hálózati kártya VMs terjesztése

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Ezen a ponton időben nem lehet VMs a egy egyetlen hálózati kártya és a több VMs az azonos erőforráscsoport, mivel a hátsó vége kiszolgálók fog alkalmazhat egy erőforrás csoportot, és minden más összetevő egy másik biztonsági csoportban. Az alábbi lépésekkel *IaaSStory* nevű fő erőforráscsoport, illetve *IaaSStory-Kódmentes* vissza vége kiszolgálók erőforráscsoport használja.

## <a name="prerequisites"></a>Előfeltételek

A visszahívási vége kiszolgálókat alkalmazhat, mielőtt kell az összes szükséges erőforrások példánkban a fő erőforráscsoport üzembe. Ezek az erőforrások telepítéséhez kövesse az alábbi lépéseket.

1. Nyissa meg [a sablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kattintson a sablonként lap jobb oldalán a **szülő erőforráscsoport** **az Azure Deploy**.
3. Ha szükséges, módosítsa a paraméterértékeket, majd az Azure előzetes portál erőforráscsoport telepítéséhez kövesse a.

> [AZURE.IMPORTANT] Ellenőrizze, hogy a tárhely fióknevét egyediek. Ismétlődő tárolási fióknevét Azure-ban nem lehet.

## <a name="understand-the-deployment-template"></a>A telepítési sablon ismertetése

Mielőtt beállítaná a mellékelt dokumentáció sablont, győződjön meg arról, hogy megismeri a Mit jelent. Az alábbi lépésekkel adja meg a szóban forgó sablon áttekintése.

1. Nyissa meg [a sablon lapot](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kattintson a **azuredeploy.json** kattintva nyissa meg a sablonfájl.
3. Figyelje meg, az alább felsorolt *osType* paraméter. A paraméter használható az adatbázis-kiszolgáló virtuális képet kiválasztására szolgál, és több operációs rendszer kapcsolódó beállításokat.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Görgessen le változó a listában, és jelölje be a **dbVMSetting** változók, az alább felsorolt definícióját. A **dbVMSettings** változó tömb elemének egyikét kapja. Ha ismeri a szoftver fejlesztési terminológiájában, a **dbVMSettings** változó egy hashtable vagy egy dictionay tekinthet meg.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Tegyük fel, hogy úgy dönt, hogy a háttér SQL futó Windows VMs üzembe. Kattintson a **osType** értékét lenne a *Windows*, és a **dbVMSetting** változó tartalmaz, az alább felsorolt az elemet, amely tartalmazza a **dbVMSettings** változóban első értékét.

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Figyelje meg a **vmSize** *Standard_DS3*értéket tartalmazza. Csak bizonyos virtuális méretű lehetővé teszi a több használatát. Ellenőrizheti, hogy melyik virtuális méretű engedélyezve van, látogasson el a [több hálózati kártya – áttekintés](virtual-networks-multiple-nics.md)hálózati több.
7. Görgessen le a **források** , és figyelje meg az első elemet. Azt ismerteti, hogy egy tárterület-fiókkal. Ehhez a fiókhoz tároló adatbázisonként virtuális által használt adatok lemezt megőrzéséhez lesz. Ebben az esetben a virtuális adatbázisonként-OS lemezre normál tárolója tárolja, és két, SSD (prémium) tárolóban lévő adatok lemez tartalmaz.

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Görgessen le a következő erőforrást, az alább felsorolt. Ez az erőforrás felel meg az adatbázis az access minden adatbázisban virtuális használható hálózati. Figyelje meg, az erőforrás a **Másolás** függvényt. A sablon lehetővé teszi, hogy minél több VMs van szüksége, az **dbCount** paraméter alapján telepítése. Ezért létrehozásához szükséges adatbázis eléréséhez, egy minden virtuális NIC ugyanannyi.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Görgessen le a következő erőforrást, az alább felsorolt. Ez az erőforrás a hálózati kártya minden adatbázisban virtuális kezeléséhez használt jelöli. Még egyszer már rá ezek NICS-adatbázisonként virtuális. Figyelje meg a **networkSecurityGroup** elem csatolása egy NSG, amely lehetővé teszi a RDP/SSH csak a hálózati való hozzáférést.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Görgessen le a következő erőforrást, az alább felsorolt. Ez az erőforrás-elérhetőségének beállítása az összes adatbázis VMs kell megosztania jelöli. Úgy, hogy Ön garantálja, hogy mindig lesz egy virtuális futtatása közben karbantartási megadása.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Görgessen le a következő erőforrást. Ez az erőforrás jelöli az adatbázis VMs naplójában az első néhány sor, az alább felsorolt Figyelje meg, a **Másolás** függvényének újra felhasználhat, biztosítva, hogy több VMs a **dbCount** paraméter alapján jönnek létre. Figyelje meg a **dependsOn** gyűjtemény is. Két kártyát van szükség, mielőtt a virtuális telepíti, az elérhetőség beállítása és a tárterület-fiókot létrehozni jeleníti meg.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Görgessen lefelé a virtuális erőforrás **networkProfile** elem, az alább felsorolt. Figyelje meg, hogy nincsenek-e minden virtuális hivatkozás küldése két kártyát. Amikor egy virtuális több hoz létre, be kell a **elsődleges** tulajdonság egy, a NICS *Igaz*és *hamis*a többi.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM sablon üzembe telepítéséhez kattintson használatával

> [AZURE.IMPORTANT] Győződjön meg arról, hogy a [előtti követelmények](#Pre-requisites) kövesse az alábbi útmutatást követve előtt.

Az űrlapsablonok érhető el a nyilvános adattárban az alapértelmezett értéket a fent leírt forgatókönyv létrehozásához használt tartalmazó paraméter fájlt használ. Ezzel a sablonnal üzembe, kattintson [erre a hivatkozásra](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)kattintás használatával üzembe helyezéséhez jobbra lévő **Kódmentes erőforrás csoport (lásd: a dokumentáció)** kattintson **Azure telepítéséhez**, cserélje le a alapértelmezett paraméterértékeket, ha szükséges, és kövesse az utasításokat a portálon.

Az alábbi ábrán a telepítés után az új erőforráscsoport tartalmát jeleníti meg.

![Vissza a befejezési erőforráscsoport](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>A sablon üzembe PowerShell használatával

A PowerShell használatával letöltött sablont telepítéséhez kövesse az alábbi lépéseket.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Futtassa a **`New-AzureRmResourceGroup`** parancsmagot a sablonnal erőforrás csoport létrehozásához.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Várt kimenet:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>A sablon telepítése az Azure CLI használatával

A sablon az Azure CLI telepítéséhez kövesse az alábbi lépéseket.

1. Ha még sosem használt Azure CLI, lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md) , és kövesse az utasításokat a pont, ahol be az Azure-fiók és az előfizetés felfelé.
2. Futtassa a **`azure config mode`** erőforrás-kezelő módban, az alább látható módon válthat parancsot.

        azure config mode arm

    Az alábbiakban a várt eredménye a fenti parancs:

        info:    New mode is arm

3. Nyissa meg a [Paraméterek fájlt](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), jelölje ki a tartalmát, és mentse a fájlt a számítógépen. Ebben a példában *parameters.json*a paraméterek fájlt menti azt.

4. Futtassa a **`azure group deployment create`** az új VNet telepítse a sablon és a paraméter segítségével parancsmag fájlok letöltött és a fenti módosított. A kimenet után megjelenik a paramétereket ismerteti.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Várt kimenet:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
