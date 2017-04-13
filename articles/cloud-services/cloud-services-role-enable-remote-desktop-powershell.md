<properties
pageTitle="A szerepkör a PowerShell használatá Azure Cloud Services engedélyezése a távoli asztali kapcsolat"
description="A PowerShell használatá engedélyezése a távoli asztali kapcsolat azure felhő szolgáltatásalkalmazás konfigurálása"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>A szerepkör a PowerShell használatá Azure Cloud Services engedélyezése a távoli asztali kapcsolat

>[AZURE.SELECTOR]
- [Azure klasszikus portál](cloud-services-role-enable-remote-desktop.md)
- [A PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Távoli asztali lehetővé teszi, hogy szerepkörbe Azure-ban futó asztali érhető el. Távoli asztali kapcsolat használatával kapcsolatos hibák elhárítása és az alkalmazás problémáinak diagnosztizálása, futása közben is.

Ez a cikk ismerteti, hogyan engedélyezése a távoli asztali a felhőalapú szolgáltatás szerepkörök PowerShell használatával. Megtudhatja, [hogy miként telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) a szükséges ez a cikk a előfeltételeihez. A PowerShell a távoli asztali bővítmény használja, így engedélyezheti a távoli asztali után az alkalmazás telepítve van.


## <a name="configure-remote-desktop-from-powershell"></a>Állítsa be a PowerShell távoli asztali

A [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) parancsmag lehetővé teszi, hogy engedélyezze a távoli asztali megadott szerepkörök vagy a felhőalapú szolgáltatás üzembe az összes szerepkörének. A parancsmaggal lehetővé teszi a felhasználónév és jelszó megadása a távoli asztali felhasználó fogadja el a PSCredential objektum *hitelesítő adatok* paraméter keresztül.

A PowerShell interaktív módon használja, ha egyszerűen beállíthatja, hogy a PSCredential objektum hívja fel a [Get-hitelesítő adatok](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.

```
$remoteusercredentials = Get-Credential
```

Ez a parancs a párbeszédpanel lehetővé teszi, hogy adja meg a felhasználónevét és jelszavát a távoli felhasználó biztonságos módon jeleníti meg.

A PowerShell automatizálást segít, mivel is beállíthatja az **PSCredential** objektumot oly módon, hogy használatához nincs szükség a felhasználói beavatkozás. Első lépésként meg kell a biztonságos jelszó beállítása. Adja meg a jelszót egyszerű szöveges [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)használata biztonságos karakterlánccá alakít kell kezdeni. Ezután kell a biztonságos karakterlánc átalakítása titkosított szabványos karakterláncra [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)használatával. Most már a titkosított szabványos karakterlánc mentheti [Set-tartalom](https://technet.microsoft.com/library/ee176959.aspx)fájlt.

A biztonságos jelszó-fájl is hozhat létre, így nem kell minden alkalommal írja be a jelszót. A biztonságos jelszó-fájl is hatékonyabb, mint egy egyszerű szövegfájl. Az alábbi PowerShell-lel való biztonságos jelszó-fájl létrehozása:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Ha állít be a jelszót, ellenőrizze, hogy teljesülnek-e a [összetettsége követelményeknek](https://technet.microsoft.com/library/cc786468.aspx).

A hitelesítőadat-objektum létrehozása a biztonságos jelszó-fájlból, olvassa el a fájl tartalmát, és alakíthatja vissza őket [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)használata biztonságos karakterlánc.

A [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) parancsmag is fogadja el a *jelszólejárati* paraméter adja meg a **dátum és idő** , a felhasználói fiók jár le. Például akkor állítsa a fiókot, az aktuális dátum és idő néhány napok lejár.

A PowerShell példa bemutatja, hogyan a távoli asztali bővítmény állíthat be egy felhőalapú szolgáltatásba:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
A telepítési tárolóhely és szerepkörök engedélyezése a távoli asztali a kívánt is tetszés szerint adja meg. Ha a paraméter nincs megadva, a parancsmagot lehetővé teszi, hogy a **gyártási** telepítési tárolóhely az összes szerepkörének távoli asztali.

A távoli asztali bővítmény társítva a telepítést. Ha hoz létre egy új telepítésének a szolgáltatást, akkor engedélyezése a távoli asztali adott példányban. Ha mindig távoli asztali engedélyezve van, majd vegye figyelembe PowerShell-parancsfájlokat integrálása a telepítési munkafolyamat.


## <a name="remote-desktop-into-a-role-instance"></a>Távoli asztali be egy szerepkör-példány
A [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag használatával távoli asztali egy szerepkör példányát a felhőalapú szolgáltatás be. A *LocalPath* paraméter használatával letöltése helyi meghajtóra az RDP-fájlt. Vagy a *indítása* paraméter segítségével közvetlenül indítsa el a távoli asztali kapcsolat párbeszédpanel elérheti a felhőalapú szolgáltatás szerepkör példánya.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Ha a távoli asztali bővítmény engedélyezve van a szolgáltatás ellenőrzése
A [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) parancsmag jeleníti meg, hogy a távoli asztal engedélyezve van-e meg a szolgáltatás telepítési. A parancsmaggal a távoli asztali felhasználó és a szerepkörök, hogy a távoli asztali bővítmény engedélyezve van a felhasználónév adja eredményül. Alapértelmezés szerint ez történik, kattintson a telepítés tárolóhely, és azt is beállíthatja, használja helyette az átmeneti tárolásra szolgáló tárolóhely.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Távoli asztal kiterjesztése megszüntetése
Ha már engedélyezve van egy példányban távoli asztali bővítmény, és a távoli asztali beállítások frissítenie kell, először távolítsa el a bővítmény. És kapcsolhatja be újra az új beállításokkal. Ha például azt szeretné, hogy a távoli felhasználói fiókot az új jelszót állíthat be, vagy a fiók lejárt. Ezzel a meglévő környezetek a távoli asztali bővítmény engedélyezve van szükség. Új telepítésekhez egyszerűen alkalmazhatja a bővítmény közvetlenül.

A távoli asztali bővítmény eltávolítása a telepítést, az [Eltávolítás-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) parancsmag is használhatja. A telepítési tárolóhely és szerepkör, amelyből el szeretné távolítani a távoli asztali bővítmény is tetszés szerint adja meg.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Távolítsa el teljesen az bővítmény konfiguráció, hívható meg a *eltávolítása* parancsmagot a **UninstallConfiguration** paraméterrel.
>
>A **UninstallConfiguration** paraméter bármely kiterjesztés konfigurációs alkalmazott a szolgáltatást eltávolítja. Minden kiterjesztés konfigurációs társítva a szolgáltatás beállításai. Így *eltávolítása* parancsmag **UninstallConfiguration** disassociates a <mark>telepítő</mark> a bővítmény konfiguráció nélkül hang-és hatékonyan eltávolítása a bővítmény. A bővítmény konfiguráció azonban a szolgáltatással társított marad.



## <a name="additional-resources"></a>További források

[Cloud Services konfigurálása](cloud-services-how-to-configure.md)
