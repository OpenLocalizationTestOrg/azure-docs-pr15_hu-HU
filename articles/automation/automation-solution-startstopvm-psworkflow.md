<properties 
    pageTitle="Indítása és leállítása virtuális gépeken futó Azure automatizálást - PowerShell munkafolyamat |} Microsoft Azure"
    description="Azure automatizálási forgatókönyv indítása és leállítása klasszikus virtuális gépeken futó runbooks együtt grafikus verziója."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure automatizálási forgatókönyv - indítása és leállítása virtuális gépeken futó

Ebben az esetben Azure automatizálási indítása és leállítása klasszikus virtuális gépeken futó runbooks tartalmazza.  Ebben az esetben használható az alábbiak egyikét:  

- A saját környezetben használja a runbooks módosítás nélkül. 
- Módosítsa a runbooks testre szabott funkciók végrehajtásához.  
- Hívja fel a runbooks át egy másik runbook átfogó megoldásban részeként. 
- Oktatóanyagok fogalmak szerzői runbook minderről a runbooks használnak. 

> [AZURE.SELECTOR]
- [A grafikus](automation-solution-startstopvm-graphical.md)
- [A PowerShell-munkafolyamat](automation-solution-startstopvm-psworkflow.md)

Az ebben az esetben a PowerShell munkafolyamat runbook verziója. Érhető el is [grafikus runbooks](automation-solution-startstopvm-graphical.md)használatával.

## <a name="getting-the-scenario"></a>Bevezetés az alkalmazási példát

Ebben az esetben, ahol a két PowerShell munkafolyamat runbooks, melyet letölthet az alábbi hivatkozásokat.  Az ebben az esetben a grafikus runbooks mutató hivatkozások [grafikus verziója](automation-solution-startstopvm-graphical.md) jelenik meg.

| Runbook | Hivatkozás | Típus | Leírás |
|:---|:---|:---|:---|
| Kezdés-AzureVMs | [Azure klasszikus VMs indítása](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | A PowerShell-munkafolyamat | Minden klasszikus virtuális gépeken futó az Azure subscriptionor az összes virtuális gépeken futó nevével kezdődik egy adott szolgáltatás. |
| Leállítás-AzureVMs | [Azure klasszikus VMs leállítása](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | A PowerShell-munkafolyamat | Leállítja az összes virtuális gépeken futó automatizálást-fiókjában vagy egy adott szolgáltatáshoz nevű összes virtuális gépeken futó.  |


## <a name="installing-and-configuring-the-scenario"></a>Való telepítéséről és konfigurálásáról az alkalmazási példát

### <a name="1-install-the-runbooks"></a>1. a runbooks telepítése

A runbooks a letöltés után importálhatja őket az eljárást követve importálása [egy Runbook](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook).

### <a name="2-review-the-description-and-requirements"></a>2. a leírás és a követelmények áttekintése
A runbooks megjegyzésként súgószöveg, amely tartalmazza a szükséges eszközök és leírását tartalmazza.  Ez a cikk a ugyanazokat az információkat is megnyithatja. 

### <a name="3-configure-assets"></a>3. a eszközök beállítása
A runbooks csak a következő eszközöket, hogy létre kell hoznia és a megfelelő értékek kitöltéséhez.

| Eszköz típusa | Eszköz neve | Leírás |
|:---|:---|:---|:---|
| Hitelesítő adatok | AzureCredential | Hitelesítésszolgáltató indítása és leállítása, az előfizetés Azure virtuális gépeken futó rendelkező fiók hitelesítő adatait tartalmazza.  Azt is megteheti a **hitelesítő adatok** paramétert a **Hozzáadás-AzureAccount** tevékenység egy másik hitelesítőadat-eszköz is megadhat. |
| Változó | AzureSubscriptionId | Előfizetés azonosítója Azure-előfizetése tartalmazza. |

## <a name="using-the-scenario"></a>Az alkalmazási példát használatával

### <a name="parameters"></a>Paraméterek

Minden egyes runbooks a következő paraméterek van.  Esetleges kötelező paramétereket értékeket kell adnia, és tetszés szerint megadhatja az értékek-további paramétereket attól függően, hogy az igényeknek megfelelően alakíthatja.

| Paraméter | Típus | Kötelező | Leírás |
|:---|:---|:---|:---|
| ServiceName | karakterlánc | nem | Ha egy értéket adni, majd az összes virtuális gépeken futó ilyen szolgáltatás nevű elindul, illetve leáll.  Ha nincs érték van megadva, majd az összes klasszikus virtuális gépeken futó az Azure előfizetés lépések vagy leállt. |
| AzureSubscriptionIdAssetName | karakterlánc | nem | Az Azure előfizetés előfizetés azonosítója tartalmazó [változó eszköz](#installing-and-configuring-the-scenario) nevét tartalmazza.  Ha egy érték nem ad meg, *AzureSubscriptionId* használják.  |
| AzureCredentialAssetName | karakterlánc | nem | A [hitelesítő adatok eszköz](#installing-and-configuring-the-scenario) használata a runbook hitelesítő adatait tartalmazó nevét tartalmazza.  Ha egy érték nem ad meg, *AzureCredential* használják.  |

### <a name="starting-the-runbooks"></a>A runbooks indítása

Használhatja a módszerek egyikét a [kezdési egy runbook az Azure automatizálást](automation-starting-a-runbook.md) indítása vagy a runbooks ebben az esetben.

A következő példa parancsok használja a Windows PowerShell **StartAzureVMs** összes virtuális gépeken futó kezdődik, a szolgáltatás neve *MyVMService*futtatásához.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Kimenet

A runbooks fogja, hogy a [kimenő üzenet](automation-runbook-output-and-messages.md) az egyes virtuális gép jelző attól függetlenül, hogy az indítás vagy a Leállítás utasítás sikerült elküldeni.  Egy adott karakterlánc határozza meg az eredményt az egyes runbook kimeneti kereshet.  Az alábbi táblázat a lehetséges kimeneti karakterláncok jelennek meg.

| Runbook | Feltétel | Üzenet |
|:---|:---|:---|
| Kezdés-AzureVMs | Virtuális gép már fut  | MyVM már fut |
| Kezdés-AzureVMs | Indítsa el a virtuális gép sikerült elküldeni kérése | MyVM van indítva. |
| Kezdés-AzureVMs | Virtuális gép kezdő kérése sikertelen volt  | Nem indult el MyVM |
| Leállítás-AzureVMs | Virtuális gép már leállítása  | MyVM már leállítása |
| Leállítás-AzureVMs | Virtuális gép sikerült elküldeni vonatkozó leállítása | MyVM leállt. |
| Leállítás-AzureVMs | Virtuális gép leállítása kérése sikertelen volt  | Nem sikerült MyVM leállítása |

Például a következő kódrészletet egy runbook a megkísérli összes virtuális gépeken futó kezdődhet a szolgáltatás neve *MyServiceName*.  Ha van ilyen a kezdő kéréseket fail hibaüzenet esetén végrehajtandó műveleteket is venni. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Részletes kifejtése

Az alábbiakban ebben az esetben a runbooks részletes kifejtése.  Az információk segítségével testre szabhatja a runbooks, vagy közvetlenül az őket a saját automatizálást szerzői bemutatása.

### <a name="parameters"></a>Paraméterek

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

A munkafolyamat indítása első az értékeket a [bemeneti paramétereket](#using-the-scenario).  Ha az eszköz nevek nem rendelkeznek az alapértelmezett neveket használják.

### <a name="output"></a>Kimenet

    # Returns strings with status messages
    [OutputType([String])]

Ebben a sorban deklarálása, hogy a runbook kimenetét lesz egy karakterlánc.  Ez a használata nem kötelező, de a legjobb módszer, a runbook szolgál a [gyermek runbook](automation-child-runbooks.md) , hogy egy szülő runbook tudni fogja, hogy a kimeneti típus számíthat, amikor.

### <a name="authentication"></a>Hitelesítés

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

A következő sort a [hitelesítő adatok](automation-configuring.md#configuring-authentication-to-azure-resources) és a többi a runbook használt Azure előfizetés beállítása.
**Get-AutomationPSCredential** első elindítása és leállítása, az előfizetés Azure virtuális gépeken futó access a hitelesítő adatokat tároló az eszköz első használjuk. **Hozzáadás-AzureAccount** Ez az eszköz ezután használja hitelesítő adatainak beállítása.  A kimenet van rendelve egy üres változó, így ez nem szerepel a runbook eredménye.  

Az előfizetés azonosítójú változó eszköz visszakeresése a **Get-AutomationVariable** és az előfizetés **Kiválasztása-AzureSubscription**be.

### <a name="get-vms"></a>VMs beszerzése

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-AzureVM** a a runbook működik a virtuális gépeken futó beolvasásához használt.  Egy értéket a **ServiceName** beviteli változó, amennyiben csak a virtuális gépeken futó ilyen szolgáltatás nevű beolvasható.  Ha üres **ServiceName** , az összes virtuális gépeken futó beolvasható.

### <a name="startstop-virtual-machines-and-send-output"></a>Virtuális gépeken futó kezdési és befejezési és a kimeneti küldése

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

A következő sort a virtuális gépeken lépéseinek.  A virtuális gép a **PowerState** először ellenőrzi, hogy ha már fut vagy leáll, attól függően, hogy a runbook.  Ha már van a cél állapotban, majd egy üzenetet a rendszer elküldi eredményt ad, és a runbook véget ér.  Ha nem, majd **Start-AzureVM** vagy **Leállítása-AzureVM** használatos indítása és leállítása, az eredmény a kérelem változó tárolt virtuális készülék megpróbálja.  Üzenet megadása, hogy elindítása és leállítása a kérelem elküldése sikerült kimeneti majd küldi.


## <a name="next-steps"></a>Következő lépések

- További tudnivalókat a gyermek runbooks című témakörben talál [az Azure automatizálást gyermek runbooks](automation-child-runbooks.md) 
- [További információ a kimenő üzenetek runbook végrehajtási és hibaelhárítási naplózás során, Runbook kimeneti és](automation-runbook-output-and-messages.md) témakörben található automatizálás Azure üzenetek
