<properties
   pageTitle="Hozzon létre egy virtuális a piactér PowerShell beállítása |} Microsoft Azure"
   description="Való beállításáról az Azure PowerShell és egy nem kötelező folyamat segítségével flow hozhat létre virtuális képeket szeretne telepíteni, és kattintson a Microsoft Azure piactéren értékesítése"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Az ajánlat létrehozása az Azure piactér Azure PowerShell beállítása
Információ a, hogy miként állíthatja be a PowerShell Azure-ban megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Egy egyszerű megközelítés, a módszerhez a tanúsítvány, amely tölthető le, és importálja a tanúsítvány hitelesítés szükséges. A **Get-AzurePublishSettingsFile** parancsmag használatával a szükséges tanúsítvány beszerzéséhez. Amikor a rendszer kéri, mentse a fájlt. A tanúsítvány importálása egy PowerShell-munkamenetet, használja az **Importálás-AzurePublishSettingsFile** parancsmag.

Konfigurálni, és az általános beállítások a Microsoft Azure előfizetés tárolása a PowerShell-munkamenetet a **Set-AzureSubscription** , és **Válassza ki-AzureSubscription** parancsmagok használata:

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Az első parancs az előfizetést (szükséges az egyes virtuális kiépítési művelet) alapértelmezett tároló számla társít.  A második lehetővé teszi az előfizetést, az aktuálistól (más parancsmagok felismeri).

## <a name="see-also"></a>Lásd még:
- [Első lépések: a Microsoft Azure piactéren felajánlás közzététele](marketplace-publishing-getting-started.md)
- [A virtuális gép kép létrehozása a piactér](marketplace-publishing-vm-image-creation.md)
