<properties 
    pageTitle="A Microsoft adatok tudományos virtuális gép kiépítése |} Microsoft Azure" 
    description="Állítsa be, és hozzon létre virtuális gép adatok tudományos analytics és a gép tanulási Azure." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>A Microsoft adatok tudományos virtuális gép kiépítése

A Microsoft adatok tudományos virtuális gép az Azure virtuális gép (virtuális) kép előre telepítette és beállította az adatok analitikai és gépi tanulási gyakran használt számos népszerű eszközt. A beépített eszközök állnak:

- Microsoft R Server Developer Edition
- Anaconda Python ki.
- Jupyter jegyzetfüzet (ha az R, Python mag)
- Visual Studio közösségi Edition
- A Power BI desktop
- Az SQL Server 2016 Fejlesztőeszközök Edition
- Gépi tanulási eszközök
    - [Számítási hálózati eszközkészlet (CNTK)](https://github.com/Microsoft/CNTK): A szoftver eszközkészlet a Microsoft Research tanulási mély.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): egy gyors gépi tanulási technikákat online, ujjlenyomat, allreduce, csökkentése, learning2search, az aktív, például támogató rendszer és interaktív oktatás.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): gyors és pontos csillapítja fa végrehajtása nyújtó eszköz.
    - [Rattle](http://rattle.togaware.com/) (az R analitikai eszköz kattintva megtudhatja, egyszerűen): olyan eszköz, amely lehetővé teszi az adatok analitikai és gépi tanulási az R egyszerűen, a grafikus-alapú adatok feltárása és modellezése R-kód automatikus létrehozása az első lépések.
    - [mxnet](https://github.com/dmlc/mxnet): mély tanulási keretet megtervezni a teljesítmény és a rugalmasság
- Az R és Python-tárak használata a Azure gépi tanulási és az egyéb Azure szolgáltatás
- Mely számjegy mely számjegy Bash forrás kód tárházakban GitHub, Visual Studio Team Services együtt dolgozhat együtt
- Windows-portok számos népszerű Linux parancssori segédprogramok (beleértve awk sed, perl, grep, keresés, wget, curl stb) parancssor keresztül érhető el. 


Adatok tudományos módon magában foglalja a tevékenységek egy sorozatát léptetés: keresése betöltésekor és feldolgozása előre adatok épület modellek tesztelése és üzembe helyezése a modellek fogyasztási intelligens alkalmazásokban. Adatok tudósok számos eszközt használni, a feladatok elvégzéséhez. Akkor igazán időigényes lehet keresse meg a megfelelő verziója a szoftvert, és töltse le és telepítse azokat. A Microsoft adatok tudományos virtuális gép is megkönnyítése érdekében ezt a terhet kiépítése használatra kész képként megadásával Azure eszközökkel összes számos népszerű előre telepítette és beállította. 

A Microsoft adatok tudományos virtuális gép jump-starts analytics projektjét. Lehetővé teszi a különféle nyelvek, többek között a R, Python, SQL a és C# a tevékenységeken végzendő munkához. Visual Studio tartalmaz egy IDE, valamint tesztelje a kód, amely a könnyen használható. Az Azure SDK szerepel a virtuális lehetővé teszi az alkalmazások használata a különböző szolgáltatások a Microsoft cloud platformon összeállítása. 

Vannak olyan nincs szoftver díjak a adatok tudományos virtuális kép. Csak fizet az Azure használatát díjak mely függő a virtuális gép, kiépítése méretét. A számítási díjak további részletes [adatok tudományos virtuális gép](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) lapon árak adatai csoportban találhatók. 


## <a name="prerequisites"></a>Előfeltételek

Microsoft adatok tudományos virtuális gép létrehozása előtt az alábbiakat kell rendelkeznie:

- **Az Azure-előfizetés**: egy beszerzéséhez lásd: az [első Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Az Azure tároló fiók**: hozzon létre egyet, lásd: [Azure tároló fiók létrehozása](storage-create-storage-account.md#create-a-storage-account). Másik lehetőségként a tárterület-fiók hozhat létre a virtuális létrehozásának, ha nem szeretné, hogy egy meglévő fiók részeként.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>A Microsoft adatok tudományos virtuális gép létrehozása

Íme egy példány a a Microsoft adatok tudományos virtuális gép létrehozásának lépései:

1.  Nyissa meg a bejegyzésére a [portálon Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm)virtuális gépen.
2.   Jelölje ki az alsó kell figyelembe venni a varázsló a **Létrehozás** gombra. ![konfigurálása-adatok-tudományos-virtuális](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   A varázsló a Microsoft adatok tudományos virtuális gép létrehozásához használt **öt lépésben** az jobb oldalán lévő az alábbi ábrán a felsorolt **bemenetben** igényel. Az alábbiakban a bemeneti adatok alapján, állítsa be ezeket a lépéseket minden szükséges:
    
    1.   **Alapjai**
        1.   **Név**: hoz létre adatok tudományos kiszolgálójának nevét.
        2.   **Felhasználónév**: rendszergazdai fiók bejelentkezési azonosító.
        3.   **Jelszó**: rendszergazdai fiók jelszava.
        4.   **Előfizetés**: Ha egynél több előfizetése van, válassza ki a amelyen a gép is létrehozott, a számlát kapni.
        5.   **Erőforráscsoport**: létrehozhat egy új vagy meglévő csoport használata.
        6.   **Hely**: válassza ki az adatközpont, amely a legmegfelelőbb. Általában azt jelenti az adatközpont, amelyet az adatok a legtöbb vagy a fizikai leggyorsabb hálózati hozzáférés helyét a legközelebb esik.
         
    2.   **Méret**: válassza ki a kiszolgáló típusát, amely megfelel a funkcionális és költségkényszerek. További lehetőségek a virtuális méretű elérheti "Nézetben az összes" kiválasztásával.
    
    3.   **Beállítások**:
        1.   **Lemezen típusa**: válassza a prémium Ha inkább egy félvezető meghajtó (SSD), más válassza a "Standard".
        2.   **Tárterület-fiók**: Azure tároló új fiók létrehozása az előfizetését, vagy a **alapvető** lépés a varázsló használata ugyanazon a *helyen* , amely a választott egy meglévőt.
        3.   **További paramétereket**: általában csak használja az alapértelmezett értékeket. Abban az esetben, ha nem az alapértelmezett értékeket a használatát érdemes is mutasson az adott mező súgója a tájékoztató hivatkozásra.

    4.   **Összefoglalás**: Ellenőrizze, hogy helyesek-e minden megadott adatokkal.
    5.   **Vásárlása**: kattintson a **vásárlása** a kiépítési indításához. Hivatkozás kapja meg a tranzakció feltételeit. A virtuális nincsenek túl a számítási a kiszolgáló méretét, a **méret** lépésben kiválasztott bármilyen további díjakat. 


>[AZURE.NOTE] A kiépítési kell vennie körülbelül 10 – 20 percet. A kiépítési állapotát a portálon Azure jelenik meg.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Hogyan érhető el a Microsoft adatok tudományos virtuális gép

Amikor létrejött a virtuális, akkor távoli asztali bele az előző szakaszban **alapjai** konfigurált rendszergazdai fiók hitelesítő adataival. 

Miután a virtuális létrejön, és kiépítéstől, készen áll a telepített és konfigurálva eszközök használatának megkezdéséhez. Vannak start menü csempék és az asztali ikonjai számos eszközt. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Erős jelszó létrehozása a Jupyter jegyzetfüzet kiszolgálón 

A számítógépen telepített Jupyter jegyzetfüzet kiszolgáló saját erős jelszó létrehozása, futtassa a következő parancsot a parancssor lévő adatok tudományos virtuális gépen.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Válassza a olyan erős jelszót a megjelenő párbeszédpanelen.

Megjelenik a jelszó kivonat "sha1:xxxxxx" kimeneti formátumban. A jelszó kivonat másolása, és cserélje le a meglévő kivonat, amely a jegyzetfüzet konfigurációs fájl helyen található: **C:\ProgramData\jupyter\jupyter_notebook_config.py** és a paraméterek neve ***c.NotebookApp.password***.

Csak a meglévő kivonat típusú érték, amely az idézőjelek (xxxxxx) részét helyére. Az árajánlatok és a ***sha1:*** a paraméter értéke is kell megmaradnak előtagját.

Végezetül kell leállítása és indítsa újra a Jupyter kiszolgálót, amely a virtuális fut a windows ütemezett feladatként **Start_IPython_Notebook**nevű. Ha az új jelszót a rendszer nem fogadja el a feladat újraindítása után, próbálja meg közvetlenül az összes futó python folyamata a Feladatkezelő, és indítsa újra az ütemezett feladat. Másik megoldás próbálkozzon a virtuális számítógépet.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Telepítve van a Microsoft adatok tudományos virtuális gép eszközök

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Ha szeretne használni a analytics R, is a virtuális telepített Microsoft R kiszolgáló Developer edition tartalmaz. Microsoft R Server támogatott R alapján, méretezhető és biztonságos szélesebb körben telepíthető nagyvállalati analytics platformot. Számos nagy adatok statisztikai, prediktív modellezési és gépi tanulási funkciók támogatása, R kiszolgáló analytics – adatfeltáró, elemzés, ábrázoló és modellezése a teljes köre támogat. Megnyitás R bővítése és használatával, a Microsoft R kiszolgáló nem teljes mértékben kompatibilis R parancsfájlok, funkciók és CRAN csomagok a vállalati skála adatok elemzése céljából. A párhuzamos és lehetővé tevő adatok feldolgozásának hozzáadásával megnyitott forrás R a memóriában korlátozások is címeket. Ez lehetővé teszi, hogy analytics sokkal nagyobb, mint a fő memória mi elférjen adatok futtatni.  Visual Studio közösségi Edition a virtuális megtalálható a Visual Studio-bővítmény, ahol egy teljes IDE a j k eszközeit tartalmazza Is letöltése és használata más IDEs [RStudio](http://www.rstudio.com)például üléstermet is. 

### <a name="python"></a>Python
Fejlesztési Python használatával Anaconda Python terjesztési 2.7 és 3.5-ös telepítve van. A terjesztési az alap Python együtt kell tudni a leggyakrabban használt matematikai, mérnöki és adatok analytics csomagok 300 tartalmazza. Python Tools for Visual Studio (PTVS), hogy telepítve van a Visual Studio 2015 közösségi edition vagy egyik, például az ÜRESJÁRATI vagy Spyder Anaconda kapcsolt IDEs belül használhatja. Indíthatja el az alábbiak szerint, a keresőmező a keresés egyike (**Win** + **S** billentyűt).

>[AZURE.NOTE] A Python Tools for Visual Studio Anaconda Python 2.7 és 3.5-ös mutassanak tartozó egyéni környezetekben létrehozásához szükséges. A Visual Studio 2015 közösségi Edition az környezet elérési utak beállításához nyissa meg azt a **eszközök** -> **Python eszközök** -> **Python környezetben** , és válassza a **+ egyéni**. 

Anaconda Python 2.7 telepítve van a C:\Anaconda és Anaconda Python 3.5-ös telepítve van a c:\Anaconda\envs\py35. [PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) dokumentációjában a lépések részletes leírását. 

### <a name="jupyter-notebook"></a>Jupyter jegyzetfüzet
Anaconda terjesztési Jupyter jegyzetfüzet kódot és elemzési megosztására környezetben is megtalálható. Jegyzetfüzet Jupyter kiszolgáló előre beállított Python 2, Python 3 és R mag. Van egy asztali ikon nevű "Jupyter jegyzetfüzet indítása a böngészőben a jegyzetfüzetet kiszolgáló elérésére. Ha a virtuális távoli asztali keresztül, is ellátogathat [https://localhost:9999 /](https://localhost:9999/) Jupyter jegyzetfüzet kiszolgáló, amikor a virtuális bejelentkezve eléréséhez.
 
>[AZURE.NOTE] Ha a tanúsítvány figyelmeztetéseket kapni a Folytatás gombra. 

Azt van csomagolt példa jegyzetfüzetek Python és a j A Jupyter jegyzetfüzetek bemutatják, hogyan Jupyter való bejelentkezést követően a Microsoft R Server, SQL Server 2016 R Services (adatbázis-elemző), Python és más Azure technológiák végezhető. A jegyzetfüzet a Kezdőlap lapon a minták mutató hivatkozást láthatja, után, hitelesíteni a Jupyter jegyzetfüzetet a az előző lépésben létrehozott jelszót használja. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 közösségi edition
Visual Studio közösségi edition a virtuális telepítve. A Microsofttól, amely a kipróbálás céljából, és a kisebb csoportok a népszerű IDE egy ingyenes verziója. Érdemes a licencfeltételeket [Itt](https://www.visualstudio.com/support/legal/mt171547).  Az asztal ikonra vagy a **Start** menü duplán kattintva nyissa meg a Visual Studio. A **Win**programok is kereshet + **S** és megadása "Visual Studio". Ha nincs nyelven, például a C#, Python, R, node.js projekteket hozhat létre. Bővítmények, amely készült Azure szolgáltatásokkal, például az Azure adatkatalógus Azure hdinsight szolgáltatáshoz (Hadoop, külső) és Azure adatok tó könnyítse meg is telepítve van. 

>[AZURE.NOTE] Egy üzenet arról, hogy a próbaidőszak lejárt jelenhetnek meg. Írja be a Microsoft-fiók hitelesítő adatait, vagy hozzon létre egy új ingyenes fiókot érheti el a Visual Studio közösségi Edition. 

### <a name="sql-server-2016-developer-edition"></a>Az SQL Server 2016 Fejlesztőeszközök edition
A virtuális rendelkezésre egy SQL Server 2016, az adatbázis-elemző futtatásához R szolgáltatásaival Fejlesztőeszközök változatát. R szolgáltatások fórumot fejlesztéséhez és intelligens alkalmazások üzembe helyezése. A multimédiás és hatékony R nyelvet és a Közösségtől a sok csomagok segítségével modellek létrehozása és az előrejelzések készítése az SQL Server-adatokhoz. Mivel az R szolgáltatások (az adatbázis-) az R nyelv integrálása az SQL Server tárolhatja analytics közelébe az adatokat. Ez megszünteti a költségek és a kapcsolódó adatok mozgását biztonsági kockázatot.

>[AZURE.NOTE] Az SQL Server 2016 Fejlesztőeszközök edition csak akkor használható, fejlesztés és tesztelése céljából. A gyártási futtatásához licencre van szüksége. 

Az SQL server által indítása az **SQL Server Management Studio**érheti el. A virtuális neve van töltve a kiszolgáló neveként. Használja a Windows-hitelesítés bejelentkezve a Windows rendszergazdaként. SQL Server Management Studio be más felhasználók létrehozása, hozza létre az adatbázisokat, importálni az adatokat, és az SQL-lekérdezések futtatása. 

Ahhoz, hogy az adatbázis-elemző Microsoft R használatával, a következő parancsot, egy idő után a kiszolgáló rendszergazdája, naplózás az SQL Server management studio művelet. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Több Azure eszközök telepítve vannak a virtuális:

- Asztali parancsikon eléréséhez az Azure SDK dokumentáció nem. 
- **AzCopy**: való áthelyezését segítik és kijelentkezés a Microsoft Azure tárterület-fiók adatait. Használatát, a használatát megtekintéséhez a parancssorba írja be a **Azcopy** megjelenítéséhez. 
- **Microsoft Azure tároló Explorer**: segítségével megkereshető belül az Azure tárterület-fiók és adatokat vihet át szeretné és Azure tárhelyről tárolt az objektumok között. Írja be a **Tárhely szót** a keresés, vagy a Windows Start menüjének, ez az eszköz eléréséhez mappájában található. 
- **Adlcopy**: adatok áthelyezése az Azure adatok tó használt. Szeretné látni a használatát, írja be a parancssor **adlcopy** . 
- **dtui**: adatok áthelyezése és az Azure DocumentDB, a felhő NoSQL adatbázis használt. A parancssorba írja be a **dtui** . 
- **A Microsoft adatkezelési átjáró**: lehetővé teszi az adatok mozgás a helyszíni adatforrások és a felhő között. Eszközök, például Azure Data Factory belül használatos. 
- **Microsoft Azure Powershell**: való az Azure erőforrások a parancsfájlok futtatásának nyelvi is telepítve van a virtuális Powershell a használt eszköz. 

###<a name="power-bi"></a>A Power BI

Segítséget össze irányítópultok és remek megjelenítések a **Power BI Desktop** telepítve van. Ezzel az eszközzel lekéri az adatokat különböző forrásból Szerző az irányítópultok és a jelentések és közzéteheti a felhőben. Információ a [Power BI](http://powerbi.microsoft.com) -webhelyen talál. Kattintson a Start menü talál a Power BI desktop. 

>[AZURE.NOTE] Office 365-fiókja, a Power BI eléréséhez szükséges. 

## <a name="additional-microsoft-development-tools"></a>További Microsoft fejlesztőeszközök
A [**Microsoft webes Platform telepítő**](https://www.microsoft.com/web/downloads/platform.aspx) Fedezze fel, és más Microsoft fejlesztőeszközök letöltéséhez használható. Az eszköz biztosított Microsoft adatok tudományos virtuális gép asztali parancsikon is van.  

## <a name="important-directories-on-the-vm"></a>A virtuális a fontos könyvtárak

| Elem                          | A címtár |
| ------------------------------| ---------------- |
|Jupyter jegyzetfüzet kiszolgáló beállításait | C:\ProgramData\jupyter |
|Jegyzetfüzet Jupyter minták otthoni címtár| c:\dsvm\notebooks|
|Más minták | c:\dsvm\samples|
| Anaconda (alapértelmezett: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5-ös környezet | c:\Anaconda\envs\py35|
|R kiszolgáló önálló példány címtár (R alapértelmezett példány) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R-kiszolgáló adatbázis-példány címtár | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Egyéb eszközök | c:\dsvm\tools|

>[AZURE.NOTE] Példányok: a Microsoft adatok tudományos virtuális gép 1.5.0 (előtt szeptember 3, 2016) előtt létrehozott használni egy némileg eltérő címtár-struktúra, mint a fenti táblázatban meghatározott. 

## <a name="next-steps"></a>Következő lépések
Íme néhány további lépések továbbra is a tanulási és feltárása. 

* Az adatok tudományos virtuális a különböző tudományos Adateszközök feltárása a start menü kattintva, majd az Eszközök menü felsorolt meg.
* Nyissa meg a RevoScaleR tárba, amely támogatja az adatok analytics a vállalati skála R használatával mintáknál **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** .  
* Olvassa el a következő cikket: [10 dolgot is tehet az adatok tudományos virtuális gépen](http://aka.ms/dsvmtenthings)
* Megtudhatja, hogy miként végpont analitikai megoldások rendszeresen használ a [Csapat tudományos adatfeldolgozás](https://azure.microsoft.com/documentation/learning-paths/data-science-process/).
* Látogasson el a [Cortana üzletiintelligencia-gyűjtemény](http://gallery.cortanaintelligence.com) mintáknál gépi tanulási és adatok analytics, amelyek a Cortana üzletiintelligencia-programcsomagot. Kattintson a **Start** menü és a virtuális gépen ebben az elemtárban asztali is nyújtott ikont.

