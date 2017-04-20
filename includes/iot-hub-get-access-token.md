## <a name="obtain-an-azure-resource-manager-token"></a>Egy erőforrás-kezelő Azure jogkivonat beszerzése

Borzas Active Directory a erőforrások Azure erőforrás-kezelővel elvégezhető feladatokat hitelesítenie kell. Az itt bemutatott példában jelszó-hitelesítést használja, más megközelítések lásd a [kérések hitelesítéséhez Azure erőforrás-kezelő][lnk-authenticate-arm].

1. Azure AD a alkalmazás azonosító és jelszó segítségével egy token lekérdezni a Program.cs a **fő** módszer adja hozzá a következő kódot.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. A token a következő kód hozzáadásával végére a **Main** metódust használó **ResourceManagementClient** objektum létrehozása:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Hozzon létre, vagy szerezzen be hivatkozni kell az erőforrás csoport használata:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx