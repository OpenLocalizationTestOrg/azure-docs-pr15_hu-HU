<properties
    pageTitle="Függőlegesen méretezheti Azure virtuális gép skála beállítása |} Microsoft Azure"
    description="Hogyan függőlegesen méretezni a virtuális gép válaszként Azure automatizálást értesítések figyelése"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>A virtuális gép skála függőleges Automatikus méretezéssel állítja be.

Ez a cikk ismerteti, hogyan Azure [Virtuális gép skála beállítása](https://azure.microsoft.com/services/virtual-machine-scale-sets/) vagy anélkül, hogy reprovisioning függőlegesen méretezni. VMs, amelyek nem skála készletben függőleges beosztását, tekintse át az [Azure automatizálási készülék Azure virtuális függőlegesen méretezni](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Függőleges beosztását, más néven _méretezése_ és _méretezni_, azt jelenti, hogy növekvő vagy csökkenő virtuális gép (virtuális) méretű terhelési válaszként. Ez a [Vízszintes méretezés](./virtual-machine-scale-sets-autoscale-overview.md), más néven _skála,_ és _a méretezés_, ahol VMs számát úgy módosul, attól függően, hogy a terhelést a összehasonlítása.

Reprovisioning azt jelenti, hogy egy meglévő virtuális eltávolítása és cseréje a egy újat. Virtuális skála megadása a VMs méretének csökkentése vagy növelése, egyes esetekben kívánt méretezze át a meglévő VMs megőrzi az adatokat, az egyéb esetekben kell az új méret új VMs telepítése során. A dokumentum mindkét esetben foglalkozik.

Függőleges méretezés akkor lehet hasznos, ha:

- Egy virtuális gépeken futó épül szolgáltatása a kihasználtság (például hétvégék) elemre. Virtuális méretének csökkentése csökkentheti a havi költségeket.
- Nagyobb igény szerint további VMs létrehozása nélkül kezelésében virtuális méretének növelésével.

Beállíthatja függőleges méretezés indítóval kezdeményezett alapján metrikus alapuló figyelmeztetések magáról a virtuális skála megadása. Az értesítés aktiválásakor, akkor indul el egy webhook adott indítók egy runbook, amely méretezheti a skála megadása a felfelé vagy lefelé. Függőleges méretezés az alábbiak szerint állítható be:

1. Azure automatizálási fiók létrehozása a Futtatás lehetőséget.
2. Azure automatizálási függőleges méret runbooks virtuális skála eredménye a importálja az előfizetést.
3. Adja hozzá a runbook egy webhook.
4. Értesítés hozzáadása a virtuális skála megadása webhook értesítést használatával.

> [AZURE.NOTE] Függőleges autoscaling csak történhet a virtuális méretű bizonyos tartományba. Hasonlítsa össze az egyes méret alkalmazásra vonatkozó technikai adatok mielőtt úgy dönt, hogy egy másik és méretezése (nagyobb szám nem mindig jelez nagyobb virtuális méret). Beállíthatja, ha át kívánja méretezni, az alábbi pár méretű között:

>| Virtuális méretű pár méretezése |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Azure automatizálási fiók létrehozása a Futtatás lehetőséggel

A legfontosabb dolog kell tennie az Azure automatizálási ugyanis az runbooks, ha át kívánja méretezni a virtuális skála megadása példányok használt fiók létrehozása. A legutóbb [Azure automatizálási](https://azure.microsoft.com/services/automation/) jelent meg a "Futtatás fiókként" funkció, amely lehetővé teszi a beállítás be a szolgáltatás egyszerű automatikusan futó a runbooks egy felhasználó nevében nagyon egyszerűen. Erről további információt az alábbi cikkben:

* [Hitelesítő Runbooks Azure Futtatás mint fiókkal](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Az előfizetés Azure automatizálási függőleges méret runbooks importálása

A szükséges függőlegesen méretezni a virtuális skála készletek runbooks már közzétett az Azure automatizálási Runbook gyűjteményben. Importálja őket a előfizetés kövesse az ebben a cikkben leírt lépéseket:

* [Az Azure automatizálási Runbook és modul gyűjtemények](../automation/automation-runbook-gallery.md)

Válassza a Tallózás gombra a gyűjtemény lehetőséget a Runbooks menüből:

![Az importálandó Runbooks][runbooks]

Az importált kell runbooks jelennek meg. Jelölje ki a runbook szeretné-e függőleges méretezés vagy anélkül, hogy reprovisioning alapján:

![Runbooks gyűjtemény][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>A runbook egy webhook hozzáadása

Miután importálta az runbooks kell felveszi egy webhook a runbook, egy virtuális skála megadása a értesítés indított kell azt. Egy webhook létrehozása a Runbook részleteit a jelen cikkben ismertetett:

* [Azure automatizálási webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Győződjön meg arról, mielőtt bezárja a webhook párbeszédpanel, ez a következő szakaszban lesz szükség szerint másolja át a webhook URI.

## <a name="add-an-alert-to-your-vm-scale-set"></a>A virtuális skála megadása a értesítés hozzáadása

Az alábbi van egy PowerShell-parancsprogramot, amely egy virtuális skála megadása a értesítés hozzáadása jeleníti meg. A hivatkozott nevének mérőszám minden olyan esetben az értesítésre, a következő cikkben: [Azure Monitor autoscaling közös mértékek](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] Az értesítés időtartamon az ablak konfigurálása annak érdekében, hogy elkerülje a függőleges méretezése és bármely társított szolgáltatáskimaradás, túl gyakran ajánlott. Fontolja meg egy ablakában legalább legalább 20 – 30 percet vesz igénybe. Fontolja meg a Vízszintes átméretezés cél megszakításának elkerülése végett.

Értesítések létrehozásával kapcsolatos további információt az alábbi cikkekben talál:

* [Azure Monitor PowerShell rövid útmutató az első minták](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor platformok CLI rövid útmutató az első minták](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Összefoglalás

Ez a cikk egyszerű függőleges méretezési példák mutatott. Az alábbi építőelemek - automatizálási fiók, runbooks, webhooks, értesítések - események gazdag számos tartalmazó testre szabott műveletek lehet csatlakozni.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
