<properties
    pageTitle="Erőforrás-kezelő Azure sablonokkal Azure egymást fedő (bérlői fejlesztők) |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure egymást fedő Azure erőforrás-kezelő sablonok segítségével üzembe helyezéséhez és az alkalmazás egyetlen, összehangolt művelet erőforrásokat kiépítése."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure egymást fedő erőforrás-kezelő Azure-sablonok használata

Azure erőforrás-kezelő sablonok üzembe helyezéséhez és az alkalmazás egyetlen, összehangolt művelet erőforrásokat kiépítése. Megadhatja az erőforrások az alkalmazást, és hogyan fog rendszerbe.  Ha módosítani szeretné az erőforrások erőforráscsoport sablonok telepítsen is újra.

Ezek a sablonok a Microsoft Azure Papírhalom portál, PowerShell, a parancssor és a Visual Studio elvégezhető.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Az alábbi sablonok a [GitHub](http://aka.ms/azurestackgithub)érhetők el:

## <a name="deploy-sharepoint-non-high-availability"></a>(Nem nyílt elérhetőség) SharePoint terjesztése

A PowerShell DSC bővítmény segítségével létrehozása a SharePoint 2013-ban, amely tartalmazza az alábbiakat:

-   Virtuális hálózaton

-   Három tárterület-fiók

-   Két külső terheléselosztókkal

-   Egy egyetlen tartomány új akinek tartományvezérlőnek konfigurált egy virtuális

-   Az SQL Server 2014-es önálló kiszolgáló konfigurált egy virtuális

-   A SharePoint 2013 egyetlen számítógépre farm konfigurált egy virtuális

## <a name="deploy-ad-non-high-availability"></a>(Nem nyílt elérhetőség) AD terjesztése

A PowerShell DSC bővítmény használatával hozzon létre egy tartományt AD-vezérlő, amely tartalmazza a következő kiszolgáló:

-   Virtuális hálózaton

-   Több tárhely fiókkal

-   Egy külső terheléselosztó

-   Egy egyetlen tartomány új akinek tartományvezérlőnek konfigurált egy virtuális

## <a name="deploy-adsql-non-high-availability"></a>(Nem nyílt elérhetőség) AD/SQL terjesztése

A PowerShell DSC bővítmény használatával hozzon létre egy SQL Server 2014-es önálló kiszolgálót, amely tartalmazza az alábbiakat:

-   Virtuális hálózaton

-   Két tárterület-fiókok

-   Egy külső terheléselosztó

-   Egy egyetlen tartomány új akinek tartományvezérlőnek konfigurált egy virtuális

-   Az SQL Server 2014-es önálló kiszolgáló konfigurált egy virtuális

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

A PowerShell DSC bővítmény használatával állítsa be egy meglévő virtuális gép helyi konfigurációs Manager (LKT), és regisztrálja Azure automatizálási fiók DSC lekérés-kiszolgálóhoz.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Virtuális gép létrehozása egy felhasználó képből

Hozzon létre egy virtuális gép egy egyéni felhasználói kép. Ezzel a sablonnal is üzembe helyezése a virtuális-hálózaton (DNS), nyilvános IP-cím és a hálózati kapcsolat.

## <a name="simple-vm"></a>Egyszerű virtuális

Telepítse egy egyszerű Windows virtuális, amely egy virtuális-hálózaton (DNS), a nyilvános IP-cím és a hálózati kártya tartalmazza.

## <a name="cancel-a-running-template-deployment"></a>A futó sablon telepítésének megszüntetése

A futó sablon telepítésének visszavonásához használja a `Stop-AzureRmResourceGroupDeployment` PowerShell-parancsmag.


## <a name="next-steps"></a>Következő lépések

[Sablonok Portal telepítése](azure-stack-deploy-template-portal.md)

[Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md)

