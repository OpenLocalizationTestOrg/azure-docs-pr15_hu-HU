<properties
   pageTitle="Telepítési műveletek REST API-val megtekintése |} Microsoft Azure"
   description="Az Azure erőforrás-kezelő REST API-t használja az erőforrás-kezelő telepítési problémák feltárása ismerteti."
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
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Azure erőforrás-kezelő REST API-val telepítési műveletek megtekintése

> [AZURE.SELECTOR]
- [Portál](resource-manager-troubleshoot-deployments-portal.md)
- [A PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-VAL](resource-manager-troubleshoot-deployments-rest.md)

Ha hiba erőforrások Azure telepítésekor arról, érdemes többet szeretne tudni az üzembe helyezési műveleteket, amelyeket végrehajtva. A REST API biztosít a műveleteket, amelyek lehetővé teszik, hogy a hibák keresése és lehetséges javítások határozza meg.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Hibák elkerülhető, ha a sablon és a telepítés előtt infrastruktúra érvényesítése. További kérelem és a válasz információkat, amelyek akkor lehet hasznos, később az alábbiakkal a telepítés során is jelentkezzen. Ellenőrzése és a kérelem és válasz naplóadatokat kapcsolatos további tudnivalókért olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy-rest.md)című témakört.

## <a name="troubleshoot-with-rest-api"></a>REST API-val kapcsolatos hibák elhárítása

1. Az erőforrások [létrehozása a sablon telepítésének](https://msdn.microsoft.com/library/azure/dn790564.aspx) művelettel üzembe. Amely akkor lehet hasznos, a hibakereséshez megőrzéséhez a **debugSetting** tulajdonság **requestContent** , illetve **responseContent**JSON-összehívásban. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Alapértelmezés szerint a **debugSetting** értéke Nincs értékre **állításával**. A **debugSetting** érték megadásakor körültekintően fontolja meg a telepítés során áthaladó az információtípust. A kérések és válaszok naplózás információkért által esetleg bizalmas adatokat másolja át az üzembe helyezési műveleteket sikerült nézetéhez. 

2. A környezet, amelyben a [adatainak megtekintése a sablon telepítési](https://msdn.microsoft.com/library/azure/dn790565.aspx) művelet kapcsolatos tájékozódáshoz.

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    A válasz jegyezze fel különösen a **provisioningState** , **correlationId** és **hiba** elemeket. A **correlationId** kapcsolódó események nyomon követéséhez szolgál, és is lehet hasznos, ha a probléma elhárításához technikai támogatási használata.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Információ a [lista összes sablon telepítési művelet](https://msdn.microsoft.com/library/azure/dn790518.aspx) művelettel Üzembe helyezési műveleteket. 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    A válasz mi megadott a **debugSetting** tulajdonság a telepítés során alapján kérés és/vagy a válasz információkat tartalmazza.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Események letölthető a naplókat a környezet, amelyben a [listában az előfizetés management események](https://msdn.microsoft.com/library/azure/dn931934.aspx) műveletet.

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Következő lépések

- Az adott telepítési hibák megoldása című témakörben kaphat segítséget [Azure Azure erőforrás-kezelővel erőforrások telepítésekor a gyakori hibák megoldásához](resource-manager-common-deployment-errors.md).
- Műveletek más típusú figyelheti a naplókat használatával kapcsolatos további tudnivalókért lásd: [az erőforrás-kezelő műveletek naplózási](resource-group-audit.md).
- A telepítési azt végrehajtása előtt ellenőrizendő olvassa el a [Deploy Azure erőforrás-kezelő sablonnal erőforráscsoport](resource-group-template-deploy.md)című témakört.
