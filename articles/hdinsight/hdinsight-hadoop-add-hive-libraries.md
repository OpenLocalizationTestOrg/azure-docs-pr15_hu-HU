<properties
pageTitle="Adja hozzá a struktúra tárak HDInsight fürt létrehozása során |} Azure"
description="Megtudhatja, hogy miként struktúra tárak (üveg fájlok) felvétele egy HDInsight fürthöz fürt létrehozása során."
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
ms.date="09/20/2016"
ms.author="larryfr"/>

#<a name="add-hive-libraries-during-hdinsight-cluster-creation"></a>Adja hozzá a struktúra tárak HDInsight fürt létrehozása során.

Ha tárak, amelyet a HDInsight struktúra gyakran használ, akkor a dokumentum egy parancsprogramot művelettel előre töltse be a csoport létrehozása során a tárak információkat tartalmaz. Ez a tárak globálisan számára elérhetővé teszi a struktúra (nincs szükség [JAR felvétele](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) segítségével töltse be őket.)

##<a name="how-it-works"></a>Működése

A csoport létrehozásakor megadhat egy parancsprogramot műveletet, amely az alábbiakon futtatható parancsfájl fürt csomópontok alatt a létrehozásuk közben. A dokumentumban a parancsfájl fogadja el a egyetlen paraméter, és a tárak (tárolt üveg-fájlként), hogy előre betöltött tartalmazó WASB helyen található.

Csoport létrehozása során a parancsfájl felsorolja a fájlokat, másolja át őket, hogy a `/usr/lib/customhivelibs/` könyvtárában címsor és dolgozó csomópontokat, majd ezek a a `hive.aux.jars.path` tulajdonság a a `core-site.xml` fájlt. A Linux-alapú fürt is frissíti a `hive-env.sh` kiterjesztésű fájlok helyét.

> [AZURE.NOTE] Ez a cikk a parancsfájl-műveletek használata elérhetővé teszi a tárakat az alábbi esetekben:
>
> * __Linux-alapú HDInsight__ - a __struktúra parancssori__ __WebHCat__ (Templeton) és __HiveServer2__használata esetén.
> * __Windows-alapú HDInsight__ - __struktúra parancssori__ és __WebHCat__ (Templeton) használata esetén.

##<a name="the-script"></a>A parancsprogram

__Parancsfájl helye__

__Linux-alapú fürtre__vonatkozóan: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

__Windows-alapú fürtre__vonatkozóan: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

__Követelmények__

* A parancsfájlok a __címsor csomópontok__ és a __dolgozó csomópontok__kell alkalmazni.

* A telepíteni kívánt kancsó __egyetlen tároló__Azure Blob-tárolóhoz kell tárolni. 

* A tároló fiókot tartalmazó üveg fájlok __kell__ a könyvtár létrehozása során a HDInsight fürt csatolható. A két módon lehet elvégezni:

    * Úgy, hogy az alapértelmezett tároló ügyfél a fürt tároló nyelvét.
    
    * Szerint kattintson egy csatolt tároló tároló tároló nyelvét. Ha például a portálon is használhatja __Választható konfigurációja__, további tárterület __csatolt tároló fiókok__ .

* A tároló WASB elérési útjának a paramétert a parancsfájl műveletet kell megadni. Ha például a kancsó __könyvtárral lenne__ __mystorage__nevű tárterület-fiókjában nevű tároló tárolja, ha a paraméter lenne __wasbs://libs@mystorage.blob.core.windows.net/__.

    > [AZURE.NOTE] A dokumentum feltételezi, hogy már rendelkezik a tárterület-fiók létrehozása, blob-tárolóhoz, és a fájlokat feltölteni azt. 
    >
    > Ha létrehozott egy tárterület-fiók nem, ehhez az [Azure portálon](https://portal.azure.com)keresztül. Azután használhatja például [Azure tároló Explorer](http://storageexplorer.com/) segédprogram hozhat létre új tároló a fiókot, és a fájlok feltöltése.

##<a name="create-a-cluster-using-the-script"></a>A parancsfájl használatával fürt létrehozása

> [AZURE.NOTE] Az alábbi lépéseket hozzon létre egy új Linux-alapú HDInsight fürt. Hozzon létre egy új Windows-alapú fürt, jelölje ki a __Windows__ a fürt OS a fürt létrehozásakor, majd a Windows (PowerShell) parancsfájl a bash parancsfájl helyett.
> 
> Ez a parancsfájl használatával fürt létrehozása Azure PowerShell vagy a HDInsight .NET SDK is használhatja. További tájékoztatást a következő módszerekkel [testreszabása HDInsight fürt parancsprogram-műveleteinek](hdinsight-hadoop-customize-cluster-linux.md)megtekintése

1. Indítsa el a kiépítési fürt [Linux rendelkezést HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)az lépésekkel, de ne hajtsa végre a kiépítési gombra.

2. Kattintson a **Választható beállítási** lap jelölje be a **Parancsfájl-műveletek**, és adja meg az adatokat, alább látható módon:

    * __Név__: Adjon meg egy rövid nevet, a parancsprogram művelet.
    * __Parancsfájl URI__: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh
    * __Címsor__: ezt a jelölőnégyzetet
    * __Dolgozó__: ezt a jelölőnégyzetet.
    * __ZOOKEEPER__: hagyja üresen a mezőt.
    * __Paraméterek__: Adja meg a WASB címet, amely tartalmazza a kancsó tároló és a tárterület-fiókjába. Ha például __wasbs://libs@mystorage.blob.core.windows.net/__.

3. A képernyő alján a **Parancsfájl-műveletek**használja a **Kijelölés** gombot a konfiguráció mentéséhez.

4. A **Választható beállítási** lap jelölje be a __Csatolt tárterület-fiók__ , és jelölje ki a __tárhely kulcs hozzáadása__ hivatkozást. Jelölje ki a tárterület-fiókot, amely tartalmazza a kancsó, és majd gombokkal a __Válassza a__ beállítások mentéséhez és a __Választható beállítási__ lap vissza.

5. Mentse a választható konfigurációs adatok használatával a **Választható beállítási** a lap alján a **Kijelölés** gombra.

6. Továbbra is a fürt kiépítési, [Linux rendelkezést HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)leírt módon.

Miután befejezte a fürt létrehozása, használhatja a parancsfájl az hozzáadni struktúrából használata nélkül kapcsoljon kancsó lesz a `ADD JAR` utasítás.

##<a name="next-steps"></a>Következő lépések

A dokumentum megtanulta van hozzáadásáról a struktúra tárak fürt létrehozása során a HDInsight fürtre üveg-fájlokban található. Struktúra használata a további tudnivalókért lásd: a [HDInsight a struktúra használata](hdinsight-use-hive.md)
