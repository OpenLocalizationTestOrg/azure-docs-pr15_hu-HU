<properties
   pageTitle="Erőforrások REST API-val és a sablon telepítése |} Microsoft Azure"
   description="Azure erőforrás-kezelő és erőforrás-kezelő REST API-t használja a telepítéshez használni egy erőforrás Azure. Az erőforrások vannak definiálva, az erőforrás-kezelő sablon"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Erőforrás-kezelő sablonok és erőforrás-kezelő REST API erőforrások terjesztése

> [AZURE.SELECTOR]
- [A PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portál](resource-group-template-deploy-portal.md)
- [REST API-VAL](resource-group-template-deploy-rest.md)

Ez a cikk ismerteti, hogyan erőforrás-kezelő sablonok Azure szeretne telepíteni, az erőforrások az erőforrás-kezelő REST API használatára.  

> [AZURE.TIP] A hiba hibakeresés a telepítés során című témakörben talál segítséget:
>
> - [Nézet üzembe helyezési műveleteket REST API-val](resource-manager-troubleshoot-deployments-rest.md) kapcsolatos információ, amely segít a hiba elhárítása
> - [Gyakori hibák elhárítása az Azure erőforrás-kezelő Azure erőforrások telepítésekor](resource-manager-common-deployment-errors.md) módjáról a gyakori telepítési hibáinak elhárítása

A sablon lehet egy helyi fájl vagy URI keresztül elérhető külső fájlba. Ha a sablon a tárterület-fiókjában található, korlátozhatja a hozzáférést a sablont, és adja meg a megosztott aláírás (Társítások) jogkivonat a telepítés során.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>A REST API-val terjesztése
1. Állítsa be [közös paramétereket és a fejlécek](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), beleértve a hitelesítési tokenek.
2. Ha nem egy meglévő erőforráscsoport, erőforráscsoport létrehozása. Az előfizetés azonosítója, a nevét, valamint új erőforráscsoport helyre van szüksége a megoldást nyújtanak. További tudnivalókért lásd: az [erőforrás csoport létrehozása](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. A telepítés előtt futtatásával a [érvényesítése egy sablon telepítési](https://msdn.microsoft.com/library/azure/dn790547.aspx) művelet végrehajtása, az Érvényesítés gombra. Amikor a központi telepítés tesztelése, adja meg a paramétereket pontosan módon a (lásd a következő lépés) telepítésben végrehajtásakor.

3. Hozzon létre egy telepítés. Adja meg az előfizetés azonosítója, a csoport nevét, az erőforrás üzembe helyezéséhez nevét a telepítési és hivatkozás a sablonhoz. A sablonfájlt információt [paraméter-fájl](#parameter-file)megtekintéséhez. Többet szeretne tudni a REST API-t egy erőforrás csoportot létrehozni a [sablon telepítés létrehozása](https://msdn.microsoft.com/library/azure/dn790564.aspx)című témakör tartalmaz. Értesítés a **mód** **Léptetési távolság**értékre van állítva. Egy teljes telepítési futtatásához **kész** **mód** beállításához. Ügyeljen arra, hogy a teljes mód használatakor, mint információforrások, amelyek nem találhatók meg a sablon véletlenül törölheti.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
              }
            }
          }
   
      Jelentkezzen be a választ tartalmat szeretne, ha kérelem tartalmakat, vagy mindkettőt bele **debugSetting** a kérést.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Beállíthatja, hogy a tárterület-fiók használni egy megosztott jogkivonat aláírás (Társítások). További tudnivalókért olvassa el a [Megosztott Access aláírás hozzáférés delegálása](https://msdn.microsoft.com/library/ee395415.aspx)című témakört.

4. A sablon példányban állapotuk. További tudnivalókért lásd: a [sablon telepítés adatainak](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Következő lépések
- Példa üzembe helyezése a .NET-ügyfél tár keresztül erőforrások olvassa el a [Deploy erőforrások .NET-tárak és a sablon használatával](virtual-machines/virtual-machines-windows-csharp-template.md)című témakört.
- A paraméterek az sablont, olvassa el a [sablonok létrehozása](resource-group-authoring-templates.md#parameters).
- A megoldást üzembe helyezné a különböző környezetekben, olvassa el [fejlesztés és a próba-verziót tartalmazó környezetek Microsoft Azure-ban](solution-dev-test-environments.md).
- Biztonságos értékeket adják át KeyVault hivatkozást használatával kapcsolatos részletekért lásd: [a telepítés során biztonságos értékeket adják át](resource-manager-keyvault-parameter.md).
