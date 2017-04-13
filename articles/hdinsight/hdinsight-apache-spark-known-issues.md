<properties 
    pageTitle="Ismert problémák a Apache külső a HDInsight |} Microsoft Azure" 
    description="Ismert problémák a Apache külső HDInsight a." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Ismert problémák a HDInsight Linux Apache külső fürthöz

A dokumentum nyomon követi az összes ismert problémáról HDInsight külső nyilvános előzetes verziójának.  

##<a name="livy-leaks-interactive-session"></a>Livius elveszít interaktív munkamenet
 
Egy interaktív munkamenetben történik (Ambari vagy headnode 0 virtuális számítógép újraindítása miatt) életben Livius újraindítása után egy interaktív feladat munkamenet szivárgását okozhatja. Emiatt az új feladatok is problémákat tapasztal, elfogadott állapotú, és nem indítható el.

**Kezelési:**

Az alábbi eljárással a probléma megoldása:

1. Ssh headnode be. 
2. A következő parancsot a Alkalmazásazonosítók keresztül Livius lépések interaktív feladatok kereséséhez. 

        yarn application –list

    A nevek lesz Livius, ha a feladatok lettek lépései a Livius interaktív munkamenet explicit nevek nélküli megadva, az Livius munkamenet Jupyter jegyzetfüzet által indított alapértelmezett feladat, a projekt nevét a remotesparkmagics_ * elindul. 

3. Ezeket a feladatokat törlése a következő parancs futtatásával. 

        yarn application –kill <Application ID>

Új feladat futtatása elindul. 

##<a name="spark-history-server-not-started"></a>A külső előzmények kiszolgáló nem kezdődött el 

A külső előzmények kiszolgáló nem automatikusan indul fürt létrehozását követően.  

**Kezelési:** 

Manuálisan indítsa el az előzmények kiszolgáló Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>A külső naplózási könyvtár jogosultsági problémát 

Ha hdiuser elküld egy feladat spark-submit, van-e egy hiba java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (hozzáférés megtagadva), és a illesztőprogram jelentkezzen be nem írja. 

**Kezelési:**
 
1. Hdiuser hozzáadása a Hadoop-csoporthoz. 
2. Adja meg a 777 engedélyek /var/log/spark a csoport létrehozását követően. 
3. Frissítse a külső napló helyének lehet 777 engedélyekkel rendelkező könyvtár Ambari használatával.  
4. Futtatás külső-küldhetnek sudo szerint.  

## <a name="issues-related-to-jupyter-notebooks"></a>Jegyzetfüzetek Jupyter kapcsolatos problémák

Az alábbiakban néhány Jupyter jegyzetfüzetek kapcsolatos ismert problémákat.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>-ASCII karakterek fájlnevekben jegyzetfüzetek

A külső HDInsight fürt használható Jupyter jegyzetfüzetek nem kell-ASCII karakterek fájlnevekben. Ha megpróbálja a Jupyter felhasználói Felülettel, amely-ASCII fájlnév mellett – fájl feltöltése, meghiúsul csendes (Ez azt jelenti, hogy Jupyter nem engedi töltse fel a fájlt, de azt nem lehet egy látható hiba vagy throw). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Hiba történt a nagyobb méretű jegyzetfüzetek betöltése

Hiba jelenhet meg **`Error loading notebook`** mikor töltődik be, amelyek a nagyobb méretű jegyzetfüzeteket.  

**Kezelési:**

Ha ezt a hibát, azt jelenti az adatok sérült vagy elvesznek.  A jegyzetfüzetek vannak még a lemezen `/var/lib/jupyter`, és SSH is be a fürt érheti el őket. Átmásolhatja a jegyzetfüzetek a fürt a helyi számítógépre (SCP vagy WinSCP segítségével) a jegyzetfüzet bármelyik fontos adatokat az adatvesztés elkerülése érdekében másolatként. Azt is megteheti majd SSH alagutas be a port 8001 anélkül, hogy az átjáró Jupyter eléréséhez a headnode.  Itt törölje a jelet a kimenet arra a jegyzetfüzetre, és mentse újra a jegyzetfüzet méretének csökkentése érdekében.

Megakadályozhatja, hogy ez a hiba történik a jövőben, kövesse az ajánlott eljárásokat:

* Fontos megtartása kisméretű a jegyzetfüzet méretét. A külső feladatok vissza Jupyter küldött kimenetét a jegyzetfüzet van állandó.  Az általános célszerű a legjobb Jupyter futó elkerülése érdekében `.collect()` a nagy RDD vagy dataframes; Ehelyett, ha szeretne egy RDD tartalom megtekintése, fontolja meg a futó `.take()` vagy `.sample()` , hogy a kibocsátás nem jutnak túl nagy.
* Is amikor menti a jegyzetfüzet, törölje a jelet minden kimeneti cellák méretének csökkentése érdekében.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Jegyzetfüzet kezdeti indítási vártnál hosszabb ideig tart 

Külső mágikus Jupyter jegyzetfüzetnek kód utasításában több, mint egy perc is eltelhet.  

**Magyarázat:**
 
Ennek oka az, hogy az első kód cellát futtatásakor. A háttérben kezdeményez munkamenet konfigurálása és a külső, SQL, és a struktúra környezetek vannak beállítva. Után ezek a környezetek vannak beállítva, az első utasítás fut, és ezt azt sugallja az, hogy a kimutatás került a hosszú időt vesz igénybe.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jegyzetfüzet időtúllépés Jupyter a munkamenet létrehozása

Külső fürt kívül erőforrások esetén a külső és Pyspark mag Jupyter jegyzetfüzetben lesz a időtúllépés létrehozni a munkamenetet. 

**Megoldásokkal kapcsolatban:** 

1. Szabadítson fel a külső fürt egyes erőforrások:

    - Egyéb külső jegyzetfüzetek megjegyzésmezőre látogasson el a Bezárás gombra, és leállítja a menüben, vagy kattintson a leállítás a jegyzetfüzet Intéző.
    - Egyéb külső alkalmazásokban fonálból leállítása.

2. Indítsa újra a használatba veszik a kívánt jegyzetfüzetet. Kell, hogy elég erőforrások érhető el, hogy most munkamenet létrehozása.

##<a name="see-also"></a>Lásd még:

* [Áttekintés: A külső Apache a Azure hdinsight szolgáltatáshoz](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Felhasználási területei

* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)

* [A külső és gépi tanulási: használata külső a HDInsight épület hőmérsékleti fűtés-és Légtechnikai adatok elemzéséhez](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)

* [Webhely napló analysis HDInsight külső használata](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Létrehozása és futtatása alkalmazások

* [Scala használatával standalone-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)

* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények

* [Létrehozása és elküldése külső Scala alkalmazást IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin.md)

* [A külső alkalmazások távolról hibáinak IntelliJ arról HDInsight eszközök beépülő modul használatával](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [A HDInsight külső fürt Zeppelin jegyzetfüzetek használata](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Elérhető az HDInsight-külső fürthöz Jupyter jegyzetfüzet mag](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Külső csomagok Jupyter jegyzetfüzeteket használata](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter telepítése a számítógépen, és csatlakozzon az HDInsight külső fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése

* [A Apache külső fürt Azure hdinsight szolgáltatáshoz a források kezelése](hdinsight-apache-spark-resource-manager.md)

* [A a HDInsight-Apache külső fürthöz nyomon követése és hibakeresési feladatok](hdinsight-apache-spark-job-debugging.md)
