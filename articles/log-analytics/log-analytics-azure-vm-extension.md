<properties
    pageTitle="Azure virtuális gépeken futó csatlakoztatása napló Analytics |} Microsoft Azure"
    description="A Windows és Linux virtuális gépeken futó operációs rendszert futtató Azure-ban az ajánlott naplókban összegyűjtött és mérőszámok módja a napló Analytics Azure virtuális bővítmény telepítése. Az Azure portálon vagy a PowerShell használatával Azure VMs alakzatot a napló Analytics virtuális gép bővítmény telepítése."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Log Analytics Azure virtuális gépeken futó csatlakoztatása

Windows és Linux számítógépek a javasolt naplók és mérőszámok gyűjtéséhez található a napló Analytics agent telepítésével.

A napló Analytics ügynököt Azure virtuális gépeken legegyszerűbben a napló Analytics virtuális bővítmény keresztül.  A bővítmény használatával egyszerűbbé teszi a telepítési folyamatot, és automatikusan konfigurálja az adatokat küld a megadott feltételeknek napló Analytics munkaterület ügynök. A agent is automatikusan frissíti, biztosítva, hogy van-e a legújabb funkciókat és javításokat.

A Windows virtuális gépeken futó lehetősége van engedélyezni a *Microsoft figyelése Agent* virtuális gép bővítmény.
Linux virtuális gépekhez lehetősége van engedélyezni a *MOBILE ügynök Linux* virtuális gép bővítmény.

