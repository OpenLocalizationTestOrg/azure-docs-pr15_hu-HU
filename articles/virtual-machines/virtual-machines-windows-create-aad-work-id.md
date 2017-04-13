<properties
   pageTitle="Hozzon létre egy munkahelyi vagy iskolai identitás a AAD |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre egy munkahelyi vagy iskolai identitást az Azure Active Directory használata a Windows virtuális gépeken futó."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="creating-a-work-or-school-identity-in-azure-active-directory-to-use-with-windows-vms"></a>A munkahelyi vagy iskolai azonosító létrehozása az Azure Active Directory Windows VMs használata

Ha létrehozott egy személyes Azure-fiók vagy személyes MSDN-előfizetést, és kihasználhatja az MSDN Azure credits – az Azure-fiók létrehozása a *Microsoft-fiók* identitás használatával hozza létre. Számos kitűnő szolgáltatást az Azure – [erőforrás csoport sablonok](../azure-resource-manager/resource-group-overview.md) egy példa--a munkahelyi vagy iskolai fiókjával (Azure Active Directory által felügyelt identitás) megkövetelése a munkát. Követheti az alábbi útmutatást hozzon létre új munkahelyi vagy iskolai fiókba, mivel Szerencsére, a személyes Azure fiókkal kapcsolatos gyakorlati formázási, hogy hozzon létre egy új munkahelyi vagy iskolai fiókját, amelyek segítségével használhatja azt igénylő Azure funkcióival használt alapértelmezett Azure Active Directory tartománynevet származik.

Jó helyen jár, a legutóbbi változások lehetővé teszik az Azure-fiók használatával bármilyen típusú, az előfizetés kezeléséhez a `azure login` leírt interaktív bejelentkezési módszer [Itt](../xplat-cli-connect.md). Ezt az eljárást használhatja, vagy hajtsa végre az kövesse az utasításokat. [Hozzon létre egy munkahelyi vagy iskolai identitás az Azure Active Directory Linux VMs használatára](virtual-machines-linux-create-aad-work-id.md)is van lehetőség.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-create-aad-work-id](../../includes/virtual-machines-common-create-aad-work-id.md)]
