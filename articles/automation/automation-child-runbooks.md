<properties 
   pageTitle="Az Azure automatizálás gyermek runbooks |} Microsoft Azure"
   description="A különböző módszerek az Azure automatizálás egy runbook egy másik runbook kezdve és a közöttük lévő információk megosztása a ismerteti."
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
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Az Azure automatizálást gyermek runbooks

Azure automatizálási újrafelhasználható, elemes runbooks különálló funkcióval más runbooks használt írni a legjobb módszer. Egy szülő runbook gyakran visszahívja egy vagy több alárendelt runbooks végrehajtásához szükséges funkciót. Hívja fel a gyermek runbook kétféle módon, és az egyes jelentős különbségek, ismerje meg, így eldöntheti, amely a különböző forgatókönyv a legalkalmasabb lesz.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>A gyermek runbook, használja a beágyazott végrehajtás meghívása

Egy runbook beágyazott át egy másik runbook meghívandó, a runbook nevével, és adjon meg értéket a paramétereken, pontosan, például egy tevékenységet vagy a parancsmaggal használható.  Az összes többi ily módon használható automatizálást ugyanazzal a fiókkal az összes runbooks érhetők el. A szülő runbook fog megvárja, amíg a gyermek runbook befejeződése után áthelyezése a következő sorra, és bármely kimeneti visszakerül közvetlenül a szülő.

Egy runbook beágyazott meghívásakor futtatja az ugyanazon feladatot, mint a szülő runbook. Nincs a gyermek runbook meg a futtatott a korábbi feltüntetése lesz. A kivételek és bármilyen a gyermek runbook a kimeneti adatfolyam lesz a szülő társítva. Kevesebb feladatok eredményez, és a megkönnyíti a nyomon követését és kapcsolatos problémák megoldásához, mivel a gyermek runbook, és az adatfolyam eredményt kivételek kapcsolódnak a szülő-feladatot.

Egy runbook közzétételekor bármely gyermek runbooks, hogy a hívások már tehető közzé. Ennek oka az Azure automatizálási bármely gyermek runbooks a társítást hoz létre, amikor egy runbook összeállítása után. Nem, ha a szülő runbook jelenik meg megfelelően a közzéteendő, de hoz létre kivétel, indításakor. Ez történik, ha a szülő runbook újbóli annak érdekében, hogy a megfelelő hivatkozást a gyermek runbooks. Nem kell a szülő runbook közzétenni, ha a gyermek runbooks bármelyikét megváltoznak, mivel a társítás lesz már készült.

