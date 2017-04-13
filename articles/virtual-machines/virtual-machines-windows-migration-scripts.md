<properties
    pageTitle="Erőforrás-kezelő Azure áttelepítési Azure-kezelő közösségi eszközök"
    description="Ez a cikk az eszközöket, hogy a kapott segítséget nyújt a Közösség által áttelepítés IaaS erőforrások az Azure szolgáltatás az erőforrás-kezelő Azure jegyzettömbhöz katalógusok."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Erőforrás-kezelő Azure áttelepítési Azure-kezelő közösségi eszközök

Ez a cikk az eszközöket, hogy a kapott segítséget nyújt a Közösség által áttelepítés IaaS erőforrások az Azure szolgáltatás az erőforrás-kezelő Azure jegyzettömbhöz katalógusok.

>[AZURE.NOTE]Ezek az eszközök hivatalos által nem támogatott Microsoft Support. Ezért nyitva a Github kifejezéskészletébe, és fogadja el a PRs javítások vagy további forgatókönyvek örömmel. Jelentés a problémát, a funkcióval Github problémák.
>
> Ezeket az eszközöket az áttelepítés okoz a klasszikus virtuális gép legrövidebb leállás. Ha támogatott platform áttelepítési keres, látogasson el a következőket: 
>
>- [Támogatott platform erőforrások áttelepítése a IaaS klasszikus az erőforrás-kezelő Azure jegyzettömbhöz](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Műszaki mély merülési platformon támogatott áttelepítést a klasszikus az Azure erőforrás-kezelő](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [IaaS erőforrások áttelepítése klasszikus az Azure erőforrás-kezelő Azure PowerShell használatával](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

Ez a egy PowerShell-parancsprogramot modult a **egyetlen** virtuális gép (virtuális) az Azure Szolgáltatáskezelés oszlopból Azure erőforrás-kezelő Papírhalom áttelepítésének. 

[Az eszköz dokumentáció mutató hivatkozás](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz Azure szolgáltatás felügyeleti IaaS erőforrások teljes körű áttelepítése Azure erőforrás-kezelő IaaS erőforrások között új lehetőség. Az áttelepítés akkor fordulhat elő, az azonos előfizetés belül, illetve különböző előfizetések és előfizetés-típusok között (ex: CSP előfizetések).

[Az eszköz dokumentáció mutató hivatkozás](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)