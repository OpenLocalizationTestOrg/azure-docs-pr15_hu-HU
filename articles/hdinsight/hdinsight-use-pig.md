<properties
   pageTitle="Hadoop malac használata a HDInsight |} Microsoft Azure"
   description="Megtudhatja, hogy miként malac használata Hadoop a hdinsight szolgáltatásból lehetőségre."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>A HDInsight Hadoop malac használata

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache malac](http://pig.apache.org/) egy platform Hadoop-programok lépésről neve *malac latin betűs*nyelvek használatával hozhat létre. Malac Java alternatívájaként *MapReduce* megoldások hozhat létre, és megtalálható az Azure hdinsight szolgáltatásból lehetőségre.

Ebben a cikkben megismerheti használatának malac a hdinsight szolgáltatásból lehetőségre.

##<a id="why"></a>Miért érdemes használni a malac?

A feldolgozás összefüggés csak egy térképen és csökkentése függvény használatával végrehajtó adatfeldolgozás Hadoop MapReduce használatával a problémáit közül. Összetett feldolgozásra gyakran kell feldolgozás szétválasztani több MapReduce műveletek vannak módszere során össze a kívánt eredmény eléréséhez.

Helyett kényszerítése, hogy csak a dokumentumot, és csökkentheti a függvények, Malac teszi feldolgozás meghatározásával átalakítások kapcsol a kívánt kimeneti keresztül folyó az adatok sorozata.

Malac Latin nyelv ismertetik a nyers adatok bevitele, egy vagy több átalakítások kapcsol a kívánt kimeneti keresztül az adatfolyam teszi lehetővé. Malac Latin programok ez általános minta hajtsa végre:

- **Betöltési**: olvassa el az adatok kezelhetők a fájlrendszerből
- **Átalakítás**: az adatok módosítására
- **Kiírása vagy a tár**: kimeneti adatok a képernyőre, és tárolja azt feldolgozásra

Malac latin betűs is támogatja a felhasználó által definiált függvényeket (UDF), amely lehetővé teszi, külső összetevőket malac Latin modell nehezen logikájának megvalósítása meghívásához.

Malac Latin kapcsolatos további tudnivalókért olvassa el a [malac Latin hivatkozás kézi 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) és [malac Latin hivatkozás kézi 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html)című témakört.

Példa a malac UDF használja a következő dokumentumokat talál:

* [Használata DataFu a malac a HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu Apache által fenntartott hasznos UDF gyűjteménye

* [Malac és struktúra HDInsight Python használata](hdinsight-python.md)

* [C# struktúra és használhatja a HDInsight malac](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Tudnivalók a mintaadatok

Ebben a példában a minta van tárolva, **/example/data/sample.log** a blob-tároló tárolóban *log4j* fájlok. A fájlban rögzít egy sort tartalmazó mezők áll egy `[LOG LEVEL]` mező megjelenítéséhez a típus és súlyosságát, például:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Az előző példában a naplózási szint jelző hiba.

> [AZURE.NOTE] Is készíthet log4j fájl az [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) naplózás eszközzel, és kattintson a fájl feltöltése a a blob. [Töltse fel adatok hdinsight szolgáltatáshoz](hdinsight-upload-data.md) talál utasításokat. Hogyan használják tárolója Azure BLOB HDInsight kapcsolatos további tudnivalókért olvassa el a [Használata Azure Blob-tárolóhoz a HDInsight](hdinsight-hadoop-use-blob-storage.md)című témakört.

A mintaadatok Azure Blob-tárolóhoz, amely HDInsight Hadoop fürtre vonatkozóan használja az alapértelmezett fájlrendszer vannak tárolva. A **wasb** előtag használatával BLOB tárolt fájlok HDInsight érheti el. Például a sample.log fájl elérésére, használja a következő szintaxist:

    wasbs:///example/data/sample.log

Mivel a WASB HDInsight alapértelmezett tárolt adatait, **/example/data/sample.log** a malac Latin használatával is elérheti a fájlt.

> [AZURE.NOTE] Az alábbi, **wasbs: / / /**, használja az alapértelmezett tároló tárolóban a HDInsight fürt tárolt fájlok eléréséhez. További tárterület fiókok megadott telepítésekor, akkor a fürt, és el szeretne érni, az alábbi fiókok tárolt fájlok, érhető el az adatokat tároló nevét és tároló fiók címét, például megadásával: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>A minta feladattal kapcsolatos

A következő malac Latin feladatot a **sample.log** fájlt a HDInsight fürt alapértelmezett tárhelyről tölt be. Végrehajtja, amelyeknél a hányszor minden naplózási szintjének történt, a bemeneti adatok számának átalakítások sorozata. Az eredmények STDOUT vannak elhelyezve.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Az alábbi képen látható az egyes átalakítási leírása az adatok bontásban.

![Az átalakítás grafikus ábrázolása][image-hdi-pig-data-transformation]

##<a id="run"></a>A malac Latin feladat futtatása

HDInsight futtatását is lehetővé teszi a malac Latin feladatok többféle módszerrel használatával. Döntse el, melyik módszert szüksége az alábbi táblázat segítségével, majd hajtsa végre az útmutató hivatkozását.

| **Ezzel a** ...                                   | .. .an **interaktív** rendszerhéj | ... **parancsfájl** | .. .with **fürt operációs rendszer** | .. .from **ügyfél operációs rendszer** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [Curl](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux vagy a Windows                          | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [.NET SDK Hadoop-](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux vagy a Windows                          | Windows (egyelőre)                        |
| [A Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux vagy a Windows                          | A Windows                                  |
| [Távoli asztal](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | A Windows                                   | A Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Malac feladatok futó Azure hdinsight szolgáltatáshoz a helyszíni SQL Server Integration Services használatával

SQL Server Integration Services (SSIS) segítségével is malac feladat futtatása. Az alábbi összetevők, amely HDInsight malac feladatok használata SSIS Azure funkciócsomag ismertetése


- [Azure hdinsight szolgáltatáshoz malac tevékenység][pigtask]
- [Azure előfizetés kapcsolatkezelő][connectionmanager]


További információt az Azure funkciócsomag az SSIS- [Itt][ssispack].


##<a id="nextsteps"></a>Következő lépések

Most, hogy malac használata a HDInsight rendelkezik megtanulta, használja az alábbi hivatkozások feltárása más módszerek a Azure hdinsight szolgáltatáshoz.

* [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
* [HDInsight struktúra használata][hdinsight-use-hive]
* [HDInsight Sqoop használata](hdinsight-use-sqoop.md)
* [HDInsight Oozie használata](hdinsight-use-oozie.md)
* [HDInsight MapReduce feladatok használata][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png