Határozza meg a gyermek runbook beágyazott nevű lehet bármilyen adattípus, többek között az összetett objektumok, és nem nincs [JSON szerializálási](automation-starting-a-runbook.md#runbook-parameters) , amíg a runbook az Azure adatkezelési portál használatával indításakor, vagy a kezdés-AzureRmAutomationRunbook parancsmagot a.


### <a name="runbook-types"></a>Runbook típusok

Milyen típusú felhívhatja egymással:

- Egy [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) és a [grafikus runbooks](automation-runbook-types.md#graphical-runbooks) felhívhatja minden más szövegközi (mindkettő PowerShell-alapú).
- Egy [PowerShell-munkafolyamat runbook](automation-runbook-types.md#powershell-workflow-runbooks) és a grafikus PowerShell munkafolyamat runbooks felhívhatja, ha minden más szövegközi (mindkettő alapú PowerShell munkafolyamat)
- A PowerShell és a PowerShell munkafolyamat típusainak nem hívja fel a beágyazott egymást, és a kezdő-AzureRmAutomationRunbook kell használnia.
    
Ha a sorrend ügy közzététele:

- Közzététel runbooks sorrendjének csak a PowerShell-munkafolyamat és grafikus PowerShell-munkafolyamat runbooks ügyekben.


Használja a beágyazott végrehajtás grafikus vagy PowerShell munkafolyamat gyermek runbook hívásakor csak használhatja a runbook nevét.  Egy PowerShell-gyermek runbook hívásakor megadhat saját neve és előtt kell *.\\ * megadhatja, hogy a parancsprogram a helyi könyvtárában található. 

### <a name="example"></a>Példa

A következő példa elindítja a próba gyermek runbook, fogadja el a három paraméterek, egy összetett objektum, egész szám vagy logikai érték. A gyermek runbook, a kimeneti változó van rendelve.  Ebben az esetben a gyermek runbook-e egy PowerShell-munkafolyamat runbook

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Az alábbiakban az azonos példa egy PowerShell runbook használatával gyermekként.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>A gyermek runbook parancsmaggal indítása

A [Kezdés-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) parancsmag használatával indítsa el a runbook, [Indítsa el a Windows PowerShell runbook](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell)című témakörben ismertetett módon. Vannak olyan két módok – ezzel a parancsmaggal használható.  Egy módban a parancsmag ad eredményül a feladat azonosítója, amint a gyermek feladat jön létre a gyermek runbook  A többi módban, amelyek a megadásával történő beállította a **– Várakozás** paraméter parancsmag várakozik, amíg a gyermek feladat befejeződik, és a kimeneti ad vissza, a gyermek runbook a.

A feladat egy Gyermekszint runbook parancsmag lépések a szülő runbook a egy külön feladatot fog futni. További feladatokat, mint a runbook beágyazott meghívása eredményez, és a készíti további nehéz lehet nyomon követni őket. A szülő aszinkron elkezdődhet több alárendelt runbooks anélkül, hogy az egyes befejezéséhez várakozás. Az, hogy hívja fel a gyermek runbooks beágyazott párhuzamos végrehajtás azonos típusú a szülő runbook kellene használható a [párhuzamos kulcsszó](automation-powershell-workflow.md#parallel-processing).

A gyermek runbook parancsmag lépések paramétereinek egy hashtable szolgálnak, [Runbook paraméterek](automation-starting-a-runbook.md#runbook-parameters)leírt módon. Csak az egyszerű adattípusok használható. Ha a runbook összetett adattípusú paraméter van, majd meg kell hívni beágyazott.

### <a name="example"></a>Példa

A következő példa a gyermek runbook kezdődik paramétereket és várakozik, használja a kezdő AzureRmAutomationRunbook befejezéséhez – Várakozás a paramétert. Ha kész, a program az eredményt a gyermek runbook gyűjtött.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Módszerek a hívásához a gyermek runbook összehasonlítása

Az alábbi táblázat összefoglalja a két módszer egy runbook mobilról egy másik runbook közötti különbségeket.

| | Beágyazott| Parancsmag|
|:---|:---|:---|
|Feladat|Az ugyanazon feladatot szülőjeként gyermek runbooks futnak.|A gyermek runbook létrejön egy külön feladatot.|
|Végrehajtási|Szülő-runbook megvárja, amíg a gyermek runbook befejeződését.|Szülő-runbook továbbra is után azonnal elindul, gyermek runbook *vagy* szülő runbook megvárja, amíg a gyermek feladat elvégzését.|
|Kimenet|Szülő-runbook közvetlenül is beolvashat kimeneti gyermek runbook.|Szülő-runbook kell beolvasni a kimeneti gyermek runbook feladat *vagy* szülő runbook közvetlenül elérheti kimeneti gyermek runbook.|
|Paraméterek|A gyermek runbook paraméterekkel értékek külön-külön van megadva, és használhatja a bármilyen típusú adatokat.|Értékek a gyermek runbook paraméterek egy egyetlen hashtable egyesíthetők, és csak egyszerű, tartalmazhatnak tömb, és objektum adattípusokat, hogy emelés JSON szerializálási.|
|Automatizálási fiók|Szülő runbook csak gyermek runbook használhatja az automatizálási ugyanazzal a fiókkal.|Szülő runbook használhatja a gyermek runbook ugyanabban az Azure-előfizetésben és más előfizetés bármely automatizálást fiókból, ha van rá.|
|Közzététel|Gyermek runbook közzé kell, mielőtt szülő runbook közzétett.|Child runbook bármikor szülő runbook megkezdése előtt tehető közzé.|

## <a name="next-steps"></a>Következő lépések

- [Egy runbook kezdése az Azure automatizálást](automation-starting-a-runbook.md)
- [Runbook kimeneti és Azure automatizálást üzenetek](automation-runbook-output-and-messages.md)
