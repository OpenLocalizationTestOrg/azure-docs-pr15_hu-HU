<properties
    pageTitle="Az első PowerShell runbook az Azure automatizálás |} Microsoft Azure"
    description="Amelyek végigvezetik Önt a létrehozási, tesztelés, és egy egyszerű PowerShell runbook a közzététel oktatóprogram."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Azure powershell, powershell parancsprogramot oktatóprogram, powershell automatizálást"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="magoedte;sngun"/>

# <a name="my-first-powershell-runbook"></a>Az első PowerShell runbook

> [AZURE.SELECTOR] - [A grafikus](automation-first-runbook-graphical.md) - [PowerShell](automation-first-runbook-textual-PowerShell.md) - [PowerShell-munkafolyamat](automation-first-runbook-textual.md)  

Ebben az oktatóanyagban végigvezeti a [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) az Azure automatizálás létrehozását. Lássuk először egy egyszerű runbook, hogy miként vizsgálat, és közben azt ismertetik a runbook feladat állapotának nyomon követéséhez tegye közzé a. Azt fogja módosítsa a runbook ténylegesen kezelheti az Azure erőforrásokat, ebben az esetben kezdve az Azure virtuális gépen. Azt fogja végezze el a runbook megbízhatóbb runbook paraméterek hozzáadásával.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban befejezéséhez szüksége lesz az alábbi.

