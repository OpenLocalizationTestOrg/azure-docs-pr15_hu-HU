<properties
    pageTitle="A .NET rendszerhez Ismerkedés az Azure CDN-tárral |} Microsoft Azure"
    description="Megtudhatja, hogy miként írhat a .NET-alkalmazások kezeléséhez Azure CDN Visual Studio segítségével."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure CDN fejlesztési – első lépések

> [AZURE.SELECTOR]
- [NODE.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

Az [Azure CDN-tárban a .NET rendszerhez](https://msdn.microsoft.com/library/mt657769.aspx) automatizálhatja a létrehozásának és kezelésének CDN-profilok és a végpontokat is használhatja.  Az alábbiakban ebben az oktatóprogramban egy egyszerű .NET console-alkalmazást, amely bemutatja az elérhető műveletek számos létrehozását.  Ebben az oktatóanyagban nem írja le az Azure CDN-tár részletekbe menően részletesen a .NET rendszerhez készült.

Visual Studio 2015 oktatóprogram elvégzéséhez szükséges.  [Visual Studio közösségi 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) szabadon letölthető.

> [AZURE.TIP] [Ebben az oktatóanyagban a projekt befejezését](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) MSDN letöltésre érhető el.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>A projekt létrehozása és hozzáadása a Nuget csomagok

Most, hogy már létrehozott erőforráscsoport az a CDN-profil, és engedélyt az Azure Active Directory alkalmazás CDN profilok kezelése és végpontok csoporton belüli, hogy elkezdheti az alkalmazás létrehozása.

A Visual Studio 2015, kattintson az **fájl**és az **Új** **projekt...** az új projekt párbeszédpanel megnyitása gombra.  Bontsa ki a **képi C#**, majd a bal oldali ablaktáblán jelölje be a **Windows** .  A középső ablaktáblában kattintson a **New** .  A projekt neve, majd kattintson az **OK gombra**.  

![Új projekt](./media/cdn-app-dev-net/cdn-new-project.png)

A projekt fog néhány Azure tárakkal Nuget csomagokban található.  Helyezzen el azokat a projekthez.

1. Kattintson az **eszközök** menü, a **Nuget csomag Manager**, majd a **Csomag kezelője konzol**.

    ![Nuget csomagok kezelése](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. A csomag Manager konzolban hajtsa végre a következő parancsot, telepítse az **Active Directory Authentication tár (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Hajtsa végre az alábbi módon telepítse az **Azure CDN könyvtár**:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Irányelvek, állandók, fő módszer és segítő módszerek

A program írt alapszerkezetét pedig hozzuk működésbe.

1. Vissza a Program.cs lap cserélje le a `using` tetején, a következő irányelvek:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Néhány a módszerhez állandók definiálása szükség.  Az a `Program` osztályban, de mielőtt a `Main` módot, adja hozzá a következő.  A helyőrzők, beleértve a helyére ne felejtse el a ** &lt;csúcsos zárójelek&gt;**, szükség szerint a saját értékű.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Az osztály szintre, de is meghatározhat e két változó.  Használjuk ezek később határozza meg, ha a saját profil és a végpont már létezik.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Cserélje le a `Main` módszer az alábbiak szerint:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Más módszerek a részét állásáról, azaz a felhasználótól, "Igen/nem" kérdésekkel.  Adja hozzá a következő módszerrel könnyebb, amely kissé:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Most, hogy a program egyszerű felépítésének íródott, hozzunk létre kell a módszereket, hívja a `Main` módot.

## <a name="authentication"></a>Hitelesítés

Ahhoz, hogy használható az Azure CDN könyvtár, szükség hitelesíteni a szolgáltatás egyszerű és egy hitelesítési jogkivonat beszerzése.  Ez a módszer ADAL használja a token beolvasásához.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Ha egyéni felhasználói hitelesítés, használja a `GetAccessToken` módszer némileg eltér.

>[AZURE.IMPORTANT] Ha használja a kódot a következő példában, hogy az egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Helyére ne felejtse el `<redirect URI>` az átirányítást az alkalmazás az Azure Active Directory regisztrálásakor megadott URI együtt.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN profilok és a végpontok

Most, hogy készen áll a CDN-műveletek hajthatók végre.  A legfontosabb dolog a módszer jelent listában a profilok és saját erőforráscsoport lévő, és ha a profil- és végpont nevek, az állandók, a megadott egyezést lehetővé teszi, amely a Megjegyzés későbbre, azt nem próbál létrehozni az ismétlődő elemeket.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN-profilok és végpont létrehozása

Ezután azt fogja hozzon létre egy profilt.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

A profil létrehozása után azt zárólap létre fogja hozni.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] A fenti példa a végpont rendel egy *kontraktor* nevű egy hostname (állomásnév) origin `www.contoso.com`.  Módosítania kell a mutasson a saját origin hostname (állomásnév).

## <a name="purge-an-endpoint"></a>Zárólap törlése

Feltételezve, hogy létrejött a végpontot, egy közös feladatot, célszerű lehet végrehajtani a program a végpontot tartalmának van törlése.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] A karakterlánc a fenti példában a `/*` azt jelzi, hogy szeretném véglegesen, amit a legfelső szintű végpont elérési.  Ez az egyenértékű a ellenőrzése **Az összes törlése** az Azure portál "véglegesen" párbeszédpanel. Az a `CreateCdnProfile` módszer, én hoztam létre saját profil kód használatával **Verizon az Azure CDN** profilként `Sku = new Sku(SkuName.StandardVerizon)`, így a sikeres lesz.  Azonban **Akamai az Azure CDN** -profilok nem támogatja az **Összes törlése**, hogy volt-Akamai profil használatának ebben az oktatóanyagban, ha szeretné kell törlése adott elérési útját.

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN-profilok és a végpontok törlése

Az utolsó módszerek végpont és a profilban is törli.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>A program futtatása

Azt most állíthat össze, és kattintson a **Start** gombra, a Visual Studióban futtatta.

![Program fut](./media/cdn-app-dev-net/cdn-program-running-1.png)

Amikor a program eléri a fenti kérdésre, láthatja, térjen vissza az erőforráscsoport az Azure-portálon, és látható, hogy a profil létrehozása.

![A siker!](./media/cdn-app-dev-net/cdn-success.png)

Azt is erősítse meg a többi program futtatásához a képernyőn megjelenő utasításokat.

![Program befejezése](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Következő lépések

Ez a forgatókönyv, [Töltse le a minta](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c)a befejezett projekt megjelenítése.

A .NET rendszerhez az Azure CDN könyvtár további dokumentációja megkereséséhez megtekintése az [MSDN hivatkozást](https://msdn.microsoft.com/library/mt657769.aspx).

Kezelése a [PowerShell](./cdn-manage-powershell.md)CDN erőforrásait.


