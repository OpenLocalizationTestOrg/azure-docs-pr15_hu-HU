<properties 
   pageTitle="Tanulási PowerShell-munkafolyamat"
   description="Ebből a cikkből megegyezik egy rövid lecke PowerShell ismerős szerzők adott különbségeiről PowerShell PowerShell munkafolyamat megértéséhez."
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

# <a name="learning-windows-powershell-workflow"></a>Oktatás a Windows PowerShell-munkafolyamat

Az Azure automatizálás Runbooks arra használják, mint a Windows PowerShell-munkafolyamatok.  A Windows PowerShell-munkafolyamat hasonlít egy Windows PowerShell-parancsprogramot, de, hogy új felhasználók kiigazodni jelentős eltérések.  Ez a cikk a már jól ismert PowerShell a felhasználók számára íródott, és röviden bemutatja, hogy szükséges, ha a PowerShell használata az egy runbook munkafolyamat egy PowerShell-parancsprogramot konvertálni fogalmak.  

Munkafolyamat több, hosszabb ideig futó feladatokat, vagy több lépést koordinálása igénylő több eszközök vagy felügyelt csomópontok programozott, csatlakoztatott lépésből áll. Fölé egy normál parancsfájl munkafolyamat előnyei közé tartozik, az azt jelenti, hogy több eszközzel elleni művelet egyidejű végrehajtása és a hibák az automatikus helyreállítás lehetősége. A Windows PowerShell munkafolyamat egy Windows PowerShell-parancsprogramot, amely a Windows folyamatkövető alaprendszer használja. Bár a munkafolyamat írása a Windows PowerShell szintaxisát, és a Windows PowerShell által indított, akkor Windows folyamatkövető alaprendszer által feldolgozott.

