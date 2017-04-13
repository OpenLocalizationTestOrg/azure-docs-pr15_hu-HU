<properties
    pageTitle="Az importálás/exportálás használatával adatok átvitele Blob-tárolóhoz |} Microsoft Azure"
    description="Megtudhatja, hogy miként létrehozása importálása és exportálása a feladatok adatok blob-tároló továbbítani szeretné az Azure klasszikus portálon."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>A Microsoft Azure importálás/exportálás szolgáltatás használatával adatok átvitele Blob-tárolóhoz

## <a name="overview"></a>– Áttekintés

Azure importálás/exportálás szolgáltatáson keresztül biztonságosan átadása szerint a merevlemez-meghajtók az Azure adatközpontja szállítási Azure blob-tárolóhoz nagy mennyiségű adatot. Ez a szolgáltatás Azure blob-tárolóhoz adatok átvitele merevlemez-meghajtók és a helyszíni webhely szállítási is használhatja. A szolgáltatás nem megfelelő esetekben, ahol szeretné vinni az adatok az Azure több TBs, de feltöltésének vagy letöltésének a hálózaton keresztül nem lehetséges korlátozott sávszélesség vagy magas hálózati költségek miatt.

A szolgáltatás igényel, hogy a merevlemez-meghajtók kell-e az adatok biztonsága érdekében titkosított bit tároló. A szolgáltatás lehetővé teszi a klasszikus tároló fiókok nyilvános Azure területe szerepel. A támogatott, a jelen cikk megadott helyek valamelyikén merevlemez-meghajtók kell kiszállítása.

Ebben a cikkben többet megtudni az importálás/exportálás Azure szolgáltatás és az adatok másolása, és az Azure Blob-tárolóhoz meghajtók szállítandó megtanulhatja.

