<properties
    pageTitle="Hozzon létre egy IoT hubhoz egy ARM sablon és a C# |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban Ismerkedés az Azure erőforrás-kezelő sablonok használatával hogyan hozhat létre egy IoT központi C# programot."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Egy IoT hubhoz C# program használata egy erőforrás-kezelő Azure-sablon létrehozása

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>– Bevezetés

Erőforrás-kezelő Azure hozhat létre és kezelhet az Azure IoT hubok programozás útján is használhatja. Ebből az oktatóanyagból megtudhatja, hogy miként, hozzon létre egy IoT központi C# programból egy erőforrás-kezelő Azure-sablon segítségével.

> [AZURE.NOTE] Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: [Azure erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk bemutatja, hogy az erőforrás-kezelő Azure telepítési modellt használja.

Ebben az oktatóanyagban a következőkre lesz szüksége:

- Microsoft Visual Studio 2015.
- Azure active fiók. <br/>Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- [Azure tároló fiók] [ lnk-storage-account] Azure erőforrás-kezelő sablon fájlok tárolására szolgál.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb verziójában.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>A Visual Studio projekt előkészítése

1. A Visual Studióban a **New** project-sablon segítségével vizuális C# Windows projektet létrehozni. A projekt **CreateIoTHub**neve.

2. A megoldás Intézőben kattintson a jobb gombbal a projekten, és kattintson a **NuGet csomagok kezelése**gombra.

3. Nuget csomag Manager, jelölje be az **előzetes tartalmazza**, és **Microsoft.Azure.Management.ResourceManager**keresése. Kattintson a **telepítés**gombra, a **Változások áttekintése** kattintson az **OK gombra**, majd kattintson az **Elfogadás lehet** fogadja el a licenceket.

4. Az Nuget csomag-kezelőben **Microsoft.IdentityModel.Clients.ActiveDirectory**keresni.  Kattintson a **telepítés**gombra, a **Változások áttekintése** kattintson az **OK gombra**, majd kattintson az **Elfogadás lehet** fogadja el a licenc.

5. Program.cs cserélje ki a meglévő kimutatások **használatával** a következőket:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. Az Program.cs adja meg a következő statikus változók cseréje a helyőrző értékeket. Egy megjegyzés **ApplicationId**, **SubscriptionId**, **TenantId**és a **jelszó** elvégzett az oktatóprogram korábbi részében. **Az Azure tároló fiók** neve az Azure tároló fiók nevét a Azure erőforrás-kezelő sablon fájlok tárolására. **Erőforráscsoport neve** a csoport nevét, az erőforrás a IoT központi létrehozásakor használ, ez lehet egy meglévő erőforráscsoport vagy egy újat. **Telepítési** neve a telepítéshez, például **Deployment_01**nevét.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Egy erőforrás-kezelő Azure sablont hozhat létre egy IoT központi elküldése

Hozzon létre egy IoT központi az erőforráscsoport a JSON sablon és a paraméterek fájl használatával. Az erőforrás-kezelő Azure-sablon segítségével egy meglévő IoT hubhoz végezze el a módosításokat.

1. A megoldás Intézőben kattintson a jobb gombbal a projekt, kattintson a **Hozzáadás**gombra, és kattintson az **Új elem**parancsra. Adja hozzá a **template.json** a projekthez című JSON-fájlt.

2. **Template.json** tartalmának cserélje ki a következő erőforrás-definíciót egy szabványos IoT hubhoz hozzáadása a **Kelet-Amerikai Egyesült Államok** régióban (az aktuális listáját, a régiók IoT központi támogató című [Azure állapot][lnk-status]):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

3. A megoldás Intézőben kattintson a jobb gombbal a projekt, kattintson a **Hozzáadás**gombra, és kattintson az **Új elem**parancsra. Adja hozzá a **parameters.json** a projekthez című JSON-fájlt.

4. **Parameters.json** tartalmának cserélje ki a következő paraméterek információkat, például: állítja be a nevét az új IoT-központban **{a monogram} mynewiothub**. Tartsa szem előtt, hogy a IoT központi nevének egyedinek kell lennie globálisan, a nevét vagy monogramját tartalmaznia kell):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. Az **Intéző Server**Azure-előfizetéséhez, az Azure-tárolóban lévő fiók hozhat létre, és **sablonok**nevű tároló. Kattintson a **Tulajdonságok** ablaktábla állítsa a **sablonok** tároló **nyilvános olvasási hozzáférést** engedélyeinek **Blob**.

6. A **Kiszolgáló Intéző**kattintson a jobb gombbal a **sablonok** tároló, és válassza a **View Blob-tárolóhoz**. Kattintson a **Feltöltés Blob** gombra, jelölje ki a két fájlokat, **parameters.json** és **templates.json**, és kattintson a **Megnyitás** a JSON-fájlok feltöltése a **sablonok** tároló. A JSON adatokat tartalmazó BLOB az URL-címét a következők:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. A következő módszerrel Program.cs felvétele:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. A következő kód hozzáadása a **CreateIoTHub** módszer a sablon és a paraméterek fájlokat az Azure erőforrás-kezelő elküldése:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. A következő kód hozzáadása a **CreateIoTHub** módszer, amely megjeleníti az állapot és az új IoT-központban billentyűparancsai:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Töltse ki, és futtassa az alkalmazást

Hívja fel a **CreateIoTHub** módszer, mielőtt össze, és indítsa el az alkalmazás letöltése végezhetik el.

1. A következő kód hozzáadása a **fő** módszer végén:

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Kattintson a **Szerkesztés** , majd **megoldás**. Javítsa ki a hibákat.

3. Kattintson **a hibakeresési** , majd **Hibakeresési indítsa el** az alkalmazásnak a futtatására. Eltarthat néhány percig, amíg a telepítő futtatását.

4. Ellenőrizheti, hogy az alkalmazás az [Azure portálon] megtalálhatók az új IoT-központban hozzáadott[ lnk-azure-portal] és erőforrások, illetve a **Get-AzureRmResource** PowerShell-parancsmag használatával listájának megtekintése.

> [AZURE.NOTE] Példa alkalmazás hozzáadása egy S1 szabványos IoT központi, amelynek számlázható. Törölheti a IoT hubon keresztül az [Azure portál] [ lnk-azure-portal] , vagy ha befejezte a **Eltávolítása-AzureRmResource** PowerShell-parancsmag használatával.

## <a name="next-steps"></a>Következő lépések

Miután telepítette az erőforrás-kezelő Azure sablonnal C# programmal IoT központi, érdemes további feltárása:

- További információ: a [IoT központi erőforrás szolgáltató REST API -t]a funkcióinak[lnk-rest-api].
- Olvassa el az [Azure erőforrás szolgáltatásának áttekintése] [ lnk-azure-rm-overview] Ha többet szeretne tudni az Azure erőforrás-kezelő lehetőségeit.

IoT központi fejlesztésével kapcsolatos további információért olvassa el az alábbiakat:

- [C SDK – bevezetés][lnk-c-sdk]
- [IoT központi SDK][lnk-sdks]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
