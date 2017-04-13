<properties
    pageTitle="Adatok importálása az gépi tanulási Studio online adatforrásokból származó |} Microsoft Azure"
    description="Hogyan lehet az adatimportálás a oktatás Azure gépi tanulási Studio online különböző forrásokból származó."
    keywords="Importálás adatok, adatformátum, adattípusok, adatforrásokhoz, oktatóanyag adatok"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Adatok importálása az Azure gépi tanulási Studio adatforrásból különböző online, az adatok importálása modul

Ez a cikk ismerteti az online adatok importálása különböző forrásokból, és az alábbi forrásokból származó adatok áthelyezése Azure gépi tanulási kísérlet szükséges információk támogatása.

> [AZURE.NOTE] Ez a cikk az [Adatok importálása] általános információt tartalmaz[ import-data] modulra. Részletes információt a típusú adatok érheti el, formátumokat, paramétereket és a kapcsolatos gyakori kérdésekre adott válaszok témakör modul hivatkozási [Adatok importálása] [ import-data] modulra.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>– Bevezetés

Elérheti az Azure gépi tanulási Studio adatainak több online adatforrások közül a kísérlet az [Adatok importálása] használatával futása közben[ import-data] modul:

- A webes URL-cím HTTP használatával
- A Hadoop HiveQL használatával
- Azure blob-tárolóhoz
- Azure-táblából
- Azure SQL-adatbázis vagy SQL Server Azure virtuális
- A helyszíni SQL Server-adatbázishoz
- Egy adatcsatorna-szolgáltató, jelenleg OData
 
A munkafolyamat az Azure gépi tanulási Studio vezető kísérletek a vászonra húzása – és – húzással történő áthelyezéséről összetevőit áll. Online adatforrások elérésére, adja hozzá az [Adatok importálása] [ import-data] modult a kísérlet az **adatforrás**kijelölése, és adja meg a a paramétereket, az adatok eléréséhez szükséges. Az online adatforrások támogatottak az alábbi táblázatban vannak részletezve. Az alábbi táblázat összefoglalja a a támogatott fájlformátumok és az adatokhoz való hozzáféréshez használt paraméterek.

Ne feledje, hogy az oktatóanyag adatok érhető el a kísérlet futása közben, mert csak kísérletet érhető el. Összehasonlítva egy adatkészlet modulban tárolt adatok érhetők el a kísérletek a munkaterületen.

> [AZURE.IMPORTANT] Jelenleg az [Adatok importálása] [ import-data] és az [Adatok exportálása] [ export-data] modulok is adatainak olvasása és írása csak tárhelyről Azure a klasszikus telepítési modell alapján létre. Más szóval Azure Blob-tárolóhoz új ügyféltípus, amely felajánlja a meleg tároló hozzáférési szint vagy izgalmas tároló hozzáférési szint még nem támogatott. 

> Általánosságban elmondható, minden Azure tárterület-fiókból, hogy esetleg létrehozott szolgáltatás beállítás elérhetővé vált, mielőtt nem befolyásolja. Ha hozzon létre egy új fiókot szeretne, jelölje ki a **Klasszikus** a telepítési modell vagy erőforrás-kezelő, és **fiók típusát**, válassza az **Általános célú** , hanem **Blob-tárolóhoz**. 

