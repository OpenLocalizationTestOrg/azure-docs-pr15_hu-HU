<properties
   pageTitle="Teljesen minősített tartománynév hozzon létre egy virtuális az Azure-portálon |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy teljesen minősített tartománynév, vagy az erőforrás-kezelő FQDN alapú virtuális gép az Azure-portálon."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal"></a>Teljesen minősített tartománynév létrehozása az Azure-portálon
Amikor egy virtuális gép (virtuális) hoz létre az [Azure portál](https://portal.azure.com) az erőforrás-kezelő telepítési modell, automatikusan létrejön egy nyilvános IP-erőforrás a virtuális gépen. A virtuális távoli eléréséhez használt IP-cím. Bár a portálon nem hoz létre egy [teljesen minősített tartománynév](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), vagy teljes Tartománynevet, alapértelmezés szerint, amikor létrejött a virtuális létrehozhat egyet. Ez a cikk lépéseinek hozhat létre a DNS-név vagy a teljes Tartománynevet.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Most csatlakozhat távolról a virtuális, például a távoli asztali Protocol (RDP) a DNS nevét használja.

## <a name="next-steps"></a>Következő lépések
Most, hogy a virtuális van nyilvános IP- és a DNS-neve, telepítheti a közös alkalmazás keretek és szolgáltatások, például az IIS, SQL vagy SharePoint.

Is további információ [az erőforrás-kezelővel](../azure-resource-manager/resource-group-overview.md) létrehozása az Azure telepítések tippeket.