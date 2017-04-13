<properties
    pageTitle="Kibocsátási megjegyzések az Azure hdinsight szolgáltatáshoz a Hadoop-összetevők |} Microsoft Azure"
    description="Legújabb kibocsátási megjegyzések és Azure hdinsight szolgáltatáshoz a Hadoop-összetevők verzióit. Tippek a fejlesztés és a részletek beszerzése Hadoop Apache vihar és HBase."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Kibocsátási megjegyzések az Azure hdinsight szolgáltatáshoz a Hadoop-összetevők

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Jegyzetek 10/26/2016 kiadását HDInsight R-kiszolgáló

- R HDInsight fürt kiépítési kiszolgálóhoz végzett finomítások révén.
- R HDInsight-kiszolgálón érhető el, a rendszeres HDInsight "R kiszolgálója" fürt típusát, és már nem egy külön HDInsight-alkalmazás van telepítve. A biztonsági a csomópont és R kiszolgáló bináris most már kiépítve az R kiszolgáló fürt telepítés részeként. Ez javítja a sebességétől és megbízhatóságára kiépítési. R Server árak modell ennek megfelelően frissül.
- R-kiszolgáló fürt típusú ár most szabványos réteg ár, valamint az R-kiszolgálót, emelt díjas ár alapján. Prémium réteg keresztül típusai eltérő fürt most már elérhető a prémium funkció fenntartott, és az R kiszolgálótípus fürt nem fogja használni. Ez a módosítás nem befolyásolja a hatékony árak R-kiszolgáló, csak változik hogyan jelennek meg a számla a költségek. Minden meglévő R kiszolgáló fürt továbbra is működnek, és ARM sablonok továbbra is szerepelni fognak működni addig, amíg a kapcsolójáról értesítés. **Ajánlott, ha frissíti a parancsfájl telepítések új ARM sablon használatához.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Jegyzetek 08/30/2016 kiadását HDInsight R-kiszolgáló

Ebben a kiadásban üzembe helyezéséhez Linux-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához   |Ambari összeállításához |
|----|----------------------|----|------------|-------------|
|a 3,2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3. |3.3.1000.0.8268980    |2.3. |2.3.3.1-25  |2.2.1.12-4   |
|3.4. |3.4.1000.0.8269383    |2.4. |2.4.2.4-5   |2.2.1.12-4   |

Ebben a kiadásban a telepített Windows-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |az 1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0-s verziója |2.0.13.0-2117 |
|3.1. |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|a 3,2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3. |3.3.0.1033.2559206    |2.3. |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Jegyzetek 08/17/2016 kiadását HDInsight R-kiszolgáló

