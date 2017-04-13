<properties
    pageTitle="Mi az Azure automatizálást |} Microsoft Azure"
    description="Megtudhatja, milyen Azure automatizálási tartalmaz értéket, és válaszok a gyakori kérdésekre, hogy a kezdéshez hoz létre, a runbooks és Azure automatizálási DSC használatával."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
    keywords="Mi az automatizálás, azure automatizálási és azure automatizálási példák"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="05/10/2016"
    ms.author="magoedte;bwren"/>

# <a name="azure-automation-overview"></a>Azure automatizálási – áttekintés

Microsoft Azure automatizálási a felhasználók a felhőben, és a vállalati környezetben gyakran végrehajtott kézi, hosszabb ideig futó, hiba jobban és gyakran ismételt feladatok automatizálása megoldást. Időt takaríthat meg, és növeli a normál felügyeleti feladatok megbízhatóságát és még ütemez rendszeres időközönként automatikusan elvégzendő őket. Runbooks használatával folyamatok automatizálása, vagy automatizálni kívánt állam konfigurációval a modellkonfigurációk kezelésére. Ez a cikk rövid áttekintést nyújt a Azure automatizálási, és néhány gyakori kérdésekre ad választ. További cikkek a tárban, a különböző témakörök tartalmaznak további részletes információkat is hivatkozhat.


## <a name="automating-processes-with-runbooks"></a>Runbooks folyamatok automatizálása

Egy runbook egy olyan sorolható feladatokat végeznek néhány automatizált eljárással az Azure automatizálás. Lehet, hogy egy egyszerű folyamat, például egy virtuális gép kezdési és egy naplóbejegyzés létrehozása, vagy előfordulhat, hogy egy összetett runbook és végrehajtásához, összetett folyamat több erőforrások vagy még több felhő között van környezetekkel kapcsolatos egyéb kisebb runbooks kombináló.  

Például, előfordulhat, hogy van egy meglévő a kézi végrehajtás az SQL-adatbázis csonkítása, ha megközelíti maximális mérete, amely több segít a csatlakozás a kiszolgálóhoz, például a adatbázishoz való csatlakozáskor, az aktuális adatbázis mérete beszerzése, ellenőrizze, hogy ha küszöb túllépte majd rövidítendő szám, akkor és felhasználó értesítése. Manuális hajt végre, ezeket a lépéseket mindegyik, helyett úgy lehetett létrehozni egy runbook, az alábbi műveleteket minden végrehajt, egyetlen folyamat. Azt, akinek a runbook indítása, adja meg a szükséges információkat, például a SQL kiszolgáló nevét, az adatbázis neve és a címzett e-mail és vissza, miközben a folyamat befejeződik, majd elhelyezkedik. 


## <a name="what-can-runbooks-automate"></a>Mi a runbooks automatizálhatja?

Az Azure automatizálást Runbooks alapuló Windows PowerShell vagy a Windows PowerShell-munkafolyamatok ugyanúgy működnek, hogy mindent PowerShell hajthatja végre, így. Ha egy alkalmazás vagy szolgáltatás egy API-t, majd a runbook dolgozhat azt. Ha az alkalmazás egy PowerShell-modult, meg, hogy a modul betöltése Azure automatizálást és adott parancsmagok szerepeltetni a runbook. Azure automatizálási runbooks futtassa az Azure felhőben, és bármely felhő erőforrások, illetve a felhőből is elérhető külső erőforrások érhetők el. [Hibrid Runbook dolgozó](automation-hybrid-runbook-worker.md)használ, runbooks futtatását is lehetővé teszi a helyi adatok központban helyi erőforrások kezelésére. 


## <a name="getting-runbooks-from-the-community"></a>Ahhoz, hogy a közösségi runbooks

