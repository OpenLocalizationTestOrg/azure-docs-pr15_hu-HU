<properties
 pageTitle="Maven tesztelése forrázást MapReduce feladatok kidolgozása |} Microsoft Azure"
 description="Megtudhatja, hogy miként maven tesztelése használatával hozzon létre egy forrázást MapReduce projektet, majd üzembe helyezéséhez és futtathatja a feladatot a Hadoop HDInsight fürt."
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
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>A HDInsight Apache Hadoop forrázást MapReduce feladatok kidolgozása

Forrázást, amely megkönnyíti a feladatok létrehozása a Hadoop MapReduce Scala tárban. Egy tömör szintaxisát, valamint a Scala szoros integrációja kínál.

A dokumentumban megtudhatja, hogy miként maven tesztelése használatával hozzon létre egy egyszerű word darab MapReduce projektet Scalding nyelven íródott. Majd megtanulhatja, hogy miként telepíthető, és a feladat futtatása a egy HDInsight fürthöz.

## <a name="prerequisites"></a>Előfeltételek

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **A Windows vagy Linux rendszerhez alapú Hadoop fürt hdinsight szolgáltatásból lehetőségre**. További információt talál [rendelkezést Linux-alapú Hadoop a HDInsight](hdinsight-hadoop-provision-linux-clusters.md) vagy [rendelkezést Windows-alapú Hadoop a hdinsight szolgáltatásból lehetőségre](hdinsight-provision-clusters.md) .

