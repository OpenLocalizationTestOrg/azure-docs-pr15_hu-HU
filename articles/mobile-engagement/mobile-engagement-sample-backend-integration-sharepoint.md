<properties 
    pageTitle="Azure mobil tetszés szerint elmélyedhet - kódmentes-integráció" 
    description="Csatlakozás Azure Mobile tetszés szerint elmélyedhet egy SharePoint-fájlok kampányok létrehozása a SharePoint-webhelyről" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure mobil tetszés szerint elmélyedhet - API-integráció

Az automatikus marketingtevékenység rendszerben létrehozásáról és a marketingkampányok aktiválása is automatikusan történik. Erre a célra - Azure Mobile tetszés szerint elmélyedhet lehetővé teszi, hogy ilyen API-khoz is használatával automatizált marketingkampányok létrehozása. 

A szokásos ügyfelek segítségével a Mobile tetszés szerint elmélyedhet előtérrészét felület hirdetmények/szavazásokra stb a marketingkampányok részeként. Azonban a marketingkampányok egyre elért, szükség van a használhatja az adatokat (például minden olyan CRM rendszerrel vagy CMS rendszer, például a SharePoint) kódmentes rendszerekben zárolva van, hogy a teljesen automatikus folyamat kampányok dinamikusan a kódmentes rendszerekből a folyó adatokon alapuló Mobile tetszés szerint elmélyedhet a létrehozó hozhatók létre. 

![][5]

Ebben az oktatóanyagban végig forgatókönyv, ahol a SharePoint üzleti felhasználó feltölti az értékesítési adatok SharePoint-lista, és az automatizált eljárással felveszi az elemet a listából, és a Mobile tetszés szerint elmélyedhet a rendelkezésre álló REST API-k használata a marketingkampányok létrehozása a SharePoint-adatokból rendszerrel csatlakozik. 
 
> [AZURE.IMPORTANT] Általánosságban elmondható használhatja a következő példában kiindulási pontként ismertetése a hívás bármely Mobile tetszés szerint elmélyedhet REST API-t, ahogy azt részletek, a két fő szempontjait az API - hitelesítő és tompított paraméterek hívásához. 

## <a name="sharepoint-integration"></a>SharePoint-integráció
1. Az alábbiakban a minta SharePoint-lista néz ki. Az értesítés létrehozása **név**, **kategória**, **NotificationTitle**, **üzenet** , és **URL-cím** segítségével. A minta automatizálási folyamat konzol program formájában által használt **IsProcessed** nevű oszlop van. Bármelyik run a konzol program az Azure WebJob, így akkor is ütemezze is, vagy közvetlenül a SharePoint-munkafolyamat létrehozása és aktiválása az értesítés, amikor a SharePoint-lista illeszteni a elem programhoz használható. Ez a példa a konzol program, amelyek megy keresztül a SharePoint-lista elemeinek és bejelentése létrehozása az Azure Mobile tetszés szerint elmélyedhet a minden egyes őket, és végül jelöli az igaz eredményt a közlemény sikeres létrehozását **IsProcessed** Üzenetjelölő fogjuk kiszámolni.

    ![][1]

2. Akkor használja a kódot a mintából *Távoli hitelesítési a SharePoint Online használata az ügyfél objektummodell* [Itt](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) hitelesíteni a SharePoint-lista.
 
3. Hitelesítését követően azt ismétlése a listaelemek megtudhatja, hogy minden újonnan létrehozott elemet (amely lesz **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobil tetszés szerint elmélyedhet-integráció
1.  Miután azt meg, hogy egy elemet, amelyhez feldolgozása - azt bontsa ki a tartalmat a listaelemek és a hívás létrehozásához szükséges információk `CreateAzMECampaign` szeretné létrehozni a dokumentumot, és ezt követően `ActivateAzMECampaign` aktiválni. Ezek a Mobile tetszés szerint elmélyedhet kódmentes ügyében telefonáló lényegében REST API hívásokat. 

2.  A mobil tetszés szerint elmélyedhet REST API-k csak egy **egyszerű auth séma engedélyezési HTTP fejléc** , amely tevődik össze a `ApplicationId` és a `ApiKey` , tagságából az Azure-portálra. Győződjön meg arról, hogy használni a kulcs **api billentyűk** szakaszából, és *nem* a **sdk billentyűk** szakaszából. 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Az értesítés létrehozásához írja be a marketingkampány - [dokumentációt](https://msdn.microsoft.com/library/azure/mt683750.aspx). Győződjön meg arról, hogy a marketingkampány ad meg kell `kind` *bejelentése* és a [tartalom](https://msdn.microsoft.com/library/azure/mt683751.aspx) és beengedné FormUrlEncodedContent szerint. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Az értesítés létrehozása után látni fogja az alábbiakhoz hasonlóan egy üzenetet a Mobile tetszés szerint elmélyedhet portálon (vegye figyelembe, hogy az állapot = Piszkozat és aktiválva = #hiányzik)

    ![][3]

5. `CreateAzMECampaign`létrehoz egy bejelentése marketingkampány, és a hívó azonosítója adja vissza. `ActivateAzMECampaign`az azonosító szükséges aktiválása a marketingkampány paraméterként. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Az értesítés aktiválása után jelennek meg az alábbiakhoz hasonlóan egy üzenetet a Mobile tetszés szerint elmélyedhet portálon:

    ![][4]

7. Amint a marketingkampány aktiválva kap, az eszközöket, amelyek megfelelnek a marketingkampány a feltétel elkezdenek értesítés jelenik meg. 

8. Is látni fogja, hogy a listaelem jelölt IsProcessed = false korábban beállított igaz, a közlemény marketingkampány létrehozása után.  

Ez a példa egy főleg a szükséges tulajdonságokat tartalmazó egyszerű bejelentése marketingkampány létrehozása. Testre szabhatja a, mint amennyit a portálon található információk segítségével is [Itt](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