- R Server 8.0.5 – főként hibajavítás végleges verzió. További információk a [R kiszolgáló kibocsátási megjegyzések](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) témakörben talál. 
- A szegély csomópontra – [a R csomag](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) AzureML csomag lehetővé teszi, hogy az R modellek közzé, és az Azure Machine Learning webszolgáltatás felhasznált.  Olvassa el a ["áttekintése az R kiszolgáló a HDInsight"](hdinsight-hadoop-r-server-overview.md) a cikk további információt a ["a modell üzemeltető"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) szakaszát.
- A [legfelső 100 legnépszerűbb R csomagok](https://github.com/metacran/cranlogs) – ezek csomag függőségek most előre telepítve Linux függőségek Linux rendszerhez.  
- A CRAN repó használni, ha az adatok csomópontjainak csomagok R hozzáadása lehetőséget. Olvassa el az ["Első lépések a HDInsight-R kiszolgálóval"](hdinsight-hadoop-r-server-get-started.md) cikk további információt a ["Telepítés R csomagok"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) szakaszát.
- Továbbfejlesztett R Server kiépítési fürt létrehozásakor megbízhatóságát.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Jegyzetek 08/01/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez Linux-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához   |Ambari összeállításához |
|----|----------------------|----|------------|-------------|
|a 3,2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3. |3.3.1000.0.8028416    |2.3. |2.3.3.1-25  |2.2.1.12-4   |
|3.4. |3.4.1000.0.8053402    |2.4. |2.4.2.4-5   |2.2.1.12-4   |

Ebben a kiadásban a telepített Windows-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |az 1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0-s verziója |2.0.13.0-2117 |
|3.1. |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|a 3,2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3. |3.3.0.1005.2488842    |2.3. |2.3.3.1-25    |

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például külső, Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight 3.4 fürt módosítása | Az alapértelmezett érték, a következő struktúra konfigurációk megváltoznak a teljesítmény növelése érdekében <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Szolgáltatás    | Az összes| A #HIÁNYZIK|
| Ebben a kiadásban alábbi javításokat tartalmaz | STRUKTÚRA-13632, HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Szolgáltatás    | Az összes| A #HIÁNYZIK

## <a name="notes-for-07142016-release-of-hdinsight"></a>Jegyzetek 07/14/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez Linux-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához   |Ambari összeállításához |
|----|----------------------|----|------------|-------------|
|a 3,2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3. |3.3.1000.0.7932505    |2.3. |2.3.3.1-18  |2.2.1.12-2   |
|3.4. |3.4.1000.0.7933003    |2.4. |2.4.2.0     |2.2.1.12-2   |

Ebben a kiadásban a telepített Windows-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |az 1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0-s verziója |2.0.13.0-2117 |
|3.1. |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|a 3,2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3. |3.3.0.989.2441725     |2.3. |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Jegyzetek 07/07/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez Linux-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához   |
|----|----------------------|----|------------|
|a 3,2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3. |3.3.1000.0.7864996    |2.3. |2.3.3.1-18  |
|3.4. |3.4.1000.0.7861906    |2.4. |2.4.2.0     |

Ebben a kiadásban a telepített Windows-alapú HDInsight fürt teljes verziószámai:

|HDI |HDI fürt verziója   |HDP |HDP összeállításához     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |az 1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0-s verziója |2.0.13.0-2117 |
|3.1. |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|a 3,2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3. |3.3.0.977.2413853     |2.3. |2.3.3.1-21    |

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például külső, Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [IntelliJ HDInsight eszközök](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ arról beépülő modul HDInsight külső fürt most integrálva van Azure eszközkészlet IntelliJ számára. Támogatja az Azure SDK v2.9.1, legújabb Java SDK, és a az önálló HDInsight beépülő modul IntelliJ összes funkcióját tartalmazza.| Eszközök    | A külső| A #HIÁNYZIK|
| [Holdas HDInsight eszközök](hdinsight-apache-spark-eclipse-tool-plugin.md) | Holdas eszközkészlete Azure hdinsight szolgáltatáshoz a külső fürt támogatja. Az alábbi szolgáltatások lehetővé teszi. <ul><li>Hozzon létre, és írja be a külső alkalmazások egyszerűen Scala és Java az első osztályjegyzetfüzet szerzői támogatás az IntelliSense, az automatikus formázás, a Hibaellenőrzés stb.</li><li>Tesztelje a külső alkalmazás helyi meghajtóra.</li><li>Feladatok HDInsight külső fürthöz nyújt, és az eredmények beolvasásához.</li><li>Jelentkezzen be az Azure, és a hozzáférési az Azure előfizetések társított minden külső fürt.</li><li>Nyissa meg a HDInsight külső fürt társított tároló-erőforrások.</li></ul>| Eszközök    | A külső| A #HIÁNYZIK

Ebben a kiadásban kezdve módosítottuk a vendégként való bekapcsolódáshoz OS javítási házirend Linux-alapú HDInsight fürtre vonatkozóan. Az új házirend a cél, jelentősen csökkenti a javítási miatt újraindul számát. Az új házirend Linux fürt minden hétfő vagy csütörtök, 12 óra UTC eltolt módon keresztül bármely adott fürt található csomópontok kezdve folytatja javítás virtuális gépeken futó (VMs). Minden megadott virtuális azonban csak újraindul legfeljebb 30 naponta egyszer Vendég OS javítása miatt. Ezeken kívül az újonnan létrehozott fürt első újraindítás nem történik hamarabb 30 nap fürt létrehozását.

>[AZURE.NOTE] A változtatások csak újonnan létrehozott fürt egyenlő vagy nagyobb, mint ezen verziójában vonatkozik.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Jegyzetek 06/06/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

|HDP    |HDI verziója    |A külső verziója  |Ambari Build száma    |HDP Build száma|
|-------|---------------|---------------|-----------------------|----------------|
|2.3.    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4.    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például külső, Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Általában érhető el a HDInsight külső | Ebben a kiadásban javítása az elérhetőség, méretezhetőség és termelékenység kattintva nyissa meg a HDInsight Apache külső forrás elérhetővé teszi. <ul><li>Kezdő nullák elérhetősége SLA, amely lehetővé teszi a vállalati munkaterhelésekből igénylő alkalmas 99,9 %-os iparágban.</li><li>Azure adatok tó tárolót használó méretezhető tároló réteg.</li><li>Hatékonyság eszközök az adatok feltárása és fejlesztési minden szakaszát. Testre szabott külső kernel Jupyter jegyzetfüzetek engedélyezése interaktív adatfeltáró, integráció a BI irányítópultok, például a Power BI, Tableau és Qlik kiválóan alkalmas a fontos adatok megosztása és a folyamatos jelentéskészítés, IntelliJ beépülő modul hosszú távú kód eltérés fejlesztés és hibakeresés megbízható beállítás.</li></ul>| Szolgáltatás    | A külső| A #HIÁNYZIK|
| IntelliJ HDInsight eszközök | Ez a egy IntelliJ arról beépülő modul HDInsight külső fürtre vonatkozóan. Az alábbi szolgáltatások lehetővé teszi.<ul><li>Hozzon létre, és írja be a külső alkalmazások egyszerűen Scala és Java az első osztályjegyzetfüzet szerzői támogatás az IntelliSense, az automatikus formázás, a Hibaellenőrzés stb.</li><li>Tesztelje a külső alkalmazás helyi meghajtóra.</li><li>Feladatok HDInsight külső fürthöz nyújt, és az eredmények beolvasásához.</li><li>Jelentkezzen be az Azure, és a hozzáférési az Azure előfizetések társított minden külső fürt.</li><li>Nyissa meg a HDInsight külső fürt társított tároló-erőforrások.</li><li>Nyissa meg a feladatok előzmények és a projekt adatait a HDInsight külső fürt.</li><li>A külső feladatok távoli asztali számítógépéről hibakeresési.</li></ul>| Eszközök    | A külső| A #HIÁNYZIK

## <a name="notes-for-05132016-release-of-hdinsight"></a>Jegyzetek 05/13/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.922.2266903 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például külső, Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Amelyek verziófrissítés és más hibajavítás | Ebben a kiadásban frissítések, a külső verzió HDInsight fürt 1.6.1 és más hibákat javítások| Szolgáltatás    | A külső| A #HIÁNYZIK

## <a name="notes-for-04112016-release-of-hdinsight"></a>Jegyzetek 04/11/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.889.2191206 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.889.2191206 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.889.2191206 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* (Windows) 3.3.0.889.2191206 HDInsight (HDP 2.3.3.1-16-nem változik)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Egyéni metastore frissítés problémák – HDI 3.4. | Ha egy egyéni metastore, és egy másik HDInsight fürt alacsonyabb verzióján használta korábban fürt létrehozása meghiúsul. Ez egy parancsprogramot hiba a frissítéskor most javított miatt volt| Csoport létrehozása    | Az összes | A #HIÁNYZIK
| Livius összeomlik helyreállítás | Projekt állapota tűrőképessége nyújt a bármely Livius keresztül küldött feladat | Megbízhatósága | A külső Linux| A #HIÁNYZIK
| HA Jupyter tartalom | Lehetővé teszi a mentése és betöltése Jupyter jegyzetfüzet tartalma és a fürthöz társított tárterület-fiókból. További tudnivalókért lásd: [mag Jupyter jegyzetfüzetek érhető el](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Jegyzetfüzetek | A külső Linux| A #HIÁNYZIK
| A jegyzetfüzetek Jupter hiveContext eltávolítása | Használat `%%sql` színű helyett `%%hive` mágikus. SqlContext hiveContext megegyezik. További tudnivalókért lásd: [mag Jupyter jegyzetfüzetek érhető el](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Jegyzetfüzetek    | A külső fürt Linux| A #HIÁNYZIK
| Régebbi külső kapcsolójáról | A szolgáltatás régebbi verzióját külső 1.3.1 kikerül a 5/31. | Szolgáltatás | A külső fürt Windows rendszeren | A #HIÁNYZIK

## <a name="notes-for-03292016-release-of-hdinsight"></a>Jegyzetek 03/29/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - változatlan marad)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - változatlan marad)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - változatlan marad)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| A felvett HDInsight 3.4 frissített HDP verziójához és az összes HDInsight fürtre vonatkozóan | Az ebben a kiadásban hogy hozzáadta a HDInsight v3.4 (HDP 2.4 alapján), és is megfelelően frissül más HDP verziói. HDP 2.4 kibocsátási megjegyzések az, hogy érhető el [az alábbi](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) és verziók lehet HDInsight további információt a megtalált [Itt](hdinsight-component-versioning.md).| Szolgáltatás    | Minden Linux fürt| A #HIÁNYZIK
| HDInsight-támogatás | HDInsight már két kategóriába - normál és támogatás érhető el. HDInsight prémium jelenleg előzetes verzióban, és csak a Hadoop használható, és a külső fürtök Linux. További információt talál [az alábbi](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Szolgáltatás    | Hadoop és a külső Linux| A #HIÁNYZIK
| A Microsoft-R-kiszolgáló | HDInsight prémium Microsoft R Server Linux Hadoop és a külső fürt beilleszthető biztosít. További információ: [Áttekintés az R-kiszolgálón hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-r-server-overview.md).| Szolgáltatás    | Hadoop és a külső Linux| A #HIÁNYZIK
| A külső 1.6.0 | HDInsight 3.4 fürt most már tartalmazza a külső 1.6.0| Szolgáltatás    | A külső fürt Linux| A #HIÁNYZIK
| Jupyter jegyzetfüzet kapcsolatos fejlesztések | Külső fürt elérhető Jupyter jegyzetfüzetek mag most nyújt további külső. Is, például felhasználása kapcsolatos fejlesztések %% mágikus, automatikus-ábrázoló és integráció a Python képi megjelenítés tárak (például matplotlib). További tudnivalókért lásd: [mag Jupyter jegyzetfüzetek érhető el](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Szolgáltatás | A külső fürt Linux | A #HIÁNYZIK

## <a name="notes-for-03222016-release-of-hdinsight"></a>Jegyzetek 03/22/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.875.2159884 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.875.2159884 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.875.2159884 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.875.2159884 (HDP 2.2.9.1-7 - változatlan marad)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (HDP 2.2.9.1-8 - változatlan marad)
* HDInsight (Linux) 3.3.1000.0.7193255 (HDP 2.3.3.1-7 - változatlan marad)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Minden HDInsight fürt frissített HDInsight-verziókban | Az ebben a kiadásban frissítettük minden HDInsight fürt HDInsight-verziókkal| Szolgáltatás    | Az összes| A #HIÁNYZIK


## <a name="notes-for-03102016-release-of-hdinsight"></a>Jegyzetek 03/10/2016 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.859.2123216 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.859.2123216 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.859.2123216 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (HDP 2.3.3.1-5 - változatlan marad)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Minden HDInsight fürt frissített HDInsight-verziókban | Az ebben a kiadásban frissítettük minden HDInsight fürt HDInsight-verziókkal| Szolgáltatás    | Az összes| A #HIÁNYZIK

## <a name="notes-for-01272016-release-of-hdinsight"></a>Jegyzetek HDInsight kiadásához 01/27-es és 2016-ban

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.817.2028315 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.817.2028315 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.817.2028315 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (HDP 2.3.3.1-5 - változatlan marad)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Minden HDInsight fürt frissített HDInsight-verziókban | Az ebben a kiadásban frissítettük minden HDInsight fürt HDInsight-verziókkal| Szolgáltatás    | Az összes| A #HIÁNYZIK

## <a name="notes-for-12022015-release-of-hdinsight"></a>Megjegyzések a 12/02/2015 kiadását hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.763.1931434 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.763.1931434 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.763.1931434 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.763.1931434 (HDP 2.2.7.1-34 - változatlan marad)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34 - változatlan marad)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| A felvett HDInsight 3.3 frissített HDP verziójához és az összes HDInsight fürtre vonatkozóan | Az ebben a kiadásban hogy hozzáadta a HDInsight v3.3 (HDP 2.3 alapján), és is megfelelően frissül más HDP verziói. HDP 2.3 kibocsátási megjegyzések az, hogy érhető el [az alábbi](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) és verziók lehet HDInsight további információt a megtalált [Itt](hdinsight-component-versioning.md).| Szolgáltatás    | Az összes| A #HIÁNYZIK

## <a name="notes-for-11302015-release-of-hdinsight"></a>Jegyzetek 11/30/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.757.1923908 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.757.1923908 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.757.1923908 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Az összes HDInsight fürt és HDP verziók (Windows és Linux) HDInsight-3,2 fürtre vonatkozóan frissített HDInsight-verziók | Az ebben a kiadásban a HDInsight-és HDP frissített | Szolgáltatás    | Az összes| A #HIÁNYZIK


## <a name="notes-for-10272015-release-of-hdinsight"></a>Megjegyzések HDInsight 10/27/2015 kiadását az

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight (Windows) 2.1.10.726.1866228 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight (Windows) 3.0.6.726.1866228 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight (Windows) 3.1.4.726.1866228 (HDP 2.1.15.0-2374 - változatlan marad)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Frissített HDInsight verzióinak minden HDInsight fürt (Windows és Linux) | Az ebben a kiadásban a HDInsight-és HDP frissített | Szolgáltatás    | Az összes| A #HIÁNYZIK
| A nagybetűs fürt rögzített Jupyter a Windows külső fürt | Fürt, amely a DNS-nevek nagybetűkkel megadott kellett volna problémák miatt az origin kérelem jelölőnégyzet Jupyter jegyzetfüzeteket. A fix Jupyter's konfiguráció a tartománynév módosítása kisbetűsre volt. | Szolgáltatás    | HDInsight külső (Windows)| A #HIÁNYZIK


## <a name="notes-for-10202015-release-of-hdinsight"></a>Megjegyzések HDInsight 10/20/2015 kiadását az

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Alapértelmezett HDP verzió HDP 2.2 módosítva | Az alapértelmezett verzióra HDInsight Windows fürtre vonatkozóan HDP 2.2 változik. 3,2 (HDP 2.2) verzió HDInsight mióta általában elérhető február 2015. Ez a változás csak az alapértelmezett fürt verzió tükrözi, amikor egy explicit kijelölés nem történt a fürt az Azure-portálra, PowerShell-parancsmagok vagy a SDK kiépítése során. | Szolgáltatás    | Az összes| A #HIÁNYZIK                  |
|Virtuális név formátuma az üzembe helyezése a Linux fürt egyetlen virtuális hálózatban több HDInsight változásai | Ebben a verzióban hozzáadott üzembe helyezése a virtuális egyetlen hálózat több HDInsight Linux fürtre támogatása. Az adott részeként a fürt a virtuális gép tartozó nevek formátumának megváltozott headnode\*, workernode\* és zookeepernode\* hn való\*, wn\*, és zk\* rendre. Még nem javasolt felvegye közvetlen függőség formátumának virtuális gép nevek, mivel ez változhatnak. Használjon "hostname (állomásnév) -f" a helyi számítógép vagy Ambari API-khoz határozza meg a hosts és a hosts összetevők megfeleltetésének listáját. További információ a [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) és [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md)is megkeresheti. | Szolgáltatás | Linux HDInsight fürt | A #HIÁNYZIK |
| Beállítások módosítása | HDInsight 3.1 fürt az alábbi beállításokat ekkor engedélyezve vannak: <ul><li>tez.yarn.ATS.Enabled és yarn.log.server.url. Ezzel a alkalmazás ütemterv és a napló kiszolgáló láthatja el a naplók szolgálnak.</li></ul>A HDInsight-3,2 fürt az alábbi beállításokat módosultak: <ul><li>2 mapreduce.fileoutputcommitter.Algorithm.Version van beállítva. Ezzel a FileOutputCommitter V2 verziójának használata.</li></ul> | Szolgáltatás | Az összes | A #HIÁNYZIK |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Jegyzetek 09/09/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.675.1768697 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.675.1768697 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.675.1768697 (HDP 2.1.15.0-2334 - változatlan marad)
* HDInsight 3.2.6.675.1768697 (HDP 2.2.6.1-0012 - változatlan marad)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Minden HDInsight fürt frissített HDInsight-verziókban | Az ebben a kiadásban HDInsight-verziók frissítése | Szolgáltatás    | Az összes| A #HIÁNYZIK                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Jegyzetek 07/31/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.640.1695824 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.640.1695824 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.640.1695824 (HDP 2.1.15.0-2334 - változatlan marad)
* HDInsight 3.2.6.640.1695824 (HDP 2.2.6.1-0012 - változatlan marad)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| A külső fürt csomópont újra Képkezelő munkafolyamat kijavítása | Rögzített egy hiba, okozta külső fürt csomópontok nem után újra a kép visszaállítása | Szolgáltatás    | A külső| A #HIÁNYZIK                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Jegyzetek 07/31/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.635.1684502 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.635.1684502 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.635.1684502 (HDP 2.1.15.0-2334 - változatlan marad)
* HDInsight 3.2.6.635.1684502 (HDP 2.2.6.1-0012 - változatlan marad)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Minden HDInsight fürt frissített HDInsight-verziókban | Az ebben a kiadásban HDInsight-verziók frissítése | Szolgáltatás    | Az összes| A #HIÁNYZIK                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Jegyzetek 07/07/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.610.1630216 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.610.1630216 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.610.1630216 (HDP 2.1.15.0-2334 - változatlan marad)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

| Cím                                           | Leírás                                          | Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK) | Fürt típusa (például Hadoop, HBase vagy vihar) | JIRA (ha van) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Frissített HDP verziók HDInsight-3,2 fürtre vonatkozóan | Az ebben a kiadásban HDInsight-3,2 HDP 2.2.6.1-0012 üzembe helyezése | Szolgáltatás    | Az összes                                                 | A #HIÁNYZIK                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Jegyzetek 06/26/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.601.1610731 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.601.1610731 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.601.1610731 (HDP 2.1.15.0-2334 - változatlan marad)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Frissített HDP verziók HDInsight-3,2 fürtre vonatkozóan</td>
<td>Az ebben a kiadásban HDInsight-3,2 HDP 2.2.6.1 üzembe helyezése</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Jegyzetek 06/18/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.596.1601657 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.596.1601657 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>További megnyitott HTTPS-portok</td>
<td>A felhőbeli szolgáltatástól nyitja meg a fürt pl. 8001 való 8005 5 portok a https://<clustername>.azurehdinsight.net:8001/. Az alábbi URL-kérelmek hitelesítése ugyanez alapszintű hitelesítés jelszavát a rendszer a 443-as port használják. Az aktív headnode azonos portjához portokhoz kötést létrehozni. Parancsfájl-műveletek, hogy a headnode és a fürt kívüli útvonalat az portok meghallgatása ügyfélszolgálat használható.</td>
<td>Felhőalapú szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>HDInsight-3,2 szakaszos MapReduce sorrendű probléma</td>
<td>A térkép csökkentése sorrendű alkalmi tevékenység hibák így nagy fürt a ritka, időnként fajta feltétel megoldása. <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a> talál további információt.</td>
<td>Hadoop Core</td>
<td>Az összes</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Ugrás a legújabb Azure Java HDInsight 3,2 2.2 SDK</td>
<td>Áthelyezve az WASB illesztőprogram által használt Java az Azure SDK legújabb verzióját. A legújabb SDK csomagjában talál néhány javításokat tartalmaz, és a kibocsátási megjegyzések az azonos érhetők el a https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoop Core</td>
<td>Az összes</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP-11959</a></td>
</tr>

<tr>
<td>Ugrás a HDP 2.1.15 HDInsight 3.1 fürtre vonatkozóan</td>
<td>Kibocsátási megjegyzések Hortonworks verzióval érhetők el <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">az alábbi</a>.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Jegyzetek 06/04/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.583.1575584 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.583.1575584 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.583.1575584 (HDP 2.1.12.1-0003 - változatlan marad)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Vihar fürtre vonatkozóan 502 Hibás átjáró hibák kijavítása</td>
<td>Ebben a kiadásban megoldja a feladat elküldése API-val és a számítógép újraindítása után nem működik a webhelyén okozó érintő hiba.</td>
<td>Szolgáltatás</td>
<td>Vihar</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Jegyzetek 06/01/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.577.1563827 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.577.1563827 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.577.1563827 (HDP 2.1.12.1-0003 - változatlan marad))
* HDInsight 3.2.4.577.1563827 (HDP 2.2.6.0-2800 - változatlan marad)
* SDK 1.5.8


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Számos hibajavítás</td>
<td>Ebben a kiadásban javítások fürt kiépítési kapcsolatos hibákat.</td>
<td>Szolgáltatás</td>
<td>Diagramtípusokat fürthöz</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Jegyzetek 05/27/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Más fürt verziókat és a SDK részeként ebben a kiadásban nincs telepítve.


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>HDP 2.2 frissítése</td>
<td>Ebben a verzióban a HDInsight-3,2 HDP 2.2.6 tartalmazza, és több fontos hibajavítás életre hdinsight szolgáltatásból lehetőségre. A teljes kibocsátási megjegyzések címen érhető el <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 engedje fel az jegyzetek</a>.</td>
<td>HDP</td>
<td>Diagramtípusokat fürthöz</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Alapértelmezett fonal tároló memória konfiguráció módosítása</td>
<td>A frissítés az alapértelmezett rendelkezésre álló memória fonal tárolók (yarn.nodemanager.resource.memory MB-os és yarn.scheduler.maximum-terhelés-mb), hogy indított által csomópont-kezelő 5632 MB-ra nő. Korábban meg volt 4608 MB csökkentett, de a különböző feladat futtatása alapján, az új értéket kell kínálnak jobban megbízhatóságának és teljesítményének a legtöbb feladatokat, így jobban alapértelmezett. Szokásos, ha Ön egy kritikus függőség rendelkezik a memória konfigurációja, állítsa be kifejezetten azt a fürt létrehozásakor.</td>
<td>HDP</td>
<td>Diagramtípusokat fürthöz</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Alapértelmezett Config eltérés áll fenn HBase és vihar fürtre vonatkozóan</td>
<td>A frissítés visszaállítja az értékeket a fonal konfigurációk tartományként Hadoop fürt Hbase és vihar fürt. Ez történik az eltérés áll fenn végig a diagramtípusokat fürt.</td>
<td>HDP</td>
<td>HBase, vihar</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Jegyzetek 05/20/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.564.1542093 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.564.1542093 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>SCP.NET EventHub támogatás</td>
<td>A frissített fürt csomagjai HDInsight vihar előtérbe SCP.NET új szolgáltatásokat. Új API-hoz a topológia használatával létrehozott, így azok könnyebben EventHubSpout vagy Java Spouts hozzáférést most lesz. A SCP.NET ügyfélprogram új fürt végezhető, mint a szerződéseket a frissített SDK frissítenie kell. Az új API-hoz, használatát és megjelenés jegyzeteket (beleértve a hibajavítás) kapcsolatos részletekért olvassa el a fontos a SCP.NET nuget csomagban.</td>
<td>Szerszámok VIEWBEN</td>
<td>A 3,2 HDInsight fürt vihar</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>JDBC illesztőprogram frissítése</td>
<td>A vezető sqljdbc_4.1.5605.100 a támogatott SQL Server-verzióra frissíteni.</td>
<td>Metastore</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Jegyzetek 04/27/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.537.1486660 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.537.1486660 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.537.1486660 (HDP 2.1.12.0-2329 - változatlan marad)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Fix DLL függőség</td>
<td>Eltávolítja a egység próba keretében függés hdinsight szolgáltatásból lehetőségre.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Versenyhelyzetből hiba javítása</td>
<td>Fürt kérelem most vár létrehozása előtt állapotát a lekérdezési fogadhatók helyezése kérésre</td>
<td>SDK</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Jegyzetek 04/14/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - változatlan marad)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Tez hibajavítás</td>
<td>Javítások Apache TEZ 2214 és TEZ 1923 szerepelnek HDI 3,2 a jelenlegi kiadásába. Az egyes struktúra lekérdezései Tez jelentős mennyiségű adat sorrendű igénylő kifejezetten ezek van szükség.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Jegyzetek 04/06/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.521.1453250 (HDP 1.3.12.0-01795 - változatlan marad)
* HDInsight 3.0.6.521.1453250 (HDP 2.0.13.0-2117 - változatlan marad)
* HDInsight 3.1.3.521.1453250 (HDP 2.1.12.0-2329 - változatlan marad)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Távolítsa el az egyes belső osztályok Linux HDInsight-frissítések.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Avro tár 1.5.6</td>
<td>A felvett <b>KnownTypeAttribute</b> mód <b>GetAllKnownTypes</b>. Rögzített NullReferenceException, amikor egy típus értéke null GetAllKnownTypes mód.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Hibajavítás</td>
<td>A szolgáltatás számos hibajavítás</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Jegyzetek 04/01/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Azt jelenti, hogy a Windows fürt .NET SDK keresztül távoli asztali hitelesítő adatok engedélyezése vagy letiltása</td>
<td>Programozott támogatásának engedélyezése vagy letiltása a Windows fürt RDP hitelesítő adatait.</td>
<td>SDK</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Azt jelenti, hogy engedélyezése fürt távoli asztali hitelesítő adatait, miközben van folyamatban</td>
<td>Programozott támogatásának engedélyezése a távoli asztali hitelesítő adatokat, a fürt készül. Ezzel a művelettel eltávolítja a két lépésből álló folyamat első kiépítési a fürt, és ezután engedélyezése a távoli asztali.</td>
<td>SDK</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>A frissített Python való 2.7.8</td>
<td>Frissített Python a HDInsight csoportok Python 2.7.8, amely tartalmaz néhány fontos biztonsági HDInsight-verziók 2.1-es, 3.0-s, 3.1-es és a 3,2 javítások</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>FONAL konfigurációs módosítása</td>
<td>Módosított fonal konfigurációs yarn.resourcemanager.max-befejeződött-alkalmazások 3.1-es és a 3,2 HDInsight verzióihoz diagramtípusokat fürt 1000. Ez az érték csak szabályozza az alkalmazáslistából bejegyzett fonal felhasználói felület. Ha az alkalmazáslistából jelenik meg a felhasználói felület előtt küldött alkalmazásokkal kapcsolatos információkra, közvetlenül látogathat el a előzmények kiszolgáló.</td>
<td>FONAL</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>A csomópontok HBase fürt átméretezése</td>
<td>HBase most lehetővé teszi az átméretezést csomópontok (felfelé és lefelé) 3.1-es és a 3,2 HDInsight-verziók</td>
<td>Szolgáltatás</td>
<td>HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>JDBC frissítés</td>
<td>SQL JDBC illesztő verzió 3,2 HDInsight-frissítve verzió sqljdbc_4.0.2206.100. Ez a verzió továbbfejlesztett fontos biztonsági.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>JVM konfigurációs frissítése</td>
<td>Frissített JVM konfigurációs networkaddress.cache.ttl 300 másodperc az alapértelmezett értékek HDInsight verziójához 3.1-es és a 3,2 -1. Konfigurációs értéket a gyorsítótár házirend sikeres neve keresések a név szolgáltatásból szabályozza. Ez a növekvő és HBase fürt kicsinyítésével kapcsolatos hiba megoldása.</td>
<td>Szolgáltatás</td>
<td>HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Frissítési Java 2.0-s SDK Azure-tárolóhoz</td>
<td>3,2 verzió HDInsight verzióját szeretné használni a legújabb az Azure tároló SDK Java frissítését. Több fontos hibajavítás tartalmazza az aktuális 0.6.0 fölé verziója.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Frissített a Apache WASB forráskód</td>
<td>HDInsight 3,2 most használja a WASB formázáshoz illesztőprogram Apache Hadoop kódjával a legújabb verzióját. A módosítással a WASB illesztőprogram most csomagolása, mint egy külön üveg. Pusztán összecsomagolása módosítás, és nem tartalmaz az WASB illesztőprogram viselkedését módosításait. Ez a üveg fájl neve hadoop-azure-2.6.0.jar.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Fájlnév-frissítések a HDInsight-3,2 JAR</td>
<td>A frissítés HDInsight verzió 3,2 tartalmaz számos hibajavítás, és frissítve van a HDP részeként csomagolt néhány belső kancsó. Ne feledje, hogy ezek a fájlok üveg belső HDP csomag, nem pedig felhasználói alkalmazások közvetlen használja. Alkalmazások kell a kancsó saját verziójának csomagot, úgy, hogy a HDP belső kancsó frissítésének nem vevő alkalmazások oldaltörés.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Jegyzetek 03/03/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.488.1375841 (HDP 1.3.9.0-01351 - változatlan marad)
* HDInsight 3.0.6.488.1375841 (HDP 2.0.9.0-2097 - változatlan marad)
* HDInsight 3.1.3.488.1375841 (HDP 2.1.10.0-2290 - változatlan marad)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - változatlan marad)
* SDK 1.5.0 (változatlan)

Ebben a kiadásban tartalmazza a következő frissítést.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Megbízhatósága javítása</td>
<td>Végeztünk, amelyek lehetővé teszik a szolgáltatást, hogy jobban méretezése a fürt létrehozása részletez nagyobb terhelés javításokat.</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Jegyzetek 02/18/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.471.1342507 (HDP 1.3.9.0-01351 - változatlan marad)
* HDInsight 3.0.6.471.1342507 (HDP 2.0.9.0-2097 - változatlan marad)
* HDInsight 3.1.3.471.1342507 (HDP 2.1.10.0-2290 - változatlan marad)
* HDInsight 3.2.3.471.1342507 (HDP 2.2.10.0, 2340)
* SDK 1.5.0

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>HDInsight-3,2 fürt</td>
<td>Hadoop 2.6/HDP2.2 tartalmazó HDInsight-3,2 fürt érhető el. Az összes megnyitott-forrás összetevő főbb újdonságai tartalmaz. További információra kíváncsi olvassa el a HDInsight és <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 – kibocsátási megjegyzések</a>újdonságai című témakört.</td>
<td>Megnyitás-adatforrás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>HDinsight Linux (előzetes verzió)</td>
<td>Fürt Ubuntu Linux futó is telepíthető. Lásd: Ismerkedés a HDInsight Linux.</td>
<td>Szolgáltatás</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Általános vihar elérhetősége</td>
<td>Apache vihar fürt általában elérhetők. További információ az első lépések című vihar használata hdinsight szolgáltatásból lehetőségre.</td>
<td>Szolgáltatás</td>
<td>Vihar</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Virtuális gép méretek</td>
<td>Azure hdinsight szolgáltatáshoz a további virtuális gép típusú és méretű érhető el. HDInsight használhat A2 a beépített általános célú; A7 méretek Ez a funkció félvezető meghajtók (SSD) és 60 %-os gyorsabb processzor; D-sorozat csomópontok és gyors hálózat támogatása A8 és A9 InfiniBand betűméretet.</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>A méretezés csoport</td>
<td>Módosíthatja a futó HDInsight fürt adatok csomópontok számának anélkül, hogy törlése, és hozza létre. Jelenleg csak Hadoop-lekérdezés és Apache vihar fürt típusú van, de fürt típusú leggyorsabban, ha követi Apache HBase támogatása ezt a lehetőséget. További tudnivalókért olvassa el a HDInsight kezelése fürt című témakört.</td>
<td>Szolgáltatás</td>
<td>Hadoop, vihar</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Visual Studio szerszámok</td>
<td>Nemcsak a teljes szerszámok Apache vihar számára, a szerszámok for Visual Studio struktúra Apache utasítás befejezése, a helyi ellenőrzési és a továbbfejlesztett hibakeresési támogatási frissült. További információ az első lépések című HDInsight Hadoop eszközökkel for Visual Studio.</td>
<td>Szerszámok</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>A DocumentDB Hadoop-összekötő</td>
<td>A Hadoop összekötő DocumentDB végezhet összetett összesítések, elemzési és műveletek fölé a séma-kisebb JSON vagy tárolt dokumentumok között DocumentDB gyűjtemények adatbázis fiókok között. További információt és oktatóanyagot című témakörben talál futtatása Hadoop feladatok DocumentDB és hdinsight szolgáltatásból lehetőségre.</td>
<td>Szolgáltatás</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Hibajavítás</td>
<td>Van végeztünk HDInsight-szolgáltatások különböző kisebb hibajavítás. Várható ügyfélkapcsolati viselkedés változzon.</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Jegyzetek 02/06/2015 kiadásának hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.463.1325367 (HDP 1.3.9.0-01351 - változatlan marad)
* HDInsight 3.0.6.463.1325367 (HDP 2.0.9.0-2097 - változatlan marad)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK #HIÁNYZIK

Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Hibajavítás</td>
<td>Van végeztünk HDInsight-szolgáltatások különböző kisebb hibajavítás. Várható ügyfélkapcsolati viselkedés változzon.</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>HDP 2.1 karbantartási frissítése</td>
<td>HDInsight 3.1 HDP 2.1.10.0 üzembe frissül. További tudnivalókért olvassa el a <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">kibocsátási megjegyzések HDP-2.1.10</a>című témakört. </td>
<td>Megnyitás-adatforrás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>HDP bináris frissítések</td>
<td>Melyik a fájl nevét a frissített HBase néhány üveg fájlok szerepelnek. Ezek a fájlok üveg belső által használt HBase, így nem várt, hogy rendelkezik-e ügyfelek függőség a következő üveg fájlok nevét a. Ezek a következők:
<ul>
<li>./lib/jetty-6.1.26.hwx.JAR</li>
<li>./lib/jetty-sslengine-6.1.26.hwx.JAR</li>
<li>./lib/jetty-Util-6.1.26.hwx.JAR</li>
</ul>
</td>
<td>Megnyitás-adatforrás</td>
<td>HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Az 1/29/2015 kiadását HDInsight jegyzetek

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.455.1309616 (HDP 1.3.9.0-01351 - változatlan marad)
* HDInsight 3.0.6.455.1309616 (HDP 2.0.9.0-2097 - változatlan marad)
* HDInsight 3.1.2.455.1309616 (HDP 2.1.9.0-2196 - változatlan marad)
* SDK #HIÁNYZIK

Ebben a kiadásban tartalmazza a következő frissítést.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Probléma által sújtott terület (például szolgáltatás, összetevő vagy SDK)</p></th>
<th>Fürt típusa (például Hadoop, HBase vagy vihar)</th>
<th>JIRA (ha van)</th>
</tr>


<tr>

<td>Hibajavítás</td>
<td>Néhány fontos hibajavítás, amely az Azure frissítéskor megbízhatóbb a HDInsight fürt van végeztünk.</td>
<td>Szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Az 1/5/2015 kiadását HDInsight jegyzetek

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt teljes verziószámai:

* HDInsight 2.1.10.420.1246118 (HDP 1.3.9.0-01351 - változatlan marad)
* HDInsight 3.0.6.420.1246118 (HDP 2.0.9.0-2097 - változatlan marad)
* HDInsight 3.1.2.420.1246118 (HDP 2.1.9.0-2196 - változatlan marad)


Ebben a kiadásban tartalmazza az alábbi frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>A minták Twitter trend elemzési és a mozgókép Mahout-alapú javaslatok</td>
<td><p>Ebben a kiadásban a HDInsight lekérdezés konzol két további minták foglalja magában:</p>

<p><b>Twitter Trend-elemzés</b><br>
Nyilvános API-k, például a Twitteren webhelyek által megadott adatok elemzése és a népszerű trendek ismertetése hasznos forrás. Ebből az oktatóanyagból megtudhatja, hogy miként struktúra, amely a legtöbb twitterre egy adott szót tartalmazó Twitter felhasználók listájának használata. </p>

<p><b>Mahout film ajánlási</b><br>
Apache Mahout egy Apache Hadoop gépi tanulási tárban. Mahout kapcsolatos adatok (például a szűrést, osztályozás és fürtözés) feldolgozása algoritmusok tartalmazza. Ebben az oktatóprogramban egy ajánlási motor segítségével film javaslatok alapján ismerősei egyszer látott filmek készítése.</p></td>
<td>Lekérdezés konzolon</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Alapértelmezett értékét struktúra konfigurációs átállítása: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Ebben a konfigurációban mérete automatikusan átalakított térkép illesztések vonatkozik. Az érték alakíthatók át a memória kivonat térképek táblákhoz méretű összegét jelöli. Előzetes verzióban ez az érték nagyobb 10 MB-ot az alapértelmezett értékek 128 MB. Az új érték 128 MB-os azonban feladatok memória hiányában miatt sikertelen volt okozza. Ebben a kiadásban visszaáll az alapértelmezett érték vissza a 10 MB-ot. Felhasználók továbbra is választhat a megadott táblázat méretét, valamint a lekérdezések csoport létrehozása során ezt az értéket felülbírálása. Ezt a beállítást, és hogyan felülbírálja kapcsolatos további tudnivalókért lásd: az <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Automatikus csatlakozás átalakítást optimalizálása</a> Hortonworks dokumentációjában. </p></td>
<td>A struktúra</td>
<td>Hadoop, Hbase</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Megjegyzések a 12/23/2014-es kiadását hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt esetében a teljes verzióját számokat a következők:

* HDInsight 2.1.10.420.1246783 (változatlanul HDP verzió)
* HDInsight 3.0.6.420.1246783 (változatlanul HDP verzió)
* HDInsight 3.1.1.420.1246783 (változatlanul HDP verzió)

Ebben a kiadásban tartalmazza a következő frissítést.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>


<tr>
<td>Szakaszos fürt létrehozási hibák miatt túl nagy terhelés</td>
<td><p>Továbbfejlesztett algoritmus HDP csomagok letöltése során fürt létrehozása lehetővé teszi a hibák miatt túl nagy terhelés megbízhatóbb kezelését.</p></td>
<td>Szolgáltatás</td>
<td>Hadoop, Hbase, vihar</td>
<td>A #HIÁNYZIK</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Megjegyzések a 12/18/2014-es kiadását hdinsight szolgáltatáshoz

Ebben a kiadásban tartalmazza a következő összetevő-frissítést.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Általános Avalability fürt testreszabása</a></td>
<td><p>Testreszabási lehetővé teszi a testre az Azure hdinsight szolgáltatáshoz fürt Apache Hadoop ökológiai érhetők el a projektek számára. A új funkcióval kipróbálhat, és üzembe Hadoop projektek Azure hdinsight szolgáltatáshoz. Ez a **Parancsprogram művelet** funkciót, ami a Hadoop fürt módosíthatja a tetszőleges módon egyéni parancsfájlok használatával keresztül engedélyezve van. A Testreszabás Hadoop, HBase és vihar HDInsight fürt diagramtípusokat érhető el. Keresztül mutatjuk a power: ezt a lehetőséget, azt a népszerű <a href = "hdinsight-hadoop-spark-install.md" target="_blank">külső</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>és <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> modul telepítése a folyamat van ismertetését. Ebben a kiadásban is hozzáadja az ügyfelek számára az Azure portálon keresztül saját egyéni parancsfájl művelet megadása a képesség iránymutatást ad és ajánlott eljárások a segítő módszerekkel egyéni parancsfájl-műveletek összeállítása és nyújt útmutatást a parancsfájl műveletet ellenőrzésével kapcsolatban. </p></td>
<td>A szolgáltatás általános elérhetősége</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Megjegyzések a 12/05/2014-es kiadását hdinsight szolgáltatáshoz

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt esetében a teljes verzióját számokat a következők:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* A #hiányzik HDInsight SDK

Ebben a kiadásban tartalmazza a következő összetevő-frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>

<tr>
<td>Hibajavítás: szakaszos hibába nagyszámú partíciót hozzáadása egy struktúra DDL táblájához </td>
<td><p>Ha hiba lép fel, időnként kapcsolatot a struktúra metastore adatbázissal sok partíciót egy struktúratáblával való hozzáadásakor, a struktúra DDL sikertelen lehet. Az alábbi utasítás fog láthatja a struktúra hibanaplójának, ha a hiba lép fel: </p><p>"Hiba [fő]: ql. Illesztőprogram (SessionState.java:printError(547)) - nem sikerült: végrehajtási hiba, a org.apache.hadoop.hive.ql.exec.DDLTask visszajelzése 1. MetaException (message:java.lang.RuntimeException: commitTransaction hívták, de openTransactionCalls = 0. Ez valószínűleg jelzi, hogy nincsenek-e openTransaction/commitTransaction kiegyenítetlenségét hívásainak)"</p></td>
<td>A struktúra</td>
<td>Hadoop, Hbase</td>
<td>STRUKTÚRA 482 (Ez a egy belső JIRA, akkor nem kell külső felekkel jegyzett. Jegyezni az alábbi hivatkozások.)</td>
</tr>

<tr>
<td>Hibajavítás: alkalmi lefagy a lekérdezés HDInsight konzolon</td>
<td>Amikor ez történik, az alábbi utasítás is láthatja a WebHCat naplóban a WebHCat megnyitó projektre vonatkozóan: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper |} org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): nem tölthető előzmények fájl {wasb URL-címét az előzmények fájl} "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>STRUKTÚRA 482 (Ez a egy belső JIRA, akkor nem kell külső felekkel jegyzett. Jegyezni az alábbi hivatkozások.)</td>
</tr>