Ez a cikk hivatkozott témakörök tartalmaznak a teljes részletekért olvassa el [A Windows PowerShell-munkafolyamat – első lépések](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Runbook típusai

Háromféle az Azure automatizálási, *PowerShell munkafolyamat*, *PowerShell* és *a grafikus*runbook van.  Hoz létre a runbook, és nem konvertálható egy runbook a másik típus van a létrehozása után meghatározhatja a runbook típusát.

PowerShell munkafolyamat runbooks és PowerShell runbooks a felhasználóknak, akik szívesebben közvetlenül dolgozhat a PowerShell-kódot vagy használja a szöveges szerkesztő az Azure automatizálás vagy egy offline programban, például a PowerShell ISE. Ha szeretne létrehozni egy PowerShell-munkafolyamat runbook ismerje meg az információkat, a jelen cikkben. 

A grafikus runbooks hozzon létre egy runbook tevékenységek és a parancsmagok használatáról, hanem grafikus csatolófelület, az alapul szolgáló PowerShell munkafolyamat bonyodalmainak elrejtő teszi lehetővé.  Ez a cikk, például pontjainak és a párhuzamos végrehajtás fogalmak érvényben grafikus runbooks, de nem kell aggódnia amiatt, hogy a részletes szintaxisa. 

## <a name="basic-structure-of-a-workflow"></a>Munkafolyamat felépítése

Egy PowerShell-parancsprogramot konvertálása PowerShell munkafolyamat első lépése van csatolva hozzá azt a **munkafolyamat** -kulcsszó.  Munkafolyamat a **munkafolyamat** kulcsszót a kapcsos zárójelek közé parancsfájlt törzsében kezdődik. Annak a munkafolyamatnak a nevére a **munkafolyamat** -kulcsszó követi, ahogy a következő szintaxissal. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Annak a munkafolyamatnak a nevére egyeznie kell az automatizálási runbook nevét. Ha a runbook importálnak, a fájlnév meg kell egyeznie a munkafolyamat, és .ps1 kell végződnie.

Paraméterek hozzáadni a munkafolyamat, használja a **paraméter** kulcsszó ugyanúgy, mint egy parancsfájl. 

## <a name="code-changes"></a>Kód módosításai

PowerShell munkafolyamat kód formátumban szinte azonos PowerShell parancsprogramot kód kivételével néhány jelentős módosításokat.  Az alábbi szakaszok ismertetik a módosításokat, amelyeket meg kell tenni egy PowerShell-parancsprogramot, futtassa az munkafolyamatokban.

### <a name="activities"></a>Tevékenységek

Egy tevékenység egy munkafolyamat egy adott tevékenységhez. Egy vagy több parancsok tevődik össze parancsfájlt, ahogyan munkafolyamat egy vagy több tevékenységek sorrendben végrehajtott tevődik össze. A Windows PowerShell-munkafolyamat automatikusan konvertálja a Windows PowerShell-parancsmagok számos tevékenységek munkafolyamat végrehajtásakor. Miután megadta a parancsmagok közül a runbook, a kapcsolódó tevékenység ténylegesen Windows folyamatkövető alaprendszer futtatni. Az adott parancsmagok megfelelő tevékenység nélkül a Windows PowerShell-munkafolyamat automatikusan elindul a parancsmag [InlineScript](#inlinescript) a tevékenységet. Van egy sor olyan tartoznak, és nem használhatók a munkafolyamat, kivéve, ha kifejezetten szerepeltetni kívánt őket egy InlineScript blokk parancsmagjai. E fogalmak a további részleteket lásd: [Parancsfájlt munkafolyamatok tevékenységek használatával](http://technet.microsoft.com/library/jj574194.aspx).

Munkafolyamat-tevékenységek oszthat meg egy sor olyan gyakori paraméterek működésük konfigurálása. A munkafolyamat közös paraméterek kapcsolatos részletekért olvassa el a [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx)című témakört.

### <a name="positional-parameters"></a>Pozíciójelző paraméterei

Pozíciójelző paraméterek tevékenységeket és parancsmagok az munkafolyamatokban nem használhatók.  A rendszer Ez azt jelenti, hogy paraméternevek kell használnia.

Például érdemes megvizsgálni a következő kódot, amely minden futó szolgáltatások kapja.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Ha a próbál meg az munkafolyamatokban futtassa a következő ugyanazt kódot, például "a paraméter megadása nem oldható használatával a megadott paramétereket nevű." üzenet vissza  A probléma elhárításához egyszerűen adja meg a paraméter neve, ahogy az alábbi.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deszerializált objektumok

A munkafolyamatokban objektumok deszerializálni vannak.  Ez azt jelenti, hogy a tulajdonságaik továbbra is elérhetők, de nem a módszereket.  Például érdemes megvizsgálni az alábbi PowerShell-kódot, amely megszünteti a Stop módszerrel a szolgáltatás objektum szolgáltatás.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Ha próbál meg ezt a munkafolyamatot futtatni, közli, hogy a "módszer meghívási nem támogatja a Windows PowerShell munkafolyamat" hibaüzenetet kapja meg.  

Egy lehetőség úgy veheti körül a következő két kódsorokat egy [InlineScript](#InlineScript) blokkon ebben az esetben $Service lenne a szolgáltatási objektumhoz Tiltás belül. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Egy másik, hogy hajt végre ugyanazokat a funkciókat, mint a módszerrel, egy másik parancsmag használja, ha ilyen.  A minta, amíg a Stop-szolgáltatás parancsmag funkcióját a Stop módszerrel is tartalmaz, és használhatja a következő is munkafolyamat.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

A **InlineScript** tevékenység akkor hasznos, ha a kell egy vagy több parancs futtatása hagyományos PowerShell-parancsprogramot PowerShell munkafolyamat helyett.  Munkafolyamat parancsok a Windows folyamatkövető alaprendszer feldolgozásra elküldi, miközben egy InlineScript blokkolása parancs dolgozza fel a Windows PowerShell. 

InlineScript használja az alábbi alább látható módon.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Egy InlineScript a kimeneti a kimeneti változó hozzárendelésével térhet vissza. A következő példa leállítja a szolgáltatást, és majd exportálja a szolgáltatás neve.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Értékek felkeresése egy InlineScript blokkon, de **$Using** hatókör módosító kell használnia.  A következő példa megegyezik az előző példában azzal a különbséggel, hogy a szolgáltatás neve változó által biztosított. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Előfordulhat, hogy bizonyos az munkafolyamatokban kritikus InlineScript tevékenységek, amíg nem támogatják a munkafolyamat tartalmazza, és csak az alábbi okok miatt szükség esetén használható:

- [Pontjainak](#Checkpoints) egy InlineScript blokkon belül nem használható. Ha hiba történik, a továbbfejlesztett fájlblokkolás belül, a továbbfejlesztett fájlblokkolás elején található akkor folytatódik.
- Nem használható a [párhuzamos végrehajtás](#parallel-execution) egy InlineScriptBlock belül.
- InlineScript méretezhetőség a munkafolyamat hatással van, mivel a Windows PowerShell-munkamenetet a InlineScript blokk teljes hosszának rendelkezik.

InlineScript használatáról további tudnivalókat talál a [Fut a Windows PowerShell-parancsait a munkafolyamat](http://technet.microsoft.com/library/jj574197.aspx) és [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>A párhuzamos feldolgozás

A Windows PowerShell-munkafolyamatok egyik előnye végrehajtása parancsok helyett párhuzamosan egymás után egy tipikus parancsfájlt, lehetősége. 

A **párhuzamos** kulcsszó segítségével egy parancsfájl építőelem létrehozása több a parancsok, amely egy időben futnak. Ez használ, az alábbi alább látható módon. Ebben az esetben Activity1 és Activity2 elkezdenek egy időben. Activity3 elkezdenek csak Activity1 és a Activity2 befejezése után.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Például érdemes megvizsgálni a következő több fájl másolása egy hálózati cél PowerShell-parancsait.  Ezek a parancsok egymás után futnak úgy, hogy egy fájl Befejezés másolása a következő megkezdése előtt.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

A következő munkafolyamat fut párhuzamosan ezek a parancsok, hogy az összes kezdik másolása egy időben.  Csak azt követően minden teljesen másolt az üzenet jelenik meg.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Használhatja a **ForEach-párhuzamos** szerkezet folyamat parancsok egyidejűleg egy webhelycsoport összes eleménél. A gyűjtemény elemeinek párhuzamosan feldolgozása közben parancsfájl Tiltás a parancs futtatása egymás után. Ez használ, az alábbi alább látható módon. Ebben az esetben Activity1 elkezdi a webhelycsoport összes eleme egyszerre. Minden elemhez Activity2 elkezdenek Activity1 befejeződése után. Activity3 elkezdenek csak azt követően Activity1 és a Activity2 befejeződött valamennyi elem esetében.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Az alábbi példa az előző példában fájlok másolása párhuzamosan hasonlít.  Ebben az esetben egy üzenet jelenik meg az egyes fájlok másolása után.  Csak azt követően minden teljesen másolja a végleges teljesítése üzenet jelenik meg.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Párhuzamosan futó gyermek runbooks, mivel ez azt mutatják mellékletei nem megbízható eredményt adhat, nem javasoljuk.  Előfordul, hogy a gyermek runbook kimenetét nem jelennek meg, és egy Gyermekszint runbook beállítások hatással lehetnek a többi párhuzamos gyermek runbooks 


## <a name="checkpoints"></a>Pontjainak

Egy *ellenőrzés* , amely tartalmazza az aktuális változók és érték bármely kimenet ponthoz során létre a munkafolyamat aktuális állapotát pillanatképét. Ha egy munkafolyamat a vágólapra a hiba, vagy fel van függesztve, majd a következő alkalommal, amikor futtatni elindul, a helyett a worfklow kezdésének az utolsó ellenőrzés.  Beállíthatja, hogy egy ellenőrzés az **Ellenőrzés-munkafolyamat** -tevékenység az munkafolyamatokban.

A következő példa kódot, a kivétel Activity2 után következik be okoz a munkafolyamat végén. Ha a munkafolyamatot újra fut, Activity2 futtatásával, mivel ez volt az utolsó ellenőrzés beállítása után kezdődik.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

A munkafolyamat pontjainak kell beállítani, miután tevékenységeket, lehet, hogy jobban a kivétel, és nem kell ismételni, ha a munkafolyamat folytatódik. Ha például a munkafolyamat hozhat létre, egy virtuális számítógépre. Állítsa a ellenőrzés előtt és után a parancsokat a virtuális gép létrehozásához. Ha létrehozása sikertelen, majd a parancsok szeretné ismételhetők meg, ha a munkafolyamat azonnal elindul, újra. Ha a a worfklow nem sikerül Miután létrejött a létrehozás, majd a virtuális gép nem jön létre újból, amikor a munkafolyamat folytatódik.

A következő példa több fájl másolása egy hálózati helyet és egy ellenőrzés állítja be a minden fájl után.  Ha a hálózati hely elvész, majd a munkafolyamat fejeződik hiba.  Indításakor rá újra, ami azt jelenti, csak azokat a fájlokat már kimásolt kihagyja az utolsó ellenőrzés folytatódik.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

A username hitelesítő vannak nem állandó, hívja a [Felfüggesztett-munkafolyamat](https://technet.microsoft.com/library/jj733586.aspx) -tevékenység vagy az utolsó ellenőrzés után, mert kell beállítania a NULL értékű, valamint kattintson ismét az eszköz áruházából **Felfüggesztett-munkafolyamat** vagy ellenőrzés hívása után a hitelesítő adatait.  Egyéb esetben az alábbi hibaüzenet jelenhet meg: A munkafolyamat-feladat *nem folytatható, vagy mert adatmegőrzési adatok nem teljesen mentve, vagy nem mentett adatmegőrzési adatok megsérült. A munkafolyamatot újra kell indítania.*

A következő ugyanazt kódot bemutatja, hogyan kezelheti a a PowerShell munkafolyamat runbooks.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Ez nem kötelező, ha a program hitelesíti a fiókkal Futtatás más néven egyszerű szolgáltatás konfigurált.  

Pontjainak kapcsolatos további tudnivalókért lásd: [Pontjainak hozzáadása a parancsprogram-munkafolyamatot](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Következő lépések

- Első lépések a PowerShell munkafolyamat runbooks, lásd: [az első PowerShell munkafolyamat runbook](automation-first-runbook-textual.md) 
