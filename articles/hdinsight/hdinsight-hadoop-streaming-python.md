<properties
   pageTitle="Feladatok Python MapReduce HDInsight kidolgozása |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre és Linux-alapú HDInsight fürt Python MapReduce feladat futtatható."
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

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Adatfolyam-programok HDInsight-Python kidolgozása

Hadoop biztosít, amely lehetővé teszi, hogy a térkép írása, és a függvények Java eltérő nyelven csökkentése MapReduce adatfolyam API-val. Ebben a cikkben megtanulhatja a használatát Python MapReduce műveletek elvégzéséhez.

> [AZURE.NOTE] A dokumentumban a Python kódot kínál a Windows-alapú HDInsight fürtre, miközben a a dokumentumban szereplő lépések Linux-alapú fürt jellemző.

Ez a cikk információkkal és példákkal [Python Hadoop MapReduce programon írása](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)a Lukács Noll által közzétett alapul.

##<a name="prerequisites"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez a következőkre lesz szüksége:

* Egy Linux-alapú Hadoop a HDInsight fürthöz

* Egy egyszerű szövegszerkesztő programban

    > [AZURE.IMPORTANT] A text szerkesztő LF kell használja a sor-végződés. Ha használja az CRLF, ennek hatására hibák Linux-alapú HDInsight fürt a MapReduce feladat forgalmi. Ha nem biztos benne, használatával a nem kötelező lépés a [Futtatás MapReduce](#run-mapreduce) szakasz bármely CRLF átalakítása LF.

* Windows-ügyfelek, gitt és PSCP. Ezek a segédprogramok <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Gitt letöltési lapon</a>érhetők el.

##<a name="word-count"></a>A szavak száma

Ebben a példában egy egyszerű a szavak száma lesz alkalmazhat egy hozzárendelést és nyomáscsökkentő használatával. A hozzárendelést külön szavakra mondatok oszlik, és a nyomáscsökkentő összesíti a szavakat, és megszámolja, hogy a kimenet létrehozására.

Az alábbi folyamatábra a Mi történik, miközben a térkép, és fázisok csökkentése mutatja be.

![ábra: a térkép csökkentése](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Miért Python?

Python egy általános célú, magas szintű programnyelv, amely lehetővé teszi, hogy kevesebb mint számos más nyelv kódsorokat az expressz fogalmak. Rendelkezik a legutóbb vált, az adatok tudósok prototípusának elkészítéséhez nyelveként népszerű, mert a parancsértelmezős természeti, dinamikus beírni, és elegáns szintaxis legyen gyors alkalmazása fejlesztési alkalmas.

Minden HDInsight fürt Python van telepítve.

##<a name="streaming-mapreduce"></a>Adatfolyam MapReduce

Hadoop adjon meg egy fájlt, amely tartalmazza a térképet, és a projekt által használt logika csökkentésére teszi lehetővé. Az adott követelményeket, a térkép és csökkentése logika vannak:

* **Beviteli**: A térkép és csökkentése összetevők STDIN bemeneti adatok kell olvasni.

* **Kimenet**: A térkép és csökkentése összetevők STDOUT kimeneti adatokat kell írnia.

* **Adatok formázása**: az adatok elfogyasztott és a megtermelt kulcs/érték két tabulátorkarakter pontosvesszővel kell lennie.

Python könnyen kezelhető ezeknek a követelményeknek a **sys** modulról STDIN olvassák és **nyomtatása** STDOUT nyomtatni. A hátralévő tevékenységek egyszerűen formázza az adatokat, egy lappal (`\t`) karaktert a kulcs és érték között.

##<a name="create-the-mapper-and-reducer"></a>A hozzárendelést és nyomáscsökkentő létrehozása

A hozzárendelést és nyomáscsökkentő szerepelnek, szövegfájlokból, ez esetben **mapper.py** és **reducer.py** (, hogy mit tesz törölje). Ezek a szerkesztő lehetőség használatával hozhat létre.

###<a name="mapperpy"></a>Mapper.PY

**Mapper.py** nevű új fájl létrehozása, és a tartalom használni a következő kódot:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Szánjon egy kis időt olvassa el a kódot, így megismerheti, mit jelent.

###<a name="reducerpy"></a>Reducer.PY

**Reducer.py** nevű új fájl létrehozása, és a tartalom használni a következő kódot:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>A fájlok feltöltése

**Mapper.py** és a **reducer.py** a központi csomópontra a fürt előtt kell azt futtathatók. A töltse fel őket legegyszerűbben **scp** (**pscp** ügyfél Windows rendszer használata esetén) használatára.

Az ügyféltől ugyanabban a mappában, valamint **mapper.py** **reducer.py**, a következő parancsot használja. **Felhasználónév** cserélje le egy SSH felhasználó, és **clustername** a csoport nevét.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Ez másolja át a fájlokat a helyi rendszer központi csomópontot.

> [AZURE.NOTE] Ha korábban beállított jelszót biztonságossá tétele SSH-fiókja, a rendszer kéri, a jelszót. Ha korábban egy SSH kulcsot, előfordulhat, hogy akkor alkalmazza a `-i` paraméterre és elérési útját a titkos kulcs, például `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>MapReduce futtatása

1. Csatlakozás a fürt SSH használatával:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ha korábban beállított jelszót biztonságossá tétele SSH-fiókja, a rendszer kéri, a jelszót. Ha korábban egy SSH kulcsot, előfordulhat, hogy akkor alkalmazza a `-i` paraméterre és elérési útját a titkos kulcs, például `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Nem kötelező) Ha egy egyszerű szövegszerkesztő programban, amely CRLF használja, mint a sor végződik, amikor a mapper.py és reducer.py fájlok létrehozása vagy nem tudja, milyen sor és Befejezés-szerkesztővel használja, az alábbi használt parancsok CRLF mapper.py és reducer.py előfordulását konvertálása LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. A következő parancs használatával indítsa el a MapReduce feladatot.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Ez a parancs az alábbi részekből áll:

    * **hadoop-streaming.jar**: adatfolyam MapReduce műveletek végrehajtásakor használt. Adja meg a külső MapReduce kóddal Hadoop azt felületek.

    * **-fájlok**: Ez az információ Hadoop, amely a megadott fájlokat a MapReduce feladat van szükség, és azok a dolgozó csomópontjait kell másolni.

    * **-hozzárendelést**: Ez az információ, Hadoop a hozzárendelést fogja használni a fájlt.

    * **-nyomáscsökkentő**: Ez az információ, Hadoop a nyomáscsökkentő fogja használni a fájlt.

    * **-beviteli**: A bemeneti fájl, amely azt kell számolni a szavakat.

    * **-kimeneti**: A könyvtárra, a kimeneti írandó.

        > [AZURE.NOTE] Ezt a címtárat a feladat hozza létre.

**Információ** a kimutatások kitöltenem látható a projekt indítása kell, és végül lásd: a **térkép** és **csökkentése** művelet százalékértékként jelenik meg.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

A feladat állapota információt fog kapni, amikor befejez.

##<a name="view-the-output"></a>A kimenet megtekintése

Ha befejezte a feladatot, a következő paranccsal a kimenet megtekintése:

    hdfs dfs -text /example/wordcountout/part-00000

Megjelenjen-e ez egy lista szavak és a word hány alkalommal történt. Az alábbiakban látható egy példa kimeneti adatai:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Következő lépések

Most, hogy adatfolyam MapRedcue feladatok használata a HDInsight rendelkezik megtanulta, használja az alábbi hivatkozások feltárása más módszerek a Azure hdinsight szolgáltatáshoz.

* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce feladatok használata](hdinsight-use-mapreduce.md)