<tr>
<td>Hibajavítás: az időtartam Hbase lekérdezések alkalmi Nyárs</td>
<td>Ez történik, ha a felhasználók figyelje a-3 másodperccel alkalmi Nyárs a Hbase lekérdezések késleltetése. </td>
<td>HDInsight fürt átjáró</td>
<td>HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>HDP JAR fájl nevének módosítása</td>
<td>A HDI fürt 3.0-s, ott a belső üveg fájlok HDP telepítettek néhány verziója. rakodóhely-6.1.26.jar felváltotta rakodóhely-6.1.26.hwx.jar. rakodóhely-segédprogram-6.1.26.jar felváltotta rakodóhely-segédprogram-6.1.26.hwx.jar. A módosítások alkalmazása Hadoop, Mahout, WebHCat és Oozie projektek.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop, HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Megjegyzések HDInsight-11-es/21/2014-es kiadását az

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt esetében a teljes verzióját számokat a következők:

* HDInsight 2.1.9.382.1169709 (itt nincs változás az 11/14/2014-es)
* HDInsight 3.0.5.382.1169709 (itt nincs változás az 11/14/2014-es)
* HDInsight 3.1.1.382.1169709 (itt nincs változás az 11/14/2014-es)
* HDINsight SDK 1.4.0

Ebben a kiadásban tartalmazza a következő összetevő-frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>

