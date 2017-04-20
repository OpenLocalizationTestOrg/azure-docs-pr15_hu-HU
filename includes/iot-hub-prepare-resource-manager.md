## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Erőforrás-kezelő Azure kérelmek hitelesítéséhez előkészítése

Az erőforrások az [Azure erőforrás-kezelő] segítségével végrehajtott műveletek hitelesítenie kell[ lnk-authenticate-arm] , Azure Active Directory (AD). Konfigurálni a legegyszerűbb módja, hogy használja a PowerShell vagy Azure CLI.

Telepítenie kell az [Azure PowerShell 1.0-s] [ lnk-powershell-install] vagy újabb, a folytatás előtt.

A következő lépések bemutatják, hogyan állítható be jelszó-hitelesítést használja a PowerShell AD alkalmazás. A parancsok egy szabványos PowerShell munkamenetben.

1. Jelentkezzen be az Azure előfizetését a következő parancsot:

    ```
    Login-AzureRmAccount
    ```

2. Jegyezze fel a **TenantId** és **SubscriptionId**. Szüksége lesz rájuk később.

3. Létrehozhat új Azure Active Directory alkalmazást a hely tulajdonosai cseréje, a következő parancsot:

    - **{Név}:** egy nevet, például a **MySampleApp** alkalmazás
    - **{Kezdőlap URL-címe}:** **http://mysampleapp/home**például az alkalmazás kezdőlapja URL-CÍMÉT. Az URL-cím mutasson egy valós alkalmazás nem szükséges.
    - **{Alkalmazás azonosítója}:** Például a **http://mysampleapp**egyedi azonosítója. Az URL-cím mutasson egy valós alkalmazás nem szükséges.
    - **{Jelszó}:** Egy jelszó, az alkalmazás hitelesítésére használni.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Jegyezze fel az **ApplicationId** az alkalmazás hozta létre. Ez később lesz szüksége.

5. Hozzon létre egy új egyszerű cseréje a **{MyApplicationId}** az **ApplicationId** az előző lépésben, a következő parancsot:

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. A telepítő a következő parancsot, **{MyApplicationId}** cseréje a **ApplicationId**szerepkör-hozzárendelés.

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Már most elkészítette az Azure AD alkalmazás, amely lehetővé teszi az egyéni C# alkalmazástól hitelesíteni. A tankönyv a később szüksége lesz a következő értékeket:

- TenantId
- SubscriptionId
- ApplicationId
- Jelszó

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
