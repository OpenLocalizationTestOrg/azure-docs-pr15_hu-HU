<properties
   pageTitle="Azure áttekintése állapot konfiguráció szükséges |} Microsoft Azure"
   description="A Microsoft Azure-bővítmény kezelő a PowerShell kívánt állam konfiguráció áttekintése. Előfeltételek, architektúrája, parancsmagok együtt."
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

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Az Azure kívánt állam konfigurációs bővítmény kezelő – bevezetés #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Az Azure virtuális ügynök és a kapcsolódó bővítmények a Microsoft Azure infrastruktúrájának szolgáltatásai részét képezik. Virtuális bővítmények összetevői szoftver, amely a virtuális bővíthetők és leegyszerűsíti a különböző virtuális adatkezelési műveletek. Például a VMAccess kiterjesztést is használható a rendszergazdai jelszó alaphelyzetbe állítása, vagy az egyéni parancsfájl bővítmény hajtsa végre a virtuális parancsfájl használható.

Ez a cikk a PowerShell kívánt állam konfigurációs (DSC) bővítmény az Azure VMs az Azure PowerShell SDK részeként vezet be. Új parancsmagjai tölthet fel, és a PowerShell DSC konfiguráció alkalmazása egy Azure virtuális engedélyezve van a PowerShell DSC kiterjesztésű is használhatja. A PowerShell DSC bővítmény hívások be a PowerShell DSC életbe léptetni a virtuális a konfiguráció érkezett DSC. Ez a funkció is érhető el az Azure portálon keresztül.

## <a name="prerequisites"></a>Előfeltételek ##
**Helyi számítógép zónában** Együttműködhet az Azure virtuális bővítmény, kell az Azure-portálra vagy az Azure PowerShell SDK használni. 

**A vendégként való bekapcsolódáshoz Agent** A Azure virtuális a DSC konfiguráció által beállított kell lennie, amely támogatja vagy Windows Management Framework (WMF) 4.0-s és 5.0-OS. A teljes listáját a támogatott operációs rendszer verziója [DSC kiterjesztése a korábbi verziók](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/)találhatók.

## <a name="terms-and-concepts"></a>A kifejezések és fogalmak ##
Ez az útmutató az alábbi fogalmak ismerős megvalósító:

Konfiguráció – DSC konfigurációs dokumentum. 

Csomópont - DSC konfiguráció cél. A jelen dokumentum "csomópont" mindig hivatkozik egy Azure virtuális.

Konfiguráció – egy .psd1 adatfájl környezeti adatokat tartalmazó konfiguráció

## <a name="architectural-overview"></a>Architektúra áttekintése ##

Az Azure DSC bővítmény használja az Azure virtuális ügynök keretrendszer olvasása, elfogadni és Azure VMs futó DSC konfigurációk jelentést. A DSC bővítmény vár, amelyben legalább egy konfigurációs dokumentum és az Azure PowerShell SDK vagy az Azure portálon keresztül megadott paraméterek halmazának .zip fájlt.

A bővítmény hívásakor az első alkalommal fut-telepítési folyamatot. Ezt a folyamatot, a Management Framework WMF (Windows) használatáról a következő összefüggés verziót telepítése:

1. A Windows Server 2016 az Azure virtuális operációs rendszer esetén nem történik művelet. A Windows Server 2016 már telepítve PowerShell legújabb verzióját.
2. Ha a `wmfVersion` tulajdonság van megadva, a WMF azon verziója telepítve van, kivéve, ha a virtuális OS kompatibilis.
3. Ha nincs `wmfVersion` tulajdonság van megadva, a WMF legújabb megfelelő verziója van telepítve.

A WMF telepítéséhez kell indítani a számítógépet. Után indítsa újra a rendszert, a bővítmény letölti a .zip fájl a megadott a `modulesUrl` tulajdonság. Ha ezen a helyen Azure blob-tárolóhoz, a biztonsági jogkivonat is kell megadni a `sasToken` tulajdonság a fájl elérésére. Miután letöltött és kibontotta a .zip, a konfigurációs függvény meghatározott `configurationFunction` futtatásával hozza létre a MOF fájlt. A bővítmény futtat `Start-DscConfiguration -Force` a MOF létrehozott dokumentumon. A bővítmény rögzíti a kibocsátás és az Azure állapot csatornához ismét írások. Ettől kezdve a DSC LKT kezeli a ellenőrzése és javítása a szokásos módon. 

## <a name="powershell-cmdlets"></a>PowerShell-parancsmagok ##

PowerShell-parancsmagok használható ARM vagy ASM csomagolása, közzététele és figyelése DSC bővítmény telepítési környezetének. A következő parancsmagok szerepel a listában a ASM modulokat, de "Azure" "AzureRm" a ARM modell használata kell cserélni. Például `Publish-AzureVMDscConfiguration` használja ASM, ahol `Publish-AzureRmVMDscConfiguration` ARM használja. 

`Publish-AzureVMDscConfiguration`Megnyitja a konfigurációs fájl keres a függő DSC erőforrások és a konfigurációs és életbe léptetni a konfiguráció szükséges DSC erőforrásokat tartalmazó .zip fájl létrehozása. A csomag helyileg használatával is létrehozhat a `-ConfigurationArchivePath` paraméter. Ellenkező esetben a .zip fájl közzé, Azure blob-tárolóhoz, és a biztonsági jogkivonat biztonságossá tenni.

