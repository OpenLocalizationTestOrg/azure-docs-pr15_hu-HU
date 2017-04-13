<properties
    pageTitle="Bizonyos VMware virtuális gépeken futó az Azure-Azure automatizálási DSC webhely helyreállítási használatával |} Microsoft Azure"
    description="Azure automatizálási DSC használatával automatikusan Azure üzembe az Azure webhely helyreállítási mobilitás szolgáltatás és az Azure agent virtuális/tényleges gépekhez ismerteti."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Párhuzamos VMware virtuális gépeken futó az Azure-Azure automatizálási DSC webhely helyreállítási használatával

A tevékenységek kezelése csomagot biztosítunk Önnek a teljes biztonsági mentése és katasztrófa helyreállítási megoldás csomagja üzleti folytonosságot részeként használható.

Ez a Hyper-V együtt utazás indította a Hyper-V kópiáját. De a támogatási egy eltérő beállítás, mert az ügyfelek van-e több hypervisors és platformokon a felhő kibővítette azt.

Ha futtatja a VMware munkaterhelésekből és/vagy a fizikai kiszolgálók ma, egy kiszolgálóra fut minden összetevő Azure webhely helyreállítási az Azure-replikáció kommunikáció és adatok kezelése, Azure esetén a cél a környezetben.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>A webhely helyreállítási mobilitás szolgáltatás üzembe automatizálási DSC használatával
Első lépésként rövid részletezzük, hogy mit jelent ez management server módon.

A management server több kiszolgálói szerepkörök fut. Ezeket a szerepköröket egyik *konfigurációs*koordinálja a kommunikációt és az adatok replikációs és helyreállítási folyamat kezeli.

Ezenkívül a *folyamat* szerepkör replikációs átjáró működik. A szerepkör replikációs adatok fogad védett forrás gépek, optimalizálja a gyorsítótár, a tömörítési és titkosítás és küld Azure tárterület-fiókkal. A folyamat szerep a függvények egyike is a mobilitás szolgáltatás telepítése leküldéses védett gépek, és végezze el az automatikus felderítését VMware VMs.

Az Azure egy visszaállás esetén a *fő cél* szerepkör kezelheti a művelet részeként replikációs adatokat.

A védett gépekhez telefonszámokkal az *mobilitás szolgáltatás*. Ez az összetevő telepíti, minden gépi (VMware virtuális vagy fizikai kiszolgáló), amelyet szeretne Azure való replikáció. Azt rögzíti adatot ír a számítógépen, és továbbítja azokat a management server (folyamat szerepkör).

Ha üzemkészségének kezelése, fontos, megtudhatja, hogy a feladatok, a infrastruktúra és az érintett összetevőket. Akkor is majd megfelelnek a helyreállítási idő cél (RTO) és helyreállítási pont cél (Készletben). Ebben a környezetben a mobilitás szolgáltatása, amelyek a munkaterhelésekből ahogy várható védett billentyűt.

Igen, hogyan is, optimalizált módon, győződjön meg arról, hogy rendelkezik-e egy megbízható védett beállítását az egyes műveletek Management csomagja összetevők közreműködése?

Ez a cikk bemutatja, hogyan használhatja Azure automatizálási kívánt állam konfigurációs (DSC), és webhely-helyreállítás ahhoz, hogy:

- Az mobilitás szolgáltatás és az Azure virtuális agent vannak telepítve a Windows gépek, amelyet védeni kíván.
- Az mobilitás szolgáltatás és az Azure virtuális agent mindig futtatja az Azure esetén a replikáció célját.

## <a name="prerequisites"></a>Előfeltételek

- Tárolja a szükséges beállításokat a tárházba
- A szükséges jelszó regisztrálhatja a management Server tárolja a tárházba

 > [AZURE.NOTE] Egy egyedi jelszót minden management server jön létre. Ha több kezelési kiszolgálók üzembe fogja, akkor győződjön meg arról, hogy a helyes jelszót a passphrase.txt fájlokban tárolt.