<tr>
<td>Hozzáférés alkalmazás naplók</td>
<td>Kapcsolódási lehetőség a programozás útján számba veheti a fürt futtatott alkalmazásokat, és töltse le a megfelelő alkalmazás-specifikus vagy tároló-specifikus naplók problematikus alkalmazások hibáinak érdekében.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Adja meg a régió nevét a IHdInsightClient.DeleteCluster lehetősége </td>
<td>Az Azure hdinsight szolgáltatáshoz SDK lehetővé teszi a **DeleteCluster**használata esetén adja meg a régió nevét. Ezzel az elemzéssel tiltásának feloldása az ügyfelek, akik erőforrásokkal két azonos nevű különböző régiókban és nem tudta törölheti őket.</td>
<td>SDK</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>A **ClusterDetails** objektum egy **DeploymentID** mezőt, amely egy egyedi azonosítót a fürt adja eredményül. Biztosított fürt létrehozási kísérletek megegyező nevű belül egyedinek kell.</td>
<td>SDK</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Megjegyzések HDInsight-11-es/14/2014-es kiadását az

Ebben a kiadásban üzembe helyezéséhez HDInsight fürt esetében a teljes verzióját számokat a következők:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Ebben a kiadásban a következő új funkcióival, összetevő frissítések és hibajavítások tartalmazza.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>

