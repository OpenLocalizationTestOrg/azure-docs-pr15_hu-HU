<properties 
   pageTitle="Az Azure automatizálás ütemezések |} Microsoft Azure"
   description="Az automatikus indításúként Azure automatizálást runbooks ütemezése automatizálást ütemezések használhatók. Ismerteti, hogyan hozhat létre és kezelhet az ütemezés, hogy automatikusan futtathat egy runbook adott időpontban vagy ismétlődő ütemezés."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="mgoedtel" />

# <a name="scheduling-a-runbook-in-azure-automation"></a>Az Azure automatizálás egy runbook ütemezése

Az Azure automatizálás, ha egy adott időben egy runbook ütemezéséhez összekapcsol azt egy vagy több ütemezést. Ütemezett beállítható úgy, hogy egyszer vagy egy ismétlődés óránként futtatása vagy napi ütemterv runbooks az Azure klasszikus portál és az Azure-portálon runbooks, továbbá ütemezheti őket hetente, havonta, a hét napjait vagy a hónap napja vagy egy a hónap adott napját.  Egy runbook csatolható több ütemezést, és az ütemezett beállíthatja, hogy a hozzá csatolt több runbooks.

>[AZURE.NOTE]  Ütemezés jelenleg nem támogatja Azure automatizálási DSC konfigurációk.

## <a name="windows-powershell-cmdlets"></a>A Windows PowerShell-parancsmagok

A parancsmagok az alábbi táblázat létrehozása és kezelése a Windows PowerShell az Azure automatizálás ütemezések szolgálnak. Azok az [Azure PowerShell-modult](../powershell-install-configure.md)részét képezi.

