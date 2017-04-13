<properties
    pageTitle="A HDInsight a portálon Linux Hadoop, HBase, vihar vagy külső fürt létrehozása |} Microsoft Azure"
    description="Megtudhatja, hogy miként létrehozása a Hadoop, HBase, vihar vagy külső fürt Linux a webböngésző és az Azure előzetes portal segítségével hdinsight szolgáltatásból lehetőségre."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>


#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Az Azure portálon HDInsight Linux-alapú fürt létrehozása

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Az Azure portal egy egy webes kezelése eszköz szolgáltatásokat és a Microsoft Azure felhőben tárolt erőforrásokat. Ez a cikk megtanulhatja, hogy miként hozhat létre a portálon Linux-alapú HDInsight fürt.

## <a name="prerequisites"></a>Előfeltételek

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Modern webböngészőben__. Az Azure portálon használja a HTML5-ös és a Javascript, és a régebbi böngészők nem működik megfelelően.

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-clusters"></a>Fürt létrehozása

Az Azure-portálra elérhetővé teszi a többsége fürt tulajdonságainak. Erőforrás-kezelő Azure sablont használ, elrejtheti a részletek sok. További tudnivalókért olvassa el a [Hadoop létrehozása Linux-alapú fürtök sablonokkal az erőforrás-kezelő Azure hdinsight szolgáltatáshoz a](hdinsight-hadoop-create-linux-clusters-arm-templates.md)című témakört.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. Kattintson az **Új**gombra, válassza az **Adatok analitika**, és válassza a **hdinsight szolgáltatásból lehetőségre**.

    ![Az Azure-portálon új fürt létrehozása] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.1.png "Az Azure-portálon új fürt létrehozása")
3. Írja be a **Csoport neve**: Ez a név globálisan egyedinek kell lennie.
4. Kattintson a **Válassza ki a csoportját típusát**, és válassza a:

    - **Fürt típusa**: Ha nem tudja, hogy mi a válassza ki, jelölje be a **Hadoop**. A leggyakrabban használt fürt típus.

        > [AZURE.IMPORTANT] HDInsight fürt fájltípusok, amelyek megfelelnek a terhelést vagy technológiát alkalmaz, amely a fürt van beállítva a különböző származnak. Nincs semmilyen módszerrel nem támogatott többféle, például vihar és egy fürt HBase kombináló fürt létrehozásához. 

    - **Operációs rendszer**: jelölje be a **Linux rendszerhez**.
    - **Verzió**: az alapértelmezett verzióra használja, ha nem tudja, hogy mi a válassza ki. További tudnivalókért olvassa el a [HDInsight fürt verzióival](hdinsight-component-versioning.md)foglalkozó.
    - **Fürt réteg**: Azure hdinsight szolgáltatáshoz a nagy adatok felhőben szeretne rendelni, a két kategóriába biztosít: szabványos réteg és prémium réteg. További tudnivalókért olvassa el a [fürt rétegek](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers)című témakört.
    
    ![HDInsight prémium szint konfigurálása](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)

4. Kattintson az **előfizetés** jelölje ki az Azure előfizetés a fürt használt elemre.

5. **Erőforráscsoport** , jelölje be a meglévő erőforráscsoport, vagy kattintson az **Új** hozhat létre új erőforráscsoport

    > [AZURE.NOTE] Ez a bejegyzés fog alapértelmezés szerint egy meglévő erőforrás csoportot, ha ilyen.

6. Kattintson a **hitelesítő adatok** , és írja be egy jelszót a rendszergazda felhasználó számára. Meg kell adnia egy **SSH felhasználónév** és a **JELSZAVÁT** vagy **Nyilvános kulcs**, amely SSH csatlakozó felhasználók hitelesítését szolgálnak. Nyilvános kulccsal a javasolt megoldás. A képernyő alján a hitelesítő adatok konfiguráció mentéséhez kattintson a **Kijelölés** gombra.

    ![Adjon fürt hitelesítő adatok] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.3.png "Adjon fürt hitelesítő adatok")

    A HDInsight SSH használja a további tudnivalókért lásd: az alábbi cikkekben:

    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)


