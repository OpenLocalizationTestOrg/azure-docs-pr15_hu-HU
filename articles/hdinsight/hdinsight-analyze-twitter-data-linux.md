<properties
    pageTitle="A HDInsight struktúra Apache Twitter adatok elemzése |} Microsoft Azure"
    description="Megtudhatja, hogy miként Python használva Twitterre, amely tartalmazza az adott kulcsszavak, majd struktúra, és a Hadoop a hdinsight szolgáltatásból lehetőségre kattintva alakíthat ki egy kereshető struktúratáblával, a Twitteren nyers adatokból."
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

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Adatelemzés Twitter segítségével HDInsight struktúra

A dokumentumban twitterre fog kapni a Twitteren, a folyamatos átvitelű API segítségével, és kattintson a folyamat a JSON Linux-alapú HDInsight fürtre Apache struktúra használata formázott adatok. Az eredmény is Twitter-felhasználók, akik a legtöbb twitterre egy adott szót tartalmazó küldött listáját.

> [AZURE.NOTE] Bár a dokumentum adott hardvereszközöket felhasználhatók a Windows-alapú HDInsight fürt (például Python), több lépésben alapuló Linux-alapú HDInsight fürt használatával. Adott a Windows-alapú fürtre című témakör tartalmazza [Twitter elemzése az adatoknak struktúra hdinsight szolgáltatásból lehetőségre](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- __Azure HDInsight Linux-alapú fürt__. Fürt létrehozásával kapcsolatos további tudnivalókért lásd a [Linux-alapú HDInsight – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md) fürt létrehozásának lépéseit.

- __SSH ügyfél__. További tájékoztatást a Linux-alapú HDInsight SSH használja az alábbi cikkekben talál:

    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ és [mezőpont](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>A hírcsatorna Twitter beszerzése

Twitter lehetővé teszi, hogy az [egyes tweetbe az adatok](https://dev.twitter.com/docs/platform-objects/tweets) beolvasásához a JavaScript objektum jelölés (JSON) dokumentumként keresztül REST API-t. [OAuth](http://oauth.net) szükség a hitelesítést az API-nak. A _Twitter alkalmazás_ hozzáférhet az API-beállításokat tartalmazó is létre kell hoznia.

###<a name="create-a-twitter-application"></a>Twitter-alkalmazás létrehozása

1. Egy webböngészőben jelentkezzen be az [https://apps.twitter.com/](https://apps.twitter.com/). Ha nincs Twitter-fiókja, kattintson a **Regisztráció most** hivatkozásra.
2. Kattintson az **Új alkalmazás létrehozása**gombra.
3. Írja be a **név**, **Leírás**, **webhelyet**. Egy URL-CÍMÉT a **webhely** mező be teheti. A következő táblázat mutatja egyes mintaértékekre használni:

  	| A mező | Érték |
  	|:----- |:----- |
  	| név  | MyHDInsightApp |
  	| Leírás | MyHDInsightApp |
  	| Webhely | http://www.myhdinsightapp.com |
    
4. Jelölje be az **Igen, elfogadom**, és kattintson a **Create a Twitteren alkalmazást**.
5. Kattintson az **engedélyek** fülre. Az alapértelmezett engedély **csak olvasható**. Ez az oktatóanyag elegendő.
6. Kattintson a **kulcsok és a hozzáférési jogkivonat** fülre.
7. Kattintson **a hozzáférési jogkivonat létrehozása**hivatkozásra.
8. A lap jobb felső sarkában kattintson a **Vizsgálat OAuth** .
9. Jegyezze fel a **fogyasztói billentyűt**, a **fogyasztói titkos**, a **hozzáférési jogkivonat**és a **hozzáférési jogkivonat titkos**. Az értékek később szüksége lesz.

>[AZURE.NOTE] A curl paranccsal a Windows rendszerben, használatakor dupla idézőjelek aposztrófok helyett az értékek lehetőséget.

###<a name="download-tweets"></a>Twitterre letöltése

A következő Python kódot 10 000 twitterre töltse le a Twitteren, és a __tweets.txt__nevű fájlt mentheti őket.

> [AZURE.NOTE] Az alábbi lépéseket a HDInsight fürt megtörténik, mivel már telepítve van a Python.

1. A HDInsight fürt SSH használatával csatlakozhat:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Ha a használt jelszót secure SSH fiókját, a rendszer kéri, adja meg. Nyilvános kulccsal használatakor előfordulhat, hogy akkor alkalmazza a `-i` paraméterrel adja meg a megfelelő titkos kulcs. Ha például `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    További tájékoztatást a Linux-alapú HDInsight SSH használja az alábbi cikkekben talál:
    
    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Alapértelmezés szerint a __mezőpont__ segédprogram nincs telepítve a HDInsight központi csomópontra. Használja a következő telepítése, és frissítse a segédprogram:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. A kódot, és töltse le a twitterre [Tweepy](http://www.tweepy.org/) és [Progressbar](https://pypi.python.org/pypi/progressbar/2.2)támaszkodik. Telepítse őket, használja az alábbi parancsot:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] A bittel python openssl, a fejlesztői python libffi-fejlesztők, libssl-fejlesztők, pyOpenSSL és az [biztonsági] kérések eltávolításával kapcsolatban elkerülése érdekében a InsecurePlatform figyelmeztetést Python a Twitteren SSL keresztül való csatlakozáskor.
    >
    > Tweepy v3.2.0 használják a [hiba](https://github.com/tweepy/tweepy/issues/576) akkor fordulhat elő, amikor twitterre feldolgozása elkerülése érdekében.

4. A következő paranccsal __gettweets.py__nevű új fájl létrehozása:

        nano gettweets.py

5. A következő használja a __gettweets.py__ fájl tartalmát. Cserélje le a helyőrző adatait __fogyasztói\_titkos__, __fogyasztói\_kulcs__, __access /\_jogkivonat__, és __access\_jogkivonat\_titkos__ az információkkal, a Twitteren levelezőprogramból.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Használja a __Ctrl + X billentyűkombinációt__, majd az __Y__ mentse a fájlt.

7. A következő parancs segítségével futtassa a fájlt, és töltse le a twitterre:

        python gettweets.py

    Egy folyamatjelző kell helyezni, és 100 %-át számítanak a twitterre letöltési és mentette a fájlt.

    > [AZURE.NOTE] Ha pedig az állapotsáv túl hosszú ideig tart, módosítania kell a szűrő trendfigyelésre témakörök; nyomon követéséhez Ha sok twitterre a témakör a szűr, nagyon gyorsan elérheti a 10000 twitterre szükséges.

###<a name="upload-the-data"></a>Az adatok feltöltése

Az adatok WASB (az elosztott fájlrendszer HDInsight által használt) feltöltéséhez használja az alábbi parancsokat:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Ez adatait a fürt csomópontjait által elérhető helyen tárolja.

##<a name="run-the-hiveql-job"></a>A HiveQL feladat futtatása


1. A következő paranccsal HiveQL utasításokat tartalmazó fájl létrehozása:

        nano twitter.hql
    
    A fájl tartalmát a következő használható:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Nyomja le a __Ctrl + X billentyűkombinációt__, majd nyomja le az __Y__ mentse a fájlt.

4. A következő parancsot használja a HiveQL a fájlban lévő futtatásához:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Ez a struktúra rendszerhéj betöltése, a HiveQL futtatja a __twitter.hql__ fájlban, és végül vissza egy `jdbc:hive2//localhost:10001/>` kérdés.
    
5. A beeline parancssorból a következő segítségével ellenőrizze, hogy adatokat a __twitterre__ tábla a __twitter.hql__ fájlban a HiveQL által létrehozott közül választhat:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Ez ad vissza legfeljebb 10 twitterre tartalmazó __Azure__ üzenet szövegében.

##<a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban azt egy strukturálatlan JSON adatkészlet alakíthat ki egy lekérdezés, adatainak vizsgálata és elemzése a Twitteren Azure hdinsight szolgáltatáshoz alkalmazással történő strukturált struktúratáblával hogyan van láthatók. További tudnivalókért lásd:

- [Első lépések a hdinsight szolgáltatáshoz](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Adatelemzés nézetbeli késleltetés segítségével hdinsight szolgáltatáshoz](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
