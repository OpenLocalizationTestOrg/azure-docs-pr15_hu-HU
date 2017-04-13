<properties
    pageTitle="Hozzon létre egy a REST API IoT hubhoz |} Microsoft Azure"
    description="Kövesse az ebben az oktatóanyagban első lépésiről a REST API-IoT központi létrehozásához."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Oktatóprogram: Hozzon létre egy IoT hubhoz C# program és a REST API segítségével

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>– Bevezetés

Használhatja a [IoT központi erőforrás szolgáltató REST API] [ lnk-rest-api] hozhat létre és kezelhet az Azure IoT hubok programozás útján. Ebből az oktatóanyagból megtudhatja, hogy miként, létrehozására is használhatja a IoT központi erőforrás szolgáltató REST API-IoT központi C# programból.

> [AZURE.NOTE] Azure magában az erőforrások létrehozásáról és használatáról a két különböző telepítési modellek: [Azure erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk bemutatja, hogy az erőforrás-kezelő Azure telepítési modellt használja.

Ebben az oktatóanyagban a következőkre lesz szüksége:

- Microsoft Visual Studio 2015.
- Azure active fiók. <br/>Ha nem rendelkeznek fiókkal, létrehozhat egy [ingyenes fiókot] [ lnk-free-trial] a mindössze néhány perc.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] vagy újabb verziójában.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>A Visual Studio projekt előkészítése

1. A Visual Studióban a **New** project-sablon segítségével vizuális C# Windows projektet létrehozni. A projekt **CreateIoTHubREST**neve.

2. A megoldás Intézőben kattintson a jobb gombbal a projekten, és kattintson a **NuGet csomagok kezelése**gombra.

3. Nuget csomag Manager, jelölje be az **előzetes tartalmazza**, és **Microsoft.Azure.Management.ResourceManager**keresése. Kattintson a **telepítés**gombra, a **Változások áttekintése** kattintson az **OK gombra**, majd kattintson az **Elfogadás lehet** fogadja el a licenceket.

4. Az Nuget csomag-kezelőben **Microsoft.IdentityModel.Clients.ActiveDirectory**keresni.  Kattintson a **telepítés**gombra, a **Változások áttekintése** kattintson az **OK gombra**, majd kattintson az **Elfogadás lehet** fogadja el a licenc.

6. Program.cs cserélje ki a meglévő kimutatások **használatával** a következőket:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. Az Program.cs adja meg a következő statikus változók cseréje a helyőrző értékeket. Egy megjegyzés **ApplicationId**, **SubscriptionId**, **TenantId**és a **jelszó** elvégzett az oktatóprogram korábbi részében. **Erőforráscsoport neve** a csoport nevét, az erőforrás a IoT központi létrehozásakor használ, ez lehet egy meglévő erőforráscsoport vagy egy újat. **IoT központi** neve a IoT hubhoz hoz létre, például **MyIoTHub** (Ez a név egyedinek kell lennie globálisan, a nevét vagy monogramját tartalmaznia kell) nevére. **Telepítési** neve a telepítéshez, például **Deployment_01**nevét.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Hozzon létre egy IoT központi a REST API segítségével

Az [IoT központi REST API] [ lnk-rest-api] az erőforráscsoport egy IoT központi létrehozásához. A REST API segítségével egy meglévő IoT hubhoz végezze el a módosításokat.

1. A következő módszerrel Program.cs felvétele:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. A **CreateIoTHub** módszerrel **HttpClient** objektum létrehozását a hitelesítési jogkivonat, a fejlécekben hozzá a következő kódot:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. A következő kód hozzáadása a **CreateIoTHub** módszerrel ahhoz a IoT központi hozhat létre, és a JSON megadott készítése (a jelenlegi listáját, amelyek támogatják a IoT központi helyen című [Azure állapot][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. A többi kérelmet Azure, jelölje be a választ, valamint az URL-címet is használhatja a telepítési feladat állapotának figyelheti a **CreateIoTHub** módszerrel hozzá a következő kódot:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. A következő kód hozzáadása a **CreateIoTHub** módszer, hogy csak a telepítés elvégzéséhez olvassa be az előző lépésben **asyncStatusUri** címet használhatja végén:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. A következő kód hozzáadása a **CreateIoTHub** módszer az IoT-központban létrehozott és nyomtathatja ki őket a konzolhoz kulcsok végén:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Töltse ki, és futtassa az alkalmazást

Hívja fel a **CreateIoTHub** módszer, mielőtt össze, és indítsa el az alkalmazás letöltése végezhetik el.

1. A következő kód hozzáadása a **fő** módszer végén:

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Kattintson a **Szerkesztés** , majd **megoldás**. Javítsa ki a hibákat.

3. Kattintson **a hibakeresési** , majd **Hibakeresési indítsa el** az alkalmazásnak a futtatására. Eltarthat néhány percig, amíg a telepítő futtatását.

4. Ellenőrizheti, hogy az alkalmazás az [Azure portálon] megtalálhatók az új IoT-központban hozzáadott[ lnk-azure-portal] és erőforrások, illetve a **Get-AzureRmResource** PowerShell-parancsmag használatával listájának megtekintése.

> [AZURE.NOTE] Példa alkalmazás hozzáadása egy S1 szabványos IoT központi, amelynek számlázható. Ha befejezte a szerkesztést, törölheti a IoT hubon keresztül az [Azure portál] [ lnk-azure-portal] , vagy ha befejezte a **Eltávolítása-AzureRmResource** PowerShell-parancsmag használatával.

## <a name="next-steps"></a>Következő lépések

Miután telepítette a REST API-IoT-elosztóhoz, érdemes további feltárása:

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

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
