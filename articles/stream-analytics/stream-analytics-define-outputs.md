<properties
    pageTitle="Adatfolyam-elemző kimeneti értékeket: tároló analysis beállításainak |} Microsoft Azure"
    description="Tudjon meg többet Értékáram-elemzés kimeneti értékeket adatbeállítások többek között a Power BI-elemzés eredménye kiválasztásával."
    keywords="átalakítási, elemzés eredménye, adattárolási beállításainak"
    services="stream-analytics,documentdb,sql-database,event-hubs,service-bus,storage"
    documentationCenter="" 
    authors="jeffstokes72"
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="stream-analytics-outputs-options-for-storage-analysis"></a>Adatfolyam-elemző kimeneti értékeket: tároló analysis lehetőségei

Értékáram-elemzés feladat készítésekor fontolja meg, hogyan az eredményül kapott adatok elfogyasztott mennyiség. Hogyan fogja a találatok megtekintésekor a Értékáram-elemzés feladat, és hol, tárolja azt?

Ahhoz, hogy az alkalmazás mintázatok számos, az Azure Értékáram-elemzés – tárolásához kimeneti elemzés eredménye különböző beállítások tartalmaz. Ezzel megkönnyíti a feladat kimeneti megtekintése és adattárolási és egyéb célokra a felhasználás és a feladat kimenet tároló rugalmasságot biztosít. Bármely kimeneti konfigurálva a feladatot a léteznie kell, mielőtt a a feladat van és események megkezdése halad. Például ha használja a Blob-tárolóhoz olyan eredménye, a feladat nem hoz létre egy tároló fiók automatikusan. A felhasználó által létrehozott, a ASA feladat megkezdése előtt szükséges.

## <a name="azure-data-lake-store"></a>Azure tó adattárhoz