7. Kattintson az **Adatforrás** választhatja ki a fürt meglévő adatforrásból listában, vagy hozzon létre egy újat.

    ![Adatforrás-lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.4.png "Megadása az adatforrás konfigurálása")

    Egy HDInsight fürt adatforrásként jelenleg is jelöljön ki egy Azure tárterület-fiókot. Használja a következő oldalon az **Adatforrás** lap megértéséhez.

    - **Kiválasztás módja**: az **összes előfizetésekről** ahhoz, hogy a előfizetésekről tároló fiókok böngészési ezeket a beállításokat. Állítsa a **Hívóbetű** , ha be szeretné írni a **Tárhely neve** és a **Hívóbetű** egy meglévő tárterület-fiókot.

    - **Jelölje ki azt a fiókot tároló / új**: kattintson a **Jelölje ki azt a fiókot tároló** tallózással keresse meg és jelöljön ki egy meglévő tárterület-fiókot, a fürt társítani szeretné. Vagy kattintson az **Új** tároló új fiók létrehozása gombra. Használja a mezőt, amely megjelenik a tárterület-fiókja nevét. Egy zöld színű pipa jel érhető el a név jelenik meg

    - **Alapértelmezett tároló kiválasztása**: Ezzel a paranccsal adja meg a fürt használandó alapértelmezett tárolóját nevét. Egy tetszőleges nevet itt adhatja meg, amíg használata javasolt, mert ugyanazt a nevet a fürt, hogy könnyen megjegyezhető szolgál, hogy a tároló a adott fürt számára.

    - **Hely**: A földrajzi területhez tartozik, amelyek a tárterület-fiókot, vagy jön létre.

        > [AZURE.IMPORTANT] Az alapértelmezett adatforrás helyének kiválasztása a HDInsight fürt helyét is beállíthat. A fürt és az alapértelmezett adatforrás ugyanabban a régióban kell lennie.
        
    - **Fürt AAD identitás**: konfigurálásával azt, akkor válik elérhetővé a fürt a az Azure adatok tó áruházak AAD konfigurációja miatt.

    Kattintson a **Jelölje ki** az adatforrás konfigurációját mentése gombra.

