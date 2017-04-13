<properties
    pageTitle="Fejleszthet olyan Java MapReduce programok Linux-alapú HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként Java MapReduce programokat fejleszt, és üzembe helyezése Linux-alapú hdinsight szolgáltatásból lehetőségre."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="Blackmist"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight-linux"></a>Fejleszthet olyan Java MapReduce programok HDInsight Linux Hadoop

A dokumentumok végigvezeti MapReduce-alkalmazás létrehozása és üzembe helyezéséhez és futtatásával egy Linux-alapú Hadoop HDInsight fürt Apache maven tesztelése használatával.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7-es vagy újabb verzióját (vagy egy ezzel egyenértékű, például OpenJDK)

- [Apache maven tesztelése](http://maven.apache.org/)

- **Az Azure előfizetéssel**

- **Azure CLI**

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="configure-environment-variables"></a>Változók környezet beállítása

Az alábbi környezeti változók Java- és a JDK telepítésekor adható meg. Jó helyen jár ellenőrizze, hogy vannak, és a rendszer a megfelelő értékeket tartalmaznak.

* A címtárhoz, amelyen telepítve van a (JRE) Java-futtatókörnyezet **JAVA_HOME** - kell mutatnia. Például OS X, Unix vagy Linux rendszerben meg kell kell egy értéknek hasonló `/usr/lib/jvm/java-7-oracle`. A Windows rendszerben azt szeretné, hogy hasonló érték`c:\Program Files (x86)\Java\jre1.7`

* **Elérési út** - kell tartalmaznia az alábbi elérési utak:

    * **JAVA_HOME** (vagy a megfelelő elérési út)

    * **JAVA_HOME\bin** (vagy a megfelelő elérési út)

    * A könyvtár, amelyen telepítve van a maven tesztelése

##<a name="create-a-new-maven-project"></a>Maven tesztelése új projekt létrehozása

1. Munkamenet vagy a fejlesztői környezet parancssor váltson a könyvtárak arra a helyre, akkor szeretne tárolni a projektben.

3. A paranccsal __mvn__ , amelyen telepítve van a maven tesztelése, készítése a projekthez a állványon.

        mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Ezzel létrehoz egy új könyvtárat az aktuális mappában (**wordcountjava** ebben a példában.) __artifactID__ paraméterben megadott nevű Ezt a címtárat fogja tartalmazni az alábbiakat:

    * __pom.xml__ – a [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , amely tartalmazza a projekt létrehozásához használt adatok és a konfigurációs részletek.

    * __forrás__ -, amely tartalmazza a __fő/java/szervezeti/apache/hadoop/példák__ könyvtár, ahol az alkalmazás fog Szerző a címtárban.

3. Akkor nem fogja használni ebben a példában a __src/test/java/org/apache/hadoop/examples/apptest.java__ fájl törlése

##<a name="add-dependencies"></a>Függőségek hozzáadása

1. Módosítsa a __pom.xml__ fájlt, és adja hozzá a következő belül a `<dependencies>` szakasz:

        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-examples</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-mapreduce-client-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>
        <dependency>
          <groupId>org.apache.hadoop</groupId>
          <artifactId>hadoop-common</artifactId>
          <version>2.5.1</version>
          <scope>provided</scope>
        </dependency>

    Ez azt jelenti az maven tesztelése, hogy a projekt szükséges-e a tárak (belül felsorolt &lt;artifactId\>) egy adott verziójával (belül felsorolt &lt;verzió\>). Fordítási időben ez lesz lehet letölteni a alapértelmezett maven tesztelése tárat. Ha többet szeretne látni a [maven tesztelése tárházba keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) is használhatja.

    A `<scope>provided</scope>` alapján maven tesztelése, hogy a függőségek kell nem kell csomagolni az alkalmazásban, ahogy azok a HDInsight fürt futásidőben nyújtanak.

2. Adja hozzá a következő a __pom.xml__ fájlt. Belül kell lennie a `<project>...</project>` címkéket a fájlt. Ha például közötti `</dependencies>` és `</project>`.

        <build>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                    <goals>
                      <goal>shade</goal>
                    </goals>
                </execution>
              </executions>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
               <source>1.7</source>
               <target>1.7</target>
              </configuration>
            </plugin>
          </plugins>
        </build>

    Az első beépülő modul konfigurálja a [Maven tesztelése árnyékolása beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/), egy uberjar (más néven egy fatjar), mely tartalmazza az alkalmazáshoz szükséges függőségek létrehozásához használt. Is megakadályozza, hogy a párhuzamos licencek belül a üveg csomag, amely bizonyos rendszereken problémákat okozhatnak.

    A második beépülő modul konfigurálja a maven tesztelése fordító, a HDInsight fürt használt verzió alkalmazás által igényelt Java változatának beállítása használt.

3. Mentse a __pom.xml__ fájlt.

##<a name="create-the-mapreduce-application"></a>Az MapReduce-alkalmazás létrehozása

1. Nyissa meg a __wordcountjava/src/fő/java/szervezeti/apache/hadoop/példák__ könyvtár, és nevezze át a __App.java__ fájlt __WordCount.java__.

2. Nyissa meg a __WordCount.java__ fájlt egy szövegszerkesztőben, és a tartalom lecserélése a következőre:

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

    A csomag nevére **org.apache.hadoop.examples** pedig az osztálynév **WordCount**látható. Ezeket a neveket a MapReduce feladat elküldésekor fogja használni.

3. Mentse a fájlt.

##<a name="build-the-application"></a>Az alkalmazás összeállítása

1. Váltson az __wordcountjava__ , ha nem már van.

2. A következő parancsot használja az alkalmazás tartalmazó üveg fájl létrehozásához:

        mvn clean package

    Ez tiszta bármely előző build eltéréseket, függőségek, nem már telepítve van, és összeállítása és az alkalmazás csomag letöltése.

3. Miután befejezte a parancs, a __wordcountjava/célhely__ könyvtár __wordcountjava-1.0-SNAPSHOT.jar__nevű fájlt tartalmazza.

    > [AZURE.NOTE] __wordcountjava-1.0-SNAPSHOT.jar__ fájl egy uberjar, nem csak a WordCount feladat, hanem a feladat igénylő futásidőben függőségek tartalmazó.


##<a id="upload"></a>Töltse fel a üveg

A következő parancsot használja a üveg fájl feltöltése a HDInsight headnode:

    scp wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Ez másolja át a fájlokat a helyi rendszer központi csomópontot.

> [AZURE.NOTE] Ha korábban beállított jelszót biztonságossá tétele SSH-fiókja, a rendszer kéri, a jelszót. Ha korábban egy SSH kulcsot, előfordulhat, hogy akkor alkalmazza a `-i` paraméterre és a titkos kulcs elérési útja. Ha például `scp -i /path/to/private/key wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

##<a name="run"></a>A MapReduce feladat futtatása

1. Csatlakozás HDInsight SSH használ, az alábbi cikkek útmutatását:

    - [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

2. A SSH munkamenetből használja a következő parancsot a MapReduce alkalmazásnak a futtatására:

        yarn jar wordcountjava.jar org.apache.hadoop.examples.WordCount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/wordcountout

    Ezzel a WordCount MapReduce alkalmazás használata a davinci.txt fájlban szavainak megszámlálására, és az eredmények való tárolására __wasbs: / / / Példa/adatok/wordcountout__. A bemeneti fájl- és a kimeneti az alapértelmezett tárolóhoz a fürt tárolja.

3. Ha befejeződött a feladatot, használja az alábbi az eredmények megtekintéséhez:

        hdfs dfs -cat wasbs:///example/data/wordcountout/*

    Szavak és megszámolja, az alábbihoz hasonló értékekkel kell megjelennie:

        zeal    1
        zelus   1
        zenith  2

##<a id="nextsteps"></a>Következő lépések

A dokumentum egy Java MapReduce feladat fejlesztésével van megtanulta. Lásd: a következő dokumentumokat más módszerek a hdinsight szolgáltatásból lehetőségre.

- [HDInsight struktúra használata][hdinsight-use-hive]
- [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]
- [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

További tudnivalókért lásd még: a [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

