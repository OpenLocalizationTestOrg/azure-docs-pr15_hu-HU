<properties
    pageTitle="Javaslatok Mahout és Linux-alapú HDInsight készítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként Linux-alapú HDInsight (Hadoop) a videóban javaslatok készítése a Apache Mahout gépi tanulási tár használatával."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Mozgókép javaslatok készítése a HDInsight Linux-alapú Hadoop-Apache Mahout használatával

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Megtudhatja, hogy miként film javaslatok készítése a [Apache Mahout](http://mahout.apache.org) gépi tanulási Azure hdinsight szolgáltatáshoz tárban használatával.

Mahout egy [gépi tanulási] [ ml] Apache Hadoop-tárat. Mahout algoritmusok adatfeldolgozás fürtképzés és szűrés, osztályozási, például a tartalmazza. Ebben a cikkben egy ajánlási motor használandó film javaslatok ismerősei egyszer látott filmek alapuló létrehozásához.

> [AZURE.NOTE] A dokumentum lépések egy Linux-alapú Hadoop fürt hdinsight szolgáltatásból lehetőségre. Információt a Windows-alapú fürtre való használatáról a Mahout című témakörben talál [Generate film javaslatok a Windows-alapú Hadoop a HDInsight Apache Mahout használatával](hdinsight-mahout.md)

##<a name="prerequisites"></a>Előfeltételek

* Egy Linux-alapú Hadoop fürt hdinsight szolgáltatásból lehetőségre. Egy létrehozásával kapcsolatos további tudnivalókért lásd: a [Linux-alapú Hadoop a HDInsight használatának első lépései][getstarted]

##<a name="mahout-versioning"></a>A verziószámozás mahout

Többet szeretne tudni a HDInsight fürt részét képező Mahout verziója a [HDInsight-verziók és Hadoop-összetevők](hdinsight-component-versioning.md)témakörben talál.

> [AZURE.WARNING] Miközben egy másik verzióját Mahout feltölteni a HDInsight fürt lehetőség, csak a HDInsight fürt ellátni összetevők teljesen támogatott, és Microsoft Support segítséget nyújt azonosíthatók, és az alábbi összetevőket kapcsolatos problémák megoldásához.
>
> Egyéni összetevők kap, akár kereskedelmi célra is lehetővé teszi ügyfélszolgálatunkat, további elhárítani a problémát. Ez a probléma megoldása vagy szólít fel a hol található az adott technológia mély szakértelmét Megnyitás technológiák elérhető csatornák folytasson vezethet. Például vannak sok közösségi webhelyek használható, például: [HDInsight-fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projekteket is projektwebhelyek a [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [külső](http://spark.apache.org/).

##<a name="recommendations"></a>Javaslatok ismertetése

A Mahout által biztosított függvények egyike egy ajánlási motor. Ez a motor fogadja el az adatok formátuma `userID`, `itemId`, és `prefValue` (a felhasználók igényei az elem). Mahout majd végezze el a közös elemként analysis meghatározása: _felhasználók, akik elem előnyben is a beállításait, ezeket az elemeket a_. Mahout majd határozza meg, hogy a hasonló többelemes beállítások, felhasználható ajánlások rendelkező felhasználók.

Az alábbi képen rendkívül egyszerű filmek használó:

* __Dokumentumok közös elemként__: Mintaügyvezető Anna és Péter összes tetszett _csillag ütközések_ _Vissza sztrájkok a Empire_és _a Jedi a vissza_. Mahout határozza meg, hogy a felhasználók, akik e filmek közül bármelyik is, mint például a másik kettő.

* __Dokumentumok közös elemként__: Péter és Anna a _A látszólagosra támadása_, _a klónok a homonimaszerű webcímmel_és _megtorlás, a Sith_tetszett is. Mahout határozza meg, hogy a felhasználók, akik szintén tetszett az előző három filmek például három.

* __Hasonló javaslat__: mivel Mintaügyvezető tetszett az első három filmek, Mahout vizsgálja meg filmek, hogy mások tetszett hasonló beállítások, de Mintaügyvezető nem rendelkezik vizsgált (tetszett/névleges). Ebben az esetben Mahout _A látszólagosra támadása_, _a klónok a homonimaszerű webcímmel_és _megtorlás, a Sith_javasolja.

###<a name="understanding-the-data"></a>Az adatok ismertetése

Kényelmesen, [GroupLens kutatási] [ movielens] értékelése az adatokat nyújt a filmek Mahout kompatibilis formátumban. Az adatok érhető el a fürt alapértelmezett tárolás `/HdiSamples/HdiSamples/MahoutMovieData`.

Két fájlok, vannak `moviedb.txt` (a mozgóképet, információt) és `user-ratings.txt`. A felhasználó-ratings.txt fájl használják-elemzés során, bár moviedb.txt felhasználóbarát szöveg követően megadni az elemzés eredményének megjelenítése.

Felhasználó-ratings.txt lévő adatok szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli velünk hogyan erősen minden felhasználó minősítette a film. Íme egy példa az adatok:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Futtatni az elemzést

A következő parancsot használja az ajánlási feladat futtatása:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] A feladat eltarthat néhány percig, és több MapReduce feladatok futtathatnak.

##<a name="view-the-output"></a>A kimenet megtekintése

1. Ha befejeződött a feladatot, a következő parancs segítségével a létrehozott kimenet megtekintése:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    A kimenet jelennek meg az alábbi képlettel történik:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Az első oszlop a `userID`. Szereplő értékeket ' ["és"] "vannak `movieId`:`recommendationScore`.

2. A kimenet együtt a moviedb.txt, további felhasználóbarát információkat jeleníthet meg is használhatja. Először is azt kell másolja át a fájlokat, az alábbi parancsokkal helyileg:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Ez a kimeneti adatok másolása **recommendations.txt** az aktuális mappában és a mozgókép adatfájlok nevű fájlt.

3. A következő parancs használatával hozzon létre egy új Python parancsfájlt, hogy az adatok javaslatok kimeneti film neveket fog kinézni:

        nano show_recommendations.py

    Amikor megnyílik a szerkesztő, a fájl tartalmát használható a következő:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Nyomja le a **Ctrl-X**, **Y**és végül **Enter** az adatok mentéséhez.

3. A következő parancsot használja, hogy a fájl végrehajtható:

        chmod +x show_recommendations.py

4. Futtassa a Python. Az alábbi lépések feltételezik, a címtárban, ahol a fájlok letöltött áll:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Ez lesz tekintse meg a javaslatok hoz létre felhasználói azonosító 4.

    * A **felhasználó-ratings.txt** fájlt, amely a felhasználó rendelkezik minősítette filmek beolvasásához használt
    * A **moviedb.txt** fájlt a filmek azoknak a beolvasásához használt
    * A **recommendations.txt** használják a film javaslatok a felhasználó számára beolvasása

    Ez a parancs kimenetét a következőhöz hasonló lesz:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Ideiglenes adat törlése

Mahout feladatok nem távolítja el a feladat feldolgozása közben létrehozott ideiglenes adatait. A `--tempDir` elkülönítése az ideiglenes fájlokat könnyen törlésre adott görbévé példa feladatban paramétert. Az ideiglenes fájlok eltávolításához használja a következő parancsot:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Ha meg szeretné futtassa újra a parancsot, a kimeneti könyvtár is törölnie kell. A következő használja ezt a címtárat törlése:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik megtanulta Mahout használatáról, felfedezése további módszerek az adatok HDInsight használata:

* [A HDInsight struktúra](hdinsight-use-hive.md)
* [Malac hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [A HDInsight MapReduce](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
