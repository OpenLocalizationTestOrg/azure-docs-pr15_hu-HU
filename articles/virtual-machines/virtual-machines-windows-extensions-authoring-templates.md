<properties
   pageTitle="Sablonok létrehozása a Windows virtuális kiterjesztésű |} Microsoft Azure"
   description="Tudnivalók a Windows VMs Azure erőforrás-kezelő sablonok kiterjesztésű létrehozására"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>A Windows virtuális kiterjesztésű Azure erőforrás-kezelő sablonok létrehozása

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Azure PowerShell futtassa a következő Azure PowerShell-parancsmagot:

      Get-AzureVMAvailableExtension


Ezzel a parancsmaggal adja eredményül a közzétevő nevét, a bővítmény nevét és a verzióját az alábbi képlettel történik:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Ezek a tulajdonságok megfeleltetése a "publisher", "típus" és "typeHandlerVersion" rendre a fenti sablon kódtöredékének a.

>[AZURE.NOTE]Még mindig a legfrissebb funkciók a legújabb bővítmény használata ajánlott.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>A bővítmény konfigurációs paraméterekkel a séma azonosítása

Egy bővítmény sablon létrehozásával kapcsolatos lépés azonosítása konfigurációs paramétereket kezeléséről a formátumát. Minden egyes bővítmény saját paraméterek támogat.

Tekintse meg a Windows-bővítmények minta konfigurációk, olvassa el [a Windows-bővítmények minták](virtual-machines-windows-extensions-configuration-samples.md).


Olvassa el a következő virtuális kiterjesztésű teljesen kész sablonból.

[Virtuális Windows rendszerű egyéni parancsfájl-bővítmény](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Miután létrehozása a sablon, telepítheti a Azure PowerShell használatával.
