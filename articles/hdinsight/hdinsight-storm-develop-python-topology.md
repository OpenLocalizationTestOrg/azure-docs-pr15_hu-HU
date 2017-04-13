<properties
   pageTitle="Python összetevők használja a HDinsight vihar topológiában |} Microsoft Azure"
   description="Megtudhatja, hogyan használhatja Python összetevőinek a Apache vihar Azure hdinsight szolgáltatáshoz a. Megtanulhatja, hogy mindkét egy Java alapú Python összetevőinek használatához, és Clojure vihar topológia alapján."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Apache vihar topológiák Python használata HDInsight kidolgozása

Apache vihar több nyelvet is támogat, még akkor is lehetővé teszi egyesítheti több nyelven egy topológiában összetevőinek. A jelen dokumentum megtanulhatja Python összetevők használatáról a Java- és Clojure-alapú vihar topológiák a hdinsight szolgáltatásból lehetőségre.

##<a name="prerequisites"></a>Előfeltételek

* Python 2.7 vagy újabb verzió

* Java JDK 1.7 vagy újabb

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Vihar több nyelv támogatása

Vihar programmal írtak bármilyen értékű programnyelv, azonban ehhez az, hogy az összetevők megtudhatja, hogyan működik együtt a [definícióját Thrift vihar](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)összetevők végezhető készült. A modul Python, a Apache vihar projektet, amely lehetővé teszi, hogy könnyen felülete, amelyen vihar részeként megadását. Ez a modul [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py)címen található.

