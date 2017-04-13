<properties 
   pageTitle="Runbook beállításai"
   description="Az Azure automatizálási, és hogyan módosíthatja őket az Azure Kezelőportálja és a Windows PowerShell használatával egy runbook beállításait ismerteti."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Runbook beállításai

Minden egyes runbook az Azure automatizálás rendszer több érdekében, hogy a naplózás viselkedésének módosítása és azonosítania kell magát. Ezeket a beállításokat a mindegyik leírása alább eljárások követ, hogyan lehet módosítani a őket.

## <a name="settings"></a>Beállítások

### <a name="name-and-description"></a>Név és leírás

Egy runbook neve után lett létrehozva nem módosítható. A leírás nem kötelező, és legfeljebb 512 karakternél.

### <a name="tags"></a>Címkék

Címkék lehetővé teszi, hogy adott szavakat vagy kifejezéseket egy runbook azonosításához. Például egy runbook [Runbook gyűjtemény](https://msdn.microsoft.com/library/dn781422.aspx)elküldésekor megadhatja adott azonosításához címkéket kell szerepelnie kell a runbook kategóriákat. Megadhat egy runbook több címkék vesszővel elválasztva.

### <a name="logging"></a>Naplózás

Alapértelmezés szerint részletes és előrehaladását rekordok korábbi nem írja. Ezeket a rekordokat bejelentkezni egy adott runbook beállításainak módosítása Ezeket a rekordokat kapcsolatos további tudnivalókért olvassa el a [Runbook kimeneti és az üzenetek](https://msdn.microsoft.com/library/dn879148.aspx)című témakört.

## <a name="changing-runbook-settings"></a>Runbook beállításainak módosítása

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Az adatkezelési portálon Azure runbook beállításainak módosítása

**A lap a runbook az** a módosíthatja a egy runbook az adatkezelési portálon Azure beállításait.

1. Az Azure adatkezelési portálon jelölje be az **automatizálási** , és kattintson majd automatizálási fiók nevére.
1. Jelölje ki a **Runbooks** fülre.
1. Kattintson egy runbook nevére.
1. Kattintson a **beállítás** fülre.

### <a name="changing-runbook-settings-with-windows-powershell"></a>A Windows PowerShell runbook beállításainak módosítása

A [Set-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) parancsmag használatával egy runbook beállításainak módosítása. Adja meg a több címkét szeretne, akkor vagy nyújthat egy tömb vagy egyetlen karakterláncot a címkék paraméterhez vesszővel elválasztott értékekkel. A [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx)az aktuális címkék elérheti.

A következő példa parancsok bemutatják, hogyan egy runbook tulajdonságainak beállításához. Ez a példa három címkéket ad a meglévő címkéket, és meghatározza, hogy be kell-e jelentkezve a részletes rekordokat.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Kapcsolódó cikkek
- [Runbook kimeneti és üzenetek](../automation-runbook-output-and-messages) 
- [Létrehozása vagy egy Runbook importálása](https://msdn.microsoft.com/library/dn643637.aspx) 