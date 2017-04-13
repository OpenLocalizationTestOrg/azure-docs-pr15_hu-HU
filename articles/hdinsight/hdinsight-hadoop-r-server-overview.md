<properties
    pageTitle="Mi az a HDInsight R? R kiszolgálón HDInsight (előzetes verzió) – bevezetés |} Microsoft Azure"
    description="Mi az R Server (előzetes verzió) HDInsight és R-kiszolgáló használata nagy adatelemzés alkalmazások készítéséhez."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>R HDInsight-kiszolgálón áttekintése \(előzetes verzió\)

A Microsoft Azure hdinsight szolgáltatáshoz Premium Microsoft R kiszolgáló jelenleg elérhető az egy Azure hdinsight szolgáltatáshoz fürt létrehozásakor. Ezt a lehetőséget biztosít az adatok tudósok statisztikusok és igény szerinti hozzáférést méretezhető, R programozók elosztott módszerek a HDInsight analytics.

Fürt lehetnek a projektek és tevékenységek kéznél méretezett és bontva, hogy már nincs rájuk szükség. Azure hdinsight szolgáltatáshoz részét is legyenek, mivel ezek fürt beépített vállalati szintű 24/7, a 99,9 % üzemidőt egy SLA és támogatás rugalmasan integrálása az Azure ökológiai más összetevőket.

>[AZURE.NOTE] R-kiszolgáló csak HDInsight támogatás érhető el. HDInsight prémium jelenleg csak Hadoop és a külső fürt. Igen jelenleg is használhatja R kiszolgáló csak a Hadoop és a külső fürt hdinsight szolgáltatásból lehetőségre. További tudnivalókért lásd: [a másik szolgáltatáscsaládba szintek és HDInsight elérhető Hadoop-összetevő?](hdinsight-component-versioning.md).

R HDInsight-kiszolgálón nagy adathalmazok Azure Blob-tárolóhoz betöltött biztosít, R-alapú analytics a legújabb funkciókhoz. R kiszolgáló Megnyitás R épül, mivel generál R-alapú alkalmazások is kihasználhatja a 8000 + R Megnyitás csomagokat, valamint a feladatok, az ScaleR, R kiszolgáló tartalmaz a Microsoft nagy adatok analytics csomag egyikét.

Prémium fürt él csomópontjának a fürt való kapcsolódáshoz és az R parancsfájlok futtatásának kényelmes helyet biztosít. Az egy él csomópont a magmintákat a biztonsági a csomópont kiszolgálón keresztül ScaleR's parallelized elosztott függvények futó lehetőséget, ha van. Mutassa meg a fürt csomópontok közötti ScaleR's Hadoop térkép csökkentése használatával lehetősége is van, vagy külső környezetek számítja ki.

