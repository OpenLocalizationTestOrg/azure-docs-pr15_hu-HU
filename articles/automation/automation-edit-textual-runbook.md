<properties 
    pageTitle="Az Azure automatizálás szöveges runbooks szerkesztése"
    description="Ez a cikk lépéseit különböző PowerShell és a PowerShell munkafolyamat runbooks az Azure automatizálás használata a szöveges-szerkesztőben."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="stevenka"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/23/2016"
    ms.author="magoedte;bwren" />

# <a name="editing-textual-runbooks-in-azure-automation"></a>Az Azure automatizálás szöveges runbooks szerkesztése

A szöveges szerkesztő az Azure automatizálás [PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) és [PowerShell munkafolyamat runbooks](automation-runbook-types.md#powershell-workflow-runbooks)szerkesztése használható. Más kód szerkesztők, például az intellisense és a szín coding további speciális funkciókat tartalmazó, amely segít a közös runbooks erőforrások elérése tipikus funkciók azt.  Ebben a cikkben a lépések részletes leírását a szerkesztő más függvények elvégzéséhez.

A szöveges szerkesztő tartalmazza a tevékenységek, az eszközök és a gyermek runbooks kódját beillesztése egy runbook szolgáltatás. Írja be a kódot saját magát, hanem a rendelkezésre álló erőforrásokat listájából válassza ki, és rendelkezik a megfelelő kódot a runbook illeszteni.

Minden egyes runbook az Azure automatizálás két verziója, a Piszkozat és a közzétett tartalmaz. Szerkesztése a runbook piszkozat verzióját, és majd közzétehető így hajtható végre. Nem lehet szerkeszteni a közzétett verzió. [Közzététel egy runbook](automation-creating-importing-runbook.md#publishing-a-runbook) talál további információt.

[A grafikus Runbooks](automation-runbook-types.md#graphical-runbooks)szeretne dolgozni, olvassa el a [grafikus létrehozása az Azure automatizálás](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Az Azure portálján egy runbook szerkesztése

Az alábbi eljárással nyissa meg szerkesztésre a szöveges szerkesztőben egy runbook.

1. Az Azure-portálon jelölje be az automatizálási fiókját.
2. Kattintson a **Runbooks** csempére kattintva nyissa meg a runbooks listáját.
3. Kattintson a nevére a runbook szeretne szerkeszteni, és kattintson a **Szerkesztés** gombra.
6. Hajtsa végre a szükséges szerkesztését.
7. Ha fejeződtek a szerkesztést, kattintson a **Mentés** gombra.
8. Ha azt szeretné, hogy a piszkozat legújabb a közzéteendő runbook, kattintson a **Közzététel** gombra.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Egy runbook parancsmag beillesztése

2. A szöveges szerkesztő vászon vigye a kurzort, ahová a parancsmag.
3. Bontsa ki a tár vezérlő **parancsmagok** csomópontot. 
3. Bontsa ki a használni kívánt parancsmag tartalmazó modulra.
4. A parancsmaggal beszúrása, és válassza a **Hozzáadás a vászon**kattintson a jobb gombbal.  Ha egynél több paraméter megadása a parancsmag, alapértelmezett adhatók hozzá.  Jelölje ki egy másik paraméter megadása parancsmag is elemre.
4. A parancsmaggal kódját a program beszúrja a paraméterek teljes listáját.
5. Adja meg a szükséges paramétereket a kapcsos zárójeleket <> körülvett adattípusát helyett a megfelelő értéket.  Távolítsa el a fölösleges paramétereket.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>A gyermek runbook kódját beillesztése egy runbook

2. A szöveges szerkesztő vászon vigye a kurzort, ahová a [gyermek runbook](automation-child-runbooks.md)kódját.
3. Bontsa ki a tár vezérlő **Runbooks** csomópontot. 
3. A Beszúrás, és válassza a **Hozzáadás a vászon**runbook kattintson a jobb gombbal.
4. A gyermek runbook kódját bármely runbook paraméterekkel bármely helyőrzőkkel kerül.
5. A helyőrzők cseréje az egyes paraméterek megfelelő értéket.

### <a name="to-insert-an-asset-into-a-runbook"></a>Egy tárgyi eszköz beillesztése egy runbook

2. A szöveges szerkesztő vászon vigye a kurzort, ahová a gyermek runbook kódját.
3. Bontsa ki a tár vezérlő **eszközök** csomópontot. 
4. Bontsa ki a kívánt eszközt típusú csomópontot.
3. Az eszköz beszúrása, és válassza a **Hozzáadás a vászon**kattintson a jobb gombbal.  [Változó eszközök](automation-variables.md)jelölje be a **"Get változó" vászon hozzáadása** vagy a **"-változó beállítása" vászon való hozzáadásához** attól függően, hogy első, illetve a változó szeretné.
4. Az eszköz kódját beilleszti a runbook.



## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Az Azure portálján egy runbook szerkesztése

Az alábbi eljárással nyissa meg szerkesztésre a szöveges szerkesztőben egy runbook.

1. Az Azure-portálon jelölje be az **automatizálási** , és kattintson majd automatizálási fiók nevére.
2. Jelölje ki a **Runbooks** fülre.
3. Kattintson a nevére a runbook szeretne szerkeszteni, és válassza a **Szerző** lehetőséget.
5. Kattintson a **Szerkesztés** gombra a képernyő alján.
6. Hajtsa végre a szükséges szerkesztését.
7. Ha fejeződtek a szerkesztést, kattintson a **Mentés** gombra.
8. Ha azt szeretné, hogy a piszkozat legújabb a közzéteendő runbook, kattintson a **Közzététel** gombra.

### <a name="to-insert-an-activity-into-a-runbook"></a>Egy tevékenység beillesztése egy Runbook

1. A szöveges szerkesztő vászon vigye a kurzort, ahová a tevékenységet.
1. A képernyő alján kattintson a **Beszúrás** , majd az **tevékenységet**.
1. **Integráció a modul** oszlopban jelölje ki a modul, amely tartalmazza a tevékenység.
1. **Tevékenység** panelen válasszon egy tevékenységet.
1. Figyelje meg a tevékenység leírását a **Leírás** oszlopban. Másik lehetőségként kattinthat megtekintése a böngészőben a tevékenységhez tartozó Súgó indítása súgó részletes.
1. Kattintson a jobbra mutató nyílra.  Ha a tevékenység paraméterek van, azok felsorolását a saját információival.
1. Kattintson az ellenőrzés gombra.  Kód futtatását a tevékenység fog illeszteni a runbook.
1. Ha a tevékenység paramétereket igényel, adja meg a helyett a kapcsos zárójeleket <> körülvett adattípusának megfelelő értéket.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>A gyermek runbook kódját beillesztése egy runbook

1. A szöveges szerkesztő vászon vigye a kurzort, ahová a [gyermek runbook](automation-child-runbooks.md).
2. A képernyő alján kattintson a **Beszúrás** , majd az **Runbook**.
3. Jelölje ki a runbook, a középső oszlop beszúrása, majd kattintson a jobbra mutató nyílra.
4. Ha a runbook paraméterek van, azok felsorolását a saját információival.
5. Kattintson az ellenőrzés gombra.  Az aktuális runbook kód futtatását a kijelölt runbook fog illeszteni.
7. Ha a runbook paramétereket igényel, adja meg a helyett a kapcsos zárójeleket <> körülvett adattípusának megfelelő értéket.

### <a name="to-insert-an-asset-into-a-runbook"></a>Egy tárgyi eszköz beillesztése egy runbook

1. A szöveges szerkesztő vászon vigye a kurzort, ahová a tevékenységet, az eszköz beolvasásához.
1. A képernyő alján kattintson a **Beszúrás** , majd az **beállítást**.
1. A **Beállítás a művelet** oszlopban válassza a kívánt műveletet.
1. Jelölje ki a rendelkezésre álló eszközök központ oszlopban.
1. Kattintson az ellenőrzés gombra.  Kód első, illetve az eszköz a runbook fog illeszteni.



## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>A Windows PowerShell szolgáltatással Azure automatizálási runbook szerkesztése

A Windows PowerShell runbook szerkesztéséhez a szerkesztővel lehetőség, és mentse a .ps1 fájlba. A [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) parancsmag segítségével beolvashatja a runbook, majd a [Set-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) parancsmag lecseréli a meglévő piszkozat runbook és a módosított dokumentum megnyitása egy tartalmát.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>A Windows PowerShell szolgáltatással Runbook tartalmának beolvasásához.

A következő példa parancsok a egy runbook parancsfájl beolvasásához, és mentse azt egy parancsfájl módját mutatják. Ebben a példában a piszkozat verzióját keresi. Akkor is a runbook közzétett verziójának beolvasásához, bár ez a verzió nem módosíthatók.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"
    
    $runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
    $runbookContent = $runbookDefinition.Content

    Out-File -InputObject $runbookContent -FilePath $scriptPath

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>A Windows PowerShell szolgáltatással Runbook tartalmának módosítása

A következő példa parancsok bemutatják, hogyan le szeretné cserélni a meglévő tartalmát egy runbook parancsfájl tartalmával. Ne feledje, hogy ezt a minta eljárást ahogy [a Windows PowerShell-parancsprogramot fájlból egy runbook importálni](../automation-creating-or-importing-a-runbook#ImportRunbookScriptPS).

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName

## <a name="related-articles"></a>Kapcsolódó cikkek

- [Létrehozása vagy egy runbook az Azure automatizálás importálása](automation-creating-importing-runbook.md)
- [Tanulási PowerShell-munkafolyamat](automation-powershell-workflow.md)
- [Grafikus Azure automatizálási létrehozására](automation-graphical-authoring-intro.md)
- [Tanúsítványok](automation-certificates.md)
- [Kapcsolatok](automation-connections.md)
- [Hitelesítő adatok](automation-credentials.md)
- [Kimutatások](automation-schedules.md)
- [Változók](automation-variables.md)