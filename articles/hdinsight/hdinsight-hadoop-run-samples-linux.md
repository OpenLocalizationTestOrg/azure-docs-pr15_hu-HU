<properties
    pageTitle="Példák Hadoop MapReduce futtatása a HDInsight Linux-alapú |} Microsoft Azure"
    description="Az első lépések MapReduce minták használata Linux-alapú hdinsight szolgáltatásból lehetőségre. Csatlakozzon a fürthöz SSH segítségével, majd a minta feladatok futtatásához használja a Hadoop parancsot."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>A Hadoop minták HDInsight futtatása

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-alapú HDInsight fürt biztosítanak MapReduce minták, hogy megismerkedjen operációs rendszert futtató Hadoop MapReduce feladatok használható. A dokumentumban tudnivalók a rendelkezésre álló minták és végigvezetik futó néhány őket.

##<a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**: lásd: az [első Azure ingyenes próbaverziója](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **A Linux-alapú HDInsight fürt**: lásd [a struktúra HDInsight Linux Hadoop használatának első lépései](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Egy SSH ügyfél**: HDInsight való SSH használatáról további tudnivalókért lásd az alábbi cikkekben:

    - [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>A minták ##

**Hely**: minták találhatók a **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar** HDInsight fürthöz

**Tartalom**: az alábbi példák találhatók a archiválása:

- **aggregatewordcount**: az összesítő alapján számolja a szavakat, a bemeneti fájlokat a térkép/csökkentése program
- **aggregatewordhist**: az összesítő térkép/csökkentése alkalmazás, amely kiszámítja a hisztogram, a bemeneti fájlok szavainak alapján
- **BBP**: számítja ki a pontos számjegye Pi Bailey-Borwein-Plouffe használó térkép/csökkentése program
- **dbcount**: egy példa megszámolja a tárolt pageview naplók feladatot egy adatbázis
- **distbbp**: számítja ki a pontos bittel pi BBP típusú képletet használó térkép/csökkentése program
- **GREP**: megszámolja a találatokat egy, a bemeneti regex a térkép/csökkentése program
- **Csatlakozás**: A feladat, amely az effektusok illesztés feletti rendezve, egyaránt particionálva adatkészleteket
- **multifilewc**: is számolja a szavakat több fájl a feladat
- **pentomino**: A térkép/csökkentése csempe szóló program pentomino kapcsolatos problémák megoldása
- **a pi**: térkép/csökkentése program, amely megbecsüli használata egy kvázi Monte Pi Carlo módszer
- **randomtextwriter**: 10 GB-nyi egyes csomópontok véletlenszerű szöveges adatokat ír térkép/csökkentése program
- **randomwriter**: 10 GB-nyi egyes csomópontok véletlenszerű adatokat ír térkép/csökkentése program
- **secondarysort**: a csökkentése egy másodlagos rendezési definiálásával példa
- **Rendezés**: térkép/csökkentése programot a véletlen író által írt adatok rendezése
- **sudoku**: egy sudoku solver
- **teragen**: a terasort az adatok létrehozása
- **terasort**: a terasort futtatása
- **teravalidate**: terasort eredményeit ellenőrzése
- **WordCount**: számolja a szavakat, a bemeneti fájlokat a térkép/csökkentése program
- **wordmean**: megszámolja a bemeneti fájlokat a szavak átlagos hosszát térkép/csökkentése program
- **wordmedian**: térkép/csökkentése program számolja a szavakat, a bemeneti fájlok medián hossza
- **wordstandarddeviation**: térkép/csökkentése program számolja a szavakat a bemeneti fájlokat hosszát szórása

**Forráskód**: ezek a minták forráskód megtalálható a HDInsight fürthöz **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] A `2.2.4.9-1` az elérési út a Hortonworks adatok Platform a HDInsight fürt verzióját, és HDInsight frissítése után megváltoznak.

## <a name="how-to-run-the-samples"></a>A minták futtatása ##

