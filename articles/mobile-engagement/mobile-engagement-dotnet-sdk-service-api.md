<properties 
    pageTitle="Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz elérése a .NET SDK segítségével" 
    description="Ismerteti, hogyan lehet a Mobile tetszés szerint elmélyedhet .NET SDK Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz eléréséhez használt"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Azure Mobile tetszés szerint elmélyedhet szolgáltatás API-khoz elérése a .NET SDK segítségével

Azure Mobile tetszés szerint elmélyedhet közzététele egy sor olyan API-hoz való eszközkezelés, vannak/leküldéses kampányok stb. Együttműködhet az alábbi API-hoz, azt is nyújt, hogy segítségével eszközökkel SDK készítése a preferált nyelvet [Swagger fájl](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) . Azt javasoljuk, hogy a SDK készítése saját Swagger-fájlból a [AutoRest](https://github.com/Azure/AutoRest) eszközzel. 

A .NET SDK van létrehozott egy hasonló módon, amely lehetővé teszi, hogy ezek API-k használata a C# csomagolópapír, és a hitelesítési jogkivonat egyeztetés és frissítése a saját maga nincs.  

Ez a példa a .NET SDK használatához kövesse a lépéseket a készlete keresztül módon történik:

1. Első lépésként meg kell az API-k használata az Azure Active Directory leírt [Itt](mobile-engagement-api-authentication.md#authentication)hitelesítés beállítása. Ezeket a lépéseket végén rendelkeznie kell a egy érvényes **SubscriptionId**, **TenantId**, **ApplicationId** és **titkos**. 

2. Egy egyszerű Windows konzol alkalmazás és a forgatókönyv egy bejelentése marketingkampány létrehozása a .NET SDK használata keresztül mutatjuk fogjuk használni. Így nyissa meg a Visual Studio és **Console-alkalmazás**létrehozása.   

3. Ezután le kell töltenie a .NET SDK csomagjában talál, amelyek **A Microsoft Azure tetszés szerint elmélyedhet könyvtár** Nuget gyűjteményben lévő elérhető az [alábbi](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
A Visual Studio telepíti a Nuget, ha kell, hogy biztosan keresésekor a csomag **előzetes belefoglalása** lehetőséget jelöli be:

    ![][1]

4. Az a `Program.cs` fájlt, és adja meg a következő névtér:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Ezután meg kell határoznia az alábbi állandók, amely fogjuk használni a hitelesítési és a bejelentése marketingkampány létrehozásához tetszés szerint elmélyedhet mobilalkalmazás használata:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Határozza meg a `EngagementManagementClient` változó, amelynek hívja fel a Mobile tetszés szerint elmélyedhet SDK módszerek fogjuk használni:

        static EngagementManagementClient engagementClient; 

7. Az alábbi módon adja hozzá a `Main` módszer:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. A következő módszerrel, amely inicializálása gondoskodik meghatározása a `EngagementManagementClient` először hitelesítése és majd magát társítása tetszés szerint elmélyedhet mobilalkalmazás, amelyben azt tervezi, hogy a bejelentése marketingkampány létrehozása:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Fontos tudni, hogy Ön használja az **Alkalmazás az erőforrás neve** , az Azure felügyeleti portálon a alkalmazásnév paraméterhez megadott. 

9. Végezetül meghatározása a CreateCampaign módszer, amely a korábban inicializált EngagementClient használó hozzon létre egy egyszerű **bármikor**fog gondoskodik & **csak értesítés** marketingkampány egy címet és egy üzenet: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Futtassa az konzol alkalmazást, és a sikeres létrehozását a marketingkampány jelenik meg a következő:

    **Marketingkampány azonosító "159" sikeres készült, és a "tervezet" állapot**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
