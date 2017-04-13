<properties
    pageTitle="Erőforrás-kezelés köteg Management .NET fiók |} Microsoft Azure"
    description="Létrehozása, törlése és Azure köteg fiók erőforrásokat a köteg Management .NET-tárral módosítása."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/19/2016"
    ms.author="marsma"/>

# <a name="manage-azure-batch-accounts-and-quotas-with-batch-management-net"></a>Azure köteg fiókok és a köteg Management .NET kvóták kezelése

> [AZURE.SELECTOR]
- [Azure portál](batch-account-create-portal.md)
- [.NET köteg kezelése](batch-management-dotnet.md)

Csökkenthető karbantartási beleszámítva az Azure köteg alkalmazásokban a [Köteg Management .NET] használatával[ api_mgmt_net] automatizálhatja a köteg fiók létrehozása, törlése, kulcskezelő és kvóta feltárás tárat.

- **Létrehozása és törlése a köteg fiókok** bármely régión belüli. Ha egy független szoftverének a szállítójával (külső), például megad egy szolgáltatást, amelyben minden egyes van-e hozzárendelve egy külön köteg fiók számlázási célokra ügyfelei, fiók létrehozásakor vagy törlésekor funkciók vehet az ügyfél portál.
- **Lekérés és újragenerálása fiók billentyűk** programozott a köteg fiókok egyikével. Ez segíthet a hivatkozási periodikus átváltási vagy a fiók billentyűk lejártát biztonsági házirendek felel meg. Amikor több köteg fiókok különböző Azure régióban, átváltási folyamat automatizálást növeli a hatékonyság a megoldás.
- **Jelölje be a fiók kvóták** és meg kell tennie a próbaverzió hiba döntési feladatokat ki annak megállapítása, mely köteg fiókok van milyen korlátok. Jelölje be a fiók kvótákat feladatok megkezdése előtt, készletek létrehozása, és a számítási csomópontok felvétele, ezzel kapcsolatban beérkező beállíthatja, hogy hol, illetve ezek kiszámítania erőforrások létrejönnek. Meghatározhatja, hogy mely fiókok megkövetelése kvóta növeli az adott fiókok további erőforrások hozzárendelése előtt.
- **Kombinálása funkciók más Azure szolgáltatás** egy, a teljes szolgáltatáskészletet nyújtó adatkezelési folyamatok – köteg Management .NET, [Azure Active Directory]használatával[aad_about], és az [Erőforrás-kezelő Azure] [ resman_overview] együtt ugyanabban az alkalmazásban. Ezek a funkciók és az API-khoz használatával adhat frictionless hitelesítési változat, az azt jelenti, hogy az erőforrás-csoportok és a Funkciók, amelyek a fent ismertetett végpont kezelési megoldás létrehozása és törlése.

> [AZURE.NOTE] Ez a cikk a köteg fiókok kulcsok és kvóták programozott irányításának koncentrál, miközben végezheti el ezeket a tevékenységeket számos az [Azure portal]által[azure_portal]. További tudnivalókért lásd a [Azure köteg fiók létrehozása az Azure portál használatával](batch-account-create-portal.md) , és [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md)lehetőséget.

## <a name="create-and-delete-batch-accounts"></a>Hozzon létre és fiókok köteg törlése

Említett, a köteg Management API-elsődleges funkciók egyike létrehozása és törlése a köteg fiókok Azure területen. Ehhez használja a [BatchManagementClient.Account.CreateAsync] [ net_create] és [DeleteAsync][net_delete], vagy a szinkronizált mint.

A következő kódrészletet létrehoz egy fiókot, az újonnan létrehozott fiók beolvassa a köteg szolgáltatásból és majd törli. A kódtöredék és a többi ebben a cikkben `batchManagementClient` egy teljesen inicializált [BatchManagementClient]-példány[net_mgmt_client].

