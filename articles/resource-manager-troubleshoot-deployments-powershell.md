<properties
   pageTitle="A PowerShell telepítésének műveletek megjelenítése |} Microsoft Azure"
   description="Az Azure PowerShell használata az erőforrás-kezelő telepítési problémák feltárása ismerteti."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Azure PowerShell telepítésének műveletek megtekintése

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [A PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-VAL](resource-manager-troubleshoot-deployments-rest.md)

A műveletek telepítés az Azure Powershellen keresztül is megtekintheti. Előfordulhat, hogy tervezheti meg megtekintése a műveletek, ha kapott hiba a telepítés során, ez a cikk koncentrál műveleteket, amelyek nem megtekintése. PowerShell parancsmagok, amelyek lehetővé teszik a hibák keresése és határozza meg az esetleges javításokat tartalmaz.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Hibák elkerülhető, ha a sablon és a telepítés előtt infrastruktúra érvényesítése. További kérelem és a válasz információkat, amelyek akkor lehet hasznos, később az alábbiakkal a telepítés során is jelentkezzen. Ellenőrzése és a kérelem és válasz naplóadatokat kapcsolatos további tudnivalókért olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy.md)című témakört.

## <a name="use-deployment-operations-to-troubleshoot"></a>Üzembe helyezési műveleteket használatával kapcsolatos hibák elhárítása

1. A telepítés általános állapotuk, használja a **Get-AzureRmResourceGroupDeployment** parancsot. Az eredmények csak a sikertelen telepítési is szűrheti.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Amely a sikertelen telepítések adja eredményül a következő formátumban:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Minden egyes telepítési általában tevődnek össze több műveletet hajtson végre, minden egyes lépés a telepítés során, amely műveletet. Megtudhatja, hogy mi történt a telepítéssel, általában szüksége szeretne tudni az üzembe helyezési műveleteket. A **Get-AzureRmResourceGroupDeploymentOperation**végzett műveletek állapotát tekintheti meg.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Amely adja eredményül a következő formátumban mindegyik több műveletek:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. További részleteket szeretne megtudni a sikertelen műveleteket, beolvashatja a **hibás** állapotba műveletek tulajdonságait.

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Amely adja eredményül a sikertelen műveleteket, és minden egyes egy, a következő formátumban:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Megjegyzés: a művelet a nyomon követés azonosítója. Meg kell használni, amely a következő lépésben kiemelése egy adott művelet.

4. Úgy juthat az adott művelet sikertelen állapotüzenete, használja az alábbi parancsot:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Amely adja eredményül:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Használja a naplókat kapcsolatos hibák elhárítása

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Telepítés hibák megtekintéséhez kövesse az alábbi lépéseket:

1. Naplóbejegyzések beolvasásához, futtassa a **Get-AzureRmLog** parancsot. A **ResourceGroup** és **állapot** paraméter segítségével vissza csak egyetlen erőforráscsoport sikertelen eseményeket. Ha nem adja meg a kezdési és befejezési időpontot, a visszaadott bejegyzéseinek az utolsó órát.
Ha például beolvasásához az elmúlt órát, futtassa a sikertelen műveleteket:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Megadhatja, hogy egy adott időszak. A következő példa azt fogja keresse meg a sikertelen műveletek az utolsó napjára. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Vagy beállíthatja, hogy egy pontos kezdési és befejezési idő sikertelen műveleteket:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Ha ez a parancs a túl sok bejegyzéseket és tulajdonságok ad vissza, összpontosíthat a naplózási erőfeszítéseket által a **Properties** tulajdonság beolvasása közben. Azt is is tartalmazni fogja a **DetailedOutput** paramétert a hibaüzenetek megjelenítéséhez.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Amely a naplóbejegyzések tulajdonságainak adja eredményül a következő formátumban:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Vegyük alapján a következő eredményeket, kiemelése a második elemet. Tovább finomíthatja az eredményt megjeleníti az adott bejegyzés állapotüzenete.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Amely adja eredményül:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Következő lépések

- Az adott telepítési hibák megoldása című témakörben kaphat segítséget [Azure Azure erőforrás-kezelővel erőforrások telepítésekor a gyakori hibák megoldásához](resource-manager-common-deployment-errors.md).
- Műveletek más típusú figyelheti a naplókat használatával kapcsolatos további tudnivalókért lásd: [az erőforrás-kezelő műveletek naplózási](resource-group-audit.md).
- A telepítési azt végrehajtása előtt ellenőrizendő olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy.md)című témakört.

