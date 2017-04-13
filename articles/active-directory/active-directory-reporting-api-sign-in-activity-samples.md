<properties
    pageTitle="Azure Active Directory bejelentkezési tevékenység jelentés API minták |} Microsoft Azure"
    description="Hogyan veheti használatba az Azure Active Directory jelentéskészítés API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory bejelentkezési tevékenység jelentés API minták

Ez a témakör az Azure Active Directory API jelentéskészítés súgótémakörök gyűjteménye része.  
Azure Active Directory jelentéskészítés biztosít az API-val, amely lehetővé teszi a kódot vagy a kapcsolódó eszközök tevékenység bejelentkezési adatok eléréséhez.  
Ez a témakör hatóköre megadására a **bejelentkezési tevékenység API**példakódot.

Lásd:

- További tájékoztatást a [naplókat](active-directory-reporting-azure-portal.md#audit-logs)
- [Az Azure Active Directory jelentéskészítés API – első lépések](active-directory-reporting-api-getting-started.md) a jelentéskészítési API kapcsolatban további tudnivalókat.

A kérdésekre, problémák vagy visszajelzést, kérjük, forduljon [AAD: segítséget](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Előfeltételek
A minták ebben a témakörben használata előtt kell fejezze be a [Előfeltételek API jelentéskészítés az Azure AD eléréséhez](active-directory-reporting-api-prerequisites.md).  


##<a name="powershell-script"></a>PowerShell-parancsprogramot

    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.windows.net/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"
    
    $i=0
    
    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {
    
        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Az parancsfájl
Miután fejezze be a szerkesztését, a parancsfájlt, indítsa el, illetve ellenőrizze, hogy az ellenőrzés várható adatainak naplózza a jelentés adja vissza.

A parancsprogram kimeneti formátumban JSON bejelentkezési jelentés adja eredményül. Is létrehoz egy `SigninActivities.json` ugyanazt a kimeneti fájl. A jelentések és, illetve más megjegyzést, a kimeneti formátumban, amely a szükségtelen adatok visszaadására parancsfájl módosításával kísérletezhet.



## <a name="next-steps"></a>Következő lépések

- Szeretné, ha testre szeretné szabni a minták ebben a témakörben? Nézze meg az [Azure Active Directory bejelentkezési tevékenység API hivatkozást](active-directory-reporting-api-sign-in-activity-reference.md). 

- Egy teljes áttekintésben használatával a jelentéskészítési API Azure Active Directory, című témakörben olvashat [az Azure Active Directory – első lépések jelentéskészítés API-val](active-directory-reporting-api-getting-started.md).

- Ha szeretne többet megtudni az Azure Active Directory jelentéskészítés, című az [Azure Active Directory jelentéskészítés útmutató](active-directory-reporting-guide.md).  