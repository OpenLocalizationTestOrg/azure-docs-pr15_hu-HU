<properties
pageTitle="A HDInsight malac DataFu használata"
description="DataFu gyűjteménye tárak Hadoop való használatra. Megtudhatja, hogyan használhatja DataFu a malac a HDInsight fürt."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>A HDInsight malac DataFu használata

DataFu gyűjteménye Megnyitás tárak Hadoop való használatra. A jelen dokumentum megtanulhatja a HDInsight fürt DataFu használatáról, és hogyan malac DataFu felhasználó által definiált függvények (UDF) használata.

##<a name="prerequisites"></a>Előfeltételek

* Egy Azure-előfizetést.

* Az Azure hdinsight szolgáltatáshoz fürt (Linux vagy Windows-alapú)

* [A HDInsight malac használata](hdinsight-use-pig.md) egyszerű ismerős

##<a name="install-datafu-on-linux-based-hdinsight"></a>Linux-alapú HDInsight DataFu telepítése

> [AZURE.NOTE] DataFu 3.3 és magasabb fürt Linux-alapú verzióját, és a Windows-alapú fürt van telepítve. Még nincs telepítve a Linux-alapú fürt verziónál 3.3.
>
> Ha egy fürt Linux-alapú verzióját 3.3 vagy újabb, vagy a Windows-alapú fürtre szolgáltatást használ, kihagyhatja ebben a szakaszban.

DataFu is letölti és telepíti át a maven tesztelése tárat. DataFu hozzáadása a HDInsight fürthöz kövesse az alábbi lépéseket:

1. Csatlakozzon a Linux-alapú HDInsight fürthöz SSH használatával. A HDInsight SSH használja a további tudnivalókért lásd: a következő dokumentumokat:

    * [A HDInsight Linux OS X és Unix Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. A következő parancs segítségével töltse le a DataFu üveg fájlt a wget segédprogram használata, másolása és másolja a hivatkozást a böngészőben a letöltés indításához.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Ezután a HDInsight fürt alapértelmezett tárolóhoz töltse fel a fájlt. Ez a fájl számára elérhetővé teszi csomópontjait a fürt, és a fájl lesz látható az tárhely, még akkor is, ha törli, és hozza létre a fürt.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] A fenti példa tárolja a üveg a `wasbs:///example/jars` mert már létezik ezt a címtárat a fürt tárolón. Mindegy, hol tárolja a HDInsight fürt tároló kívánt is használhatja.

##<a name="use-datafu-with-pig"></a>Malac DataFu használata

Ebben a szakaszban ismertetett lépések feltételezik, jól ismert HDInsight malac használ, és csak adja meg a malac Latin kimutatásokat, nem a bemutatjuk, hogyan használhatók a fürt. Malac használata hdinsight szolgáltatásból lehetőségre a további tudnivalókért lásd: [Használata malac hdinsight szolgáltatásból lehetőségre](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] A malac DataFu Linux-alapú HDInsight fürtre használatakor regisztrálnia kell a a üveg fájlt, az alábbi malac Latin utasítás használatával:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> Alapértelmezés szerint a Windows-alapú HDInsight fürt DataFu regisztrált.

Általában DataFu függvények alias határozza meg. Példa:

    DEFINE SHA datafu.pig.hash.SHA();
    
Alias elnevezett határozza meg `SHA` esetében a kivonatolás függvény rendszerállapot-ÜGYNÖK. Használhatja ezt malac Latin parancsfájl a bemeneti adatok kivonat létrehozásához. Ha például az alábbi a neveket a bemeneti adatok egy kivonat értékkel helyettesíti:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Ha a következő bemeneti adatok való használatra:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Hoz létre, akkor a következő kimenet:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Következő lépések

DataFu vagy malac további tájékoztatást talál a következő dokumentumokat:

* [Útmutató a Apache DataFu malac](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
