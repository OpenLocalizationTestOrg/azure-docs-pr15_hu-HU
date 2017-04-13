<properties
    pageTitle="A HDInsight Hadoop Twitter adatok elemzése |} Microsoft Azure"
    description="Megtudhatja, hogy miként struktúra használja a Hadoop HDInsight egy adott szót használatát gyakoriságának keresése a Twitteren adatok elemzése céljából."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Adatelemzés Twitter segítségével HDInsight struktúra

Közösségi webhely olyan egyik fő vezetői nagy adatok elfogadásához. Nyilvános API-k, például a Twitteren webhelyek által megadott adatok elemzése és a népszerű trendek ismertetése hasznos forrás. Ebben az oktatóanyagban használni twitterre beszerzése a Twitteren, a folyamatos átvitelű API segítségével, és futtasson Apache struktúra Azure hdinsight szolgáltatáshoz a Twitteren a felhasználók, akik a legtöbb twitterre egy adott szót tartalmazó küldött listájának.

> [AZURE.NOTE] A dokumentumban a lépéseket a Windows-alapú HDInsight fürtre szükség. Adott Linux-alapú fürtre című témakör tartalmazza [elemzése Twitter adatok HDInsight (Linux) struktúra](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Munkaállomás** Azure PowerShell, telepítette és beállította. 

    A Windows PowerShell-parancsfájlokat végrehajtás, Azure PowerShell Futtatás rendszergazdaként, és állítsa az adatvégrehajtás házirendet *RemoteSigned*. Lásd: [futtassa a Windows PowerShell-parancsfájlokat][powershell-script].

    A Windows PowerShell-parancsfájlokat futtatásához győződjön meg arról, hogy az alábbi parancsmag használatával csatlakozik az Azure-előfizetésébe:

        Login-AzureRmAccount

    Ha több Azure előfizetéssel rendelkezik, használja a következő parancsmagot az aktuális előfizetés beállítása:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Az Azure hdinsight szolgáltatáshoz fürt**. Fürt kiépítési, tanulmányozza [használatba HDInsight] [ hdinsight-get-started] vagy [rendelkezést HDInsight fürt] [hdinsight-provision]. Az oktatóprogram a később szüksége lesz a csoport nevét.

Az alábbi táblázat az ebben az oktatóanyagban használt fájlok:

Fájlok|Leírás
---|---
/tutorials/Twitter/Data/tweets.txt|A forrásadatok a struktúra projektre vonatkozóan.
/tutorials/Twitter/output|A kimenet mappát a struktúra projektre vonatkozóan. Az alapértelmezett struktúra feladat kimeneti fájl neve **000000_0**.
tutorials/Twitter/Twitter.hql|A HiveQL parancsfájl.
/tutorials/Twitter/JobStatus|A Hadoop feladat állapotát.


##<a name="get-twitter-feed"></a>Get-Twitter-hírcsatorna

Ebben az oktatóanyagban használandó [adatfolyam API-khoz Twitter][twitter-streaming-api]. Az adott Twitter használni kívánt API streaming [állapotok vagy szűrés][twitter-statuses-filter].

>[AZURE.NOTE] Egy nyilvános Blob-tárolóban lévő 10 000 twitterre tartalmazó fájlt, és a struktúra parancsprogram (a következő szakaszban szereplő) van feltöltve. Ez a szakasz kihagyhatja, ha azt szeretné használni a feltöltött fájlokat.

A JavaScript objektum jelölés (JSON) formátumot, amely tartalmazza a beágyazott összetett struktúrában [twitterre adatok](https://dev.twitter.com/docs/platform-objects/tweets) vannak tárolva. Sok kódsorokat ír, a hagyományos programnyelv, hanem alakíthatja a beágyazott struktúra struktúra táblába, hogy azt egy Structured Query Language (SQL) lekérdezhető-hasonló nevű HiveQL nyelvet.

Twitter OAuth használja a meghatalmazott hozzáférést biztosít az API-hoz. OAuth, amely lehetővé teszi a felhasználóknak, hogy az alkalmazások nélkül jelszavukat megosztása más nevében eljáró jóváhagyása hitelesítési protokoll. További információk találhatók [oauth.net](http://oauth.net/) vagy a kiváló [OAuth kezdőknek](http://hueniverse.com/oauth/) a Hueniverse.

Az első OAuth használandó lépésként új alkalmazás létrehozása a fejlesztői Twitter webhelyen.

**A Twitter-alkalmazás létrehozása**

1. Jelentkezzen be a [https://apps.twitter.com/](https://apps.twitter.com/). Ha nincs Twitter-fiókja, kattintson a **Regisztráció most** hivatkozásra.
2. Kattintson az **Új alkalmazás létrehozása**gombra.
3. Írja be a **név**, **Leírás**, **webhelyet**. Egy URL-CÍMÉT a **webhely** mező be teheti. A következő táblázat mutatja egyes mintaértékekre használni:

    A mező|Érték
    ---|---
    név|MyHDInsightApp
    Leírás|MyHDInsightApp
    Webhely|http://www.myhdinsightapp.com

4. Jelölje be az **Igen, elfogadom**, és kattintson a **Create a Twitteren alkalmazást**.
5. Kattintson az **engedélyek** fülre. Az alapértelmezett engedély **csak olvasható**. Ez az oktatóanyag elegendő.
6. Kattintson a **kulcsok és a hozzáférési jogkivonat** fülre.
7. Kattintson **a hozzáférési jogkivonat létrehozása**hivatkozásra.
8. A lap jobb felső sarkában kattintson a **Vizsgálat OAuth** .
9. Jegyezze fel a **fogyasztói billentyűt**, a **fogyasztói titkos**, a **hozzáférési jogkivonat**és a **hozzáférési jogkivonat titkos**. Az oktatóprogram a később szüksége lesz az értékeket.

Ebben az oktatóanyagban, hogy a webszolgáltatás hívása a Windows PowerShell fogja használni. .NET C# minta című [elemzés valós idejű Twitter sentiment együtt a HDInsight HBase][hdinsight-hbase-twitter-sentiment]. A más népszerű web service hívásokhoz eszköze [*Curl*][curl]. Curl letölthető [Itt][curl-download].

>[AZURE.NOTE] A curl paranccsal a Windows rendszerben, használatakor dupla idézőjelek aposztrófok helyett az értékek lehetőséget.

**Az első twitterre**

1. Nyissa meg a Windows PowerShell integrált parancsfájl-környezet (ISE). (A Windows 8 kezdőképernyőjén, írja be a **PowerShell_ISE** , és válassza a **Windows PowerShell ISE**. Lásd: a [Windows PowerShell a Windows 8 és a Windows Start][powershell-start].)

2. A parancsprogram-ablakban másolja be a következőt:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Az első öt-nyolc változók meg a parancsfájlt:


    Változó|Leírás
    ---|---
    $clusterName|Ez a nevét a HDInsight fürt, amelyhez szeretne az alkalmazásnak a futtatására.
    $oauth_consumer_key|Ez az írott lefelé korábbi a Twitter-alkalmazás létrehozásakor Twitter alkalmazás **fogyasztói billentyűt** .
    $oauth_consumer_secret|Ez egy, a Twitteren alkalmazás **fogyasztói titkos** korábbi írta le.
    $oauth_token|Ez egy, a Twitteren alkalmazás **hozzáférési jogkivonat** korábbi írta le.
    $oauth_token_secret|Ez egy, a Twitteren alkalmazás **hozzáférési jogkivonat titkos** korábbi írta le.
    $destBlobName|Ez a kimeneti blob nevét. Az alapértelmezett érték **tutorials/twitter/data/tweets.txt**. Az alapértelmezett értéket módosíthatja, ha szüksége lesz a Windows PowerShell-parancsfájlokat árnyalatait.
    $trackString|A webszolgáltatás kulcsszavakkal kapcsolódó twitterre ad vissza. Az alapértelmezett érték **Azure, a felhőben, a hdinsight szolgáltatásból lehetőségre**. Ha módosítja az alapértelmezett érték, akkor frissíti a Windows PowerShell-parancsfájlokat.
    $lineMax|Az érték azt határozza meg, hány twitterre felirat jelenik meg a parancsfájlt. 100 twitterre olvasható körülbelül három percet vesz igénybe. Beállíthatja, hogy nagyobb számot, de több időt letöltése tart.

5. Nyomja le az **F5** futtatása. Ha problémákat tapasztal, megoldásként minden sor kijelölése, és nyomja le az **F8**.
6. Megjelenik a "Kész!" kell a kimenet végén. Hibaüzenetek jelennek meg piros.

Egy adatérvényesítési eljárást: érdemes az Azure Blob-tárolóhoz Azure tároló Intéző vagy Azure PowerShell használatával **/tutorials/twitter/data/tweets.txt**, a kimeneti fájl is. A Windows PowerShell mintaparancsfájl fájlok azoknak, témakörben [használata blobtárolóhoz HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Hozzon létre HiveQL parancsfájl

Azure PowerShell használata esetén futtassa a több HiveQL kimutatások egy egyszerre, vagy a HiveQL utasítás parancsprogram-fájlba csomag. Ebben az oktatóanyagban HiveQL parancsfájl hoz létre. A parancsprogram kell tölthető Azure Blob-tárolóhoz. A következő szakaszban a parancsprogram futtatható Azure PowerShell használatával.

>[AZURE.NOTE] A struktúra parancsfájl és egy 10 000 twitterre tartalmazó fájlt egy nyilvános Blob-tárolóban van feltöltve. Ez a szakasz kihagyhatja, ha azt szeretné használni a feltöltött fájlokat.

A HiveQL parancsfájl hajtsa végre a következő lesz:

1. **Húzza a tweets_raw tábla** jelenik meg a táblázat már létezik.
2. **A tweets_raw struktúratáblával létrehozása**. A strukturált táblázat adatait tartalmazza további ideiglenes struktúra kibontása, átalakítás és betöltése (ETL) feldolgozása. Partíciót a további tudnivalókért lásd [a struktúra oktatóprogram][apache-hive-tutorial].  
3. **Adatok betöltése** a forrás mappából /tutorials/twitter/data. A nagyméretű twitterre adatkészlet beágyazott JSON formátumban be egy ideiglenes struktúra táblázatszerkezet most már átalakítva.
3. **Húzza a twitterre tábla** jelenik meg a táblázat már létezik.
4. **A twitterre tábla létrehozása**. Mielőtt struktúra használatával lekérdezheti a twitterre adatkészlet ellen, egy másik ETL folyamat futtatásához szükséges. Ez a folyamat ETL a "twitter_raw" táblában tárolt adatokhoz részletesebb táblázat séma határozza meg.  
5. A **Felülírás táblázat beszúrása**. A összetett struktúrában parancsfájl fog egy sor olyan hosszú MapReduce feladatok elindításához a Hadoop fürt. Ez az adatkészlet és a fürt méretétől függően eltarthat KB.
6. **Beszúrás felülírja a címtár**-e. Lekérdezések futtatása és a kimeneti fájl az adatkészlet. Ezt a lekérdezést ad vissza, amely tartalmazza a word a legtöbb twitterre küldő Twitter felhasználók listájának "Azure".

**Hozzon létre egy struktúra parancsfájlt, és töltse fel Azure**

1. Nyissa meg a Windows PowerShell ISE.
2. A parancsprogram-ablakban másolja be a következőt:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Az első két változót meg a parancsfájlt:

    Változó|Leírás
    ---|---
    $clusterName|Írja be a HDInsight fürt nevét, amelyhez szeretne az alkalmazásnak a futtatására.
    $subscriptionID|Írja be az Azure előfizetés azonosítójával.
    $sourceDataPath|Azure Blob tárolási helye a struktúra lekérdezések hol olvassa el az adatokat. Nem kell a változó módosítása.
    $outputPath|Az Azure Blob tárolási helye, ahol a struktúra lekérdezések fog kimeneti az eredmények. Nem kell a változó módosítása.
    $hqlScriptFile|A helyet, és a fájlnevet, a HiveQL parancsfájl. Nem kell a változó módosítása.

5. Nyomja le az **F5** futtatása. Ha problémákat tapasztal, megoldásként minden sor kijelölése, és nyomja le az **F8**.
6. Megjelenik a "Kész!" kell a kimenet végén. Hibaüzenetek jelennek meg piros.

Egy adatérvényesítési eljárást: érdemes az Azure Blob-tárolóhoz Azure tároló Intéző vagy Azure PowerShell használatával **/tutorials/twitter/twitter.hql**, a kimeneti fájl is. A Windows PowerShell mintaparancsfájl fájlok azoknak, témakörben [használata blobtárolóhoz HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Twitter adatfeldolgozás struktúra használatával

Befejezte a előkészítése munkáját. Ezután a struktúra parancsfájl meghívása, ellenőrizze az eredményt.

### <a name="submit-a-hive-job"></a>Struktúra feladat elküldése

Az alábbi Windows PowerShell-parancsprogramot segítségével futtassa a struktúra. Szüksége lesz az első-változó beállítása.

>[AZURE.NOTE] Az utolsó két szakaszok használni a twitterre és a feltöltött HiveQL parancsfájl, állítsa $hqlScriptFile "/ tutorials/twitter/twitter.hql". A lehetőségekből, hogy egy nyilvános blob-feltöltött használni, állítsa $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Ellenőrizze az eredményt

Az alábbi Windows PowerShell-parancsprogramot használni a struktúra feladat eredménye. Szüksége lesz az első két változót beállítása.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] A struktúratáblával \001 használja, mint a mező elválasztó. Az elválasztó nem láthatók, az eredményben.

Az elemzés eredménye Azure Blob-tárolóhoz kerültek, miután exportálja az adatokat egy Azure SQL-adatbázis vagy SQL server, az adatok exportálása az Excelbe a Power Query használatával vagy az adatokat az alkalmazást a struktúra ODBC-illesztőprogram használatával csatlakozni. További tudnivalókért lásd: [A HDInsight használata Sqoop][hdinsight-use-sqoop], [elemzés nézetbeli késési adataival HDInsight használatával][hdinsight-analyze-flight-delay-data], [Hdinsight szolgáltatáshoz a Power Query az Excel csatlakozni][hdinsight-power-query], és az [Excel programban csatlakoztatása a Microsoft struktúra ODBC-illesztőprogram HDInsight][hdinsight-hive-odbc].

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban azt egy strukturálatlan JSON adatkészlet alakíthat ki egy lekérdezés, adatainak vizsgálata és elemzése a Twitteren Azure hdinsight szolgáltatáshoz alkalmazással történő strukturált struktúratáblával hogyan van láthatók. További tudnivalókért lásd:

- [Első lépések a hdinsight szolgáltatáshoz][hdinsight-get-started]
- [A HDInsight HBase a valós idejű Twitter sentiment elemzése][hdinsight-hbase-twitter-sentiment]
- [Adatelemzés nézetbeli késleltetés segítségével hdinsight szolgáltatáshoz][hdinsight-analyze-flight-delay-data]
- [Az Excel csatlakozni hdinsight szolgáltatáshoz a Power Query használatával][hdinsight-power-query]
- [Az Excel csatlakozni a Microsoft-struktúra ODBC-illesztőprogram hdinsight szolgáltatáshoz][hdinsight-hive-odbc]
- [HDInsight Sqoop használata][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