További tudnivalók az [Azure virtuális gép extensions](../virtual-machines/virtual-machines-windows-extensions-features.md) és az [Linux ügynök] (... / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Ügynök alapú webhelycsoport naplózási adatokat használ, ha meg kell adnia a [napló Analytics adatforrások](log-analytics-data-sources.md) határozza meg, hogy naplók mértékek, amely a gyűjtendő.

>[AZURE.IMPORTANT] Ha napló Analytics napló adatait indexelni [Azure diagnosztika](log-analytics-azure-storage.md)segítségével konfigurálása, és beállíthatja az ügynök az azonos naplógyűjtés, majd a naplók begyűjtési kétszer. Előfizetést terhelő mindkét adatforráshoz. Ha a agent telepítette, majd meg kell adatgyűjtés napló egyedül a ügynök – a napló Analytics naplóadatok gyűjt a Azure diagnosztika nem konfigurálása.

Háromféleképpen egyszerűen ahhoz, hogy a napló Analytics virtuális gép bővítmény:

+ Az Azure portál használatával
+ Azure PowerShell használatával
+ Az erőforrás-kezelő Azure-sablon segítségével

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>Az Azure-portálon a virtuális bővítmény engedélyezése

A ügynököt a napló elemzéséhez, és csatlakozás a Azure virtuális gépen futó azt az [Azure portál](https://portal.azure.com)használatával.

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>A napló Analytics agent telepítése, és csatlakozás a virtuális gép napló Analytics-munkaterület

1.  Jelentkezzen be az [Azure-portálon](http://portal.azure.com).
2.  Válassza a **Tallózás gombra** a portálon bal oldalán, és nyissa meg a **Napló Analytics (MOBILE)** , és jelölje ki.
3.  A napló Analytics-munkaterületek listájában jelölje ki a kívánt a Azure virtuális használata.  
    ![MOBILE-munkaterületek](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  A **napló analytics-kezelés**jelölje ki a **virtuális gépeken futó**.  
    ![Virtuális gépeken futó](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  **Virtuális gépeken futó**listájában jelölje ki a virtuális gépen, amelyen a ügynököt a kívánt. A virtuális a **MOBILE kapcsolat állapotát** jelzi, hogy **Nincs kapcsolat**.  
    ![Virtuális kapcsolat nélkül](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  A virtuális gép részleteit kattintson a **Csatlakozás**elemre. A agent van telepítve, és automatikusan a napló Analytics-munkaterület konfigurálva. Ez a folyamat vesz igénybe néhány percet, amely idő alatt a MOBILE kapcsolati állapot: *Csatlakozás...*  
    ![Kapcsolódás virtuális](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Miután telepítése és csatlakozás a agent, **MOBILE kapcsolati** állapotának megjelenítése **a munkaterület**frissülnek.  
    ![Csatlakoztatott](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>A PowerShell használatá virtuális bővítmény engedélyezése

Vannak más parancsok Azure klasszikus virtuális gépeken futó és erőforrás-kezelő virtuális gépeken futó. Az alábbiakban példákat is tartalmaz klasszikus és erőforrás-kezelő virtuális gépeken futó is.

Klasszikus virtuális gépeken futó használja az alábbi PowerShell példa:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Erőforrás-kezelő virtuális gépeken futó használja az alábbi PowerShell példa:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Ha a virtuális gép PowerShell használatával állítja be, meg kell adnia a **Munkaterület Azonosítóját** és az **Elsődleges kulcs**. Az azonosító és a kulcs megtalálhatja a **Beállítások** lapon, a MOBILE portál vagy az előző példában látható módon a PowerShell használatával.

![Munkaterület-Azonosítóját és az elsődleges kulcs](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>A sablon használatával virtuális bővítmény telepítése

Erőforrás-kezelő Azure használatával hozhat létre egyszerű sablon (JSON formátumban), amely meghatározza a telepítési és az alkalmazás beállításai. Ezzel a sablonnal erőforrás-kezelő sablonként ismert, és üzembe meghatározásához deklaráció útján biztosít. Egy sablon segítségével többször telepítheti az alkalmazást az app életciklus során és megbízhatóság, az erőforrások vannak éppen felületek konzisztens állapotban van.

Az erőforrás-kezelő sablon részeként a napló Analytics agent felvételével biztosíthatja, hogy virtuális gépeken egy előre konfigurált jelentést a napló Analytics-munkaterületre.

Erőforrás-kezelő sablonok kapcsolatos további tudnivalókért olvassa el a [szerzői Azure erőforrás-kezelő sablonok](../resource-group-authoring-templates.md)című témakört.

Következő képen egy erőforrás-kezelő sablonhoz, amellyel egy virtuális számítógépre telepítve van a Microsoft figyelése Agent kiterjesztésű Windows rendszerű telepítésével. Ez a sablon egy tipikus virtuális gép sablon, a következő kiegészítéseket:

+ workspaceId és workspaceName paraméterei
+ Erőforrás-bővítmény szakasz Microsoft.EnterpriseCloud.Monitoring
+ Kimeneti értékeket lépések elvégzésével keresheti meg a workspaceId és workspaceSharedKey


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Sablon használatával az alábbi PowerShell-parancsot telepítheti:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Hibaelhárítás a Windows virtuális gépeken futó

Ha a *Microsoft figyelése Agent* virtuális ügynök bővítmény nem telepítése vagy jelentése az alábbi lépéseket a probléma elhárításához végre tud hajtani.

1. Jelölőnégyzetet, ha telepítve van a az Azure virtuális ügynök és megfelelően az lépésekkel [KB 2965986](https://support.microsoft.com/kb/2965986#mt1)a munkát.
  + A virtuális ügynök naplófájl is áttekintheti`C:\WindowsAzure\logs\WaAppAgent.log`
  + A napló nem létezik, ha a virtuális agent nincs telepítve.
    - [Telepítse az Azure virtuális ügynök klasszikus VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Erősítse meg a Microsoft figyelése Agent bővítmény szívveréséhez tevékenység fut, az alábbi lépésekkel:
  + Jelentkezzen be a virtuális gépen
  + Nyissa meg a Feladatütemező, és keresse meg a `update_azureoperationalinsight_agent_heartbeat` tevékenység
  + Erősítse meg a tevékenység engedélyezve van, és egy percenként fut
  + A szívveréséhez naplófájlban beadása`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Tekintse át a Microsoft Agent figyelése virtuális bővítmény naplófájlok`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. A virtuális gép futtatását is lehetővé teszi a PowerShell-parancsfájlokat biztosítása
4. Győződjön meg róla, nem módosította az engedélyeket a C:\Windows\temp
5. Az alábbi beírásával, emelt PowerShell ablakban a virtuális gépen a Microsoft figyelése Agent állapotának megtekintése`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Tekintse át a Microsoft figyelése ügynökök beállítása a naplófájlok`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

További tudnivalókért tekintse át a [Windows bővítmények hibaelhárítási](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Hibaelhárítási Linux virtuális gépeken futó

Ha a *MOBILE Agent Linux* virtuális ügynök bővítmény nem telepítése vagy jelentése az alábbi lépéseket a probléma elhárításához végre tud hajtani.

1. Ha a bővítmény állapota *Ismeretlen* jelölőnégyzetet, ha telepítve van a az Azure virtuális ügynök és megfelelően megtekintésével által a virtuális ügynök naplófájl végezhető műveletek`/var/log/waagent.log`
  + A napló nem létezik, ha a virtuális agent nincs telepítve.
  - [Telepítse az Azure virtuális ügynök Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Más sérült állapotok, tekintse át a MOBILE Agent Linux virtuális bővítmény fájlok jelentkezik be a `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` és`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Ha a bővítmény állapota megfelelő, de nem feltölteni adatok tekintse át a MOBILE Agent Linux naplófájlok a`/var/opt/microsoft/omsagent/log/omsagent.log`

További tudnivalókért tekintse át a [hibaelhárítási Linux bővítmények](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Következő lépések

+ Adja meg a naplók és mérőszámok gyűjthetők össze az [adatforrások napló Analytics](log-analytics-data-sources.md) konfigurálása.
+ Adatok gyűjtése a virtuális gépek [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md).
+ Egyéb információforrások, amelyek az Azure futtatja a [adatgyűjtés Azure diagnosztika segítségével](log-analytics-azure-storage.md) .

Olyan számítógépen, amelyek nem Azure-ban a következő cikkekben ismertetett módon telepítheti a napló Analytics-agent:

+ [Csatlakozás Windows rendszerű számítógépeken napló Analytics](log-analytics-windows-agents.md)
+ [Log Analytics Linux számítógépek csatlakoztatása](log-analytics-linux-agents.md)
