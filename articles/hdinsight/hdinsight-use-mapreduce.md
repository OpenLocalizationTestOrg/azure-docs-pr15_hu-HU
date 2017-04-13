<properties
   pageTitle="A Hadoop a HDInsight MapReduce |} Microsoft Azure"
   description="Megtudhatja, hogy miként MapReduce feladatok futtatásához Hadoop fürt hdinsight szolgáltatásból lehetőségre. Egy egyszerű word darab művelet Java MapReduce feladat végrehajtását futtatásával."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>A HDInsight Hadoop MapReduce használata

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ebben a cikkben megismerheti, hogyan, MapReduce feladatok futtatásához Hadoop HDInsight fürt lesz. Java MapReduce feladat végrehajtását egyszerű word darab műveletet futtatni azt.

##<a id="whatis"></a>Mi az MapReduce?

Hadoop MapReduce feladatok folyamat nagy mennyiségű adatot ír szoftver kerete. A bemeneti adatok független szövegadattömb, majd párhuzamosan futnak végig a fürt csomópontjai oszlik. A két MapReduce feladat áll:

* **Hozzárendelést**: bemeneti adatok fogyasztása elemzi azt (általában a szűrés és rendezés műveletek) és bocsát kockára (kulcs – érték párokká)
* **Nyomáscsökkentő**: a hozzárendelést által kibocsátott kockára fogyasztása és összefoglaló műveletet, amely hoz létre egy kisebb, az egyesített eredményét az hozzárendelést adatok hajt végre

Egy egyszerű word darab MapReduce feladat példa mutatja be a következő diagramon:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

A kimenet a feladat minden szó következett be a szöveget, amelynek elemezni volt, hogy hány alkalommal számának.

* A hozzárendelést megnyitja az egyes címsorokra, a bemeneti szövegre egy bemeneteként, és oszlik a szavakat. Egy kulcsot értéket bocsát ki, akkor pár szó akkor fordul elő, a Word minden alkalommal, amikor a egy 1 követ. A kimenet vannak rendezve, mielőtt elküldené nyomáscsökkentő.

* A nyomáscsökkentő ezek egyes száma az egyes word összesíti, és bocsát ki egyetlen kulcs/érték, amely tartalmazza a szó az előfordulások összegét és két.

MapReduce végrehajtható különböző nyelven. Java a legelterjedtebb, és a dokumentum, bemutató célokra szolgál.

### <a name="hadoop-streaming"></a>A folyamatos átvitelű Hadoop

Közvetlenül feladatként MapReduce, Java-alkalmazások hasonló nyelvek vagy Java- és a Java virtuális gép (például Scalding vagy kaszkádolás,) alapuló keretek is futtatta. Mások számára, például a C# vagy Python vagy különálló végrehajtható, kell használnia a folyamatos átvitelű Hadoop.

A folyamatos átvitelű Hadoop kommunikál a hozzárendelést és nyomáscsökkentő STDIN és STDOUT – a hozzárendelést és nyomáscsökkentő adatok vonal STDIN egyszerre, és olvasási a kimenet STDOUT. Minden sor, olvassa el, vagy a hozzárendelést és nyomáscsökkentő által kibocsátott kulcs/érték pár, egy lap charaacter elválasztva a formátumban kell megadni:

    [key]/t[value]

További tudnivalókért olvassa el a [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html)című témakört.

Példák a HDInsight streaming Hadoop talál a következő:

* [Python MapReduce feladatok kidolgozása](hdinsight-hadoop-streaming-python.md)

##<a id="data"></a>Tudnivalók a mintaadatok

Ebben a példában a mintaadatokat, akkor érdemes használni Leonardo Da Vinci a jegyzetfüzeteket, a HDInsight fürt szöveges dokumentumként rendelkeznek.

A mintaadatok Azure Blob-tárolóhoz, amely HDInsight Hadoop fürtre vonatkozóan használja az alapértelmezett fájlrendszer vannak tárolva. A **wasb** előtag használatával Blob-tárolóban lévő tárolt fájlok HDInsight érheti el. Például a sample.log fájl elérésére, használja a következő szintaxist:

    wasbs:///example/data/gutenberg/davinci.txt

