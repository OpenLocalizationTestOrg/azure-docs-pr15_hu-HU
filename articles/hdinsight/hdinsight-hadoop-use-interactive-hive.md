<properties
    pageTitle="A HDInsight interaktív struktúra |} Microsoft Azure"
    description="Megtudhatja, hogy miként interaktív struktúra (LLAP a struktúra) a hdinsight szolgáltatásból lehetőségre."
    keywords=""
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
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Interaktív struktúra a HDInsight (előzetes verzió)

Interaktív (más néven struktúra [Élő hosszú, és a folyamat]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) új HDInsight [fürt típus]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types)van.  Interaktív struktúra lehetővé teszi, a memóriahasználat, hogy segítségükkel struktúra lekérdezések sokkal több interaktív gyorsabb gyorsítótárazás. Ez a szolgáltatás lehetővé teszi HDInsight a világ rugalmas, a legtöbb performant, és a memóriában az a felhő megnyitott nagy adatok megoldás gyorsítótárát (struktúra és a külső használatával), és a speciális analytics-mély-integráció a R szolgáltatások keresztül. 

A interaktív struktúra fürt eltér a Hadoop fürt. Csak a struktúra szolgáltatást tartalmaz. 

> [AZURE.NOTE] MapReduce, malac, Sqoop, Oozie és más szolgáltatások törlődnek az adott fürt hamarosan.
A interaktív struktúra fürt a struktúra szolgáltatás csak az a Ambari struktúra nézet Beeline és struktúra ODBC keresztül érhető el. Struktúra konzol, Templeton, Azure CLI és Azure PowerShell keresztül nem érhető el. 


 


## <a name="create-an-interactive-hive-cluster"></a>Hozzon létre egy interaktív struktúra fürthöz

Interaktív struktúra fürt Linux-alapú fürt csak támogatott. HDInsight fürt létrehozásával kapcsolatos további tudnivalókért lásd: [létrehozása Linux-alapú Hadoop fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Hajtsa végre a struktúra interaktív struktúrából

Vannak a különböző beállításokat, hogy hogyan hajthat végre struktúra lekérdezések:

- Futtassa az Ambari struktúra nézet struktúra

    A struktúra nézet használatával kapcsolatos információkért olvassa el a [Hadoop a HDInsight a struktúra nézet használata]( hdinsight-hadoop-use-hive-ambari-view.md)című témakört.

- Futtassa a struktúra Beeline használatával

    Beeline használatáról a HDInsight tudnivalókért lásd [A Hadoop HDInsight Beeline és a struktúra használatát](hdinsight-hadoop-use-hive-beeline.md).

    A headnode vagy egy üres él csomópont Beeline segítségével.  Egy üres él csomópont Beeline használata ajánlott.  Egy HDInsight fürthöz hoz létre egy üres edgenode a további tudnivalókért lásd [használata üres él csomópontok hdinsight szolgáltatásból lehetőségre](hdinsight-apps-use-edge-node.md).

- Futtassa a struktúra struktúra ODBC használatával

    A struktúra ODBC használatával kapcsolatos információkért lásd: [Excel csatlakoztatása a Microsoft-struktúra ODBC-illesztőprogram hadoop](hdinsight-connect-excel-hive-odbc-driver.md).

**A JDBC kapcsolati karakterlánc megkeresése:**

1.  Jelentkezzen be az alábbi URL-cím használatával Ambari: https://<ClusterName>. AzureHDInsight.net.
2.  A bal oldali menüben kattintson a **struktúra** .
3.  Kattintson a kiemelt ikonra kattintva másolja a vágólapra az URL-cím:

    ![HDInsight Hadoop interaktív struktúra LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Lásd még:
-   [A HDInsight létrehozása Linux-alapú Hadoop fürt](hdinsight-hadoop-provision-linux-clusters.md): megtudhatja, hogy miként létrehozása interaktív struktúra fürt hdinsight szolgáltatásból lehetőségre.
-   [A Hadoop HDInsight Beeline és a struktúra használata](hdinsight-hadoop-use-hive-beeline.md): megtudhatja, hogy miként struktúra lekérdezések elküldése Beeline használatával.
-   [Excel csatlakoztatása a Microsoft-struktúra ODBC-illesztőprogram hadoop](hdinsight-connect-excel-hive-odbc-driver.md): Útmutató az Excel csatlakozni struktúra.
