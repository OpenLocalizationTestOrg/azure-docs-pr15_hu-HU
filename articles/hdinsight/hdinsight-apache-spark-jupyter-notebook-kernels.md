<properties 
    pageTitle="Elérhető a HDInsight külső Jupyter jegyzetfüzeteket mag fürtök Linux |} Microsoft Azure" 
    description="Tudjon meg többet a további Jupyter jegyzetfüzet mag külső fürt HDInsight Linux számára érhető el." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Mag HDInsight Linux Apache külső fürt Jupyter jegyzetfüzetek érhető el

A HDInsight (Linux) Apache külső fürt Jupyter jegyzetfüzetek használó tesztelje az alkalmazásokat tartalmazza. Kernel rendszer fut, és a kód értelmezi programot. HDInsight külső fürt adja meg a két mag Jupyter jegyzetfüzet használható. A következők:

1. **PySpark** (az alkalmazásai Python)
2. **A külső** (az alkalmazásai Scala)

Ebben a cikkben megismerheti, ezek mag használatáról, és milyen előnyökkel, letölthető használja őket.

**Előfeltételek:**

Rendelkeznie kell a következőket:

- Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Egy HDInsight Linux Apache külső fürthöz. Című cikkben olvashat [létrehozása Apache külső fürt az Azure hdinsight szolgáltatásból lehetőségre](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Hogyan használhatom a mag? 

1. Az [Azure-portált](https://portal.azure.com/)a a startboard kattintson a csempére a külső fürt (Ha a startboard a kiemelt). Az **Összes böngészése**a fürthöz is navigálhat > **Fürt hdinsight szolgáltatásból lehetőségre**.   

2. A külső fürt lap **Fürt irányítópult**elemre, és kattintson a **Jupyter jegyzetfüzet**elemre. Ha a rendszer kéri, írja be a fürt a rendszergazdai hitelesítő adataival.

    > [AZURE.NOTE] Előfordulhat, hogy is elér Jupyter jegyzetfüzet a fürt számára a böngészőben nyissa meg az alábbi URL-címet. __CLUSTERNAME__ cserélje ki a fürt neve:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Új jegyzetfüzet létrehozása a új mag. Kattintson az **Új**gombra, és válassza a **Pyspark** vagy **külső**. A külső mag Scala alkalmazások és a PySpark kernel Python alkalmazások kell használni.

    ![Új Jupyter jegyzetfüzet létrehozása] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Új Jupyter jegyzetfüzet létrehozása") 

3. A a kijelölt kernel ez célszerű megnyitni egy új jegyzetfüzetet.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Miért érdemes használni a PySpark vagy külső mag?

Az alábbiakban az új mag használatának néhány előnyeit.

1. **Előre definiált környezetek**. A **PySpark** vagy **külső** mag Jupyter jegyzetfüzetek által biztosított nem kell beállítania a külső vagy a struktúra környezetek kifejezetten előtt: Ha fejlesztéséhez; az alkalmazás használata Ezek alapértelmezés szerint elérhető. Ezek a környezetek a következők:

    * **sc** - külső környezetben
    * **sqlContext** - struktúra környezetben


    Igen nem kell futtassa a következő környezet beállításához utasításokat:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Ehelyett közvetlenül a is használhatja az előre beállított környezetek az alkalmazást.
    
2. **Cella magics**. A PySpark kernel tartalmaz néhány előre definiált "magics", melyek, amelyek a felhívhatja speciális parancsok `%%` (pl. `%%MAGIC` <args>). A színű parancsot kell a kód cellában az első szóra, és többsoros tartalom engedélyezése. A színű szót kell lennie a cellában az első szóra. Hozzáadás mágikus, még akkor is megjegyzéseket, mielőtt bármilyen hibát jelez.   Magics kapcsolatos további tudnivalókért lásd [az alábbi](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Az alábbi táblázat felsorolja a különböző magics a mag keresztül érhető el.

  	| Mágikus     | Példa                         | Leírás  |
  	|-----------|---------------------------------|--------------|
  	| segítség      | `%%help`                            | Létrehoz egy táblát, az összes rendelkezésre álló magics példa és leírás |
  	| információ      | `%%info`                          | Az aktuális Livius végpont kimeneti értékeket munkamenet adatait |
  	| konfigurálása | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Beállítja a paramétereit munkamenet létrehozása. A kötelező jelző (-f) kötelező, ha már létezik egy munkamenetet, és a munkamenet a rendszer eltávolítja, és az ismételt megadásukra. Tekintse meg [Livius a bejegyzés /sessions összehívás törzsébe](https://github.com/cloudera/livy#request-body) érvényes paramétereinek listáját. Paraméterek át kell adni a JSON karakterláncként és későbbinek kell lennie a következő sorban a mágikus, például oszlopában látható módon. |
  	| SQL       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | A sqlContext ellen struktúra lekérdezés végrehajtása. Ha a `-o` paramétert, a lekérdezés eredményét a megőrződnek a %% helyi Python környezetben, mint egy [Pandas](http://pandas.pydata.org/) dataframe.   |
  	| helyi     |     `%%local`<br>`a=1`              | A további vonalak összes kód helyileg hajtható végre. Kód érvényes Python kódot kell lennie. |
  	| naplók      | `%%logs`                        | A naplókat, az aktuális Livius munkamenet kimeneti értékeket.  |
  	| törlése    | `%%delete -f -s <session number>` | Az aktuális Livius végpont adott munkamenet törli. Figyelje meg, hogy magát a kernel kezdeményezett munkamenet nem törölhető. |
  	| karbantartása   | `%%cleanup -f`                    | Az aktuális Livius végpontot, beleértve a jegyzetfüzet-munkamenetet a munkamenetek törli. A kötelező jelző -f kötelező.  |

    >[AZURE.NOTE] Mellett a magics a PySpark kernel által hozzáadott, is használhatja a [beépített IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), beleértve a `%%sh`. Használhatja a `%%sh` mágikus a fürt headnode parancsfájlok és kódrészlet futtatásához. 

3. **Automatikus megjelenítést**. A **Pyspark** kernel automatikusan megjeleníti a struktúra, és az SQL-lekérdezés eredménye. Ha választhatja ki a képi megjelenítéseket, beleértve a táblázat, kör, vonal-, terület-, sáv számos különböző típusú között.

## <a name="parameters-supported-with-the-sql-magic"></a>A támogatott paramétereket a %% sql mágikus

A %% sql mágikus támogatja a különböző paraméterek használó lekérdezések futtatásakor kapott kimeneti típusának szabályozásához. Az alábbi táblázat a kimenet.

| Paraméter     | Példa                         | Leírás  |
|-----------|---------------------------------|--------------|
| o –      | `-o <VARIABLE NAME>`                          | A lekérdezés eredményét a probléma továbbra is fennáll a paraméter használatával a %% helyi Python környezetben, mint egy [Pandas](http://pandas.pydata.org/) dataframe. A dataframe változó neve a megadott változóinak nevéből. |
| -kérdések      | `-q`                          | Ezzel kapcsolja ki a képi megjelenítések cellájára. Ha nem szeretné egy cella tartalmának automatikus ábrázolása, és csak egy dataframe felvétele, majd használja szeretné `-q -o <VARIABLE>`. Ha szeretné-e az eredmények rögzítése nélkül kapcsolja ki a képi megjelenítések (például SQL-lekérdezés futtatása az oldalsó effektusok, például egy `CREATE TABLE` utasítás), egyszerűen `-q` nélkülire egy `-o` argumentum. |
| -m       |  `-m <METHOD>`    | **Ha módja **venni** vagy a **minta** ** (az alapértelmezett érték **készítése**). Ha a módszer az **venni**, a kernel MaxRows értéke (később az alábbi táblázatban ismertetett) által megadott adatok eredményhalmaz tetején elemek választja ki. **Példa**a mód esetén a kernel véletlenszerűen minta az adathalmaz következők szerint elemei `-r` paraméter, ezután az alábbi táblázatban felsorolt.   |
| -r     |     `-r <FRACTION>`            | **Itt tört_nevező 0.0 és 1,0 közötti lebegőpontos szám.** Ha a minta az SQL-lekérdezés módja `sample`, majd a kernel véletlenszerűen minták a megadott részét, az eredmény beállításában; az elemeket például ha az argumentumokkal egy SQL-lekérdezés futtatása `-m sample -r 0.01`, majd az eredmény sorok 1 %-os véletlenszerűen mintát. |
| -n      | `-n <MAXROWS>`                        | **MaxRows értéke** nem egész szám érték. A kernel korlátozza a **MaxRows értéke**kimeneti sorok számát. Ha **a MaxRows értéke** negatív szám, például a **-1**, akkor az eredményhalmaz sorok száma nem lesz korlátozott. |

**Példa:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

A fenti utasítás az alábbi műveleteket végzi el:

* **Hivesampletable**minden rekord kijelölése
* Mivel használjuk - kérdések, automatikus-megjelenítés kikapcsolja.
* Mivel használjuk `-m sample -r 0.1 -n 500` véletlenszerűen 10 %-a hivesampletable sora minták és 500 sorok eredményhalmazt méretének korlátozza.
* Végül mivel ez `-o query2` a kimenet be egy úgynevezett **lekérdezés2**dataframe is menti.
    

## <a name="considerations-while-using-the-new-kernels"></a>Az új mag használata során megfontolandó szempontok

Bármelyik kernel használata (PySpark vagy külső), hagyja a jegyzetfüzeteket, futó felhasználja fürt erőforrásait.  Az alábbi mag, a környezetek jelenjenek meg, mert egyszerűen Kilépés a jegyzetfüzetek nem törlése a környezet és így a fürt erőforrások továbbra is használhatja. Tanácsos új célközönségszabályt felvenni a PySpark és a külső mag és a **Bezárás gombra, és leállítja** a beállítással a Jegyzetfüzet **fájl** menüből is. Ez az űrlapokat állítjuk leáll a környezetben, és majd kilép a jegyzetfüzetet.    


## <a name="show-me-some-examples"></a>Néhány példa megjelenítése

Amikor megnyit egy Jupyter jegyzetfüzetet, látni fogja a két a gyökérszinten elérhető mappák.

* A **PySpark** mappába, amelyek az új **Python** kernelt minta jegyzetfüzetek tartalmaz.
* A **Scala** mappába, amelyek az új **külső** kernelt minta jegyzetfüzetek tartalmaz.

Ha többet szeretne tudni a rendelkezésre álló különböző magics **PySpark** vagy **külső** mappából megnyithatja a **00 – [további ÉN első] külső mágikus Kernel szolgáltatások** jegyzetfüzetet. A többi elérhető minta jegyzetfüzetek csoportban a két mappa megtudhatja, hogy miként HDInsight külső fürt Jupyter jegyzetfüzetek használata különböző forgatókönyvek eléréséhez is használhatja.

## <a name="where-are-the-notebooks-stored"></a>Hol tárolják a jegyzetfüzeteket?

Jegyzetfüzetek Jupyter menti a program a **/HdiNotebooks** mappán fürthöz társított tárterület-fiókjába.  Jegyzetfüzetek, fájlok és mappák alapján létrehozott Jupyter belül lesz elérhető a WASB.  Ha például Jupyter használatával hozzon létre egy mappát **SajátMappa** és a Jegyzetfüzet **myfolder/mynotebook.ipynb**, érhető el az adott jegyzetfüzet, a `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  A Fordított sorrend szintén igaz, ez azt jelenti, hogy közvetlenül az a tárterület-fiókjába a jegyzetfüzet a feltöltött `/HdiNotebooks/mynotebook1.ipynb`, a jegyzetfüzetet, valamint Jupyter láthatók lesznek.  Jegyzetfüzetek után a program törli a fürt tároló fiók marad.

A tárterület-fiókba mentett jegyzetfüzeteket legegyszerűbben Fájlrendszerhez kompatibilis. Így, ha a fürt is használhatja az SSH fájl kezeléséhez szükséges parancsokat a következőhöz hasonló:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Abban az esetben, ha szeretne hozzáférni a tárterület-fiókjához a fürt problémák lépnek fel, a jegyzetfüzetek is menti a program a headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Támogatott böngészőt
Futó HDInsight külső fürt Jupyter jegyzetfüzetek csak a Google Chrome támogatottak.

## <a name="feedback"></a>Visszajelzés

Az új mag változó szakaszában találhatók, és az idő fog esedékes. Jelentheti a is, hogy API-hoz, ezek mag érett változhat. Nagyra értékeljük szeretné az esetleges visszajelzéseit a következő új mag használata közben van. Ez nagyon hasznos a végleges verziójának megjelenése után következő mag kialakításában lesz. A megjegyzések visszajelzését hagyhatja, ha ez a cikk alján a **Megjegyzések** csoportban.


## <a name="seealso"></a>Lásd még:


* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával önálló-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Külső csomagok Jupyter jegyzetfüzeteket használata](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)