```csharp
// Create a new Batch account
await batchManagementClient.Account.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
AccountResource account = await batchManagementClient.Account.GetAsync(
    "MyResourceGroup",
    "mynewaccount");

// Delete the account
await batchManagementClient.Account.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] A köteg Management .NET tár és a BatchManagementClient osztály használó alkalmazások férnie **szolgáltatás rendszergazdája** vagy **coadministrator** az előfizetést, a tulajdonosa a köteg fiókot kell kezelni. További tudnivalókért lásd: az [Azure Active Directory](#azure-active-directory) szakasz, és a [AccountManagement] [ acct_mgmt_sample] kód minta.

## <a name="retrieve-and-regenerate-account-keys"></a>Lekérés és újragenerálása fiók kulcsok

Az elsődleges és másodlagos fiók billentyűk szerzi bármely köteg fiók belül az előfizetés [ListKeysAsync]használatával[net_list_keys]. Ezek a billentyűparancsok helyreállíthatók [RegenerateKeyAsync]használatával[net_regenerate_keys].

```csharp
// Get and print the primary and secondary keys
BatchAccountListKeyResult accountKeys =
    await batchManagementClient.Account.ListKeysAsync(
        "MyResourceGroup",
        "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.Primary);
Console.WriteLine("Secondary key: {0}", accountKeys.Secondary);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys =
    await batchManagementClient.Account.RegenerateKeyAsync(
        "MyResourceGroup",
        "mybatchaccount",
        new BatchAccountRegenerateKeyParameters() {
            KeyName = AccountKeyType.Primary
            });
```

> [AZURE.TIP] Az adatkezelési alkalmazások egyesíti kapcsolat munkafolyamat készíthet. Először szerezze be a fiókkulcs a köteg fiók [ListKeysAsync]kezelni kívánt[net_list_keys]. Ezután, használja a termékkulcsát a köteg .NET tár [BatchSharedKeyCredentials] inicializálásakor[ net_sharedkeycred] osztály [BatchClient]inicializálásakor használt[net_batch_client].

## <a name="check-azure-subscription-and-batch-account-quotas"></a>Azure előfizetést és a köteg fiók kvóták ellenőrzése

Azure előfizetések és az egyes Azure szolgáltatásokat, mint a köteg minden van alapértelmezett kvótákat, amelyek a bennük lévő egyes személyek korlátozása. Azure előfizetésekhez alapértelmezett kvóták olvassa el a [Azure előfizetés és szolgáltatás korlátozások, kvótákat, és korlátozások](./../azure-subscription-service-limits.md)című témakört. A köteg szolgáltatás alapértelmezett kvóták olvassa el a [kvótáinak és a köteg Azure szolgáltatás korlátozások](batch-quota-limit.md)című témakört. A köteg Management .NET tár használatával e kvóták ellenőrizheti az alkalmazásokban. Ez lehetővé teszi, hogy terhelés döntések fiókok felvétele vagy erőforrások, mint a készletek számítja ki, és csomópontok számítja ki.

### <a name="check-an-azure-subscription-for-batch-account-quotas"></a>Jelölje be a köteg fiók kvóták Azure-előfizetés

Előtt köteg fiók létrehozása területen, érdemes az Azure előfizetés kattintva megtekintheti, hogy tudunk fiók felvétele az adott régióban.

Az alábbi kódtöredéket az első használjuk [BatchManagementClient.Account.ListAsync] [ net_mgmt_listaccounts] az összes köteg számla belüli előfizetés gyűjtemény eléréséhez. Miután azt importáljon ebben a gyűjteményben, azt állapítsa meg, hány fiókhoz a cél régióban. Majd [BatchManagementClient.Subscriptions] használjuk[ net_mgmt_subscriptions] beszerzése a köteg fiók kvóta, és határozza meg, hány fiókhoz (ha van ilyen) az adott régióban létrehozhatók.

```csharp
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse =
        await batchManagementClient.Account.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}",
    creds.SubscriptionId,
    accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