* **[Maven tesztelése](http://maven.apache.org/)**

* **[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 vagy újabb verzió**

## <a name="create-and-build-the-project"></a>Hozzon létre, és hozza létre a projekt

1. A következő paranccsal maven tesztelése új projekt létrehozása:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Ez a parancs **scaldingwordcount**nevű új könyvtár létrehozása, és a állványon Scala alkalmazás létrehozása.

2. A **scaldingwordcount** címtárban nyissa meg a **pom.xml** fájlt, és a tartalom lecserélése a következőre:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Ez a fájl a projekt, függőségek és bővítmények ismerteti. Az alábbiakban fontos elemeit:

    * **maven.Compiler.Source** és **maven.compiler.target**: Beállítja a Java verzió ehhez a projekthez

    * **tárházakban**: a projekt által használt függőség fájlokat tartalmazó tárházakban

    * **forrázást core_2.11** és **hadoop-core**: ehhez a projekthez Scalding és a Hadoop core csomagok függ.

    * **maven tesztelése-scala-bővítmény**: beépülő modul scala alkalmazások összeállításához

    * **maven tesztelése-árnyékolása-bővítmény**: beépülő modul létrehozása (fat) kancsó szürke. A beépülő modul vonatkozik, szűrők és átalakítások; specificially:

        * **szűrők**: alkalmazott szűrők módosítsa a üveg fájlban részét képező metaadatok adatokat. Megakadályozására aláíráshoz kivételek futás során, ez kizárja különböző aláírás fájlok, amelyeket érinthetnek megtalálható a függőségeket.

        * **végrehajtások**: A csomag fázis végrehajtás konfigurálása adja meg a **com.twitter.scalding.Tool** osztály a csomag fő osztály. Nélkül, kellene com.twitter.scalding.Tool, valamint a munka futtatásakor a hadoop parancs az alkalmazás logika tartalmazó osztály megadásához.

3. A **forrás/próba** könyvtár, nem létrehozandó vizsgálatok ebben a példában a törlése

4. Nyissa meg a **src/main/scala/com/microsoft/example/App.scala** fájlt, és a tartalom lecserélése a következőre:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Egy egyszerű word darab feladat végrehajtja a.

5. Mentse és zárja be a fájlokat.

6. A következő parancsot a **scaldingwordcount** címtárból segítségével továbbépítése és az alkalmazás csomag:

        mvn package

    A feladat befejeződése után az WordCount alkalmazás tartalmazó csomag **target/scaldingwordcount-1.0-SNAPSHOT.jar**találhatók.

## <a name="run-the-job-on-a-linux-based-cluster"></a>A feladat futtatása a Linux-alapú fürthöz

> [AZURE.NOTE] Az alábbi lépésekkel SSH és a Hadoop parancsot használja. Más módszereket MapReduce feladatok fut [A Hadoop a HDInsight használata MapReduce](hdinsight-use-mapreduce.md)című témakör tartalmaz.

1. A következő parancsot használja a HDInsight fürt a csomag feltöltése:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Ez másolja át a fájlokat a helyi rendszer központi csomópontot.

    > [AZURE.NOTE] Ha korábban beállított jelszót biztonságossá tétele SSH-fiókja, a rendszer kéri, a jelszót. Ha korábban egy SSH kulcsot, előfordulhat, hogy akkor alkalmazza a `-i` paraméterre és a titkos kulcs elérési útja. Ha például`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. A következő parancsot használja a központi csomópont csatlakozhat:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ha korábban beállított jelszót biztonságossá tétele SSH-fiókja, a rendszer kéri, a jelszót. Ha korábban egy SSH kulcsot, előfordulhat, hogy akkor alkalmazza a `-i` paraméterre és a titkos kulcs elérési útja. Ha például`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Miután létrejött a központi csomópontra, használja a következő parancsot a word darab feladat futtatása

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    A program elvégzi a WordCount osztály korábbi szerepelni fog. `--hdfs`a feladat Fájlrendszerhez használatára utasítja. `--input`Adja meg a bemeneti szövegfájl, miközben `--output` kimenet helyét adja meg.

4. A feladat befejeződése után a következő használja a kimenet megtekintéséhez.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Ekkor megjelenik a következőhöz hasonló adatokat:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>A feladat futtatása a Windows-alapú fürthöz

Az alábbi lépéseket a Windows PowerShell-lel. Más módszereket MapReduce feladatok fut [A Hadoop a HDInsight használata MapReduce](hdinsight-use-mapreduce.md)című témakör tartalmaz.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Indítsa el a Azure PowerShell és bejelentkezés az Azure-fiókjába. Adja meg a hitelesítő adatokat, miután a parancsot a fiókkal kapcsolatos információk adja eredményül.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Ha több előfizetéssel rendelkezik, adja meg a telepítéshez használni kívánt előfizetés azonosítója.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Használható `Get-AzureRMSubscription` egy listát, amely tartalmazza az előfizetési azonosító mindegyik a fiókkal társított minden előfizetés.

4. használja az alábbi parancsprogramot tölthet fel, és futtassa a WordCount feladatot. Csere `CLUSTERNAME` a HDInsight nevű fürt, és győződjön meg arról, hogy `$fileToUpload` a helyes-e a fájl elérési útját __scaldingwordcount-1.0-SNAPSHOT.jar__ van.

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Futtatja a parancsfájlt, amikor a rendszer kéri a HDInsight fürt adja meg a rendszergazdai felhasználónevével és jelszavával. A feladat futtatása közben fellépő hibák a konzol fog kell bejelentkeznie.
     
6. Ha befejeződött a feladatot, a kimeneti fájl __kimenet.txt__ az aktuális mappában letölt. A következő parancsot használja az eredmények megjelenítéséhez.

        cat output.txt

    A fájlnak kell tartalmaznia az értékeket az alábbiakhoz hasonló:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Következő lépések

Most, hogy hogyan Scalding segítségével MapReduce feladatok létrehozása a HDInsight rendelkezik megtanulta, használja az alábbi hivatkozások feltárása más módszerek a Azure hdinsight szolgáltatáshoz.

* [HDInsight struktúra használata](hdinsight-use-hive.md)

* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)

* [HDInsight MapReduce feladatok használata](hdinsight-use-mapreduce.md)