8. Kattintson a **Csomópontra árak rétegek** létrejön a fürt csomópontok kapcsolatos információk megjelenítéséhez. Van szüksége a fürt dolgozó csomópontok számának megadása. A becsült költsége a fürt belül a lap jelenik meg.

    ![A csomópont árak rétegek lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.5.png "Fürt csomópontok számának megadása")
    
    > [AZURE.IMPORTANT] Ha több mint 32 dolgozó csomópontok, fürt létrehozása, vagy a fürt méretezéssel után létrehozása, majd ki kell választania egy központi csomópont méretének és legalább 8 magmintákat 14GB ram.
    >
    > Csomópont méretét és a kapcsolódó költségek a további tudnivalókért olvassa el a [HDInsight árak](https://azure.microsoft.com/pricing/details/hdinsight/)című témakört.

    Kattintson a **Jelölje ki** a csomópont árak konfiguráció mentéséhez.

9. Kattintson a **Választható beállítási** jelölje ki a fürt verziót, valamint egyéb választható beállítások, mint, például egy **Virtuális hálózati**csatlakozás, állíthat be egy **Külső Metastore** adatok tárolására struktúra és Oozie, testre szabhatja az egyéni összetevők telepítése fürt parancsfájl-műveletek segítségével, vagy további tárterület fiókok használata a fürt.

    * **Virtuális hálózati**: jelölje be az Azure virtuális hálózati és az alhálózathoz, ha el szeretné helyezni a fürt egy virtuális hálózatba.  

        ![Virtuális hálózati lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.6.png "Adja meg a hálózati virtuális részletei")

        Adatok HDInsight használatáról virtuális a hálózat, beleértve a virtuális hálózat adott konfigurációs követelményei [kiterjesztése HDInsight funkciók egy Azure virtuális hálózat segítségével](hdinsight-extend-hadoop-virtual-network.md)című cikkben talál.

    * Kattintson a **Külső Metastores** fürthöz társított metaadatok struktúra és Oozie mentéséhez használni kívánt SQL-adatbázis megadása gombra.
    
        > [AZURE.NOTE] Metastore konfigurációs HBase fürt típusnál nem érhető el.

        ![Egyéni metastores lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.7.png "Adja meg a külső metastores")

        Metaadat **-struktúra meglévő SQL DB használata** kattintson az **Igen**gombra, jelölje be az SQL-adatbázishoz, és majd adja meg az adatbázis felhasználónevével és jelszavával. Ismételje meg ezeket a lépéseket, ha szeretne **egy meglévő SQL DB Oozie metaadatok használata**. Kattintson a **Kijelölés** gombra, amíg biztos nem lehet jelentkezzen be a **Választható beállítási** lap.

        >[AZURE.NOTE] Az Azure SQL-adatbázis a metastore használt engedélyeznie kell a kapcsolatot más Azure szolgáltatást, többek között a Azure hdinsight szolgáltatáshoz. Kattintson a jobb oldalon az Azure SQL adatbázis irányítópult a kiszolgáló nevét. Az SQL-adatbázis példány kiszolgálón. Egyszer a kiszolgáló nézetben, kattintson a **Konfigurálás**gombra, és kattintson az **Azure-szolgáltatások**, kattintson az **Igen**gombra, és kattintson a **Mentés**gombra.

        &nbsp;

        > [AZURE.IMPORTANT] Egy metastore létrehozásakor ne használjon az adatbázis nevét, amely tartalmazza a szaggatott vonal vagy kötőjeleket, mint ez leállíthatja a fürt létrehozása sikertelen lesz.

    * **Parancsfájl-műveletek** fürt testreszabása egyéni parancsfájl használatával tetszés szerint a fürt készül. Parancsfájl-műveletek kapcsolatos további tudnivalókért olvassa el a [parancsprogram művelettel testreszabása HDInsight fürt](hdinsight-hadoop-customize-cluster-linux.md)című témakört. Adja meg a részleteket a parancsfájl-műveletek lap a képernyőképet látható módon.

        ![A lap Script művelet] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.8.png "Adja meg a parancsfájlt művelet")

    * **Csatolt tároló fiókok** megadhatja a fürt társíthatja további tárterület-fiókok parancsára. Az **Azure tároló kulcsok** lap **hozzáadása egy tároló billentyűt**, és jelöljön ki egy meglévő tárterület-fiókot vagy hozzon létre egy új fiókot.

        ![További tárterület lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.CreateCluster.9.png "További tárterület-fiókok megadása")

        További tárterület fiókok felveheti a fürtre létrehozását követően is.  Lásd: [testreszabása Linux-alapú HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster-linux.md).

        Kattintson a **Kijelölés** gombra, amíg biztos nem lehet jelentkezzen be az **Új HDInsight fürt** lap.
        
        Nemcsak a Blob-tároló fiókjából is össze lehet kapcsolni Azure adatok tó tárolja. A konfiguráció lehet által konfigurálása AAD, ahol úgy állította be az alapértelmezett tároló ügyfél és az alapértelmezett tároló adatforrásból.

10. Kattintson az **Új HDInsight fürt** lap győződjön meg arról, hogy a **PIN-kód Startboard** választógombot, és kattintson a **Létrehozás**gombra. Ezzel a fürt létrehozása és hozzáadása a mozaik, az Azure portálja Startboard. Az ikon jelzi, hogy a fürt kiépítési van, és a HDInsight ikont jeleníti meg, kiépítési befejeződése után.

  	| Miközben kiépítése | Teljes kiépítése |
  	| ------------------ | --------------------- |
  	| ![Kiépítési startboard jelölője](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioning.png) | ![Kiépített fürt csempe](./media/hdinsight-hadoop-create-linux-cluster-portal/provisioned.png) |

    > [AZURE.NOTE] Egy kis időt, a fürt hozható létre, általában körülbelül 15 percet fog tartani. Ellenőrizheti a kiépítési folyamat használata a Startboard vagy a lap bal szélén lévő **értesítések** tétel a csempére.

