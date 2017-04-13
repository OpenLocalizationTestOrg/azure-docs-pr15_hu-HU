<properties
   pageTitle="Helyi beállításai R Server (előzetes verzió) HDInsight kiszámítania |} Microsoft Azure"
   description="További tudnivalók: a különböző számítási környezeti beállítások R-kiszolgálóval felhasználók számára elérhető HDInsight (előzetes verzió)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Helyi beállításterület R Server (előzetes verzió) HDInsight kiszámítása

Microsoft Azure hdinsight szolgáltatáshoz (előzetes verzió) R kiszolgáló R-alapú analytics a legújabb funkciókhoz biztosít. Az [Azure Blob]-(../storage/storage-introduction.md "Azure Blob-tárolóhoz") tároló fiókja vagy a helyi Linux fájlrendszer tárolóban Fájlrendszerhez tárolt adatokat használ. R kiszolgáló Megnyitás R épül, mivel az R-alapú alkalmazások generál is kihasználhatja 8000 + R Megnyitás csomagok közül. Azok is kihasználhatja, hogy a feladatok, az [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "Forradalom Analytics ScaleR"), a Microsoft nagy adatok analytics csomag R kiszolgáló tartalmaz.  

A szegély csomópontot prémium fürt csatlakozzon a fürthöz, és az R parancsfájlok futtatásának kényelmes helyet biztosít. Az egy él csomópont a magmintákat a biztonsági a csomópont kiszolgálón keresztül ScaleR's parallelized elosztott függvények futó lehetőséget, ha van. Mutassa meg a fürt csomópontok közötti ScaleR's Hadoop térkép csökkentése használatával lehetősége is van, vagy külső környezetek számítja ki.

## <a name="compute-contexts-for-an-edge-node"></a>Egy él csomópont környezetek kiszámítása

Az általános-R parancsfájl, a szegély csomópontra R kiszolgálón futó fut, az R értelmező belül csomópontra. A kivétel ez alól ezek a lépések, amelyek ScaleR függvény. A ScaleR hívások határozzák meg, hogyan állíthatja az ScaleR számítási környezetben számítási környezetben futtatni.  Az R parancsprogram-él csomópont futtatásakor a számítási környezet lehetséges értékei a következők helyi valamilyen sorrendben követik egymást ("helyi"), helyi párhuzamos (localpar), a térkép csökkentése és a külső.

A helyi"és"localpar"beállításai különböznek egymástól csak rxExec hívások végrehajtásának módját. Mindkettő végrehajtása más rx feladatkört hívások párhuzamos módon át az összes rendelkezésre álló magmintákat másképp ScaleR numCoresToUse lehetőséggel, például rxOptions(numCoresToUse=6) keresztül. A következő összefoglalja a különböző számítási környezeti beállítások

| Helyi kiszámítása  | Hogyan kell beállítani                      | A végrehajtási környezet                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Helyi egymás után következő | rxSetComputeContext('local')    | Biztonsági csomópontot, kivéve a rxExec kiszolgálón keresztül a magmintákat parallelized végrehajtása meghívja, amely sorozatban végrehajtása |
| Helyi párhuzamos   | rxSetComputeContext('localpar') | Biztonsági a csomópont kiszolgálón keresztül a magmintákat parallelized végrehajtása                                 |
| A külső            | RxSpark()                       | A HDI fürt csomópontok közötti keresztül külső elosztott végrehajtása parallelized      |
| Térkép csökkentése       | RxHadoopMR()                    | A HDI fürt csomópontok közötti keresztül térkép csökkentése elosztott végrehajtása parallelized |


Feltételezve, hogy meg szeretné parallelized végrehajtási teljesítmény szempontjából, majd nincsenek három lehetőség közül választhat. A választott jellegét analytics munkáját, és a méret és az adatok helyét függ.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Útmutató a számítási környezetben kiválasztásához

Jelenleg nem képletet, amely közli, hogy használandó helyi számítja ki. Vannak azonban néhány irányadó elvek, amely segít a helyes választás legyen-e, és legalább szűkítheti a választási lehetőségek egy javasolt futtatása előtt. Irányadó elvek ezek a következők:

1.  A helyi Linux fájl rendszer Fájlrendszerhez gyorsabb.
2.  Ismétlődő elemzések gyorsabb, ha az adatok helyi, és ha a XDF.
3.  Célszerű adatfolyam-adatok; szöveg adatforrásból kis lépésekben Ha az adatok mennyiségét nagyobb, átalakíthatja XDF elemzés előtt.
4.  Az általános másolása vagy az adatok elemzése él csomópontját streaming Kezelhetetlen nagyon nagy mennyiségű adat lesz.
5.  A külső gyorsabban térkép csökkentheti a Hadoop elemzéshez van.

Ezek az elvek tekintve néhány általános szabályok a legjobb megoldás számítási környezetben kijelölésére szolgáló vannak:

### <a name="local"></a>Helyi

- Ha az összeg, az elemezni kívánt adatokat kicsi, és nem igénylő ismételt elemzése, közvetlenül az elemzés gyakorlatának adatfolyam azt, és "helyi" vagy "localpar".
- Ha az elemezni kívánt adatokat mennyiségét kicsi, közepes méretű vagy, és csak az ismételt elemzése, majd másolja a vágólapra a helyi fájlrendszerben, XDF importálja, és elemezheti azt helyi"vagy"localpar".

### <a name="hadoop-spark"></a>A külső Hadoop

- Ha az elemezni kívánt adatokat mennyiségét túl nagy, majd importálnia kell azt XDF Fájlrendszerhez a (kivéve, ha a tároló probléma), és elemezhetők a "Külső" keresztül.

### <a name="hadoop-map-reduce"></a>Hadoop térkép csökkentése

- Csak akkor, ha a külső számítási környezet használatával egy megoldhatatlan problémát tapasztal, mivel ez általában a lesz lassabban használja.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Beágyazott súgó rxSetComputeContext

A Súgó nyelve R rxSetComputeContext módszer, például beágyazott talál további információkat és ScaleR számítási környezetek példákat:

    > ?rxSetComputeContext

Akkor is érdemes lehet a "ScaleR elosztott számítások útmutató" áll rendelkezésre a [R kiszolgáló MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R kiszolgáló MSDN") -tárból.


## <a name="next-steps"></a>Következő lépések

Ebben a cikkben megtanulta azt is tartalmazó új HDInsight fürt R kiszolgáló létrehozása. Emellett megtanulta azt is az a R konzol egy SSH munkamenetből használatának alapjai. Most már erről felderítésére, más módokon R HDInsight-kiszolgálón való használatáról az alábbi cikkekben:

- [A Hadoop R Server – áttekintés](hdinsight-hadoop-r-server-overview.md)
- [R-kiszolgáló Hadoop használatának első lépései](hdinsight-hadoop-r-server-get-started.md)
- [Adja hozzá a RStudio kiszolgálót HDInsight-támogatás](hdinsight-hadoop-r-server-install-r-studio.md)
- [HDInsight prémium R Server Azure tárhely beállításai](hdinsight-hadoop-r-server-storage.md)
