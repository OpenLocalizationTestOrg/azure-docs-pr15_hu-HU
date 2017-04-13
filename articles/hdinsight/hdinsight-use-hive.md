<properties
    pageTitle="Ismerje meg, mi az struktúra és használatáról a HiveQL |} Microsoft Azure"
    description="Tudjon meg többet a Apache struktúra, és hogyan használják a HDInsight Hadoop. Válassza ki a struktúra feladat futtatása, és amelyek HiveQL Apache log4j mintafájl módját."
    keywords="Mi az struktúra, hiveql"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Mintafájl Apache log4j elemzéséhez Hadoop HDInsight a struktúra, és HiveQL használata

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Ebben az oktatóanyagban meg fog Apache Hadoop a struktúra használata a hdinsight szolgáltatásból lehetőségre, és válassza a struktúra feladat futtatása. Meg kell is megtudhatja HiveQL és elemezheti Apache log4j mintafájl.

##<a id="why"></a>Mi struktúra, és miért érdemes használni?
[Apache struktúra](http://hive.apache.org/) rendszer adatok raktári a Hadoop, amely lehetővé teszi az adatok összegzésének lekérdezése és adatok elemzése HiveQL (egy lekérdezési nyelv SQL hasonló) használatával. Struktúra használható interaktív adatai feltárása vagy újrafelhasználható köteg létrehozásához a feldolgozás feladatokat.

Struktúra nagymértékben strukturálatlan adatokat a projekt szerkezetének lehetővé teszi. A szerkezet határozza meg, miután a struktúra lekérdezni, adatokat Java vagy MapReduce ismerete nélkül is használhatja. **HiveQL** (a struktúra lekérdezésnyelv) lehetővé teszi a hasonlóak az SQL-T a kimutatások-lekérdezéseket írni.

Struktúra értelmezésére képes szeretné a strukturált és félig strukturált adatok, például a szövegfájlok, ahol mezőket határolja adott karaktereket. Struktúra egyéni **szerializáló/deserializers (SerDe)** is támogatja az összetett vagy szabálytalan strukturált adatokat. További tudnivalókért lásd: [egy egyéni JSON SerDe HDInsight való használatáról](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Felhasználó által definiált függvények (UDF)

**Felhasználó által definiált függvényeket (UDF)**keresztül is bővíthető struktúra. Egy UDF funkciók, vagy egyszerűen nem modellezni logikájának megvalósítása a HiveQL teszi lehetővé. Példa a struktúra UDF használja talál a következő:

* [Használata Java felhasználó által definiált struktúra függvény](hdinsight-hadoop-hive-java-udf.md)

* [Struktúra, és a HDInsight malac Python használata](hdinsight-python.md)

* [C# struktúra és használhatja a HDInsight malac](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Egyéni struktúra UDF hozzáadása hdinsight szolgáltatáshoz](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Dátum/idő formátumok konvertálása struktúra időbélyeg egyéni struktúra UDF példa](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Táblázatok belső és külső táblák struktúra

Néhány dolgot kell tudnia kapcsolatos struktúratáblával belső és külső tábla:

- A **Táblázat létrehozása** parancs egy belső táblázatot hoz létre. Az alapértelmezett tárolóban az adatfájl kell elhelyezni.
- A **Táblázat létrehozása** parancs helyezi át az adatfájl /hive/raktári/<TableName> mappát.
- A **Külső tábla létrehozása** parancs egy külső táblázatot hoz létre. Az alapértelmezett tároló kívül az adatfájl is található.
- A **Külső tábla létrehozása** parancs ne helyezze át az adatokat tartalmazó fájl.
- A **Külső tábla létrehozása** parancs nem engedélyezi a minden olyan helyen mappát. Ez az az oka, miért teszi az oktatóanyagot, a sample.log fájl egy példányát.

További tudnivalókért lásd: [HDInsight: struktúra belső és külső táblák – bevezetés][cindygross-hive-tables].


##<a id="data"></a>Tudnivalók a mintaadatokat egy Apache log4j fájl

Ebben a példában a minta van tárolva, **/example/data/sample.log** a blob-tároló tárolóban *log4j* fájlok. A fájlban rögzít egy sort tartalmazó mezők áll egy `[LOG LEVEL]` mező megjelenítéséhez a típus és súlyosságát, például:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Az előző példában a naplózási szint jelző hiba.

> [AZURE.NOTE] Is készíthet log4j fájl az [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) naplózás eszközzel, és a blob-tárolóhoz töltse ki a fájlt. [Töltse fel adatok hdinsight szolgáltatáshoz](hdinsight-upload-data.md) talál utasításokat. Tudnivalók Azure Blob-tárolóhoz hdinsight szolgáltatásból lehetőségre a további tudnivalókért lásd: [Használata Azure Blob-tárolóhoz a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-use-blob-storage.md).

A mintaadatok Azure Blob-tárolóhoz, amely HDInsight használja az alapértelmezett fájlrendszer vannak tárolva. A **wasb** előtag használatával BLOB tárolt fájlok HDInsight érheti el. Például a sample.log fájl elérésére, használja a következő szintaxist:

    wasbs:///example/data/sample.log

Mivel a Azure Blob-tárolóhoz HDInsight alapértelmezett tárolt adatait, **/example/data/sample.log** a HiveQL használatával is elérheti a fájlt.

> [AZURE.NOTE] Az alábbi, **wasbs: / / /**, használja az alapértelmezett tároló tárolóban a HDInsight fürt tárolt fájlok eléréséhez. További tárterület fiókok megadott telepítésekor, akkor a fürt, és el szeretne érni, az alábbi fiókok tárolt fájlok, érhető el az adatokat tároló nevét és tároló fiók címét, például megadásával **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Minta feladat: projekt oszlopok alakzatot a tagolt adatok

A következő HiveQL utasításokat a alakzatot, amely a tagolt adatok oszlopok project lesz a **wasbs: / / / / példaadatok** Directoryban:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Az előző példában a HiveQL kimutatások hajtsa végre a következő műveleteket:

* __hive.execution.engine=tez; beállítása__: az adatvégrehajtás motor Tez használandó állítja be. MapReduce helyett Tez használatával adhat a lekérdezési teljesítmény növekedését. További tájékoztatást a Tez című [Apache Tez használata a nagyobb teljesítmény](#usetez) .

    > [AZURE.NOTE] Ez az utasítás csak van szükség, ha a Windows-alapú HDInsight fürtre; használatával Tez a alapértelmezett adatvégrehajtás-motort Linux-alapú hdinsight szolgáltatásból lehetőségre.

* **Táblázat LEVÁLASZTÁSA**: a táblázat és az adatfájl törli, ha a táblázat már létezik.
* **Külső tábla létrehozása**: új táblát hoz létre **külső** a struktúra. Külső táblákat a tábla definícióját csak tárolását struktúra; az adatok az eredeti helyén, és az eredeti formátumában marad.
* **Sor FORMAT**: Ez az információ struktúra, az adatok formázását. Ebben az esetben a mezőket az egyes naplók vannak elválasztva szóközt.
* **AS TEXTFILE helyen tárolt**: Ez az információ struktúra az adatokat tárolja (a példában/adatkönyvtárának), és szövegként tárolt. Az adatok egy fájl lehet vagy a címtáron belül több fájl között.
* **Jelölje ki**: a száma, ahol az oszlop **t4** **[hiba]**értéket tartalmazza az összes sor kijelölése. Meg kell visszaadni értéke **3** , mert a három, ezt az értéket tartalmazó sorok.
* **INPUT__FILE__NAME PÉLDÁUL "%.log"** - struktúra alapján visszaadó azt kell csak adatok végződésű fájlokból. naplót. E beállítás hatására a Keresés a adatokat tartalmazza, amely megőrzi a származó adatokat visszaadó más példából, amelyek nem felelnek meg a definiált séma adatfájlok sample.log fájlt.

> [AZURE.NOTE] Külső tábla a mögöttes adatok frissítése külső forrásból, például egy automatizált adatok feltöltése folyamatban, illetve egy másik MapReduce művelet várt, ezért mindig struktúra lekérdezések használata a legfrissebb adatokat kell használni.
>
> Tartalmaz külső tábla húzással **nem** törölheti az adatokat, csak a táblázat definíció törlése.

A külső tábla létrehozása után a következő utasítások egy **belső** táblázat létrehozásához használt.

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Ezek a kimutatások hajtsa végre az alábbi műveleteket:

* **Hozzon létre táblázat esetén nem létezik**: táblát hoz létre, ha még nem létezik. A **külső** kulcsszó nem használatos, mert az egy belső tábla, amely a struktúra adatraktár tárolja, és a struktúra teljesen kezeli.
* **TÁROLVA mint ORC**: optimalizált sor oszlopos (ORC) formátumban adatait tárolja. Ez a struktúra adatok tárolására szolgáló erősen optimalizált és hatékony formátumot.
* Beszúrás **FELÜLÍRÁSA... VÁLASSZA a**: sorok kiválasztása a **log4jLogs** táblából, amelyen **[hiba]**, és ezután illeszti be az adatokat a **errorLogs** táblázatba.

> [AZURE.NOTE] Külső táblázatok, eltérően egy belső tábla húzással is törli a mögöttes adatok.

##<a id="usetez"></a>Nagyobb teljesítmény Apache Tez használata

[Apache Tez](http://tez.apache.org) , amely lehetővé teszi, hogy az adatok igénylő alkalmazások, például a struktúra, a méretezés sokkal hatékonyabban futtatásához keretrendszert. A HDInsight legújabb verzióját a struktúra támogatja Tez futó. Linux-alapú HDInsight fürt alapértelmezés szerint engedélyezve van a Tez.

> [AZURE.NOTE] Tez jelenleg alapértelmezés szerint nincs engedélyezve a Windows-alapú HDInsight fürtre vonatkozóan, és azt engedélyezve kell lennie. Tez kihasználhatja az alábbi értéket kell beállítani a struktúra lekérdezés:
>
> ```set hive.execution.engine=tez;```
>
>Ez elküldhetők lekérdezés alapon helyezze a lekérdezés elején. Is beállíthatja, hogy ez legyen a fürtre alapértelmezés szerint a fürt létrehozásakor a konfigurációs érték megadásával. További részleteket a [Kiépítési HDInsight fürt](hdinsight-provision-clusters.md)található.

A végrehajtás választási lehetőségek és az eszközbeállítási konfigurációk számú [Tez Tervező dokumentumok struktúra](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) tartalmaz.

A feladatok hibakeresés futtatta Tez használatával, HDInsight biztosít a következő webes előkészíthetik, amelyek lehetővé teszik a Tez feladatok részletes adatainak megjelenítéséhez:

* [Windows-alapú HDInsight a Tez felhasználói felületének használata](hdinsight-debug-tez-ui.md)

* [A HDInsight Linux-alapú Ambari Tez nézet használata](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Válassza ki a HiveQL feladat futtatása

HDInsight módszerek számos HiveQL feladatok futtathatók. Döntse el, melyik módszert szüksége az alábbi táblázat segítségével, majd hajtsa végre az útmutató hivatkozását.

| **Ezzel a** ...                                                     | .. .an **interaktív** rendszerhéj | ... **parancsfájl** | .. .with **fürt operációs rendszer** | .. .from **ügyfél operációs rendszer** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Struktúra megtekintése](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Bármely (böngészőalapú) |
| [Beeline parancs (munkamenetből egy SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [A struktúra (munkamenetből egy SSH) parancs](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [Curl](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux vagy a Windows                          | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [Lekérdezés konzolon](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | A Windows                                   | Bármely (böngészőalapú)                            |
| [HDInsight tools for Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux vagy a Windows                          | A Windows                                  |
| [A Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux vagy a Windows                          | A Windows                                  |
| [Távoli asztal](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | A Windows                                   | A Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Struktúra feladatok futó Azure hdinsight szolgáltatáshoz a helyszíni SQL Server Integration Services használatával

SQL Server Integration Services (SSIS) a struktúra feladat futtatása is használhatja. SSIS Azure funkciócsomag biztosít az alábbi összetevők, amely a struktúra feladatok használata a hdinsight szolgáltatásból lehetőségre.


- [Azure HDInsight-struktúra tevékenység][hivetask]
- [Azure előfizetés kapcsolatkezelő][connectionmanager]


További információt az Azure funkciócsomag az SSIS- [Itt][ssispack].


##<a id="nextsteps"></a>Következő lépések

Most, hogy mi struktúra, és hogyan használják a HDInsight Hadoop-már megtanulta, használja az alábbi hivatkozások feltárása más módszerek a Azure hdinsight szolgáltatáshoz.


- [Töltse fel az adatok hdinsight szolgáltatáshoz][hdinsight-upload-data]
- [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
- [HDInsight Sqoop használata](hdinsight-use-sqoop.md)
- [HDInsight Oozie használata](hdinsight-use-oozie.md)
- [HDInsight MapReduce feladatok használata][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