Mivel a Azure Blob-tárolóhoz HDInsight alapértelmezett tárolt adatait, **/example/data/gutenberg/davinci.txt**használatával is elérheti a fájlt.

> [AZURE.NOTE] Az előző szintaxisú utasítással **wasbs: / / /** használatával érheti el, amelyek a HDInsight fürt alapértelmezett tároló tárolója tárolja. Ha további tárterületet fiókok megadott telepítésekor, akkor a fürt, és el szeretne érni, az alábbi fiókok tárolt fájlok, elérheti az adatokat tároló nevét és a tárhely fiók címét megadásával. Ha például **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/gutenberg/davinci.txt**.

##<a id="job"></a>A példa MapReduce kapcsolatban

Az ebben a példában használt MapReduce feladat **wasbs://example/jars/hadoop-mapreduce-examples.jar**helyen található, és azt a HDInsight fürthöz megadva. A word darab példa futtatásakor **davinci.txt**ellen tartalmaz.

> [AZURE.NOTE] A HDInsight 2.1 fürt az elérési út **wasbs:///example/jars/hadoop-examples.jar**.

A hivatkozás, az alábbiakra a Java-kód, a word darab MapReduce projektre vonatkozóan:

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

      public static class TokenizerMapper
           extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
          StringTokenizer itr = new StringTokenizer(value.toString());
          while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
          }
        }
      }

      public static class IntSumReducer
           extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                           Context context
                           ) throws IOException, InterruptedException {
          int sum = 0;
          for (IntWritable val : values) {
            sum += val.get();
          }
          result.set(sum);
          context.write(key, result);
        }
      }

      public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
          System.err.println("Usage: wordcount <in> <out>");
          System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
      }
    }

A saját MapReduce feladat írni című témakör nyújt tájékoztatást [kidolgozása Java MapReduce HDInsight-alkalmazást](hdinsight-develop-deploy-java-mapreduce-linux.md).

##<a id="run"></a>A MapReduce futtatása

HDInsight futtatását is lehetővé teszi a HiveQL feladatok többféle módszerrel használatával. Döntse el, melyik módszert szüksége az alábbi táblázat segítségével, majd hajtsa végre az útmutató hivatkozását.

| **Ezzel a**...                                                    | **.. fenti művelet**                                       | .. .with **fürt operációs rendszer** | .. .from **ügyfél operációs rendszer** |
|:-------------------------------------------------------------------|:--------------------------------------------------------|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md)                       | A Hadoop paranccsal **SSH** keresztül                  | Linux                                     | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [Curl](hdinsight-hadoop-use-mapreduce-curl.md)                     | A feladat távolról elküldése **többi** használatával               | Linux vagy a Windows                          | Linux rendszerhez, a Unix, a Mac OS X vagy a Windows        |
| [A Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) | A feladat távolról nyújt a **Windows PowerShell** használatával | Linux vagy a Windows                          | A Windows                                  |
| [Távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop)    | A Hadoop paranccsal **Távoli asztali** keresztül       | A Windows                                   | A Windows                                  |

##<a id="nextsteps"></a>Következő lépések

Bár a MapReduce hatékony diagnosztikai tulajdonságainak lehetővé teszi, egy kicsit minta nehéz lehet. Vannak, amelyek megkönnyítik a MapReduce alkalmazások, valamint technológiák, például malac és struktúra, amelyek nyújtanak az adatok HDInsight használata egyszerűbb megadása több Java-alapú keretek. További tudnivalókért lásd: az alábbi cikkekben:

* [Fejleszthet olyan Java MapReduce programok hdinsight szolgáltatáshoz](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Folyamatos átvitelű MapReduce programok HDInsight-Python kidolgozása](hdinsight-hadoop-streaming-python.md)

* [A HDInsight Apache Hadoop forrázást MapReduce feladatok kidolgozása](hdinsight-hadoop-mapreduce-scalding.md)

* [HDInsight struktúra használata][hdinsight-use-hive]

* [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]

* [A HDInsight minták futtatása][hdinsight-samples]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[powershell-install-configure]: ../powershell-install-configure.md

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