11. Ha befejeződött a létrehozási folyamat, kattintson a Startboard a fürt lap elindítani a fürt csempéje. A fürt lap a fürt, például a nevét, az erőforrás-csoporthoz tartozik, a helyet, az operációs rendszer URL-CÍMÉT a fürt irányítópult stb alapvető információt tartalmaz.

    ![Fürt lap] (./media/hdinsight-hadoop-create-linux-cluster-portal/HDI.Cluster.Blade.png "Fürt tulajdonságai")

    Ez a lap, valamint a **Essentials** szakaszban a felső részén található ikonokra megértéséhez használja a a következő:

    * **Az összes**és **Beállítások** : jeleníti meg a **Beállítások** lap a fürt, amely lehetővé teszi a fürt részletes konfigurációs adatok eléréséhez.

    * **Irányítópult** **Fürt irányítópult**és **URL-címe**: ezek a fürt irányítópult, amely a webes portál feladat futtatható a fürt elérésének összes módjai.

    * **Biztonságos rendszerhéj**: SSH használ a fürt eléréséhez szükséges információkat.

    * **Törlése**: törli a HDInsight fürt.

    * **Quickstart útmutató** (![felhő és thunderbolt ikon quickstart útmutató =](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)): jelenít meg információt, amely segít a HDInsight használatának megkezdéséhez.

    * **Felhasználók** (![felhasználók ikon](./media/hdinsight-hadoop-create-linux-cluster-portal/users.png)): lehetővé teszi, hogy az engedélyeket az _adatkezelési portál_ a fürt más felhasználók számára az Azure-előfizetésben.

        > [AZURE.IMPORTANT] Ez _csak_ hatással van a fürt az Azure-portálon szükséges engedélyek és hozzáférés, és ki csatlakozhat, vagy a HDInsight fürt feladatok kezdeményezése nem befolyásolja.

    * **Címkék** (![címkeikon](./media/hdinsight-hadoop-create-linux-cluster-portal/tags.png)): címkék csoportban adhatja meg egy egyéni besorolás a felhőalapú szolgáltatások meghatározása kulcs/érték párokká. Például, előfordulhat, hogy hozzon létre egy __Projekt__nevű kulcsot, és kattintson egy adott projekttel kapcsolatos szolgáltatások használata a közös értéket.

##<a name="customize-clusters"></a>Fürt testreszabása

- Lásd: [testreszabása HDInsight fürt betöltő használatával](hdinsight-hadoop-customize-cluster-bootstrap.md).
- Lásd: [testreszabása Linux-alapú HDInsight fürt parancsfájl művelettel](hdinsight-hadoop-customize-cluster-linux.md).

##<a name="delete-the-cluster"></a>A csoport törlése

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight fürthöz, használja az alábbi megtudhatja, hogy miként dolgozhat a fürt:

###<a name="hadoop-clusters"></a>Hadoop fürt

* [HDInsight struktúra használata](hdinsight-use-hive.md)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight MapReduce használata](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase fürt

* [A HDInsight HBase – első lépések](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase az Java-alkalmazások fejlesztői számára](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Vihar fürt

* [Fejleszthet olyan Java topológiák vihar a hdinsight szolgáltatáshoz](hdinsight-storm-develop-java-topology.md)
* [A HDInsight vihar Python összetevők használata](hdinsight-storm-develop-python-topology.md)
* [Üzembe helyezéséhez és a HDInsight a vihar topológiák figyelése](hdinsight-storm-deploy-monitor-topology-linux.md)

###<a name="spark-clusters"></a>A külső fürt

* [Scala használatával önálló-alkalmazás létrehozása](hdinsight-apache-spark-create-standalone-application.md)
* [Feladat távolról futtatható a külső fürtre Livius használatával](hdinsight-apache-spark-livy-rest-interface.md)
* [A BI külső: interaktív adatelemzés használata a külső HDInsight az Üzletiintelligencia-eszközeiről](hdinsight-apache-spark-use-bi-tools.md)
* [A külső és gépi tanulási: a HDInsight élelmiszer vizsgálati eredmények előrejelzésére használata külső](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [A külső adatfolyam: Használata külső a HDInsight valós idejű adatfolyam alkalmazások készítéséhez](hdinsight-apache-spark-eventhub-streaming.md)
