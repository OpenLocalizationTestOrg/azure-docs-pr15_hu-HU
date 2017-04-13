<properties
    pageTitle="Kezelése a PowerShell Azure CDN |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure PowerShell-parancsmagok használata Azure CDN kezeléséhez."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>A PowerShell Azure CDN kezelése

PowerShell biztosít a legtöbb rugalmas módszereket, amelyekkel az Azure CDN profilok kezelése és végpontok közül.  Használhatja a PowerShell interaktív, illetve a parancsfájlok írása adatkezelési feladatok automatizálása.  Ebben az oktatóanyagban bemutat néhány a leggyakoribb feladatok a PowerShell használatá az Azure CDN profilok kezelése és végpontok elvégezhető.

## <a name="prerequisites"></a>Előfeltételek

A Azure CDN-profilok és a végpontok kezelése a PowerShell használatával, a Azure PowerShell-modult telepítve kell rendelkeznie.  Megtudhatja, hogy miként Azure PowerShell telepítése és csatlakoztatása Azure használata a `Login-AzureRmAccount` parancsmag, megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Be kell jelentkeznie a `Login-AzureRmAccount` Azure PowerShell-parancsmagok végrehajtás előtt.

## <a name="listing-the-azure-cdn-cmdlets"></a>Az Azure CDN-parancsmagok listázása

Jeleníthet meg az összes az Azure CDN-parancsmagok használata a `Get-Command` parancsmag.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>A Súgó használata

A parancsmagok használatáról a fordulhat segítségért az `Get-Help` parancsmag.  `Get-Help`használatát és szintaxis biztosít, és tetszés szerint a példákat mutat.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Listaelem meglévő Azure CDN profilok

A `Get-AzureRmCdnProfile` parancsmag paraméter nélkül a meglévő CDN profilok ad vissza.

```powershell
Get-AzureRmCdnProfile
```

A kimenet átirányítható a felsorolás parancsmagjai.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

A profil nevét és az erőforrás csoport megadásával is egyetlen profil térhet vissza.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Akkor lehet, hogy az azonos nevű több CDN-profil mindaddig, amíg a különböző erőforráscsoport.  Név elhagyása a `ResourceGroupName` paraméter egyező nevű összes profilok adja vissza.

## <a name="listing-existing-cdn-endpoints"></a>Listaelem meglévő CDN-végpontok

`Get-AzureRmCdnEndpoint`egyes zárólap vagy egy profilt végpontjait meghallgathatja.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>CDN-profilok és végpont létrehozása

`New-AzureRmCdnProfile`és `New-AzureRmCdnEndpoint` CDN-profilok és a végpontok létrehozására.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Végpontjának neve elérhetőségének ellenőrzése

`Get-AzureRmCdnEndpointNameAvailability`egy objektumot, jelezve, hogy egy végpontjának neve elérhető adja vissza.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Egyéni tartomány hozzáadása

`New-AzureRmCdnCustomDomain`egyéni tartománynév hozzáadása meglévő zárólap.

>[AZURE.IMPORTANT] A DNS-szolgáltatónál [megfeleltetéséhez az egyéni tartomány végpontra (CDN-tartalom kézbesítési hálózati) módját](./cdn-map-content-to-custom-domain.md)ismertetett módon, be kell állítania a CNAME.  A hozzárendelés módosítása, a végpontok használata előtt tesztelheti `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Zárólap módosítása

`Set-AzureRmCdnEndpoint`meglévő zárólap módosítja.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Végleges törlés/előkamrás-loading CDN eszközök

`Unpublish-AzureRmCdnEndpointContent`pon gyorsítótárazott eszközök, miközben `Publish-AzureRmCdnEndpointContent` előre betölti támogatott végpontok eszközöket.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Indítása és leállítása CDN-végpontok
`Start-AzureRmCdnEndpoint`és `Stop-AzureRmCdnEndpoint` indítása és leállítása, egyes végpontokat vagy csoportok, a végpontokat is használható.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>CDN-erőforrások törlése

`Remove-AzureRmCdnProfile`és `Remove-AzureRmCdnEndpoint` is használható, ha el szeretné távolítani a profilok és a végpontok.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként Azure CDN [.NET](./cdn-app-dev-net.md) vagy [Node.js](./cdn-app-dev-node.md)automatizálása.

Az a CDN funkciókról, a [CDN áttekintése](./cdn-overview.md)című témakörben találhat.


