<properties
   pageTitle="Virtuális gép skála értékkészletet állapot konfiguráció használata kívánt |} Microsoft Azure"
   description="Az Azure DSC kiterjesztéssel használatáról virtuális gép skála beállítása"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Az Azure DSC kiterjesztéssel használatáról virtuális gép skála beállítása

[Virtuális gép skála beállítása (VMSS)](virtual-machine-scale-sets-overview.md) kínál az [Azure kívánt állam konfigurációs (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) bővítmény kezelő. VMSS üzembe helyezéséhez és kezeléséhez nagyszámú virtuális gépeken futó lehetőséget nyújt, és elastically méretezheti és kicsinyítés válaszként való betöltéséhez. Állítsa be a VMs azokat az online állapotba, így a gyártási szoftver futnak DSC szolgál.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Virtuális és VMSS bevezetéshez közötti különbségek

Az alapul szolgáló sablon szerkezet VMSS némileg eltér egy egyetlen virtuális. Egy egyetlen virtuális kifejezetten, a "virtualMachines" csomópontot a bővítmények üzembe helyezése. Van egy bejegyzés "bővítmények" típusú hol DSC bekerül a sablon

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Egy VMSS csomópont "Tulajdonságok" szakasz a "VirtualMachineProfile", "extensionProfile" attribútumot tartalmazó tartalmaz. DSC megjelenik a "bővítmények"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>VMSS viselkedése

A vezérlő viselkedése a egyetlen virtuális azonos VMSS működése. Egy új virtuális létrehozásakor automatikusan kiépítve DSC kiterjesztésű. A WMF újabb verzióját szükség szerint a bővítmény a virtuális újraindítja, mielőtt online lesz. Online állapotban, ha letölti a DSC konfigurációs .zip és kiépítése, kattintson a virtuális. További részletek találhatók [az Azure DSC bővítmény](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md)áttekintésben.

## <a name="next-steps"></a>Következő lépések ##
Tekintse meg az [erőforrás-kezelő Azure-sablon DSC bővítményhez](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Megtudhatja, hogyan [DSC bővítmény biztonságosan kezeli a hitelesítő adatait](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

További tájékoztatást a az Azure DSC bővítmény kezelő című témakörben [az Azure kívánt állam konfigurációs bővítmény kezelője](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

További információt a PowerShell DSC, [Keresse fel a PowerShell dokumentáció központját](https://msdn.microsoft.com/powershell/dsc/overview). 


