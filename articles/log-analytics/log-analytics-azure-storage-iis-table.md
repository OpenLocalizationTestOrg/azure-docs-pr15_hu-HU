<properties
    pageTitle="IIS és a táblázat tárterület események blob-tárolóhoz használatának |} Microsoft Azure"
    description="Log Analytics erről a naplókat Azure táblatárolóhoz diagnosztika írása szolgáltatások vagy IIS naplók blob-tároló írni."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="using-blob-storage-for-iis-and-table-storage-for-events"></a>IIS blob-tárolóhoz és táblatároló használatával eseményekre vonatkozóan:

Log Analytics elolvashatja a naplókat, az alábbi szolgáltatások táblatárolóhoz diagnosztika írt vagy blob-tároló írt IIS naplók:

+ Szolgáltatás háló fürtök (előzetes verzió)
+ Virtuális gépeken futó
+ Webes/dolgozó szerepkörök

Log Analytics összegyűjtheti ezek az erőforrások adatait, mielőtt Azure diagnosztika engedélyezve kell lennie.

Diagnosztikai engedélyezve vannak, miután is használhatja az Azure-portálra, vagy PowerShell konfigurálása a naplógyűjtés napló elemzést.

Azure diagnosztika az Azure kiterjesztéssel, amely lehetővé teszi, hogy a diagnosztikai adatok összegyűjtése dolgozó szerepkört, webes szerepkör vagy virtuális gépen futó Azure-ban. Az adatok Azure tároló fiók tárolja, és ezután gyűjthetők a napló Analytics.

Log Analytics gyűjthetők össze az alábbi Azure diagnosztikai naplók a naplókat kell a következő helyeken:

| Naplófájl típusa | Erőforrás típusa | Hely |
| -------- | ------------- | -------- |
| IIS naplók | Virtuális gépeken futó <br> Webes szerepkörök <br> Dolgozó szerepkörök | üvegvatta-iis-naplófájlok (Blob-tárolóhoz) |
| Syslog | Virtuális gépeken futó | LinuxsyslogVer2v0 (tároló tábla) |
| Szolgáltatás háló működési események | Szolgáltatás háló csomópontok | WADServiceFabricSystemEventTable |
| Szolgáltatás háló megbízható szereplő események | Szolgáltatás háló csomópontok | WADServiceFabricReliableActorEventTable |
| Szolgáltatás háló megbízható szolgáltatás események | Szolgáltatás háló csomópontok | WADServiceFabricReliableServiceEventTable |
| Windows-eseménynaplók | Szolgáltatás háló csomópontok <br> Virtuális gépeken futó <br> Webes szerepkörök <br> Dolgozó szerepkörök | WADWindowsEventLogsTable (Táblatároló) |
| Esemény-nyomkövetés a Windows-naplók | Szolgáltatás háló csomópontok <br> Virtuális gépeken futó <br> Webes szerepkörök <br> Dolgozó szerepkörök | WADETWEventTable (Táblatároló) |

>[AZURE.NOTE] IIS naplók Azure webhelyekről jelenleg nem támogatott.

Virtuális gépekhez meg, hogy a [napló Analytics ügynök](log-analytics-azure-vm-extension.md) telepítése a virtuális gép be ahhoz, hogy további az összefüggéseket. Mellett nem elemezheti az IIS naplók és eseménynaplók, további elemzés, beleértve a konfigurációs a változások követésének, az SQL értékelése és a frissítés értékelési végezheti el.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a>Azure diagnosztika virtuális gépen engedélyezése az Eseménynapló és az IIS jelentkezzen be a webhelycsoport

Az alábbi eljárással ahhoz, hogy a Microsoft Azure-portálon Eseménynapló és az IIS napló webhelycsoport virtuális gép az Azure diagnosztika.

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a>Ahhoz, hogy egy virtuális gépen az Azure portálján Azure diagnosztika

1. A virtuális ügynököt virtuális gép létrehozásakor. Ha a virtuális gép már létezik, ellenőrizze a virtuális Agent már telepítve van.
    - Az Azure-portálon nyissa meg azt a virtuális gép, válassza a **Választható beállítási**, majd a **diagnosztikai** és **az** **állapot** beállítása.

    Befejeztével a virtuális kiterjesztése az Azure diagnosztika telepítése és futtatása. A diagnosztikai adatok gyűjtése erre a mellékre feladata.

2. Engedélyezze a figyelése, és állítsa be a bejelentkezés egy meglévő virtuális esemény. Lehetősége van engedélyezni a virtuális szintjén diagnosztika. Diagnosztikai engedélyezése, és adja meg az eseménynaplózást, hajtsa végre az alábbi lépéseket:
    1. Jelölje ki a virtuális.
    2. Kattintson az **ellenőrzés**gombra.
    3. Kattintson a **Diagnosztika lehetőségre**.
    4. Az **állapot** beállítása a **bekapcsolva**.
    5. Jelölje ki az egyes diagnosztikai naplók, amely a gyűjtendő.
    7. Kattintson az **OK gombra**.

Azure PowerShell használatával pontosan megadhatja az Azure-tárolóhoz írt eseményeket. A hivatkozott táblatároló vagy blob-írt IIS naplók írt Azure diagnosztika segítségével adatokat szeretne [gyűjteni](log-analytics-azure-storage-json.md).


## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a>Az IIS naplókban és esemény webhelycsoport Web szerepkört Azure diagnosztika engedélyezése

Olvassa el a munkafüzetek engedélyezéséről Azure diagnosztika általános lépéseket az [Hogyan szeretné engedélyezni diagnosztika a egy felhőalapú szolgáltatásba](../cloud-services/cloud-services-dotnet-diagnostics.md) . Az alábbi útmutatást ezen információk, és testre szabhatja a napló Analytics való használatra.

Az Azure diagnosztika engedélyezett:

- IIS naplók alapértelmezés szerint az átvitt scheduledTransferPeriod átadás gyakorisággal naplóadatok tárolja.
- Alapértelmezés szerint a Windows-eseménynaplók nem kerülnek.

### <a name="to-enable-diagnostics"></a>Ahhoz, hogy diagnosztika

Ahhoz, hogy a Windows-eseménynaplók, vagy módosíthatja a scheduledTransferPeriod, konfigurálása Azure diagnosztika segítségével az XML-konfigurációs fájl (diagnostics.wadcfg), ahogy azt [lépés: 4: a diagnosztikai konfigurációs fájl létrehozása, és a bővítmény telepítése](../cloud-services/cloud-services-dotnet-diagnostics.md)

A következő példa konfigurációs fájl az alkalmazás és a rendszer naplók IIS naplók és az összes eseményének gyűjti össze:

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

Győződjön meg arról, hogy a ConfigurationSettings Megadja azt a tárterület-fiókkal, a következő példának megfelelően:

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

A **fióknév** és **AccountKey** értéket az Azure-portálon tároló fiók kezelése hívóbetűk irányítópulton találhatók. A kapcsolati karakterlánc jegyzőkönyv **https**kell lennie.

Miután a frissített diagnosztikai konfiguráció érvényesül a felhőalapú szolgáltatásba, és diagnosztika ír, Azure tárolóhoz, majd készen áll napló Analytics konfigurálása.


## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a>Az Azure portal segítségével naplógyűjtés Azure-tárhelyről

Az Azure portal segítségével napló Analytics gyűjt a naplókat, a következő Azure-szolgáltatások beállítása:

+ Szolgáltatás háló fürt
+ Virtuális gépeken futó
+ Webes/dolgozó szerepkörök

Az Azure-portálon nyissa meg azt a napló Analytics-munkaterület, és végezze el az alábbi műveleteket:

1. Válassza a *tárterület-fiókok naplók*
2. Kattintson a feladat *hozzáadása*
3. Jelölje ki a tárterület-fiókot, amely tartalmazza a diagnosztikai naplók
  - Ehhez a fiókhoz egy klasszikus tárterület-fiókkal vagy Azure erőforrás-kezelő tároló fiók lehet.
4. Válassza ki a kívánt naplógyűjtés az adatok
  - A következő lehetőségek közül az IIS naplók; Események; Syslog (Linux); Esemény-nyomkövetés naplók; Szolgáltatás háló események
5. A forrás automatikusan kitölti a rendszer az adattípus alapján, illetve nem módosítható
6. Az OK gombra kattintva mentse a konfigurációt

További tárterület fiókok és típusú adatokat gyűjt a napló Analytics kívánt ismételje meg a 2-6.

Az körülbelül 30 percig nem látja a napló Analytics tárterület-fiók adatait. Csak akkor láthatja a konfiguráció alkalmazása után tároló írt adatok. Log Analytics nem olvassa fel a meglévő adatok a tárterület-fiókból.

>[AZURE.NOTE] A portál nem ellenőrzi, hogy a forrás szerepel-e a tárterület-fiókot, vagy ha új adatokat jelenleg írt.

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a>Azure diagnosztika virtuális gépen engedélyezése az Eseménynapló és az IIS jelentkezzen be a PowerShell használatá gyűjtemény

[Log Analytics konfigurálása Azure diagnosztika indexelendő](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) ismertetett lépésekkel hozhatja létre az Azure diagnosztika táblatárolóhoz írt olvasni a PowerShell használatával.

Azure PowerShell használatával pontosan megadhatja az Azure-tárolóhoz írt eseményeket.
További tudnivalókért olvassa el a [Diagnosztika segítségével, így az Azure virtuális gépeken futó](../virtual-machines-dotnet-diagnostics.md)című témakört.

Engedélyezze, és használja az alábbi PowerShell parancsprogramot Azure diagnosztika frissítése.
Ez a parancsfájl az egyéni naplózási beállításokkal is használhatja.
Módosítsa a parancsfájlt a tárterület-fiók, a szolgáltatás neve és a virtuális számítógép nevét.
A parancsprogram parancsmagok klasszikus virtuális gépeken futó használja.

Tekintse át a következő parancsfájl minta, másolja a vágólapra, szükség szerint módosítsa a minta PowerShell parancsprogramot fájlként mentheti, és futtassa a parancsfájl.

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a>Következő lépések

- Olvassa el a naplók az Azure [blob-tároló használatára JSON fájlok](log-analytics-azure-storage-json.md) szolgáltatások e írási diagnosztika blob-tárolóhoz JSON formátumban.
- [Megoldások engedélyezése](log-analytics-add-solutions.md) az adatok betekintést nyújt.
- [Használja a keresési lekérdezések](log-analytics-log-searches.md) hozzáadva elemezheti az adatokat.