<tr>
<td>Parancsfájl műveletet (előzetes verzió)</td>
<td>A fürt testreszabási szolgáltatást, amely lehetővé teszi, hogy a Hadoop módosítását mintája fürtök tetszőleges módon egyéni parancsfájlok használatával. Ezzel a szolgáltatással a felhasználók kísérletezni és üzembe projektek Apache Hadoop ökológiai az Azure hdinsight szolgáltatáshoz fürt elérhető. A testreszabási funkció a HDInsight fürt, beleértve a Hadoop, HBase és vihar diagramtípusokat.</td>
<td>Új szolgáltatás</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Beépített feladatok Azure webhelyekre és tároló analysis jelentkezzen</td>
<td>A HDInsight lekérdezés konzol egy első lépések gyűjteménye, amely támogatja a megoldások működő, az adatok vagy a mintaadatokat tartalmaz.
<p>**Megoldások, amelyek az adatok használata**:<br>
Korábban létrehozott feladatok egyes nyújt a kiindulási pont egyéni megoldások leggyakoribb adatok elemzése forgatókönyvet. Az adatok és a feladathoz segítségével látni, hogyan működik. Ha készen áll, használhatja tanultak van létrehozása után a beépített feladat modellezni megoldás.</p>
<p>**Megoldások, amelyek a mintaadatok használata**:<br>
Megtudhatja, hogy miként dolgozhat a HDInsight által útmutató néhány alapvető felhasználási helyzet (például a webes naplókról és a szenzoradatokat elemzése) alapján. Megtanulhatja a használatát HDInsight ilyen adatok elemzése céljából, és hogyan csatlakozhat más alkalmazások és szolgáltatások ezeket az adatokat. Példa: Ezzel a módszerrel a power adatok megjelenítése a Microsoft Excel útján biztosít.</p></td>
<td>Lekérdezés konzolon</td>
<td>Hadoop</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>A memória fejlesztő fix a Templeton</td>
<td>Az ügyfelek, akik volna hosszan futó fürt vagy 100s feladat volt elküldése érintett Templeton fejlesztő kéri, hogy egy második foglalkozott már. A probléma számos Templeton 5xx hibákat, valamint a megoldáshoz volt, indítsa újra a szolgáltatást. A megoldáshoz már nincs szükség.</td>
<td>Templeton</td>
<td>Az összes</td>
<td>https://issues.Apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Megjegyzés**: az új funkciók fürt testreszabási elérhetővé tett szemléltetésére parancsfájl művelettel külső és R modul telepítése fürt eljárások dokumentált. További tudnivalókért lásd:

