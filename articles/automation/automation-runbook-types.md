<properties 
   pageTitle="Azure automatizálást Runbook típusai"
   description="A különböző típusú, amelyek segítségével használhatja az automatizálási Azure és dolgot, figyelembe kell vennie, ha annak megállapítása, hogy melyik típust válassza runbooks ismerteti. "
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
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="azure-automation-runbook-types"></a>Azure automatizálási runbook típusai

Azure automatizálási az alábbi táblázat röviden ismertetett runbooks négyféle támogatja.  Az alábbi szakaszokban beleértve mikor érdemes használni az egyes szempontokat típusonként további információt.


| Típus |  Leírás |
|:---|:---|
| [A grafikus](#graphical-runbooks) | A Windows PowerShell és a létrehozott és szerkesztett teljesen grafikus szerkesztő az Azure-portálon alapján. | 
| [A grafikus PowerShell-munkafolyamat](#graphical-runbooks) | A Windows PowerShell-munkafolyamatok alapján és létrehozott és szerkesztett teljesen, a grafikus szerkesztőben az Azure-portálon. 
| [A PowerShell](#powershell-runbooks) | A Windows PowerShell-parancsprogramot alapján a szöveg runbook.
| [A PowerShell-munkafolyamat](#powershell-workflow-runbooks) | A Windows PowerShell-munkafolyamatok alapján a szöveg runbook. |


## <a name="graphical-runbooks"></a>A grafikus runbooks

[Grafikus](automation-runbook-types.md#graphical-runbooks) és a grafikus PowerShell munkafolyamat runbooks hozhatók létre, és a grafikus szerkesztő az Azure-portálon szerkeszthetők.  Exportálása fájlba, és ezt követően importálhatja őket az automatizálási egy másik fiókba, de nem hozhat létre, és nem szerkeszthetők a másik eszközzel.  A grafikus runbooks PowerShell-kód létrehozása, de közvetlenül tekinthető meg, és módosítsa a kódot. Grafikus runbooks nem konvertálható a [Szövegformátumok](automation-runbook-types.md), sem a szöveg runbook alakíthatók át grafikus formátumban. A grafikus runbooks konvertálhatók grafikus PowerShell munkafolyamat runbooks során az importálás és fordítva.

### <a name="advantages"></a>Előnyei

- Hozzon létre runbooks [PowerShell](automation-powershell-workflow.md)minimális ismerete.
- Adatkezelési folyamatok vizuális megjelenítése céljából.
- Magas szintű munkafolyamatok létrehozása a gyermek runbooks más runbooks közé.


### <a name="limitations"></a>Korlátozások

- Nem lehet szerkeszteni runbook kívüli Azure portálon.
- Megkövetelheti egy összetett logikájának végrehajtása a PowerShell-kódot tartalmazó kód tevékenységet.
- Tekinthető meg és szerkesztheti közvetlenül a PowerShell-kód, a grafikus munkafolyamat által létrehozott. Figyelje meg, hogy a kód hoz létre a kód tevékenységek tekinthet meg.


## <a name="powershell-runbooks"></a>A PowerShell runbooks

A Windows PowerShell PowerShell runbooks alapul.  Szerkesztheti közvetlenül a runbook az Azure-portálon szövegszerkesztővel kódját.  Minden kapcsolat nélküli szövegszerkesztőben és [a runbook importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure automatizálási is használhatja.

### <a name="advantages"></a>Előnyei

- Végrehajtása a PowerShell munkafolyamat további bonyodalmainak nélkül PowerShell kóddal összes komplex logikai. 
- Runbook gyorsabban grafikus vagy PowerShell munkafolyamat runbooks elindul, mivel ez nem kell futtatása előtt össze kell állítani.

### <a name="limitations"></a>Korlátozások

- A parancsfájlok PowerShell ismernie kell.
- Nem használható a [párhuzamos feldolgozási](automation-powershell-workflow.md#parallel-processing) párhuzamosan több művelet végrehajtásához.
- Nem használható [pontjainak](automation-powershell-workflow.md#checkpoints) hiba esetén runbook önéletrajz.
- PowerShell munkafolyamat runbooks és a grafikus runbooks állhat gyermek runbooks szerepel a kezdés-AzureAutomationRunbook parancsmaggal amely hoz létre új feladatot.

### <a name="known-issues"></a>Ismert problémák
Az alábbiakban PowerShell runbooks az aktuális ismert problémáit.

- PowerShell runbooks nem lehet beolvasni nem null értéket tartalmazó [változó eszköz](automation-variables.md) titkosítatlan.
- PowerShell runbooks nem tudja beolvasni a [változó eszköz](automation-variables.md) *~* nevét.
- Get-Process a egy PowerShellben hurkot runbook körülbelül 80 közelítő összeomolhat. 
- Egy PowerShell runbook meghiúsulhat, ha kísérel meg egyszerre igen nagy mennyiségű adat írni a kimeneti adatfolyam.   A szokásos dolgozhat a probléma megoldásához írása nagyméretű objektumok használata esetén a szükséges információk.  Ha például helyett az alábbihoz hasonló *Get-Process*írása, is kimeneti *Get-Process csak a szükséges mezőket |} Jelölje ki a Folyamatnév, Processzor*.

## <a name="powershell-workflow-runbooks"></a>A PowerShell munkafolyamat runbooks

PowerShell munkafolyamat runbooks olyan szöveget runbooks a [Windows PowerShell-munkafolyamatok](automation-powershell-workflow.md)alapján.  Szerkesztheti közvetlenül a runbook az Azure-portálon szövegszerkesztővel kódját.  Minden kapcsolat nélküli szövegszerkesztőben és [a runbook importálása](http://msdn.microsoft.com/library/azure/dn643637.aspx) az Azure automatizálási is használhatja.

### <a name="advantages"></a>Előnyei

- Végrehajtása a PowerShell munkafolyamat kóddal összes komplex logikai.
- [Pontjainak](automation-powershell-workflow.md#checkpoints) segítségével hiba esetén runbook önéletrajz.
- Használja a [párhuzamos feldolgozási](automation-powershell-workflow.md#parallel-processing) párhuzamosan több művelet végrehajtásához.
- Grafikus runbooks és lehetnek PowerShell munkafolyamat runbooks mint gyermek runbooks magas szintű munkafolyamatok létrehozása.


### <a name="limitations"></a>Korlátozások

- A szerző PowerShell munkafolyamat ismernie kell lennie.
- Runbook kell kezelni, az [objektumok deszerializálni](automation-powershell-workflow.md#code-changes)például PowerShell munkafolyamat további komplexitását.
- Runbook indításához PowerShell runbooks-nél, mivel a futtatása előtt össze kell állítani több időt vesz igénybe.
- A PowerShell runbooks is csak fog szerepelni gyermek runbooks parancsmaggal a kezdés-AzureAutomationRunbook amely hoz létre új feladatot.


## <a name="considerations"></a>Megfontolandó szempontok

Figyelembe kell vennie a következő szempontokat, amikor annak megállapítása, hogy melyik típust válassza az egy adott runbook.

- A grafikus runbooks nem alakíthatók szöveges írja vagy fordítva.
- Különböző típusú runbooks a használják a gyermek runbook korlátozások érvényesek.  További információt talál [az Azure automatizálás gyermek runbooks](automation-child-runbooks.md) .

  
## <a name="next-steps"></a>Következő lépések

- További információk a grafikus runbook szerzői, lásd: a [grafikus létrehozása az Azure automatizálás](automation-graphical-authoring-intro.md)
- PowerShell és PowerShell közötti különbségek megértéséhez runbooks, munkafolyamatok, lásd: [Oktatás Windows PowerShell-munkafolyamatok](automation-powershell-workflow.md)
- Létrehozhat vagy importálhat egy Runbook kapcsolatos további tudnivalókért lásd: [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)