> [AZURE.IMPORTANT] Hozzon létre és importálás kezeléséhez, és a klasszikus portál vagy az [Importálás/exportálás szolgáltatás REST API -k](http://go.microsoft.com/fwlink/?LinkID=329099)használata a klasszikus tároló feladatok exportálása. Erőforrás-kezelő tároló fiókok a egyelőre nem támogatottak.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Mikor érdemes használni az importálás/exportálás Azure szolgáltatás?

Megpróbálkozhat Azure importálás/exportálás szolgáltatás feltöltésekor vagy adatok letöltése a hálózaton keresztül túl lassú, vagy tiltják költség első további hálózati sávszélesség áll.

Használhatja ezt a szolgáltatást az esetek például:

- Adatok áttelepítése a felhőbe: nagy mennyiségű adatot áthelyezése Azure gyorsan és hatékony költség.
- Tartalomtípus-eloszlás: gyorsan adatok küldése a ügyfél-webhelyekhez.
- Biztonsági mentése: A helyszíni adatok tárolása Azure blob-tárolóhoz a biztonsági másolatok készítése.
- Adatok helyreállítása: nagy mennyiségű blob-tárolóban lévő adatok helyreállítása, és azt a helyszíni helyre kézbesítve.

## <a name="pre-requisites"></a>Előzetes követelmények

Ebben a részben azt szerepel a előtti követelmények, ez a szolgáltatás használatához szükséges. Véleményezze a következőt őket gondosan, mielőtt a meghajtókon.

### <a name="storage-account"></a>Tárterület-fiók

Azure meglévő előfizetéshez és egy vagy több **Klasszikus** tároló fiókok lehetőséget az importálás/exportálás szolgáltatással kell rendelkeznie. Minden feladat át szeretné vagy csak egy klasszikus tárterület-fiókból adatok használhatók. Ez azt jelenti egyetlen importálás/exportálás feladat időtartamát nem több tárterületet fiók között. Megtudhatja, [hogy miként tárterület-fiók létrehozása](storage-create-storage-account.md#create-a-storage-account)olvashat a tárhely új fiók létrehozása.

### <a name="blob-types"></a>BLOB-típusok

Azure importálás/exportálás szolgáltatással adatok másolása **blokk** BLOB vagy **lap** BLOB. Viszont csak exportálhatja **blokk** BLOB, **oldal** BLOB vagy **hozzáfűző** BLOB Azure tárhelyről ezzel a szolgáltatással.

### <a name="job"></a>Feladat

Importálása vagy exportálása a Blob-tárolóhoz a folyamat indításához először létrehozhat egy feladatot. A feladat lehet egy importálása vagy az Exportálás feldolgozás:

- Hozzon létre egy importálás szeretné vinni az Azure tárterület-fiókja van BLOB a helyszíni adatok.
- Hozzon létre egy Exportálás feladatot, ha adatátvitel jelenleg tárolja a BLOB-tároló fiókjában, szállított merevlemez-meghajtók szeretné.

Amikor létrehoz egy feladatot, akkor értesítse az importálás/exportálás szolgáltatás, hogy Ön fog kell szállítási egy vagy több merevlemez-meghajtók az Azure adatközpontja.

- Hogy az importálás akkor fog kell szállítási merevlemezeken lévő adatokat tartalmazó.
- Az Exportálás feladat meg fog kell szállítási üres merevlemez-meghajtók.
- Legfeljebb 10 merevlemez-meghajtók Feladatonkénti is kiszállítása.

Létrehozhat importálási vagy exportálja a feladatot a [Klasszikus portál](https://manage.windowsazure.com/) vagy az [Azure tároló importálás/exportálás REST API -val](http://go.microsoft.com/fwlink/?LinkID=329099)segítségével.

### <a name="client-tool"></a>Ügyfél-eszköz

Az első lépés egy **importálása** feladat létrehozása a megrendelt meghajtón Felkészülés importálása. Felkészülés a meghajtóra, csatlakoztassa helyi kiszolgálót, és az Azure importálás/exportálás ügyfél eszköz futtatása a helyi kiszolgálón. Az ügyfél eszközben lehetővé teszi az adatok másolása a meghajtóra, az adatok BitLocker meghajtón titkosítása és a meghajtó lapjának fájlok létrehozása.

A napló fájlokat a feladatot, és a meghajtón, például meghajtó dátumértékét és a tárhely fióknév alapvető adatainak tárolására. A napló fájl nem tárolja a meghajtón. Importálás feladat létrehozása során használatos. Feladat létrehozása részleteket lépésről lépésre a jelen cikk találhatók.

64 bites Windows operációs rendszer lehetőség csak a ügyfél eszközt. Az adott operációs rendszer verziója támogatott [operációs rendszer](#operating-system) szakaszban olvashat.

Töltse le az [Azure importálás/exportálás ügyfél eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)legújabb verzióját. Az Azure importálás/exportálás eszköz használatával kapcsolatos további tájékoztatásért lásd: az [Azure importálás/exportálás eszköz hivatkozást](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Merevlemez-meghajtók

Csak a 3,5 hüvelykes SATA II/III belső merevlemez-meghajtók támogatottak az importálás/exportálás szolgáltatással való használatra. Használhatja a merevlemez-meghajtók legfeljebb 10TB.
Importálás feladatokhoz adatok mennyiségi meghajtón csak az első feldolgozása lesz. Az adatok mennyiségi NTFS van formázva.
A merevlemezen tárolt adatok másolása, amikor csatolhat közvetlenül a SATA összekötők használatával, illetve csatolhat külső felekkel használatával egy külső SATA II/III USB-adaptert. Azt javasoljuk, hogy az alábbi külső SATA II/III USB-adaptert egyik:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Ha egy konverter, amely nem szerepel a fenti, megpróbálhatja fut az Azure importálás/exportálás eszköz használatával a konverter előkészítése a meghajtóba, és látni, ha egy támogatott konverter vásárlása előtt működik.

> [AZURE.IMPORTANT] Ez a szolgáltatás által nem támogatott külső merevlemez-meghajtók egy beépített USB-adaptert mellékelt. Emellett a lemezt a géphez, egy külső merevlemez belül nem használhatók; Ne küldjön külső HDDs.

### <a name="encryption"></a>Titkosítás:

Az adatok a meghajtón titkosítani BitLocker titkosítással-meghajtó kell. Ez a védelmet nyújt az adatok a hálózaton átvitt.

Importálás feladatokhoz kétféleképpen a titkosítási végrehajtásához. Az első úgy, hogy használja a / titkosítsa a paraméter meghajtó előkészítése során az ügyfél eszköz futtatásakor. A második úgy, hogy manuálisan a meghajtón BitLocker titkosítás engedélyezése, és adja meg a titkosítási kulcs az ügyfél eszköz parancssori meghajtó előkészítése során.

Az exportálás feladatok az adatok másolása, meghajtók után a szolgáltatás titkosítja a meghajtó BitLocker használata előtt szállítás vissza szeretné. A titkosítási kulcs fog biztosítható, hogy a klasszikus portálon.  

### <a name="operating-system"></a>Operációs rendszer

Készítse elő a eszközzel Azure importálás/exportálás az Azure meghajtót, mielőtt a merevlemezen egyet az alábbi 64 bites operációs rendszerek használható:

Windows 7 vállalati, Windows 7 Ultimate, a Windows 8 Pro, a nagyvállalati verzió a Windows 8, Windows 8.1 Pro, a Windows 8.1 vállalati, a Windows 10<sup>1</sup>, a Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Az összes alábbi operációs rendszerek támogatja a BitLocker.

> [AZURE.IMPORTANT] <sup>1</sup> Ha a merevlemezen előkészítése a Windows 10-es gépet használ, töltse le az Azure importálás/exportálás eszköz legújabb verzióját.

### <a name="locations"></a>Helyek

Az Azure importálás/exportálás szolgáltatás támogatja az adatok másolása, és az összes nyilvános Azure tárterület-fiókból. Az alábbi helyek valamelyikén merevlemez-meghajtók is kiszállítása. Ha tárterületet fiókja nyilvános Azure helyen, amely itt nincs megadva, az alternatív szállítási helyeket nyújtanak a feladatot a klasszikus portál vagy az importálás/exportálás REST API segítségével létrehozásakor.

Támogatott szállítási helyekre:

- Kelet-Amerikai Egyesült Államok

- Nyugati Amerikai Egyesült Államok

- Kelet-amerikai 2

- A központi Amerikai Egyesült Államok

- A központi Észak-amerikai

- A központi Dél-Amerikai Egyesült Államok

- Észak-Európa

- Nyugati Európa

- Kelet-ázsiai

- Délkelet-ázsiai

- Ausztrália keleti

- Ausztrália Könyvesbolt is.

- Japán nyugati

- Japán keleti

- A központi indiai


### <a name="shipping"></a>Szállítási

**Az Adatközpont meghajtók szállítási:**

Az importálás és exportálás feladat létrehozásakor megkapja azt a meghajtókon szállítandó a támogatott helyek egyikének a szállítási címet. Hol található a tárterület-fiók megadott szállítási címet függ, de nem lehet ugyanaz, mint a fiók tárolóhelyen.

A meghajtókon szállítási címre szállítandó légitársaságok FedEx, DHL, UPS és a Kapcsolatfelvételi postai szolgáltatást is használhatja.

**Az az Adatközpont meghajtók szállítási:**

Az importálás és exportálás feladat létrehozásakor meg kell adnia a Microsoft használja, ha a meghajtók szállítási, újra a feladat befejezése után a feladó címét. Győződjön meg arról, hogy Ön megadja-e egy érvényes visszatérési címre elejét veheti feldolgozása.

Meg kell adnia egy érvényes FedEx vagy DHL carrier számla száma a Microsoft által használandó szállítási a meghajtók vissza. Egy FedEx számlaszám szükség meghajtók szállítási vissza helyekről az Amerikai Egyesült Államokban és Európa. Egy DHL számlaszám szükség meghajtók szállítási Ázsia és az Ausztrália helyekről vissza. Egy [FedEx](http://www.fedex.com/us/oadr/) (az Amerikai Egyesült Államok és Európa) vagy a [DHL](http://www.dhl.com/) (Ázsia és az Ausztrália elemre) carrier fiókot is létrehozhat, ha nincs telepítve egyik. Ha már van egy carrier számlaszám, győződjön meg arról, hogy az érvényes.

A szállítás a csomagok, hajtsa végre a feltételek a [Microsoft Azure szolgáltatás feltételeit](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Felhívjuk a figyelmét arra, hogy a tényleges médiafájlokat, hogy valóban szükséges nemzetközi szegélyek cross. Felelősek annak biztosítására, hogy a fizikai és az adatok vannak importált és/vagy exportálja az alkalmazandó törvények megfelelően. A fizikai adathordozó szállítási, előtt ellenőrizze a tanácsadóknak, ellenőrizze, hogy a média és adatokat is jogilag történni azonosított adatközpontja. Ez segít, annak érdekében, hogy a Microsoft időben lehet ér. Bármely csomagot fog átívelő nemzetközi szegélyek például szeretne mellékelni (kivéve, ha átlépése az Európai Unióban szegélyek) csomaggal kereskedelmi számla van szüksége. A kereskedelmi számla carrier webhelyről töltött másolatának sikerült kinyomtatása. Példa a kereskedelmi számla (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) [DHL kereskedelmi számla] vagy [FedEx kereskedelmi számla](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Győződjön meg arról, hogy a Microsoft nem lett jelezte, mint a exportáló.

## <a name="how-does-the-azure-importexport-service-work"></a>Az importálás/exportálás Azure szolgáltatás működése

Adatok átviheti a helyszíni webhely és a Azure blob-tárolóhoz az Azure importálás/exportálás szolgáltatással feladatok létrehozásával, és a szállítási merevlemez-meghajtók az Azure adatközpontja között. Egyes merevlemez-meghajtók szállított társítva egyetlen feladat. Minden feladat hozzárendelve egy egyetlen tárolási fiókot. Tekintse át az [előzetes követelmények szakasz](#pre-requisites) gondosan részletei határozzák meg, ez például támogatott szolgáltatás blob-típusok, a lemez típusú, a helyek és a szállítási ismertetése.

Ebben a részben azt ismertetik a lépései importálása és exportálása a feladatok magas szintű. A [szakasz első lépések](#quick-start)a későbbi importálási létrehozása és a projekt exportálása című témakörben ismertetettek lesz elérhető.

### <a name="inside-an-import-job"></a>Az importálási feladat belül

Magas szintű egy importálás az alábbi lépéseket foglalja magában:

- Határozza meg az importálni kívánt adatokat, és szüksége lesz meghajtók számát.
- Azonosítsa a cél BLOB Blob-tárolóban lévő az adatokhoz.
- Eszközzel az Azure importálás/exportálás másolja az adatokat egy vagy több merevlemez-meghajtók és BitLocker titkosítása őket.  
- Az importálási feladat létrehozása a cél klasszikus tároló fiókja a klasszikus portál vagy az importálás/exportálás REST API-t. Ha a klasszikus portálon használja, a meghajtó lapjának fájlok feltöltése a.
- Adja meg a feladó címét és a carrier számlaszám használható a meghajtók szállítási vissza szeretné.
- Feladat létrehozása során megadott szállítási címre a merevlemez-meghajtók kiszállítása.
- A szám az importálás feladat részletei nyomon követése kézbesítési frissítése, és küldje el a az importálás.
- Meghajtók kapott, és az Azure adatközpont feldolgozása.
- Meghajtók szállítása a carrier fiókkal az importálás megadott feladói címet.

    ![Ábra 1:Import feladat továbbításához](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>Az exportálási feladat belül

Magas szintű egy exportálási feladat az alábbi lépéseket foglalja magában:

- Határozza meg az exportálandó adatok és a szüksége lesz meghajtók számát.
- Azonosítsa a forrás BLOB- vagy tároló elérési út az Blob-tárolóban lévő adatok.
- A forrás tároló fiókja a klasszikus portál vagy az importálás/exportálás REST API-exportálási feladat létrehozása
- Adja meg a forrás BLOB vagy tároló elérési út az adatok az exportálási feladat.
- Adja meg a feladó címét és a carrier számlaszáma használható a meghajtók szállítási vissza szeretné.
- Feladat létrehozása során megadott szállítási címre a merevlemez-meghajtók kiszállítása.
- A szám az Exportálás feladat részletei nyomon követése kézbesítési frissítése, és küldje el a az exportálási feladat.
- A meghajtók kapott, és az Azure adatközpont feldolgozása.
- A meghajtók titkosított BitLocker; a billentyűk érhetők el a klasszikus portálon keresztül.  
- A meghajtók szállítása a carrier fiókkal az importálás megadott feladói címet.

    ![Ábra 2:Export feladat továbbításához](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>A feladat állapotának megtekintése

A klasszikus portál feladatok exportálhat vagy nyomon követheti az állapotát az importálást. Keresse meg a klasszikus portál tárterület-fiókjába, és kattintson az **Importálás/Exportálás** fülre. A feladatok lista a lapon megjelenik. A feladat állapotát, a feladat nevét, a projekt típusa vagy a nyilvántartási számmal listáját szűrheti.

Attól függően, hogy hol a meghajtón található, a folyamat az alábbi feladatok állapotok egyikét jelenik meg.

| Feladat állapota   | Leírás                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Létrehozása     | A feladatok létrejött, de még nem szerepel a szállítási adatokat.                                                                                                                                                                                                                                                                                                |
| Szállítási     | A feladatok létrehozták, és a szállítási adatokat is meg van adva. **Megjegyzés**: a meghajtó az Azure adatközpont érkeznek, az állapot továbbra is jelenhet meg a "Szállítási" egy kis időt. Miután a szolgáltatás elindul, az adatok másolása, az állapot "Átadása" változik. Ha azt szeretné, hogy a meghajtó szűkebb állapotát, az importálás/exportálás REST API-t is használhatja. |
| Átvitele | Az adatok átvitele a merevlemezről (hogy az importálás) vagy a merevlemezen (feladathoz az Exportálás).                                                                                                                                                                                                                                                                 |
| Csomagolása    | Az adatok továbbítása befejeződött, és vissza szeretné szállítási szánt, a merevlemezen.                                                                                                                                                                                                                                                                             |
| Befejezése     | Vissza a merevlemezen teljesítették.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Feldolgozási feladat ideje

Ennyi ideig tart a szállítási időt, például különböző tényezők függően változik az importálás/exportálás feladat folyamat projekt típusa, típusa és a másolt adatok méretét és a lemez megadott méretét. Az importálás/exportálás szolgáltatás nincs egy SLA. A projekt előrehaladásának jobban nyomon követésére REST API-t is használhatja. A feladatok lista művelet Másolás folyamatban feltüntetése diagramalakzatok százalékos készültségi paraméter van. Keresse meg a kapcsolatfelvételi Ha szüksége van egy idő kritikus importálás/exportálás feladat elvégzéséhez becslést.

### <a name="pricing"></a>Árak

**Meghajtó díj kezelése**

Nincs minden az importálást részeként feldolgozott meghajtó meghajtó kezelése díjat vagy a projekt exportálása. Nézze meg a részleteket az [Azure importálás/exportálás árak](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Szállítási költség**

Amikor Ön Azure meghajtók, a szállítási költség a szállítási carrier, fizet. Microsoft a meghajtók Önnek ad vissza, ha a szállítási költség a carrier fiók, amely a feladat létrehozása idején megadott van rendelve.

**Transaction költségek**

Nincsenek tranzakció költség adatok importálásával blob-tárolóhoz. A szokásos kilépési díjat kell alkalmazni, blob-tárolóhoz származó adatok exportálásakor. A tranzakciók költségek, lásd: [adatátviteli árak.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Rövid útmutató

Ebben a részben importálási és az exportálási feladat létrehozására vonatkozó lépésenkénti útmutatót lesz elérhető. Ellenőrizze, hogy a [előtti követelmények](#pre-requisites) összes mielőtt előre megfelelnek.

## <a name="how-to-create-an-import-job"></a>Hogyan hozhat létre az importálás?

Hozzon létre egy adatok másolása a tárhely Azure-fiókjába merevlemez-meghajtók által megadott adatközpontja adatokat tartalmazó egy vagy több meghajtók szállítási importálás. Az importálás körű merevlemez-meghajtók másolható, Megcélozhat tárterület-fiók és a szállítási adatokat az importálás/exportálás Azure szolgáltatás adatokat olvashat. Az Importálás létrehozása a következő három lépésből áll. Első lépésként készítse elő a meghajtókon az Azure importálás/exportálás ügyfél eszközét használva. Második elküldése az importálás a klasszikus portálon. Harmadik a projekt létrehozása során megadott szállítási címet meghajtók kiszállítása, és a feladat részletei a szállítási adatainak frissítése.   

> [AZURE.IMPORTANT] Csak egy feladatot egy tároló fiók beküldheti. Minden szállított meghajtó importálhatók egy tárterület-fiókjába. Ha például tegyük fel, hogy ki szeretne importálni az adatokat az két tárterület-fiókot. Meg kell külön merevlemez-meghajtók minden tárterület-fiók létrehozása és használata külön feladatokat tároló fiók per.

### <a name="prepare-your-drives"></a>Felkészülés a meghajtókon

Az első lépés, ha importálja az adatokat az importálás/exportálás Azure szolgáltatás használatával a meghajtókon az Azure importálás/exportálás ügyfél eszközzel készít. Hajtsa végre az alábbi lépéseket követve a meghajtókon előkészítése.

1.  Azonosítsa az importálni kívánt adatokat. Könyvtárak és a helyi kiszolgálón vagy hálózati megosztáson önálló fájlok lehet.  

2.  Határozza meg, hogy szüksége lesz, attól függően, hogy az adatok teljes méret meghajtók számát. A szükséges számú 3,5 hüvelyk SATA II/III merevlemez-meghajtók beszerezni.

3.  Azonosítsa a cél tárterület-fiókot, tároló, virtuális könyvtárak és BLOB.

4.  A könyvtárak és/vagy az egyes merevlemez-meghajtón másolt önálló fájlok határozza meg.

5.  Az [Azure importálás/exportálás eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) segítségével másolja az adatokat egy vagy több merevlemez-meghajtók.

    - Az importálás/exportálás Azure eszköz másolás munkamenetek forrásból származó adatok másolása a merevlemez-meghajtók hoz létre. Egyetlen példányt folytat az eszköz másolhatja egy egyetlen könyvtárat és alkönyvtárait vagy egy fájlt.

    - Szükség lehet több másolatot munkamenetek, ha a forrásadatokban sok könyvtárak hasábokat.

    - Egyes merevlemez-meghajtók előkészítheti esetén kell legalább egy példány munkamenetet.

6.  Megadhatja a paraméter ahhoz, hogy a merevlemez-meghajtó Bitlocker titkosításának titkosítás /. Azt is megteheti sikerült is engedélyezi Bitlocker használatára, manuálisan a a merevlemezt, és adja meg az eszköz futtatása közben kulcsot.

7.  Az Azure importálás/exportálás eszköz minden meghajtó meghajtó lapjának fájl hoz létre, készen áll arra. A helyi számítógépen, nem a meghajtón magát a meghajtó lapjának fájlt tárolja. Az importálás létrehozásakor, feltöltődnek a Jegyzetfüzet-fájlban. Meghajtó lapjának fájl tartalmaz, a meghajtó azonosító és a BitLocker kulcsot, valamint a meghajtó kapcsolatos egyéb adatok.
**Fontos**: egyes merevlemez-meghajtók arra készül, a naplófájl eredményez. Az importálás a klasszikus portálon hoz létre, ha akkor fel kell töltenie a meghajtók, importálás részét képező lapjának fájlokat. Meghajtók fájlokat nem lehet feldolgozni lapjának nélkül.

8.  Lemezen előkészítése befejezése után ne módosítsa az adatokat a merevlemez-meghajtók vagy a Jegyzetfüzet-fájlban.

Alatt, hogy a parancsok és előkészítése a merevlemez-meghajtó Azure importálás/exportálás ügyfél eszközzel példákat is tartalmaz.

Azure importálás/exportálás ügyfél eszköz PrepImport parancs az első példányt munkamenet könyvtárában másolása:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Példa:**

Az alábbi példában azt minden fájl másolása és könyvtárakat az H:\Video a merevlemez-meghajtón lévő x csatlakoztatott sub. Az adatok a tárterület-fiók billentyűt, és a című videót tároló tárolóba megadott cél tároló fiókot is importálva lesznek. Ha nem szerepel a tárhely tároló, létrejön. Ez a parancs is formázhatja, és a cél merevlemez-meghajtó titkosítása.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure importálás/exportálás ügyfél eszköz PrepImport parancs a könyvtár másolása későbbi másolás folyamatokhoz:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

A későbbi másolás folyamatokhoz az azonos merevlemez-meghajtón adja meg a napló egy fájlnevet, és adja meg a egy új munkamenet-azonosító; Adja meg ismét a fiók billentyű- és célwebhelyek meghajtót sem szeretne formázni, vagy a meghajtó titkosítására nincs szükség van. Ez a példa azt másol a H:\Photo mappát és annak sub könyvtárak ugyanarra a cél meghajtóra, a tárhely tárolóba fénykép nevű.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Az importálás/exportálás Azure ügyfél eszköz PrepImport parancs az első példányt munkamenet fájl másolása:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Az importálás/exportálás Azure ügyfél eszköz PrepImport parancs a fájl másolása későbbi másolás folyamatokhoz:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Ne feledje**: alapértelmezés szerint az adatokat, a továbbfejlesztett fájlblokkolás BLOB importált. Egy lap BLOB az adatok importálása a /BlobType paraméter is használhatja. Ha például ha virtuális fájlokat, amelyeket fel egy Azure virtuális a csatlakoztatási importál, importálnia kell őket, oldal BLOB. Ha biztos benne, mely blob típust válassza, megadhatja, hogy /blobType:auto, és azt segítséget nyújt a megfelelő típusának megállapítása. Ebben az esetben, oldal BLOB importált minden virtuális és vhdx fájl, és a többi importálja, a továbbfejlesztett fájlblokkolás BLOB.

Lásd: az Azure importálás/exportálás ügyfél-eszköz használata [Előkészítése merevlemez-meghajtók importáláshoz](https://msdn.microsoft.com/library/dn529089.aspx)olvashat bővebben.

A [Minta munkafolyamat előkészítése merevlemez-meghajtók hogy az importálás](https://msdn.microsoft.com/library/dn529097.aspx) részletes, lépésenkénti útmutatót is hivatkozhat.  

### <a name="create-the-import-job"></a>Az importálási feladat létrehozása

1.  A Felkészülés a meghajtó után nyissa meg azt a [Klasszikus portál](https://manage.windowsazure.com) tárterület-fiókjába, és az irányítópult megtekintése. A **Quick Glance**kattintson a **Létrehozás az importálás**gombra. Tekintse át a lépéseket, és a jelölőnégyzet bejelölésével jelezheti, hogy van-e készíteni a meghajtóra, és hogy rendelkezik-e a meghajtó naplófájl.

2.  Az 1 Ez az importálás és érvényes válaszlevél felelős személy elérhetőségi adatok megadása. Az importálás a részletes naplózás adatokat is menteni szeretné, ha jelölje be **a "waimportexport" blob-tárolóban a részletes napló mentése**lehetőséget.

3.  A 2 töltse fel a meghajtó előkészítése lépés során kapott meghajtó lapjának fájlokat. Szüksége lesz az egyes készített meghajtót egy fájl feltöltése.

    ![Importálás – a 3 létrehozása](./media/storage-import-export-service/import-job-03.png)

4.  Írja be egy jól értelmezhető nevet az importálás, a 3. Figyelje meg, hogy a név is tartalmazhatnak, csak a kisbetűket, számok, kötőjeleket és aláhúzás karakterek találhatók, egy betűvel kell kezdődnie, és nem tartalmazhat szóközt. A neve, úgy dönt, hogy a feladatok nyomon követheti, amíg ők folyamatban lévő és után azok befejezetlen fogja használni.

    Ezután jelölje ki az adatterület központ a listából. Az adatterület központ jelzi az adatközpont és a címet, amelyre a csomagot kell kiszállítása. Olvassa el az Gyakori alatti további információt.

5.  A 4 a feladó carrier válasszon a listából, és adja meg a carrier. A Microsoft az importálás befejeződése után vissza szeretné a meghajtók szállítandó használja ehhez a fiókhoz.

    Ha a nyilvántartási számmal, jelölje be a kézbesítési carrier a listából, és adja meg a nyomon követés.

    Ha meg van a nyomon követés még nem, válassza a **lehet amint lehet van teljesült a csomag projekt importálása a szállítási információt nyújt**, majd töltse ki az importálási folyamat.

6. A nyomon követés telefonszám megadása a csomagot, térjen vissza az **Importálás/Exportálás** lapra, a tárterület-fiók a klasszikus portálon teljesítették után a feladatok válasszon a listából, és válassza a **Szállítási információ**. Nyissa meg a varázsló lépéseit, és adja meg a nyomon követés a 2.

    Ha a nyomon követés szám nem frissül a feladat létrehozása 2 hétből belül, a feladat lejár.

    Ha a feladat létrehozása, szállítási vagy átadása állapotban van, a carrier számlaszám a 2 a varázsló is módosíthatja. Miután a feladatot a csomagolást állapotban van, nem lehet frissíteni a carrier számlaszáma, hogy a feladat.

7. A portál irányítópulton követheti nyomon a projekt előrehaladását. Az előző szakasz minden feladat állapota jelentése megtekintésével, hogy [a feladat állapotának](#viewing-your-job-status)megtekintése

## <a name="how-to-create-an-export-job"></a>Hozzon létre egy exportálása feladatot hogyan?

Hozzon létre egy exportálási feladat arra az importálás/exportálás szolgáltatást, hogy akkor fogja kell szállítási egy vagy több üres meghajtók adatközpontja, hogy az adatok exportálása a tárterület-fiókjából a és a meghajtók, majd teljesült Önnek.

### <a name="prepare-your-drives"></a>Felkészülés a meghajtókon

Az Exportálás projektre vonatkozóan a meghajtókon előkészítési ajánlott következő előtti ellenőrzése:

1. Jelölje be a lemezre van szükség az Azure importálás/exportálás eszköz PreviewExport paranccsal számát. További információért találja [Előnézet meghajtó exportálása feladathoz](https://msdn.microsoft.com/library/azure/dn722414.aspx). A program segít lehetőséget választotta, a BLOB alapú fogja használni a meghajtók méretének megtekintése a meghajtó használatát.

2. Ellenőrizze, hogy akkor is olvasási/írási a merevlemezre, amely a Exportálás feladat fog történni.

### <a name="create-the-export-job"></a>Az exportálási feladat létrehozása

1.  Az exportálási feladat létrehozásához nyissa meg azt a [Klasszikus portál](https://manage.windowsazure.com)tárterület-fiókjába, és az irányítópult megtekintése. A **Quick Glance**kattintson **az Exportálás feladat létrehozása** , és kövesse a varázsló lépéseit.

2.  A 2 adja meg a exportálási feladat felelős személy kapcsolattartási adatait. Az Exportálás feladat részletes naplózás adatokat is menteni szeretné, ha jelölje be **a "waimportexport" blob-tárolóban a részletes napló mentése**lehetőséget.

3.  A 3 az üres meghajtó vagy meghajtók exportálása a tárterület-fiókjából szeretne blob adatok megadása. Lehetősége van arra, hogy a tárterület-fiókban minden blob-adatok exportálása, vagy megadhatja a BLOB vagy BLOB az exportálandó állítja be.

    Adja meg az exportálandó blob, használja a **Egyenlő** sorkijelölőre, és adja meg a relatív elérési útját a blob kezdődő tároló nevét. *$Root* segítségével adja meg a legfelső szintű tároló.

    Adja meg a előtaggal kezdődő összes BLOB, **Elindítja az** Adatkijelölő használja, és adja meg a perjellel kezdve az előtag "/". Lehet, hogy az előtag az előtag a tároló nevet, a készültségi tárolóhoz vagy a készültségi tárolóhoz nevét az előtag blob név követ.

    A következő táblázat mutatja az érvényes blob elérési utak példákat:

    Adatkijelölő|BLOB elérési út|Leírás
    ---|---|---
    Karakterekkel kezdődik.|/|Exportálja az összes BLOB-tároló fiók
    Karakterekkel kezdődik.|/$root /|Exportálja a legfelső szintű tárolóban lévő valamennyi BLOB
    Karakterekkel kezdődik.|/Book|Bármelyik tárolóban **könyv** előtaggal kezdődő összes BLOB exportálja.
    Karakterekkel kezdődik.|/Music/|Az összes BLOB-tárolóhoz **zenét** a exportálja.
    Karakterekkel kezdődik.|/ zenét/szeretetet|Exportálja a tároló **zenét** valamennyi kezdetű előtag **közkedvelt** BLOB
    Egyenlő|$root/logo.bmp|Exportnak blob- **logo.bmp** a legfelső szintű tárolóban
    Egyenlő|Videos/Story.mp4|Exportnak blob- **story.mp4** tároló **Videók**

    Meg kell adnia a blob elérési utak érvényes formátumú és hibák elkerülése érdekében a feldolgozás során, ez képernyőképen látható módon.

    ![Exportálási feladat - a 3 létrehozása](./media/storage-import-export-service/export-job-03.png)


4.  Lépés: 4 írja be egy jól értelmezhető nevet az exportálási feladat. Megadott is tartalmazhatnak, csak a kisbetűket, számok, kötőjeleket és aláhúzás karakterek találhatók, egy betűvel kell kezdődnie, és nem tartalmazhat szóközt.

    Az adatterület központ jelzik az adatközpont, amelyhez a csomagot kell kiszállítása. Olvassa el az Gyakori alatti további információt.

5.  Az 5 jelölje be a feladó carrier a listából, és írja be a carrier számla számát. A Microsoft az exportálási feladat befejeződése után vissza szeretné a meghajtókon szállítandó használja ehhez a fiókhoz.

    Ha a nyilvántartási számmal, jelölje be a kézbesítési carrier a listából, és adja meg a nyomon követés.

    Ha meg van a nyomon követés még nem, válassza a **lehet a Exportálás feladat amint lehet van teljesült a csomag a szállítási információt nyújt**, majd töltse ki az exportálási folyamat során.

6. A nyomon követés telefonszám megadása a csomagot, térjen vissza az **Importálás/Exportálás** lapra, a tárterület-fiók a klasszikus portálon teljesítették után a feladatok válasszon a listából, és válassza a **Szállítási információ**. Nyissa meg a varázsló lépéseit, és adja meg a nyomon követés a 2.

    Ha a nyomon követés szám nem frissül a feladat létrehozása 2 hétből belül, a feladat lejár.

    Ha a feladat létrehozása, szállítási vagy átadása állapotban van, a carrier számlaszám a 2 a varázsló is módosíthatja. Miután a feladatot a csomagolást állapotban van, nem lehet frissíteni a carrier számlaszáma, hogy a feladat.

    > [AZURE.NOTE] Az exportálandó blob a merevlemezre másolni idején használatban van, ha a Azure importálás/exportálás szolgáltatás a blob pillanatképet készíthet, és másolja a pillanatkép.

7.  A feladat nyilvántartása az irányítópulton a klasszikus portálon. Minden feladat állapota jelentése az előző szakaszban látható a "a feladat állapotának megtekintése".

8.  Miután visszakapta a meghajtók az exportált adatokat tartalmazó, megtekintése, és másolja a BitLocker kulcsok a meghajtó szolgáltatás által generált. Keresse meg a klasszikus portál tárterület-fiókjába, és kattintson az importálás/exportálás fülre. Az exportálási feladat válasszon a listából, és kattintson a kulcsok megtekintése gombra. A BitLocker kulcsok jelennek meg, az alább látható módon:

    ![Az exportálási feladat BitLocker billentyűk megtekintéséhez](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Kérjük, feldolgozzuk a gyakori kérdések szakaszban alatt, hogy az elfedje az ügyfelek a Ez a szolgáltatás használata során felmerülő leggyakoribb kérdésekre.

## <a name="frequently-asked-questions"></a>Gyakori kérdések ##


**Mennyi ideig tart az adatok másolása, a megteheti az Adatközpont eléri után?**

Másolja a vágólapra az idő függően különböző tényezők projekt típusa, például a megadott típusú és méretű adatokat másolt, lemezre méretét, és a meglévő terhelést változik. A hét, attól függően, hogy az alábbi tényezőket néhány a nap, néhány változhat. Ezért akkor adja meg az általános becslést nehezen. A szolgáltatás próbálja optimalizálása a feladatok több meghajtók másolásával párhuzamosan, amikor csak lehetséges. Ha egy idő kritikus importálás/exportálás feladatot, keresse meg a kapcsolatfelvételi becslést.

**Mikor érdemes használni a Azure importálás/exportálás szolgáltatás?**
Egy figyelembe 7 napnál becslése nagyjából használatával Azure importálás exportálás, ha feltöltésének vagy letöltésének hálózaton keresztül tart. Mennyi ideig fog tartani bármely online Számológép vagy [letöltése](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) a mintában a Azure importálása exportálása REST API Azure minták adattárban, az [Adatok átvitele sebesség Számológép](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html)található egy kiszámíthatja meg. Ez nem pontos kiszámítása, de csak durva jelzése.

**Van lehetőség az importálás/exportálás Azure szolgáltatás egy erőforrás-kezelő tárterület-fiókkal?**

Nem, nem másolhat adatok való vagy egy erőforrás-kezelő tároló fiókból közvetlenül az Azure importálás/exportálás szolgáltatással. A szolgáltatás csak a klasszikus tároló fiókokat támogat. Erőforrás-kezelő tároló fiók támogatása fog hamarosan. Alternatív megoldásként klasszikus tárterület-fiókba importálhatja az adatokat, és áttelepítése az erőforrás-kezelő tároló fiókjához a [AzCopy](storage-use-azcopy.md) vagy CopyBlob.

**Az importálás/exportálás Azure szolgáltatás használatával Azure fájlok másolása**

Nem, az importálás/exportálás Azure szolgáltatás csak blokk BLOB és támogatja lap BLOB. Egyéb tároló típusú Azure fájlok, táblázatok és sorok nem támogatottak.

**Az importálás/exportálás Azure szolgáltatás érhető el a CSP verzióhoz?**

Az importálás/exportálás Azure szolgáltatás nem, nem támogatja a CSP előfizetések. A támogatás megjelenik a jövőben.

**E átugorhatja a meghajtó előkészítése az importálási feladat vagy lehet előkészítheti a meghajtó másolás nélkül?**

Bármely meghajtó kiszállítása a kívánt adatok importálásával az Azure importálás/exportálás ügyfél eszközzel kell készíteni. Az ügyfél eszköz adatainak másolása a meghajtóra kell használnia.

**Szükség van egy exportálási feladat létrehozásakor, végezze el minden lemezre előkészítése?**

Nem, de néhány előtti ellenőrzések ajánlott. Jelölje be a lemezre van szükség az Azure importálás/exportálás eszköz PreviewExport paranccsal számát. További információért találja [Előnézet meghajtó exportálása feladathoz](https://msdn.microsoft.com/library/azure/dn722414.aspx). A program segít lehetőséget választotta, a BLOB alapú fogja használni a meghajtók méretének megtekintése a meghajtó használatát. Is ellenőrizze, hogy olvassák, és írja be a merevlemezre, amely a Exportálás feladat fog történni.

**Mi történik, ha véletlenül küld egy merevlemez, amelyek nem felelnek meg a támogatott követelményei?**

Az Azure adatközpont ad vissza a meghajtóra, amely nem felel meg Önnek a támogatott követelményeknek. Ha csak néhány a meghajtót az előkészítés felel meg a támogatási követelmények, ezek meghajtók feldolgozása lesz, és a meghajtókon, amelyek nem felelnek meg visszakerül.

**A feladat is megszakítása?**

A feladat állapota létrehozásakor, illetve az szállítási vonhatja vissza.

**Mennyi ideig is befejezett feladatok állapotának megtekintése a klasszikus portál?**

A kész feladatokat állapota 90 napig tekinthet meg. Befejezett feladatok után 90 napon is törli.

**Ha nem szeretném, importálhat vagy exportálhat 10-nél több meghajtók, mit kell tenni?**

Egy importálási vagy exportálási feladat hivatkozhat egyetlen feladat számára az importálás/exportálás szolgáltatás csak 10 meghajtók. Ha kiszállítása a 10-nél több meghajtók, létrehozhat több feladat. Az ugyanazon feladatot társított meghajtók együtt kell teljesült az egyazon csomagban.

**Használhatom-e egy USB-SATA adaptert, amely nem szerepel a javasolt listában?**

Ha egy konverter, amely nem szerepel a fenti, megpróbálhatja fut az Azure importálás/exportálás eszköz használatával az konverter előkészítése a meghajtóba, és látni, ha egy támogatott konverter vásárlása előtt működik.

**A szolgáltatás formázása nem a meghajtók visszatérés őket előtt?**

nem. Az összes meghajtó BitLocker titkosított.

**Is meghajtók importálás/exportálás projektekhez is vásárolhat a Microsofttól?**

nem. Kiszállítása a saját meghajtók mindkét importálása és exportálása az feladatokat kell.

**Az importálás befejeződése után milyen lesz a adatok néz tároló fiók? A címtár-hierarchia megmaradnak?**

Merevlemez-meghajtó előkészítése az importálás, ha a cél által a /dstdir megadva: paraméter. Ez az a cél tároló a tárterület-fiókot, amelyhez a merevlemezről adatokat másolja. A cél tárolóban lévő virtuális könyvtárak létrehozásakor a merevlemezről a mappák és BLOB-fájlok létrehozásakor.

**Ha a meghajtóba, amelyek már szereplő tárterület-fiókom van, a szolgáltatás felülírja a tárterület-fiókom meglévő BLOB?**

A meghajtó előkészítésekor megadhatja, hogy a cél fájlokat felülírható vagy /Disposition figyelmen kívül hagyott a paraméter használatával című: < átnevezése |} nem írja felül |} felülírása >. Alapértelmezés szerint a szolgáltatást fogja az új fájlok átnevezése, hanem írja felül a meglévő BLOB.

**Az importálás/exportálás Azure ügyfél eszköz kompatibilis a 32 bites operációs rendszerek az?**
nem. 64 bites Windows operációs rendszerek lehetőség csak a ügyfél eszközt. Olvassa el az operációs rendszerek szakasz az [előzetes követelmények](#pre-requisites) teljes listáját a támogatott operációs rendszer verziója.

**E tartalmaznia kell semmit a merevlemez-meghajtó kívül saját csomag?**

Kérjük, kiszállítása csak a merevlemez-meghajtók. Ne kerüljön bele az elemekhez, például a forrás kábelek vagy USB-kábel csatlakoztatva.

**Szükség van a FedEx vagy DHL meghajtók kiszállítása?**

Az Adatközpont használatával FedEx, DHL, UPS vagy US Posta például minden olyan ismert carrier meghajtók is kiszállítása. Azonban a szállítás a meghajtók vissza az az adatközpont, meg kell adnia egy FedEx fiókot az Amerikai Egyesült Államokban és az Európai Unió, vagy DHL fiók szám az ázsiai és az Ausztrália régiókban.

**Van valamilyen korlátozás a szállítási nemzetközileg a meghajtó?**

Felhívjuk a figyelmét arra, hogy a tényleges médiafájlokat, hogy valóban szükséges nemzetközi szegélyek cross. Felelősek annak biztosítására, hogy a fizikai és az adatok vannak importált és/vagy exportálja az alkalmazandó törvények megfelelően. A fizikai adathordozó szállítási, előtt ellenőrizze a ellenőrizze, hogy a média és adatokat is jogilag történni azonosított adatközpontja tanácsadók. Ez segít, annak érdekében, hogy a Microsoft időben lehet eléri.

**Egy feladatot létrehozni, a szállítási cím esetén olyan helyre, ahol eltérő tárhelyéről a fiókot. Mit kell tenni?**

Néhány tárolási fiók helyek alternatív szállítási helyen vannak megfeleltetve. A korábban elérhető szállítási helyek is ideiglenes csatolható más helyekre. Mindig jelölje be a meghajtókon, mielőtt a feladat létrehozása során megadott szállítási címet.

**Miért tartalmaz a feladat állapota a klasszikus portál Ismerkedés "szállítási" Ha a Carrier webhely jeleníti meg a csomag kézbesítve?**

A klasszikus portálon állapotát állapotról szállítási átvitele mikor a meghajtó feldolgozása kezdődik. Ha a meghajtó megérkezett a létesítmény, de nem indult el feldolgozása, a feladat állapota szerint szállítási jelennek meg.

**Amikor a meghajtó szállítási, a carrier megkérdezi, hogy az adatok központ ügyfél nevét és telefonszámát. Mit kell adnia?**

A telefonszám kapja meg, feladat létrehozása során. Ha a partner nevére, lépjen kapcsolatba velünk a waimportexport@microsoft.com és fog biztosítunk Önnek ezeket az információkat.

**Az importálás/exportálás Azure szolgáltatás segítségével PST-postaládákhoz és a SharePoint-adatok másolása az Office 365?**

Olvassa el a [importálása PST-fájlokból](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx)vagy az Office 365 SharePoint-adatokat.

**Az importálás/exportálás Azure szolgáltatás segítségével másolja a vágólapra a biztonsági másolatok offline az Azure biztonsági másolat szolgáltatás?**

Olvassa el [az Azure biztonsági másolat Offline biztonsági másolat munkafolyamat](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Lásd még:

- [Az importálás/exportálás Azure ügyfél eszköz beállítása](https://msdn.microsoft.com/library/dn529112.aspx)

- [A AzCopy parancssori segédprogrammal adatátviteli](storage-use-azcopy.md)

- [Azure importálása exportálás REST API-val minta](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