Ezzel a parancsmaggal által létrehozott .zip fájl a .ps1 konfigurációs parancsfájl van, a legfelső szintű az archiválási mappát. Erőforrások, hogy a modul mappát a archiválási mappába helyezi. 

`Set-AzureVMDscExtension`a virtuális konfigurációs objektummá, amely alkalmazhatók az Azure virtuális, majd a PowerShell DSC bővítmény szükséges beállításokat beszúrhatja `Update-AzureVM`.

`Get-AzureVMDscExtension`olvassa be az egy adott virtuális DSC bővítmény állapotát. 

`Get-AzureVMDscExtensionStatus`olvassa be a DSC bővítmény kezelő által helyeztek DSC konfigurációjának állapotát. Ez a művelet egyetlen virtuális vagy VMs csoportja hajtható végre.

`Remove-AzureVMDscExtension`a bővítmény kezelő eltávolítja a megadott virtuális géphez. Ezzel a parancsmaggal nem **eltávolítása a konfigurációt,** a WMF, módosítása vagy eltávolítása a virtuális gép alkalmazott beállítást. Csak a bővítmény kezelő távolítja el. 

**Főbb különbségek ASM és ARM parancsmagok**

- ARM-parancsmagok olyan szinkron. A parancsmagok ASM olyan aszinkron.
- ResourceGroupName, VMName, ArchiveStorageAccountName, verzió és a hely olyan minden új szükséges paramétert.
- ArchiveResourceGroupName a ARM paramétert új nem kötelező megadni. A paraméter is megadhat, amikor a tároló fiók tartozik egy másik erőforráscsoport, mint a hol a virtuális gép jön létre.
- ConfigurationArchive ArchiveBlobName neve a ARM
- ContainerName ArchiveContainerName neve a ARM
- StorageEndpointSuffix ArchiveStorageEndpointSuffix neve a ARM
- Az automatikus váltás a bővítmény kezelő a legújabb verzióra, és érhető el az automatikus frissítés engedélyezése ARM lett hozzáadva. A paraméter van a lehetséges, hogy a fősémát újraindul a virtuális a, a WMF új verziójának megjelenésekor. 


## <a name="azure-portal-functionality"></a>Azure portál funkciók ##
Tallózással keresse meg a klasszikus virtuális. A beállítások -> Általános kattintson "Bővítmények." Egy új ablak jön létre. Kattintson a "Hozzáadás" gombra, és jelölje be a PowerShell DSC.

A portál beviteli van szüksége.
**Konfigurációs modulokat vagy parancsprogramot**: A mező kitöltése nem kötelező. Konfigurációs parancsfájl tartalmazó .ps1 fájl vagy .zip fájl az .ps1 konfigurációs parancsfájl van a legfelső szintű és a modul mappák belül a .zip összes függő erőforrás van szükség. A létrehozható a `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` parancsmag az Azure PowerShell SDK szerepel. A .zip fájlt töltenek fel a biztonsági jogkivonat által biztosított felhasználói blob-tárolóban be. 

**Konfigurációs PSD1 adatfájl**: Ez a mező kitöltése nem kötelező. Ha a konfiguráció szükséges .psd1 konfigurációs adatok fájlban, használja ezt a mezőt kijelölheti, és töltse fel a felhasználót blob-tárolóhoz a biztonsági jogkivonat által biztosított hol. 
 
**Konfigurációs neve Module-Qualified**: .ps1 fájlokat lehet több konfigurációs függvényt. Írja be a nevet a konfigurációs .ps1 parancsfájl, ezek után pedig egy "\' és a konfigurációs függvény neve. Ha például a .ps1 parancsfájl "configuration.ps1" nevet, és a konfigurációs "IisInstall", ha írná be:`configuration.ps1\IisInstall`

**Konfigurációs argumentumokat**: Ha a kereséskonfigurációs függvény argumentumai tart, adja meg azokat az alábbi formátumban `argumentName1=value1,argumentName2=value2`. Megjegyzés: Ez a formátum alapértelmezettől eltérő hogyan konfigurációs argumentumokat elfogadott PowerShell-parancsmagok vagy az erőforrás-kezelő sablonok révén. 

## <a name="getting-started"></a>Első lépések ##

Az Azure DSC bővítmény DSC dokumentumokban tart, és ír elő őket a Azure VMs. A konfiguráció egy egyszerű példa követi. Mentse helyileg "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Az alábbi lépéseket a IisInstall.ps1 parancsfájl elhelyezése a megadott virtuális, hajtsa végre a konfigurációs és állapota a jelentést.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Naplózás ##

Naplók kerül:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[verziószáma]

## <a name="next-steps"></a>Következő lépések ##

További információt a PowerShell DSC, [Keresse fel a PowerShell dokumentáció központját](https://msdn.microsoft.com/powershell/dsc/overview). 

Tekintse meg az [erőforrás-kezelő Azure-sablon DSC bővítményhez](virtual-machines-windows-extensions-dsc-template.md
). 

További szolgáltatások keresése a PowerShell DSC, [Keresse meg a PowerShell gyűjtemény](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) DSC további forrásokat is kezelheti.

A bizalmas paraméterek átadása konfigurációk a részletekért olvassa el [kezelése hitelesítő adatok biztonságos a DSC bővítmény kezelő](virtual-machines-windows-extensions-dsc-credentials.md).