A fenti, a kódtöredék `creds` [TokenCloudCredentials]egy példánya[azure_tokencreds]. Példa az objektum létrehozása című témakör tartalmazza a [AccountManagement] [ acct_mgmt_sample] GitHub minta kódot.

### <a name="check-a-batch-account-for-compute-resource-quotas"></a>A köteg számítási kiszolgálóerőforrás-kvótájának-fiók ellenőrzése

A köteg megoldás számítási erőforrásainak növekszik, mielőtt ellenőrizheti ahhoz, hogy a hozzárendelni kívánt erőforrások nem haladhatja meg a fiók kvóták. Az alábbi kódrészletet azt a köteg nevű fiókot kvóta adatait nyomtatása `mybatchaccount`. A saját alkalmazásban ezeket az információkat segítségével határozza meg, hogy a fiók kezelheti a további információforrásokat hozható létre.

```csharp
// First obtain the Batch account
BatchAccountGetResponse getResponse =
    await batchManagementClient.Account.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Azure előfizetések és a szolgáltatások alapértelmezett kvóták várakozó, ezek a korlátok számos is léptethető küld az [Azure portál][azure_portal]. Útmutatást a köteg fiók kvótákat növekvő például talál [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md) .

## <a name="batch-management-net-azure-ad-and-resource-manager"></a>Köteg Azure Active Directory, .NET, kezelése és erőforrás-kezelő

A köteg Management .NET tárban dolgozik, amikor a szokásos is használhat [Azure Active Directory] [ aad_about] (Azure Active Directory) és az [Erőforrás-kezelő Azure][resman_overview]. Az itt tárgyalt minta projekt Azure Active Directory és a az erőforrás-kezelő használja azt szemlélteti, hogy a köteg Management .NET API közben.

### <a name="azure-active-directory"></a>Azure Active Directory

Azure magát a hitelesítést az ügyfelek, a szolgáltatás-rendszergazdák és a szervezeti felhasználók Azure Active Directory használja. A köteg Management .NET környezetben használatával Azure AD egy előfizetés vagy közös rendszergazdája hitelesítést végezni. Ezzel a könyvtár a köteg szolgáltatás lekérdezés és a jelen cikkben ismertetett műveletek elvégzéséhez.

A minta projekt tárgyalt alatt, az Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) segítségével a felhasználótól Microsoft adataikat. Szolgáltatás rendszergazdája vagy coadministrator vannak hitelesítő adatok, amikor az alkalmazás Azure lekérdezés – előfizetések listáját és is létrehozhat és törölhet az erőforrás-csoportok és a köteg fiókok.

### <a name="resource-manager"></a>Erőforrás-kezelő

A köteg Management .NET-tárral köteg fiókok létrehozásakor a szokásos létrehozandó őket [erőforráscsoport]belül[resman_overview]. Az erőforráscsoport programozás útján hozhat létre, a [ResourceManagementClient] használatával[ resman_client] az [Erőforrás-kezelő .NET] osztály[ resman_api] tárat. Fiók hozzáadása az [Azure portal]segítségével a korábban létrehozott meglévő erőforráscsoport vagy[azure_portal].

## <a name="sample-project-on-github"></a>Minta projekt GitHub

Szeretné látni a köteg Management .NET működését, olvassa el a [AccountManagment] [ acct_mgmt_sample] GitHub minta projekt. A New jeleníti meg a létrehozás és [BatchManagementClient] használatát[ net_mgmt_client] és [ResourceManagementClient][resman_client]. Azt is bemutat a használatát az Azure [Active Directory Authentication Library] [ aad_adal] (ADAL), amelyek mindkét ügyfél által igényelt.

A minta alkalmazás futtatásához sikeresen, először regisztrálnia kell azt az Azure Active Directory az Azure portál használatával. Kövesse a [integrálása] alkalmazásokban és az Azure Active Directory [felvétele az alkalmazások](../active-directory/active-directory-integrating-applications.md#adding-an-application) szakaszban[ aad_integrate] a minta alkalmazás belül a saját fiók regisztrálása adatait a címtár alapértelmezett. Ügyeljen arra, jelölje be az alkalmazás típusát **Natív ügyfélalkalmazás** , és megadhatja, hogy bármely érvényes URI (például: `http://myaccountmanagementsample`) az **Átirányítás URI**– akkor nem kell lennie egy valós végpontot.

