<properties
    pageTitle="Tároló – bevezetés |} Microsoft Azure"
    description="Azure tárolására, a Microsoft online adattárolás a felhőben áttekintése. Megtudhatja, hogy miként az alkalmazások használata a rendelkezésre álló felhőalapú tárolási legjobb megoldás."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>Bevezetés a Microsoft Azure-tárolóhoz

## <a name="overview"></a>– Áttekintés

Azure tároló a felhőalapú tárolási megoldást tartóssági, elérhetőségét és méretezhetőség az igényeknek megfelelően ügyfeleikkel támaszkodó modern alkalmazásokhoz. Ez a cikk, a fejlesztők, az informatikai szakemberek és üzleti döntések olvasásával készítők is további tudnivalók:

- Mi az Azure tárhely, és hogyan használatba előnye, hogy a felhőben, mobil, a kiszolgáló és az asztali alkalmazások
- Milyen típusú tárolhat az Azure tároló szolgáltatások: blob-adatok (objektum), NoSQL Táblázatadatok, üzenetek, és fájlmegosztások.
- Hogyan kezeli az Azure-tárolóban lévő az adatokhoz való hozzáférés
- Hogyan történik tartós redundancia és a replikáció az Azure tárolási adatok
- Ugrás a következő az első Azure tároló alkalmazás összeállítása helye

Lépéseket Azure adathordozós gyorsan című témakörben kaphat [Azure tároló öt perc az első lépések](storage-getting-started-guide.md).

