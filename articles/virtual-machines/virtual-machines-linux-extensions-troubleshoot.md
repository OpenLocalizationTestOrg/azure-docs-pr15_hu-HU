<properties
   pageTitle="Linux virtuális bővítmény hibák elhárítása |} Microsoft Azure"
   description="További tudnivalók: Azure Linux virtuális bővítmény hibák elhárítása"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Azure Linux virtuális bővítmény hibák elhárítása

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Bővítmény állapotának megtekintése
Az Azure CLI az Azure erőforrás-kezelő sablonok futtatható. Miután végrehajtása a sablont, a bővítmény állapotának Azure erőforrás Explorer vagy a parancssor eszközök tekinthetők meg.

Lássunk egy példát:

Azure CLI:

      azure vm get-instance-view


A példa eredménye a következő:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extenson-failures"></a>Extenson hibák elhárítása:

### <a name="re-running-the-extension-on-the-vm"></a>A bővítmény ismételt futó a virtuális

Ha futtatja a parancsfájlok a virtuális a egyéni parancsfájl bővítményében, időnként hibát jelez, ahol virtuális megtörtént, de nem sikerült a parancsfájl fellépő esetleges. Az alábbi feltételek ezt a hibát helyreállítása az ajánlott módszereket, a bővítmény eltávolítása, és futtassa újra ismét a sablont.
Megjegyzés: A jövőben, ez a funkció javítani fogja eltávolítása a bővítmény eltávolítása van szükség.

#### <a name="remove-the-extension-from-azure-cli"></a>A bővítmény eltávolítása Azure CLI

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Ahol "publsher-neve" megfelelő adattípusra bővítmény "get-példány-nézet azure virtuális" eredményéből és a sablonból a bővítmény erőforrás neve

Miután a bővítmény eltávolították, a sablon ismét végre, hogy a parancsprogramokat futtassanak a virtuális lehet.
