<properties
    pageTitle="Mi az adatok tudományos virtuális gép? | Microsoft Azure"
    description="Ismerje meg az esetek, funkciókat, és az adatok tudományos virtuális gépeken futó, egy környezetben, és készen áll a analytics eszközkészlet lépések."
    keywords="tudományos Adateszközök, adatok tudományos virtuális gép, az adatok science linux adatok tudományos eszközök"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="bradsev" />


# <a name="introduction-to-the-cloud-based-data-science-virtual-machine-for-linux-and-windows"></a>Bevezetés a felhőalapú adatok tudományos virtuális gépen Linux és a Windows

Az adatok tudományos virtuális gép egy testre szabott virtuális képe, a Microsoft Azure felhő épített kifejezetten adatok tudományos módon. Számos népszerű adatok tudomány és egyéb eszközök előtelepítve és előre konfigurált jump-start fejlett analitikai az intelligens alkalmazások létrehozásába rendelkezik. Érhető el a Windows Server 2012 vagy Linux OpenLogic 7.2 CentOS-alapú verzióján. 

Ez a témakör azt ismerteti, hogy mit tehet az adatok tudományos virtuális tartalmazó, az esetek a virtuális használatának néhány ismertet, részletezi érhető el ablakok és Linux verzióin fontosabb funkciói és ismertető első lépésiről őket.


## <a name="what-can-i-do-with-the-data-science-virtual-machine"></a>Mire van lehetőségem az adatok tudományos virtuális gépen?

A cél, az adatok tudományos virtuális gép, adja meg az összes szakértelem szintek és szerepkörök adatok szakemberei súrlódás ingyenes adatok tudományos környezettel rendelkező. A virtuális jelentős Ha, kellett közzétételének egy hasonló környezet saját maga szeretné töltött időt takaríthat meg. Ehelyett a adatok tudományos projekt azonnal elindul egy újonnan létrehozott virtuális példány. 

Az adatok tudományos virtuális kialakításának és be van állítva egy széles használatát esetek használata. Méretezheti a környezet felfelé vagy lefelé a projekt igények változásával. Ön a program adatok tudományos tevékenységekhez preferált nyelvet használhatja. Egyéb eszközök telepítése, és a rendszer testre a pontos igényeinek megfelelően.
 
## <a name="key-scenarios"></a>Esetek
Ebben a részben néhány, amelynek a adatok tudományos virtuális telepíthető esetek javasolja.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Előre beállított analytics asztali a felhőben

Az adatok tudományos virtuális adatok tudományos csapatok helyi Asztalukat cserélje ki egy felügyelt felhő asztali szeretné egy eredeti konfigurációt biztosít. Ez az eredeti gondoskodhat arról, hogy a csapat adatok tudósok egy következetes beállítása, amellyel kísérletek ellenőrzése és együttműködési előléptetése. A rendszergazdák terhet csökkentése, és mentse a felmérése, telepítése, és a különböző szoftvercsomagok fejlett analitikai végrehajtásához szükséges karbantartása szükséges idő költségek is lejjebb.  

### <a name="data-science-training-and-education"></a>Adatok tudományos képzés és oktatás

Vállalati oktatók és oktatók, amely lapon adatok tudományos osztályok általában egy virtuális gép kép gondoskodhat arról, hogy a diákok egy következetes beállítást adja meg, és a minták előre működő. Az adatok tudományos virtuális igény szerinti környezetben, amely megkönnyíti a támogatási és nem kompatibilis problémáit egységes beállításokkal hoz létre. Ha ilyen környezetben kell különösen a dolgozók rövidebb oktatás osztályok, gyakran beépített esetek jelentősen összekapcsolhatók.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Igény szerinti rugalmas kapacitás nagyméretű projektekhez

Adatok tudományos hackathons/versenyek vagy nagyméretű adatmodellezés és feltárása megkövetelése a hardver beosztását, általában rövid ideig, amelyekkel méretarányos. Az adatok tudományos virtuális segítségével, hogy bizonyos adatok tudományos környezetre gyorsan igény, méretezett meg, amelyek lehetővé teszik a nagy teljesítményű számítógépes erőforrásokat fog futni igénylő kísérletek kiszolgálókon.

### <a name="short-term-experimentation-and-evaluation"></a>Rövid érvényességi idejű kísérletezés és értékelése

Az adatok tudományos virtuális lehet felmérése, vagy ismerje meg, például Microsoft R Server, SQL Server eszközök használt Visual Studio eszközök, Jupyter, mély tanulási / Machine Learning eszközök gazdag, és a népszerű közösségi minimális és új eszközöket beállítása munkamennyiség. Az adatok tudományos virtuális beállítható úgy gyorsan, mivel, például a közzétett kísérletek, a bemutatók, a következő forgatókönyvek online munkamenetek vagy konferencia oktatóanyagok végrehajtása replikálása más rövid érvényességi idejű használatát esetben alkalmazható.


## <a name="whats-included-in-the-data-science-vm"></a>Az adatok tudományos virtuális található?

Az adatok tudományos virtuális gép tartalmaz számos népszerű tudományos Adateszközök nincs telepítve és beállítva. Eszközök, amelyek megkönnyítik a készült különböző Azure adatok és az elemzést termékek is tartalmaz. Ismerje meg, és a prediktív modellek épülnek nagyméretű adatkészletek Microsoft R kiszolgálót használ, vagy SQL Server 2016 használatának. Egyéb eszközök a szolgáltató, a Megnyitás közösségi és a Microsoft is része, valamint a minta kódot, valamint a jegyzetfüzetet. Az alábbi táblázat részletezi, és miben más a Windows és Linux kiadásokban, az adatok tudományos virtuális számítógépen található a fő összetevői.


