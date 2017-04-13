<properties
   pageTitle="Sablonok Linux virtuális kiterjesztésű szerzői |} Microsoft Azure"
   description="További tudnivalók: Azure erőforrás-kezelő sablonok kiterjesztésű Linux VMs létrehozására"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Erőforrás-kezelő Azure sablonok Linux virtuális kiterjesztésű létrehozása

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Azure CLI futtassa a következő parancs:

      Azure VM extension list

Ez a parancs értéket ad vissza, a közzétevő nevét, a bővítmény nevét és a következőképpen verzióját:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Ezek a tulajdonságok megfeleltetése a "publisher", "típus" és "typeHandlerVersion" rendre a fenti sablon kódtöredékének a.

>[AZURE.NOTE]Még mindig a legfrissebb funkciók a legújabb bővítmény használata ajánlott.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>A bővítmény konfigurációs paraméterekkel a séma azonosítása

Egy bővítmény sablon létrehozásával kapcsolatos lépés azonosítása konfigurációs paramétereket kezeléséről a formátumát. Minden egyes bővítmény saját paraméterek támogat.

Minta konfigurációk Linux Extensions ismételt megjelenítéséhez kattintson a [Linux eExtensions minták](virtual-machines-linux-extensions-configuration-samples.md)olvassa át.

Olvassa el a következő virtuális kiterjesztésű teljesen kész sablonból.

[Kattintson egy Linux virtuális egyéni parancsfájl-bővítmény](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Után létrehozása a sablon, telepítheti az Azure CLI használatával.
