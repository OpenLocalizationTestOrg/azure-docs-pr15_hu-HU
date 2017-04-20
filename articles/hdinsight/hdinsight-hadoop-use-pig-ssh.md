<properties
   pageTitle="Hadoop sertés használata SSH egy HDInsight fürt |}] A Microsoft Azure"
   description="Tájékoztatás arról, hogyan kapcsolódjanak a Linux-alapú Hadoop fürt SSH és a sertés sertés Latin kimutatások interaktív futtatandó parancsot használhatja, vagy kötegelt feladatként."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Sertés feladatok futtatása Linux-alapú fürt esetén a sertés paranccsal (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

A dokumentum bemutatja Secure Shell (SSH), a sertés paranccsal futtassa a sertés Latin kimutatások interaktív módon, vagy kötegelt feladatként Azure HDInsight Linux-alapú fürt kapcsolódás folyamatát.

A sertés Latin programozási nyelv lehetővé teszi a bemeneti adatok előállításához a kívánt kimeneti alkalmazott átalakítások leírása.

> [AZURE.NOTE] Ha ismeri a Linux-alapú Hadoop kiszolgálókat használ, de az új HDInsight, ötletek [Linux-alapú HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Előfeltételek

A cikkben szereplő lépések elvégzéséhez szüksége lesz a következő.

* A Linux-alapú HDInsight (a HDInsight Hadoop) fürtben.

* Az SSH ügyfél. Linux, Unix és Mac OS egy SSH ügyfél érkezik. Windows-felhasználók le kell töltenie egy ügyfél, például [gitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>SSH kapcsolatot

Csatlakoztassa a teljesen minősített tartománynevét (FQDN) a fürt HDInsight az SSH parancs segítségével. A teljesen minősített tartománynév lesz majd a fürthöz megadott névnek **. azurehdinsight.net**. Például a következő kíván csatlakozni, egy **myhdinsight**nevű fürt.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Amennyiben egy tanúsítvány SSH hitelesítési kulcsot** a HDInsight a fürt létrehozása után, szükség lehet az ügyfélrendszeren helyét a személyes kulcsot.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**SSH hitelesítési jelszó amennyiben** a HDInsight a fürt létrehozása után, meg kell adja meg a jelszót, amikor a rendszer kéri.

SSH használata HDInsight bővebben lásd [Használata SSH és a HDInsight a Linux, OS X, és a Unix, Linux alapú Hadoop](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>GITT (Windows-alapú ügyfelek)

A Windows nem biztosít a beépített SSH ügyfél. **Gitt**, amely letölthető a [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)használata ajánlott.

GITT használatával kapcsolatos további tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop a Windows HDInsight ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>A sertés paranccsal

2. A csatlakozás után a következő paranccsal indítsa el a sertés parancssori felület (CLI).

        pig

    Egy kicsit után megjelenik egy `grunt>` kérdés.

3. Írja be a következő utasítást.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Ez a parancs a sample.log fájl tartalmát betölti a NAPLÓKAT. A fájl tartalmát az alábbi segítségével tekintheti meg.

        DUMP LOGS;

4. Ezután átalakítása az adatokat a naplózási szintet csak kinyerése minden egyes rekord a következő használatával a reguláris kifejezés alkalmazásával.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **DUMP** az átalakítás után az adatok megtekintésére használható. Ebben az esetben használja a `DUMP LEVELS;`.

5. Továbbra is az alábbi utasítások segítségével átalakítások alkalmazása. Használata `DUMP` az átalakítás minden lépés után az eredmények megjelenítéséhez.

    <table>
    <tr>
    <th>Nyilatkozat</th><th>Mire?</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = szűrő szintek szerint LOGLEVEL nem null;</td><td>A naplózási szint null értéket tartalmazó sorok eltávolítása és az eredményt a FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = a csoport FILTEREDLEVELS által LOGLEVEL;</td><td>Naplózási szint a sorok csoportosítása és az eredményt a GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREKVENCIA = foreach GROUPEDLEVELS készítése a csoport mint LOGLEVEL száma (FILTEREDLEVELS. LOGLEVEL) szerint száma;</td><td>Új készlet létrehozása, amely tartalmazza minden egyes egyedi napló adatok értéke a szinten, és hány alkalommal történik. Ez a frekvencia van tárolva.</td>
    </tr>
    <tr>
    <td>EREDMÉNY = rendelési frekvenciák száma DESC jelölést kell használni;</td><td>A hibakeresési szintek rendelések (csökkenő) száma szerint, és tárolja a eredmény.</td>
    </tr>
    </table>

6. Az átalakítás eredménye használatával is mentheti a `STORE` utasítás. Például a következő menti a `RESULT` a fürt számára az alapértelmezett tárolási tároló **/example/data/pigout** könyvtárába.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Az adatok **rész-nnnnn**nevű fájlban megadott könyvtárban tárolja. Ha a könyvtár már létezik, a program hibaüzenetet jelenít meg.

7. Lépjen ki a grunt kérdés, írja be a következő utasítást.

        QUIT;

###<a name="pig-latin-batch-files"></a>Sertés Latin kötegfájlok

A sertés parancs segítségével futtassa a fájlban lévő sertések Latin.

3. Miután kilép a grunt kérdés, a következő paranccsal STDIN pipe egy **pigbatch.pig**nevű fájlba. Ez a fájl a kezdőkönyvtárba, a fiók van bejelentkezve a SSH-munkamenet létrejön.

        cat > ~/pigbatch.pig

4. Írja be vagy illessze be a következő sorokat, és használja a Ctrl + D befejezése.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. A következő használatával futtassa a **pigbatch.pig** fájlt a sertés parancs használatával.

        pig ~/pigbatch.pig

    A kötegelt feldolgozás befejezése után kell a következő kimenet jelenik meg, amely ugyanaz, mint amikor használt `DUMP RESULT;` az előző lépésekben.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Összegzés

Mint látható, a sertés parancs lehetővé teszi, hogy interaktív MapReduce műveletek futtatása sertés Latin használatával, valamint a kötegelt fájlban tárolt utasítás futtatása.

##<a id="nextsteps"></a>A következő lépések

Sertés HDInsight a általános tájékoztatást.

* [A HDInsight Hadoop sertés használata](hdinsight-use-pig.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat.

* [Hadoop HDInsight a struktúra használata](hdinsight-use-hive.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
