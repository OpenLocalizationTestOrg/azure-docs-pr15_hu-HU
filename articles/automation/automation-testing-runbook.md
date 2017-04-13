<properties 
    pageTitle="Azure automatizálást egy runbook tesztelése |} Microsoft Azure"
    description="Mielőtt közzétenne egy runbook az Azure automatizálás, ellenőrizheti, hogy győződjön meg arról, hogy a várt módon működik.  Ez a cikk ismerteti, hogyan egy runbook megvizsgálja, és megtekintheti az eredményt."
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

# <a name="testing-a-runbook-in-azure-automation"></a>Azure automatizálási egy runbook tesztelése
Egy runbook tesztelésekor [vázlatverziója](automation-creating-importing-runbook.md#publishing-a-runbook) megy végbe, és a végrehajtható műveleteket hajtja végre. Korábbi nem jön létre, de a [kibocsátás](automation-runbook-output-and-messages.md#output-stream) és a [Figyelmeztetés és hiba](automation-runbook-output-and-messages.md#message-streams) adatfolyamok jelennek meg a tesztet a kimeneti ablakban. A [részletes adatfolyam](automation-runbook-output-and-messages.md#message-streams) levelek megjelenését a kimeneti ablakban csak akkor, ha a [$VerbosePreference változó](automation-runbook-output-and-messages.md#preference-variables) értéke a Folytatás gombra.

Annak ellenére, hogy a piszkozat verziója fut, a runbook továbbra is a szokásos módon végrehajtja a munkafolyamatot, és ellen erőforrások esetleges műveleteket végez a környezetben. Emiatt csak tesztelje a tesztkörnyezetben erőforrások runbooks.

A lépéseket minden [runbook típusú](automation-runbook-types.md) tesztelésére, és a teszteli a szöveges szerkesztője és a grafikus szerkesztő az Azure-portálon között nincs különbség.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Egy runbook tesztelje az Azure-portálon

Dolgozhat minden [runbook írja be](automation-runbook-types.md) az Azure-portálon.

1. Nyissa meg a runbook piszkozat verzióját a [szöveges szerkesztőben](automation-editing-a-runbook.md#Portal) vagy a [grafikus szerkesztő](automation-graphical-authoring-intro.md).
2. Kattintson a **vizsgálat** gombra kattintva nyissa meg a próba lap.
3. Ha a runbook paraméterek van, azok felsorolását a bal oldali ablaktáblában a teszt használandó értéket adni.
4. A tesztet a [Hibrid Runbook dolgozó](automation-hybrid-runbook-worker.md)futtatni szeretné, ha **Futtatása beállítások** módosításával **Hibrid dolgozó** , és válassza ki a cél csoport nevét.  Az alapértelmezett **Azure** futtatásához a tesztet a felhőben, hagyja.
5. Kattintson a **Start** gombra kattintva indítsa el a tesztet.
6. Ha a runbook [PowerShell munkafolyamat](automation-runbook-types.md#powershell-workflow-runbooks) vagy [grafikus](automation-runbook-types.md#graphical-runbooks), leállítása vagy felfüggesztése, miközben a kimeneti ablakban alatt található gombokkal van tesztelt. Amikor felfüggeszti a runbook, akkor az aktuális tevékenység befejezése előtt elhalasztása folyamatban. Miután a runbook fel van függesztve, állítsa le, vagy indítsa újra.
7. Vizsgálja meg a kimeneti ablakban a runbook kimenetét.


## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként létrehozhat vagy importálhat egy runbook, [vagy](automation-creating-importing-runbook.md) című témakört létrehozása az Azure automatizálás egy runbook importálása
- Grafikus létrehozásával kapcsolatos további információért lásd a [grafikus Azure automatizálási létrehozására](automation-graphical-authoring-intro.md)
- Első lépések a PowerShell munkafolyamat runbooks, lásd: [az első PowerShell munkafolyamat runbook](automation-first-runbook-textual.md)
- Lásd: való visszatéréshez állapotüzenetek és a hibák, beleértve az ajánlott eljárások, runboks beállításával kapcsolatos további [Runbook kimeneti és Azure automatizálási üzenetek](automation-runbook-output-and-messages.md)