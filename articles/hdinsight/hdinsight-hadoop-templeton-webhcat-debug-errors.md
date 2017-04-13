<properties
 pageTitle="Dátumtáblázatok ismertetése és HDInsight WebHCat hibák megoldása"
 description="Megtudhatja, hogy körülbelül kapcsolatos gyakori hibákra által visszaadott WebHCat a HDInsight és azok megoldását."
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
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Dátumtáblázatok ismertetése és WebHCat (Templeton,) kapott hibák megoldása a HDInsight

Használatával (korábbi nevén Templeton,) WebHCat HDInsight, hiba jelenhet meg. Ehhez a dokumentumhoz a gyakori hibák – miért előfordulási helyük, és mit tehet megoldási lehetőséget nyújt útmutatást.

##<a name="what-is-webhcat"></a>Mi az WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) Hadoop-réteg [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a táblázat és a tárhely kezelése a REST API. WebHCat HDInsight fürt alapértelmezés szerint engedélyezve van, és használja, különböző eszközök feladatok elküldése gombra, majd a feladat állapota, stb a fürthöz bejelentkezés nélkül.

##<a name="modifying-configuration"></a>Konfigurációs módosítása

> [AZURE.IMPORTANT] A hibák, a dokumentumban szereplő számos oka, hogy beállított legfeljebb túllépve. Ha a megoldás lépéssel "Említések", hogy egy értéket módosíthatja, kell használnia az alábbiak egyikét a módosítás végrehajtása:

* **A Windows** fürt esetében: a parancsprogram művelettel állítsa be a csoport létrehozása során a értéket. További tudnivalókért olvassa el a [fejlesztése parancsfájl-műveletek](hdinsight-hadoop-script-actions.md)című témakört.

* **Linux** fürtre vonatkozóan: használata Ambari (webes vagy REST API-val) módosítása az értéket. További tudnivalókért lásd: a [HDInsight kezelése Ambari használatával](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Alapértelmezett beállítás

Alapértelmezett beállításokkal WebHCat a teljesítményt, és hibákat okozhatnak, ha ezeket az értékeket túllépik a következők:

| A beállítás | Rendeltetés | Alapérték |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.maximum-alkalmazások][maximum-applications] | Egyidejűleg lehet aktív feladatok maximális száma (függőben vagy fut) | 10 000 |
| [templeton.Exec.max-procs][max-procs] | Egyidejűleg felszolgált kérések maximális száma | 20 |
| [mapreduce.jobhistory.max-kor-ms][max-age-ms] | Az, hogy hány napig korábbi megmarad. | 7 nap |

##<a name="too-many-requests"></a>Túl sok kérések

**HTTP-állapotkód**: 429

| OK | Megoldás |
| ----- | ---------- |
| Ha a művelet túllépi a maximális egyidejű kérelmek WebHCat, melyet egy perc alatt (alapértelmezés szerint 20) | Csökkentse a terhelést annak érdekében, hogy nem elküldése több, mint az egyidejű kérelmek maximális száma vagy az egyidejű kérések korlátozása növelése módosításával `templeton.exec.max-procs`. További tájékoztatás [konfigurációs módosítása](#modifying-configuration) : |

##<a name="server-unavailable"></a>Kiszolgáló nem érhető el

**HTTP-állapotkód**: 503

| OK | Megoldás |
| ---------------- | ------------------- |
| Ez általában a fürt az elsődleges és másodlagos HeadNode közötti feladatátvételkor történik | Várjon két percet, majd próbálkozzon újra |

##<a name="bad-request-content-could-not-find-job"></a>Hibás kérés tartalom: nem találja a feladat

**HTTP-állapotkód**: 400

| OK | Megoldás |
| ---------------- | ------------------- |
| Feladat részletei van lett eltávolítva által a korábbi tisztító | Az alapértelmezett az adatmegőrzési időszak a korábbi 7 napig tart. Ez a módosításával módosítható `mapreduce.jobhistory.max-age-ms`. További tájékoztatás [konfigurációs módosítása](#modifying-configuration) : |
| Feladat leállította feladatátvevő miatt | Ismételje meg a két percig feladat elküldése |
| Az érvénytelen feladat azonosító használt | Ellenőrizze, hogy a feladat azonosítója megfelelő |

##<a name="bad-gateway"></a>Hibás átjáró

**HTTP-állapotkód**: 502

| OK | Megoldás |
| ---------------- | ------------------- |
| Mi az WebHCat folyamat belső szemétgyűjtő | Várja meg a Befejezés vagy, indítsa újra a WebHCat szolgáltatást szemétgyűjtő |
| Az erőforrás-kezelő szolgáltatás választ vár időtúllépés. Ez akkor fordulhat elő, amikor az aktív alkalmazás a számot a megadott maximális (alapértelmezett 10 000) | Várja meg a futó feladatok befejezéséhez, illetve a feladat egyidejű korlát növelése módosításával `yarn.scheduler.capacity.maximum-applications`. További tájékoztatás [konfigurációs módosítása](#modifying-configuration) :  |
| Amikor megpróbál – az [első /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) hívás közben az összes feladat beolvasásához `Fields` beállítása`*` | Nem *minden* feladat részletei beolvasásához, ehelyett `jobid` beolvasásához csak nagyobb, mint bizonyos feladat azonosítója feladatok részletes adatait. Vagy ne használjon`Fields` |
| A WebHCat szolgáltatás HeadNode feladatátvételkor nem működik | Várjon két percig, és ismételje meg a műveletet |
| Vannak olyan WebHCat elküldeni több mint 500 függőben lévő feladatok | Várja meg, amíg a jelenleg folyamatban van a feladatok befejezése további feladatok elküldése előtt |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