A modell vagy elemzések eredménye letölthető az előrejelzések használja a helyszíni. Azok is is lehet operationalized máshol az Azure, például az [Azure gépi tanulási Studio](http://studio.azureml.net) [webszolgáltatás](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md)keresztül.

## <a name="get-started-with-r-on-hdinsight"></a>A HDInsight R – első lépések

R kiszolgáló szerepeltetni egy HDInsight fürthöz, létre kell hoznia egy Hadoop vagy a külső fürt prémium lehetőséggel fürt létrehozásakor a Azure portál használatával. Fürt mindkettőt ugyanazt a konfigurációt, tartalmazó R kiszolgáló fürt adatok csomópontok és egy él csomópontot, R kiszolgálóalapú elemzéséhez érkezési zónán használja. [R HDInsight-kiszolgálón – első lépések](hdinsight-hadoop-r-server-get-started.md) talál egy fürt létrehozásával kapcsolatos részletes segédlet.

## <a name="learn-about-data-storage-options"></a>Adattárolási beállításainak ismertetése

Alapértelmezett tárterület HDInsight fürt megfeleltetve blob-tárolóhoz Fájlrendszerhez fájl rendszerrel Blob-tárolóhoz. Biztosan megtörténjen az, hogy bármilyen adat töltenek fel a fürt tárhely, vagy írt fürt tárterület-elemzés során állandó végeznek. A [AzCopy](../storage/storage-use-azcopy.md) segédprogrammal másolhatja az adatokat, és a blob onnan.

A Blob-tárolóhoz, mellett van az [Azure tó adattárolás](https://azure.microsoft.com/services/data-lake-store/) használata a fürt választógombot. Adatok tó használata esetén majd is használhatja Blob-tárolóhoz és a adatok tó Fájlrendszerhez tárhelyként használható.

[Azure fájlok](../storage/storage-how-to-use-files-linux.md) tároló lehetőségként az él csomóponton használatra is használhatja. Azure fájlok lehetővé teszi a fájl Linux rendszerhez készült Azure-tárolóban lévő fájlmegosztás csatlakoztatni. További információt a HDInsight fürthöz adattárolási beállításainak R Server [HDInsight fürt R kiszolgálón tárolási lehetőségek](hdinsight-hadoop-r-server-storage.md)találhatók.

## <a name="access-r-server-on-the-cluster"></a>A fürt Access R-kiszolgálón

Miután létrehozta a fürt R-kiszolgálóval, csatlakozhat az R konzol a SSH/gitt keresztül a fürt él csomópontra. Is csatlakozhat böngészőn keresztül, ha úgy dönt, hogy a választható RStudio kiszolgáló IDE telepítése a szegély csomópontot. RStudio kiszolgáló telepítésével kapcsolatos további tudnivalókért olvassa el a [HDInsight fürt RStudio Server telepítése](hdinsight-hadoop-r-server-install-r-studio.md)című témakört.   

## <a name="develop-and-run-r-scripts"></a>Kidolgozása és R parancsfájlok futtatása

Az R parancsprogramok alapján elkészítheti, létrehozása és futtatása bármelyikét használhatja a parallelized és elosztott eljárások mellett 8000 + R Megnyitás csomagok a ScaleR tárban. Az általános parancsfájl, a szegély csomópontra R kiszolgálón futó fut, az R értelmező belül csomópontra. A kivétel ez alól ezek a lépések, amelyek egy ScaleR hívás működik, ha egy számítási helyi ennek már jó beállítása Hadoop térkép csökkentése (RxHadoopMR) vagy külső (RxSpark).

Ebben az esetben a függvény futtatja elosztott módon adatai között (tevékenység) csomópontot, a hivatkozott adatok társított a fürt. Többet szeretne tudni a különböző számítási környezeti beállításokat olvassa el a [számítja ki a környezet beállításainak HDInsight prémium R-kiszolgálón](hdinsight-hadoop-r-server-compute-contexts.md)című témakört.

## <a name="operationalize-a-model"></a>A modell üzemeltető

Amikor befejeződött a adatmodellezés, meg is üzemeltető a modellt, hogy az új adatokat, mind az Azure és a helyszíni az előrejelzések. Ez a folyamat pontozási nevezik. Íme néhány példa.

### <a name="score-in-hdinsight"></a>A HDInsight pontszámhoz

A HDInsight pontszám írja be az R függvény, amely hív meg, hogy a tárterület-fiók betöltött új adatfájl az előrejelzések a modell. Mentse az előrejelzések vissza a tárterület-fiókot. A szokásos igény szerinti futtatását is lehetővé teszi a szegély csomópontra a fürt vagy ütemezett feladat használatával.  

### <a name="score-in-azure-machine-learning"></a>Az Azure gépi tanulási pontszámhoz

Pontszám az Azure gépi tanulási webszolgáltatás használatával, a Megnyitás Azure gépi tanulási R csomag [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) neve segítségével a modell, mint az Azure webszolgáltatás közzététele. Az egyszerűség kedvéért a csomag előre telepítve van a a szegély csomópontot. Ezután a gépi tanulási létesítményeit igénybe a webszolgáltatás felhasználói felület, és akkor a webszolgáltatás hívja meg a pontozási szükség szerint.

Ha ezt a beállítást választja, kell ScaleR modell objektumok átalakítása egyenértékű Megnyitás-forrás modell objektumok a webes szolgáltatással való használatra. Ezt megteheti való ScaleR kényszerítés funkciókat, például: `as.randomForest()` ensemble-alapú modellekhez.


### <a name="score-on-premises"></a>Helyszíni pontszám

Pontszám a helyszíni a modell létrehozását követően, hogy szerializálni R a modellt, töltse le, akkor vonja szerializálni és használja azt az új adatok pontozási. Új adatokat is pontszám, a korábbi részében [pontozás a HDInsight](#scoring-in-hdinsight) ismertetett megközelítés vagy [DeployR](https://deployr.revolutionanalytics.com/)használatával.

## <a name="maintain-the-cluster"></a>A fürt kezelése

### <a name="install-and-maintain-r-packages"></a>Telepítse és R csomagok kezelése

Az Ön által használt R-csomagok a legtöbb kötelező a szegély csomópontra, mivel az R parancsfájlok a legtöbb nem futtathatók. További R csomagok telepítése a szegély csomópontot, a szokásos módon használható `install.packages()` j módszer

A legtöbb esetben nem kell további R csomagok telepítése az adatok csomópontok alkalmazás használatakor csak eljárások a ScaleR tárból a fürt keresztül. További csomagok támogatja **rxExec** vagy **RxDataStep** végrehajtási adatok csomóponton előfordulhat, hogy szüksége.

Ebben az esetben a további csomagokat meg kell adni egy parancsprogramot művelet segítségével a fürt létrehozása után. További tudnivalókért lásd: a [egy HDInsight-R-kiszolgálóval fürt létrehozása](hdinsight-hadoop-r-server-get-started.md)elemre.   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Hadoop térkép csökkentése memória beállításainak módosítása

A fürtre módosíthatja a rendelkezésre álló R-kiszolgálóhoz, amikor a térkép csökkentése feladat fut memória módosítható. A fürtre módosításához használja a Apache Ambari felhasználói Felülettel, amely a fürt Azure portál lap keresztül érhető el. Hogyan érhető el a Ambari felhasználói felületének a fürt, című [kezelése HDInsight fürt Ambari webes a felhasználói felület használatával](hdinsight-hadoop-manage-ambari.md).

Lehetőség arra is használatával Hadoop kapcsolók **RxHadoopMR** a hívást a következőképpen R-kiszolgálóra rendelkezésre álló memória módosítása:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>A fürt méretezése

Egy meglévő fürthöz megadhat felfelé vagy lefelé a portálon keresztül. Beosztását, kaphatnak a további kapacitása ahhoz, hogy szükség lehet nagyobb feldolgozási tevékenység vagy méretezheti vissza fürt tétlen állapotában. Arról, hogy miként méretezheti fürt útmutatásért lásd: [fürt kezelése hdinsight szolgáltatásból lehetőségre](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>A rendszer karbantartása

Karbantartási OS javítások és frissítések munkaidőn kívüli során az alapul szolgáló Linux VMs HDInsight fürt történik. Általában karbantartási befejeződött a 3:30 de (alapján a helyi idővé a virtuális) minden hétfő és csütörtök. Frissítések oly módon, hogy azok ne hatással lehet a fürt egyszerre több, mint egy negyedéve megtörténik.  

A központi csomópontok felesleges, és nem az összes adat csomópontok hatással vannak, mivel minden megadott időszakban futó feladatok lassíthatja. Kell futnak száma, akkor jó helyen jár. Minden olyan egyéni szoftver vagy a helyi adatok, amelyekhez megmarad át az alábbi karbantartási események kivéve, ha egy Katasztrofális hiba lép fel, amely igényel egy fürt újraépítő.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>További tudnivalók: IDE egy HDInsight fürt R kiszolgáló lehetőségei

A Linux él csomópont HDInsight prémium fürt a érkezési zóna R-alapú elemzéshez. A fürthöz csatlakozás után indítsa el a konzol felületet R kiszolgáló Linux a parancssorba írja be a **R** . A konzol felületének használata van továbbfejlesztett, ha a k parancsfájl fejlesztési szövegszerkesztőben egy másik ablakban, és kivágása és szakaszok a parancsprogram beillesztése a R konzol szükség szerint.

A bonyolultabb a R parancsfájl fejlesztésének eszköze az R-alapú IDE használata az asztalon, például a Microsoft nemrégiben bejelentve [R Tools for Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS). Az asztal- és eszközök [RStudio](https://www.rstudio.com/products/rstudio-server/)családját. Walware's Holdas-alapú [StatET](http://www.walware.de/goto/statet)is használhatja.

Egy másik, hogy egy IDE telepítése a csomóponton Linux él magát.  A népszerű választás [RStudio kiszolgáló](https://www.rstudio.com/products/rstudio-server/), amely tartalmaz egy böngészőalapú IDE távoli ügyfelek számára. Teljes IDE felületet RStudio Server telepítése HDInsight prémium fürt él csomóponton nyújt a fejlesztés és a fürt R-kiszolgálóval R parancsfájlok végrehajtása. Ez lehet jelentős mértékben hatékonyabb, mint az R konzol.  Ha azt szeretné, RStudio kiszolgálót használ, akkor olvassa el [A HDInsight fürt RStudio Server telepítése](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Árak ismertetése

A díjak egy HDInsight prémium fürthöz R-kiszolgálóval társított hasonlóan a szabványos HDInsight fürt díjakkal szerkezete. Azok a nevét, az adatok és a szegély csomópontok prémium verzióban a core órás uplift hozzáadásával végig az alapul szolgáló VMs méretezését alapulnak. További információt a HDInsight prémium árak, beleértve a Public Preview és a 30 napig ingyenes próbaverziója elérhetőségét során árak lásd: [HDInsight árak](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Következő lépések

Hajtsa végre az alábbi hivatkozásokra kattintva további információ arról, hogy miként R-kiszolgáló használata fürt hdinsight szolgáltatásból lehetőségre.

- [Első lépések – R HDInsight-kiszolgálón](hdinsight-hadoop-r-server-get-started.md)

- [Adja hozzá a RStudio kiszolgálót HDInsight-támogatás](hdinsight-hadoop-r-server-install-r-studio.md)

- [Helyi beállításai R Server (előzetes verzió) HDInsight kiszámítása](hdinsight-hadoop-r-server-compute-contexts.md)

- [HDInsight prémium R Server Azure tárhely beállításai](hdinsight-hadoop-r-server-storage.md)
