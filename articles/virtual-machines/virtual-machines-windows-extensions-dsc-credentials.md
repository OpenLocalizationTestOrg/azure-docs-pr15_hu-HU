<properties
   pageTitle="Hitelesítő adatok használata DSC Azure átadása |} Microsoft Azure"
   description="A hitelesítő adatok biztonságos átadása Azure virtuális gépeken futó kívánt állam konfiguráció PowerShell használatával – áttekintés"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="passing-credentials-to-the-azure-dsc-extension-handler"></a>Az Azure DSC bővítmény kezelő átadása hitelesítő adatok #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ez a cikk bemutatja az állapot konfigurációs kívánt bővítmény, az Azure. A DSC bővítmény kezelő áttekintése érhető el [a kívánt állapot konfigurációs Azure bővítmény kezelő bemutatása](virtual-machines-windows-extensions-dsc-overview.md). 


## <a name="passing-in-credentials"></a>A hitelesítő adatok átadása
A beállítási folyamat részeként, előfordulhat, hogy szükséges állítson be felhasználói fiókokat, szolgáltatások elérése vagy program telepítése felhasználói környezetben. Hajtsa végre az alábbi tényezőket, meg kell adnia a hitelesítő adatait. 

DSC paraméteres konfigurációk, amelyben hitelesítő adatok a konfigurációs átadott és biztonságosan az MOF-fájlokban tárolt tesz lehetővé. Az Azure-bővítmény kezelő egyszerűsíti a hitelesítő adatok kezelése azzal, hogy a tanúsítványok automatikus kezelése. 

Vegye figyelembe az alábbi DSC konfigurációs parancsprogramot, amely létrehoz egy helyi felhasználói fiókot a megadott jelszóval:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Fontos *csomópont localhost* szerepeltetni a konfiguráción. Ha ez az utasítás hiányzik, az alábbi lépésekkel nem működik, mint a bővítmény kezelő kifejezetten az csomópont localhost utasítás keres. Fontos is typecast *[PsCredential]*felvenni, az adott elemtípushoz elindítja a bővítmény titkosítsa a hitelesítő adatokat. 

Tegye közzé a parancsfájl blob-tárolóhoz:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Adja meg az Azure DSC bővítmény, és adja meg a hitelesítő adatok:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"
 
$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments
 
$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hogyan biztosított a hitelesítő adatok
A hitelesítő kód futó kér. Miután megadva, akkor vannak tárolva memória röviden. Ha közzé a `Set-AzureVmDscExtension` parancsmag, azt a virtuális HTTPS való továbbításkor, ahol Azure tárolja titkosítva lemezen, a helyi virtuális tanúsítvány használatával. Ezután röviden visszafejti a memóriában és újra titkosított DSC átadandó.

Ez a jelenség eltér [a bővítmény kezelő nélkül biztonságos konfiguráció használata](https://msdn.microsoft.com/powershell/dsc/securemof). Az Azure környezetben küldött konfigurációs adatok tanúsítványok keresztül biztonságosan úgy adja vissza. A DSC bővítmény kezelő használatakor, akkor nincs szükség $CertificatePath vagy egy $CertificateID / ConfigurationData $Thumbprint bejegyzést.


## <a name="next-steps"></a>Következő lépések ##

További tájékoztatást a az Azure DSC bővítmény kezelő című témakörben [az Azure kívánt állam konfigurációs bővítmény kezelője](virtual-machines-windows-extensions-dsc-overview.md). 

Tekintse meg az [erőforrás-kezelő Azure-sablon DSC bővítményhez](virtual-machines-windows-extensions-dsc-template.md).

További információt a PowerShell DSC, [Keresse fel a PowerShell dokumentáció központját](https://msdn.microsoft.com/powershell/dsc/overview). 

További szolgáltatások keresése a PowerShell DSC, [Keresse meg a PowerShell gyűjtemény](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) DSC további forrásokat is kezelheti.