-   Azure előfizetés. Ha egy még nem rendelkezik, akkor [az MSDN előfizetői előnyeinek aktiválása](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) vagy a <a href="/pricing/free-account/" target="_blank"> [ingyenes fiókot regisztrálni](https://azure.microsoft.com/free/).
-   [Automatizálási fiók](automation-security-overview.md) tartsa lenyomva az ujját a runbook és Azure erőforrásokhoz hitelesítést végezni.  Ehhez a fiókhoz elindítása és leállítása a virtuális gép engedéllyel kell rendelkeznie.
-   Azure virtuális gépen. Hogy leállítása, és indítsa el az ezen a számítógépen, így nem kell lennie a termelési.

## <a name="step-1---create-new-runbook"></a>Lépés: 1 – új runbook létrehozása

Lássuk először: hozzon létre egy egyszerű runbook, hogy a szöveg *Helló, világ*exportálja.

1.  Az Azure-portálon nyissa meg az automatizálási fiókját.  
    Automatizálási fióklapjának elemre koppintva az erőforrások gyors áttekintést ehhez a fiókhoz. Egyes eszközök már rendelkeznie kell. A táblázatparancsok nagy része automatikusan bekerülnek az új fiók automatizálási modulokat. Rendelkeznie kell is szerepel a [Előfeltételek](#prerequisites)hitelesítő digitális eszköz kiválasztása.
2.  Kattintson a nyissa meg a listát a runbooks **Runbooks** csempére.  
    ![RunbooksControl](media/automation-first-runbook-textual-powershell/automation-runbooks-control.png)  
3.  Hozzon létre egy új runbook a **egy runbook Hozzáadás** gombra, majd a **Létrehozás egy új runbook**gombjára kattintva.
4.  Nevezze el a runbook a *MyFirstRunbook-PowerShell*.
5.  Ebben az esetben megyünk hozzon létre egy [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) , jelölje be a **Powershell** **Runbook**típusra.  
    ![Runbook típusa](media/automation-first-runbook-textual-powershell/automation-runbook-type.png)  
6.  Kattintson a **Létrehozás** hozhat létre a runbook, és nyissa meg a szöveges szerkesztőben.

## <a name="step-2---add-code-to-the-runbook"></a>Lépés: 2 - kód hozzáadása a runbook

Bármelyik kódját közvetlenül a runbook is, vagy válassza ki a tár vezérlő parancsmagok, runbooks és eszközök, és azokat a runbook kapcsolódó paramétereket az adott hozzá. Ez a forgatókönyv azt fogja írja be közvetlenül a runbook.

1.  A runbook jelenleg üres lesz, típus *írási-kimeneti "Helló, világ."*.  
    ![helló világ](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2.  Mentse a runbook **mentése**gombra kattintva.  
    ![Mentés gomb](media/automation-first-runbook-textual-powershell/automation-save-button.png)  

## <a name="step-3---test-the-runbook"></a>3 - próba a runbook lépés

Mielőtt közzétesszük a runbook gyártási elérhetővé szeretné tenni, szeretnénk tesztelése, hogy győződjön meg arról, hogy helyesen működik-e. Amikor egy runbook teszteléséhez a **Piszkozat** verzióját futtatja, és interaktív módon tekintheti meg a kimenetét.

1.  **Teszt ablakban** a próba-ablak megnyitása gombra.  
    ![Teszt ablaktábla](media/automation-first-runbook-textual-powershell/automation-testpane.png)  
2.  Kattintson a **Start** indítsa el a tesztet. A csak engedélyezett lehetőséget kell.
3.  A [runbook feladat](automation-runbook-execution.md) jön létre, és állapota jelenik meg.  
    A feladat állapota *várólistás* jelző Várakozás hamarosan érhető el a felhőben runbook dolgozó induljon el fog. *Kezdő* majd azt helyezi át, amikor egy dolgozó állítja a feladatot, majd a *operációs rendszert futtató* indításakor a runbook ténylegesen operációs rendszert futtató.  
4.  A runbook feladat befejezésekor jelenik meg az eredményt. Ebben az esetben azt jelennie *Helló, világ*  
    ![Tesztelje a kimeneti ablakban](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
5.  A próba-való visszatéréshez a vászonra ablak bezárása

## <a name="step-4---publish-and-start-the-runbook"></a>Lépés: 4 - közzé, és indítsa el a runbook

Az imént létrehozott runbook továbbra is a vázlat módban van. A közzétételre, mielőtt azt is futtathatja a gyártás szükséges. Egy runbook közzétételekor piszkozat verziójával felülírják a meglévő közzétett verzió. Ebben az esetben azt a közzétett verzió még nincs, mert az imént létrehozott a runbook.

1.  Kattintson a **Közzététel** a runbook közzététele, majd az **Igen** .  
    ![Közzététel gomb](media/automation-first-runbook-textual-powershell/automation-publish-button.png)  
2.  Ha balra a runbook most megtekintése a **Runbooks** ablakban görgessen le, akkor **közzétett** **Állapotát szerzői** jeleníti meg.
3.  Görgetés az ablakban **MyFirstRunbook -**PowerShell jobbra.  
    Is, a képernyő tetején a beállítások lehetővé számunkra, hogy a Start menü a runbook megtekintése a runbook, ütemezze a kezdő sorszám egy kis időt, a jövőben, vagy hozzon létre egy [webhook](automation-webhooks.md) , így indítható el a HTTP-hívás keresztül.
4.  Szeretnénk egyszerűen indítsa el a runbook, kattintson a **Start** gombra, és ezután kattintson az **OK gombra** a Start Runbook lap megnyitásakor.  
    ![Start gomb](media/automation-first-runbook-textual-powershell/automation-start-button.png)  
5.  A feladat ablaktáblában a runbook feladat, amely az imént létrehozott nyitja meg. Azt is zárja be az ablaktábla, de ebben az esetben azt fogja hagyja nyitva, akkor megnézheti, hogy a feladat előrehaladását.
6.  A feladat állapota a **Projekt összefoglaló** jelenik meg, és azt a runbook vizsgálva bekerül az állapotok megegyezik.  
    ![Projekt összefoglalása](media/automation-first-runbook-textual-powershell/automation-job-summary.png)  
7.  Miután a runbook állapota *kész*, kattintson a **kimeneti**. Nyitja meg a kimeneti ablakban, és azt láthatja, hogy a *Helló, világ*.  
    ![Feladat kimeneti](media/automation-first-runbook-textual-powershell/automation-job-output.png)
8.  Zárja be a kimeneti ablakban.
9.  Kattintson **Az összes naplók** a runbook projektre vonatkozóan a adatfolyamok ablak megnyitásához. Csak a kimeneti adatfolyam *Helló, világ* kell láthatja, de ez más adatfolyamok runbook feladat például részletes és hiba megjelenítheti a őket a runbook ír.  
    ![Az összes naplók](media/automation-first-runbook-textual-powershell/automation-alllogs.png)  
10. Zárja be a adatfolyam megjelenítését és a feladat ablaktábla, a MyFirstRunbook-PowerShell-ablaktáblán való visszatéréshez.
11. Kattintson a **feladatok** kattintva nyissa meg a feladatok ablakban a runbook. Az e runbook által létrehozott feladatok listája. Csak egy feladatot szerepel a felsorolásban, mivel a probléma csak adódott a feladat egyszer kell láthatja.  
    ![Feladatlista](media/automation-first-runbook-textual-powershell/automation-job-list.png)  
12. A feladat az azonos feladat munkaablak, amely azt nézett meg azt a runbook indításakor választhatja. Lehetővé teszi, hogy térjen vissza az időt, és minden feladat, amely az egy adott runbook készült részleteinek megtekintését.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Lépés az 5 - hitelesítés Azure erőforrások hozzáadása

Azt már vizsgálni, és címjegyzékemben runbook közzé, de az eddigi azt nem bármit hasznos. Azure erőforrások telepítve szeretnénk. Teheti, kivéve, ha van rá hitelesíteni a [Előfeltételek](#prerequisites)hivatkozni hitelesítő adatokkal azonban nem. Azt a érheti el, amely a **Hozzáadás-AzureRmAccount** parancsmag.

1.  A PowerShell MyFirstRunbook munkaablakban **szerkesztése** gombra kattintva nyissa meg a szöveges szerkesztőben.  
    ![Runbook szerkesztése](media/automation-first-runbook-textual-powershell/automation-edit-runbook.png)  
2.  Az **írás** sorrá már nem szükséges, így jóváhagyást, és törölje azt.
3.  Írja be vagy illessze be a következő kódot, amely a hitelesítés, a Futtatás automatizálási más néven fiókjával fogja kezelni:

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ``` 
<br>
4.  Kattintson a **ablaktábla tesztelése** , hogy azt tesztelheti a runbook.
5.  Kattintson a **Start** vizsgálat indításához. Ha befejeződött, a kimeneti hasonlóan, mint a következő, a Megjelenítés az alapvető információk a fiókjából kell kapnia. Ez megerősíti, hogy a hitelesítő adatok érvényes. <br> ![Hitelesítés](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Lépés a 6 - kód egy virtuális gép indítása

Most, hogy a runbook hitelesíti az Azure-előfizetéséhez, azt is kezelheti az erőforrásokat. Adunk virtuális gép indítása parancsot. Virtuális gépi is választhat az Azure-előfizetése, és most parancsmag be nevet hardcoding azt is.

1.  *Hozzáadás-AzureRmAccount*, után írja be a *Start-AzureRmVM-neve "VMName" - ResourceGroupName "NameofResourceGroup"* kezeléséről a és a virtuális gép indítása erőforráscsoport nevét.  
    
    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
     ```
<br>
2.  Mentse a runbook, és kattintson az **ablak tesztelése** , hogy azt tesztelheti.
3.  Kattintson a **Start** indítsa el a tesztet. Ha befejeződött, ellenőrizze, hogy a virtuális gép indított.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Lépés a 7 – a bemeneti paraméterre hozzáadása a runbook

A runbook jelenleg elindítja a virtuális számítógép-e azt a runbook a szoftveresen kötött, de több hasznos lenne, ha azt megadhatja a virtuális gép a runbook indításakor. Most már hozzáadja azt, hogy funkciókat nyújtson a runbook bemeneti paramétereket.

1.  Paraméterek *VMName* és *ResourceGroupName* hozzáadása a runbook, és ezek a változók használata a **Kezdés-AzureRmVM** parancsmag, ahogy az alábbi példában.  
    
    ```
    Param(
       [string]$VMName,
       [string]$ResourceGroupName
    )
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint 
     Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
     ```
<br> 
2.  A runbook mentse, és nyissa meg a próba ablaktáblát. Látható, hogy most értékek biztosíthat a két bemeneti változót a próba használt.
3.  A próba-ablak bezárása
4.  Kattintson a **Közzététel** a runbook új verziójának közzététele.
5.  Állítsa le a virtuális gép, Ön által indított az előző lépésben.
6.  Kattintson a **Start** a runbook indítása gombra. Írja be a **VMName** és **ResourceGroupName** a virtuális gép, amely szeretné elindítani.  
    ![A sikeres paraméter](media/automation-first-runbook-textual-powershell/automation-pass-params.png)  
7.  A runbook befejezése után ellenőrizze, hogy a virtuális gép indított.

## <a name="differences-from-powershell-workflow"></a>Eltérések a PowerShell-munkafolyamat

PowerShell runbooks ugyanazon életciklus, funkciók és kezelése a PowerShell munkafolyamat runbooks másként rendelkezik, de néhány különbségek vannak és korlátai:

1.  A PowerShell runbooks gyors futtatásához PowerShell munkafolyamat runbooks képest, az összeállítás lépés nincs is.
2.  PowerShell munkafolyamat runbooks pontjainak, pontjainak használatával támogatja, mivel PowerShell runbooks csak folytathatja az elejétől PowerShell munkafolyamat runbooks folytathatja a a runbook bármely pontjára.
3.  PowerShell munkafolyamat runbooks támogatja a párhuzamos és a soros végrehajtás, mivel PowerShell runbooks is csak parancsok végrehajtása sorozatban.
4.  Egy PowerShell-munkafolyamat runbook a tevékenységet, egy parancs vagy egy parancsfájlt szövegrészt beállíthatja, hogy a saját runspace, mivel az egy PowerShell runbook mindent parancsfájlt egy egyetlen runspace fut. A natív PowerShell runbook és egy PowerShell-munkafolyamat runbook közötti [különbségek szintaktikai](https://technet.microsoft.com/magazine/dn151046.aspx) is vannak.

## <a name="next-steps"></a>Következő lépések

-   Első lépések a grafikus runbooks, lásd: [az első grafikus runbook](automation-first-runbook-graphical.md)
-   Első lépések a PowerShell munkafolyamat runbooks, lásd: [az első PowerShell munkafolyamat runbook](automation-first-runbook-textual.md)
-   Szeretne többet megtudni a runbook típusú, a előnyei és korlátai, lásd: [Azure automatizálási runbook típusai](automation-runbook-types.md)
-   További információt a PowerShell-parancsprogramot támogatja a szolgáltatást című témakörben [található automatizálás Azure támogatja natív PowerShell-parancsprogramot](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)
