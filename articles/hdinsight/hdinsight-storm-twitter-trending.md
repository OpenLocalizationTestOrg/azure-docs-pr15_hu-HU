<properties
   pageTitle="Témakörök trendfigyelésre Apache vihar a HDInsight a Twitter |} Microsoft Azure"
   description="Megtudhatja, hogy miként Trident használatával hozzon létre egy Apache vihar topológia trendfigyelésre témakörök a Twitteren hashtags alapján határozza meg."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>A HDInsight Apache vihar a Twitteren trendfigyelésre témakörök meghatározása

Megtudhatja, hogy miként létre határozza meg, hogy trendfigyelésre témakörök (kivonat címkék) a Twitteren vihar topológiát Trident használatával.

Trident nyújt eszközöket, például az illesztés, összesítések, csoportosítási, funkciók és szűrők a magas szintű absztrakció. Ezenkívül Trident primitívek összeadja az állapot-nyilvántartó, egyre növekvő tendenciát mutat feldolgozás módon. Ez a példa bemutatja, hogyan készíthet egy olyan egyéni spout, a függvény és a Trident által biztosított számos beépített függvények topológia.

> [AZURE.NOTE] Ez a példa akkor erősen alapján a [Trident vihar](https://github.com/jalonsoramos/trident-storm) példa Juan Alonso.

##<a name="requirements"></a>Követelmények

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java- és a JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven tesztelése</a>

* <a href="http://git-scm.com/" target="_blank">Mely számjegy</a>

* A Twitter Fejlesztőeszközök fiók

##<a name="download-the-project"></a>A projekt letöltése

A projekt helyileg klónozhatja használja a következő kódot.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topológia

Ebben a példában a topológia van az alábbi képlettel történik:

![topológiája](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Az egyszerűsített nézetének a topológia. Több példányon összetevő fog elosztva a csomópontok a fürt.

A Trident kódot, amely a topológia van az alábbi képlettel történik:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Ez a kód az alábbi műveleteket végzi el:

1. Egy új adatfolyam a spout hoz létre. A spout twitterre Twitter, és az adott kulcsszavak (szeretetet, zenei és ebben a példában kávézó) szűrők őket.

2. HashtagExtractor, egy egyéni függvény minden tweetbe kivonat címkék kinyerése használják. Ezek kibocsátott az adatfolyam.

3. Az adatfolyamban kivonat címke szerint csoportosítva, és egy összesítő átadott. Ez az összesítő minden kivonat címke történt, hány alkalommal számának hoz létre. Az adatok megőrződnek memóriában. Végül egy új adatfolyam kibocsátott, amely tartalmazza a kivonat címke és a számát.

4. Mivel azt csak egy adott tétel twitterre a leggyakrabban használt kivonat címkék érdekli, az **FirstN** összeállítás érvényes csak a felső 10 adja vissza, a Számláló mező alapján.

> [AZURE.NOTE] A spout és HashtagExtractor eltérő beépített Trident funkcióit használja azt.
>
> Beépített műveletekkel kapcsolatos további tudnivalókért olvassa el a <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">csomag storm.trident.operation.builtin</a>című témakört.
>
> Munkafolyamat Trident megvalósítás MemoryMapState eltérő talál a következő:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Vihar Trident rugalmas keresés</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident vgx.dll</a>

###<a name="the-spout"></a>A spout

Spout, **TwitterSpout**, a Twitteren twitterre lekérdezni <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> használja. Szűrő létrehozása (szeretetet, zenei és ebben a példában kávézó), és egy csatolt blokkoló várólista tárolja, amelyek megegyeznek a szűrő bejövő twitterre (állapot). (További tudnivalókért lásd: <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Osztály LinkedBlockingQueue</a>.) Végül elemek lekért kikapcsolása a várakozási sorban található, és a topológia kibocsátott.

###<a name="the-hashtagextractor"></a>A HashtagExtractor

Ki kell olvasni kivonat címkék, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> a tweetbe szereplő összes kivonat-címkék beolvasásához használt. Ezek majd kibocsátott az adatfolyam.

##<a name="enable-twitter"></a>Engedélyezze a Twitteren

Az alábbi lépésekkel segítségével regisztrálhatja a Twitteren új alkalmazást, és a fogyasztói és a hozzáférési jogkivonat szükséges információk megszerzése olvassa el a Twitteren:

1. Nyissa meg <a href="https://apps.twitter.com" target="_blank">Az alkalmazások Twitter</a> , és kattintson az **Új alkalmazás létrehozása** gombra. Ha az űrlap kitöltése a **Visszahívás URL-cím** mezőt hagyja üresen.

2. Az alkalmazás létrehozásakor, kattintson a **kulcsok és a hozzáférési jogkivonat** fülre.

3. Másolja a **Fogyasztói billentyű** és a **Fogyasztói titkos** adatokat.

4. A lap alján válassza **létrehozása a hozzáférési jogkivonat** , ha nincs tokenek létezik. A tokenek létrehozás másolja a **Hozzáférési jogkivonat** , és a **Hozzáférési jogkivonat titkos** adatokat.

5. A projektben **TwitterSpoutTopology** korábban klónozva nyissa meg a **resources/twitter4j.properties** fájlt, adja meg a, összegyűjtötte az adatokat az előző lépéseket, és mentse a fájlt.

##<a name="build-the-topology"></a>A topológia összeállítása

A projekt létrehozásához használandó a következő kódot:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>A topológia tesztelése

A következő parancsot használja a topológia helyileg tesztelése:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

A topológia indítása után meg kell jelennie hibakeresési információkat, amely tartalmazza a kivonat címkéket, és megszámolja, a topológia által kibocsátott. A kimenet jelenjen meg az alábbihoz hasonló:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Következő lépések

Most, hogy a topológia helyileg tesztelése, megtudhatja, hogy miként bevezetését tervezi a topológia: [Deploy és kezelheti a HDInsight Apache vihar topológiák](hdinsight-storm-deploy-monitor-topology.md).

Is lehet érdekli a következő vihar témakörök:

* [Fejleszthet olyan Java topológiák vihar a HDInsight használatával maven tesztelése](hdinsight-storm-develop-java-topology.md)

* [A Visual Studio segítségével HDInsight vihar fejleszthet olyan C# topológiák](hdinsight-storm-develop-csharp-visual-studio-topology.md)

HDinsight további vihar példákat:

* [Példa a HDInsight vihar topológiát](hdinsight-storm-example-topology.md)
