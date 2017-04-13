<properties
    pageTitle="Javaslatok Mahout és a WIndows-alapú HDInsight készítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként készítése a Windows-alapú HDInsight (Hadoop) a videóban javaslatok a Apache Mahout gépi tanulási tár használatával."
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
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Mozgókép javaslatok készítése a HDInsight Hadoop-Apache Mahout használatával

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Megtudhatja, hogy miként film javaslatok készítése a [Apache Mahout](http://mahout.apache.org) gépi tanulási Azure hdinsight szolgáltatáshoz tárban használatával.

> [AZURE.NOTE] Ehhez a dokumentumhoz a lépések a Windows-ügyfél és a Windows-alapú HDInsight fürtre. Linux-alapú HDInsight fürt való használatáról a Mahout Linux, OS X vagy Unix ügyfélprogram, további tudnivalókért lásd [Generate film javaslatok a Linux-alapú Hadoop a HDInsight Apache Mahout használatával](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Mi ismerkedhet meg

Mahout egy [gépi tanulási] [ ml] Apache Hadoop-tárat. Mahout algoritmusok adatfeldolgozás fürtképzés és szűrés, osztályozási, például a tartalmazza. Ebben a cikkben egy ajánlási motor használandó film javaslatok ismerősei egyszer látott filmek alapuló létrehozásához. Is megismerheti, hogyan végezhetők el egy döntés erdővel besorolása elemet. Ez lesz lapon, a következőket:

* Hogyan Mahout feladatok futtatható a Windows PowerShell használatával

* Hogyan kell a Hadoop parancssori Mahout feladatokat lebonyolítása

* A HDInsight 3.0-s és HDInsight 2.0-s fürt Mahout telepítéséről

    > [AZURE.NOTE] A fürt HDInsight 3.1-es verziójával mahout megadva. Ha korábbi HDInsight-esetén, olvassa el a [Mahout telepítése](#install) előtt, hogy továbbra is.

##<a name="prerequisites"></a>Előfeltételek

- **Egy Windows-alapú Hadoop-fürt hdinsight szolgáltatásból lehetőségre**. Egy létrehozásával kapcsolatos további tudnivalókért lásd [a HDInsight Hadoop használatának első lépései][getstarted]
- **Azure PowerShell munkaállomás**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Javaslatok készítése a Windows PowerShell használatával

> [AZURE.NOTE] Bár a feladat az ebben a szakaszban a Windows PowerShell használatával működik, az osztályok Mahout mellékelt számos jelenleg nem működik a Windows PowerShell és Hadoop parancssorból kell futtatni. Az osztályok, amelyek nem működnek a Windows PowerShell listájában című [Hibaelhárítás](#troubleshooting) .
>
> Példa a Hadoop parancssorból Mahout feladat futtatása olvassa el a [besorolás adatok kezelése a Hadoop parancssorból](#classify)című témakört.

A Mahout által biztosított függvények egyike egy ajánlási motor. Ez a motor fogadja el az adatok formátuma `userID`, `itemId`, és `prefValue` (a felhasználók igényei az elem). Mahout majd végezze el a közös elemként analysis meghatározása: _felhasználók, akik elem előnyben is a beállításait, ezeket az elemeket a_. Mahout majd határozza meg, hogy a hasonló többelemes beállítások, felhasználható ajánlások rendelkező felhasználók.

Az alábbi képen rendkívül egyszerű filmek használó:

* __Dokumentumok közös elemként__: Mintaügyvezető Anna és Péter összes tetszett _csillag ütközések_ _Vissza sztrájkok a Empire_és _a Jedi a vissza_. Mahout határozza meg, hogy a felhasználók, akik e filmek közül bármelyik is, mint például a másik kettő.

* __Dokumentumok közös elemként__: Péter és Anna a _A látszólagosra támadása_, _a klónok a homonimaszerű webcímmel_és _megtorlás, a Sith_tetszett is. Mahout határozza meg, hogy a felhasználók, akik szintén tetszett az előző három filmek például három.

* __Hasonló javaslat__: mivel Mintaügyvezető tetszett az első három filmek, Mahout vizsgálja meg filmek, hogy mások tetszett hasonló beállítások, de Mintaügyvezető nem rendelkezik vizsgált (tetszett/névleges). Ebben az esetben Mahout _A látszólagosra támadása_, _a klónok a homonimaszerű webcímmel_és _megtorlás, a Sith_javasolja.

###<a name="understanding-the-data"></a>Az adatok ismertetése

Kényelmesen, [GroupLens kutatási] [ movielens] értékelése az adatokat nyújt a filmek Mahout kompatibilis formátumban. Az adatok érhető el a fürt alapértelmezett tárolás `/HdiSamples/MahoutMovieData`.

Két fájlok, vannak `moviedb.txt` (a mozgóképet, információt) és `user-ratings.txt`. A felhasználó-ratings.txt fájl használják-elemzés során, bár moviedb.txt felhasználóbarát szöveg követően megadni az elemzés eredményének megjelenítése.

Felhasználó-ratings.txt lévő adatok szerkezete `userID`, `movieID`, `userRating`, és `timestamp`, amely közli velünk hogyan erősen minden felhasználó minősítette a film. Íme egy példa az adatok:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>A feladat futtatása

Az alábbi Windows PowerShell-parancsprogramot használva a film-adatok a Mahout ajánlási motort használó feladat futtatása:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
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
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Mahout feladatok nem távolítja el a feladat feldolgozása közben létrehozott ideiglenes adatait. A `--tempDir` elkülönítése az ideiglenes fájlok egy adott könyvtárban példa feladatban paramétert.

A Mahout feladat nem ad vissza a kimenet STDOUT. Ehelyett, tárolja azt a megadott kimeneti címtárban __rész-r-00000__. A parancsprogram __kimenet.txt__ a számítógépen az aktuális mappában letölti ezt a fájlt.

Az alábbi képen a tartalom, a fájl:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Az első oszlop a `userID`. Szereplő értékeket ' ["és"] "vannak `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>A kimenet megtekintése

Annak ellenére, a létrehozott kimenet előfordulhat, hogy az OK gombra az alkalmazásokban használatra, még nem nagyon olvasható. A `moviedb.txt` a kiszolgálóról használható megoldani a `movieId` film a nevét, de először le kell töltenie és a minősítések fájlt a kiszolgálóról, az alábbi parancsfájl használatával:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
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
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Miután letöltötte a fájlokat, használja az alábbi PowerShell parancsprogramot film nevek javaslatok megjelenítéséhez:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Az alábbi képen a parancsprogram futtatása:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

A kimenet jelenjen meg az alábbihoz hasonló:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Sorolják be adatokat a Hadoop parancssor használatával

Az osztályozási módszerek Mahout elérhető egyik [véletlen erdő]összeállítása[forest]. Ez a több lépésben, amely magában foglalja a oktatóanyag adatok használata döntési fák, majd használt osztályozásához adatok létrehozásához. Ez az a __org.apache.mahout.classifier.df.tools.Describe__ osztály Mahout által biztosított használ. Ez jelenleg kell futtatni a Hadoop parancssor használatával.

###<a name="load-the-data"></a>Adatok betöltése

1. A következő fájlok letöltése [a NSL-KDD adatkészletet](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): a oktatás fájl

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): a tesztadatokat

2. Nyissa meg a minden fájl, és távolítsa el a felső részén található karakterekkel kezdődő sorokat '@', és mentse a fájlt. Ha ezek nem törlődnek, kap hibaüzenetek jelennek meg, ezeket az adatokat tartalmazó Mahout használatakor.

2. A fájlok feltöltése a __Mintaadatok /__. Ehhez az alábbi parancsfájl használatával. __CLUSTERNAME__ cserélje le a HDInsight fürt nevére. FÁJLNÉV cserélje ki a nevet fő a fájlt, fel kell tölteni.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
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

###<a name="run-the-job"></a>A feladat futtatása

1. A feladat szükséges a Hadoop parancsot. A HDInsight fürt engedélyezése a távoli asztali, majd a [Csatlakozás RDP segítségével HDInsight fürt](hdinsight-administer-use-management-portal.md#rdp)utasításait követve csatlakozni.

3. Csatlakozás után használja a __Hadoop parancssor__ ikonra kattintva nyissa meg a Hadoop parancsot:

    ![hadoop cli][hadoopcli]

3. A következő parancsot használja a fájl leíró (__KDDTrain + .info__), amelyet használ Mahout létrehozásához.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    A `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` a fájlban lévő adatokat tulajdonságainak ismerteti. Ha például L azt jelzi, címke.

4. Döntési fák erdő felépítése a következő parancs használatával:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Ez a művelet eredményét tárolja a tárterület a HDInsight fürthöz __ wasbs://user/ található __nsl erdőn__ címtárban&lt;felhasználónév > / nsl-erdő/nsl-forest.seq. A &lt;felhasználónév > van a felhasználó nevét a távoli asztali munkamenetben használt. Ez a fájl nem is olvashatják az emberek.

5. Tesztelje a erdő az __KDDTest + .arff__ adatkészlet osztályozásához. A következő parancsot használja:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    Ez a parancs adja eredményül a minősítési folyamat vonatkozó összegző információk az alábbihoz hasonló:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  A feladat is létrehoz egy fájlt, __wasbs:///example/data/predictions/KDDTest+.arff.out__helyen található. Ez a fájl viszont nem is olvashatják az emberek.

> [AZURE.NOTE] Mahout feladatok nem írja felül a fájlokat. Ha szeretne futtassa újra az alábbi feladatok, törölnie kell a korábbi feladatok által létrehozott fájlok.

##<a name="troubleshooting"></a>Hibaelhárítás

###<a name="install"></a>Mahout telepítése

Mahout HDInsight 3.1 fürt van telepítve, és hogy telepíthető manuálisan HDInsight 3.0-s vagy HDInsight 2.1 fürt az alábbi lépésekkel:

1. A használandó Mahout verziója attól függ, hogy a fürt HDInsight verzióját. A fürt verzió megtalálhatja a fürt tulajdonságainak megtekintése az Azure-portálon.

  * __HDInsight 2.1__, ingyenesen letöltheti a [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar)tartalmazó Java archív (üveg) fájlok.

  * __HDInsight 3.0__, kell [a forrásból származó Mahout összeállítása] [ build] , és adja meg a HDInsight által biztosított Hadoop-verziója. Telepítse a Előfeltételek, megjelenik a Szerkesztés lapon, és töltse le a forrás majd használja a következő parancsot a Mahout üveg fájlok létrehozásához:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Mahout 1.0 megjelenésekor használhatja a beépített csomagok HDInsight 3.0-s verziójában kell lennie.

2. __Példa/kancsó__ az alapértelmezett tárolója a fürt töltse fel a üveg fájlt. A következő parancsfájl CLUSTERNAME helyettesítése a HDInsight fürt neve, és FILENAME cserélje ki a fájl elérési útját __mahout-coure-0,9-job.jar__ .

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
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
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Fájlok nem írható felül.

Mahout feladatok do nem karbantartása az ideiglenes fájlok létrehozott feldolgozása közben. Ezenkívül a feladatok nem írja felül meglévő kimeneti fájl.

Hibák elkerülése érdekében Mahout feladatok fut, milyen gyakran fusson ideiglenes és a kimeneti fájlok törlése vagy egyedi ideiglenes és a kimeneti címtár neveket használják.

###<a name="cannot-find-the-jar-file"></a>Nem találja a üveg fájlt

HDInsight 3.1 fürt Mahout tartalmazzák. Az elérési utat és a fájl nevét tartalmazza, hogy telepítve van a fürt Mahout verziószáma. A Windows PowerShell mintaparancsfájl ebben az oktatóanyagban használja az elérési útjuk érvényes November 2015 kezdve, de a verziószám változik későbbi frissítések hdinsight szolgáltatásból lehetőségre. Annak megállapításához, az aktuális fájl elérési útját Mahout JAR a fürt, az alábbi Windows PowerShell-parancsot, és módosítsa a fájl elérési útját, a függvény által visszaadott hivatkozni szeretne a parancsfájl:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Osztályok, amelyek nem működnek a Windows PowerShell

A következő osztályok alkalmazó mahout feladatok hibaüzenetek számos vissza, a Windows PowerShell használatakor:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Ezek az osztályok alkalmazó feladatok futtatásához a HDInsight fürthöz csatlakozni, és a feladatok kezelése a Hadoop parancssorból futtasson. [A Hadoop parancssorból besorolás adatok](#classify) talál példát.

##<a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik megtanulta Mahout használatáról, felfedezése további módszerek az adatok HDInsight használata:

* [A HDInsight struktúra](hdinsight-use-hive.md)
* [Malac hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [A HDInsight MapReduce](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
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
 