Értékáram-elemzés [Azure tó adattár](https://azure.microsoft.com/services/data-lake-store/)támogatja. A tároló lehetővé teszi, hogy minden méretét, a típus és a bevitel sebességét működési és felderítő analytics adatainak tárolása. Most létrehozása és konfigurálása tó adattár kimeneti értékeket támogatott csak az Azure klasszikus portálon. Értékáram-elemzés további, kell engedélyezhető az adatok tó áruházból eléréséhez. Hitelesítés és regisztrálni szeretne az Adatvillámnézet tó áruházból (ha szükséges) módját részletes [adatok tó kimeneti cikk](stream-analytics-data-lake-output.md)tárgyalja.

### <a name="authorize-an-azure-data-lake-store"></a>Az Azure adattár tó engedélyezése

Adattárolás tó kijelölt nyelve olyan eredménye az Azure adatkezelési portálon, a rendszer kéri, engedélyezése a meglévő adatok tó tárolóhoz kapcsolatot.  

![Tó adattár engedélyezése](./media/stream-analytics-define-outputs/06-stream-analytics-define-outputs.png)  

Alább látható módon a majd töltse ki a tó adattár kimenet tulajdonságait:

![Tó adattár engedélyezése](./media/stream-analytics-define-outputs/07-stream-analytics-define-outputs.png)  

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását, egy tó adattár kimenet készítéséhez szükséges.

<table>
<tbody>
<tr>
<td><B>TULAJDONSÁG NEVE</B></td>
<td><B>LEÍRÁS</B></td>
</tr>
<tr>
<td>Kimeneti Alias</td>
<td>Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a adatok tó áruház rövid nevét.</td>
</tr>
<tr>
<td>Fióknév</td>
<td>Ha szeretné elküldeni a kimeneti tó adattárolás fiók neve. Választhat a tartalmazó legördülő listát, amelyhez hozzáférése van a portálra bejelentkezett felhasználó tó adattár fiókok.</td>
</tr>
<tr>
<td>Elérési út előtag mintát [<I>választható</I>]</td>
<td>A fájl elérési útja, írja be a fájl a megadott adatok tó áruházból számla belüli használt. <BR>{date}, {idő}<BR>Példa: 1: mappa1/naplók / {date} / {time}<BR>Példa 2: mappa1/naplók / {date}</td>
</tr>
<tr>
<td>A dátumformátum [<I>választható</I>]</td>
<td>Ha a dátum jogkivonat az előtag elérési utat, választhat a dátumformátumot, amelyben a fájlok vannak rendezve. Példa: YYYY/hh/nn</td>
</tr>
<tr>
<td>Időformátum [<I>választható</I>]</td>
<td>Ha az időt jogkivonat az előtag elérési utat, adja meg az időformátumot, ahol a fájlok vannak rendezve. Jelenleg az egyetlen támogatott érték HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>Kimeneti adatai szerializálási formátum. JSON CSV és Avro támogatottak.</td>
</tr>
<tr>
<td>Kódolás</td>
<td>Ha a CSV- vagy JSON formázásához kódolást meg kell adni. UTF-8 jelenleg csak az támogatott kódolási formátumot.</td>
</tr>
<tr>
<td>Elválasztó karakterrel</td>
<td>Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos támogatja a CSV-adatok szerializálása. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:.</td>
</tr>
<tr>
<td>Formátum</td>
<td>Csak a JSON szerializálási alkalmazható. Vonal elválasztott Itt adhatja meg, hogy úgy, hogy minden új sor elválasztva JSON-objektum kimenetének lesz formázva. Tömb Itt adhatja meg, hogy a kimenet formátumú JSON objektumok tömbjét.</td>
</tr>
</tbody>
</table>

### <a name="renew-data-lake-store-authorization"></a>Tó adattár engedélyezési megújítása

Szüksége újra hitelesítést végezni tó adattár fiókjához, ha a jelszó megváltozott a feladatok létrehozásának vagy az utolsó hitelesített lesz.

![Tó adattár engedélyezése](./media/stream-analytics-define-outputs/08-stream-analytics-define-outputs.png)  


## <a name="sql-database"></a>SQL-adatbázis

[Azure SQL-adatbázis](https://azure.microsoft.com/services/sql-database/) olyan eredménye jellegű relációs adatok vagy egy relációs adatbázisban tárolt tartalom függő alkalmazásokhoz használható. Adatfolyam-elemző feladatok ír meglévő táblához Azure SQL-adatbázisban.  Figyelje meg, hogy a táblázat séma pontosan egyeznie kell a mezők és azok a feladatok kimenetét éppen típusát. Az [Azure SQL-adatraktár](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) egy kimenetként az SQL-adatbázis kimeneti beállítások, valamint (Ez a tulajdonság előzetes verzió) keresztül is meg kell adni. Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy SQL-adatbázis kimenet.

| Tulajdonság neve | Leírás |
|---------------|-------------|
| Kimeneti Alias | Ez a egy rövid nevet, használt lekérdezések irányítsa át a lekérdezés eredménye az adatbázishoz. |
| Adatbázis | Az adatbázis, ahol szeretné elküldeni a kimeneti neve |
| Kiszolgálónév | Az SQL-adatbázis-kiszolgáló neve |
| Felhasználónév | Az adatbázis írási jogosultsággal rendelkező felhasználónév |
| Jelszó | A jelszót az adatbázishoz való csatlakozáshoz |
| Táblázat | Hol kell írni a kimenet tábla neve. A táblázat neve kis-és nagybetűk, és meg kell egyeznie a séma a táblázat pontosan a mezők és azok a feladat kimeneti által készített típusát számát. |

> [AZURE.NOTE] Egy feladat kimeneti Értékáram-elemzés jelenleg az Azure SQL-adatbázis felületek használata támogatott. Az Azure virtuális gépen futó SQL Server csatolt adatbázis készítésének azonban nem támogatott. Ez a megváltozik a későbbi kiadásokban.

## <a name="blob-storage"></a>BLOB-tárolóhoz

BLOB-tárolóhoz felajánlja a nagy mennyiségű adatot strukturálatlan tárolásához a felhőben költséghatékony és méretezhető megoldást.  Azure Blob-tárolóhoz és annak használatát a Bevezetés a dokumentációjában [BLOB használatáról](../storage/storage-dotnet-how-to-use-blobs.md)a.

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy blob kimeneti.

<table>
<tbody>
<tr>
<td>TULAJDONSÁG NEVE</td>
<td>LEÍRÁS</td>
</tr>
<tr>
<td>Kimeneti Alias</td>
<td>Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a blob-tárolóhoz rövid nevét.</td>
</tr>
<tr>
<td>Tárterület-fiók</td>
<td>Ha szeretné elküldeni a kimeneti tárterület-fiók neve.</td>
</tr>
<tr>
<td>Tárterület Fiókkulcs</td>
<td>A titkos kulcs a tárhely partnernél.</td>
</tr>
<tr>
<td>Tárterület-tárolóhoz</td>
<td>Tárolók adja meg a Microsoft Azure Blob-szolgáltatásban tárolt BLOB-logikai csoportja. Amikor blob feltöltése a Blob-szolgáltatás, meg kell adnia, hogy a blob-tároló.</td>
</tr>
<tr>
<td>Elérési út előtag mintát [választható]</td>
<td>A fájl elérési útja, írja be a megadott tárolóban a BLOB használt.<BR>A PATH előfordulhat, hogy válasszon a következő 2 változót egy vagy több példány használatával adja meg az ismétlődési BLOB írt:<BR>{date}, {idő}<BR>Példa: 1: cluster1/naplók / {date} / {time}<BR>Példa 2: cluster1/naplók / {date}</td>
</tr>
<tr>
<td>A dátumformátum [választható]</td>
<td>Ha a dátum jogkivonat az előtag elérési utat, választhat a dátumformátumot, amelyben a fájlok vannak rendezve. Példa: YYYY/hh/nn</td>
</tr>
<tr>
<td>Időformátum [választható]</td>
<td>Ha az időt jogkivonat az előtag elérési utat, adja meg az időformátumot, ahol a fájlok vannak rendezve. Jelenleg az egyetlen támogatott érték HH.</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>Kimeneti adatai szerializálási formátum.  JSON CSV és Avro támogatottak.</td>
</tr>
<tr>
<td>Kódolás</td>
<td>Ha a CSV- vagy JSON formázásához kódolást meg kell adni. UTF-8 jelenleg csak az támogatott kódolási formátumot.</td>
</tr>
<tr>
<td>Elválasztó karakterrel</td>
<td>Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos támogatja a CSV-adatok szerializálása. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:.</td>
</tr>
<tr>
<td>Formátum</td>
<td>Csak a JSON szerializálási alkalmazható. Vonal elválasztott Itt adhatja meg, hogy úgy, hogy minden új sor elválasztva JSON-objektum kimenetének lesz formázva. Tömb Itt adhatja meg, hogy a kimenet formátumú JSON objektumok tömbjét.</td>
</tr>
</tbody>
</table>

## <a name="event-hub"></a>Esemény központi

[Esemény hubok](https://azure.microsoft.com/services/event-hubs/) erősen méretezhető közzététel előfizetés esemény ingestor. Események másodpercenként milliónyi gyűjt azt.  Egy egy esemény hubhoz ezt az eredményt használata esetén a kimeneti Értékáram-elemzés feladat adatfolyam állást változtat, a bemeneti lesz.

Vannak olyan esemény központi adatfolyamok beállítása egy kimenetként szükséges néhány paramétert.

| Tulajdonság neve | Leírás |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimeneti Alias | Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a esemény-hubon keresztül csatlakozott, egy rövid nevet. |
| Szolgáltatás Bus Namespace | A szolgáltatás Bus névtere üzenetküldés személyek halmaza tárolója. Ha létrehozott egy új esemény hubon keresztül csatlakozott, is létrehozott szolgáltatás Bus névteret |
| Esemény központi | Az esemény központi kimeneti neve |
| Esemény központi házirend neve | A megosztott access szabályt, amely az esemény központi konfigurálása lapon létrehozhatók. Minden megosztott hozzáférési házirend fog rendelkező lapjának a neve, beállított engedélyek, és a hívóbetűk |
| Esemény központi házirend kulcs | A megosztott hívóbetű hitelesíti a szolgáltatás Bus névtér eléréséhez |
| Partition kulcs oszlopban [választható] | Ez az oszlop tartalmazza a esemény központi kimeneti partíciót billentyűjét. |
| Esemény szerializálási formátum | Kimeneti adatai szerializálási formátum.  JSON CSV és Avro támogatottak. |
| Kódolás | Az CSV- és JSON UTF-8 jelenleg az egyetlen támogatott kódolás |
| Elválasztó karakterrel | Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos CSV-formátumban szerializálási adatok használatát támogatja. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:. |
| Formátum | Csak a JSON-típus alkalmazható. Vonal elválasztott Itt adhatja meg, hogy úgy, hogy minden új sor elválasztva JSON-objektum kimenetének lesz formázva. Tömb Itt adhatja meg, hogy a kimenet formátumú JSON objektumok tömbjét. |

## <a name="power-bi"></a>A Power BI

[Power BI](https://powerbi.microsoft.com/) elemzés eredménye egy gazdag képi megjelenítés élményét nyújtsanak egy kimenetként Értékáram-elemzés feladathoz használható. Ez a képesség műveleti irányítópultok, a jelentés létrehozásához és a jelentési alapú metrikus használható.

### <a name="authorize-a-power-bi-account"></a>Engedélyezheti a Power BI-fiók

1.  Ha olyan eredménye az Azure adatkezelési portált a Power BI van kiválasztva, kérni fogja engedélyezése a meglévő Power BI-felhasználó, vagy hozzon létre egy új Power BI-fiókot.  

    ![Engedélyezheti a Power BI felhasználói](./media/stream-analytics-define-outputs/01-stream-analytics-define-outputs.png)  

2.  Hozzon létre egy új fiókot, ha nem, mégis van, majd kattintson a engedélyezése gombra.  A képernyő, az alábbihoz hasonló jelennek meg.  

    ![Azure-fiók a Power BI](./media/stream-analytics-define-outputs/02-stream-analytics-define-outputs.png)  

3.  Ebben a lépésben a szükséges a munkahelyi vagy iskolai fiók engedélyezése a Power BI eredménye. Ha nem már jelentkezett a Power BI, válassza a bejelentkezési lehetőséget. A Power BI használt munkahelyi vagy iskolai fiókjával nem feltétlenül egyeznek az Azure előfizetés fiókból, amely jelenleg be van bejelentkezve.

### <a name="configure-the-power-bi-output-properties"></a>A Power BI kimeneti tulajdonságainak konfigurálása

Miután hitelesített Power BI-fiókkal rendelkezik, akkor tulajdonságait módosíthatja a Power BI kimeneti. Az alábbi táblázatban, a tulajdonságnév és azok leírását a Power BI kimeneti beállítása.

| Tulajdonság neve | Leírás |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimeneti Alias | Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a PowerBI teljesítményével rövid nevét. |
| Csoport-munkaterület | Ahhoz, hogy a Power BI másokkal megosztási adatok a Power BI számla belüli jelölje ki a csoportokat, vagy válassza ki a "Saját munkaterületet", ha nem szeretné írni a csoport.  Meglévő csoport frissítése a Power BI-hitelesítés megújítása szükséges. | 
| Adatkészlet neve | Adja meg az adatkészlet nevét, amely azt a Power BI kimeneti használatára van szükség |
| Táblázat neve | Adja meg a kívánt tábla neve alatt az adatkészlet a Power BI kimenet. Jelenleg a Power BI kimenetét Értékáram-elemzés feladatok csak lehet egy táblából egy adatkészlet |

A Power BI kimeneti- és irányítópult segédlet kérjük, témakörben található [Azure Értékáram-elemzés és a Power BI](stream-analytics-power-bi-dashboard.md) .

> [AZURE.NOTE] Nem kifejezetten létrehozni az adatkészlet és a táblázat a Power BI irányítópulton. Az adatkészlet és a táblázat tölti fel a automatikusan elindul, a feladatot, és a feladat szivattyúzó kimeneti indul el, a Power BI. Figyelje meg, hogy a feladat lekérdezés nem készítése a találatokat, ha az adathalmazban és a táblázat nem jön létre. Ügyeljen arra is, hogy a Power BI már egy adatkészlet és a megjelenítő Értékáram-elemzés feladat leírt egyik azonos nevű táblázat, ha a meglévő adatok felülíródnak.

### <a name="renew-power-bi-authorization"></a>A Power BI engedélyezési megújítása

Szüksége lesz, ha a jelszó megváltozott a feladatok létrehozásának vagy az utolsó hitelesített újból hitelesíteni a Power BI-fiókját. Ha többtényezős hitelesítés (MFA) be van állítva az Azure Active Directory (AAD) bérlőjén is szüksége lesz a Power BI engedélyezési minden 2 hétből megújítása. A jelenség, a probléma nem feladat kibocsátás és a "hitelesítés felhasználói hiba" a művelet naplókban:

  ![A Power BI frissítés jogkivonat hiba](./media/stream-analytics-define-outputs/03-stream-analytics-define-outputs.png)  

A probléma megoldásához a futó feladat leállítása, és nyissa meg a Power BI kimeneti.  A "Megújítása engedélyezési" hivatkozásra, és indítsa újra a feladat leállítása utoljára adatvesztés elkerülése érdekében.

  ![A Power BI megújítása engedély](./media/stream-analytics-define-outputs/04-stream-analytics-define-outputs.png)  

## <a name="table-storage"></a>Táblatároló

[Azure táblatárolóhoz](../storage/storage-introduction.md) felajánlja a könnyen hozzáférhető, nagymértékben méretezhető tárhely, hogy az alkalmazás automatikusan méretezheti felhasználói igényeknek. Táblatároló, melyik strukturált adatok a séma kevesebb korlátozásokkal is kihasználhatja a Microsoft NoSQL kulcs/attribútum áruházból. Azure táblatárolóhoz tárolja az adatokat az adatmegőrzési és hatékony lekérés használható.

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy táblázatot kimenet.

| Tulajdonság neve | Leírás |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimeneti Alias | Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a táblatárolóhoz rövid nevét. |
| Tárterület-fiók | Ha szeretné elküldeni a kimeneti tárterület-fiók neve. |
| Tárterület Fiókkulcs | A tároló fiókkal társított hívóbetű. |
| Táblázat neve | Annak a táblának a nevére. A tábla első létrejön, ha még nem létezik. |
| Partition billentyűt | A kimeneti oszlopban a partíciót kulcsot tartalmazó neve. A partíciók kulcsa a partíciót egy entitás elsődleges kulcs első részét képező adott táblázaton belül egy egyedi azonosítót. Érték karakterlánc, amelyek mérete legfeljebb 1 KB. |
| Sor billentyűt | A kimeneti oszlopban, a sor kulcsot tartalmazó neve. A sor egy egyedi azonosítót belül egy adott partíciót entitás kulcsa. Elsődleges kulcs egy entitás második részét képezi. A sor kulcsa, amelyek mérete legfeljebb 1 KB szöveges értékként. |
| Köteg mérete | A köteg művelet a rekordok számát. Általában az alapértelmezett érték elegendő a legtöbb feladatok, olvassa el a [táblázat köteg művelet specifikáció](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx) Ez a beállítás módosításával kapcsolatos további információt. |

## <a name="service-bus-queues"></a>A szolgáltatás Bus sorban várakozó

[Szolgáltatás Bus sorban várakozó](https://msdn.microsoft.com/library/azure/hh367516.aspx) tud nyújtani a első, első ki (FIFO) üzenetkézbesítés egy vagy több versengő felhasználóknak. Általában üzenetek várhatóan kapott, és a címzett, a időbeli sorrendben, amelyben a sor hozzáadásuk, és minden üzenet kapott, és csak egy üzenet fogyasztói által feldolgozott által feldolgozott.

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy várólista kimenet.

| Tulajdonság neve | Leírás |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimeneti Alias | Ez a használt lekérdezések irányítsa át a lekérdezés eredménye a szolgáltatás Bus sorba rövid nevét. |
| Szolgáltatás Bus Namespace | A szolgáltatás Bus névtere üzenetküldés személyek halmaza tárolója. |
| Várólista neve | A szolgáltatás Bus várólista neve. |
| A várakozási házirend neve | Várólista létrehozásakor a várólista konfigurálása lapon is létrehozhat megosztott hozzáférési. Minden megosztott hozzáférési házirend beállított engedélyek, nevet, és a hívóbillentyűk. |
| A várakozási házirend billentyűt | A megosztott hívóbetű hitelesíti a szolgáltatás Bus névtér eléréséhez |
| Esemény szerializálási formátum | Kimeneti adatai szerializálási formátum.  JSON CSV és Avro támogatottak. |
| Kódolás | Az CSV- és JSON UTF-8 jelenleg az egyetlen támogatott kódolás |
| Elválasztó karakterrel | Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos CSV-formátumban szerializálási adatok használatát támogatja. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:. |
| Formátum | Csak a JSON-típus alkalmazható. Vonal elválasztott Itt adhatja meg, hogy úgy, hogy minden új sor elválasztva JSON-objektum kimenetének lesz formázva. Tömb Itt adhatja meg, hogy a kimenet formátumú JSON objektumok tömbjét. |

## <a name="service-bus-topics"></a>Szolgáltatás Bus témakörök

Szolgáltatás Bus sorban várakozó biztosítása egy az egyhez kommunikációs módszer a feladó telefonkagylót, miközben [szolgáltatás Bus](https://msdn.microsoft.com/library/azure/hh367516.aspx) témakörök kommunikációs egy-a-többhöz űrlap.

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy táblázatot kimenet.

| Tulajdonság neve | Leírás |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kimeneti Alias | Ez a használt lekérdezések irányítsa át a lekérdezés eredménye szolgáltatás Bus témakör rövid nevét. |
| Szolgáltatás Bus Namespace | A szolgáltatás Bus névtere üzenetküldés személyek halmaza tárolója. Ha létrehozott egy új esemény hubon keresztül csatlakozott, is létrehozott szolgáltatás Bus névteret |
| Témakör neve | Témakörök vannak üzenetkezelés hasonló esemény hubok és sorban várakozó személyek. Esemény adatfolyamok gyűjt a számos különböző eszközök és szolgáltatások azokat is készült. A tematikus létrehozásakor is számítható egy egyedi nevet. A témakör küldött üzenetek nem lesznek elérhetők Ha nem előfizetés hoz létre, így biztosítani a témakör az egy vagy több előfizetések |
| A témakör házirend neve | A tematikus létrehozásakor is hozhat létre megosztott hozzáférési a témakör konfigurálása lapon. Minden megosztott hozzáférési házirend fog rendelkező lapjának a neve, beállított engedélyek, és a hívóbetűk |
| A témakör házirend billentyűt | A megosztott hívóbetű hitelesíti a szolgáltatás Bus névtér eléréséhez |
| Esemény szerializálási formátum | Kimeneti adatai szerializálási formátum.  JSON CSV és Avro támogatottak. |
| Kódolás | Ha a CSV- vagy JSON formázásához kódolást meg kell adni. UTF-8 jelenleg csak támogatott kódolás |
| Elválasztó karakterrel | Csak a CSV-szerializálási alkalmazható. Értékáram-elemzés közös határolójel számos CSV-formátumban szerializálási adatok használatát támogatja. Támogatott értékek vesszővel, pontosvesszővel, hely, lapon, és függőleges vonás:. |

## <a name="documentdb"></a>DocumentDB

[Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) egy teljes körű felügyelt NoSQL dokumentum adatbázis-szolgáltatás, amely felajánlja a séma ingyenes adatkészletekből, a kiszámíthatóvá és megbízható teljesítmény és a gyors fejlesztése lekérdezés és a tranzakciók.

Az alábbi táblázat felsorolja a tulajdonságok neve és azok leírását hozhat létre egy DocumentDB kimenet.

<table>
<tbody>
<tr>
<td>TULAJDONSÁG NEVE</td>
<td>LEÍRÁS</td>
</tr>
<tr>
<td>Fióknév</td>
<td>A DocumentDB fiók neve.  A fiók végpont is lehet.</td>
</tr>
<tr>
<td>Fiókkulcs</td>
<td>A megosztott hívóbetű DocumentDB fiók.</td>
</tr>
<tr>
<td>Adatbázis</td>
<td>Az adatbázis DocumentDB nevét.</td>
</tr>
<tr>
<td>A webhelycsoport neve mintával</td>
<td>A webhelycsoport neve mintájának a gyűjtemények használható. A webhelycsoport névformátum is alakítható ki a {partíciót} token használ, ahol partíciót a 0-tól indítása.<BR>Pl. A követő érvényes bemeneti adatok alapján:<BR>{Partíciót} MyCollection<BR>MyCollection<BR>Figyelje meg, hogy gyűjtemények léteznie kell, mielőtt a Értékáram-elemzés feladat azonnal elindul, és nem jön létre automatikusan.</td>
</tr>
<tr>
<td>Partition billentyűt</td>
<td>Az Itt adhatja meg a kimeneti szétválasztás keresztül gyűjtemények billentyűjét kimeneti események a mező neve.</td>
</tr>
<tr>
<td>Dokumentumazonosító</td>
<td>Az Itt adhatja meg az elsődleges kulcs kimeneti események, amelyek beszúrása műveletek vagy módosítása a mező nevét a alapul.</td>
</tr>
</tbody>
</table>


## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Korábban már ismerje meg az adatfolyam Analytics, egy felügyelt szolgáltatást a folyamatos átvitelű analytics az internetes dolgot adataiból. Ezzel a szolgáltatással kapcsolatos további tudnivalókért lásd:

- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
