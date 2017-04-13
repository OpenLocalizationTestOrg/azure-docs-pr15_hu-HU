<properties
   pageTitle="Azure RemoteApp PowerShell-parancsmagok használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként használhatja a Windows PowerShell-parancsmagok az Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>A Windows PowerShell-parancsmagok használata Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

 Azure RemoteApp PowerShell-parancsmagok felügyelete és a webhelycsoportok kezelése is használhatja. Első lépések használja az alábbi információkat.

## <a name="get-the-cmdlets"></a>A parancsmagok beszerzése 
-------------
Először töltse le az Azure Powershell-parancsmagok [Itt](http://go.microsoft.com/?linkid=9811175), a benne szereplő parancsmagok RemoteApp. 

Nézze meg az [Azure RemoteApp parancsmag segítségével](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Azure-parancsmagok használata az előfizetés beállítása
------------------
Kövesse az [Ez az útmutató](../powershell-install-configure.md) , így használhatja a parancsmagok az Azure előfizetés szemben.

Ismerkedés a frissítést használhatja ezeket a lépéseket:

1.  Töltse le és telepítse az [Azure PowerShell-parancsmagok](http://go.microsoft.com/?linkid=9811175).
2.  Indítsa el a Microsoft Azure PowerShell.
3.  Futtassa a **Hozzáadás-AzureAccount** Azure-előfizetéséhez hitelesítést végezni. Amikor a rendszer kéri, adja meg a használt felhasználónevet és jelszót, jelentkezzen be az Azure-portálra.  
4.  Futtassa a **Get-AzureSubscription** a felhasználói fiókját társított előfizetések listáját. 
5.  Futtassa a **Select-AzureSubscription** , és adja meg, az előfizetés nevét vagy a PowerShell konzolban használni kívánt Azonosítót.

Gratulálunk, az Azure PowerShell konzol beállítva, és használatra kész. Ügyeljen arra, hogy meg kell 2 – 5 repeate lépéseket minden indításakor: Ha a az Azure PowerShell konzolt.  

## <a name="create-a-cloud-collection"></a>Hozzon létre egy felhőalapú gyűjteményt
--------------------
Érdemes egyszerű, futtassa a következő parancsot:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

A fenti parancs automatikusan teszi közzé a Microsoft Office 365-alkalmazások (az Excel, OneNote, Outlook, PowerPoint, a Visio és Word).

Webhelycsoport létrehozása a 30 percig is eltarthat vagy hosszú befejezéséhez. Ez a parancs emiatt a nyomon követés Azonosítóját, amely a következőképpen használhatja adja eredményül:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Miután végzett a gyűjteményben, hozzáadhat a felhasználókat a gyűjteménybe az alábbi paranccsal:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

És ezzel elkészült! Kell, hogy a felhasználó számára tud csatlakozni az alkalmazást az Azure RemoteApp ügyfél található [Itt](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Rendelkezésre álló parancsmagok
Van még más parancsok rengeteg, dokumentációjában találhat őket a rendszer hamarosan lesz:

Egyszerű RemoteApp webhelycsoport-parancsmagok: 

- Új AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Set-AzureRemoteAppCollection
- Frissítés-AzureRemoteAppCollection
- Eltávolítás-AzureRemoteAppCollection
- AzureRemoteAppUser hozzáadása
- Get-AzureRemoteAppUser
- Eltávolítás-AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Kapcsolat bontása AzureRemoteAppSession
- Meghívása AzureRemoteAppSessionLogoff
- Küldés-AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Közzététel AzureRemoteAppProgram
- AzureRemoteAppProgram közzétételének megszüntetése
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Távoli alkalmazás formájában használva virtuális hálózati parancsmagok:

- Új AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Set-AzureRemoteAppVNet
- Eltávolítás-AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get – AzureRemoteAppVpnDeviceConfigScript
- Alaphelyzetbe-AzureRemoteAppVpnSharedKey

A parancsmagok RemoteApp sablon képe:

- Új AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Átnevezés-AzureRemoteAppTemplateImage
- Eltávolítás-AzureRemoteAppTemplateImage

Más RemoteApp parancsmagok:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Set-AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
