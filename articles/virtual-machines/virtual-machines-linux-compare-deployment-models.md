<properties
   pageTitle="Számítási, hálózati és tárolását szolgáltatók |} Microsoft Azure"
   description="A számítási, a hálózati és a tárhely erőforrás szolgáltatók (KSZT NRP és összegző) áttekintése Linux alkalmazások Azure erőforrás-kezelő telepítési modell"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/19/2015"
   ms.author="tomfitz"/>

# <a name="azure-compute-network-and-storage-providers-for-linux-applications-under-azure-resource-manager-deployment-model"></a>Az erőforrás-kezelő Azure telepítési modell Linux alkalmazások Azure számítási, hálózati és tárolását szolgáltatók

Az erőforrás-kezelő Azure telepítési modell számítási, hálózati és tárolási lehetőségek a elküldésének alapvetően egyszerűsíti a telepítési és IaaS futó összetett alkalmazások kezelése. Sok alkalmazás erőforrások, például egy virtuális hálózaton, tárterület-fiók, virtuális gép és a hálózati kapcsolat kombinációját igényel. Az erőforrás-kezelő Azure telepítési modell kínál az azt jelenti, hogy telepíthető, és ezek az erőforrások együttes kezelése egyetlen alkalmazásként JSON sablon létrehozása.

[AZURE.INCLUDE [virtual-machines-common-compare-deployment-models](../../includes/virtual-machines-common-compare-deployment-models.md)]
