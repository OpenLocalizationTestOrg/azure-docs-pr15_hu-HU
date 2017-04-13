<properties
   pageTitle="Távoli asztal HDInsight Hadoop malac használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként malac Latin kimutatások lebonyolítása távoli asztali kapcsolaton HDInsight a Windows-alapú Hadoop fürtre a malac paranccsal."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Távoli asztali kapcsolaton malac feladatokat lebonyolítása

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

A dokumentum útmutató nyújt a malac paranccsal a Windows-alapú HDInsight fürtre malac Latin kimutatások lebonyolítása távoli asztali kapcsolaton. Malac Latin lehetővé teszi leíró adat átalakítások, MapReduce alkalmazások létrehozása, hanem a megfeleltetése és a függvények csökkentése.

A dokumentum megtudhatja, hogy miként

##<a id="prereq"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez az alábbi lesz szüksége.

* A Windows-alapú HDInsight (Hadoop a HDInsight) fürtre

* A Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

##<a id="connect"></a>Csatlakozás távoli asztali

A HDInsight fürt engedélyezése a távoli asztali, majd a [Csatlakozás RDP segítségével HDInsight fürt](hdinsight-administer-use-management-portal.md#rdp)utasításait követve csatlakozni.

##<a id="pig"></a>A malac paranccsal

2. Után távoli asztali kapcsolaton, indítsa el a **Hadoop parancssori** a ikonra az asztalon.

2. A következő segítségével a malac parancs indítása:

        %pig_home%\bin\pig

    A bemutatott egy `grunt>` kérdés.

3. Írja be az alábbi utasítás:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Ez a parancs a sample.log fájl tartalmát a naplók fájlba tölt be. A fájl tartalmát a következő parancs segítségével tekintheti meg:

        DUMP LOGS;

4. Az adatok átalakítása az egyes rekordok csak a naplózási szintet kinyerése a reguláris kifejezések alkalmazásával:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **Kiírása** kattintva megtekintheti az adatokat a átalakítása után használható. Ebben az esetben `DUMP LEVELS;`.

5. Folytassa a következő kimutatások használatával a átalakítások alkalmazását. Használat `DUMP` az átalakítás minden egyes lépés után az eredmények megjelenítéséhez.

    <table>
    <tr>
    <th>Kimutatás</th><th>Rendeltetés</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = szűrő szintek szerint LOGLEVEL nem null;</td><td>A naplózási szint null értéket tartalmazó sorok eltávolítása és az eredményt FILTEREDLEVELS be.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = csoport FILTEREDLEVELS által LOGLEVEL;</td><td>A naplózási szint sorok csoportok és az eredményt GROUPEDLEVELS be.</td>
    </tr>
    <tr>
    <td>GYAKORISÁGOK = foreach GROUPEDLEVELS csoport létrehozása, LOGLEVEL, a DARABSZÁM (FILTEREDLEVELS. LOGLEVEL) szerint száma;</td><td>Új hoz létre, amely tartalmazza az egyes egyedi naplófájl adatok szintű érték és a hány alkalommal fordul elő. Ez az GYAKORISÁGOK tárolt</td>
    </tr>
    <tr>
    <td>EREDMÉNY = sorrendben GYAKORISÁGOK által desc száma;</td><td>A naplózási szintek rendelések száma (csökkenő) szerint, és tárolja, az eredmény</td>
    </tr>
    </table>

6. Az átalakítás eredménye használatával is mentheti a `STORE` utasítás. Ha például a következő parancsot a menti a `RESULT` a **/example/data/pigout** címtárhoz az alapértelmezett tároló tárolóban a fürt:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Az adatokat a megadott nevű fájlt a **része-nnnnn**címtár vannak tárolva. A könyvtár már létezik, ha hibaüzenetet kap.

7. Lépjen ki a grunt kérdésre, adja meg a következő utasítást.

        QUIT;

###<a name="pig-latin-batch-files"></a>Malac Latin köteg fájlok

A malac paranccsal is malac latin betűs lévő fájl futtatása.

3. Kilépés a grunt kérdés után nyissa meg a **Jegyzettömb** , és hozzon létre egy új fájlt nevű **pigbatch.pig** a **PIG_HOME %** címtárban.

4. Írja be vagy illessze be a következő sort a **pigbatch.pig** fájlt, és mentse azt:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. A következő segítségével futtassa a malac paranccsal **pigbatch.pig** fájlt.

        pig %PIG_HOME%\pigbatch.pig

    A köteg feladat befejezésekor meg kell jelennie a következő, érdemes lehet ugyanaz, mint amikor használt kimenet `DUMP RESULT;` az előző lépésekben:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Összefoglalás

Amint látható, a malac parancs segítségével interaktív MapReduce műveletek futtatása, vagy egy köteg-fájlban tárolt malac Latin feladatok futtatása.

##<a id="nextsteps"></a>Következő lépések

A HDInsight malac általános információt:

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

További módszerek információt dolgozhat a HDInsight Hadoop:

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