Eszközök, tárak és más erőforrások: az Azure tároló használatához a részletekért olvassa el az alábbi [Lépéseket](#next-steps) .

## <a name="what-is-azure-storage"></a>Mi az Azure tároló?

Az a felhő lehetővé teszi, hogy új forgatókönyveket méretezhető, tartós és erősen rendelkezésre álló tárhely megkövetelése a adataikat – amely pontosan miért a Microsoft Azure tároló fejlesztett alkalmazások számítások. Mellett, amely lehetővé teszi a fejlesztők számára új eseteket támogató nagyméretű alkalmazások, Azure tárterület is biztosít a tárhely foundation az Azure virtuális gépeken futó, egy további testament annak robosztussági szeretne.

Azure tárterület is nagymértékben méretezhető, ezért tárolhatja és tudományos, pénzügyi elemzés, és a multimédia-alkalmazások által igényelt terabájt támogatja az adatok nagy jelenik meg az adatok több száz folyamata. Vagy a kis mennyiségű adat szükséges a kisvállalati webhelyet tárolhat. Tartoznak az igényeinek, ahol kifizeti csak az adatokat tárolja. Azure tároló jelenleg tárolja az egyedi ügyfél-objektumok trillions tízesre, és kérelmek egy második milliónyi átlagosan kezeli.

Azure tárterület is rugalmas, ezért a globális nagyobb közönség számára alkalmazások tervezéséről és - tárolt adatok mennyiségét és szemben kérelmek száma szükség szerint átméretezheti a ezeket az alkalmazásokat. Csak akkor használja, és csak ha vele fizet.

Azure tároló automatikus szétválasztás rendszer, amely automatikusan betöltés-balances az adatok alapján a forgalom használja. Ez azt jelenti, hogy a igények az alkalmazás megnövesztés jelennek meg, mint Azure tároló automatikusan osztja ki a megfelelő erőforrások felel meg őket.

Azure tároló érhető el bárhol az ügyfélgép, bármilyen típusú alkalmazás, hogy fut a felhőben, az asztalon, egy helyszíni kiszolgálót vagy egy mobil vagy a táblagép eszközt. A mobil olyan esetek, amikor az alkalmazás lévő adatok egy részhalmazát tárolja az eszközön, és szinkronizálja a teljes a felhőben tárolt adatokat tartalmazó Azure tárterület is használhatja.

Azure tárterület (beleértve a Windows és Linux) operációs rendszerek és (beleértve a .NET, Java, Node.js, Python, fonetikus, PHP C++ és mobil programnyelv) programnyelven számos különböző készletét használó kényelmes fejlesztési ügyfelek támogatja. Azure tárterület is elérhetővé teszi a via egyszerű REST API-hoz, a HTTP-/ HTTPS-e adatok küldésére és fogadására alkalmas bármilyen ügyfél számára rendelkezésre álló adatforrásai.

Azure prémium tároló I/O intenzív munkaterhelésekből fut a Azure virtuális gépeken futó nagy teljesítményű, alacsony-késés lemezt támogatása biztosítja. Azure prémium adathordozós több állandó adatok lemezre csatolni virtuális géphez, és állítsa be őket a teljesítmény követelményeknek. Egy SSD lemez maximális I/O teljesítményt Azure prémium tárolóban lévő minden adat lemezre biztonsági. Lásd: [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](storage-premium-storage.md) további információt.

## <a name="introducing-the-azure-storage-services"></a>Az Azure tároló Services bemutatása

Azure tárhelyet biztosít a következő négy szolgáltatások: Blob-tárhely, táblatároló, várólista tárterület és tárhely.

- BLOB-tárolóhoz objektum strukturálatlan adatokat tárolja. Blob szöveg vagy a bináris adatokat, például egy dokumentum, médiafájlok vagy alkalmazás installer bármilyen típusú is lehet. BLOB-tárolóhoz is nevezik objektum tárhelyet.
- Táblatároló strukturált adatkészleteket tárolja. Táblatároló a billentyű-attribútumok adatai gyors fejlesztés és nagy mennyiségű adat gyors hozzáférést tesz lehetővé NoSQL áruházból.
- Várólista-tároló megbízható üzenetküldést, munkafolyamat feldolgozása és felhőszolgáltatások összetevői közötti kommunikációhoz biztosít.
- Tárhely kínál a régebbi alkalmazásokhoz a szabványos kis-és Középvállalatok protokoll használatával megosztott tárolási. Azure virtuális gépeken futó és felhőszolgáltatások fájlban lévő adatok megoszthatja alkalmazásösszetevők keresztül csatlakoztatott megosztások keresztül, és a helyszíni környezetbe applications hozzáférhetnek fájl adatainak megosztás fájl Service REST API-t.

Az Azure tárolási egy egy biztonságos fiók, amely férhet hozzá az Azure-tárolóban lévő szolgáltatások. Tárterület-fiókját az egyedi névtér nyújt a tárhely erőforrásnak. Az alábbi képen az Azure tároló erőforrások tárterület-fiókjában közötti kapcsolatokat ismerteti:

![Azure tároló-erőforrások](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>BLOB-tárolóhoz

A nagy mennyiségű adatot strukturálatlan objektum felhasználók a felhőben tárolhatja Blob-tárolóhoz kínál költséghatékony és méretezhető megoldást. Blob-tárolóhoz használhatja tartalmak tárolását, például:

- Dokumentumok
- Közösségi adatok, például a fényképeket, videókat, zenei és blogok
- Biztonsági másolatok fájlokat, a számítógépek, a adatbázisok és eszközök
- Képek és szöveg webalkalmazások számára
- Konfigurációs adatok felhőben alkalmazáshoz
- Nagy adatokat, például a naplókat és más nagyméretű adatkészletek

Minden blob tároló van rendezve. Tárolók is kínál hasznos biztonsági házirendek hozzárendelése objektumok csoportjainak. A tároló fiók tárolók tetszőleges számú is tartalmazhat, és tároló tetszőleges számú BLOB legfeljebb 500 TB kapacitás korlátot a tárterület-fiókot is tartalmazhat.  

BLOB-tároló ajánlatok háromféle BLOB, blokkolása BLOB, hozzáfűző BLOB és lap BLOB (lemez).

- Blokkokból álló BLOB streaming és a felhő objektumokra tárolása optimalizálva, és a dokumentumok, a médiafájlokat, a biztonsági másolatok stb tárolásához kinek ajánljuk.
- Hozzáfűző BLOB blokk BLOB hasonló, de vannak optimalizálva Hozzáfűzés művelet. Egy hozzáfűző blob frissíthető csak hozzáadásával egy új szövegrészt a végére. Hozzáfűző BLOB, például a naplózás, ahol új adatok van szüksége ahhoz, hogy csak a blob végén az esetek kinek ajánljuk.
- Lap BLOB optimalizálva IaaS lemezre jelölő és támogató véletlen ír, és lehetséges, hogy a mérete legfeljebb 1 TB. Az Azure virtuális gép hálózat lemez egy virtuális, oldal blob-ként tárolt IaaS csatolt.

Nagyon nagy adatkészletek, ahol a hálózati korlátozások teszik feltöltésének vagy letöltésének adatok Blob-tárolóhoz a hálózaton irreális-szállított Microsoft importálása és exportálása az adatokat közvetlenül az Adatközpont a merevlemez-meghajtó. Lásd: [a Microsoft Azure importálás/exportálás szolgáltatással átadása Blob-tároló az adatokat](storage-import-export-service.md).

## <a name="table-storage"></a>Táblatároló

Alkalmazásokat modern gyakran igény nagyobb méretezhetőség és rugalmasság, mint az előző generációs kötelező szoftver-adatokat tárolja. Táblatároló felajánlja a könnyen hozzáférhető, nagymértékben méretezhető tárhely, hogy az alkalmazás automatikusan méretezheti felhasználói igényeknek. Táblatároló a Microsoft NoSQL kulcs/attribútum áruház – schemaless látványterv, így különböző hagyományos relációs adatbázisból tartalmaz. A schemaless adattár nagyon egyszerűen is alkalmassá teheti az adatok, az alkalmazás evolve szükségleteinek. Táblatároló is könnyű kezelhetőséget, ezért a fejlesztők gyorsan hozhat létre alkalmazásokat. Hozzáférés az adatokhoz gyors és az alkalmazás az összes különféle költséghatékony.  A művelet rendszerint költség jelentősen kisebb, mint a hagyományos SQL-hasonló mennyiségű adattal táblatároló.

Táblatároló egy kulcsot-attribútum mentése, tehát, hogy minden táblában értéket beírt tulajdonság néven tárolja. A tulajdonság neve a szűrés és kiválasztási feltételek megadásával használható. Tulajdonságok és értékeik gyűjteménye entitás tartalmazzák. Mivel táblatároló schemaless, a táblázatból két entitás tulajdonságok különböző gyűjteményeit is tartalmazhat, és tulajdonságokat a különböző is lehet.

Rugalmas adatkészleteket, például a webalkalmazások, címjegyzékek, eszköz adatait és más típusú metaadatokat a szolgáltatáshoz felhasználói adatok tárolására táblatároló is használhatja.  Tetszőleges számú személyek is tárolnia egy táblában, és egy tárterület-fiókot is tartalmazhatnak tetszőleges számú táblázatok, a tárhely fiók kapacitás határértékén felfelé.

BLOB és a sorok, például a fejlesztők kezelése és tábla eléréséhez protokoll segítségével szabványos többi tárhely, azonban a táblatároló is támogatja a OData protokollhoz, csak egy részhalmazát egyszerűsítése speciális funkciókat lekérdezése és engedélyezi az JSON és atompub protokollok használatával (XML-alapú) formátumot.

Az aktuális internetes alkalmazások például táblatároló NoSQL adatbázisok kínálnak a népszerű ahelyett, hogy hagyományos relációs adatbázisok.

## <a name="queue-storage"></a>A várakozási tárhely

Az alkalmazások skála tervez, alkalmazásösszetevők is gyakran leválasztott, úgy, hogy azok a független méretezheti. Várólista-tároló megbízható üzenetben megoldást kínál aszinkron kommunikációját alkalmazásösszetevők, akár a felhőben, az asztalon, a helyszíni kiszolgálón vagy mobileszközön futnak. Várólista-tároló támogatja a aszinkron feladatok kezelése és a munkafolyamatok fejlesztésére is.

A tárterület-fiók a sorok értéke is tartalmazhat. Egy tetszőleges számú a tárterület-fiók kapacitás határértékén felfelé az üzenetek is tartalmazhat. Lehet, hogy az egyes üzenetek mérete legfeljebb 64 KB.

## <a name="file-storage"></a>Tárhely

Azure fájltároló felajánlja a felhőalapú kis-és Középvállalatok fájlmegosztások, hogy áttelepítheti a régebbi alkalmazások támaszkodó fájlmegosztások Azure használatával gyorsan és költséges felülírja nélkül. Azure fájltároló az Azure virtuális gépeken futó vagy felhőszolgáltatások futó alkalmazások ugyanúgy, mint egy asztali alkalmazás csatlakoztatja egy tipikus kis-és Középvállalatok megosztás csatlakoztathat egy fájl megosztása a felhőben. Tetszőleges számú alkalmazásösszetevők csatlakoztatása majd egyidejű elérni a fájlt tároló megosztás.

Mivel a tárhely fájlmegosztás kis-és Középvállalatok szabványos fájlmegosztás, Azure-ban futó alkalmazások adatok megosztása fájl rendszert I/O API-khoz keresztül érheti el. A fejlesztők ezért is kihasználhatja a meglévő alkalmazások áttelepítendő szakértelemmel, valamint a meglévő kódot. Az informatikai szakemberek PowerShell-parancsmagok segítségével létrehozása, csatlakoztatása és fájlmegosztások tárhely kezelése az Azure-alkalmazások felügyeleti részeként.

Más Azure tároló szolgáltatások, például a fájltároló eléréséhez a megosztás közzététele REST API-t. Helyszíni alkalmazások felhívhatja a fájlt tároló hozzáférés az adatokhoz fájlmegosztás REST API-t. Ezzel a módszerrel egy vállalati választhat áttelepítendő Azure néhány régebbi alkalmazást, és folytathatja a mások nem fut a saját a szervezeten belül. Ne feledje, hogy fájlmegosztásról csatlakoztatása csak lehetséges Azure; az alkalmazások a helyszíni alkalmazás csak férhetnek hozzá a fájlmegosztás keresztül a REST API-t.

Elosztott alkalmazások fájltároló tárolására és megosztására a hasznos alkalmazás adatok és a fejlesztés és a vizsgálati eszközöket is használhatja. Az alkalmazások tárolhatnak konfigurációs fájlok és a diagnosztikai adatok, például a naplókat, mértékek és összeomlik kiírása tároló megosztása, hogy érhetők el a több virtuális gépeken futó vagy szerepkörök fájlban. A fejlesztők és a rendszergazdák tárolhat segédprogramok, összeállítása és minden összetevő, rendelkezésre álló tárhely fájlmegosztás az alkalmazások kezelése van szükségük a telepít minden virtuális gép vagy szerepkör-példány helyett.

## <a name="access-to-blob-table-queue-and-file-resources"></a>Hozzáférés Blob, tábla, várólista és fájl erőforrások

Alapértelmezés szerint csak a tárhely fióktulajdonos hozzáférhet erőforrásokat a tárterület-fiókjában. Az adatok biztonsága ellen erőforrások fiókban minden kérelme sikerült hitelesíteni. Hitelesítés egy megosztott kulcs modell támaszkodik. BLOB is beállítható úgy, hogy névtelen hitelesítés támogatására.

Tárterület-fiókja van-e hozzárendelve két magánjellegű hívóbetűk a létrehozását hitelesítéshez használt. Két billentyűk biztosítja, hogy az alkalmazás továbbra is elérhető marad, amikor a közös biztonsági kulcskezelő ajánlott rendszeresen újragenerálása a billentyűket.

Ha kell felhasználók ellenőrzött elérésének a tároló erőforrásokat, majd létrehozhat egy megosztott access aláírást. A megosztott access aláírás (Társítások) jogkivonat, amely egy URL-címre, amely lehetővé teszi a meghatalmazott hozzáférést tároló erőforrás fűzhetők. Bárki, aki rendelkezik a token hozzáférhet az erőforrás mutat a engedélyekkel, adja meg, az időtartamot, hogy nem érvényes. 2015-04-05 verzió kezdve Azure tároló támogatja a kétféle átengedése aláírások: Társítások szolgáltatás és a fiók Társítások.

A szolgáltatás Társítások delegálja a tárterület-szolgáltatások csak egy erőforrás való hozzáférést: a Blob, várólista, táblázat vagy fájl szolgáltatást.

Fiók Társítások delegálja a tárhely szolgáltatások közül legalább egyet az erőforrások elérését. Access szolgáltatói műveletek, amelyek nem találhatók meg a szolgáltatás Társítások átadhatja. A meghatalmazás olvasni, írja be és blob tárolók, táblák, sorok és Társítások szolgáltatás nem megengedettek fájlmegosztások lévő műveletek törlése.

Végezetül adhatja meg, hogy a tároló és annak BLOB vagy egy adott blob érhetők el a nyilvános elérés. Ha egy tároló vagy blob nyilvános, bárki is elérhető legyen névtelenül; hitelesítés nem kötelező.  Nyilvános tárolók és BLOB hasznosak erőforrások, például a média és webhelyeken tárolt dokumentumok ki.  Ha csökkenteni szeretné hálózati késés globális célközönséghez, gyorsítótárba helyezi az Azure CDN a webhely által használt blob-adatok.

További információt a megosztott access aláírások [Használata megosztott Access aláírások (Társítások)](storage-dotnet-shared-access-signature-part-1.md) megtekintése További információt a tárterület-fiókjába, biztonságos hozzáférés [kezelése névtelen olvasási hozzáférést tárolók és BLOB](storage-manage-access-to-resources.md) és a [hitelesítéshez az Azure tárolási szolgáltatások](https://msdn.microsoft.com/library/azure/dd179428.aspx) megtekintése

## <a name="replication-for-durability-and-high-availability"></a>Replikációs tartóssági és magas elérhetősége

A Microsoft Azure tároló fiók adatainak mindig replikált, tartóssági és elérhetőséget biztosítása érdekében. A replikáció másolja az adatokat, akár az azonos adatközpont, akár egy második adatközpontja, attól függően, hogy melyik replikációs lehetőséget választja. A replikáció védi az adatokat, és megőrzi az alkalmazás up – időbeosztás megelőzve tranziens hardver hibák. Ha az adatok egy második adatközpontja van replikált, amely is megakadályozza, hogy az adatok egy elsődleges hely Katasztrofális hiba.

Replikációs biztosítja, hogy a tárterület-fiók megfelel-e a [Szolgáltatói szerződést (SLA) tárolására](https://azure.microsoft.com/support/legal/sla/storage/) is hibák szemben. Lásd: az Azure tároló információt SLA garantálja tartóssági és elérhetősége. 

Amikor létrehoz egy tárterület-fiókkal, a következő replikációs lehetőségek közül választhat:  

- **Helyi meghajtóra felesleges tárhely (LRS).** Helyi meghajtóra felesleges tároló három példányt az adatok kezeli. LRS van replikált háromszor egyetlen adatközpont, egy egyetlen területén belül. A normál hardver hibák, de nem egy egyetlen adatközpont meghibásodása, LRS védi az adatokat.  

    LRS árengedménnyel lehetősége. Maximális tartósság ajánlott geo felesleges tároló leírása alább használható.


- **Zóna felesleges tárhely (ZRS).** Zóna felesleges tárolási fenntartja az adatok három példányát. ZRS háromszor között két vagy három létesítményekhez, egyetlen terület belül vagy két területek között van replikált, LRS-nál magasabb tartóssági kezeléséről. ZRS biztosítja, hogy az adatok egy egyetlen régión belüli tartós.  

    ZRS biztosít eltarthatóság LRS;-nál magasabb szintre azonban maximális tartósság ajánlott geo felesleges tárolási leírása alább használható.  

    > [AZURE.NOTE] ZRS a problémának jelenleg csak az továbbfejlesztett fájlblokkolás BLOB, és csak támogatott verziójához 2014-02-14 vagy újabb verzió.
    >
    > Miután létrehozta a tárterület-fiók és ZRS kijelölve, akkor nem konvertálható való replikáció, más típusú használata (vagy fordítva).

- **Geo felesleges tárterület (GRS)**. GRS megtartja az adatok hat másolatát. GRS az adatok van replikált háromszor a elsődleges régión belüli és is replikált háromszor egy másodlagos régiójában több száz mérföld az elsődleges régió, a legmagasabb szintű tartóssági nyújtó hagyta. Az elsődleges régió a hiba esetén Azure tároló lesz a másodlagos régió áttérni. GRS biztosítja, hogy az adatok tartós két külön régióban.

    Elsődleges és másodlagos párosítása régió szerint kapcsolatos tudnivalókért lásd: [Azure régiók](https://azure.microsoft.com/regions/).

- **Olvasás-elérés geo felesleges tároló (TS-GRS)**. Olvasásra geo felesleges tároló replikálja az adatok egy másodlagos földrajzi helyét, és is az adatok az kiegészítő helyén olvasási hozzáférést biztosít. Olvasásra geo felesleges tároló lehetővé teszi hozzáférhet az adatok az elsődleges vagy a másodlagos helyre abban az esetben, ha egy helyen nem érhető el. Olvasási-hozzáférés geo felesleges tároló az alapértelmezett beállítás alapértelmezés szerint a tárterület-fiók esetén létrehozni a dokumentumot. 

    > [AZURE.IMPORTANT] Módosíthatja, hogy hogyan az adatok van replikált a tárterület-fiók létrehozását követően másképp ZRS a fiók létrehozásakor. Megjegyzendő, hogy egy további egyszeri adatátvitel költséget, ha az LRS GRS vagy TS-GRS fellépő.

Kapcsolatos további tudnivalók: [Azure tároló replikációs](storage-redundancy.md) további tárterület replikációs beállításai.

Tárterület-fiók replikációs árinformációkat, [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/)talál. Lásd: [Azure régiókban](https://azure.microsoft.com/regions/#services) milyen szolgáltatások érhetők el az egyes régiókra kapcsolatban további tudnivalókat.

Azure adathordozós tartóssági építészeti olvashat, lásd: [SOSP papír - Azure tároló: A nagyon elérhető felhőalapú Tárhelyszolgáltatáshoz az erős konzisztencia](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).


## <a name="transferring-data-to-and-from-azure-storage"></a>Adatok átvitele, hogy és Azure tárhelyről

A AzCopy parancssori segédprogrammal blob, a fájl, és a táblázatok adatait a másolandó belül a tárterület-fiók vagy a tárterület-fiókok. Lásd: az [adatok, mellettük a AzCopy parancssori segédprogram átadása](storage-use-azcopy.md) további információt.

AzCopy épülő az [Azure adatforrástárban mozgását](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/), amely a problémának jelenleg előzetes verzióban.

Az importálás/exportálás Azure szolgáltatás be blob-adatok importálása vagy blob-adatok exportálása az Azure adatközpont megkap merevlemez-meghajtó lemezen a tárhely fiók megoldást. Az importálás/exportálás szolgáltatással kapcsolatos további tudnivalókért lásd: az [adatok átvitele Blob-tárolóhoz a Microsoft Azure importálás/exportálás szolgáltatással](storage-import-export-service.md).

## <a name="pricing"></a>Árak

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>Tároló API, tárak és eszközök

Azure tároló-erőforrások is használhatók fel, hogy HTTP-/ HTTPS-kérések bármely nyelvre. Emellett a Azure tároló felajánlja a különféle programozási tárak számos népszerű nyelvhez. A tárak használata a Azure tárhely kezelése a részleteket, például szinkron és aszinkron hívás, és így tovább kötegelés műveletek, kivétel kezelése, automatikus újrapróbálkozások, működési viselkedés számos tulajdonságát egyszerűsítése érdekében. Tárak jelenleg érhetők el az alábbi nyelvek és platformokon másokkal a során:

### <a name="azure-storage-data-services"></a>Azure adatainak tárolása

- [Tárterület-szolgáltatások REST API-val](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Tárterület ügyfél tár .NET, a Windows Phone és a Windows futtatókörnyezet](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Tárterület ügyfél dokumentumtár a C++](https://github.com/Azure/azure-storage-cpp)
- [Tárterület ügyfél tár Java/androidra](/develop/java/)
- [Tárterület ügyfél dokumentumtár a Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [Tárterület ügyfél dokumentumtár a PHP](/develop/php/)
- [Tárterület ügyfél dokumentumtár a fonetikus](/develop/ruby/)
- [Tárterület ügyfél dokumentumtár a Python](/develop/python/)
- [Tárterület-parancsmagok az PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure tároló szolgáltatások

- [Tárterület erőforrás szolgáltató REST API-hivatkozás](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [Tárterület erőforrás szolgáltató ügyfél tár a .NET rendszerhez](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [Tárterület erőforrás szolgáltató PowerShell 1.0 parancsmagjai](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [Tárterület szolgáltatás felügyeleti REST API (klasszikus)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure tároló mozgását adatszolgáltatások

- [Tároló importálás/exportálás szolgáltatás REST API-val](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [Tárterület mozgását ügyfél adatforrástárban a .NET rendszerhez](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>Eszközök és segédprogramok

- [Azure tároló Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure tároló ügyféleszközök elől](storage-explorers.md)
- [Azure SDK és eszközök](https://azure.microsoft.com/tools/)
- [Azure tároló irányító](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy parancssori segédprogram](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>Következő lépések

Azure tároló olvashat az alábbi források:

### <a name="documentation"></a>Dokumentáció

- [Azure tároló dokumentáció](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>A rendszergazdák számára

- [Azure adathordozós Azure PowerShell használatával](storage-powershell-guide-full.md)
- [Azure tárolási Azure CLI használata](storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET-fejlesztők számára

- [Első lépések a .NET használatával Azure Blob-tárolóhoz](storage-dotnet-how-to-use-blobs.md)
- [Azure táblatárolóhoz .NET használata – első lépések](storage-dotnet-how-to-use-tables.md)
- [Azure várólista-tároló .NET használata – első lépések](storage-dotnet-how-to-use-queues.md)
- [A Windows Azure fájltároló – első lépések](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>Java/Android fejlesztők számára

- [Használatáról a Java Blob-tárolóhoz](storage-java-how-to-use-blob-storage.md)
- [A Java táblatároló használata](storage-java-how-to-use-table-storage.md)
- [Java várólista tárhelyet használata](storage-java-how-to-use-queue-storage.md)
- [Java a tárhely használata](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>Node.js fejlesztők számára

- [Használatáról a Node.js Blob-tárolóhoz](storage-nodejs-how-to-use-blob-storage.md)
- [A Node.js táblatároló használata](storage-nodejs-how-to-use-table-storage.md)
- [A Node.js várólista-tároló használata](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP fejlesztők számára

- [Használatáról a PHP Blob-tárolóhoz](storage-php-how-to-use-blobs.md)
- [A PHP táblatároló használata](storage-php-how-to-use-table-storage.md)
- [A PHP várólista-tároló használata](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>A fonetikus fejlesztők számára

- [A fonetikus Blob-tárolóhoz használata](storage-ruby-how-to-use-blob-storage.md)
- [A fonetikus táblatároló használata](storage-ruby-how-to-use-table-storage.md)
- [A fonetikus várólista-tároló használata](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python fejlesztők számára

- [Használatáról a Python Blob-tárolóhoz](storage-python-how-to-use-blob-storage.md)
- [A Python táblatároló használata](storage-python-how-to-use-table-storage.md)
- [A Python várólista-tároló használata](storage-python-how-to-use-queue-storage.md)
- [A Python fájltároló használata](storage-python-how-to-use-file-storage.md)
