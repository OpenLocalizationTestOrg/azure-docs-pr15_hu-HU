<properties
    pageTitle="R HDInsight Linux-alapú telepítés |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti és R segítségével Linux-alapú Hadoop fürt."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Telepítése és R HDInsight Hadoop fürt használata

A HDInsight Hadoop-fürt bármilyen típusú R telepíthető **Parancsfájl műveletet** fürt testreszabási használatával. Ezzel az adatok tudósok és munkatársai a hatékony MapReduce/fonal programozási keretrendszer R használatával a nagy mennyiségű adatot Hadoop fürt HDInsight telepített a folyamat.

> [AZURE.IMPORTANT] A HDInsight kínáló [prémium réteg](https://azure.microsoft.com/pricing/details/hdinsight/) R kiszolgáló a HDInsight fürt részeként tartalmazza. Ebben a csoportban adhatja R parancsfájlok MapReduce és a külső segítségével elosztott számítások futtathatja. További tudnivalókért olvassa el a [R Server HDInsight használatának első lépései](hdinsight-hadoop-r-server-get-started.md)című témakört. 


## <a name="what-is-r"></a>Mi az R?

A <a href="http://www.r-project.org/" target="_blank">Projekt R statisztikai számítások</a> egy megnyitott Forrásnyelv és környezet statisztikai számítások. R több száz beépített statisztikai függvények és a saját programnyelv funkcionális és objektumorientált programozáshoz tulajdonságát kombináló biztosít. Teljes körű grafikus funkciókat is tartalmaz. R a legtöbb profi statisztikusok és tudósok számos különböző típusú mezők az előnyben részesített programozási környezetben.

R parancsfájlok futtatását is lehetővé teszi a Hadoop fürt HDInsight az, hogy testre művelettel parancsfájl létrehozásakor a R környezet telepítéséhez. R a kompatibilis az Azure Blob-tároló (WASB), így ott tárolt adatokhoz R használata HDInsight lehet feldolgozni.

## <a name="what-the-script-does"></a>Mit jelent a parancsprogram

A parancsprogram művelet használta az R a HDInsight fürt telepíti az egyszerű R telepítés következő Ubuntu csomagok:

* [r alapul](http://packages.ubuntu.com/precise/r-base): Alap GNU R csomag
* [a fejlesztői alap r](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R csomagok

A következő RHadoop csomagokban is telepítve van, amely integrációt biztosítanak MapReduce és Fájlrendszerhez:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): lehetővé teszi, hogy a R fejlesztők Hadoop MapReduce használata
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): lehetővé teszi, hogy a fejlesztők R Hadoop Fájlrendszerhez (HDInsight-WASB) használata

Emellett a következő R csomagok vannak telepítve:

| R-csomag | Mit tartalmaz |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Alacsony R Java felületére. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R és C++ integráció. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Objektumok R szerializálni/deszerializálni JSON-ba |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Egész módszerek bitenként műveleteket függvények. |
| [kivonat](https://cran.r-project.org/web/packages/digest/index.html) | Titkosított kivonat emésztett R-objektumok létrehozása. |
| [funkcionális](https://cran.r-project.org/web/packages/functional/index.html) | Curry, írása és más magasabb sorrendben függvény |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Rugalmasan átalakítása és összesített adatok. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Egyszerű, egységes csomagolást közös karakterlánc műveletekhez. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Felosztás, alkalmazása és kombinációit eszközöket. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Ellenőrző eszközök áthelyezése ablak statisztika, GIF, Base64, ROC AUC, stb. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Karakterlánc egyező közelíteni, és távolság függvények karakterlánc. |

## <a name="install-r-using-script-actions"></a>Telepítse az R parancsfájl-műveletek használata

A következő parancsfájl műveletet R telepítése egy HDInsight fürthöz szolgál. https://hdiconfigactions.BLOB.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.SH
    
Ez a témakör használatáról a parancsfájl létrehozása az Azure portálon új fürt utasításokat. 

> [AZURE.NOTE] Azure PowerShell, az Azure CLI, a HDInsight .NET SDK vagy Azure erőforrás-kezelő sablonokat is használható parancsfájl műveletet. Parancsfájl-műveletek alkalmazása már fut fürt. További tudnivalókért lásd: a [parancsprogram-műveleteinek testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md).

1. Indítsa el a kiépítési fürt [rendelkezést Linux-alapú HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)az lépésekkel, de ne hajtsa végre a kiépítési gombra.

2. Kattintson a **Választható beállítási** lap jelölje be a **Parancsfájl-műveletek**, és az alábbi adatokat:

    * __Név__: Adjon meg egy rövid nevet, a parancsprogram művelet.
    * __Parancsfájl URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Címsor__: ezt a jelölőnégyzetet
    * __Dolgozó__: ezt a jelölőnégyzetet
    * __ZOOKEEPER__: ezt a jelölőnégyzetet a Zookeeper csomópontra telepítéséhez.
    * __Paraméterek__: hagyja üresen a mezőt

3. A képernyő alján a **Parancsfájl-műveletek**használja a **Kijelölés** gombot a konfiguráció mentéséhez. Végezetül használja a **Kijelölés** gombot a **Választható beállítási** a lap alján a választható konfigurációs adatok mentéséhez.

4. Továbbra is a fürt kiépítési, [rendelkezést Linux-alapú HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)leírt módon.

## <a name="run-r-scripts"></a>R parancsfájlok futtatása

Miután a fürt befejeződött kiépítési, kövesse az alábbi lépéseket MapReduce végrehajtása a fürt R használatával.

1. A HDInsight fürt SSH használatával csatlakozhat:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    A HDInsight SSH használja a további tudnivalókért lásd: az alábbi:

    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Az a `username@hn0-CLUSTERNAME:~$` kéri, írja be a interaktív R indítása a következő parancsot:

        R

3. Írja be a következő R programot. A számok, 1 és 100 hoz létre, és a őket majd megszorzása 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Az első sor hívja fel a RHadoop tár rmr2, amelyet az MapReduce műveletek.

    A második sorban hoz létre a 1-100, értéket, majd a tárolja őket a Hadoop fájl rendszer használatával `to.dfs`.

    A harmadik sorban létrehoz egy MapReduce folyamatot rmr2 által biztosított funkció használata, és megkezdi a feldolgozása. Meg kell jelennie múltbeli görgessen a feldolgozás kezdődik, több sor.

4. Ezután a következő használatával megtekintheti az ideiglenes elérési utat a MapReduce kimeneti tárolt:

        print(calc())

    El kell végezni hasonló `/tmp/file5f615d870ad2`. A tényleges kimenet megjelenítéséhez használja a következő:

        print(from.dfs(calc))

    A kimenet így néz ki:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Kilépés R írja be a következőket:

        q()


## <a name="next-steps"></a>Következő lépések

- [Telepítési és használati HDInsight a szín fürtök](hdinsight-hadoop-hue-linux.md). Szín egy webhely felhasználói Felületét, amely egyszerűen hozhat létre, futtatni és mentés malac és a struktúra feladatok, és keresse meg az alapértelmezett tárhely a HDInsight fürt.

- [A HDInsight fürt telepítése Giraph](hdinsight-hadoop-giraph-install.md). Fürt testreszabási segítségével HDInsight Hadoop fürt Giraph telepítése. Giraph lehetővé teszi, hogy Hadoop használatával graph feldolgozás végezheti el, és az Azure hdinsight szolgáltatáshoz is használható.

- [A HDInsight fürt telepítése Solr](hdinsight-hadoop-solr-install.md). Fürt testreszabási segítségével HDInsight Hadoop fürt Solr telepítése. Solr lehetővé teszi a tárolt adatok hatékony keresés műveletek hajthatók végre.

- [A HDInsight fürt telepítése szín](hdinsight-hadoop-hue-linux.md). Fürt testreszabási segítségével HDInsight Hadoop fürt szín telepítése. Szín webalkalmazások kezelése a Hadoop fürtre használt áll.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