Mivel Apache vihar egy Java folyamat, amely a Java virtuális gép (JVM) fut, a más nyelven írt összetevők mint alfolyamatok végre kell hajtani. A JVM futó vihar bit ezek alfolyamatok keresztüli stdin/stdout JSON üzenetküldés használatával kommunikál. További részletek összetevők közötti kommunikációt [többszörös-lang protokoll](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentációjában találhatók.

###<a name="the-storm-module"></a>A vihar modul

A vihar modul (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py,) a Python összetevők vihar verzióval használható létrehozásához szükséges bittel biztosít.

Ezzel a megoldással például `storm.emit` elhelyezése kockára, hogy és `storm.logInfo` a naplókat írni. Volna ösztönzése, ezzel a fájllal fölé és érthetőbbé, mit tartalmaz.

##<a name="challenges"></a>Kihívásokkal kapcsolatban

__Storm.py__ modulról is létrehozhat, amelyek felhasználása az adatokat, és csapszegek, amely adatfeldolgozás, azonban a teljes vihar topológia definícióját felfelé összetevők közötti kommunikáció huzalok továbbra is íródott Java vagy Clojure Python spouts. Ezenkívül Java használata esetén meg is létre kell hoznia Java-összetevők, amely a használni kívánt felületet az Python összetevőket.

Is mivel vihar fürt elosztott módon futtatni, győződjön meg arról, hogy a Python összetevők által igényelt modulokat érhetők el a fürt csomópontjait dolgozó. Vihar nem nyújt bármely egyszerűen többszörös-lang erőforrások ehhez – minden függőség szerepeltetni a topológia üveg fájlt, vagy manuálisan telepíteni a függőségek minden dolgozó csomóponton a fürt vagy van.

###<a name="java-vs-clojure-topology-definition"></a>Java összehasonlítása Clojure topológia meghatározása

A két módszer a topológia meghatározása Clojure szerint, amennyiben a legegyszerűbb/cleanest, mint közvetlenül referenc python összetevőinek a topológia definícióban. Java-alapú topológia definíciók meg kell határoznia kezelje a dolgokat, mint a kockára mezőinek deklarálása a Python összetevők által eredményül adott Java-összetevőit is.

Ehhez a dokumentumhoz példa projektek mindkét módszerhez témakörben olvashat.

##<a name="python-components-with-a-java-topology"></a>Egy Java topológia Python összetevők

> [AZURE.NOTE] Ebben a példában a [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) a __JavaTopology__ címtárban címen érhető el. Ez a projekt alapján maven tesztelése. Ha nem ismeri maven tesztelése, olvassa el a [kidolgozása Java-alapú topológiák Apache vihar a HDInsight az](hdinsight-storm-develop-java-topology.md) egy vihar topológia maven tesztelése projektté létrehozásával kapcsolatban további információt.

Java-alapú topológia, Python (vagy más JVM nyelvi összetevők) használó kezdetben Java összetevőket; használandó jelenik meg de jelenik meg az egyes spouts/csapszegek Java, látni fogja az alábbihoz hasonló kódot:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Ez pedig Java Python elindítja a parancsfájlt, amely tartalmazza a tényleges rögzített logika futtatja. A Java spouts/csapszegek (például a) egyszerűen deklarálása a sor bármelyik eleme, hogy a program a Python összetevőjének által kibocsátott mezőjét.

A tényleges Python fájlok vannak tárolva a `/multilang/resources` könyvtár ebben a példában. A `/multilang` címtár hivatkozott a __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>{basedir} $/ multilang</directory>
    </resource>
</resources>

Ide tartoznak a lévő összes fájl a `/multilang` a üveg, hogy a projekt épül mappájában.

> [AZURE.IMPORTANT] Megjegyzendő, hogy ez csak adja meg a `/multilang` könyvtár és nem `/multilang/resources`. Vihar vár nem JVM erőforrások egy `resources` címtár, így azt van már belső keresi. Úgy, hogy a összetevők ebbe a mappába lehetővé teszi a hivatkozás csak a Java-kód név szerint. Ha például `super("python", "countbolt.py");`. Tekintsen úgy, hogy a úgy, hogy vihar láthatja a `resources` könyvtár a legfelső szintű (/) többszörös-lang erőforrások elérése.
>
> Ez a példa projekthez a `storm.py` modulban található a `/multilang/resources` címtár.

###<a name="build-and-run-the-project"></a>Készítése és a projekt futtatása

Ehhez a projekthez helyileg futtatásához csak a következő paranccsal maven tesztelése létre, és a helyi módban fut:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

A folyamat leállítása használata a ctrl + c billentyűkombinációt.

A projekt-Apache vihar rendszerű HDInsight fürtre telepítéséhez kövesse az alábbi lépéseket:

1. Egy uber üveg össze:

        mvn package

    Ezzel hoz létre egy __WordCount – 1.0-SNAPSHOT.jar__ a nevű fájlt a `/target` könyvtár ehhez a projekthez.

2. Töltse fel a üveg fájlt a Hadoop fürt, az alábbi módszerek egyikével:

    * __Linux-alapú__ HDInsight fürtre vonatkozóan: használata `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` másolhatja a üveg a fürt helyettesítve a felhasználónév SSH felhasználónevét és CLUSTERNAME HDInsight fürt nevű.

        Amikor a fájl befejeződik feltöltése, csatlakozzon a fürthöz SSH használatával, és a topológia használatának megkezdése`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * __Windows-alapú__ HDInsight fürtre vonatkozóan: Csatlakozás a vihar irányítópult HTTPS://CLUSTERNAME.azurehdinsight.net/ nyissa meg a böngészőben. CLUSTERNAME cserélje le a HDInsight fürt nevére, és adja meg a rendszergazda nevét és jelszavát, amikor a rendszer kéri.

        A képernyő használatával, hajtsa végre az alábbi műveletek:

        * __Fájl JAR__: válassza a __Tallózás gombra__, majd jelölje ki a __WordCount-1.0-SNAPSHOT.jar__ fájlt
        * __Osztály neve__: írja be`com.microsoft.example.WordCount`
        * __További jólétéhez__: írja be például a felhasználóbarát név `wordcount` a topológia azonosítása

        Végül jelölje be a __Küldés gombra__ kattintva indítsa el a topológia.

> [AZURE.NOTE] Miután elindult, a egy vihar topológia fut, amíg a Leállítva (el). A következőképpen leállítja a topológiát, a `storm kill TOPOLOGYNAME` parancs a munkamenetből parancssori (SSH Linux fürtre például) vagy a felhasználói felület vihar használatával jelölje ki a topológia, és válassza a __törlése__ gombra.

##<a name="python-components-with-a-clojure-topology"></a>Egy Clojure topológia Python összetevők

> [AZURE.NOTE] Ebben a példában a [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) a __ClojureTopology__ címtárban címen érhető el.

Ez a topológia [Leiningen](http://leiningen.org) használatával hozhat [létre új projektet Clojure](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)készült. Ezt követően a scaffolded projekthez a következő módosításokat végzett:

* __Project.CLJ__: függőségek hozzáadott vihar és elemek, előfordulhat, hogy okozó probléma esetén a HDInsight-kiszolgálóra telepített egésze.
* __erőforrások és erőforrások__: Leiningen létrehoz egy alapértelmezett `resources` könyvtár, azonban a itt tárolt fájlok jelennek meg, a rendszer hozzáadja az ehhez a projekthez létrehozott üveg fájl legfelső szintű és vihar vár alszint nevű könyvtárában található fájlok `resources`. Alszint könyvtárában hozzáadva, és a Python fájlok vannak tárolva `resources/resources`. Futásidőben a rendszer értékként kezeli a legfelső szintű (/) Python összetevők eléréséhez.
* __src/WordCount/Core.CLJ__: ezt a fájlt tartalmazza, a topológia definícióját, és hivatkozott __project.clj__ fájlból. Clojure használatával egy vihar topológia meghatározása a további tudnivalókért olvassa el a [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html)című témakört.

###<a name="build-and-run-the-project"></a>Készítése és a projekt futtatása

__Létre, és futtassa a projekt helyi meghajtóra__, használja az alábbi parancsot:

    lein clean, run

A topológia leállításához használja __Ctrl + C billentyűkombinációt__.

__Egy uberjar létre és helyezhetnek üzembe, hdinsight szolgáltatáshoz__, kövesse az alábbi lépéseket:

1. Hozzon létre egy a topológia és függőség tartalmazó uberjar:

        lein uberjar

    Ez hoz létre egy új nevű fájlt `wordcount-1.0-SNAPSHOT.jar` a a `target\uberjar+uberjar` címtár.
    
2. Telepíthető, és futtassa a topológia HDInsight fürthöz használja az alábbi lehetőségek közül:

    * __Linux-alapú hdinsight szolgáltatáshoz__
    
        1. Másolja át a fájlt a HDInsight fürt fő csomópont használatával `scp`. Példa:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            FELHASZNÁLÓNÉV helyére egy SSH felhasználóval, a csoportját, és a CLUSTERNAME a HDInsight fürt nevére.
            
        2. Ha a fájlt a fürt másolták, használatával SSH a fürthöz és a feladat elküldése. A HDInsight SSH használja a további tudnivalókért lásd a következők valamelyikét:
        
            * [Linux-alapú HDInsight Linux, Unix vagy OS X SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [A Windows Linux-alapú HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Miután létrejött, a következő segítségével a topológia indítása:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windows-alapú hdinsight szolgáltatáshoz__
    
        1. Csatlakozás a vihar irányítópult HTTPS://CLUSTERNAME.azurehdinsight.net/ nyissa meg a böngészőben. CLUSTERNAME cserélje le a HDInsight fürt nevére, és adja meg a rendszergazda nevét és jelszavát, amikor a rendszer kéri.

        2. A képernyő használatával, hajtsa végre az alábbi műveletek:

            * __Fájl JAR__: válassza a __Tallózás gombra__, majd jelölje ki a __wordcount-1.0-SNAPSHOT.jar__ fájlt
            * __Osztály neve__: írja be`wordcount.core`
            * __További jólétéhez__: írja be például a felhasználóbarát név `wordcount` a topológia azonosítása

            Végül jelölje be a __Küldés gombra__ kattintva indítsa el a topológia.

> [AZURE.NOTE] Miután elindult, a egy vihar topológia fut, amíg a Leállítva (el). A következőképpen leállítja a topológiát, a `storm kill TOPOLOGYNAME` parancs a munkamenetből parancssori (SSH a Linux fürtre) vagy a felhasználói felület vihar használatával jelölje ki a topológia, és válassza a __törlése__ gombra.

##<a name="next-steps"></a>Következő lépések

A jelen dokumentum Python összetevők használatáról a egy vihar topológia megtanulta azt. Lásd: a következő dokumentumok szolgáltatást mellőző formáit HDInsight Python használata:

* [A folyamatos átvitelű MapReduce feladatok Python használatáról](hdinsight-hadoop-streaming-python.md)
* [Python felhasználó által definiált függvények (UDF) malac és struktúra használata](hdinsight-python.md)
