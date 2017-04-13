<properties
   pageTitle="Azure tó adattár áttekintése |} Microsoft Azure"
   description="Mi az Azure tó adattár és más adatok áruházak keresztül biztosít értékét ismertetése"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="overview-of-azure-data-lake-store"></a>Azure tó adattár áttekintése

Azure tó adattár egy vállalati szintű hyper skála a tárháza nagy adatok analitikus munkaterhelésekből. Azure adatok tó lehetővé teszi, hogy minden olyan méretét, a típus és a bevitel sebességének működési és felderítő elemzéséhez egy egyetlen helyen adatok rögzítése.

> [AZURE.TIP] [Tó adattár tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/data-lake-store-self-guided-training/) segítségével az Azure tó adattár szolgáltatás felfedezőút megkezdéséhez.

Azure tó adattár Hadoop (HDInsight fürthöz érhető el) elérheti a WebHDFS-kompatibilis REST API-k használata. Ahhoz, hogy a gyorsítótárban tárolt adatokat a analytics kifejezetten azt, és a teljesítmény elérése érdekében adatok analytics felhasználási területei van beállítva. Beépített, a vállalati szintű összes funkcióját tartalmazza – biztonság, kezelhetőség, méretezhetőség, megbízhatóságának és elérhetősége – a való életben vállalati használati eset alapvető.


![Azure adatok tó](./media/data-lake-store-overview/data-lake-store-concept.png)

A főbb funkciói az Azure adatok tó közé tartoznak az alábbiak.

### <a name="built-for-hadoop"></a>A Hadoop épített

Az Azure adatok tó tároló-kompatibilis a Hadoop elosztott fájl rendszer (hdfs) lehetőségre, és az együttműködik az Hadoop ökológiai Apache Hadoop fájlrendszer.  A meglévő HDInsight-alkalmazások és szolgáltatások WebHDFS API-t használó egyszerű integrálhatja tó áruházzal. Tó adattár is elérhetővé teszi a alkalmazások WebHDFS-kompatibilis REST felület

Adatok tó tárban tárolt adatok egyszerűen elemezheti Hadoop analitikus keretek például MapReduce vagy struktúra használatával. Microsoft Azure HDInsight fürt is kiépítéstől, és így közvetlenül elérheti az adatok tó tárban tárolt adatok beállítva.

### <a name="unlimited-storage-petabyte-files"></a>Korlátlan tárolására, petabyte fájlok

Azure tó adattár korlátlan tárolására szolgál, és alkalmas az adatok elemzéséhez számos tárolásához. Bármilyen fiókhoz méretek, fájlok méretének vagy egy adatok tó tárolt adatok mennyiségét vonatkozó korlátok nem ír elő. Egyes fájlokat így bármilyen típusú adatok tárolására kiváló választás méretű petabytes terjedhet méretűek lehetnek. Adatok tartósan tárolásának több másolásával, és nem az időtartam, amelynek az adatokat az adatok tó tárolható korlátozott.

### <a name="performance-tuned-for-big-data-analytics"></a>Beállítva, nagy adatok elemzéséhez

Azure tó adattár-rendszerű nagyméretű analitikus lekérdezéséhez és elemezhet nagy mennyiségű adat tömeges átviteli igénylő épül. Az adatok tó egyes tároló kiszolgáló több oldalpárokat fájl részei Ez az olvasási átviteli javítja a a fájlt, ezzel párhuzamosan adatok analytics elvégzésére vonatkozó olvasásakor.


### <a name="enterprise-ready-highly-available-and-secure"></a>Vállalati kész: Magas rendelkezésre és biztonságos

Azure tó adattár szabványos rendelkezésre állásának és megbízhatóságának biztosít. Az adatok eszközök megelőzése érdekében a váratlan hibák felesleges másolásával tartósan tárolja. Környezetbe a meglévő adatok platform fontos részeként használhatja azok megoldását Azure adatok tó.