* [Telepítése és a külső 1.0 HDInsight fürt használata](hdinsight-hadoop-spark-install.md)
* [Telepítése és R HDInsight Hadoop fürt használata](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Megjegyzések HDInsight-11-es/07/2014-es kiadását az

Ebben a kiadásban telepített HDInsight fürt esetében a teljes verziója számokat a következők:

* A HDInsight 2.1 2.1.9.374.1153876
* A HDInsight 3.0 3.0.5.374.1153876
* A HDInsight 3.1 3.1.1.374.1153876

Ebben a kiadásban tartalmazza a következő összetevő-frissítéseket.

<table border="1">
<tr>
<th>Cím</th>
<th>Leírás</th>
<th>Összetevő</th>
<th>Fürt típusa</th>
<th>JIRA (ha van)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Ebben a kiadásban a Hortonworks adatok Platform (HDP) 2.1.7 alapul. További tudnivalókért olvassa el a <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Kibocsátási megjegyzések az HDP 2.1.7</a>című témakört.</td>
<td>HDP</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>FONAL ütemterv kiszolgáló</td>
<td>Alapértelmezés szerint engedélyezve van a fonal ütemterv Server (más néven az általános előzmények alkalmazáskiszolgáló). Az ütemterv Server (például azonosítója, az alkalmazás neve, alkalmazás állapot, kérelem benyújtása idő és befejezési idő) bejegyzett alkalmazások általános információt tartalmaz.

Alkalmazás ezek az információk is lehet beolvasni a központi csomópontot a URI http://headnodehost:8188 elérésével vagy a fonal parancs futtatásával: fonal alkalmazás – – appStates lista összes.

Ez az információ is beolvasható távolról, ha a REST API-t a https://{ClusterDnsName}. azurehdinsight.NET/ws/V1/applicationhistory/.

További információ a <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Fonal ütemterv kiszolgáló</a>témakörben talál.</td>
<td>FONAL-szolgáltatás</td>
<td>Hadoop, HBase</td>
<td>A #HIÁNYZIK</td>
</tr>

<tr>
<td>Fürt telepítési azonosító</td>
<td>SDK változatának 1.3.3.1.5426.29232 kezdve, felhasználói hozzáférhetnek minden fürthöz HDInsight kiadó egyedi Azonosítót. A szolgáltatás lehetővé teszi az ügyfelek számára tudni, fürt egyedi előfordulását, amikor a DNS-neve alatt újra keresztben létrehozása vagy leválasztása esetek.</td>
<td>SDK</td>
<td>Az összes</td>
<td>A #HIÁNYZIK</td>
</tr>
</table>
<br>

**Megjegyzés**: Ebben a kiadásban a hiba jelenik meg a portálon vagy a Windows PowerShell vagy a SDK által visszaadott a teljes verziószám megakadályozó megoldását.

## <a name="notes-for-10152014-release"></a>Jegyzetek 10/15/2014-es kiadását.

Ebben a kiadásban gyorsjavítás fejlesztő Templeton nehéz felhasználóinak érintett Templeton rögzíteni. Bizonyos esetekben a felhasználók, akik Templeton erősen gyakorolni hibákat, mint 500 hibakódok azt, mivel a kérések volna nem elegendő memória futtatásához volna látható. A probléma megoldása volt, indítsa újra a Templeton szolgáltatást. Ez a probléma megoldását.


## <a name="notes-for-1072014-release"></a>Jegyzetek 10/7/2014-es kiadását.

* Ambari végpontot, használata esetén "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", a *gazdaszámítógép_neve* mezőben adja eredményül a teljes tartománynevét (FQDN) a csomópont helyett az állomásnév. Például helyett visszaadó "**headnode0**", akkor a teljesen minősített tartománynév "**headnode0. { ClusterDNS} .azurehdinsight .net**". Ez a változás esetek, amikor egy virtuális hálózati (például HBase és Hadoop) többféle fürt telepíthető megkönnyítésére volt szükség. Ez történik, ha például HBase a háttéradatbázist platformot, Hadoop-használatakor.

* A HDInsight fürt alapértelmezett telepítéshez új memória beállításainak nyújtott. Előző alapértelmezett memória beállításainak megfelelően nem befolyásolja a üzembe helyezéséhez Processzormagok számú útmutatást. Az új memória beállítások meg kell határoznia (szerinti Hortonworks javaslatok) jobb alapértékeit. Ha módosítani szeretné ezeket, olvassa el a SDK hivatkozás dokumentáció fürt konfigurálása módosításával kapcsolatos további. Az új, az alapértelmezett 4-es Processzor (8 tároló) core HDInsight fürt által használt memória beállítások vannak részletezve az alábbi táblázatban. (A jelenlegi kiadás előtt használt értékek is találhatók segítségével történik.)

<table border="1">
<tr><th>Összetevő</th><th>Memóriafoglalást</th></tr>
<tr><td> yarn.Scheduler.minimum-terhelés</td><td>768 MB (korábban 512 MB)</td></tr>
<tr><td> yarn.Scheduler.maximum-terhelés</td><td>6144 MB (változatlan)</td></tr>
<tr><td>yarn.nodemanager.Resource.Memory</td><td>6144 MB (változatlan)</td></tr>
<tr><td>mapreduce.Map.Memory</td><td>768 MB (korábban 512 MB)</td></tr>
<tr><td>mapreduce.Map.Java.opts</td><td>mellett dönt (korábban - Xmx410m) =-Xmx512m</td></tr>
<tr><td>mapreduce.reduce.Memory</td><td>1536 MB (korábban 1024 MB)</td></tr>
<tr><td>mapreduce.reduce.Java.opts</td><td>mellett dönt (korábban - Xmx819m) =-Xmx1024m</td></tr>
<tr><td>yarn.App.mapreduce.am.Resource</td><td>768 MB (korábban 1024 MB)</td></tr>
<tr><td>yarn.App.mapreduce.am.Command</td><td>mellett dönt (korábban - Xmx819m) =-Xmx512m</td></tr>
<tr><td>mapreduce.Task.IO.sort</td><td>256 MB (korábban 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (változatlan)</td></tr>

</table><br>

A memória beállítások fonal és a platformon Hortonworks adatok HDInsight által használt MapReduce által használt kapcsolatos további tudnivalókért olvassa el a [HDP memória konfigurációs beállítások megállapítása](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html)című témakört. Hortonworks is adni a számítja ki a megfelelő memória beállításai eszközben.

Az Azure PowerShell és a HDInsight SDK hibaüzenet: "*fürt nincs beállítva a HTTP-szolgáltatások hozzáférést*":

* Ez a hiba miatt a HDInsight SDK vagy Azure PowerShell verziója és a fürt közötti különbség esetleg fellépő ismert [kompatibilitási probléma](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) . 8/15 vagy újabb létrehozott fürt van virtuális hálózatok be új kiépítési szolgáltatás támogatása. De ez a lehetőség nem megfelelően értelmezi a függvény a HDInsight SDK vagy Azure PowerShell régebbi verziói. Az egyes feladat Beküldési műveletek hiba eredménye. Ha HDInsight SDK API-khoz vagy Azure PowerShell-parancsmagok (**Használata-AzureRmHDInsightCluster** vagy **Invoke-AzureRmHDInsightHiveJob**) küldhetnek feladatokat, ezek a műveletek során, melyről hibaüzenet "*fürt <clustername> nincs beállítva a HTTP-szolgáltatások hozzáférést*." Vagy (attól függően, hogy a művelet), más hibaüzenetek, például a "*nem tud csatlakozni a fürt*" jelenhetnek meg.

* E kompatibilitási problémák megoldott a HDInsight SDK és Azure PowerShell legújabb verzióját. Javasoljuk, hogy a HDInsight SDK módosítása verziója 1.3.1.6 vagy újabb és Azure verzióra 0.8.8 PowerShell-eszközök vagy újabb verziójában. Hozzáférhet a legújabb HDInsight SDK csomagjában talál a [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) és az Azure PowerShell eszközök vele, [hogyan telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Az 9/12/2014-es kiadását HDInsight 3.1 megjegyzéseket

* Ebben a kiadásban a Hortonworks adatok Platform (HDP) 2.1.5 alapul. Ebben a kiadásban javított listáját lásd az [Ebben a kiadásban rögzített](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) a Hortonworks webhelyen.
* A tárak malac mappában a "avro mapred, 1.7.4.jar" fájl módosítását követően "avro-mapred-1.7.4-hadoop2.jar." Ez a fájl tartalmát egy kisebb hibajavítás, amely nem törhető tartalmazza. Ajánlott, felhasználók ne folytasson közvetlen függőség a oldaltörések elkerülése a fájlok átnevezéskor üveg fájl nevére.


## <a name="notes-for-8212014-release"></a>Jegyzetek 8/21/2014-es kiadását.

* Az alábbi WebHCat konfiguráció (struktúra-7155), amely állítja be a feladat Templeton vezérlő alapértelmezett memória korlátját 1 GB azt hozzáadni. (Az előző alapértelmezett érték volt 512 MB).

     (= 1024) templeton.mapper.Memory.MB

    * Ez a változás az alábbi hiba, amely bizonyos struktúra lekérdezések volna futtassa alsó memória korlátai miatt szünteti meg: "Tároló fut túl fizikai memória korlátai."
    * Visszatérés a régi alapértelmezett beállításait, beállíthatja, hogy a konfigurációs érték 512 Azure Powershellen keresztül fürt létrehozáskor a következő parancs használatával:

        Adja hozzá AzureRmHDInsightConfigValues-Core@{"templeton.mapper.memory.mb"="512";}


* Az állomásnév a zookeeper szerepkör *zookeeper*változott. Ez hatással van a névfeloldás a fürt belül, de nincs hatással a külső REST API-khoz. Ha a *zookeepernode* állomásnév használt összetevők, frissítenie kell őket az új név használatára. Az új neveket a három zookeeper csomópontok a következők:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase verzió támogatási mátrix frissül. Csak a HDInsight (HBase verzió 0,98.) 3.1-es verzióját HBase munkaterhelésekből gyártási használata támogatott. 3.0-s verziója (ez volt előzetes elérhető) nem támogatott mozgatása előre.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>8/15/2014-es előtt létrehozott fürt kapcsolatos megjegyzések

Azure PowerShell vagy HDInsight SDK hibaüzenet "fürt <clustername> nincs beállítva a HTTP-szolgáltatások hozzáférést" (vagy attól függően, hogy a művelet más hibaüzeneteket például: "Nem tud csatlakozni fürt") Azure PowerShell vagy a HDInsight SDK és fürt verzió eltérése miatt előfordulhat, hogy előforduló. 8/15 vagy újabb létrehozott fürt van virtuális hálózatok be új kiépítési szolgáltatás támogatása. Ezt a funkciót nem megfelelően értelmezi az Azure PowerShell vagy a HDInsight SDK, feladat Beküldési műveletek hibák eredményez, amelyek régebbi verzióit. Ha HDInsight SDK API-k és Azure PowerShell-parancsmagok (például használata-AzureRmHDInsightCluster vagy Invoke-AzureRmHDInsightHiveJob) használatával küldhetnek feladatokat, ezek a műveletek során, melyről közül a hibaüzenetek leírt.

E kompatibilitási problémák megoldott a HDInsight SDK és Azure PowerShell legújabb verzióját. Javasoljuk, hogy a HDInsight SDK módosítása verziója 1.3.1.6 vagy újabb és Azure verzióra 0.8.8 PowerShell-eszközök vagy újabb verziójában. Hozzáférhet a legújabb HDInsight SDK a [NuGet][nuget-link]. A [Microsoft webes Platform telepítő]érhetők el az Azure PowerShell eszközök[webpi-link].


## <a name="notes-for-7282014-release"></a>Jegyzetek 7/28/2014-es kiadását.

* **Új régióban elérhető HDInsight**: azt HDInsight földrajzi jelenléti kibontott három területre. HDInsight-ügyfelek e régiók fürt hozhat létre:
    * Kelet-ázsiai
    * A központi Észak-amerikai
    * A központi Dél-Amerikai Egyesült Államok
* HDInsight verzió 1,6 (HDP 1.1-es és Hadoop 1.0.3) és a HDInsight (HDP1.3 és Hadoop 1.2-es) 2.1-es verziójával is eltávolítása a csoportból az Azure-portálra. Az e verzió Hadoop fürt létrehozásához, az Azure PowerShell parancsmaggal, [Új-AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) vagy a [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx)használatával is. Olvassa el a [HDInsight-összetevő verziószámozás](hdinsight-component-versioning.md) lap további információt.
* Ebben a kiadásban változások Hortonworks adatok Platform (HDP):

<table border="1">
<tr><th>HDP</th><th>Módosítások</th></tr>
<tr><td>AZ 1.3 HDP / HDI 2.1</td><td>Nincs módosítás</td></tr>
<tr><td>HDP 2.0-S / HDI 3.0</td><td>Nincs módosítás</td></tr>
<tr><td>HDP 2.1 / HDI 3.1.</td><td>zookeeper: [3.4.5.2.1.3.0-1948]-[3.4.5.2.1.3.2-0002] ></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Jegyzetek 6/24/2014-es kiadását.

Ebben a kiadásban továbbfejlesztett a HDInsight szolgáltatáshoz:

* **HDP 2.1 elérhetősége**: (mely tartalmazza a HDP 2.1-es) HDInsight 3.1 általában elérhető, és az alapértelmezett verzió új fürtre vonatkozóan.
* **HBase – Azure portál fejlesztések**: azt készít HBase fürt előzetes verzióban érhető el. Mindössze néhány kattintással portálról HBase fürt hozhat létre. 

HBase, a készíthet valós idejű munkaterhelésekből számos HDInsight, a nagy adathalmazok végpontjait milliónyi érzékelő és telemetriai adatokat tároljon szolgáltatások használata interaktív webhelyről. A következő lépés a következő lesz az alábbi feladatok Hadoop feladatok található adatok elemzése céljából, és ez lehetséges Azure PowerShell és a struktúra fürt irányítópult hdinsight szolgáltatásból lehetőségre.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>HDInsight 3.1 előtelepítve Apache Mahout

 [Mahout](http://hortonworks.com/hadoop/mahout/) is előre telepítve van a HDInsight 3.1 Hadoop fürt, ezért nincs szükség további fürt konfigurálása Mahout feladatok futtatását is lehetővé teszi. Ha például segítségével távoli be egy Hadoop fürthöz távoli asztali Protocol (RDP), és további lépések nélkül futtatását is lehetővé teszi a következő helló világ Mahout parancsot:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Ez az eljárás részletesebb leírását dokumentációjában a [Breiman példa](https://mahout.apache.org/users/classification/breiman-example.html) a Apache Mahout webhelyen.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Struktúra lekérdezések használható Tez HDInsight 3.1

Struktúra 0,13 HDInsight 3.1 érhető el, és képes Tez, amelyhez képest jelentős teljesítménybeli javulást érhet valószínűleg kihasználják lekérdezések futtatása.
Tez nem struktúra lekérdezések alapértelmezés szerint engedélyezve. Ahhoz, hogy használhassa, meg kell csatlakozás. A következő kódrészletet futtatásával Tez engedélyezheti:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

A szokásos szintek szállított Hortonworks közzétett struktúra lekérdezés teljesítménnyel kapcsolatos fejlesztések Tez részletes kifejtése. A részletekért olvassa [mérés Apache struktúra 13 vállalati Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Tez struktúra használatáról további tudnivalókat talál [a Tez struktúra](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globális elérhetősége
A Hadoop 2.2 HDInsight kiadásával Microsoft HDInsight elérhetővé tett az összes fő geographies, ahol Azure áll rendelkezésre. Konkrétan a nyugati Európa és Délkelet-ázsiai adatközpontokkal hívták online. Az ügyfelek számára egy adatközpontban bezárása gombra, és esetleg a hasonló megfelelőségi követelmények zónán keresse meg a fürt lehetővé teszi.


###<a name="dos--donts-between-cluster-versions"></a>Tennivalók & listával 's fürt verziói között

**Egy HDInsight 3.1 fürthöz használt Oozie metastores nem kompatibilis a HDInsight 2.1 fürt, és nem használható a korábbi verziójával**.

Egy HDInsight 3.1 fürthöz rendszerbe egyéni Oozie metastore adatbázis-HDInsight 2.1 fürthöz nem felhasználható. Ez a helyzet, ha a metastore egy HDInsight 2.1 fürthöz származik. Ebben az esetben nem támogatott, mert a metastore séma kap frissített, így a már nem kompatibilis a követel meg az HDInsight 2.1 fürt metastore egy HDInsight 3.1 fürthöz használva. Bármely olyan Oozie metastore egy HDInsight 3.1 fürthöz használt újrafelhasználásának kísérlete lesz jeleníti meg a HDInsight 2.1 fürt használhatatlan.

**Oozie metastores nem lehet megosztani, fürt keresztül.**

Adott fürt csatolt Oozie metastores, és nem oszthatók fürt keresztül.

###<a name="breaking-changes"></a>Módosítások megszakítása

**Szintaxis előtag**: csak a "wasbs: / /" szintaxis HDInsight 3.1-es és 3.0-s fürt támogatott. A régebbi "asv: / /" szintaxis HDInsight 2.1-es és 1,6 fürt támogatott, de nem támogatja a HDInsight 3.1-es vagy 3.0-s fürt. Ez azt jelenti, hogy minden feladat HDInsight 3.1-es vagy 3.0-s fürthöz kifejezetten használó a "asv: / /" szintaxis sikertelen lesz. A "wasbs: / /" szintaxis helyett kell alkalmazni. Bármely HDInsight 3.1-es vagy egy meglévő metastore erőforrásoknak explicit hivatkozásokat tartalmazó eszközzel létrehozott 3.0-s fürt elé is, a feladatok a "asv: / /" szintaxis sikertelen lesz. Ezek a metastores kell újra használatával hozható létre a "wasbs: / /" cím erőforrások szintaxist.


**Portokat**: a HDInsight szolgáltatás által használt portokat megváltozott. Az éppen használt portszámokat volt a Windows operációs rendszer porttartományt tartományon belül. Egy előre definiált rövid életű cellatartományból Internet Protocol (protokoll) alapú kommunikáció rövid életű portok van rendelve automatikusan. Az újonnan létrehozott engedélyezett Hortonworks adatok Platform (HDP) szolgáltatás portszámokat vannak, ha az a központi csomóponton futó szolgáltatás által használt portokat sikerült felmerülő ütközések elkerülése érdekében a tartományon kívül eső. Az új portszámokat ne okozzon bármely bekövetkezett változásokkal kapcsolatban. A számok, használja a következők:

 **A HDInsight 1,6 (HDP 1.1-es)**
<table border="1">
<tr><th>név</th><th>Érték</th></tr>
<tr><td>DFS.http.address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.job.Tracker.http.address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>mapreduce.history.Server.http.address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

 **A HDInsight 3.1-es és 3.0-s (HDP 2.1-es és 2.0-s)**
<table border="1">
<tr><th>név</th><th>Érték</th></tr>
<tr><td>DFS.namenode.http-cím</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.https-cím</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.IPC.address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.http-cím</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.nodemanager.WebApp.address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Függőségek

A következő függőségeket hozzáadott HDInsight 3.x (HDP2.x):

* guice-servlet
* optiq-core
* javax.inject
* az aktiválás
* jsr305
* geronimo-jaspic_1.0_spec
* július – slf4j
* Java-xmlbuilder
* telepítsenek
* Commons-fordító
* jdo-api
* Commons-math3
* paranamer
* jaxb-impl
* stringtemplate
* eigenbase-xom
* Jersey-servlet
* Commons-végrehajtási
* jaxb-api
* az összes rakodóhely-kiszolgáló
* janino
* xercesImpl
* optiq-avatica
* jta
* eigenbase-tulajdonságok
* groovy-all
* hamcrest-core
* levelek
* linq4j
* jpam
* Jersey-ügyfél
* aopalliance
* geronimo-annotation_1.0_spec
* telepítsenek-indító ikon
* Jersey-guice
* XML-API-hoz
* stax-api
* Asm-commons
* Asm fa
* wadl
* geronimo-jta_1.1_spec
* guice
* leveldbjni-all
* sebesség
* ellehetetlenülését okozta
* klassz kis java
* rakodóhely-all
* Commons-dbcp

A következő függőségeket már nem léteznek HDInsight 3.x (HDP2.x):

* jdeb
* kfs
* sqlline
* ivy
* aspectjrt
* JSON
* alapvető
* jdo2-api
* avro-mapred
* datanucleus-enhancer
* jsp
* Commons-naplózás-api
* a matematikai Commons
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* klassz kis

###<a name="version-changes"></a>Módosítása verziója

A következő verzió változtak közötti HDInsight 2.x (HDP1.x) és a HDInsight 3.x (HDP2.x):

* Mértékek-core: [2.1.2]-[3.0.0] >
* derbynet: [10.4.2.0]-[10.10.1.1] >
* datanucleus: ["rdbms-3.0.8'] -> [" rdbms-3.2.9']
* jasper-fordító: [5.5.12]-[5.5.23] >
* log4j: [1.2.15. "pontját", "1.2.16"] -> ["1.2.16", "1.2.17"]
* derbyclient: [10.4.2.0]-[10.10.1.1] >
* httpcore: [4.2.4]-[4.2.5] >
* hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0] >
* a java protobuf: [2.4.1]-[2.5.0] >
* Derby: [10.4.2.0]-[10.10.1.1] >
* jasper: ["futtatókörnyezet-5.5.12'] -> [" futtatókörnyezet-5.5.23']
* Commons-démon: ["1.0.1-es verziójú"] [1.0.13] ->
* datanucleus-core: [3.0.9]-[3.2.10] >
* datanucleus-api-jdo: [3.0.7]-[3.2.6] >
* zookeeper: [3.4.5.1.3.9.0-01320]-[3.4.5.2.1.3.0-1948] >
* bonecp: [0.7.1.RELEASE] -> ["
* 0.8.0.RELEASE "]


### <a name="drivers"></a>Illesztőprogramok
Az SQL Server Java adatbázis Connnectivity (JDBC) illesztőprogram belső használatú hdinsight szolgáltatásból lehetőségre, és nem használható a külső műveletekhez. Ha Open Database Connectivity (ODBC) használatával HDInsight csatlakozni szeretne, használja a Microsoft-struktúra ODBC-illesztőprogram. További tudnivalókért lásd: az [Excel csatlakozni a Microsoft struktúra ODBC-illesztőprogram hdinsight szolgáltatáshoz](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Hibajavítás

Az ebben a kiadásban azt frissítése után az alábbi HDInsight-verziók számos hibajavítás a:

* A HDInsight 2.1 (HDP 1.3-as)
* A HDInsight 3.0-s (HDP 2.0-s)
* A HDInsight 3.1 (HDP 2.1-es)


## <a name="hortonworks-release-notes"></a>Hortonworks kibocsátási megjegyzések

Kibocsátási megjegyzések az a Hortonworks-adatok platformon (HDPs) HDInsight verzió fürt által használt érhetők el az alábbi helyeken:

* HDInsight 3.1-es verzióját használja a [Hortonworks adatok Platform 2.1.7]alapuló Hadoop eloszlás[hdp-2-1-7]. Ez az alapértelmezett Hadoop fürt jön létre, ha az Azure portál használatával 11/7/2014-es után. Előtt 11/7/2014-es [Hortonworks adatok Platform 2.1.1] azok alapján létrehozott HDInsight 3.1 fürt[hdp-2-1-1]

* HDInsight 3.0-s verzió használja a [Hortonworks adatok Platform 2.0-s]alapuló Hadoop eloszlás[hdp-2-0-8].

* HDInsight 2.1-es verziójával használja a [Hortonworks adatok Platform 1.3]alapuló Hadoop eloszlás[hdp-1-3-0].

* HDInsight 1,6 verzióját használja a [Hortonworks adatok Platform 1.1]alapuló Hadoop eloszlás[hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
