<properties
    pageTitle="A HDInsight Linux fürt szín használata Hadoop |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti és a szín használata HDInsight Linux Hadoop fürt."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Telepítése és HDInsight Hadoop fürt szín használata

Megtudhatja, hogy miként szín telepítése HDInsight Linux fürt és tunneling segítségével a kérések szín továbbítja.

## <a name="what-is-hue"></a>Mi az, hogy a szín?

Szín webalkalmazások kezelése a Hadoop fürtre használt áll. Szín segítségével keresse meg a társított Hadoop fürt (ha HDInsight fürt WASB) tárolására, futtassa a struktúra feladatok és malac parancsfájlok stb. Az alábbi összetevőket szín telepítések egy HDInsight Hadoop fürt találhatók.

* Méhviasz struktúra szerkesztő
* Malac
* Metastore manager
* Oozie
* FileBrowser (amely folytatott beszélgetést WASB alapértelmezett tárolójához)
* Feladat böngészőben

> [AZURE.WARNING] A HDInsight fürt összetevői teljesen támogatott, és Microsoft Support segítséget nyújt azonosíthatók, és az alábbi összetevőket kapcsolatos problémák megoldásához.
>
> Egyéni összetevők kap, akár kereskedelmi célra is lehetővé teszi ügyfélszolgálatunkat, további elhárítani a problémát. Ez a probléma megoldása vagy kéri, hogy a hol található az adott technológia mély szakértelmét Megnyitás technológiák elérhető csatornák folytasson vezethet. Például vannak sok közösségi webhelyek használható, például: [HDInsight-fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache projekteket is projektwebhelyek a [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Telepítse a szín parancsfájl-műveletek használata

A következő parancsfájl műveletet szín telepítése Linux-alapú HDInsight fürt használható.
https://hdiconfigactions.BLOB.Core.Windows.NET/linuxhueconfigactionv02/Install-hue-uber-v02.SH
    
Ez a szakasz útmutatás használatáról a parancsfájl kiépítésekor a fürt az Azure-portálon. 

> [AZURE.NOTE] Azure PowerShell, az Azure CLI, a HDInsight .NET SDK vagy Azure erőforrás-kezelő sablonokat is használható parancsfájl műveletet. Parancsfájl-műveletek alkalmazása már fut fürt. További tudnivalókért lásd: a [parancsprogram-műveleteinek testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md).

1. Indítsa el a kiépítési fürt [Linux rendelkezést HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)az lépésekkel, de ne hajtsa végre a kiépítési.

    > [AZURE.NOTE] A HDInsight fürt szín telepítéséhez ajánlott headnode mérete legalább A4 (8 magmintákat, 14 GB memóriát).

2. Kattintson a **Választható beállítási** lap jelölje be a **Parancsfájl-műveletek**, és adja meg az adatokat, alább látható módon:

    ![Szín megadása parancsfájl művelet paraméterei] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Szín megadása parancsfájl művelet paraméterei")

    * __Név__: Adjon meg egy rövid nevet, a parancsprogram művelet.
    * __Parancsfájl URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Címsor__: ezt a jelölőnégyzetet
    * __Dolgozó__: hagyja üresen a mezőt.
    * __ZOOKEEPER__: hagyja üresen a mezőt.
    * __Paraméterek__: hagyja üresen a mezőt.

3. A képernyő alján a **Parancsfájl-műveletek**használja a **Kijelölés** gombot a konfiguráció mentéséhez. Végezetül használja a **Kijelölés** gombot a **Választható beállítási** a lap alján a választható konfigurációs adatok mentéséhez.

4. Továbbra is a fürt kiépítési, [Linux rendelkezést HDInsight fürt](hdinsight-hadoop-provision-linux-clusters.md#portal)leírt módon.

## <a name="use-hue-with-hdinsight-clusters"></a>Szín használata HDInsight fürt

SSH Tunneling csak úgy lehet hozzáférni a fürt szín, miután elindult. Via SSH Tunneling lehetővé teszi, hogy a elemre kattintva közvetlenül a headnode a fürt szín működik, ahol a forgalom. Miután a fürt befejeződött kiépítési, kövesse az alábbi lépéseket szín használata egy HDInsight Linux fürt.

1. Hozzon létre egy SSH alagutas az ügyfél rendszerből a HDInsight fürthöz használja protokollbújtatásban [használata SSH Ambari webes felület, erőforrás-kezelő, JobHistory, NameNode, Oozie, és más webes Felhasználóifelület-féle eléréséhez](hdinsight-linux-ambari-ssh-tunnel.md) az információkat, és adja meg a webböngészőben a SSH alagutas tartományként proxy.

2. Miután létrehozott egy SSH alagutas és beállítva a böngésző proxy forgalom keresztül, az elsődleges fő csomópont állomásneve, meg kell keresnie. Ehhez a SSH használata port 22 fürthöz kapcsolódó. Ha például `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` ahol __felhasználónév__ SSH felhasználónevét, __CLUSTERNAME__ pedig a csoport nevére.

    SSH használatával kapcsolatos további tudnivalókért lásd: a következő dokumentumokat:

    * [Linux-alapú HDInsight Linux rendszerhez, a Unix vagy a Mac OS X ügyfélprogramból SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [A Windows ügyfélről Linux-alapú HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Miután létrejött, a következő parancs segítségével szerezze be a tartománynevét, az elsődleges headnode:

        hostname -f

    Ez ad vissza egy nevet a következőhöz hasonló:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Ez a a hostname (állomásnév), az elsődleges headnode, ahol a szín webhelyen található.

2. A böngésző használatával nyissa meg a szín portált a http://HOSTNAME:8888. Cserélje ki az előző lépésben névből hostname (állomásnév).

    > [AZURE.NOTE] Jelentkezzen be az első alkalommal, amikor a rendszer kéri, jelentkezzen be a szín portál fiók létrehozásához. Az itt megadott hitelesítő adatok a portálon korlátozódik, és nem kapcsolódnak a rendszergazdai vagy SSH megadott rendelkezést közben a fürt felhasználó hitelesítő adatait.

    ![Jelentkezzen be a szín portáljára] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Szín portál hitelesítő adatok megadása")

### <a name="run-a-hive-query"></a>Struktúra lekérdezések futtatása

1. A szín portálon kattintson a **Lekérdezés szerkesztők**, és kattintson a **struktúra** a struktúra szerkesztő megnyitása gombra.

    ![Struktúra] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Struktúra")

2. A **segítségnyújtás** lap **adatbázisban**, alatt **hivesampletable**láthatók. Ez a minta táblához tartozó összes Hadoop fürt hdinsight szolgáltatásból lehetőségre. Írja be a példa lekérdezésre a jobb oldali ablaktáblán, és a kimenet jelenik meg az **eredmények** lapon, a ablakban az alábbi a képernyőképet látható módon.

    ![A lekérdezés futtatása struktúra] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "A lekérdezés futtatása struktúra")

    A **diagram** lap segítségével vizuálisan ábrázolhatók az eredmény látható.

### <a name="browse-the-cluster-storage"></a>Tallózással keresse meg a fürt tárolására

1. A szín portálon kattintson a **Fájl böngészőben** a menüsáv jobb felső sarkában.

2. Alapértelmezés szerint a fájlt a böngészőben megnyílik, a **/user/myuser** címtárban elemre. Kattintson a perjellel közvetlenül a felhasználó címtár elérési útját, nyissa meg a Azure tároló tároló fürthöz társított legfelső szintű előtt.

    ![Használja a fájl böngészőben] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Használja a fájl böngészőben")

3. Kattintson a jobb gombbal egy fájlt vagy mappát az elérhető műveletek megtekintéséhez. Fájlok feltöltése az aktuális könyvtár használja a felső sarokban **feltöltése** gombját. Új fájlok vagy mappák létrehozásához használhatja az **Új** gombra.

> [AZURE.NOTE] A szín fájl böngészőben csak megjelenítheti a HDInsight fürthöz társított alapértelmezett tároló tartalmát. Bármilyen további tárterületet fiókok/tárolók előfordulhat, hogy van társítva a fürt nem lesz elérhető a fájl böngészőn keresztül. A további tárolókat fürthöz társított azonban mindig lesznek elérhetők, a struktúra feladatokhoz. Ha például a paranccsal adhatja `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` , valamint a további tárolók tartalmának megjelenik a struktúra szerkesztőben. Ez a parancs a **newcontainer** nem áll a fürt társított alapértelmezett tároló.

## <a name="important-considerations"></a>Fontos tudnivalók

1. A szín telepítéséhez használt parancsfájl telepíti csak az elsődleges headnode a fürt.

2. A telepítés során több Hadoop-szolgáltatások (Fájlrendszerhez, fonal, MR2, Oozie) újraindítása után a konfigurációs frissítéséhez. A parancsfájl befejezése szín telepítése után eltarthat egy kis időt, más Hadoop-szolgáltatásokhoz indításához. A szín a teljesítmény kezdetben hatással lehet. Miután az összes szolgáltatás használatba veszik a, a szín teljes funkcionalitású lesz.

3.  Szín nem érti Tez feladatok, amely olyan, az aktuális alapértelmezett struktúra. Ha szeretné használni a struktúra végrehajtás motor MapReduce, frissítése a parancsprogramot, amely a következő parancsot használja a parancsfájl:

        set hive.execution.engine=mr;

4.  Linux fürt, ahol a szolgáltatások futó az elsődleges headnode közben az erőforrás-kezelő sikerült futó a másodlagos példa is. Egy ilyen esetben a szín részleteinek FUTÓ feladatok megjelenítése a fürt eredményezhet hibák (lásd alább). Azonban megtekinthetők a projekt adatait a feladat befejezése

    ![Szín portál hiba] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Szín portál hiba")

    Az ismert probléma miatt. Kerülő módosítása Ambari, hogy az aktív erőforrás-kezelő is alábbiakon futtatható az elsődleges headnode.

5.  Szín értelmez WebHDFS HDInsight fürt használata Azure tároló használata során `wasbs://`. Az egyéni parancsfájl parancsfájl végrehajtásához használja, és WebWasb, amely a beszélgetésben WASB WebHDFS-kompatibilis szolgáltatás a telepítést. Igen annak ellenére, hogy a szín portál Fájlrendszerhez helyen (például amikor, vigye az egérmutatót fölé a **Fájlt a böngészőben**) felirat látható, akkor úgy kell értelmezni, WASB.


## <a name="next-steps"></a>Következő lépések

- [A HDInsight fürt telepítése Giraph](hdinsight-hadoop-giraph-install-linux.md). Fürt testreszabási segítségével HDInsight Hadoop fürt Giraph telepítése. Giraph lehetővé teszi, hogy Hadoop használatával graph feldolgozás végezheti el, és az Azure hdinsight szolgáltatáshoz is használható.

- [A HDInsight fürt telepítése Solr](hdinsight-hadoop-solr-install-linux.md). Fürt testreszabási segítségével HDInsight Hadoop fürt Solr telepítése. Solr lehetővé teszi a tárolt adatok hatékony keresés műveletek hajthatók végre.

- [A HDInsight fürt telepítése R](hdinsight-hadoop-r-scripts-linux.md). Fürt testreszabási segítségével HDInsight Hadoop fürt R telepítése. R-Megnyitás Forrásnyelv és környezet statisztikai számítások. Több száz beépített statisztikai függvények és a saját programnyelv funkcionális és objektumorientált programozáshoz tulajdonságát kombináló biztosít. Teljes körű grafikus funkciókat is tartalmaz.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