A [Runbook gyűjtemény](automation-runbook-gallery.md#runbooks-in-runbook-gallery) a Microsoft runbooks tartalmazza, és a Közösség, hogy a vagy használata is változatlan marad a környezetben, illetve testre szabhatja őket a saját céljából. Elérik azt is megtudhatja, hogy miként hozhat létre saját runbooks hivatkozásként hasznos. Saját runbooks a dokumentumtárba, hogy úgy gondolja, hogy más felhasználók hasznosak is közreműködhet. 


## <a name="creating-runbooks-with-azure-automation"></a>Azure automatizálási Runbooks létrehozása 

[A saját runbooks létrehozása](automation-creating-importing-runbook.md) előzmények nélkül is vagy runbooks a [Runbook gyűjtemény](http://msdn.microsoft.com/library/azure/dn781422.aspx) az saját igényeinek megfelelően módosíthatja. Nincsenek három különböző [típusú runbook](automation-runbook-types.md) választhat PowerShell felület és a követelmények alapján. Ha szeretne közvetlenül dolgozhat a PowerShell-kódot, majd használhatja egy [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) vagy szerkesztésekor offline vagy a [szöveges szerkesztő](http://msdn.microsoft.com/library/azure/dn879137.aspx) az Azure-portálon [PowerShell munkafolyamat runbook](automation-runbook-types.md#powershell-workflow-runbooks) . Ha inkább egy runbook szerkesztheti anélkül, hogy a mögöttes kóddal kitéve, majd létrehozhat egy [grafikus runbook](automation-runbook-types.md#graphical-runbooks) , a [grafikus szerkesztő](automation-graphical-authoring-intro.md) használata az Azure-portálon. 

Inkább az olvasóablak szolgáltatásra? Van egy pillantást a Microsoft Ignite munkamenetből május 2015 a videó alatti. Megjegyzés: A fogalmak és ebben a videóban bemutatott funkciók helyességét, Azure automatizálási fejlődött sokkal mivel ez a videó lett felvéve, miközben meg most szélesebb körű kezelőfelülete az Azure-portálon, és további lehetőségeket támogatja.

> [AZURE.VIDEO microsoft-ignite-2015-automating-operational-and-management-tasks-using-azure-automation]


## <a name="automating-configuration-management-with-desired-state-configuration"></a>Megpróbálja automatizálni kívánt állam konfigurációval a modellkonfigurációk kezelésére 

[PowerShell DSC](https://technet.microsoft.com/library/dn249912.aspx) egy management platform, amely lehetővé teszi, hogy kezelése, üzembe helyezéséhez és a hivatkozási fizikai hosts és a virtuális gépeken futó deklaráció PowerShell szintaxissal beállítását. A központi DSC lekérés kiszolgálón, amely a cél gépek automatikusan beolvasásához és alkalmazhatók a konfigurációk adhatja meg. DSC biztosít PowerShell-parancsmagok konfigurációk és erőforrások kezelésére használható.  

[Azure automatizálási DSC](automation-dsc-overview.md) a vállalati környezetben szükséges szolgáltatásokat nyújtó PowerShell DSC felhőalapú megoldást.  Az Azure automatizálási DSC erőforrások kezelésére, és a beállítások alkalmazása beolvasni a kiszolgálóról DSC húzza a felhőben Azure virtuális vagy a fizikai gépek.  Jelentéskészítési szolgáltatások, például a fontos eseményeket csomópontok a tevékenységhez rendelt konfiguráció van deviated, illetve új konfiguráció alkalmazása tájékoztatni is tartalmaz. 


## <a name="creating-your-own-dsc-configurations-with-azure-automation"></a>A saját DSC konfigurációk létrehozása az Azure automatizálási

[DSC konfigurációk](automation-dsc-overview.md#azure-automation-dsc-terms) adja meg a kívánt állapota csomópontot.  Több csomópontokat is alkalmazhatja ugyanazt a konfigurációt, hogy biztosítsa, hogy azok minden állapotban egy azonos.  Szerkesztő használata a benne lévő szöveg konfiguráció létrehozása a helyi számítógépen, és majd importálnia kell Azure automatizálási, ahol állíthat össze, és alkalmazza csomópontok.


## <a name="getting-modules-and-configurations"></a>Az első modulok és beállítások 

[PowerShell-modult](automation-runbook-gallery.md#modules-in-powershell-gallery) tartalmazó-runbooks és DSC konfigurációk a [PowerShell gyűjtemény](http://www.powershellgallery.com/)használható parancsmagokról elérheti. Indítsa el az ebben a gyűjteményben az Azure portálról, és modulok importálása a közvetlenül az Azure automatizálási, vagy töltse le és a manuális importálhatja őket. Nem telepíthető a modulokat közvetlenül az Azure portálról, de letöltheti őket bármely más modul módon telepítse azokat. 


## <a name="example-practical-applications-of-azure-automation"></a>Példa Azure automatizálási gyakorlati alkalmazásai 

Az alábbiakban néhány példa Mik azok az Azure automatizálási automatizálást típusú. 

* Hozzon létre, és másolja a vágólapra virtuális gépeken futó különböző Azure előfizetések. 
* Ütemezés fájl egy Azure Blob-tárolóhoz tároló egy helyi számítógépre másolja át. 
* Biztonsági funkciókat, például: megtagadja kér egy ügyfél egy megtagadását támadások észlelésekor automatizálása. 
* Győződjön meg róla, gépek folyamatosan zárt beállítást a beállított biztonsági házirendje.
* Alkalmazás kódja a folyamatos telepítésének kezelése a felhő között, majd a helyi infrastruktúra. 
* Laboratóriumi környezetben az Azure Active Directory-erdő összeállítása. 
* SQL-adatbázis táblájához csonkolja, ha DB közelítő maximális méretét. 
* Távolról frissítse az Azure webhely környezeti beállításokat. 


## <a name="how-does-azure-automation-relate-to-other-automation-tools"></a>Hogyan kapcsolódnak Azure automatizálási más automatizálási eszközöket?

Magánjellegű felhőbeli adatkezelési feladatok automatizálása [Szolgáltatás felügyeleti automatizálást (SMA)](http://technet.microsoft.com/library/dn469260.aspx) szolgál. Telepítés helyileg az Adatközpont a [Microsoft Azure csomag](https://www.microsoft.com/en-us/server-cloud/)részeként. SMA és Azure automatizálási használata ugyanabban a Windows PowerShell és a Windows PowerShell-munkafolyamatok alapján runbook formátumban, de SMA nem támogatja a [grafikus runbooks](automation-graphical-authoring-intro.md).  

A helyszíni erőforrások automatizálási [System Center 2012 Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) szól. Különböző runbook alapértelmezettől Azure automatizálást és szolgáltatás felügyeleti automatizálást használ, és hozhat létre runbooks bármely a parancsfájlok használatát anélkül, a grafikus felületen van. A runbooks tevékenységek való integráció csomagok kifejezetten Orchestrator írt tevődnek össze. 


## <a name="where-can-i-get-more-information"></a>Hol kaphatok további információt? 

A különböző erőforrások érhetők el, ha többet szeretne megtudni az Azure automatizálási és a saját runbooks létrehozása. 

* **Azure automatizálási tárban** található, pillanatban. Az a tár cikkekben teljes dokumentáció konfigurációs és Azure automatizálást és a saját runbooks szerzői kezeléséről. 
* [Azure PowerShell-parancsmagok](http://msdn.microsoft.com/library/jj156055.aspx) a Windows PowerShell szolgáltatással Azure műveletek automatizálása vonatkozó információkat tartalmazza. Runbooks használata parancsmagok az Azure forrásokból. 
* [Adatkezelési Blog](https://azure.microsoft.com/blog/tag/azure-automation/) Ez a témakör a legfrissebb információkat Azure automatizálási és más kezelési technológiák a Microsofttól. A blog naprakész a legújabb Azure automatizálási csapattag elő kell fizetnie. 
* [Automatizálási fórum](http://go.microsoft.com/fwlink/p/?LinkId=390561) lehetővé teszi a Microsoft és az automatizálási közösségi kell foglalkozni az Azure automatizálási kérdéseket tehet közzé. 
* [Azure automatizálást parancsmagok](https://msdn.microsoft.com/library/mt244122.aspx) a felügyeleti feladatok automatizálása vonatkozó információkat tartalmazza. Automatizálási számlákhoz, az eszközök, a runbooks, DSC kezelése parancsmagok tartalmaz.


## <a name="can-i-provide-feedback"></a>Adjon visszajelzést is? 

**Adjon visszajelzést!** Az Azure automatizálási runbook megoldás vagy -integráció a modul keres, ha közzé a egy parancsprogramot kérése Script Center. Azure automatizálást visszajelzések vagy szolgáltatás kérések telepítve van, ha közzé őket [Felhasználói hangposta](http://feedback.windowsazure.com/forums/34192--general-feedback). kösz! 