Miután megadta az alkalmazást, az alkalmazás beállításai a portálon a *Windows Azure szolgáltatás felügyeleti API* -alkalmazás **Hozzáférést Azure Szolgáltatáskezelés, a szervezet** jogosult delegálása:

![Szolgáltatásalkalmazás jogosultságainak az Azure-portálon][2]

> [AZURE.TIP] Az *engedélyek más alkalmazások*nem jelenik a **Windows Azure szolgáltatás felügyeleti API** , kattintson az **alkalmazás hozzáadása**parancsra, jelölje be a **Windows Azure szolgáltatás felügyeleti API**, majd kattintson a pipajeles gombra. Ezután meghatalmazotti engedélyek fent ismertetett módon.

Miután elhelyezte az alkalmazás fentebb ismertetett, frissítse `Program.cs` a [AccountManagment] a[ acct_mgmt_sample] minta project az alkalmazás URI átirányítás és az ügyfél-azonosítóval. Ezeket az értékeket az alkalmazás **beállítása** lapon találja meg:

![Alkalmazás-konfiguráció az Azure-portálon][3]

A [AccountManagment] [ acct_mgmt_sample] minta alkalmazás bemutatja az alábbi műveleteket:

1. A biztonsági jogkivonat az Azure Active Directory beszereznie [ADAL]használatával[aad_adal]. Ha a felhasználó már nem jelentkezteti be, azok megjelenik egy kérdés a Azure hitelesítő adatait.
2. A biztonsági jogkivonat Azure Active Directory nyert használatával hozzon létre egy [SubscriptionClient] [ resman_subclient] Query Azure annak a fióknak előfizetések listáját. Ezzel a kijelölést ki egy előfizetést, ha több találhatók.
3. A kijelölt előfizetéshez tartozó hitelesítő adatok-objektum létrehozása.
4. Hozzon létre [ResourceManagementClient] [ resman_client] a hitelesítő adataival.
5. Használja a [ResourceManagementClient] [ resman_client] erőforrás csoport létrehozásához.
6. Használja a [BatchManagementClient] [ net_mgmt_client] több köteg fiók műveleteket:
  - Az új erőforráscsoport egy köteg fiók létrehozása
  - Az újonnan létrehozott-fiók beszerzése a köteg szolgáltatásból.
  - Az új fiók fiók billentyűparancsok nyomtatása.
  - A fiók új elsődleges kulcsot újragenerálása.
  - Nyomtassa ki a kvóta adatait a fiókhoz.
  - Nyomtassa ki az előfizetés kvóta adatait.
  - Az előfizetés belül minden fiók nyomtatása.
  - Újonnan létrehozott fiókjának törlése
7. Törölje az erőforrás-csoportot.

Az újonnan létrehozott köteg fiók és az erőforrás csoport törlése, előtt megvizsgálhatja, mind az [Azure portál][azure_portal]:

![Az erőforráscsoport és a köteg fiók Azure portál][1]
<br />
*Új erőforráscsoport és a köteg fiók Azure portál*

[aad_about]: ../active-directory/active-directory-whatis.md "Mi az Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Az Azure Active Directory Authentication esetek"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Alkalmazások integrálása az Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspxs
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../azure-resource-manager/resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png
[2]: ./media/batch-management-dotnet/portal-02.png
[3]: ./media/batch-management-dotnet/portal-03.png