|A parancsmagok|Leírás|
|:---|:---|
|**Azure erőforrás-kezelő parancsmagok**||
|[Get-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603733.aspx)|Olvassa be az ütemezést.|
|[Új AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx)|Létrehoz egy új kimutatást.|
|[Eltávolítás-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603691.aspx)|Ütemezett eltávolítja.|
|[Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx)|A meglévő ütemezés tulajdonságait állítja be.|
|[Get-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt619406.aspx)|Ütemezett runbooks kérdezi le.|
|[Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx)|Ütemezett egy runbook társít.|
|[Unregister AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603844.aspx)|Az ütemezés egy runbook dissociates.|
|**Azure Service kezelő parancsmagok**||
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|Olvassa be az ütemezést.|
|[Új AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|Létrehoz egy új kimutatást.|
|[Eltávolítás-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|Ütemezett eltávolítja.|
|[Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|A meglévő ütemezés tulajdonságait állítja be.|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|Ütemezett runbooks kérdezi le.|
|[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Ütemezett egy runbook társít.|
|[Unregister AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Az ütemezés egy runbook dissociates.|

## <a name="creating-a-schedule"></a>A kimutatás létrehozása

Létrehozhat egy új runbooks ütemtervet az Azure-portálon a klasszikus portálon vagy a Windows PowerShell. Akkor is, hogy egy új kimutatást létrehozása, amikor az Azure klasszikus vagy Azure portálon ütemezett csatol egy runbook.

>[AZURE.NOTE] Ütemezett társítása egy runbook, amikor automatizálási modulokat aktuális verzióira tárolja a fiók, és csatolja őket, hogy az ütemezés.  Ez azt jelenti, hogy ha a 1.0 verziójú modul fiókban Miután létrehozott egy ütemtervet, és frissíti a modul 2.0-s verzióját, az ütemtervet folytatja 1.0.  Annak érdekében, hogy a modul frissített verzióját használja, létre kell hoznia egy új kimutatást. 

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Az ütemterv készítése az Azure-portálon

1. Az Azure-portálon automatizálási fiókjából kattintson az **eszközök** csempére az **eszközök** lap megnyitásához.
2. Kattintson az **Ütemezés** csempére az **Ütemezés** lap megnyitásához.
3. Kattintson a **Hozzáadás ütemezett** elemre a lap tetején.
4. Írja be az **Új ütemezés** lap **nevét** és tetszőlegesen a **leírását** az új ütemezés.
5. Jelölje ki, hogy az ütemtervet egyszeri fog futni, vagy feladatról időközönként **egyszer** vagy **Ismétlődés**kiválasztásával.  Ha **, jelölje be** a **Kezdő időpont** megadása, és kattintson a **Létrehozás**gombra.  Ha **az ismétlődés**lehetőséget választja, adja meg a **Kezdő időpont** és a gyakoriság az, hogy milyen gyakran szeretné megismételni az runbook - **óra**, **nap**, **hét**vagy **hónap**szerint.  Ha bejelöli a **heti** vagy **havi** a legördülő listából, az **Ismétlődés parancs** megjelenik a lap kiválasztása után formában jelennek meg az **ismétlődés lehetőséget** a lap és a hét napja választhat, ha lehetőséget választotta, **a hét**.  Ha lehetőséget választotta, **hónap**, válassza a **hét napjai** vagy az adott napon a hónap a naptárra, és végezetül tegye meg szeretné futtatni a hónap utolsó nap, vagy sem, és kattintson **az OK**gombra.   

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Az ütemterv készítése az Azure klasszikus portálon

1. Az Azure klasszikus portálon jelölje be az automatizálási, és majd jelölje be az automatizálási fiók nevére.
1. Kattintson az **eszközök** fülre.
1. Az ablak alján kattintson a **Beállítás hozzáadása**lehetőséget.
1. Kattintson a **Kimutatás hozzáadása**gombra.
1. Írjon be egy **nevet** , és tetszés szerint egy **leírást** az új schedule.your ütemezés **Egyszeri**, **óránkénti**, **napi**, **heti**vagy **havi**futtathatók.
1. Adja meg a **Kezdés időpontja** és egyéb lehetőségek a kijelölt ütemezés típusától függően.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Az ütemterv készítése a Windows PowerShell

Használhatja a [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) parancsmag egy új kimutatást létrehozása az Azure automatizálási klasszikus runbooks vagy a [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) parancsmag runbooks az Azure-portálon. Meg kell adnia a kezdési időpontot, az ütemezést és a gyakoriság futnia kell.

A következő példa parancsok hozzon létre egy erőforrás-kezelő Azure parancsmaggal havonta 30 és 15-e ütemezett mutatja.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"

A következő példa parancsok bemutatják, hogyan hozhat létre egy új kimutatást, a 3:30 du a 2015 január 20 kezdve az Azure Szolgáltatáskezelés parancsmag naponta futó.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

## <a name="linking-a-schedule-to-a-runbook"></a>Egy runbook ütemezett csatolása

Egy runbook csatolható több ütemezést, és az ütemezett beállíthatja, hogy a hozzá csatolt több runbooks. Ha egy runbook paraméterek tartalmaz, majd hozzá lehet adni értékek azokat. Esetleges kötelező paramétereket értékeket kell adnia, és előfordulhat, hogy adjon meg értéket a választható paramétereket.  Ezeket az értékeket Ez az ütemezés szerint a runbook indításakor minden alkalommal lesz.  Az azonos runbook csatolni egy másik ütemezése, és adja meg a másik paraméterértékeket.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Ütemezett hivatkozni szeretne egy runbook az Azure portálján

1. Az automatizálási fiókjából Azure-portálon kattintson a **Runbooks** csempére kattintva nyissa meg a **Runbooks** lap.
2. Kattintson a nevére a runbook ütemezése.
3. Ha a runbook jelenleg nem kapcsolódik egy ütemtervet, majd, kapnak a új kimutatást vagy egy meglévő kimutatás mutató hivatkozás létrehozása lehetőséget.  
4. Ha a runbook paraméterek, kiválaszthatja a beállítást, **futtassa (alapértelmezett: Azure) beállításainak módosítása** , és a **Paraméterek** lap bemutatják, ahol megadhatja a megfelelően.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Ütemezett hivatkozni szeretne egy runbook az Azure klasszikus portálján

1. Az Azure klasszikus portálon jelölje be az **automatizálási** , és kattintson majd automatizálási fiók nevére.
2. Jelölje ki a **Runbooks** fülre.
3. Kattintson a nevére a runbook ütemezése.
4. Kattintson az **Ütemezés** fülre.
5. Ha a runbook jelenleg nem kapcsolódik egy ütemtervet, majd, kapnak a beállítás **hivatkozást egy új kimutatást** vagy **egy meglévő kimutatás mutató hivatkozás**.  Ha a runbook jelenleg frissítésre van csatolva, kattintson a **hivatkozás** az ablak alján elérhető beállításokat.
6. Ha a runbook paraméterek tartalmaz, a rendszer kéri, ezek az értékek.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Ütemezett hivatkozni szeretne egy runbook szolgáltatást a Windows PowerShell használatával

A klasszikus runbook és a [Külső.FÜGGV-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) parancsmag az Azure-portálon runbooks az ütemezés csatolni a [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) is használhatja.  A paraméterek paraméterrel adhatja meg a runbook paraméterek értékeit. Lásd: a [egy Runbook az Azure automatizálás kezdve](automation-starting-a-runbook.md) paraméterértékek megadásával kapcsolatban további információt.

A következő példa parancsok bemutatják, hogyan ütemezett hivatkozni szeretne egy runbook egy erőforrás-kezelő Azure parancsmaggal paraméterrel.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"
A következő példa parancsok bemutatják, hogyan az Azure Szolgáltatáskezelés parancsmaggal paraméterekkel ütemezett hivatkozni.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## <a name="disabling-a-schedule"></a>Ütemezett letiltása

Ütemezett letiltásakor bármely hozzá csatolt runbooks nem fog működni, hogy az ütemezés szerint. Kézzel ütemezett letiltása vagy beállítása a kimutatások gyakorisággal elévülési időt, amikor létrehozza őket. A lejárati idő elérésekor letiltjuk az ütemtervet.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Az Azure portálról ütemezett letiltása

1. Az Azure-portálon automatizálási fiókjából kattintson az **eszközök** csempére az **eszközök** lap megnyitásához.
2. Kattintson az **Ütemezés** csempére az **Ütemezés** lap megnyitásához.
2. Kattintson a nevére kattintva nyissa meg a Részletek lap ütemezett.
3. Módosítsa az **nem** **engedélyezett** .

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>Az Azure klasszikus portálról ütemezett letiltása

Az Ütemterv részleteinek lapról az ütemezés az Azure klasszikus portál ütemezés tilthatja le.

1. Az Azure klasszikus portálon jelölje be az automatizálási, és kattintson majd automatizálási fiók nevére.
1. Kattintson az eszközök fülre.
1. Kattintson a nevére kattintva nyissa meg a részletek lapon ütemezett.
2. Módosítsa az **nem** **engedélyezett** .

### <a name="to-disable-a-schedule-with-windows-powershell"></a>A Windows PowerShell ütemezett letiltása

A [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) parancsmag használatával klasszikus runbook vagy az Azure-portálon runbooks a [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) parancsmag meglévő ütemezés tulajdonságainak módosítása. Tiltsa le az ütemtervet, adja meg a **hamis** **IsEnabled** paraméter.

A következő példa parancsok megjelenítése letiltása egy runbook egy erőforrás-kezelő Azure parancsmaggal ütemezése.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"

A következő példa parancsok megjelenítése a Azure Szolgáltatáskezelés parancsmaggal ütemezett letiltása.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

## <a name="next-steps"></a>Következő lépések

- Az Azure automatizálás runbooks, című cikkben ismerkedhet [egy Runbook az Azure automatizálás indítása](automation-starting-a-runbook.md) 