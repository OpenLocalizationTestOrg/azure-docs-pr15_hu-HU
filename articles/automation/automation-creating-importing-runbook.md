<properties
    pageTitle="Létrehozása vagy egy runbook az Azure automatizálás importálása"
    description="Ez a cikk ismerteti, hogyan hozzon létre egy új runbook Azure automatizálási vagy fájlból való importálásához."
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
    ms.author="magoedte;bwren" />

# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Létrehozása vagy egy runbook az Azure automatizálást importálása

Felveheti egy runbook Azure automatizálási vagy [újat hozna létre](#creating-a-new-runbook) , vagy egy meglévő runbook fájlból, illetve a [Runbook gyűjtemény](automation-runbook-gallery.md)importálásával. Ez a cikk információt nyújt hozhat létre és runbooks importálása fájlból.  Elérheti a részletek, a közösségi runbooks és modulokat [az Azure automatizálási Runbook és modul gyűjtemények](automation-runbook-gallery.md)elérése.

## <a name="creating-a-new-runbook"></a>Egy új runbook létrehozása

Az Azure automatizálás az egyik Azure portálokról vagy a Windows PowerShell használatával hozhat létre egy új runbook. A runbook létrehozása után adatokat [Tanulási PowerShell munkafolyamat](automation-powershell-workflow.md) vagy [grafikus létrehozása az Azure automatizálás](automation-graphical-authoring-intro.md)használatával módosíthatja.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Egy új Azure automatizálási runbook létrehozása az Azure klasszikus portálján

Csak dolgozhat [PowerShell munkafolyamat runbooks](automation-runbook-types.md#powershell-workflow-runbooks) az Azure-portálon.

1. Az Azure klasszikus-portálon kattintson a, **Új** **alkalmazás szolgáltatások**, **automatizálást**, **Runbook**, **Gyors létrehozásához**.
2. Adja meg a szükséges információkat, és kattintson a **Létrehozás**gombra. A runbook neve betűvel kell kezdődnie, és beállíthatja, hogy a betűket, számokat, aláhúzás és a szaggatott vonal menüpontra.
3. Ha szeretne a runbook szerkesztése most választógombot, majd kattintson a **Szerkesztés Runbook**. Egyéb esetben kattintson az **OK gombra**.
4. Az új runbook a **Runbooks** lap jelenik meg.


### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Egy új Azure automatizálási runbook létrehozása az Azure portálján

1. Az Azure-portálon nyissa meg az automatizálási fiókját.
2. Kattintson a nyissa meg a listát a runbooks **Runbooks** csempére.
3. Kattintson a **Hozzáadás egy runbook** gombra, majd a **Létrehozás egy új runbook**.
2. Írja be a runbook **nevét** , és válassza ki a megfelelő [típusú](automation-runbook-types.md). A runbook neve betűvel kell kezdődnie, és beállíthatja, hogy a betűket, számokat, aláhúzás és a szaggatott vonal menüpontra.
3. Kattintson a **Létrehozás** hozhat létre a runbook, és nyissa meg a szerkesztő gombra.


### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>A Windows PowerShell hozzon létre egy új Azure automatizálási runbook

A [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) parancsmag használatával hozzon létre egy üres [PowerShell munkafolyamat runbook](automation-runbook-types.md#powershell-workflow-runbooks). Megadhatja a **név** paraméter hozhat létre egy üres runbook, hogy később szerkesztheti, vagy megadhatja, hogy az **elérési út** paraméter runbook fájl importálása. A **Type** paraméter fel kell venni a runbook négyféle megadását.

A következő példa parancsok bemutatják, hogyan hozhat létre egy új üres runbook.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Egy fájlból importál egy runbook Azure automatizálási

Létrehozhat egy új runbook az Azure automatizálási egy PowerShell-parancsprogramot vagy PowerShell munkafolyamat (.ps1 kiterjesztésű) vagy egy exportált grafikus runbook (.graphrunbook) importálásával.  Meg kell adnia az importálás, figyelembe véve a következő szempontokat mérlegelve alapján létrehozott [runbook típusát](automation-runbook-types.md) .

- .Graphrunbook fájl csak előfordulhat, hogy importálja egy új [grafikus runbook](automation-runbook-types.md#graphical-runbooks), és a grafikus runbooks csak .graphrunbook-fájlból hozhatók létre.
- Egy .ps1-fájlt egy PowerShell-munkafolyamat csak egy [PowerShell-munkafolyamat runbook](automation-runbook-types.md#powershell-workflow-runbooks)importálhatók.  Ha a fájlban lévő több PowerShell-munkafolyamatok, majd az importálás sikertelen lesz. Saját fájl mentése a munkafolyamat egyes és importálása mindegyik külön-külön kell.
- .Ps1 fájl, amely nem tartalmaz egy munkafolyamatot egy [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) vagy egy [PowerShell-munkafolyamat runbook](automation-runbook-types.md#powershell-workflow-runbooks)importálható.  Egy PowerShell-munkafolyamat runbook importálja, majd azt lesznek konvertálva munkafolyamat és megjegyzések fog szerepelni a runbook végzett módosítások megadása.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Egy runbook importálása az Azure klasszikus portálján fájlból
A következő eljárással script-fájl importálása az Azure automatizálást.  Figyelje meg, hogy csak importálhatja .ps1 fájl egy PowerShell-munkafolyamat runbook a portálon.  Más típusú kell használnia az Azure-portálra.

1. Az Azure felügyeleti portálon kattintson a **automatizálási** , és válassza a automatizálási fiókkal.
2. Kattintson az **Importálás**gombra.
3. Kattintson a **Tallózás gombra a fájl** , és keresse meg az importálni kívánt parancsfájlt fájlt.
4. Ha szeretne a runbook szerkesztése most választógombot, majd kattintson a **Szerkesztés Runbook**. Egyéb esetben kattintson az OK gombra.
5. Az automatizálási fiókot a **Runbooks** lapon az új runbook fog megjelenni.
6. [Tegye közzé a runbook](#publishing-a-runbook) előtt kell meg tudja nyitni.


### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Egy runbook importálása az Azure portálján fájlból
A következő eljárással script-fájl importálása az Azure automatizálási.  

>[AZURE.NOTE] Figyelje meg, hogy csak importálhatja .ps1 fájl egy PowerShell-munkafolyamat runbook a portálon.

1. Az Azure-portálon nyissa meg az automatizálási fiókját.
2. Kattintson a nyissa meg a listát a runbooks **Runbooks** csempére.
3. A **Hozzáadás a runbook** gombra, és a **importálása**gombra.
4. Kattintson a **fájl Runbook** jelölje ki az importálni kívánt fájlt
2. Engedélyezve van a **név** mezőbe, majd esetén megváltoztathatja azt a beállítást.  A runbook neve betűvel kell kezdődnie, és beállíthatja, hogy a betűket, számokat, aláhúzás és a szaggatott vonal menüpontra.
3. [Runbook típusa](automation-runbook-types.md) automatikusan kijelöli, de a vonatkozó korlátozások figyelembe vétele után módosíthatja a típusát. 
3. Az új runbook runbooks listájában megjelennek a automatizálást fiókom.
4. [Tegye közzé a runbook](#publishing-a-runbook) előtt kell meg tudja nyitni.

>[AZURE.NOTE] A grafikus runbook vagy egy grafikus PowerShell munkafolyamat runbook importálás után a vezérlőt, amellyel a más típusú átalakítása, ha van. Nem konvertálható szöveges.

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Egy runbook importálása a Windows PowerShell-parancsprogramot fájlból

Az [Importálás-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) parancsmag használatával script-fájl importálása PowerShell munkafolyamat runbook vázlatként. Ha a runbook már létezik, az importálás meghiúsul, ha nem használ a *-hatályba* paraméter. 

A következő példa parancsok script-fájl importálása egy runbook módját mutatják.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Egy runbook közzététele

Amikor hoz létre, vagy egy új runbook importálása, kell közzététel azt futtatása előtt.  Egyes runbook található automatizálás tartalmaz, a Vázlat és a közzétett verzió. Csak a közzétett verzió érhető el kell futtatni, és csak a vázlatverziója szerkeszthető. A közzétett verzió nem érinti a vázlatverziója módosításait. Amikor a vázlatverziója elérhetővé kell tenni, majd teszi közzé azt, amely felülírja a közzétett verzió piszkozat verziójával.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Egy az Azure klasszikus portálon runbook közzététele

1. Nyissa meg a runbook az Azure klasszikus portálon.
1. A képernyő tetején kattintson a **Szerző**parancsra.
1. A képernyő alján kattintson a **Közzététel** majd az **Igen gombra** a üzenet.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Egy az Azure portálon runbook közzététele

1. Nyissa meg a runbook az Azure-portálon.
1. Kattintson a **Szerkesztés** gombra.
1. Az üzenetben kattintson a **Közzététel** gombra, majd **Igen** .


## <a name="to-publish-a-runbook-using-windows-powershell"></a>A Windows PowerShell szolgáltatással runbook közzététele

A [Közzététel-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) parancsmag egy runbook szolgáltatást a Windows PowerShell használatával közzé is használhatja. A következő példa parancsok bemutatják, hogyan teheti közzé a minta runbook.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Következő lépések
- Hogyan előnyeivel a Runbook és PowerShell-modult gyűjtemény kapcsolatos további tudnivalókért lásd: [az Azure automatizálást Runbook és modul gyűjtemények](automation-runbook-gallery.md)
- Egy szöveges szerkesztő PowerShell és a PowerShell munkafolyamat runbooks szerkesztéséről további információért lásd: [Szerkesztés szöveges runbooks az Azure automatizálás](automation-edit-textual-runbook.md)
- További információk a grafikus runbook szerzői, lásd: a [grafikus létrehozása az Azure automatizálás](automation-graphical-authoring-intro.md)
