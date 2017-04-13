<properties
    pageTitle="Alkalmazás szolgáltatás API-alkalmazások metaadatainak API feltárás és kód generációs |} Microsoft Azure"
    description="Megtudhatja, hogyan API-alkalmazások Azure App szolgáltatásban Swagger metaadatok használatának megkönnyítése érdekében API feltárás és kód létrehozása."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Alkalmazás szolgáltatás API-alkalmazások metaadatokat API feltárás és kód létrehozása céljából 

API-metaadatok [Swagger 2.0](http://swagger.io/) támogatási szolgáltatás API Alkalmazásindítónalkalmazások beépített. Nem kell Swagger használni, de ha használ, kihasználhatja API-alkalmazások funkciók, amelyek megkönnyítik a feltárás és felhasználási.   

## <a name="swagger-endpoint"></a>Végpont swagger

Zárólap, ahol a 2.0-s JSON Swagger metaadatok az API-alkalmazás tulajdonságban az API-alkalmazásokban is megadhat. Az endpoint viszonyított alap URL-CÍMÉT az API-alkalmazást vagy az abszolút URL lehet. Abszolút URL-címeit is mutathat kívül az API-alkalmazást. 

Sok lefelé irányuló ügyfél (a példában a Visual Studio kód generálása és PowerApps "API hozzáadása" forgalom), az URL-címet kell nyilvánosan hozzáférhető (felhasználó vagy a hitelesítési szolgáltatás nem védett). Ez azt jelenti, hogy ha az alkalmazás szolgáltatás hitelesítési használ, és szeretné elérhetővé tenni a API definíció magát az alkalmazásból, akkor használja, amely lehetővé teszi, hogy elérje a API névtelen forgalom hitelesítést követeljen meg. További tudnivalókért lásd: a [hitelesítési és az alkalmazás szolgáltatás API-alkalmazások engedélyezési](app-service-api-authentication.md).

### <a name="portal-blade"></a>A lap portál

Az [Azure portál](https://portal.azure.com/) végpont URL-CÍMÉT is látható és az **API-definíciós** lap megváltozott.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure erőforrás-kezelő tulajdonság

Parancssori eszközök, például az [Azure PowerShell](../powershell-install-configure.md) és az [Azure CLI](../xplat-cli-install.md) [Erőforrás Explorer](https://resources.azure.com/) vagy az [erőforrás-kezelő Azure sablonok](../resource-group-authoring-templates.md) használatával is beállíthatja API definition URL-CÍMÉT az API-alkalmazásokban. 

Nyissa meg az **Erőforrás Explorer**, **előfizetések > {az előfizetés} > resourceGroups > {az erőforráscsoport} > szolgáltatók > Microsoft.Web > webhelyek > {a webhely} > Beállítások > webes**, jelennek meg a `apiDefinition` tulajdonság:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Példa egy erőforrás-kezelő Azure-sablon állítja be a `apiDefinition` tulajdonság, nyissa meg a [Feladatlista minta alkalmazásban azuredeploy.json fájlt](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Keresse meg a sablont, amelyet a fent látható JSON minta néz szakasza.

### <a name="default-value"></a>Alapérték

Ha Visual Studio hozhat létre API-alkalmazást használja, az API-definíciós végpontot automatikusan beírja a plusz az API-alkalmazás alap URL-címe `/swagger/docs/v1`. Ez az alapértelmezett URL-címet a [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet csomag alkalmazó dinamikusan létrehozott Swagger metaadatok ASP.NET webes API projekt szolgáló. 

## <a name="code-generation"></a>Kód létrehozása

Swagger integrálása az Azure API-alkalmazások előnyeiről egyik kód automatikus generálása. Létrehozott ügyfél osztályok megkönnyítik a kódírás felhívja az API-alkalmazásokban.

Ügyfél-kódot az API-alkalmazásokban a parancssorból vagy Visual Studio segítségével hozhat létre. Ügyfél osztályok készítése a Visual Studio projekt ASP.NET webes API-val kapcsolatos további tudnivalókért lásd [API-alkalmazások és az ASP.NET első lépések](app-service-api-dotnet-get-started.md#codegen). Információ arról, hogy miként teheti a parancssorból által támogatott összes nyelven GitHub.com az [Azure/autorest](https://github.com/azure/autorest) tárházba a fontos fájl kapcsolatban.
 
## <a name="next-steps"></a>Következő lépések

A részletes tanulásra, amely végigvezeti Önt a létrehozás üzembe helyezése, és az API-alkalmazásokban használata más lásd: [Azure App szolgáltatásban API-alkalmazások – első lépések](app-service-api-dotnet-get-started.md).

Azure API-kezelés az API-alkalmazások használja, ha a Swagger metaadat-alapú az API importálhatja az API-kezelés is használhatja. További információért megtudhatja, [hogy miként importálhat egy API-val kapcsolatos műveletek Azure API-kezelés definícióját](../api-management/api-management-howto-import-api.md). 
