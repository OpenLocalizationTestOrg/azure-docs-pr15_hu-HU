<properties
   pageTitle="A szükséges értékeket beszerzése hitelesítő kódot az SQL-adatbázis eléréséhez az alkalmazások |} Microsoft Azure"
   description="Hozzon létre egy egyszerű kódból SQL-adatbázis eléréséhez."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>A szükséges értékeket beszerzése hitelesítő kódot az SQL-adatbázis eléréséhez az alkalmazások

Hozhat létre és SQL-adatbázis kezelése az kódot, ahol az Azure erőforrások készült Azure Active Directory (AAD) tartományban az előfizetés regisztrálnia kell az alkalmazást.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Szolgáltatást hozhat létre egyszerű források az access-alkalmazásból

A legújabb a [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) telepítése és futtatása van szükség. Információ megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

Az alábbi PowerShell parancsprogramot hoz létre, az Active Directory (AD) alkalmazás és a szolgáltatás egyszerű, amely a C# alkalmazás hitelesítő szükség. A parancsprogram szükséges az előző C# minta értékeket exportálja. Részletes tudnivalókért lásd: [Azure PowerShell használatá hozzon létre egy egyszerű erőforrások eléréséhez szolgáltatást](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Lásd még:

- [SQL-adatbázis létrehozása C#](sql-database-get-started-csharp.md)
- [SQL-adatbázis csatlakoztatása az Azure Active Directory-hitelesítés használatával](sql-database-aad-authentication.md)