> További tudnivalókért lásd: [Azure Blob-tárolóhoz: hideg- és melegvíz tároló rétegek](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Támogatott online adatforrások
Azure gépi tanulási **Adatimportálás** modul támogatja a következő adatforrásokhoz:

Adatforrás | Leírás | Paraméterek |
---|---|---|
HTTP-n keresztül, a webhely URL-címe |Vesszővel elválasztott értékek (CSV), tabulátorral tagolt értékek (TSV), attribútum-kapcsolata fájlformátum (ARFF) és támogatási vektoros gépek (SVM – világos) formátumot, az adatokat olvas bármely webes URL-cím HTTP használó | <b>URL-címe</b>: Adja meg a fájlban, például a webhely URL-CÍMÉT, és a fájlnevet, a bármelyik mellék teljes nevét. <br/><br/><b>Adatok formázása</b>: Adja meg a támogatott adatok egyik formátumok: CSV, TSV, ARFF vagy SVM – világos. Ha az adatokat a táblázat fejlécsora, oszlopnevek hozzárendelése használják.|
Hadoop/Fájlrendszerhez|A Hadoop elosztott tárhelyről adatokat olvas be. Megadhatja, hogy HiveQL, egy SQL-szerű lekérdezési nyelv használatával kívánt adatokat. HiveQL is használható adatok összesítése, és hajtsa végre az adatok szűrése, mielőtt gépi tanulási Studio hozzáadni az adatokat.|<b>Adatbázis-lekérdezés struktúra</b>: Adja meg a struktúra lekérdezés létre az adatokat.<br/><br/><b>HCatalog kiszolgáló URI</b> : a megadott formátumban *fürt neve megadott &lt;a fürt neve&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop felhasználói fiók neve</b>: a fürt létrehozására használt Hadoop-felhasználói fiók nevét adja meg.<br/><br/><b>Hadoop felhasználói fiók jelszava</b> : Itt adhatja meg a hitelesítő adatok kiépítésekor a fürt. További tudnivalókért lásd: [létrehozása Hadoop fürt a hdinsight szolgáltatásból lehetőségre](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Kimeneti adatok helyét</b>: Itt adhatja meg, hogy az adatok tárolása a Hadoop elosztott fájlrendszer (Fájlrendszerhez) vagy Azure-ban. <br/><ul>Ha Fájlrendszerhez kimeneti adatokat tárol, adja meg a Fájlrendszerhez kiszolgáló URI. (Ne felejtse el használja a HDInsight fürt nevét a HTTPS:// előtagot nélkül). <br/><br/>Ha Azure-ban tárolja az adatokat, meg kell adnia az Azure tároló fiók nevét, tároló hívóbetű és tároló tároló.</ul>|
SQL-adatbázis |Olvassa be az Azure virtuális gépen futó SQL Server-adatbázisból vagy Azure SQL-adatbázisban tárolt adatokhoz. | <b>Adatbázis-kiszolgáló neve</b>: Itt adhatja meg a nevét az adatbázis kiszolgálója.<br/><ul>Azure SQL-adatbázis esetén adja meg a kiszolgáló nevét, amely jön létre. A szokásos rajta az űrlap * &lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Az SQL server Azure virtuális gépre a szolgáltatott esetén adja meg a *tcp:&lt;virtuális gép tartománynév&gt;, 1433*</ul><br/><b>Az adatbázis neve </b>: Itt adhatja meg az adatbázis nevét, a kiszolgálón. <br/><br/><b>Kiszolgáló felhasználói fiók neve</b>: Itt adhatja meg a felhasználó nevét az adatbázis az access-engedélyekkel rendelkező fiókkal. <br/><br/><b>Kiszolgáló felhasználói fiók jelszava</b>: a felhasználói fiók jelszava.<br/><br/><b>Fogadja el a bármely kiszolgálói tanúsítvány</b>: ezt a beállítást (kevésbé biztonságos) használja, ha ki szeretné hagyni a webhely tanúsítvány megtekintésével, mielőtt, olvassa el az adatok.<br/><br/><b>Adatbázis-lekérdezés</b>: Adjon meg egy SQL-utasítást, amely leírja az elolvasni kívánt adatokat.|
A helyszíni SQL-adatbázis |Olvassa be egy helyszíni SQL-adatbázisban tárolt adatokhoz. | <b>Adatok átjáró</b>: az adatkezelési átjáró telepítve van egy olyan számítógépen, ahol az SQL Server-adatbázishoz elérhetik nevét adja meg. Az átjáró beállításával kapcsolatos további tudnivalókért olvassa el a [végrehajtása speciális analytics az Azure gépi tanulási egy helyszíni SQL server adatainak használata](machine-learning-use-data-from-an-on-premises-sql-server.md)című témakört.<br/><br/><b>Adatbázis-kiszolgáló neve</b>: Itt adhatja meg a nevét az adatbázis kiszolgálója.<br/><br/><b>Az adatbázis neve </b>: Itt adhatja meg az adatbázis nevét, a kiszolgálón. <br/><br/><b>Kiszolgáló felhasználói fiók neve</b>: Itt adhatja meg a felhasználó nevét az adatbázis az access-engedélyekkel rendelkező fiókkal. <br/><br/><b>Felhasználónév és jelszó</b>: kattintson <b>az Enter értékeket</b> a adatbázis hitelesítő adatok. Windows-hitelesítés és az SQL Server-hitelesítés attól függően, hogy a helyszíni SQL Server-beállításaitól is használhatja.<br/><br/><b>Adatbázis-lekérdezés</b>: Adjon meg egy SQL-utasítást, amely leírja az elolvasni kívánt adatokat.|
Azure-táblából|A táblázat szolgáltatásból Azure-tárolóban lévő adatokat olvas be.<br/><br/>Ha nagy mennyiségű adatot ritkán olvasni, használja az Azure-táblából szolgáltatás. Egy rugalmas biztosít nem relációs (NoSQL), nagymértékben méretezhető olcsó és erősen rendelkezésre álló tárhely megoldás.| Az **Adatok importálása** a beállítások módosítása attól függően, hogy nyilvános vagy a személyes tárterület-fiókot, amelyet a bejelentkezési hitelesítő adatokat kér, elérése. Ez határozza meg a <b>Hitelesítés típusa</b> beállíthatja, hogy "PublicOrSAS" vagy "Számla" értékét, amelyek saját rendelkezik a paraméterek. <br/><br/><b>Nyilvános vagy megosztott Access aláírás (Társítások) URI</b>: az paramétere:<br/><br/><ul><b>Táblázat URI</b>: Itt adhatja meg a nyilvános vagy Társítások URL-CÍMÉT a tábla.<br/><br/><b>Adja meg a sorok keresése a tulajdonságnév</b>: értékek <i>TopN</i> szeretne képet beolvasni a megadott számú sort vagy <i>ScanAll</i> a tábla minden sorának eléréséhez. <br/><br/>Ha az adatok homogén és kiszámítható, azt ajánljuk, hogy jelölje ki a *TopN* , és adjon meg egy számot n Nagy táblázatok esetében is Emiatt gyorsabban olvasási időpontokat.<br/><br/>Ha az adatok felépítése tulajdonságaik vannak, amelyek eltérő lehet a magassági és a táblázat alapján értékkészletet, válassza a szeretne képet beolvasni a minden sor *ScanAll* lehetőséget. Ezzel biztosíthatja, hogy a kapott tulajdonság és a metaadat-alapú átalakítás integritását.<br/><br/></ul><b>Magánjellegű tároló fiók</b>: az paramétere: <br/><br/><ul><b>Fióknév</b>: a fiók, olvassa el a táblázatot tartalmazó nevét adja meg.<br/><br/><b>Fiókkulcs</b>: Adja meg a annak a fióknak tároló billentyűt.<br/><br/><b>Táblanév</b> : olvassa el az adatokat tartalmazó tábla nevét adja meg.<br/><br/><b>Sorok keresése a tulajdonságnév</b>: értékek <i>TopN</i> szeretne képet beolvasni a megadott számú sort vagy <i>ScanAll</i> a tábla minden sorának eléréséhez.<br/><br/>Ha az adatok homogén és kiszámítható, azt javasoljuk, jelölje be a *TopN* , és adjon meg egy számot n Nagy táblázatok esetében is Emiatt gyorsabban olvasási időpontokat.<br/><br/>Ha az adatok felépítése tulajdonságaik vannak, amelyek eltérő lehet a magassági és a táblázat alapján értékkészletet, válassza a szeretne képet beolvasni a minden sor *ScanAll* lehetőséget. Ezzel biztosíthatja, hogy a kapott tulajdonság és a metaadat-alapú átalakítás integritását.<br/><br/>|
Azure Blob-tárolóhoz | Az Azure tárolására, például képek, a strukturálatlan szöveg vagy a bináris adatokat Blob szolgáltatásban tárolt adatokat olvas be.<br/><br/>Blob szolgáltatással nyilvánosan elérhetővé szeretné vagy magánjellegű az alkalmazás adatait tárolja. Az adatok bárhonnan elérhetők HTTP vagy HTTPS-kapcsolaton. | Az **Adatok importálása** modulban a beállítások módosítása attól függően, hogy nyilvános vagy a személyes tárterület-fiókot, amelyet a bejelentkezési hitelesítő adatokat kér, elérése. Ez határozza meg a <b>Hitelesítés típusa</b> , amelyre beállíthatja, hogy a "PublicOrSAS", vagy a "Számla" értéket.<br/><br/><b>Nyilvános vagy megosztott Access aláírás (Társítások) URI</b>: az paramétere:<br/><br/><ul><b>URI</b>: Adja meg a nyilvános vagy Társítások URL-cím a tárhely blob.<br/><br/><b>Fájlformátum</b>: Itt adhatja meg az adatok formátumát a Blob szolgáltatásban. A támogatott formátumok CSV, TSV és ARFF.<br/><br/></ul><b>Magánjellegű tároló fiók</b>: az paramétere: <br/><br/><ul><b>Fióknév</b>: a fiókot, amely tartalmazza az elolvasni kívánt blob nevét adja meg.<br/><br/><b>Fiókkulcs</b>: Adja meg a annak a fióknak tároló billentyűt.<br/><br/><b>A tároló, címtár, vagy blob elérési útja</b> : a blob, olvassa el az adatokat tartalmazó nevét adja meg.<br/><br/><b>BLOB-fájlformátum</b>: Itt adhatja meg az adatok formátumát a blob szolgáltatásban. A támogatott formátumok CSV, TSV, ARFF, CSV egy megadott kódolás és az Excel alkalmazásban. <br/><br/><ul>Ha a formátumot CSV- vagy TSV, feltétlenül jelzi, hogy a fájl tartalmaz-e a fejlécsor.<br/><br/>Olvassa el az adatok Excel-munkafüzetből használhatja az Excel lehetőséget. Jelzik az <i>Excel-adatok formátum</i> választógombot, hogy az adatok egy Excel-munkalap tartományban vagy az Excel-táblázat van-e. Az <i>Excel-munkalapra, vagy a beágyazott táblázat </i>lehetőséget adja meg a lap, vagy olvassa el a kívánt tábla neve.</ul><br/>|
Adatcsatorna-szolgáltató | Adatokat olvas be egy támogatott hírcsatorna-szolgáltatótól. Jelenleg csak az adatok protokollhoz (OData-) formátum támogatott. | <b>Adatok tartalomtípus</b>: Itt adhatja meg az OData formátumát.<br/><br/><b>Forrás URL-címe</b>: Adja meg a teljes URL-CÍMÉT az adatcsatornát. <br/>Például az alábbi URL-cím beolvassa a Northwind mintaadatbázis: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