|**Windows Edition** | **Linux Edition** |
|----------------|---------------|
|Microsoft R Server Developer Edition | Microsoft R Server Developer Edition |
|Anaconda Python 2.7, 3.5-ös | Anaconda Python 2.7, 3.5-ös |
|Jupyter jegyzetfüzet kiszolgáló (R, Python) | JupyterHub: Több felhasználó Jupyter jegyzetfüzetek (R, Python, Ágnes) |
|Az SQL Server 2016 Fejlesztőeszközök Edition: Méretezhető adatbázis-elemző R szolgáltatásokkal | Postgres, erdeimókus SQL (adatbázis-kezelő eszköz), az SQL Server-illesztőprogramok és parancssori (bcp, sqlcmd) |
|Visual Studio közösségi Edition 2015 (IDE) </br> -Azure hdinsight szolgáltatáshoz (Hadoop), adatok tóból, SQL Server Data tools </br> -Node.js Python és R tools for Visual Studio |IDEs és szerkesztők </br> -Holdas az Azure eszközkészlet beépülő modul </br> -(Ha az ESS, auctex) Emacs gedit |
|A Power BI desktop | -- |
|Gépi tanulási eszközök </br> -Azure gépi tanulási integráció </br> -CNTK mély tanulási/AI) </br> -Xgboost (az adatok tudományos versenyek népszerű Machine Learning eszköz) </br> -Vowpal Wabbit (gyors online tanuló) </br> -Rattle (vizuális rövid adatok és analytics eszköz) </br> -Mxnet mély tanulási/AI) | Gépi tanulási eszközök </br> -Az Azure gépi tanulási integrációs </br> -CNTK mély tanulási/AI) </br> -Xgboost (az adatok tudományos versenyek népszerű Machine Learning eszköz) </br> -Vowpal Wabbit (gyors online tanuló) </br> -Rattle (vizuális rövid adatok és analytics eszköz)  |
| Azure és a Cortana üzletiintelligencia-csomagja szolgáltatások eléréséhez SDK | Azure és a Cortana üzletiintelligencia-csomagja szolgáltatások eléréséhez SDK |
| Adatok mozgását és Azure és nagy adatok erőforrások kezelésére szolgáló eszközök: Azure tároló Explorer CLI, PowerShell, AdlCopy (Azure adatok tó), AzCopy, dtui (a DocumentDB), a Microsoft adatkezelési átjáró | Adatok mozgását és Azure és nagy adatok erőforrások kezelésére szolgáló eszközök: Azure tároló Explorer, a CLI |
| Mely számjegy, Visual Studio Team Services beépülő modul | Mely számjegy |
| A leggyakrabban használt Linux/Unix parancssori segédprogramok GitBash: parancssor keresztül érhető el Windows port | -- |



## <a name="how-to-get-started-with-the-windows-data-science-vm"></a>Hogyan kell a Windows adatok tudományos virtuális – első lépések

- A virtuális példányának létrehozása a Windows Navigálás [erre](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) a lapra, és válassza a zöld **virtuális gép létrehozása** gombra.
- Jelentkezzen be a virtuális távoli asztali változatában a virtuális létrehozásakor megadott hitelesítő adataival.
- Fedezze fel, és indítsa el a rendelkezésre álló eszközök, kattintson a **Start** menü.


## <a name="get-started-with-the-linux-data-science-vm"></a>A Linux adatok tudományos virtuális – első lépések

- Hozzon létre egy példányát a virtuális Linux (OpenLogic CentOS-alapú) Navigálás [erre](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/) a lapra, és válassza a **virtuális gép létrehozása** gombra.
- Jelentkezzen be a virtuális SSH ügyfélről, például gitt vagy SSH parancs, a virtuális létrehozásakor megadott hitelesítő adataival.
- Rendszerhéj kérdésnél írja be a dsvm-több-információ.
- Grafikus asztali, töltse le az ügyfél platform X2Go ügyfél [ide](http://wiki.x2go.org/doku.php/doc:installation:x2goclient) , és kövesse az utasításokat a Linux adatok tudományos virtuális dokumentumban [Linux adatok tudományos virtuális gép rendelkezni](machine-learning-data-science-linux-dsvm-intro.md#installing-and-configuring-x2go-client).


## <a name="next-steps"></a>Következő lépések

### <a name="for-the-windows-data-science-vm"></a>A Windows-adatok tudományos virtuális számára

- Elérhető adott eszköz futtatásával Windows verziójától kapcsolatos további tudnivalókért lásd: [a Microsoft adatok tudományos virtuális gép rendelkezés](machine-learning-data-science-provision-vm.md) és
-  További információt olvashat a Windows virtuális az adatok tudományos projekt szükséges változatos műveleteket hajthat végre olvassa el a [tíz dolgot is tehet az adatok tudományos virtuális gép](machine-learning-data-science-vm-do-ten-things.md)című témakört.

### <a name="for-the-linux-data-science-vm"></a>Az adatok Linux tudományos virtuális számára

- Adott elérhető eszköz futtatásával Linux verzión kapcsolatos további tudnivalókért lásd: [a Linux adatok tudományos virtuális gép rendelkezni](machine-learning-data-science-linux-dsvm-intro.md).
- Megtudhatja, hogy miként néhány gyakori adatok tudományos feladatok elvégzéséhez a Linux virtuális útmutató olvassa el a [adatok tudományos a Linux adatok tudományos virtuális gépen](machine-learning-data-science-linux-dsvm-walkthrough.md)című témakört.