- Management Framework WMF (Windows) 5.0-s (egy automatizálási DSC követelmény) védelem engedélyezni szeretné, számítógépén telepítve

 > [AZURE.NOTE] Ha WMF 4.0 telepítve van a Windows DSC gépek használni kívánt, című [Használata DSC leválasztott környezetben](#Use DSC in disconnected environments).

A mobilitás szolgáltatás a parancssorból keresztül telepíthető, és fogadja el a több argumentumokat. Ennek az, hogy miért kell rendelkeznie a bináris (után kinyerése őket a beállítása), és tárolja őket a hely, ahol meghallgathatja őket a DSC konfiguráció használatával.

## <a name="step-1-extract-binaries"></a>Lépés: 1: Kivonat bináris

1. Bontsa ki a fájlokat, ez a beállítás van szüksége, nyissa meg azt a következő könyvtár a kiszolgálóra:

    **\Microsoft azure webhely Recovery\home\svsystems\pushinstallsvc\repository**

    Ebben a mappában megjelenik egy névvel ellátott MSI-fájlt:

    **A Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    A következő parancsot használja a telepítő kibontásához:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Jelölje ki az összes fájlt, és küldje el nekik a tömörített mappa.

Ekkor a bináris kell a beállítást a mobilitás szolgáltatás automatizálása automatizálási DSC használatával.

### <a name="passphrase"></a>A jelszó

Következő lépésként meg kell határoznia, ahová a tömörített mappa. Azure tároló fiókkal is használhatja, a telepítő van szüksége a jelszó tárolása újabb látható módon. A agent majd regisztrálja a management Server a folyamat részeként.

A jelszó, hogy telepítette a kulcskezelő kiszolgálótól kapott passphrase.txt szövegfájlhoz menthetők.

Helyezze a tömörített mappa és a jelszó dedikált tároló az Azure tárterület-fiókjában.

![Mappa helye](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Ha inkább tartsa ezeket a fájlokat a hálózaton a megosztás, megteheti. Csak akkor szükséges győződjön meg arról, hogy fogja használni, akkor később DSC erőforrás hozzáféréssel rendelkezik, és a telepítő és jelszava is elérheti.

## <a name="step-2-create-the-dsc-configuration"></a>Lépés: 2: A DSC konfiguráció létrehozása

A telepítő WMF 5.0-s függ. A számítógép sikeresen alkalmazni a konfiguráció automatizálási DSC keresztül WMF 5.0-s kell lennie.

A környezet használja az alábbi példa DSC konfiguráció:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
A konfiguráció fog tegye a következőket:

- A változók a program tájékoztatja a konfigurációs helye a bináris beszerzése a mobilitás szolgáltatás és az Azure virtuális ügynök hol találja meg a jelszót, és a kimeneti helyének.
- A konfiguráció importálja a xPSDesiredStateConfiguration DSC erőforrás, hogy használható `xRemoteFile` a fájlokat tölthet le a tárat.
- A konfiguráció hoz létre, amelyhez szeretne tárolni a bináris könyvtárában.
- Az archiválás erőforrás a fájlok kinyerése a tömörített mappa.
- A csomag a telepítés erőforrás a mobilitás szolgáltatás telepíti át a UNIFIEDAGENT. Az adott argumentumokkal installer EXE. (Az argumentumokat Egyenletszerkesztővel változók módosítani kell a környezet igazodjon.)
- A csomag AzureAgent erőforrás telepíti az Azure virtuális ügynök, amelyek minden futó Azure virtuális ajánlott. Az Azure virtuális ügynök is lehetővé teszi a virtuális feladatátvétel után bővítések hozzáadása.
- A szolgáltatás vagy erőforrást biztosítja, hogy a kapcsolódó mobilitás services és az Azure services mindig futnak.

A konfiguráció mentése **ASRMobilityService**.

>[AZURE.NOTE] Ne feledje, hogy le szeretné cserélni a CSIP a konfigurációban a tényleges management server megfelelően, így a agent megfelelően csatlakoztatva, és a megfelelő jelszót használja.

## <a name="step-3-upload-to-automation-dsc"></a>Lépés 3: Automatizálási DSC feltöltése

Mivel a korábban elvégzett DSC konfiguráció szükséges DSC erőforrás modul (xPSDesiredStateConfiguration) importálja, kell importálni, hogy a modul található automatizálás, a DSC konfiguráció feltöltés előtt.

Jelentkezzen be az automatizálási fiókját, tallózással keresse meg azt a **eszközök** > **modulokat**, és kattintson a **Tallózás gombra a gyűjtemény**.

Itt is kereshet a modult, és importálja a fiókjába.

![Importálás a modul](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Amikor ezzel elkészült, nyissa meg a számítógépen, ahol az erőforrás-kezelő Azure modulokat telepítve van, és folytassa az újonnan létrehozott DSC konfigurációt importálni.

### <a name="import-cmdlets"></a>Importálás parancsmagok

A PowerShell jelentkezzen be az Azure-előfizetéséhez. A parancsmagok futtatásával környezetnek megfelelően, és rögzítse az automatizálási fiókadatok változó módosítása:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Töltse fel a konfigurációs automatizálási DSC használatával az alábbi parancsmagot:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>A konfiguráció található automatizálás DSC összeállítása

Ezután kell állíthat össze a konfiguráció található automatizálás DSC, így azt csomópontok regisztrálhatja elkezdheti. Amely elérése a következő parancsmagot:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Jelképező néhány percet, mert a konfiguráció esetén alapvetően telepítésével, a szolgáltatott DSC ki szolgáltatás.

Után a konfigurációs lefordításához meg információkat gyűjthet ki a feladat (Get-AzureRmAutomationDscCompilationJob) PowerShell használatával, illetve az [Azure portálon](https://portal.azure.com/).

![Feladat beolvasása](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Ezzel sikeresen közzé, és a DSC konfigurációs automatizálási DSC feltölteni.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Lépés: 4: Automatizálási DSC beépített gépek
>[AZURE.NOTE] Ebben az esetben végrehajtásával előfeltételei egyik, hogy a Windows gépeken futó frissülnek a legújabb verziójával WMF. Akkor letöltheti és telepítheti a megfelelő verziójú a Platform [Letöltéséhez](https://www.microsoft.com/download/details.aspx?id=50395).

Most, hogy a program alkalmaz a csomópontok DSC az egy metaconfig hoz létre. Ez a sikeresek, szüksége beolvasásához végpont URL-CÍMÉT, és az elsődleges kulcs kijelölt automatizálási fiókjának Azure-ban. Ezek az értékek területen, **kulcs** az **összes beállítások** lap automatizálási fióknak a is megkeresheti.

![Fő érték](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Ebben a példában ha a Windows Server 2012 R2 fizikai kiszolgáló, amely a webhely helyreállítási védelemmel ellátni kívánt.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Minden függőben lévő fájlműveletek nevezze át a beállításjegyzékben ellenőrzése

Mielőtt megkezdené a kiszolgáló társíthatja az automatizálási DSC végpontot, azt javasoljuk, hogy ellenőrizze a beállításjegyzékben Átnevezés fájlműveletek függőben. A telepítő őket előfordulhat, hogy tilthatják a befejezési miatt függőben kell indítani a számítógépet.

Futtassa a következő parancsmagot, ellenőrizze, hogy nincs függőben indítsa újra a rendszert a kiszolgálón:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Ezzel jeleníti meg üres, ha Ön az OK gombra a folytatáshoz. Ha nem, ez a kiszolgáló a karbantartási időszakokban újraindításával kell foglalkoznia.

A konfiguráció alkalmazása a kiszolgálón, indítsa el a PowerShell integrált parancsfájlok környezet (ISE), és futtassa az alábbi parancsfájlt. Ez a beállítás lényegében egy DSC helyi, amely arra utasítja az helyi Configuration Manager motor regisztrálhatja automatizálási DSC szolgáltatással, valamint a konkrét konfigurációja (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Ez a beállítás hatására a helyi Configuration Manager motor magának regisztrálása automatizálási DSC. Azt is meghatározza, hogy hogyan működnek a motor, mit kell tenni, ha egy konfigurációs eltolódás (ApplyAndAutoCorrect), és hogyan meg kell a konfigurálás folytatásához újraindítás szükségessége esetén.

A parancsfájl futtatása után a csomópont kell kezdje automatizálási DSC regisztrálása.

![Folyamatban lévő csomópont regisztráció](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Ha visszalép az Azure-portálra, megtekintheti, hogy az imént bejegyzett csomópont most már megjelent a portálon.

![A portál regisztrált csomóponthoz](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

A kiszolgálón futtatását is lehetővé teszi a PowerShell-parancsmagot annak ellenőrzéséhez, hogy a csomópont helyesen regisztrálva:

```powershell
Get-DscLocalConfigurationManager
```

Miután a konfigurációs lekért, és a alkalmazza a kiszolgálóra, ennek ellenőrzéséhez a következő parancsmagot:

```powershell
Get-DscConfigurationStatus
```

A kimenet jeleníti meg, hogy a kiszolgáló sikeresen lekért konfigurációját:

![Kimenet](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Ezeken kívül az mobilitás szolgáltatás beállítása foglalja magában: saját található *SystemDrive*\ProgramData\ASRSetupLogs a napló

Ez azt. Ezzel sikeresen rendszerbe, és a mobilitás szolgáltatás a számítógépen, amely a webhely helyreállítási védelemmel ellátni kívánt regisztrált. DSC gondoskodik róla, hogy a szükséges szolgáltatások mindig futnak.

![A sikeres telepítési](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Után a management server azt észleli, hogy a sikeres példányban, védelem beállítása, és a számítógépen a replikáció engedélyezése webhely helyreállítási használatával.

## <a name="use-dsc-in-disconnected-environments"></a>DSC használata kapcsolat nélküli környezetben

Ha a számítógép nem csatlakozik az internethez, továbbra is számíthat üzembe helyezését és a mobilitás szolgáltatás konfigurálása a védelemmel ellátni kívánt feladatok a DSC.

A saját DSC ki kiszolgáló létrehozhatnak a lényegében megadására, amely automatizálási DSC el ugyanolyan lehetőségeket a környezetben. Ez azt jelenti, hogy az ügyfelek a konfigurációt (miután regisztrált), a DSC végpont fog olvashat. Azonban egy másik, hogy manuálisan leküldéses a DSC konfiguráció a gép, helyi vagy távoli.

Megjegyzés: Ebben a példában az a számítógép egy hozzáadott paramétert. A távoli fájlok letöltése találhatók egy távoli megosztáson elérhető kell, hogy a védelemmel ellátni kívánt gép szerint. A parancsfájl végére ír elő a konfigurációs, és megkezdi a DSC konfiguráció alkalmazni a számítógépre.

### <a name="prerequisites"></a>Előfeltételek

Győződjön meg arról, hogy telepítve van-e a xPSDesiredStateConfiguration PowerShell-modult. Gépekhez Windows Ha WMF 5.0-s telepítve van a következő parancsmagot a cél gépeken is a xPSDesiredStateConfiguration modul telepítése:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Töltse le is, és a modul mentéséhez abban az esetben, ha azt eljuttatja WMF 4.0-s Windows gépek kell. Futtatása ezzel a parancsmaggal a számítógépen, ahol PowerShellGet (WMF 5.0-s) nem tartalmaz adatokat:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Is WMF 4.0, győződjön meg arról, hogy a [Windows 8.1 KB2883200 frissítése](https://www.microsoft.com/download/details.aspx?id=40749) számítógépén telepítve van.

Az alábbi beállításokkal is kell a Windows gépek, amelyek WMF 5.0-s és WMF 4.0 eltolni:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

A saját DSC ki a vállalati hálózathoz, amely letölthető automatizálási DSC lehetőségével imitálása kiszolgálón elindítását, című témakörben olvashat [ki DSC webkiszolgálóra beállításának](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Nem kötelező: Üzembe a DSC konfiguráció egy erőforrás-kezelő Azure-sablon használatával

Ez a cikk van összpontosít hogyan hozhat létre saját DSC konfiguráció automatikusan a mobilitás szolgáltatás és az Azure virtuális ügynök – telepítése, és győződjön meg róla, amely a védelemmel ellátni kívánt gépek futnak. Azt is, hogy egy erőforrás-kezelő Azure sablont, amely egy új vagy meglévő automatizálási Azure-fiókjába a DSC konfiguráció telepítésének. Automatizálási eszközök, amely tartalmazni fogja a változók környezetben létrehozása a sablon bemeneti paramétereket fogja használni.

Miután telepítette a sablont, is egyszerűen a hivatkozott lépés: 4 ebből az útmutatóból a beépített a gép.

A sablon fog tegye a következőket:

1. Meglévő automatizálási fiókot használ, vagy hozzon létre egy újat
2. Bemeneti paramétereket készítése a:
    - ASRRemoteFile – a mobilitás szolgáltatás beállítása tárolási helye
    - ASRPassphrase – a passphrase.txt fájl tárolási helye
    - ASRCSEndpoint – az adatkezelési kiszolgáló IP-címe
3. A xPSDesiredStateConfiguration PowerShell-modult importálása
4. Hozzon létre és állíthat össze a DSC konfiguráció

Az előző lépéseket, hogy elkezdheti bevezetési a védelem gépekhez a megfelelő sorrendben történik.

A sablon a telepítéshez, utasításokat [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC)található.

Telepítse a sablon a PowerShell használatával:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Következő lépések

Után a mobilitás-szolgáltatási ügynökök telepít, akkor a [Replikáció engedélyezése](site-recovery-vmware-to-azure.md#step-6-replicate-applications) a virtuális gépeken futó.
