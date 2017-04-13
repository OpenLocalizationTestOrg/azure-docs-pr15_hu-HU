<properties
    pageTitle="A Hadoop minták futtatja a HDInsight |} Microsoft Azure"
    description="Ismerkedés az Azure hdinsight szolgáltatáshoz szolgáltatással együtt a minták a nyújtott. Adatok fürt MapReduce programok futtatása PowerShell-parancsprogramok használja."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>

#<a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>A Windows-alapú HDInsight minták Hadoop MapReduce futtatása

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Egy sor olyan mintákat segítséget elindított futó MapReduce feladatok Hadoop fürt használatával Azure hdinsight szolgáltatáshoz a találhatók. Ezeket a mintákat is elérhetők a felügyelt HDInsight fürt létrehozott minden egyes. Ezeket a minták futtatása Ismerkedjen meg az Azure PowerShell-parancsmagok használata Hadoop fürt feladat futtatható.

- [**Word-darab**][hdinsight-sample-wordcount]: megszámolja a word előfordulások szövegfájlban.
- [**C# a szavak száma streaming**][hdinsight-sample-csharp-streaming]: megszámolja a Hadoop adatfolyam felületén szövegfájl word előfordulás.
- [**Pi becslés**][hdinsight-sample-pi-estimator]: egy statisztikai használ (kvázi Monte Carlo) módszer átböngészve felmérheti a pi matematikai állandót.
- [**10 GB-os Graysort**][hdinsight-sample-10gb-graysort]: 10 GB-os fájl egy általános célú GraySort futtatni HDInsight használatával. Vannak olyan három feladat: Teragen az adatokat, az adatok rendezését szolgáló Terasort és Teravalidate kattintva erősítse meg, hogy az adatok megfelelően rendezett létrehozásához.

>[AZURE.NOTE] A forráskód függelékében találhatók. 

A webes Hadoop kapcsolódó technológiák, például Java-alapú MapReduce programozás és a folyamatos átvitelű és a Windows PowerShellben használt parancsmagokról dokumentációjában létezik sokkal dokumentációt parancsfájlok. Ezek az erőforrások kapcsolatos további tudnivalókért lásd:

- [A HDInsight Hadoop fejleszthet olyan Java MapReduce programok](hdinsight-develop-deploy-java-mapreduce-linux.md)
- [A HDInsight Hadoop-feladatok elküldése](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Azure hdinsight szolgáltatáshoz – bevezetés][hdinsight-introduction]

Ma sok személyt válassza a struktúra és malac MapReduce fölé.  További tudnivalókért lásd:

- [A HDInsight struktúra](hdinsight-use-hive.md)
- [Malac HDInsight használata](hdinsight-use-pig.md)
 
**Előfeltételek**:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **egy HDInsight fürthöz**. A különböző módon, amelyben ilyen fürt hozhatók létre, tanulmányozza [a HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md).
- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="hdinsight-sample-wordcount"></a>Word-száma - Java 

MapReduce projekt elküldése, akkor először hozzon létre egy MapReduce feladat definíciója. A feladat definíciója, adja meg a MapReduce program üveg fájlt, és a üveg fájl, amely **wasbs:///example/jars/hadoop-mapreduce-examples.jar**, az osztály nevét és az argumentumokat.  A wordcount MapReduce program veszi két argumentuma: a forrásfájl szavakat, és a kimenet helyét tartalmazó cellákat számlálnia használt.

A forráskód [mintája](#apendix-a---the-word-count-MapReduce-program-in-java)találhatók.

Az eljárás fejlesztését Java MapReduce-programot, lásd: – [a HDInsight Hadoop-programok Java MapReduce kidolgozása](hdinsight-develop-deploy-java-mapreduce-linux.md)
 
**A word darab MapReduce feladat elküldése**

1. Nyissa meg a **Windows PowerShell ISE**. Című cikkben olvashat [Telepítse és állítsa be a Azure PowerShell][powershell-install-configure].
2. Illessze be az alábbi PowerShell parancsprogramot:

        $subscriptionName = "<Azure Subscription Name>"
        $resourceGroupName = "<Resource Group Name>"
        $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name
        
        Select-AzureRmSubscription -SubscriptionName $subscriptionName
        
        # Define the MapReduce job
        $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "wordcount" `
                                    -Arguments "wasbs:///example/data/gutenberg/davinci.txt", "wasbs:///example/data/WordCountOutput1"
        
        # Submit the job and wait for job completion
        $cred = Get-Credential -Message "Enter the HDInsight cluster HTTP user credential:" 
        $mrJob = Start-AzureRmHDInsightJob `
                            -ResourceGroupName $resourceGroupName `
                            -ClusterName $clusterName `
                            -HttpCredential $cred `
                            -JobDefinition $mrJobDefinition 
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -JobId $mrJob.JobId 
        
        # Get the job output
        $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
        $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
        $defaultStorageContainer = $cluster.DefaultStorageContainer
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $clusterName `
            -HttpCredential $cred `
            -DefaultStorageAccountName $defaultStorageAccount `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultContainer $defaultStorageContainer  `
            -JobId $mrJob.JobId `
            -DisplayOutputType StandardError

        # Download the job output to the workstation
        $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
        Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force
        
        # Display the output file
        cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"

    A MapReduce feladat hoz létre egy *része-r-00000*, mely tartalmazza a szavakat, és a számlálás nevű fájlt. A parancsfájl a **findstr** parancs, amely tartalmazza a *"nem"*szót az összes lista.

3. Az első 3 változók beállítva, és futtassa a.

## <a name="hdinsight-sample-csharp-streaming"></a>Word-száma - C# streaming

Hadoop adatfolyam API-val MapReduce, amely lehetővé teszi, hogy a térkép írása, és csökkentése Java eltérő nyelvű függvényeket tartalmazza.

> [AZURE.NOTE] A lépéseket, ebben az oktatóanyagban csak a Windows-alapú HDInsight fürt vonatkoznak. Példa a folyamatos átvitelű Linux-alapú HDInsight fürtre vonatkozóan olvassa el a [kidolgozása Python adatfolyam HDInsight-alkalmazást](hdinsight-hadoop-streaming-python.md)című témakört.

A példában a hozzárendelést és a nyomáscsökkentő is a bemeneti olvassák [stdin] végrehajtható[ stdin-stdout-stderr] (által sorról sorra) és a kimeneti [stdout]elhelyezése[stdin-stdout-stderr]. A program a szöveget a szavak Összeszámlálja.

Végrehajtható **mappers**van megadva, ha egyes hozzárendelést tevékenységek elindul, a végrehajtható fájl egy külön folyamatot, ha a hozzárendelést inicializált van. A hozzárendelést feladat fut, akkor annak a bemeneti konvertálja a sorok, és a sorokban a [stdin] hírcsatornákkal[ stdin-stdout-stderr] a folyamat.

Időközben a hozzárendelést gyűjti össze a sor alapú eredménye a stdout a folyamat. Minden sor alakítja a kulcs/érték pár, amelyet a hozzárendelést kibocsátásának gyűjtenek. Alapértelmezés szerint az előtag felfelé az első tabulátorjel vonal a billentyűt, és a vonal (kivéve a tabulátorjelet) maradéka az az érték. Ha a sor nincs tabulátorjel, teljes sor tekinteni a billentyűt, és az értéke null.

Végrehajtható **szűkítő**van megadva, ha egyes nyomáscsökkentő tevékenységek elindul, a végrehajtható fájl egy külön folyamatot, ha a nyomáscsökkentő inicializált van. A nyomáscsökkentő feladat fut, a bemeneti értékek/kulcs párban alakítja sorokat, és azt a sorokban a [stdin] hírcsatornákkal[ stdin-stdout-stderr] a folyamat.

Időközben a nyomáscsökkentő gyűjti össze a sor alapú eredménye a [stdout] [ stdin-stdout-stderr] a folyamat. Minden sor konvertál egy kulcsot/érték pár, amelyet a nyomáscsökkentő kibocsátásának gyűjtenek. Alapértelmezés szerint az előtag felfelé az első tabulátorjel vonal a billentyűt, és a vonal (kivéve a tabulátorjelet) maradéka az az érték.

A Hadoop Streaming felület kapcsolatos további tudnivalókért olvassa el a [Hadoop adatfolyam] [hadoop-adatfolyam] című témakört.

**A C# word darab feladat streaming elküldése**

- Kövesse a [szavak száma - Java](#word-count-java), és a feladat definíciója lecserélése a következőre:

        $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                                -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                                -Mapper "cat.exe" `
                                -Reducer "wc.exe" `
                                -InputPath "/example/data/gutenberg/davinci.txt" `
                                -OutputPath "/example/data/StreamingOutput/wc.txt"  


    A kimeneti fájl a következők:
    
        example/data/StreamingOutput/wc.txt/part-00000      
                                
## <a name="hdinsight-sample-pi-estimator"></a>A PI becslés

A pi becslés használja a statisztikai (kvázi Monte Carlo) módszer a pi matematikai állandót. Véletlenszerűen helyezni időegység pontok négyzetes is tartoznak a kör területe egyenlő valószínűség belül négyzetre ütemtervben kör pi/4. A pi értékét a 4R, R esetén, amelyek belül, amelyek a téglalap alakú belül pontok száma a kör pontok száma arányát az értékek is becsült. Minél nagyobb a minta felhasznált pontok, annál hatékonyabb becslés van.

A parancsfájl, ez a példa előírt Hadoop üveg feladatot nyújt, és értéke be értéket tartalmazó futtatása 16 térképeket, amelyek szükséges 10 millió minta pontok kiszámítania paraméter értékét. Következő paraméterértékeket javítható a pi matematikai állandót becsült módosítható. Hivatkozás az első 10 tizedesjegyek pi 3.1415926535 is.

**A pi becslés feladat elküldése**

- Kövesse a [szavak száma - Java](#word-count-java), és a feladat definíciója lecserélése a következőre:

        $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                    -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
                                    -ClassName "pi" `
                                    -Arguments "16", "10000000"

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB-os Graysort

Ez a példa használja a mérsékelt 10GB adatot, hogy gyorsan viszonylag futtathatók. A Owen O'Malley és Arun Murthy, amely a éves általános célú ("daytona") adjon rendezés viszonyítási alap nyert 2009 sebesség 0.578 TB/Min (100 TB 173 perc) által fejlesztett MapReduce alkalmazást használ. Ez, és más rendezési szintek a további tudnivalókért lásd: a [Sortbenchmark](http://sortbenchmark.org/) webhelyet.

Ez a példa három különböző MapReduce programokat használja:

1. **TeraGen** egy olyan MapReduce program, a sorok adatok létrehozásához használható.
2. **TeraSort** minták a bemeneti adatok, és rendezze az adatokat egy teljes rendelés MapReduce használ. TeraSort MapReduce függvények kivételével egy egyéni partitioner egy rendezett listát, amely az egyes csökkentése kulcs esik mintát N-1 kulcsok használó szabványos rendezésének. Különösen az összes kulcsokat ilyen példa a [i-1] < = billentyű < minta [i] küldjük i csökkentése érdekében. Ki, hogy a kimeneti értékeket a csökkentése i garanciákkal-nél kisebb kibocsátásának csökkentése i + 1 billentyűkombinációt.
3. **TeraValidate** rendszer ellenőrzi, hogy a kimenet globálisan rendezett MapReduce programot. A kimenet könyvtár hoz létre egy térképen fájlonként, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő az előzőt. A térkép függvény is létrehozza az első és utolsó kulcsok, az egyes fájlok rekordokat, és a csökkentése függvény biztosítja, hogy az első billentyű fájl i nagyobb, mint a fájl i-1 az utolsó billentyűt. Esetleges problémákat, amelyek nem működik a billentyűkkel a csökkentése egy kimenetét jelentett.

A bemeneti és kimeneti formátumban, az összes három alkalmazások által használt beolvassa és a szöveg fájlokat a megfelelő formátumban. A kimenet: a csökkentése tartalmaz a replikáció értéke 1, az alapértelmezés 3 helyett, mert a javasolt Beküldve nincs szükség az, hogy a kimeneti adatok replikált kell-e több csomópontok alkalmazásba című témakör tartalmaz.

Három műveletet a minta megfelelő a bevezetőben leírt MapReduce programok által van szükség:

1. Az adatok rendezése a **TeraGen** MapReduce feladat futtatása készítéséhez.
2. Rendezze az adatokat a **TeraSort** MapReduce feladat futtatásával.
3. Győződjön meg arról, hogy az adatok megfelelően szerint rendezett a **TeraValidate** MapReduce feladat futtatása.

**A feladatok elküldése**

- Kövesse a [szavak száma - Java](#word-count-java), és használja az alábbi feladatdefinícióinak:

    $teragen új-AzureRmHDInsightMapReduceJobDefinition = `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - osztálynév "teragen" "-argumentumokat"-Dmapred.map.tasks=50 ","100000000","/ Példa / / 10 GB-rendezés-adatbevitel"
    
    $terasort új-AzureRmHDInsightMapReduceJobDefinition = `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - osztálynév "terasort" "-argumentumokat"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ Példa / / 10 GB-rendezés-adatbevitel","/ Példa / / 10 GB-rendezés-kimeneti adatok"
    
    $teravalidate új-AzureRmHDInsightMapReduceJobDefinition = `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" ` 
   - osztálynév "teravalidate" "-argumentumokat"-Dmapred.map.tasks=50 ","-Dmapred.reduce.tasks=25 ","/ Példa / / 10 GB-rendezés-kimeneti adatok","/ Példa/adatok/10 GB – rendezés-érvényesítése"


##<a name="next-steps"></a>Következő lépések 

Ez a cikk és a minták mindegyikében a cikkek hogyan kell futtatni a minták Azure PowerShell használatával a HDInsight fürt részét képező megtanulta azt. Az oktatóprogram HDInsight malac struktúra és MapReduce használatáról a következő témakörökben olvashatók:

* [Elkezdeni használni Hadoop HDInsight struktúra mobil kézibeszélőt használati elemzéséhez][hdinsight-get-started]
* [A HDInsight Hadoop malac használata][hdinsight-use-pig]
* [A HDInsight Hadoop struktúra használata][hdinsight-use-hive]
* [A HDInsight Hadoop-feladatok elküldése] [hdinsight-submit-jobs]
* [Azure hdinsight szolgáltatáshoz SDK dokumentáció][hdinsight-sdk-documentation]
* [A HDInsight Hadoop hibakeresési: hibaüzenetek] [hdinsight-errors]


## <a name="appendix-a---the-word-count-source-code"></a>Mintája – a Word darab forráskód

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


## <a name="appendix-b---the-word-count-streaming-source-code"></a>Függelék F – a szavak száma a folyamatos átvitelű forráskód

A MapReduce program cat.exe alkalmazása egy hozzárendelés kapcsolat kattintva letöltheti a szöveget a konzol és vannak lejátszott online, a dokumentum szavainak megszámlálására a csökkentése felület wc.exe alkalmazása. A hozzárendelést és a nyomáscsökkentő karakter, amelyet sorról sorra, a normál beviteli adatfolyam (stdin) és olvasási a normál kimenő adatfolyam (stdout).

    // The source code for the cat.exe (Mapper).

    using System;
    using System.IO;

    namespace cat
    {
        class cat
        {
            static void Main(string[] args)
            {
                if (args.Length > 0)
                {
                    Console.SetIn(new StreamReader(args[0]));
                }

                string line;
                while ((line = Console.ReadLine()) != null)
                {
                    Console.WriteLine(line);
                }
            }
        }
    }



A cat.cs fájlban a hozzárendelést kódot használ egy [StreamReader osztályra] [ streamreader] olvassa el a karaktereket a konzolt, majd az adatfolyam ír a normál kimenő adatfolyam a statikus [Console.WriteLine("a(z)] a bejövő adatfolyam-objektum[ console-writeline] módot.


    // The source code for wc.exe (Reducer) is:

    using System;
    using System.IO;
    using System.Linq;

    namespace wc
    {
        class wc
        {
            static void Main(string[] args)
            {
                string line;
                var count = 0;

                if (args.Length > 0){
                    Console.SetIn(new StreamReader(args[0]));
                }

                while ((line = Console.ReadLine()) != null) {
                    count += line.Count(cr => (cr == ' ' || cr == '\n'));
                }
                Console.WriteLine(count);
            }
        }
    }


A wc.cs fájlban a nyomáscsökkentő kódot használ egy [StreamReader osztályra] [ streamreader] karakterek olvashatja őket a szabványos, amelyeket a cat.exe hozzárendelést kimenete bemeneti adatfolyam-objektumot. Mivel a [Console.WriteLine("a(z)] a karaktert olvas[ console-writeline] módszer, azt számolja a szavakat szóközök és minden szó végén sor végén karakterek megszámlálása útján. A normál kimenő adatfolyam a [Console.WriteLine("a(z)] a teljes majd bejegyzi[ console-writeline] módot.





## <a name="appendix-c---the-pi-estimator-source-code"></a>Függelék C – a Pi becslés forráskód

A pi becslés Java-kód, amely tartalmazza a hozzárendelést és nyomáscsökkentő függvény az alábbi ellenőrzésre érhető el. A hozzárendelést a program létrehoz egy megadott számú ponttal, egység négyzet véletlenszerűen helyezni, és ezután megszámolja azokat, amelyek a körbe pontok száma. A nyomáscsökkentő program által a mappers megszámlált pontok összegzi, és ezután becslést ad a képlet 4R, R esetén, amelyek a téglalap alakú belül pontok száma a körbe megszámlált pontok száma arányát a pi matematikai állandót.

    /**
    * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements. See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership. The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License. You may obtain a copy of the License at
    *
    * http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or   implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */

    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.math.BigDecimal;
    import java.util.Iterator;

    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.BooleanWritable;
    import org.apache.hadoop.io.LongWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Writable;
    import org.apache.hadoop.io.WritableComparable;
    import org.apache.hadoop.io.SequenceFile.CompressionType;
    import org.apache.hadoop.mapred.FileInputFormat;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.MapReduceBase;
    import org.apache.hadoop.mapred.Mapper;
    import org.apache.hadoop.mapred.OutputCollector;
    import org.apache.hadoop.mapred.Reducer;
    import org.apache.hadoop.mapred.Reporter;
    import org.apache.hadoop.mapred.SequenceFileInputFormat;
    import org.apache.hadoop.mapred.SequenceFileOutputFormat;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;


    //A Map-reduce program to estimate the value of Pi
    //using quasi-Monte Carlo method.
    //
    //Mapper:
    //Generate points in a unit square
    //and then count points inside/outside of the inscribed circle of the square.
    //
    //Reducer:
    //Accumulate points inside/outside results from the mappers.
    //Let numTotal = numInside + numOutside.
    //The fraction numInside/numTotal is a rational approximation of
    //the value (Area of the circle)/(Area of the square),
    //where the area of the inscribed circle is Pi/4
    //and the area of unit square is 1.
    //Then, Pi is estimated value to be 4(numInside/numTotal).
    //

    public class PiEstimator extends Configured implements Tool {
    //tmp directory for input/output
    static private final Path TMP_DIR = new Path(
    PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

    //2-dimensional Halton sequence {H(i)},
    //where H(i) is a 2-dimensional point and i >= 1 is the index.
    //Halton sequence is used to generate sample points for Pi estimation.
    private static class HaltonSequence {
    // Bases
    static final int[] P = {2, 3};
    //Maximum number of digits allowed
    static final int[] K = {63, 40};

    private long index;
    private double[] x;
    private double[][] q;
    private int[][] d;

    //Initialize to H(startindex),
    //so the sequence begins with H(startindex+1).
    HaltonSequence(long startindex) {
    index = startindex;
    x = new double[K.length];
    q = new double[K.length][];
    d = new int[K.length][];
    for(int i = 0; i < K.length; i++) {
    q[i] = new double[K[i]];
    d[i] = new int[K[i]];
    }

    for(int i = 0; i < K.length; i++) {
    long k = index;
    x[i] = 0;

    for(int j = 0; j < K[i]; j++) {
    q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
    d[i][j] = (int)(k % P[i]);
    k = (k - d[i][j])/P[i];
    x[i] += d[i][j] * q[i][j];
    }
    }
    }

    //Compute next point.
    //Assume the current point is H(index).
    //Compute H(index+1).
    //@return a 2-dimensional point with coordinates in [0,1)^2
    double[] nextPoint() {
    index++;
    for(int i = 0; i < K.length; i++) {
    for(int j = 0; j < K[i]; j++) {
    d[i][j]++;
    x[i] += q[i][j];
    if (d[i][j] < P[i]) {
    break;
    }
    d[i][j] = 0;
    x[i] -= (j == 0? 1.0: q[i][j-1]);
    }
    }
    return x;
    }
    }

    //Mapper class for Pi estimation.
    //Generate points in a unit square and then
    //count points inside/outside of the inscribed circle of the square.
    public static class PiMapper extends MapReduceBase
    implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

    //Map method.
    //@param offset samples starting from the (offset+1)th sample.
    //@param size the number of samples for this map
    //@param out output {ture->numInside, false->numOutside}
    //@param reporter
    public void map(LongWritable offset,
    LongWritable size,
    OutputCollector<BooleanWritable, LongWritable> out,
    Reporter reporter) throws IOException {

    final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
    long numInside = 0L;
    long numOutside = 0L;

    for(long i = 0; i < size.get(); ) {
    //generate points in a unit square
    final double[] point = haltonsequence.nextPoint();

    //count points inside/outside of the inscribed circle of the square
    final double x = point[0] - 0.5;
    final double y = point[1] - 0.5;
    if (x*x + y*y > 0.25) {
    numOutside++;
    } else {
    numInside++;
    }

    //report status
    i++;
    if (i % 1000 == 0) {
    reporter.setStatus("Generated " + i + " samples.");
    }
    }

    //output map results
    out.collect(new BooleanWritable(true), new LongWritable(numInside));
    out.collect(new BooleanWritable(false), new LongWritable(numOutside));
    }
    }


    //Reducer class for Pi estimation.
    //Accumulate points inside/outside results from the mappers.
    public static class PiReducer extends MapReduceBase
    implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

    private long numInside = 0;
    private long numOutside = 0;
    private JobConf conf; //configuration for accessing the file system

    //Store job configuration.
    @Override
    public void configure(JobConf job) {
    conf = job;
    }


    // Accumulate number of points inside/outside results from the mappers.
    // @param isInside Is the points inside?
    // @param values An iterator to a list of point counts
    // @param output dummy, not used here.
    // @param reporter

    public void reduce(BooleanWritable isInside,
    Iterator<LongWritable> values,
    OutputCollector<WritableComparable<?>, Writable> output,
    Reporter reporter) throws IOException {
    if (isInside.get()) {
    for(; values.hasNext(); numInside += values.next().get());
    } else {
    for(; values.hasNext(); numOutside += values.next().get());
    }
    }

    //Reduce task done, write output to a file.
    @Override
    public void close() throws IOException {
    //write output to a file
    Path outDir = new Path(TMP_DIR, "out");
    Path outFile = new Path(outDir, "reduce-out");
    FileSystem fileSys = FileSystem.get(conf);
    SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
    outFile, LongWritable.class, LongWritable.class,
    CompressionType.NONE);
    writer.append(new LongWritable(numInside), new LongWritable(numOutside));
    writer.close();
    }
    }

    //Run a map/reduce job for estimating Pi.
    //@return the estimated value of Pi.
    public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
    )
    throws IOException {
    //setup job conf
    jobConf.setJobName(PiEstimator.class.getSimpleName());

    jobConf.setInputFormat(SequenceFileInputFormat.class);

    jobConf.setOutputKeyClass(BooleanWritable.class);
    jobConf.setOutputValueClass(LongWritable.class);
    jobConf.setOutputFormat(SequenceFileOutputFormat.class);

    jobConf.setMapperClass(PiMapper.class);
    jobConf.setNumMapTasks(numMaps);

    jobConf.setReducerClass(PiReducer.class);
    jobConf.setNumReduceTasks(1);

    // turn off speculative execution, because DFS doesn't handle
    // multiple writers to the same file.
    jobConf.setSpeculativeExecution(false);

    //setup input/output directories
    final Path inDir = new Path(TMP_DIR, "in");
    final Path outDir = new Path(TMP_DIR, "out");
    FileInputFormat.setInputPaths(jobConf, inDir);
    FileOutputFormat.setOutputPath(jobConf, outDir);

    final FileSystem fs = FileSystem.get(jobConf);
    if (fs.exists(TMP_DIR)) {
     throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
     + " already exists. Please remove it first.");
     }
     if (!fs.mkdirs(inDir)) {
     throw new IOException("Cannot create input directory " + inDir);
     }

     //generate an input file for each map task
     try {
     for(int i=0; i < numMaps; ++i) {
     final Path file = new Path(inDir, "part"+i);
     final LongWritable offset = new LongWritable(i * numPoints);
     final LongWritable size = new LongWritable(numPoints);
     final SequenceFile.Writer writer = SequenceFile.createWriter(
     fs, jobConf, file,
     LongWritable.class, LongWritable.class, CompressionType.NONE);
     try {
     writer.append(offset, size);
     } finally {
     writer.close();
     }
     System.out.println("Wrote input for Map #"+i);
     }

     //start a map/reduce job
     System.out.println("Starting Job");
     final long startTime = System.currentTimeMillis();
     JobClient.runJob(jobConf);
     final double duration = (System.currentTimeMillis() - startTime)/1000.0;
     System.out.println("Job Finished in " + duration + " seconds");

     //read outputs
     Path inFile = new Path(outDir, "reduce-out");
     LongWritable numInside = new LongWritable();
     LongWritable numOutside = new LongWritable();
     SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
     try {
     reader.next(numInside, numOutside);
     } finally {
     reader.close();
     }

     //compute estimated value
     return BigDecimal.valueOf(4).setScale(20)
     .multiply(BigDecimal.valueOf(numInside.get()))
     .divide(BigDecimal.valueOf(numMaps))
     .divide(BigDecimal.valueOf(numPoints));
     } finally {
     fs.delete(TMP_DIR, true);
     }
     }

    //Parse arguments and then runs a map/reduce job.
    //Print output in standard out.
    //@return a non-zero if there is an error. Otherwise, return 0.
     public int run(String[] args) throws Exception {
     if (args.length != 2) {
     System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
     ToolRunner.printGenericCommandUsage(System.err);
     return -1;
     }

     final int nMaps = Integer.parseInt(args[0]);
     final long nSamples = Long.parseLong(args[1]);

     System.out.println("Number of Maps = " + nMaps);
     System.out.println("Samples per Map = " + nSamples);

     final JobConf jobConf = new JobConf(getConf(), getClass());
     System.out.println("Estimated value of Pi is "
     + estimate(nMaps, nSamples, jobConf));
     return 0;
     }

     //main method for running it as a stand alone command.
     public static void main(String[] argv) throws Exception {
     System.exit(ToolRunner.run(null, new PiEstimator(), argv));
     }
     }
     
## <a name="appendix-d---the-10gb-graysort-source-code"></a>Függelék E – a 10 GB-os graysort forráskód

A kódot a TeraSort MapReduce programban ellenőrzésre ebben a szakaszban látható.


    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     *     http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

    package org.apache.hadoop.examples.terasort;

    import java.io.IOException;
    import java.io.PrintStream;
    import java.net.URI;
    import java.util.ArrayList;
    import java.util.List;

    import org.apache.commons.logging.Log;
    import org.apache.commons.logging.LogFactory;
    import org.apache.hadoop.conf.Configured;
    import org.apache.hadoop.filecache.DistributedCache;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.NullWritable;
    import org.apache.hadoop.io.SequenceFile;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapred.FileOutputFormat;
    import org.apache.hadoop.mapred.JobClient;
    import org.apache.hadoop.mapred.JobConf;
    import org.apache.hadoop.mapred.Partitioner;
    import org.apache.hadoop.util.Tool;
    import org.apache.hadoop.util.ToolRunner;

    /**
     * Generates the sampled split points, launches the job,
     * and waits for it to finish.
     * <p>
     * To run the program:
     * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
     */

    public class TeraSort extends Configured implements Tool {
      private static final Log LOG = LogFactory.getLog(TeraSort.class);

      /**
       * A partitioner that splits text keys into roughly equal
       * partitions in a global sorted order.
       */

      static class TotalOrderPartitioner implements Partitioner<Text,Text>{
        private TrieNode trie;
        private Text[] splitPoints;

        /**
         * A generic trie node
         */
        static abstract class TrieNode {
          private int level;
          TrieNode(int level) {
            this.level = level;
          }
          abstract int findPartition(Text key);
          abstract void print(PrintStream strm) throws IOException;
          int getLevel() {
            return level;
          }
        }

        /**
         * An inner trie node that contains 256 children based on the next
         * character.
         */
        static class InnerTrieNode extends TrieNode {
          private TrieNode[] child = new TrieNode[256];

          InnerTrieNode(int level) {
            super(level);
          }
          int findPartition(Text key) {
            int level = getLevel();
            if (key.getLength() <= level) {
              return child[0].findPartition(key);
            }
            return child[key.getBytes()[level]].findPartition(key);
          }
          void setChild(int idx, TrieNode child) {
            this.child[idx] = child;
          }
          void print(PrintStream strm) throws IOException {
            for(int ch=0; ch < 255; ++ch) {
              for(int i = 0; i < 2*getLevel(); ++i) {
                strm.print(' ');
              }
              strm.print(ch);
              strm.println(" ->");
              if (child[ch] != null) {
                child[ch].print(strm);
              }
            }
          }
        }

        /**
         * A leaf trie node that does string compares to figure out where the given
         * key belongs between lower..upper.
         */
        static class LeafTrieNode extends TrieNode {
          int lower;
          int upper;
          Text[] splitPoints;
          LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
            super(level);
            this.splitPoints = splitPoints;
            this.lower = lower;
            this.upper = upper;
          }
          int findPartition(Text key) {
            for(int i=lower; i<upper; ++i) {
              if (splitPoints[i].compareTo(key) >= 0) {
                return i;
              }
            }
            return upper;
          }
          void print(PrintStream strm) throws IOException {
            for(int i = 0; i < 2*getLevel(); ++i) {
              strm.print(' ');
            }
            strm.print(lower);
            strm.print(", ");
            strm.println(upper);
          }
        }


        /**
         * Read the cut points from the given sequence file.
         * @param fs the file system
         * @param p the path to read
         * @param job the job config
         * @return the strings to split the partitions on
         * @throws IOException
         */
        private static Text[] readPartitions(FileSystem fs, Path p,
                                             JobConf job) throws IOException {
          SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
          List<Text> parts = new ArrayList<Text>();
          Text key = new Text();
          NullWritable value = NullWritable.get();
          while (reader.next(key, value)) {
            parts.add(key);
            key = new Text();
          }
          reader.close();
          return parts.toArray(new Text[parts.size()]);  
        }

        /**
         * Given a sorted set of cut points, build a trie that will find the correct
         * partition quickly.
         * @param splits the list of cut points
         * @param lower the lower bound of partitions 0..numPartitions-1
         * @param upper the upper bound of partitions 0..numPartitions-1
         * @param prefix the prefix that we have already checked against
         * @param maxDepth the maximum depth we will build a trie for
         * @return the trie node that will divide the splits correctly
         */
        private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                          Text prefix, int maxDepth) {
          int depth = prefix.getLength();
          if (depth >= maxDepth || lower == upper) {
            return new LeafTrieNode(depth, splits, lower, upper);
          }
          InnerTrieNode result = new InnerTrieNode(depth);
          Text trial = new Text(prefix);
          // append an extra byte on to the prefix
          trial.append(new byte[1], 0, 1);
          int currentBound = lower;
          for(int ch = 0; ch < 255; ++ch) {
            trial.getBytes()[depth] = (byte) (ch + 1);
            lower = currentBound;
            while (currentBound < upper) {
              if (splits[currentBound].compareTo(trial) >= 0) {
                break;
              }
              currentBound += 1;
            }
            trial.getBytes()[depth] = (byte) ch;
            result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                         maxDepth);
          }
          // pick up the rest
          trial.getBytes()[depth] = 127;
          result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                        maxDepth);
          return result;
        }

        public void configure(JobConf job) {
          try {
            FileSystem fs = FileSystem.getLocal(job);
            Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
            splitPoints = readPartitions(fs, partFile, job);
            trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
          } catch (IOException ie) {
            throw new IllegalArgumentException("can't read paritions file", ie);
          }
        }

        public TotalOrderPartitioner() {
        }

        public int getPartition(Text key, Text value, int numPartitions) {
          return trie.findPartition(key);
        }

      }

      public int run(String[] args) throws Exception {
        LOG.info("starting");
        JobConf job = (JobConf) getConf();
        Path inputDir = new Path(args[0]);
        inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
        Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
        URI partitionUri = new URI(partitionFile.toString() +
                                   "#" + TeraInputFormat.PARTITION_FILENAME);
        TeraInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        job.setJobName("TeraSort");
        job.setJarByClass(TeraSort.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(Text.class);
        job.setInputFormat(TeraInputFormat.class);
        job.setOutputFormat(TeraOutputFormat.class);
        job.setPartitionerClass(TotalOrderPartitioner.class);
        TeraInputFormat.writePartitionFile(job, partitionFile);
        DistributedCache.addCacheFile(partitionUri, job);
        DistributedCache.createSymlink(job);
        job.setInt("dfs.replication", 1);
        TeraOutputFormat.setFinalSync(job, true);
        JobClient.runJob(job);
        LOG.info("done");
        return 0;
      }

      /**
       * @param args
       */

      public static void main(String[] args) throws Exception {
        int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
        System.exit(res);
      }
    }














 




[hdinsight-errors]: hdinsight-debug-jobs.md

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md


[powershell-install-configure]: ../powershell-install-configure.md

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
