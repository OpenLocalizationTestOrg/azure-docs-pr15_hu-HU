<properties
    pageTitle="Felhasználás R HDInsight fürt testreszabása |} Microsoft Azure"
    description="Megtudhatja, hogyan telepítheti az R parancsfájl műveletével, és R használata fürt hdinsight szolgáltatásból lehetőségre."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Telepítése és R HDInsight Hadoop fürt használata

Ismerje meg, testreszabása a Windows Script művelettel R HDInsight fürt alapján és fürtök R használatáról hdinsight szolgáltatásból lehetőségre. A [prémium réteg](https://azure.microsoft.com/pricing/details/hdinsight/) kínáló HDInsight-R-kiszolgáló a HDInsight fürt részeként tartalmazza. Ebben a csoportban adhatja MapReduce és a külső használható elosztott számítások futtatásához R parancsfájlokat. További tudnivalókért olvassa el a [R Server HDInsight használatának első lépései](hdinsight-hadoop-r-server-get-started.md)című témakört. A Linux-alapú fürt R használja a további tudnivalókért lásd [telepítése és használata R HDinsight Hadoop fürt (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
Azure hdinsight szolgáltatáshoz fürt (Hadoop, vihar, HBase, külső) bármilyen típusú R telepíthető *Parancsfájl*műveletével. A csak olvasható Azure tároló blob [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)a R telepítése egy HDInsight fürthöz mintaparancsfájl érhető el. 

**Kapcsolódó cikkek**

- [Telepítése és használata R HDinsight Hadoop fürt (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [A HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md): HDInsight fürt létrehozásával kapcsolatos általános információk
- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-cluster-customize]: általános tudnivalók a parancsprogram művelettel HDInsight fürt testreszabása
- [Fejleszthet olyan parancsfájl műveletet parancsfájlok hdinsight szolgáltatáshoz](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Mi az R?

A <a href="http://www.r-project.org/" target="_blank">Projekt R statisztikai számítások</a> egy megnyitott Forrásnyelv és környezet statisztikai számítások. R több száz beépített statisztikai függvények és a saját programnyelv funkcionális és objektumorientált programozáshoz tulajdonságát kombináló biztosít. Teljes körű grafikus funkciókat is tartalmaz. R a legtöbb profi statisztikusok és tudósok számos különböző típusú mezők az előnyben részesített programozási környezetben.

R a kompatibilis az Azure Blob-tároló (WASB), így ott tárolt adatokhoz R használata HDInsight lehet feldolgozni.  

## <a name="install-r"></a>R telepítése

R telepítése egy HDInsight fürthöz [mintaparancsfájl](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) az Azure-tárolóban lévő csak olvasható blob érhető el. Ez a témakör a mintaparancsfájl használatáról a fürt az Azure-portálon létrehozásakor utasításokat.

> [AZURE.NOTE] A mintaparancsfájl 3.1-es HDInsight fürt jelent meg. HDInsight fürt verziók kapcsolatos további tudnivalókért olvassa el a [HDInsight fürt verziók](hdinsight-component-versioning.md)című témakört.

1. Amikor létrehoz egy HDInsight fürthöz a portálról, kattintson a **Választható konfigurációja**, és kattintson a **Parancsprogram-műveletek**.
2. **Parancsfájl-műveletek** lapon írja be az alábbi értékeket:

    ![Parancsfájl műveletek fürt testreszabása] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Parancsfájl műveletek fürt testreszabása")

    <table border='1'>
        <tr><th>A tulajdonság</th><th>Érték</th></tr>
        <tr><td>név</td>
            <td>Adja meg, hogy a parancsprogram műveletet, például <b>Telepítése R</b>nevét.</td></tr>
        <tr><td>Parancsfájl URI</td>
            <td>Adja meg a parancsfájlt, ha testre szeretné szabni a fürt, például <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i> hív meg a URI</td></tr>
        <tr><td>Csomópont típusa</td>
            <td>Adja meg a csomópontokat a testreszabási parancsfájl futtatható. Megadhatja <b>Az összes csomópontok</b>, <b>csak a központi csomópontok</b>vagy <b>dolgozó csomópontok</b> csak.
        <tr><td>Paraméterek</td>
            <td>Szükség szerint a parancsfájlt, adja meg a paraméterek. Azonban a parancsfájl R telepítéséhez nincs szükség paramétereket, így üresen is hagyhat ki.</td></tr>
    </table>

    A fürt több összetevők telepítése egynél több parancsfájl műveletet is hozzáadhat. Miután hozzáadta a parancsfájlok, kattintson a jelölőnégyzet be van jelölve a fürt crating indítása.

A parancsfájl használatával R telepítése HDInsight Azure PowerShell vagy a HDInsight .NET SDK használatával. Ezeket a műveleteket az utasításokat a jelen cikk.

## <a name="run-r-scripts"></a>R parancsfájlok futtatása
Ez a szakasz ismerteti, hogyan a Hadoop fürthöz HDInsight-R-parancsprogram futtatásához.

1. **Távoli asztali kapcsolat létrehozása a fürthöz**: A a portálon engedélyezése a távoli asztali a fürt R telepítve van a létrehozott, és csatlakoztassa a fürthöz. Útmutatásért lásd: [Csatlakozás HDInsight fürt RDP segítségével](hdinsight-administer-use-management-portal.md#rdp).

2. **Nyissa meg az R konzolt**: az asztalon, a fej csomópont az R telepítési helyezi az R konzolon hivatkozás. Kattintással nyissa meg az R konzolt.

3. **Futtassa a R**: az R parancsfájlt közvetlenül az R konzolról beillesztéssel, szeretne, jelölje ki, és nyomja le az ENTER BILLENTYŰT. Íme egy egyszerű példa parancsfájlt, ami a számokat, 1 és 100 hoz létre őket majd megszorzása 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Az első két sor hívja fel a j telepített RHadoop tárak Az utolsó sorban az eredmények a konzolhoz nyomtatja. A kimenet így néz ki:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Telepítse az R Aure PowerShell használatával

Lásd: [testreszabása HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  A minta bemutatja, hogyan telepítheti a külső Azure PowerShell használatával. Ha testre szeretné szabni a parancsfájlt [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)szüksége.

## <a name="install-r-using-net-sdk"></a>.NET SDK használatával R telepítése

Lásd: a [parancsprogram művelettel testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). A minta bemutatja, hogyan lehet a .NET SDK használatával külső telepítése. Ha testre szeretné szabni a parancsfájlt [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)szüksége.


## <a name="see-also"></a>Lásd még:

- [Telepítése és használata R HDinsight Hadoop fürt (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [A HDInsight létrehozása Hadoop fürt](hdinsight-provision-clusters.md): HDInsight fürt létrehozásával kapcsolatos általános információk
- [Parancsfájl művelettel HDInsight fürt testreszabása][hdinsight-cluster-customize]: általános tudnivalók a parancsprogram művelettel HDInsight fürt testreszabása
- [Fejleszthet olyan parancsfájl műveletet parancsfájlok hdinsight szolgáltatáshoz](hdinsight-hadoop-script-actions.md)
- [Telepítése és használata a külső HDInsight fürt][hdinsight-install-spark]: külső telepítéséről parancsfájl műveletet minta
- [A HDInsight fürt telepítése Giraph](hdinsight-hadoop-giraph-install.md): Giraph telepítéséről parancsfájl műveletet minta
- [A HDInsight fürt telepítése Solr](hdinsight-hadoop-solr-install-linux.md): parancsfájl műveletet minta Solr telepítéséről.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
