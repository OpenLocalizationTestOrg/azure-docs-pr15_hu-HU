<properties
    pageTitle="Azure Active Directory jelentéskészítés naplózási API minták |} Microsoft Azure"
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
    ms.date="09/28/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-reporting-audit-api-samples"></a>Azure Active Directory naplózási API mintát jelentése

Ez a témakör az Azure Active Directory API jelentéskészítés súgótémakörök gyűjteménye része.  
Azure Active Directory jelentéskészítés biztosít egy API-val, amely lehetővé teszi a kódot vagy a kapcsolódó eszközök naplózási adatok eléréséhez.
Ez a témakör hatóköre megadására az **API naplózási**példakódot.

Lásd:

- További tájékoztatást a [naplókat](active-directory-reporting-azure-portal.md#audit-logs)

- [Az Azure Active Directory jelentéskészítés API – első lépések](active-directory-reporting-api-getting-started.md) a jelentéskészítési API kapcsolatban további tudnivalókat.

A kérdésekre, problémák vagy visszajelzést, kérjük, forduljon [AAD: segítséget](mailto:aadreportinghelp@microsoft.com).


## <a name="prerequisites"></a>Előfeltételek
A minták ebben a témakörben használata előtt kell fejezze be a [Előfeltételek API jelentéskészítés az Azure AD eléréséhez](active-directory-reporting-api-prerequisites.md).  
  

## <a name="known-issue"></a>Ismert problémák

Alkalmazás Auth nem fog működni, ha a bérlő az Európai Unió régióban. Felhasználói Auth használható szeretne hozzáférni a naplózási API megoldásként mindaddig, amíg a probléma megoldása azt. 


## <a name="powershell-script"></a>PowerShell-parancsprogramot
    # This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

    # Constants
    $ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
    $ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
    $loginURL       = "https://login.microsoftonline.com"     # AAD Instance; for example https://login.microsoftonline.com
    $tenantdomain   = "your-tenant-domain.onmicrosoft.com"    # AAD Tenant; for example, contoso.onmicrosoft.com
    $resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
    $7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
    Write-Output "Searching for events starting $7daysago"

    # Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    # Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
    if ($oauth.access_token -ne $null) {   
        $i=0
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
        $url = 'https://graph.windows.net/' + $tenantdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago

        # loop through each query page (1 through n)
        Do{
            # display each event on the console window
            Write-Output "Fetching data using Uri: $url"
            $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
            foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
                Write-Output ($event | ConvertTo-Json)
            }
        
            # save the query page to an output file
            Write-Output "Save the output to a file audit$i.json"
            $myReport.Content | Out-File -FilePath audit$i.json -Force
            $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
            $i = $i+1
        } while($url -ne $null)
    } else {
        Write-Host "ERROR: No Access Token"
        }

    Write-Host "Press any key to continue ..."
    $x = $host.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown")


### <a name="executing-the-powershell-script"></a>Végrehajtása a PowerShell parancsfájl
Miután fejezze be a szerkesztését, a parancsfájlt, indítsa el, illetve ellenőrizze, hogy az ellenőrzés várható adatainak naplózza a jelentés adja vissza.

A parancsprogram kimeneti formátumban JSON a naplójelentés adja eredményül. Is létrehoz egy `audit.json` ugyanazt a kimeneti fájl. A jelentések és, illetve más megjegyzést, a kimeneti formátumban, amely a szükségtelen adatok visszaadására parancsfájl módosításával kísérletezhet.


## <a name="bash-script"></a>Bash parancsprogram

    #!/bin/bash

    # Author: Ken Hoff (kenhoff@microsoft.com)
    # Date: 2015.08.20
    # NOTE: This script requires jq (https://stedolan.github.io/jq/)

    CLIENT_ID="your-application-client-id-here"         # Should be a ~35 character string insert your info here
    CLIENT_SECRET="your-application-client-secret-here" # Should be a ~44 character string insert your info here
    LOGIN_URL="https://login.windows.net"
    TENANT_DOMAIN="your-directory-name-here.onmicrosoft.com"    # For example, contoso.onmicrosoft.com

    TOKEN_INFO=$(curl -s --data-urlencode "grant_type=client_credentials" --data-urlencode "client_id=$CLIENT_ID" --data-urlencode "client_secret=$CLIENT_SECRET" "$LOGIN_URL/$TENANT_DOMAIN/oauth2/token?api-version=1.0")

    TOKEN_TYPE=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.token_type')
    ACCESS_TOKEN=$(echo $TOKEN_INFO | ./jq-win64.exe -r '.access_token')

    # get yesterday's date

    YESTERDAY=$(date --date='1 day ago' +'%Y-%m-%d')

    URL="https://graph.windows.net/$TENANT_DOMAIN/activities/audit?api-version=beta&$filter=eventTime%20gt%20$YESTERDAY"


    REPORT=$(curl -s --header "Authorization: $TOKEN_TYPE $ACCESS_TOKEN" $URL)

    echo $REPORT | ./jq-win64.exe -r '.value' | ./jq-win64.exe -r ".[]"

## <a name="python-script"></a>Python parancsfájl

    # Author: Michael McLaughlin (michmcla@microsoft.com)
    # Date: January 20, 2016
    # This requires the Python Requests module: http://docs.python-requests.org

    import requests
    import datetime
    import sys

    client_id = 'your-application-client-id-here'
    client_secret = 'your-application-client-secret-here'
    login_url = 'https://login.windows.net/'
    tenant_domain = 'your-directory-name-here.onmicrosoft.com'

    # Get an OAuth access token
    bodyvals = {'client_id': client_id,
                'client_secret': client_secret,
                'grant_type': 'client_credentials'}

    request_url = login_url + tenant_domain + '/oauth2/token?api-version=1.0'
    token_response = requests.post(request_url, data=bodyvals)

    access_token = token_response.json().get('access_token')
    token_type = token_response.json().get('token_type')

    if access_token is None or token_type is None:
        print "ERROR: Couldn't get access token"
        sys.exit(1)

    # Use the access token to make the API request
    yesterday = datetime.date.strftime(datetime.date.today() - datetime.timedelta(days=1), '%Y-%m-%d')

    header_params = {'Authorization': token_type + ' ' + access_token}
    request_string = 'https://graph.windows.net/' + tenant_domain + 'activities/audit?api-version=beta&$filter=eventTime%20gt%20' + yesterday   
    response = requests.get(request_string, headers = header_params)

    if response.status_code is 200:
        print response.content
    else:
        print 'ERROR: API request failed'





## <a name="next-steps"></a>Következő lépések

- Szeretné, ha testre szeretné szabni a minták ebben a témakörben? Nézze meg az [Azure Active Directory naplózási API hivatkozást](active-directory-reporting-api-audit-reference.md). 

- Egy teljes áttekintésben használatával a jelentéskészítési API Azure Active Directory, című témakörben olvashat [az Azure Active Directory – első lépések jelentéskészítés API-val](active-directory-reporting-api-getting-started.md).

- Ha szeretne többet megtudni az Azure Active Directory jelentéskészítés, című az [Azure Active Directory jelentéskészítés útmutató](active-directory-reporting-guide.md).  