Tó adattár a vállalati szintű biztonsága a gyorsítótárban tárolt adatokat is tartalmaz. További információ a [Azure tó adattár biztonságossá tétele adatok](#DataLakeStoreSecurity)témakörben olvasható.


### <a name="all-data"></a>Az összes adat

Azure tó adattár semmilyen adatot tárolhat natív formátumban, adott állapotában, anélkül, hogy előzetes átalakításokat. Tó adattár nincs szükség az adatok betöltése, előtt definiálni sémát hagynia a felfelé az egyes analitikus keretrendszer gombra az adatok értelmezéséhez, és állítsa be a séma az elemzés idején. Tetszőleges méretű és formátumok tárolásához is lehetővé teszi tó adattár kell kezelni strukturált, félig strukturált és strukturálatlan adatokat.

Azure tó adattár tárolók az adatokat is lényegében mappák és a fájlokat. A gyorsítótárban tárolt adatokat SDK, Azure-portál és Azure Powershell működnek. Mindaddig, amíg ki a felületek, és használja a megfelelő tárolók tárolóba helyezze el az adatokat, bármilyen típusú adatot tárolhat. Tó adattár nem hajt végre bármely különleges kezelési adatok alapján, milyen típusú adatokat tárolja.


## <a name="DataLakeStoreSecurity"></a>Adatok az Azure tó adattár biztonságossá tétele

Azure tó adattár hitelesítési Azure Active Directory használ, és az access (-szabályozási listák) az adatokhoz való hozzáférés kezelése.

| A szolgáltatás                                 | Leírás                              |
|-----------------------------------------|------------------------------------------|
| Hitelesítés | Azure tó adattár az Azure Active Directory (AAD) egyesíti az Azure tó adattár tárolható összes adatra vonatkozó identitás- és a hozzáférés kezelése. Többtényezős hitelesítés, a feltételes access, a szerepköralapú hozzáférés-vezérlés, a alkalmazások használatát figyelése, biztonsági figyelés és riasztási, beleértve minden AAD funkció előnyeinek Azure adatok tó integrációja eredményeként stb. Azure tó adattár támogatja a OAuth 2.0-s protokollt, a többi felületén a hitelesítést. |
| Hozzáférés-vezérlés                          | Azure tó adattár biztosít a hozzáférés-vezérlés WebHDFS protokollt által elérhetővé tett POSIX stílusú engedélyeket támogatásával. Az aktuális programcsomag a hozzáférés-vezérlési listák engedélyezhető a legfelső szintű mappa, almappákat, valamint egyes fájlokat. A hozzáférés-vezérlési listák alkalmaz a legfelső szintű mappa automatikusan is alkalmazandó összes a gyermek/mappafájlok is.|

Szeretné, ha többet szeretne tudni az adatok tó áruház adatainak védelme. Kövesse az alábbi hivatkozásokat.

* Biztonságos adattár tó adatokat olvashat útmutatásért lásd: [Azure tó adattár biztonságossá tétele adatok](data-lake-store-secure-data.md).
* Inkább a videók? [Ebből a videóból](https://mix.office.com/watch/1q2mgzh9nn5lx) biztonságos adattár tó tárolt adatok olvashat.

## <a name="applications-compatible-with-azure-data-lake-store"></a>Azure tó adattár kompatibilis alkalmazások

Azure tó adattár, Hadoop ökológiai leginkább megnyitott forrás összetevőket kompatibilis. Azt is integrálódik megfelelően legyenek rendezve más Azure-szolgáltatásokkal. Így tó adattár az adatokat tároló igényeinek egy tökéletes lehetőséget. Ha többet szeretne megtudni, hogy miként használható tó adattár, mind a forrás megnyitása, valamint egyéb Azure szolgáltatás az alábbi hivatkozásokat követve.

* Című témakörben [alkalmazások és szolgáltatások Azure tó adattár kompatibilis](data-lake-store-compatible-oss-other-applications.md) Megnyitás alkalmazások tó adattár együttműködik.
* Lásd: [más Azure szolgáltatások integrálása](data-lake-store-integrate-with-other-services.md) megértéséhez, hogy miként tó adattár használható más Azure szolgáltatásokhoz ahhoz, hogy a esetek szélesebb köre.
* Lásd: az [esetek tó adattár az](data-lake-store-data-scenarios.md) tó adattár használata helyzetekben, például adatok ingesting, adatok feldolgozása, adatainak letöltése és adatok megjelenítése.

## <a name="what-is-azure-data-lake-store-file-system-adl"></a>Mi az Azure tó adattár fájlrendszerben (adl: / /)?

Tó adattár azok webböngészőn keresztül az új formázáshoz, a AzureDataLakeFilesystem (adl: / /), Hadoop-környezetben (HDInsight fürthöz érhető el). Alkalmazások és szolgáltatások adl használó: / / további, amelyek nem érhető el a WebHDFS optimalizálása hasznát. Emiatt tó adattár megadhatók a hogy vagy a legjobb teljesítmény elérése érdekében javasolt a beállítást választja, a adl használatára: / / vagy a meglévő kódot karbantartása továbbra is a WebHDFS API közvetlenül használható. Azure hdinsight szolgáltatáshoz teljesen használja a AzureDataLakeFilesystem tó adattár biztosít a legjobb teljesítmény elérése érdekében.

Adatok ábrázolása a tó adattár segítségével elérheti `adl://<data_lake_store_name>.azuredatalakestore.net`. További információt olvashat elérni az adatokat az adatok tó áruházban, akkor olvassa el [a gyorsítótárban tárolt adatokat tulajdonságainak megtekintése](data-lake-store-get-started-portal.md#properties)

## <a name="how-do-i-start-using-azure-data-lake-store"></a>Hogyan kezdhetem meg, hogy Azure adatok tó tárolót használó?

Nézze meg [az Azure-portál tó adattár – első lépések használatával](data-lake-store-get-started-portal.md), egy tó adattár az Azure-portálon hozhatók létre. Azure adatok tó van kiépítve, miután nagy adatokat szeretne rendelni, például Azure adatok tó Analytics és Azure hdinsight szolgáltatáshoz használata tó áruházzal talál. Is létrehozhat a .NET-alkalmazások tó adattár Azure-fiók létrehozása és a műveleteket, például a feltöltés adatok, töltse le az adatok stb.

- [Azure adatok tó Analytics – első lépések](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Első lépések az Azure tó adattár .NET SDK használatával](data-lake-store-get-started-net-sdk.md)


## <a name="data-lake-store-videos"></a>Videók tó adattárhoz

Ha inkább videókból megtudhatja, hogy nézi, tó adattár videók számos szolgáltatást biztosít.

* [Azure tó adattár fiók létrehozása](https://mix.office.com/watch/1k1cycy4l4gen)
* [Az adatok Intézővel Azure tó adattár adatok kezelése](https://mix.office.com/watch/icletrxrh6pc)
* [Azure adatok tó Analytics csatlakoztatása Azure tó adattárhoz](https://mix.office.com/watch/qwji0dc9rx9k)
* [Az Access Azure tó adattár adatok tó Analytics keresztül](https://mix.office.com/watch/1n0s45up381a8)
* [Csatlakozás Azure tó adattár Azure hdinsight szolgáltatáshoz](https://mix.office.com/watch/l93xri2yhtp2)
* [Az Access Azure tó adattár struktúra és malac keresztül](https://mix.office.com/watch/1n9g5w0fiqv1q)
* [Másolja az adatokat, és az Azure tó adattár DistCp (Hadoop Distributed másolása) használatával](https://mix.office.com/watch/1liuojvdx6sie)
* [Adatok áthelyezése fiókok között relációs adatforrások és Azure tó adattár Apache Sqoop használatával](https://mix.office.com/watch/1butcdjxmu114)
* [Azure Data Factory használatának Azure tó adattár adatok üzletifolyamat-tervező](https://mix.office.com/watch/1oa7le7t2u4ka)
* [A Azure adatok tó tárolóban lévő adatok biztonságossá tétele](https://mix.office.com/watch/1q2mgzh9nn5lx)



