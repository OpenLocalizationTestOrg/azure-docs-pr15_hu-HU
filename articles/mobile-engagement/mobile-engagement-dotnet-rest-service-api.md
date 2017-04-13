<properties 
    pageTitle="A REST API-t használja az Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz eléréséhez" 
    description="A mobil tetszés szerint elmélyedhet REST API-k használata Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz eléréséhez"       
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="wesmc;ricksal" />

#<a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz elérése a többi segítségével

Azure Mobile tetszés szerint elmélyedhet biztosít az [Azure Mobile tetszés szerint elmélyedhet REST API -val](https://msdn.microsoft.com/library/azure/mt683754.aspx) való eszközkezelés, vannak/leküldéses kampányok stb. Ez a példa fogja használni a REST API-k közvetlenül egy bejelentése marketingkampány létrehozása, majd aktiválása és eszközök használatával egy leküldéses azt. 

Ha nem szeretné, hogy közvetlenül a REST API-k használandó, kínálunk is, amely egy [fájl Swagger](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) használható eszközök SDK hozható létre a használni kívánt nyelvet. Azt javasoljuk, hogy a SDK készítése saját Swagger-fájlból a [AutoRest](https://github.com/Azure/AutoRest) eszközzel. Egy hasonló módon, amely lehetővé teszi, hogy ezek API-k használata a C# csomagolópapír létrehozott egy .NET SDK van, és a hitelesítési jogkivonat küldött tevékenységhozzárendelés-frissítések és frissítése a saját maga nincs. Használja a csomagolópapír hasonló minta elemre, című témakörben olvashat [Szolgáltatás API .NET SDK – példa](mobile-engagement-dotnet-sdk-service-api.md)

Ez a példa fogja használni a REST API-k közvetlenül létrehozása és egy értesítés marketingkampány aktiválása. 

## <a name="adding-a-username-appinfo-to-the-mobile-engagement-app"></a>A felhasználónév appInfo hozzáadása a Mobile tetszés szerint elmélyedhet alkalmazásba

Ez a példa egy app – útmutatók címkét a Mobile tetszés szerint elmélyedhet alkalmazásba felvett van szükség. Az alkalmazás tetszés szerint elmélyedhet portálon, felveheti a címke **beállításai**gombra kattintva > **címke (app – útmutatók)** > **Új címke (app – útmutatók)**. Adja hozzá az új címke nevű **felhasználónév** **karakterlánc** típus.

![](./media/mobile-engagement-dotnet-rest-service-api/user-name-app-info.png)



Ha követte az [Azure Mobile tetszés szerint elmélyedhet a Windows univerzális alkalmazások – első lépések](mobile-engagement-windows-store-dotnet-get-started.md), a **felhasználónév** címke küldése által egyszerűen felülbírálása tesztelheti `OnNavigatedTo()` a a `MainPage` osztály küldeni az app – útmutatók címke hasonlóan, mint a következő kódot:

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        String name = "Wesley"; // Prompt the user to provide this in your client app.

        Dictionary<object, object> info = new Dictionary<object, object>();
        info.Add("user_name", name);
        EngagementAgent.Instance.SendAppInfo(info);
    }
 


## <a name="creating-the-service-api-app"></a>A szolgáltatás API-alkalmazás létrehozása

1. Első lépésként szüksége lesz négy hitelesítési paraméter használata a következő példában. Ezek a paraméterek **SubscriptionId**, **TenantId**, **ApplicationId** és **titkos**. Úgy juthat az alábbi hitelesítési paramétereket, ajánlott használata az *egyszeri beállításához a (parancsfájl használatával)* területen a [hitelesítési](mobile-engagement-api-authentication.md#authentication) oktatóanyagból említett PowerShell parancsprogramot megközelítés. 

2. Egy egyszerű Windows konzol alkalmazás a többi szolgáltatás API-k létrehozása, és egy új bejelentése marketingkampány aktiválása használata keresztül mutatjuk fogjuk használni. Így nyissa meg a Visual Studióban, és hozzon létre egy új **New**.   

3. Ezután a **Newtonsoft.Json** NuGet csomag hozzáadása a projekthez.

4. Az a `Program.cs` fájlt, és adja meg a következő `using` utasításait a következő névtér:

        using System.IO;
        using System.Net;
        using Newtonsoft.Json.Linq;
        using Newtonsoft.Json;

5. Ezután meg kell határoznia a következő témakörben a `Program` osztály. Ezek lesznek hitelesítéshez használt és a Mobile tetszés szerint elmélyedhet alkalmazásban a bejelentése marketingkampány létrehozásához egymással kölcsönhatásban lévő:


        // Parameters needed for authentication of API calls.
        // These are returned from the PowerShell script in the authentication tutorial. 
        // https://azure.microsoft.com/documentation/articles/mobile-engagement-api-authentication/
        static String SubscriptionId = "<Your Subscription Id>";
        static String TenantId = "<Your TenantId>";
        static String ApplicationId = "<Your Application Id>";
        static String Secret = "<Your Secret>";

        // The token for authenticating with your Mobile Engagement app.
        static String Token = null;

        // This is the Azure Resource group concept for grouping together resources 
        // See: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        static String ResourceGroup = "MobileEngagement";

        // For Mobile Engagement operations
        // App Collection Name from the Azure portal 
        static String Collection = "<Your App Collection Name>";

        // Application Resource Name - make sure you are using the one as specified in the dashboard of the
        // Azure portal. (This is NOT the App Name)
        static String AppName = "WesmcRestTest-windows";

        // New campaign id returned from creating the new campaign and used to activate and push the campaign
        // a set of devices.
        static String NewCampaignID = null;

        // This list will hold the device Ids we choose to push the campaign to.
        static List<String> deviceList = new List<String>();


6. Adja hozzá az alábbi két módszer, hogy a `Program` osztály. Ezek a REST API-khoz végrehajtása, és a válaszokat a konzol megjelenítéséhez lesz.

        private static async Task<HttpWebResponse> ExecuteREST(string httpMethod, string uri, string authToken, WebHeaderCollection headers = null, string body = null, string contentType = "application/json")
        {
            //=== Execute the request 
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(uri);
            HttpWebResponse response = null;

            request.Method = httpMethod;
            request.ContentType = contentType;
            request.ContentLength = 0;

            if (authToken != null)
                request.Headers.Add("Authorization", authToken);

            if (headers != null)
            {
                request.Headers.Add(headers);
            }

            if (body != null)
            {
                byte[] bytes = Encoding.UTF8.GetBytes(body);

                try
                {
                    request.ContentLength = bytes.Length;
                    Stream requestStream = request.GetRequestStream();
                    requestStream.Write(bytes, 0, bytes.Length);
                    requestStream.Close();
                }
                catch (Exception e)
                {
                    Console.WriteLine(e.Message);
                }
            }

            try
            {
                response = (HttpWebResponse)await request.GetResponseAsync();
            }
            catch (WebException we)
            {
                if (we.Response != null)
                {
                    response = (HttpWebResponse)we.Response;
                }
                else
                    Console.WriteLine(we.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

            return response;
        }


        private static void DisplayResponse(dynamic data, HttpWebResponse response)
        {
            Console.WriteLine("HTTP Status " + (int)response.StatusCode + " : " + response.StatusDescription);
            Console.WriteLine(data + "\n");
        }

    }

7. Adja hozzá a következő kódot a `Main` módszer egy hitelesítési jogkivonat összegyűjtött hitelesítési paraméterekkel létrehozásához:

        //***************************************************************************
        //*** Get a valid authorization token with your authentication parameters ***
        //***************************************************************************

        String methodUrl = "https://login.microsoftonline.com/" + TenantId + "/oauth2/token";
        String requestBody = "grant_type=client_credentials&client_id=" + ApplicationId +
                            "&client_secret=" + Secret +
                            "&resource=https://management.core.windows.net/";
        Console.WriteLine("Getting authorization token...\n");
        HttpWebResponse response = ExecuteREST("POST", methodUrl, null, null, requestBody, null).Result;
        Stream receiveStream = response.GetResponseStream();
        StreamReader readStream = new StreamReader(receiveStream, Encoding.UTF8);
        dynamic data = JObject.Parse(readStream.ReadToEnd());
        readStream.Close();
        receiveStream.Close();
        DisplayResponse(data, response);

        if (response.StatusCode == HttpStatusCode.OK)
        {
            Token = data.token_type + " " + data.access_token;
            Console.WriteLine("Got authorization token...");
            Console.WriteLine("Authorization : " + Token + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to get authorization token. Check your parameters for API calls.\n");
            return;
        }

8. Most, hogy elkészült a érvényes hitelesítési jogkivonat létrehozunk egy új vannak marketingkampány [létrehozása marketingkampány](https://msdn.microsoft.com/library/azure/mt683742.aspx) REST API-t használja. Az új marketingkampány lesz egy egyszerű **bármikor** & **csak értesítés** bejelentése marketingkampány egy címet és egy üzenetet. Ez a kézi leküldéses marketingkampány lesz, az alábbi képernyőfelvételen látható módon. Ez azt jelenti, hogy fog csak kell eltolni az API-k használata.


    ![](./media/mobile-engagement-dotnet-rest-service-api/manual-push.png)

    Adja hozzá a következő kódot a `Main` módszer a bejelentése marketingkampány létrehozása: 

        //*****************************************************************************
        //*** Create a campaign to send a notification using the user-name app-info ***
        //*****************************************************************************

        String newCampaignMethodUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
               "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
               Collection + "/apps/" + AppName + "/campaigns/announcements?api-version=2014-12-01";

        String campaignRequestBody = "{ \"name\": \"BirthdayCoupon\", " +
                                        "\"type\": \"only_notif\", " +
                                        "\"deliveryTime\": \"any\", " +
                                        "\"notificationType\": \"popup\", " +
                                        "\"pushMode\":\"manual\", " +
                                        "\"notificationTitle\": \"Happy Birthday ${user_name}\", " +
                                        "\"notificationMessage\": \"Take extra 10% off on your orders today!\"}";

        Console.WriteLine("Creating new campaign...\n");
        HttpWebResponse newCampaignResponse = ExecuteREST("POST", newCampaignMethodUrl, Token, null, campaignRequestBody).Result;
        Stream receiveCampaignStream = newCampaignResponse.GetResponseStream();
        StreamReader readCampaignStream = new StreamReader(receiveCampaignStream, Encoding.UTF8);
        dynamic newCampaignData = JObject.Parse(readCampaignStream.ReadToEnd());
        readCampaignStream.Close();
        receiveCampaignStream.Close();
        DisplayResponse(newCampaignData, newCampaignResponse);

        if (newCampaignResponse.StatusCode == HttpStatusCode.Created)
        {
            NewCampaignID = newCampaignData.id;
            Console.WriteLine("Created new campaign...");
            Console.WriteLine("New Campaign Id    : " + NewCampaignID + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to create birthday campaign.\n");
            return;
        }


9. Mielőtt bármilyen eszközöket is tolni aktiválnia kell a marketingkampány. Azt menti az új marketingkampány az Azonosítóját a `NewCampaignID` változó. Fogjuk használni ezt URI elérési út paraméterként aktiválása a marketingkampány az [Aktiválás marketingkampány](https://msdn.microsoft.com/library/azure/mt683745.aspx) REST API-t használja. Ez módosítsa a marketingkampány állapotának **ütemezett** annak ellenére, hogy azt fog csak kell eltolni kézi és az API-khoz.

    Adja hozzá a következő kódot a `Main` módszer a bejelentése marketingkampány aktiválása: 

        //******************************************
        //*** Activate the new birthday campaign ***
        //******************************************

        String activateCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID +
                   "/activate?api-version=2014-12-01";

        Console.WriteLine("Activating new campaign (ID : " + NewCampaignID + ")...\n");
        HttpWebResponse activateCampaignResponse = ExecuteREST("POST", activateCampaignUrl, Token).Result;
        Stream activateCampaignStream = activateCampaignResponse.GetResponseStream();
        StreamReader activateCampaignReader = new StreamReader(activateCampaignStream, Encoding.UTF8);
        dynamic activateCampaignData = JObject.Parse(activateCampaignReader.ReadToEnd());
        activateCampaignReader.Close();
        activateCampaignStream.Close();
        DisplayResponse(activateCampaignData, activateCampaignResponse);

        if (activateCampaignResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Activated new campaign...");
            Console.WriteLine("New Campaign State : " + activateCampaignData.state + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to activate birthday campaign.\n");
            return;
        }

10. A marketingkampány leküldéses azt kell az eszköz azonosítók a felhasználóknak, hogy azt szeretné, hogy a értesítést szeretne kapni a szükséges. A [lekérdezés-eszközök](https://msdn.microsoft.com/library/azure/mt683826.aspx) REST API-t az összes eszközt azonosítók megszerezni fogjuk használni. Hozzáadunk egyes eszközök azonosító listájához, ha társított **felhasználónév** appInfo.

    Adja hozzá a következő kódot a `Main` módszert felvenni az összes eszköz azonosítóját, és a deviceList feltöltése:

        //************************************************************************
        //*** Now that the manualPush campaign is activated, get the deviceIds ***
        //*** that you want to trigger the push campaign on.                   ***
        //************************************************************************

        String getDevicesUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/devices?api-version=2014-12-01";

        Console.WriteLine("Getting device IDs...\n");
        HttpWebResponse devicesResponse = ExecuteREST("GET", getDevicesUrl, Token).Result;
        Stream devicesStream = devicesResponse.GetResponseStream();
        StreamReader devicesReader = new StreamReader(devicesStream, Encoding.UTF8);
        dynamic devicesData = JObject.Parse(devicesReader.ReadToEnd());
        devicesReader.Close();
        devicesStream.Close();
        DisplayResponse(devicesData, devicesResponse);

        if (devicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Got devices.");
            foreach (dynamic device in devicesData.value)
            {
                // Just adding all in this example
                if (device.appInfo.user_name != null)
                {
                    Console.WriteLine("Adding device for push campaign...");
                    Console.WriteLine("Device ID    : " + device.deviceId);
                    Console.WriteLine("user_name    : " + device.appInfo.user_name);
                    deviceList.Add((String)device.deviceId);
                }
            }
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine("*** Failed to get devices.\n");
            return;
        }


11. Végezetül azt fogja leküldéses a marketingkampány összes eszköz azonosítójához a listában a [leküldéses marketingkampány](https://msdn.microsoft.com/library/azure/mt683734.aspx) REST API. Ez az **alkalmazás az** értesítést. Az alkalmazás lesz így futnia kell ahhoz, hogy a felhasználó megkapja az eszközön.

    Adja hozzá a következő kódot a `Main` módszert a campign leküldéses a készülékeken a deviceList:

        //**************************************************************
        //*** Trigger the manualPush campaign on the desired devices ***
        //**************************************************************

        String pushCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID + 
                   "/push?api-version=2014-12-01";

        Console.WriteLine("Triggering push for new campaign (ID : " + NewCampaignID + ")...\n");
        String deviceIds = "{\"deviceIds\":" + JsonConvert.SerializeObject(deviceList) + "}";
        Console.WriteLine("\n" + deviceIds + "\n");
        HttpWebResponse pushDevicesResponse = ExecuteREST("POST", pushCampaignUrl, Token, null, deviceIds).Result;
        Stream pushDevicesStream = pushDevicesResponse.GetResponseStream();
        StreamReader pushDevicesReader = new StreamReader(pushDevicesStream, Encoding.UTF8);
        dynamic pushDevicesData = JObject.Parse(pushDevicesReader.ReadToEnd());
        pushDevicesReader.Close();
        pushDevicesStream.Close();
        DisplayResponse(pushDevicesData, pushDevicesResponse);

        if (pushDevicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Triggered push on new campaign.\n");
        }
        else
        {
            Console.WriteLine("*** Failed to push campaign to the device list.\n");
        }


12. Építse fel, és futtassa a konzol alkalmazást. A kimenet kell lennie az alábbihoz hasonló:


        C:\Users\Wesley\AzME_Service_API_Rest\test.exe

        Getting authorization token...
        
        HTTP Status 200 : OK
        {
          "token_type": "Bearer",
          "expires_in": "3599",
          "expires_on": "1458085162",
          "not_before": "1458081262",
          "resource": "https://management.core.windows.net/",
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPW
        b3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFh
        NzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0cy53
        MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoiNzJm
        F5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE3OAE
        hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-HASI8
        }
        
        Got authorization token...
        Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNN
        aW5kb3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYt
        Zi1jNzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0
        OGU3MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoi
        iI-oF5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE
        vsf3hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-H
        
        Creating new campaign...
        
        HTTP Status 201 : Created
        {
          "id": 24,
          "state": "draft"
        }
        
        Created new campaign...
        New Campaign Id    : 24
        
        Activating new campaign (ID : 24)...
        
        HTTP Status 200 : OK
        {
          "id": 24,
          "state": "scheduled"
        }
        
        Activated new campaign...
        New Campaign State : scheduled
        
        Getting device IDs...
        
        HTTP Status 200 : OK
        {
          "value": [
            {
              "meta": {
                "lastSeen": 1458080534371,
                "firstSeen": 1458080534371,
                "lastLocation": 1458081389617,
                "lastInfo": 1458080571603
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "1d6208b8f281203ecb49431e2e5ce6b3"
            },
            {
              "meta": {
                "lastSeen": 1457990584698,
                "firstSeen": 1457949384025,
                "lastLocation": 1457990914895,
                "lastInfo": 1457990584597
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "302486644890e26045884ee5aa0619ec"
            }
          ]
        }
        
        Got devices.
        Adding device for push campaign...
        Device ID    : 1d6208b8f281203ecb49431e2e5ce6b3
        user_name    : Wesley
        Adding device for push campaign...
        Device ID    : 302486644890e26045884ee5aa0619ec
        user_name    : Wesley
        
        Triggering push for new campaign (ID : 24)...
        
        
        {"deviceIds":["1d6208b8f281203ecb49431e2e5ce6b3","302486644890e26045884ee5aa0619ec"]}
        
        HTTP Status 200 : OK
        {
          "invalidDeviceIds": []
        }
        
        Triggered push on new campaign.
        


<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
