<properties
   pageTitle="Telepítési műveletek az Azure CLI megtekintése |} Microsoft Azure"
   description="Megtudhatja, hogy miként használhatja az Azure CLI feltárása az erőforrás-kezelő telepítési problémák."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Az Azure CLI telepítési műveletek megtekintése

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [A PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-VAL](resource-manager-troubleshoot-deployments-rest.md)

Ha hiba erőforrások Azure telepítésekor arról, érdemes többet szeretne tudni az üzembe helyezési műveleteket, amelyeket végrehajtva. Azure CLI biztosít, amelyek lehetővé teszik, hogy a hibák keresése és határozza meg az esetleges javítások parancsok.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Hibák elkerülhető, ha a sablon és a telepítés előtt infrastruktúra érvényesítése. További kérelem és a válasz információkat, amelyek akkor lehet hasznos, később az alábbiakkal a telepítés során is jelentkezzen. Ellenőrzése és a kérelem és válasz naplóadatokat kapcsolatos további tudnivalókért olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy-cli.md)című témakört.

## <a name="use-audit-logs-to-troubleshoot"></a>Használja a naplókat kapcsolatos hibák elhárítása

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Telepítés hibák megtekintéséhez kövesse az alábbi lépéseket:

1. Ha látni szeretné a naplókat, a **naplófájl azure csoport megjelenítése** parancsot. Is elhelyezhet a **– utolsó-deployment** csak a napló a legutóbbi telepítéshez beolvasni lehetőséget.

        azure group log show ExampleGroup --last-deployment

2. A **napló azure csoport megjelenítése** parancs a sok olyan fontos információt adja eredményül. Hibaelhárítási, általában kívánt műveleteket, amelyeket nem sikerült összpontosíthat. A következőt használja a **--json** lehetőséget, és a [jq](https://stedolan.github.io/jq/) JSON segédprogram telepítési hibák keresése.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Megtekintheti a **Tulajdonságok** tartalmaz információkat a json sikertelen működéséről.

    Használhatja a **– részletes** és **-"w"** beállításai lehetőséget választva megtekintheti a naplókat további adatait.  Használja a **– részletes** jelölőnégyzetet, azzal megjeleníti a műveleteket hajtsa végre a lépéseket `stdout`. Egy teljes kérelem előzmények **-"w"** lehetőséggel. Az üzenetek gyakran megadja a alapvető tudnivalók a hibák okát.

3. Hibás bejegyzések állapotüzenete kiemelése, használja az alábbi parancsot:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Üzembe helyezési műveleteket használatával kapcsolatos hibák elhárítása

1. A környezet, amelyben az **azure csoport telepítési megjelenítése** parancs általános állapotuk. Az alábbi példában a telepítés nem sikerült.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. A telepítés nem sikerült műveletek üzenet jelenik meg, hogy használja:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Következő lépések

- Az adott telepítési hibák megoldása című témakörben kaphat segítséget [Azure Azure erőforrás-kezelővel erőforrások telepítésekor a gyakori hibák megoldásához](resource-manager-common-deployment-errors.md).
- Műveletek más típusú figyelheti a naplókat használatával kapcsolatos további tudnivalókért lásd: [az erőforrás-kezelő műveletek naplózási](resource-group-audit.md).
- A telepítési azt végrehajtása előtt ellenőrizendő lásd: [az erőforrás-kezelő Azure sablonnal erőforráscsoport Deploy](resource-group-template-deploy.md).