1. Csatlakozás HDInsight SSH használ, az alábbi cikkek útmutatását:

    - [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Az a `username@#######:~$` kérdés, használja a következő parancsot a minták listáját:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Ez a minta listáját a dokumentum előző szakaszában hoz létre.

3. A következő paranccsal adott mintájából segítséget. Ebben az esetben a **wordcount** minta:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    A következő üzenet kell megjelennie:

        Usage: wordcount <in> [<in>...] <out>

    Ez azt jelzi, hogy több beviteli elérési utak biztosíthat a forrás dokumentumok. A végleges elérési útja hol tárolja a kimenet (a forrás dokumentumok szavainak számát).

4. A következő használja a jegyzetfüzeteket a Leonardo Da Vinci, amely szolgálnak, a mintaadatok a fürthöz összes szavainak megszámlálására:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Ehhez a feladathoz beviteli a **wasbs:///example/data/gutenberg/davinci.txt**olvasható.

    Az ebben a példában tárolódnak a kimeneti **wasbs: / / / Példa/adatok/davinciwordcount**.

    > [AZURE.NOTE] A wordcount minta súgójában amint beviteli fájlokat is megadhatja. Ha például `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` szeretné számolni a davinci.txt és ulysses.txt szavak számát.

5. Ha befejeződött a feladatot, használja a következő parancsot a kimenet megtekintése:

        hdfs dfs -cat /example/data/davinciwordcount/*

    A feladat készített kimeneti fájlok fűzi össze, és megjelenítheti őket. Egyszerű példa van csak egy fájlt, akkor jó helyen jár, ha többet ez a parancs volna ismétlés mindegyikük volt.

    A kimenet szövege a következőhöz hasonló:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Minden egyes sor egy szót, és hány alkalommal előfordult, a bemeneti adatok jelöli.

## <a name="sudoku"></a>Sudoku

A Sudoku példa tartalmaz utasításokat némileg nem hasznosként használatát; "Egy a parancssorban kirakó tartalmazzák."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) egy logikai kirakó kilenc 3 x 3 rácsok tevődik össze. Néhány a rács cellákban számok, miközben mások üresek, és a célja, hogy oldja meg a üres cellák. A fenti hivatkozást tartalmaz további tájékoztatást a kirakó, de a következő példában az a célja, hogy oldja meg a üres cellák. A beviteli úgy kell lennie egy fájlt, amely a következő formátumban:

- Az oszlopok kilenc kilenc sorok

- Minden egyes oszlopát tartalmazhat vagy egy számot vagy `?` (amely mutatja az üres cella)

- Cellák szóközzel vannak elválasztva.

Van egy bizonyos módszer Sudoku egyik; összeállításához egy szám, egy oszlop vagy sor nem ismételjük meg. A helyes összeállítás HDInsight fürt példa szerepel. Felső **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** található és a az alábbiakat tartalmazza:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] A `2.2.4.9-1` módosíthatja, hogy az elérési út része, a HDInsight fürt módosításai frissítések.

Ez futtathatja a Sudoku példa, használja az alábbi parancsot:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Az eredmények megjelennek a következőhöz hasonló:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>A pi (π)

A pi minta használja a statisztikai (kvázi Monte Carlo) módszer a pi matematikai állandót. Véletlenszerűen helyezni időegység pontok négyzetes is tartoznak a kör területe egyenlő valószínűség belül négyzetre ütemtervben kör pi/4. A pi értékét a 4R, R esetén, amelyek belül, amelyek a téglalap alakú belül pontok száma a kör pontok száma arányát az értékek is becsült. Minél nagyobb a minta felhasznált pontok, annál hatékonyabb becslés van.

A hozzárendelést, az alábbi példa létrehoz egy véletlenszerűen belül egység négyzet pontok száma, és majd megszámolja azokat, amelyek a körbe pontok száma.

A nyomáscsökkentő majd összegzi a mappers által megszámlált pontok és becslést ad a képlet 4R, R esetén, amelyek a téglalap alakú belül pontok száma a körbe megszámlált pontok száma arányát a pi matematikai állandót.

A következő paranccsal futtassa a következő példában. A 16 térképek rendelkező használ 10,000,000 minták átböngészve felmérheti a pi értékét:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Ez a visszaadott érték **3.14159155000000000000**hasonló kell lennie. Hivatkozás az első 10 tizedesjegyek pi 3.1415926535 is.

##<a name="10gb-greysort"></a>10 GB-os Greysort

GraySort egy javasolt rendezés, amelynek mérőszáma rendezés ráta (TB/perc), amely a rendezést nagyon nagy mennyiségű adatot, általában egy 100TB minimális közben érhető el.

Ez a példa használja a mérsékelt 10GB adatot, hogy gyorsan viszonylag futtathatók. A Owen O'Malley és Arun Murthy, amely a éves általános célú ("daytona") adjon rendezés viszonyítási alap nyert 2009 sebesség 0.578 TB/Min (100 TB 173 perc) által fejlesztett MapReduce alkalmazást használ. Ez, és más rendezési szintek a további tudnivalókért lásd: a [Sortbenchmark](http://sortbenchmark.org/) webhelyet.

Ez a példa három különböző MapReduce programokat használja:

- **TeraGen**: MapReduce program által generált adatok sorok

- **TeraSort**: minták a bemeneti adatok, és rendezze az adatokat egy teljes rendelés MapReduce használ

    TeraSort MapReduce függvények kivételével egy egyéni partitioner egy rendezett listát, amely az egyes csökkentése kulcs esik mintát N-1 kulcsok használó szabványos rendezésének. Különösen az összes kulcsokat ilyen példa a [i-1] < = billentyű < minta [i] küldjük i csökkentése érdekében. Ki, hogy a kimeneti értékeket a csökkentése i garanciákkal-nél kisebb kibocsátásának csökkentése i + 1 billentyűkombinációt.

- **TeraValidate**:, amely ellenőrzi, hogy a kimenet globálisan rendezett MapReduce program

    A kimenet könyvtár hoz létre egy térképen fájlonként, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő az előzőt. A térkép függvény is létrehozza az első és utolsó kulcsok, az egyes fájlok rekordokat, és a csökkentése függvény biztosítja, hogy az első billentyű fájl i nagyobb, mint a fájl i-1 az utolsó billentyűt. Esetleges problémákat, amelyek nem működik a billentyűkkel a csökkentése egy kimenetét jelentett.

Használja az alábbi lépéseket követve létrehozni az adatokat, rendezése és a kimeneti majd ellenőrzése:

1. 10 GB-os az adatokat, amelyeket tárolni a HDInsight fürt alapértelmezett tárolás készítése **wasbs: / / / Példa / / 10GB-rendezés-adatbevitel**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    A `-Dmapred.map.tasks` Hadoop használni ehhez a feladathoz hány térkép feladatok alapján. Az utolsó két paraméterrel kérje meg a feladat létrehozásához értékű 10GB adatot, és tárolja őket az **wasbs: / / / Példa / / 10GB-rendezés-adatbevitel**.

2. A következő parancsot használja az adatok rendezéséhez:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    A `-Dmapred.reduce.tasks` számának csökkentése a feladathoz használhatja a tevékenységek Hadoop alapján. A végleges két paraméterek nem csak a bemeneti és kimeneti helyeken az adatokat.

3. A rendezés által generált adatok érvényesítése használja a következő:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Következő lépések ##

Ez a cikk a futtatása a minták a Linux-alapú HDInsight fürt részét képező megtanulta azt. Az oktatóprogram HDInsight malac struktúra és MapReduce használatáról a következő témakörökben olvashatók:

* [A HDInsight Hadoop malac használata][hdinsight-use-pig]
* [A HDInsight Hadoop struktúra használata][hdinsight-use-hive]
* [A HDInsight Hadoop MapReduce használata] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
