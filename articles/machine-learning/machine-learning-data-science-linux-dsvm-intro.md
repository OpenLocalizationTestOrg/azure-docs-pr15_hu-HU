<properties
    pageTitle="A Linux adatok tudományos virtuális gép kiépítése |} Microsoft Azure"
    description="Állítsa be, és hozzon létre virtuális gép Linux adatok tudományos analytics és a gép tanulási Azure."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>A Linux adatok tudományos virtuális gép kiépítése

A Linux adatok tudományos virtuális gépre egy Azure virtuális gép részét képező előre telepített eszközök gyűjteménye. Ezek az eszközök általában arra használják adatok analytics módon, és a számítógép tanulási. A fő szoftver található összetevői:

- Microsoft R Server Developer Edition
- Anaconda Python eloszlás (2.7 és 3.5-ös verzió), beleértve a népszerű adatok elemzése tárak
- JupyterHub - R-Python, Ágnes mag támogató többfelhasználós Jupyter jegyzetfüzet kiszolgáló
- Azure tároló Explorer
- Azure parancssori kezelőfelületről az Azure-erőforrások kezelése
- PostgresSQL adatbázis
- Gépi tanulási eszközök
    - [Számítási hálózati eszközkészlet (CNTK)](https://github.com/Microsoft/CNTK): A szoftver eszközkészlet a Microsoft Research tanulási mély.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): egy gyors gépi tanulási technikákat online, ujjlenyomat, allreduce, csökkentése, learning2search, az aktív, például támogató rendszer és interaktív oktatás.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): gyors és pontos csillapítja fa végrehajtása nyújtó eszköz.
    - [Rattle](http://rattle.togaware.com/) (az R analitikai eszköz kattintva megtudhatja, egyszerűen): olyan eszköz, amely lehetővé teszi az adatok analitikai és gépi tanulási az R egyszerűen, a grafikus-alapú adatok feltárása és modellezése R-kód automatikus létrehozása az első lépések.
- Java, Python, node.js, és Azure SDK fonetikus, PHP
- Az R és Python-tárak használata a Azure gépi tanulási és az egyéb Azure szolgáltatás
- Fejlesztőeszközök és szerkesztők (Holdas, Emacs, gedit VI.)

Adatok tudományos módon a tevékenységek egy sorozatát léptetés foglalja magában:

1. Keresése, betöltése és előfeldolgozása adatok
2. Összeállítását, és az adatmodellek vizsgálata
3. A modellek fogyasztási intelligens alkalmazások telepítése

Adatok tudósok különböző eszközök segítségével a feladatok elvégzéséhez. Akkor igazán időigényes lehet keresse meg a megfelelő verziója a szoftvert, és majd letöltéséhez összeállítása, és telepítse az alábbi verziókat.

A Linux adatok tudományos virtuális gép jelentősen ezt a terhet megkönnyítése érdekében. Annak segítségével a analytics projekt jump-start. Lehetővé teszi a különféle nyelvek, többek között a R, Python, SQL, Java és C++ a tevékenységeken végzendő munkához. Holdas tartalmaz egy IDE, valamint tesztelje a kód, amely a könnyen használható. Az Azure SDK szerepel a virtuális lehetővé teszi az applications készíthet különböző szolgáltatások használatával Linux a Microsoft cloud platform. Ezeken kívül más nyelvekre előre is telepítve van, például a fonetikus, Perl, PHP és node.js hozzáférése van.

Vannak olyan nincs szoftver díjak a adatok tudományos virtuális kép. Csak az Azure hardver használatát díjakat, amely a virtuális gép, amely a virtuális képpel, kiépítése méretének alapján értékelni fizet. A számítási díjak rendszeren a [virtuális bejegyzésére lapon kattintson a Microsoft Azure piactéren ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/)található.


## <a name="prerequisites"></a>Előfeltételek

Mielőtt Linux adatok tudományos virtuális gép hozhat létre, a következőket kell rendelkeznie:

- **Az Azure-előfizetés**: egy beszerzéséhez lásd: az [első Azure ingyenes próbaverziót](https://azure.microsoft.com/free/).
- **Az Azure tároló fiók**: hozzon létre egyet, lásd: [Azure tároló fiók létrehozása](storage-create-storage-account.md#create-a-storage-account). Másik lehetőségként a tárterület-fiók hozhat létre a virtuális, létrehozásának folyamata részeként Ha nem szeretné, hogy egy meglévő fiók.


## <a name="create-your-linux-data-science-virtual-machine"></a>Az adatok Linux tudományos virtuális gép létrehozása

Íme egy példány a Linux adatok tudományos virtuális gépen létrehozásának lépései:

1.  Nyissa meg a bejegyzésére a [portálon Azure](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)virtuális gépen.
2.   Kattintson a **Létrehozás** gombra (alul) kattintva jelenítse meg a varázsló. ![konfigurálása-adatok-tudományos-virtuális](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   A következő szakaszokban a bemeneti adatok alapján az egyes (jobb oldalán lévő a fenti ábrán felsorolásos) a varázsló lépéseit a Microsoft adatok tudományos virtuális gép létrehozásához használt. Az alábbiakban a bemeneti adatok alapján, állítsa be ezeket a lépéseket minden szükséges:

    egy. **Alapismeretek**:

  - **Név**: hoz létre adatok tudományos kiszolgálójának nevét.
  - **Felhasználónév**: először fiókot bejelentkezés azonosítójával.
  - **Jelszó**: (használható SSH nyilvános kulcs jelszó helyett) első fiók jelszava.
  - **Előfizetés**: Ha egynél több előfizetése van, válassza ki a amelyen a gép is létrehozott, a számlát kapni. Az előfizetéshez tartozó erőforrás létrehozásához szükséges jogosultsággal kell rendelkeznie.
  - **Erőforráscsoport**: létrehozhat egy új vagy meglévő csoport használata.
  - **Hely**: válassza ki az adatközpont, amely a legmegfelelőbb. Általában azt jelenti az adatközpont, amelyet az adatok a legtöbb, vagy a fizikai leggyorsabb hálózati hozzáférés helyét a legközelebb esik.

    b. **Méret**:

  - Jelölje ki azt a Kiszolgálótípusok, amely megfelel a funkcionális és költségkényszerek. Válassza az **Összes megtekintése** a virtuális méretű további lehetőségek megjelenítéséhez.

    c billentyűkombinációt. **Beállítások**:

  - **Lemezen típusa**: válassza az egyszínű állam meghajtó (SSD) inkább **prémium verzió** . Egyéb esetben válassza a **normál**.
  - **Tárterület-fiók**: Azure tároló új fiók létrehozása az előfizetését, vagy a **alapvető** lépés a varázsló használata a kiválasztott ugyanazon a helyen egy meglévőt.
  - **További paramétereket**: A legtöbb esetben csak használja az alapértelmezett értékeket. Nem az alapértelmezett értékeket figyelembe, mutasson az adott mező súgója a tájékoztató hivatkozásra.

    d. **Összefoglalás**:

  - Győződjön meg arról, hogy helyesek-e minden megadott adatokkal.

    e. **Vásárlása**:

  - A kiépítési elindításához kattintson a **vásárlása**parancsra. Hivatkozás kapja meg a tranzakció feltételeit. A virtuális nincsenek túl a számítási a kiszolgáló méretét, a **méret** lépésben kiválasztott bármilyen további díjakat.

A kiépítési kell vennie körülbelül 10 – 20 percet. A kiépítési állapotát a portálon Azure jelenik meg.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Hogyan érhető el a Linux adatok tudományos virtuális gépen

A virtuális létrehozását követően jelentkezzen, SSH használatával. A fiók hitelesítő adatait, a szöveg rendszerhéj felület 3 lépés **alapjai** részében létrehozott használja. A Windows töltse le egy SSH ügyfél eszköz például [gitt](http://www.putty.org). Ha inkább grafikus asztali (X Windows rendszer), a hívásátirányítás gitt X11 használja, vagy az X2Go ügyfélcsomag telepítése.

>[AZURE.NOTE] A X2Go ügyfél elvégzett lényegesen nagyobb mértékben teszteli a továbbítás X11. Azt javasoljuk, hogy a X2Go ügyfél használata egy asztali grafikus felületen.


## <a name="installing-and-configuring-x2go-client"></a>Való telepítéséről és konfigurálásáról X2Go ügyfél

A Linux virtuális már kiépített X2Go kiszolgálóval, és készen áll a ügyfélkapcsolatok. A grafikus Linux virtuális asztali csatlakozni, tegye a következőket az ügyfélgépen:

1. Töltse le és telepítse az ügyfél platform X2Go ügyfél [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Futtassa a X2Go ügyfélprogram, és válassza az **Új munkamenetet**. Több fülekkel konfigurációs ablak nyílik meg. Írja be az alábbi konfigurációs paramétereket:
    * **Munkamenet lapon**:
        - **A Host**: az állomásnév vagy a Linux adatok tudományos virtuális IP-címét.
        - **Bejelentkezés**: a Linux virtuális a felhasználónevet.
        - **SSH Port**: 22, az alapértelmezett érték a hagyja.
        - **Munkamenet típusa**: XFCE változtassa meg az értéket. A Linux virtuális jelenleg csak asztali XFCE használatát támogatja.
    * **Médiafájlok lap**: hang és a nem kell a segítségükkel nyomtatásának ügyfél kikapcsolhatja.
    * **Megosztott mappák**: Ha az ügyfél gépek a Linux virtuális a csatlakoztatni kívánt könyvtárak, adja meg az ügyfél gépi könyvtárak, amely a virtuális ezen a lapon a megosztani kívánt.

Miután bejelentkezett a virtuális SSH ügyfél vagy a X2Go ügyfél XFCE grafikus asztalának használatával, készen áll az eszközöket, hogy telepítve és beállítva a virtuális használatának megkezdéséhez. A XFCE látható alkalmazások parancsikonjai és az asztali ikonok a számos eszközt.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Kattintson a Linux adatok tudományos virtuális gépre telepített eszközök

### <a name="microsoft-r-open"></a>A Microsoft R megnyitása
R a leggyakrabban használt nyelvek adatelemzés és gépi tanulási egyike. Ha szeretné használni a analytics R, is a virtuális Microsoft R megnyitása (MRO) a matematikai Kernel Library (MKL) tartalmaz. A MKL matematikai műveletek az analitikai algoritmusok közös optimalizálja. MRO 100 %-os CRAN-R-kompatibilis, és a közzétett CRAN R tárak közül a MRO telepíthető. Az R-alkalmazások alapértelmezett-szerkesztők, például vi, Emacs vagy gedit egyikében szerkesztheti. Is töltse le és más IDEs, például [RStudio](http://www.rstudio.com)használja. Egy egyszerű parancsfájl (installRStudio.sh) kényelme telepíti az RStudio **/dsvm/tools** címtárban megadva. A Emacs szerkesztő használatakor, hogy a Emacs csomag ESS (Emacs Speaks statisztika), amely egyszerűbbé teszi a Megjegyzés előre telepítve van a Emacs szerkesztője R-fájlok használata került.

R elindítására éppen ír **R** a rendszerhéj. Ekkor megjelenik egy interaktív környezetben. Az R programot fejlesztése, például Emacs vagy vi vagy gedit szerkesztő általában használja, és futtassa a j parancsfájlok RStudio telepítette, ha egy teljes grafikus IDE környezet kialakítása az R programot.

Az R parancsfájl, a [felső 20 R csomagok](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) telepítése, ha azt szeretné, hogy is van. A parancsfájl futtatását is lehetővé teszi, a R interaktív felülettel, amely megadható (mint az imént említett) a rendszerhéj **R** beírásával után.  

### <a name="python"></a>Python
Fejlesztési Python használatával Anaconda Python terjesztési 2.7 és 3.5-ös telepítve van. A terjesztési az alap Python együtt kell tudni a leggyakrabban használt matematikai, mérnöki és adatok analytics csomagok 300 tartalmazza. Az alapértelmezett szöveget szerkesztők is használhatja. Ezeken kívül Spyder, egy Python IDE Anaconda Python terjesztését mellékelt is használhatja. Spyder van szüksége, grafikus asztali vagy X11 továbbítása. A grafikus asztali parancsikon Spyder kapja.

Mivel Python 2.7 és 3.5-ös is van, akkor kifejezetten verziót kell aktiválnia a kívánt Python az aktuális munkamenetben a használni kívánt. Az aktiválási folyamat állítja a PATH változó Python a kívánt verziót.

Python 2.7 aktiválásához az rendszerhéj futtassa a következő:

    source /anaconda/bin/activate root

Python 2.7 */anaconda/bin*van telepítve.

Python 3.5-ös aktiválásához az rendszerhéj futtassa a következő:

    source /anaconda/bin/activate py35


Python 3.5-ös */anaconda/envs/py35/bin*van telepítve.

Interaktív Python munkamenet indítása, egyszerűen írja be **python** a rendszerhéj. Ha vannak olyan grafikus felületen vagy X11 továbbítása beállítás be van, **spyder** elindítani az Python IDE írhat.

### <a name="jupyter-notebook"></a>Jupyter jegyzetfüzet

A Anaconda eloszlás Jupyter jegyzetfüzet kódot és elemzési megosztására környezetben is megtalálható. A Jupyter jegyzetfüzet JupyterHub keresztül érhető el. Jelentkezzen be a helyi Linux felhasználónevét és jelszavát.

A Jupyter jegyzetfüzet kiszolgáló előre beállított Python 2, Python 3 és R mag. Van egy asztali ikonra kattintva indítsa el a böngészőben a jegyzetfüzetet kiszolgáló elérésére "Jupyter jegyzetfüzet" nevű. Ha a virtuális SSH vagy X2Go ügyfél keresztül, is ellátogathat [https://localhost:8000 /](https://localhost:8000/) a Jupyter jegyzetfüzet kiszolgáló elérésére.

>[AZURE.NOTE] Ha a tanúsítvány figyelmeztetéseket kapni a Folytatás gombra.

A Jupyter jegyzetfüzet kiszolgáló bármely állomástól is elérheti. Egyszerűen írja be a *https://\<virtuális DNS-nevét és IP-cím\>: 8000 /*

>[AZURE.NOTE] Port 8000 nyitja meg a tűzfal alapértelmezés szerint a virtuális már kiépítve esetén.

Azt van csomagolt példa jegyzetfüzetek – egy a Python és egy j A jegyzetfüzet a Kezdőlap lapon a minták mutató hivatkozást láthatja, után a helyi Linux felhasználónév és jelszó használatával hitelesíteni a Jupyter jegyzetfüzetbe. **Új**, majd a megfelelő nyelvi kernel kiválasztásával is létrehozhat új jegyzetfüzetet. Ha nem látja az **Új** gombra, kattintson az Ugrás a kezdőlapra, a jegyzetfüzet kiszolgáló bal felső **Jupyter** ikonjára.


### <a name="ides-and-editors"></a>IDEs és szerkesztők

A még egy több kódot szerkesztők. Ide tartoznak a vi/VIM, Emacs, gEdit és Holdas. gEdit Holdas grafikus szerkesztők és szükséges teszi bejelentkezni grafikus asztali használhatók. Ezek a szerkesztők van asztali és az alkalmazás parancsikonjai meg őket.

**VIM** és **Emacs** olyan szöveges szerkesztők. A Emacs azt az Emacs Speaks statisztika (ESS), amely megkönnyíti a k használata a Emacs szerkesztője nevű bővítmény használatára feljogosító csomagok telepítette. További információt a [ESS](http://ess.r-project.org/)találhatók.

**Holdas** egy nyissa meg a forrás, amely támogatja a többnyelvű bővíthető IDE. A fejlesztők Java edition a példány telepítve a virtuális. Nincsenek telepített kiterjesztése a Holdas környezet számos népszerű nyelvek bővítmények. Azt is, hogy a beépülő modul telepítve van az **Azure eszközkészlete Holdas**nevű Holdas. Lehetővé teszi létrehozása, kialakítása, tesztelése és helyezhetnek üzembe a Java hasonló nyelvek támogató Holdas fejlesztői környezet segítségével Azure alkalmazásokat. Egy **Azure SDK Java** lehetővé teszi, hogy a különböző Azure szolgáltatásai a Java környezetben is van. Holdas az Azure eszközkészlet további információt a [Holdas Azure eszközkészlete](../azure-toolkit-for-eclipse.md)találhatók.

**LaTex** keresztül egy Emacs bővítmény [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) csomag egyszerűsíti a LaTex dokumentumok belül Emacs létrehozásával együtt a texlive csomag van telepítve.  

### <a name="databases"></a>Adatbázisok

#### <a name="postgres"></a>Postgres
A megnyitott forrásadatbázis **Postgres** érhető el a virtuális, futó szolgáltatások és initdb már elvégezték. Van szüksége adatbázisok és a felhasználók létrehozására. További tudnivalókért lásd: a [Postgres dokumentációt](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>A grafikus SQL ügyfélhez
**Erdeimókus SQL**, a grafikus SQL ügyfélhez kapott különböző adatbázisok (például a Microsoft SQL Server Postgres és MySQL) való kapcsolódáshoz és SQL-lekérdezések futtatása. (A X2Go ügyfélprogram, például használatával) grafikus asztali munkamenetből futtathatók. Erdeimókus SQL meghívni, válassza az asztali ikonra kattintva indítsa el, vagy futtassa a következő parancsot a rendszerhéj.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Az első használat előtt állítsa be a illesztőprogramok és az adatbázis aliasok. A JDBC illesztőprogramok találhatók:

*/usr/share/Java/jdbcdrivers*

További tudnivalókért olvassa el a [Erdeimókus SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots)című témakört.

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Microsoft SQL Server eléréséhez parancssori eszközök

Az SQL Server ODBC-vezérlőcsomagjának két parancssori eszközöket is nyújtja:

**BCP**: bcp segédprogram tömeges másolja az adatokat a Microsoft SQL Server-példányt, és az adatfájl között a felhasználó által megadott formátumban. Új sorok nagyszámú importálni az SQL Server-táblák, illetve ki a táblázatok adatok exportálása adatfájlok, a bcp segédprogram használható. Az adatokat importálhatja egy táblába, kell táblához tartozó létrehozott formátumú fájl használata, vagy a táblázat és az adatok érvényesek a benne szereplő oszlopokat típusú értelmezésekor.

További tudnivalókért olvassa el a [Csatlakozás a bcp](https://msdn.microsoft.com/library/hh568446.aspx).

**Sqlcmd**: Adja meg a Transact-SQL-utasításait a sqlcmd segédprogramot, valamint a rendszer eljárások együtt, és a parancssorablakban parancsfájlok. Ez a segédprogram ODBC használja a Transact-SQL kötegeket végrehajtása.

További tudnivalókért olvassa el a [Csatlakozás az sqlcmd](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] Bizonyos különbségek vannak ezzel az eszközzel Linux és a Windows platformon között. Használatáról további információt talál.


#### <a name="database-access-libraries"></a>Adatbázis az access-tárak

Tárak áll rendelkezésre, R és Python az access-adatbázisokra.

- Az R a **RODBC** programcsomag vagy **dplyr** lehetővé teszi a lekérdezés, vagy hajtsa végre az adatbázis-kiszolgáló SQL-utasításait.
- Python a **pyodbc** tár nyújt az ODBC adatbázis az access, az alapul szolgáló réteg.  

**Postgres**elérése:

- A csomag **RPostgreSQL**R: Használja.
- Python: Használja a **psycopg2** tárat.


### <a name="azure-tools"></a>Azure eszközök
A következő Azure eszközök telepítve van a virtuális:

- **Azure parancssor**: az Azure CLI lehetővé teszi, hogy létrehozása és kezelése az Azure erőforrások rendszerhéj parancsok keresztül. Az Azure eszközök meghívandó írja a **azure segítséget**. További tudnivalókért lásd: az [Azure CLI dokumentáció lapot](../virtual-machines-command-line-tools.md).
- **Microsoft Azure tároló Explorer**: Microsoft Azure tároló Explorer eszköz olyan grafikus, amelynek segítségével lépkedjen végig az Azure tároló fiókjában tárolt objektumokat és feltöltése és letöltése az adatokat, és az Azure BLOB. Az asztali parancsikon tároló Explorer elérhet. Hívhatja rá rendszerhéj parancssorból **StorageExplorer**beírásával. Kell bejelentkeznie X2Go ügyfélről, vagy X11 továbbítása beállítás be van.
- **Azure-tárak**: az alábbiakban néhány olyan előre telepítve van a tárak.

 - **Python**: az Azure kapcsolatos tárakat, telepített Python **azure**, **azureml**, **pydocumentdb**és **pyodbc**. Az első három tárak elérheti Azure tároló services, az Azure gépi tanulási és Azure DocumentDB (Azure NoSQL adatbázis). A negyedik tár pyodbc (együtt a Microsoft ODBC-illesztőprogram az SQL Server), lehetővé teszi a hozzáférést az SQL Server Azure SQL-adatbázis és Azure SQL-adatraktár Python az ODBC felületet használatával. Írja be a **mezőpont listában** , hogy a felsorolt tárakat. Ügyeljen arra, hogy ez a parancs futtatása a Python 2.7 és 3.5-ös környezetben is.
 - **R**: az Azure kapcsolatos tárakat R, telepített **AzureML** és **RODBC**.
 - **Java**: Azure Java-tárak listája megtalálható a virtuális a címtár **/dsvm/sdk/AzureSDKJava** . A fő tárak Azure tárolási és kezelési API-hoz, DocumentDB és JDBC illesztőprogramok az SQL Server.  

Az [Azure portált](https://portal.azure.com) a előre telepítve van a Firefox böngészőből érheti el. Az Azure portálon elkészítheti, kezelheti és Azure erőforrások figyelése.

### <a name="azure-machine-learning"></a>Azure gépi tanulási

Azure gépi tanulási egy teljes körű felügyelt felhőalapú szolgáltatásba, amely lehetővé teszi, hogy összeállítása, üzembe helyezéséhez és a prediktív analytics-megoldások megosztása. A kísérletek és az adatmodellek Azure gépi tanulási Studio build. Azt webböngészőn keresztül elérhetők az adatok tudományos virtuális gépen webböngészőben látogasson el a [Microsoft Azure gépi tanulási](https://studio.azureml.net).

Miután bejelentkezett a Azure gépi tanulási Studio, van hozzáférése egy kísérletezés vászon egy logikai folyamata a gépi tanulási algoritmusok vagy. Is rendelkezik hozzáféréssel a Jupyter jegyzetfüzet Azure gépi tanulási is, és zökkenőmentesen dolgozhat a kísérletek gépi tanulási Studio alkalmazásban. A gépi tanulási modelleket, amelyek szerkesztésével tördelése őket egy webszolgáltatási felületet üzemeltető. Bármilyen nyelven írt ügyfelek meghívása a gépi tanulási modellek az előrejelzések lehetővé teszi. További tudnivalókért lásd: a [gépi tanulási dokumentációt](https://azure.microsoft.com/documentation/services/machine-learning/).

A modellek R vagy Python is épülnek a virtuális, és a Azure gépi tanulási gyártási telepítheti. R (**AzureML**) és a Python (**azureml**) ahhoz, hogy ez a funkció tárakat telepítve azt.

Az R és Python levő Azure gépi tanulási történő telepítéséről további tudnivalókért lásd [tíz dolgot is tehet az adatok tudományos virtuális gép](machine-learning-data-science-vm-do-ten-things.md) (különösen a szakasz "R vagy Python modellek összeállítása és üzemeltető őket Azure gépi tanulási használata").

>[AZURE.NOTE] Ezeket az utasításokat a adatok tudományos virtuális a Windows-verzióhoz készült. De az információ a modellek bevezetéshez Azure gépi tanulási nem alkalmazható a Linux virtuális.

### <a name="machine-learning-tools"></a>Gépi tanulási eszközök

A virtuális megtalálható néhány gépi tanulási eszközök és algoritmusok, amelyet a előre lefordított előtelepítve helyi meghajtóra. Ezek a következők:

* **CNTK** (A Microsoft Research számítási hálózati eszközkészlet): A mély eszközkészlet tanulási.
* **Vowpal Wabbit**: egy gyors online oktatási algoritmus.
* **xgboost**: optimalizált, csillapítja fa algoritmusok egy eszközt.
* **Python**: Anaconda Python gépi tanulási algoritmusok a tárak Scikit megtudhatja például együtt csomagolva megtalálható. Használatával is telepítheti a tárakat a `pip install` parancsot.
* **R**: gépi tanulási függvények gazdag tár érhető el az R. A tárak, amelyek előre telepítve lm, glm, randomForest, rpart. Más tárak telepíthető futtatásával:

        install.packages(<lib name>)

Íme néhány további információt az első három gépi tanulási eszközök a listában.

#### <a name="cntk"></a>CNTK
Ez a megnyitott forrás, mély eszközkészlet tanulási. Parancssori eszköz (cntk), és már szerepel az elérési ÚTJÁT.

Egy egyszerű példa futtatásához hajtsa végre az alábbi parancsokat a rendszerhéj:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

A modell kimeneti *~/cntkdemo/Output/Models*szerepel.

További tudnivalókért olvassa el a [GitHub](https://github.com/Microsoft/CNTK), és a [CNTK wiki](https://github.com/Microsoft/CNTK/wiki)CNTK szakaszát.


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit egy gépi tanulási technikákat online, ujjlenyomat, allreduce, csökkentése, learning2search, az aktív, például használó rendszer és interaktív oktatás.

Az eszköz legalapvetőbb például futtatásához tegye a következőket:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Vannak más, nagyobb bemutatók könyvtárra. VW kapcsolatos további tudnivalókért olvassa el az [Ebben a szakaszban a GitHub](https://github.com/JohnLangford/vowpal_wabbit), és a [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki)című témakört.

#### <a name="xgboost"></a>xgboost
Ez a egy dokumentumtárba, amelynek kialakításának és algoritmusok csillapítja (fa) optimalizálva. A tár célja gépek kiszámítása határain leküldéses való megadására szolgáló lehetőségekről, hogy nagyszabású fa szükséges méretek is méretezhető hordozható és pontos.

Biztosítjuk, mint a parancssor, valamint az R tár.

Ez az R tár használatához indítása interaktív R (csak beírásával **R** a rendszerhéj), és töltse be a tár.

Íme egy egyszerű példa R-parancssorában futtatását is lehetővé teszi:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Futtassa a xgboost parancssort, Íme a parancsokat a rendszerhéj végrehajtása:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


.Model fájl a megadott címtárhoz íródott. Ebben a példában a bemutató információt [GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification)megtalálható.

Xgboost kapcsolatos további tudnivalókért lásd: a [xgboost dokumentáció lapra](https://xgboost.readthedocs.org/en/latest/), és annak [Github tárházba](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
Rattle ( **R** **A**nalytical **T**öz **T**o **L**hírnévpontszámokat **E**asily) használ, grafikus-alapú adatok feltárása és modellezésre. Így megjelenített adatok átalakítások, hogy könnyen modellezni, hozza létre az adatok felügyelet, mind a felügyelt modellek, hogyan jeleníti meg a teljesítmény modellek grafikusan, statisztikai és vizuális összegzéseinek és értékek új adatok állítja be. Is R kód, a műveletek, amely közvetlenül R futtatását, vagy további elemzés céljából kiindulási pontként használt felhasználói felület replikálása hoz létre.

Rattle futtatásához kell lennie a grafikus asztali bejelentkezési munkamenetben. Írja be a terminálablakba ```R``` beírása a R-környezetet. R a parancssorba írja be az alábbi parancsokat:

    library(rattle)
    rattle()

A grafikus felületen most megnyílik, és egy sor olyan lapokon. A minta időjárási adatok szabálykészletet használ, és hozza létre a modell szükséges Rattle az alábbiakban a rövid útmutató az első lépéseket. Az alábbi lépésekkel néhány kéri automatikusan telepítse és töltse be néhány szükséges R csomagokat, amelyek még nem a rendszer.

>[AZURE.NOTE] Ha nem az access a szoftvercsomag telepítésekor a rendszer címtárban (alapértelmezett), megjelenhet egy kérdés az R konzol ablakban saját személyes tárát csomagok telepítése. *Y* fogadni, ha megjelenik a következő képernyőn megjelenő utasításokat.

1. Kattintson a **végrehajtása**.
2. Felfelé, megjelenik egy párbeszédpanel kéri, ha szeretné, hogy a példa időjárási adatkészlet használata. Kattintson az **Igen gombra** a példa betöltése.
3. Kattintson a **modell** fülre.
4. Kattintson a **végrehajtás** döntési fa össze.
5. Kattintson a **rajz** a döntési fa megjelenítése gombra.
6. Kattintson a **erdő** választógombra, és **végrehajtás** véletlen erdő összeállításához gombra.
7. Kattintson a **kiértékelés** fülre.
8. Kattintson a **kockázat** választógombra, és a **végrehajtás** két kockázatok (eloszlásfv) teljesítmény felvétel megjelenítése gombra.
9. Kattintson a **Log** fülre kattintva jelenítse meg a fenti műveletek létrehozása R kódját.
(Ebben a kiadásban a Rattle egy hiba miatt kell beszúrása egy *#* karakter elé *a napló... exportálása* a szöveget a napló.)
10. Kattintson az **Exportálás** gombra, a *weather_script nevű R script fájl mentéséhez. R* az otthoni mappába.

Kiléphet Rattle és R. Most módosítsa a létrehozott R parancsfájlt, vagy úgy, ahogyan futtatásával bármikor Rattle a felhasználói felület belül végrehajtott minden ismétlődő vele. Különösen kezdőknek az R az ugyanígy gyorsan elemzéseket végezni, és a számítógép tanulási egy egyszerű grafikus felületen automatikusan a j billentyűt a módosítása, illetve megtudhatja, a kód létrehozása közben.


## <a name="next-steps"></a>Következő lépések
Az alábbiakban hogyan a tanulási és feltárása továbbra is:

* Az [adatok tudományos a Linux adatok tudományos virtuális gépen](machine-learning-data-science-linux-dsvm-walkthrough.md) forgatókönyv mutatja be néhány általános adatok tudományos feladatok elvégzéséhez, a kiépítéstől következő Linux adatok tudományos virtuális. 
* Az adatok tudományos virtuális a különböző tudományos Adateszközök feltárása a jelen cikkben ismertetett eszközök ki szeretné próbálni. További információt az eszközök a virtuális telepítve a rendszerhéj belül a virtuális gép egy egyszerű – bevezetés és a mutatók a is futtathatók *dsvm-több-információ* .  
* Megtudhatja, hogy miként végpontok közötti analitikai megoldások rendszeresen összeállítása a [Csapat adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)használatával.
* Látogasson el a [Cortana Analytics gyűjtemény](http://gallery.cortanaanalytics.com) mintáknál gépi tanulási és adatok analytics, amelyek a Cortana Analytics programcsomagot.
