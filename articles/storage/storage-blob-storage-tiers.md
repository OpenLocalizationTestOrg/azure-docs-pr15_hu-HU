<properties
    pageTitle="Azure BLOB izgalmas tárterület |} Microsoft Azure"
    description="Az Azure Blob-tárolóhoz tároló rétegek objektum adatai alapján access mintázatok költség hatékony tárterület kínálnak. A izgalmas tároló réteg ritkábban elérhető adatok van optimalizálva."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure Blob-tárolóhoz: Gyorsbillentyűk használatát, és tároló rétegek hűtsük

## <a name="overview"></a>– Áttekintés

Azure tároló kínál két tároló rétegek Blob-tárolóhoz (objektum tároló), az, hogy a legtöbb költséghatékonyan attól függően, hogy hogyan használható az adatok tárolhat. Az Azure **meleg tároló réteg** gyakran használt adatok tárolására van optimalizálva. Az Azure **izgalmas tároló réteg** , amely a ritkán használt és élettartamú adatok tárolására van optimalizálva. Adatok a izgalmas tároló réteg elviseli egy kicsit kisebb elérhetőségét, de továbbra is és elő kell készítenie magas tartóssági és hasonló idő eléréséhez átviteli tulajdonságokban meleg adatok. Izgalmas adatok esetén kissé alsó elérhetősége SLA és újabb access költségek elfogadható kompromisszumok sok alsó tárolási költségek.

Ma a felhőben tárolt adatok az exponenciális tempójában nő. Növekvő tároló igényeinek költségek kezelésére, érdemes rendszerezheti az adatok, például access és a tervezett Adatmegőrzés időtartama gyakorisága tulajdonságok alapján. Lehet, hogy a felhőben tárolt adatok igazán különböző hogyan azt van létrehozott, feldolgozott és élettartama keresztül érhető el. Bizonyos adatok aktívan elérhető és egész élettartama módosított. Bizonyos adatok érhető el gyakran élettartama, a korai jelentősen az adatok életkor leválasztása hozzáféréssel rendelkező. Néhány adatot a felhőben tétlen és ritkán, ha egyszer, érhető el.

Egy adott access mintát optimalizált tárolási eltérő réteg előnyei a fenti esetek elérése minden ezeket az adatokat. Izgalmas és melegvíz tároló meghatározási bevezetésével Azure Blob-tároló most szünteti meg ez az eltérő tároló rétegek szükség külön modellek árak.

## <a name="blob-storage-accounts"></a>BLOB-tároló fiókok

**BLOB-tároló fiókok** speciális tároló számlák BLOB (objektum) Azure-tárolóban lévő, a strukturálatlan adatokat tároljon. Az Blob-tároló fiókokkal most közül választhat hideg- és melegvíz tároló rétegek tárolni a ritkábban használt izgalmas adatait egy alacsonyabb tárolási költség és a költség alsó hozzáférés a gyakrabban használt meleg adattárolásra. BLOB-tároló fiókok hasonlítanak a meglévő általános célú tárterület-fiókra, és az összes a nagyszerű tartóssági, elérhetősége, méretezhetőség és megosztása a teljesítményjavító funkciók használt ma, beleértve a 100 %-os API konzisztencia blokk BLOB-, és a Hozzáfűzés BLOB.

> [AZURE.NOTE] BLOB-tároló fiókok csak a továbbfejlesztett fájlblokkolás támogatja, és a Hozzáfűzés BLOB, és nem lap BLOB.

BLOB-tároló fiókok jelenítik meg az **Access réteg** tulajdonság, amelyek lehetővé teszik, hogy adja meg a tárhely réteg **érzékeny** vagy **izgalmas** attól függően, hogy a fiók tárolt adatok. Az adatok használatát szerkezetében módosítás esetén is válthat a tárhely rétegek bármikor között.

> [AZURE.NOTE] További díjakat vonhat a tárhely réteg módosítása. Talál további információt a [árak és számlázás](storage-blob-storage-tiers.md#pricing-and-billing) szakaszban.

A meleg tároló réteg használatát példák a következők:

- Aktív használja, vagy érhető el (az olvasási és beírva) gyakran várhatóan adatokat.
- A izgalmas tároló réteg feldolgozó és az esetleges áttelepítése elő van készítve adatokra.

A izgalmas tároló réteg használatát példák a következők:

- Biztonsági másolat, archiválás és katasztrófa helyreállítási adatkészleteket.
- Régebbi médiafájl nem megtekintett gyakran bezárhatja, de várhatóan megnyitásakor azonnal elérhető lesz.
- Kell tárolt költség hatékony több adat éppen összegyűjtött jövőbeli feldolgozása közben nagyméretű adatkészletek. (*például*hosszú távú tudományos adatok, egy gyártási létesítmény nyers telemetriai adatainak tárolására)
- Eredeti (nyers) adatokat kell hagyni, dolgozva végleges felhasználható űrlapba után. (*pl.*nyers médiafájlok más formátumokba transcoding után)
- Megfelelőség és az archiválás adatok hosszú ideig kell tárolni és alig érhető el. (*például*biztonsági kamera elejét vagy végét; régi X-Rays/MRIs egészségügyi szervezetek, hangfelvételek és az ügyfél átiratok a hívások engedélyezése a pénzügyi szolgáltatások)

További információt a tárterület-fiókok [kapcsolatos Azure tárterület-fiókok](storage-create-storage-account.md) talál.

Csak igénylő alkalmazások letiltani vagy blob-tárolóhoz hozzáfűző, használata javasolt, mert Blob-tároló fiókok, Többszintű tárolási eltérő árak modell előnyeit. Azonban azt megértéséhez, hogy ez nem lehet bizonyos körülmények között nem lehetséges módja szeretne ugrani, például általános célú tárterület-fiókokkal lenne szükség:

- Táblázatok, a sorok vagy a fájlok végzése a BLOB-tároló ugyanazzal a fiókkal tárolt szüksége. Megjegyzés: az nem technikai előny való tárolásáról ugyanazzal a fiókkal nem azonos az billentyűk megosztott.
- Van szüksége a klasszikus telepítési modell használni. Az erőforrás-kezelő Azure telepítési modell BLOB-tároló fiókok csak érhetők el.
- Akkor használja, oldal BLOB. BLOB-tároló fiókok lap BLOB nem támogatják. Általában azt javasoljuk, blokkokból álló BLOB használatával, kivéve, ha rendelkezik, oldal BLOB adott szükség.
- A [Tároló szolgáltatások REST API -val](https://msdn.microsoft.com/library/azure/dd894041.aspx) , 2014-02-14 verziónál vagy ügyfél lévő verziójának használata kisebb, mint 4.x verzióval, és nem tudja frissíteni az alkalmazást.

> [AZURE.NOTE] BLOB-tároló fiókok az Azure régiókat további többsége követése jelenleg támogatott. A frissített listában elérhető régiójában, a [Régió az Azure-szolgáltatások](https://azure.microsoft.com/regions/#services) lapon található.

## <a name="comparison-between-the-storage-tiers"></a>A tároló Rétegek összehasonlítása

Az alábbi táblázat néhány fontosabb funkció a két tároló Rétegek összehasonlítása:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Réteg meleg tárhely</center></strong></td>
    <td><strong><center>Izgalmas tároló szint</center></strong></td
</tr>
<tr>
    <td><strong><center>Elérhetőség</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %-os</center></td>
</tr>
<tr>
    <td><strong><center>Elérhetőség<br>(TS-GRS felolvassa)</center></strong></td>
    <td><center>99.99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Használati költségek</center></strong></td>
    <td><center>Nagyobb tárolási költségek<br>Alsó access és a tranzakciók költségek</center></td>
    <td><center>Alsó tárolási költségek<br>Újabb access és a tranzakciók költségek</center></td>
</tr>
<tr>
    <td><strong><center>Minimális objektum méretének<center></strong></td>
    <td colspan="2"><center>A #HIÁNYZIK</center></td>
</tr>
<tr>
    <td><strong><center>Minimális tárolási időtartam<center></strong></td>
    <td colspan="2"><center>A #HIÁNYZIK</center></td>
</tr>
<tr>
    <td><strong><center>Időtartam<br>(Az első bájt idő)<center></strong></td>
    <td colspan="2"><center>ezredmásodpercben</center></td>
</tr>
<tr>
    <td><strong><center>Méretezhetőség és a teljesítmény cél<center></strong></td>
    <td colspan="2"><center>Ugyanaz, mint általános célú tárterület-fiókok</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB-tároló fiókok támogatja az azonos teljesítmény és méretezhetőség célok általános célú tároló partnerként. [Azure tároló méretezhetőség és a teljesítmény célok](storage-scalability-targets.md) talál további információt.

## <a name="pricing-and-billing"></a>Árak és számlázás

A tárhely réteg alapján blob-tárolóhoz BLOB-tároló fiókok használata új árak modell. Ha egy Blob-tároló fiókot használ, a következő számlázási érvényesek:

- **Tárolási költségek**: tárolt adatok mennyiségét mellett az adatok tárolására költsége a tárhely réteg függően változik. A GB alapú költség a meleg tároló réteg mint izgalmas tároló réteg az alsó.
- **Adatelérési költségek**: Ha az adatok a izgalmas tároló réteg, akkor ráterheljük GB adatok access díjat olvasása és írása.
- **Transaction költségek**: mindkét rétegek tranzakció díjat van. A izgalmas tároló réteg tranzakció költségét azonban nem nagyobb, mint a meleg tároló réteg számára.
- **A replikáció GEO adatátviteli költségek**: Ez csak érvényes fiókok ismétlésekkel geo-beállítva, beleértve a GRS és TS-GRS. A replikáció GEO adatátvitel GB díjat vonz.
- **Kimenő adatátviteli költségek**: kimenő adatátvitel (Kilépés az Azure területről átvitt adatok) alapján GB – általános célú tároló fiókok követik sávszélesség-használat számlázását merülnek fel.
- **A tároló réteg módosítása**: a tárterület réteg módosítása a látványos, hogy menet merülnek fel, amelyekre az összes adatot tároló fiók minden átmenet meglévő olvasási egyenlő díjat. Azonban hogy a tárhely réteg meleg helyett látványos lesz szabad költség.

> [AZURE.NOTE] Annak érdekében, hogy a felhasználók próbálja ki az új tároló rétegek és funkciók bejegyzés indítási, a tárhely módosítása díja érvényesítése látványos való gyorsbillentyűk használatát a réteg fog lemondás kikapcsolása addig, amíg a június 30 2016-ban. Július 1 2016 kezdve, a költség fog vonatkozni összes áttűnések a látványos való gyorsbillentyűk használatát. További információt a árak modell Blob-tárolóhoz fiókok megtekintéséhez lap [Azure tároló árak](https://azure.microsoft.com/pricing/details/storage/) esetében. További információt a kimenő adatai átadás díjakat lásd: [Adatok átvitelek árak részletei](https://azure.microsoft.com/pricing/details/data-transfers/) lapon.

## <a name="quick-start"></a>Rövid útmutató

Ebben a részben azt fogja bemutatják az Azure portálon az alábbi esetekben:

- Hogyan lehet Blob-tároló fiók létrehozása.
- Hogyan lehet egy Blob-tároló fiók kezelése.

### <a name="using-the-azure-portal"></a>Az Azure portál használatával

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Az Azure portálon Blob-tároló fiók létrehozása

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. A központi menüben válassza az **Új** > **adatok + tárhely** > **tárterület-fiókot**.

3. Írja be a tárterület-fiókja nevét.

    Ez a név globálisan egyedi; kell lennie. Ez az URL-címet az objektumok tárterület fiók eléréséhez használt részeként használatos.  

4. **Erőforrás-kezelő** jelölje be a telepítési modell.

    Többszintű tároló csak akkor használható erőforrás-kezelő tároló fiókokkal; az új erőforrás a javasolt telepítési modell. További információt tanulmányozza az [Azure erőforrás szolgáltatásának áttekintése](../azure-resource-manager/resource-group-overview.md).  

5. A fiók típusa legördülő listában jelölje ki a **Blob-tárolóhoz**.

    Ez a hol jelölje ki a tárterület-fiók típusát. Többszintű tároló az általános célú tároló; nem érhető el a lehetőség csak a Blob-tároló típusú fiókjában található.    

    Figyelje meg, hogy amikor ezt a lehetőséget választja, a teljesítmény réteg Standard van beállítva. Többszintű tárhely és a prémium verzió teljesítményének réteg nem érhető el.

6. Jelölje ki a replikáció lehetőséget a tárterület-fiókhoz: **LRS**, **GRS**vagy **TS-GRS**. Az alapértelmezett érték **TS-GRS**.

    LRS = helyileg felesleges tároló; GRS = geo felesleges tároló (2 régió); TS-GRS az olvasásra geo felesleges-tároló (2 régiókat olvasási hozzáférést a második).

    További információ az Azure tároló replikációs beállításai kivétele [Azure tároló replikációs](storage-redundancy.md).

7. Jelölje ki a megfelelő tároló réteg az igényeinek: állítsa az **Access réteg** **izgalmas** vagy **interaktív**. Az alapértelmezett érték **interaktív**.

8. Jelölje ki azt az előfizetést, amelyben létre szeretne hozni az új tárterület-fiók.

9. Adja meg az új erőforráscsoport, vagy válassza ki a meglévő erőforráscsoport. További tájékoztatást az erőforrás csoportok az [erőforrás-kezelő Azure áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat.

10. Jelölje ki a régió, a tárterület-fiókjához.

11. Kattintson a **Létrehozás** a tárterület-fiók létrehozása gombra.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>A tároló réteg az Azure portálon Blob tároló fiók módosítása

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).

2. Nyissa meg a tárterület-fiókjába, jelölje ki az összes erőforrás, majd jelölje ki a tárterület-fiókját.

3. Kattintson a beállítások lap **konfigurációs** megtekintése, illetve módosíthatja a fiók konfigurálása.

4. Jelölje ki a megfelelő tároló réteg az igényeinek: állítsa az **Access réteg** **izgalmas** vagy **interaktív**.

5. Kattintson a Mentés elemre a lap tetején.

> [AZURE.NOTE] További díjakat vonhat a tárhely réteg módosítása. Talál további információt a [árak és számlázás](storage-blob-storage-tiers.md#pricing-and-billing) szakaszban.

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Értékelése és Blob-tároló fiókok áttelepítése

Ez a szakasz célja segíti a felhasználókat, hogy egy így fokozatosan térhet át a Blob-tároló fiókokkal. Forgatókönyv két felhasználói:

- Meglévő általános célú tároló fiókja van, és a megfelelő tároló réteg egy Blob tároló fiók módosításának ki kell számítani.
- Úgy döntött, Blob-tároló fiókot használ, vagy már van, és szeretné értékelni, hogy a meleg vagy izgalmas tároló réteg kell használnia.

Mindkét esetben az első sorrend az üzleti is tárolásához és az egy Blob-tároló fiókjában tárolt adatok eléréséhez költségét, és hasonlítsa össze és a jelenlegi költségeket.

### <a name="evaluating-blob-storage-account-tiers"></a>Blob-tároló fiók rétegek értékelése

Tárolásához és egy Blob-tároló fiókjában tárolt adatok eléréséhez költségét becsléséhez szüksége lesz a meglévő használatát mintát felmérése vagy a várt használatát mintát közelíteni. Az általános érdemes tudnia:

- A tárterület-felhasználás – hogyan sok adatok tárolásának alatt, és hogyan módosítja Ez a havi rendszerességgel?
- A tároló access mintát -, hogy mennyi adatot alatt olvassák és a fiók (az új adatokat is beleértve) írt? Hány tranzakció segítségével adatokhoz való hozzáférés, és milyen típusú tranzakciók őket?

#### <a name="monitoring-existing-storage-accounts"></a>Létező tárterület-fiók ellenőrzése

A meglévő tárterület-fiókok figyelésére, és az adatok gyűjtése, teheti Azure tároló Analytics naplózás hajt végre, és mérőszámok adatokat nyújt a tárterület-fiók használata.
Tároló Analytics tárolhatja a mértékek, összesített tranzakció statisztika és kapacitásbeli feldolgozó kérelmek a Blob tárhelyszolgáltatáshoz egyaránt általános célú tároló fiókok, valamint Blob-tároló fiókokat kapcsolatban.
Ezeket az adatokat tároló ugyanazzal a fiókkal a jól ismert táblák vannak tárolva.

További részletekért tekintse át a [Kapcsolatos tárolási Analytics mértékek](https://msdn.microsoft.com/library/azure/hh343258.aspx) és [Tároló Analytics mértékek táblázat séma](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB-tároló fiókok jelenítik meg a táblázat szolgáltatás végpontjának csak történő tárolásához és a fiókhoz tartozó mértékek adatok eléréséhez.

Lync-a tárterület-felhasználás a Blob-tároló szolgáltatás, szüksége lesz ahhoz, hogy a kapacitás mérési módja miatt.
Engedélyezve van a kapacitás adatok rögzítését napi egy tárterület-fiókkal Blob-szolgáltatás, és a tárhely ugyanazzal a fiókkal *$MetricsCapacityBlob* táblázatát írt táblázat bejegyzésének rögzített.

Figyelheti a Blob-tároló szolgáltatás adatok access mintájának, szüksége lesz az óránkénti tranzakció mértékek API szinten engedélyezése.
Engedélyezve van a használati API tranzakciók óránként összesíti, és a tárhely ugyanazzal a fiókkal *$MetricsHourPrimaryTransactionsBlob* táblázatát írt táblázat bejegyzésének felvett. A *$MetricsHourSecondaryTransactionsBlob* tábla rögzíti a másodlagos végpont TS-GRS tárterület-fiókok esetén a tranzakciók.

> [AZURE.NOTE] Abban az esetben, amelyben tárol, oldal BLOB és virtuális gép lemezt blokkolása mellett, és blob-adatok hozzáfűzése általános célú tároló fiókkal rendelkezik, a becsült folyamat esetében nem alkalmazható. Ennek az oka, hogy semmilyen módon megkülönböztető kapacitás lesz, és tranzakció mérőszámok alapú blob-típusú legyen, és a Hozzáfűzés BLOB, amely Blob-tároló fiókhoz telepíthető át.

Ha egy jó közelítését adatok fogyasztása és az access mintát, azt javasoljuk a mértékek, a rendszeres használatát jellemző az adatmegőrzési időszak legördülő menüből válasszon, és extrapolálják.
Egy, hogy megtartja a mértékek adatok 7 napig és a hónap végén elemzéshez hetente adatgyűjtés.
Egy másik, hogy a mértékek adatok megtartja az elmúlt 30 nap és összegyűjtése és elemezheti az adatokat a 30 napos időszak végén.

További információ a munkafüzetek engedélyezéséről adatgyűjtési és megjelenítési mértékek, tekintse át, [így Azure tárolási mértékek és mérőszámok adatainak megtekintése](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Tárolás, elérése és analytics adatainak letöltése is fel hasonlóan a normál felhasználói adatok.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Költségbecslés való használatát mértékek hasznosítása

##### <a name="storage-costs"></a>Tárolási költségek

A sor kulcs *"adatok"* kapacitás mértékek-táblázat *$MetricsCapacityBlob* legújabb bejegyzésében jeleníti meg a felhasználói adatok elfogyasztott tárolókapacitással rendelkezik.
A legújabb bejegyzésében a kapacitás mértékek táblázat *$MetricsCapacityBlob* a sor kulcs *"elemzés"* , az elfogyasztott a analytics naplók tárolókapacitású jeleníti meg.

Analytics naplók (engedélyezés esetén) kattintson az adatok tárolása a tárterület-fiók költsége becsléséhez használhatók, és a teljes kapacitásának elfogyasztott mindkét felhasználói adatok.
Ugyanazzal a módszerrel tárolási költségek blokk becsléséhez is használható, és az általános célú tároló fiókok BLOB Hozzáfűzés.

##### <a name="transaction-costs"></a>Transaction költségek

*"TotalBillableRequests"*, az API a tranzakció mértékek tábla minden bejegyzésnél összegét azt jelzi, hogy az adott API tranzakcióinak száma. *például*az adott időszakra. *"GetBlob"* tranzakciók száma számítható ki az összes Naplóbejegyzéshez tartozó számlázható kérések teljes összege a sor kulccsal *"felhasználó; GetBlob "*.

Annak érdekében, hogy Blob-tároló fiókok tranzakció költségbecslés szüksége lesz a tranzakciók három csoportba lebontva, mivel másképp áron.

- Műveletek, például a *"PutBlob"*, *"PutBlock"*, *"PutBlockList"*, *"AppendBlock"*, *"ListBlobs"*, *"ListContainers"*, *"CreateContainer"*, *"SnapshotBlob"*és *"CopyBlob"*írni.
- Törölje a műveletek, például a *"DeleteBlob"* és *"DeleteContainer"*.
- Egyéb tranzakciók.

Annak érdekében, hogy általános célú tároló fiókok tranzakció költségbecslés kell a művelet API függetlenül minden tranzakcióinak összesítése.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Adatok az access és a replikáció geo adatátviteli költségek

Míg tároló analytics nem nyújt olvassák és a tárterület-fiókhoz írt adatok mennyiségét, nagyjából becslése megjeleníti a tranzakciók mértékek táblába.
*"TotalIngress"* összegét az API a tranzakció mértékek tábla minden bejegyzésben, hogy az adott API bájtban bejövő adatok adatok mennyiségét jelzi.
Hasonlóképpen *"TotalEgress"* összege jelzi kilépési adatok bájtokban mennyiségét.

Annak érdekében, hogy költségbecslés az adatokat az access Blob-tároló fiókok, szüksége lesz a tranzakciók két csoportokba lebontva.

- Adatok beolvasása a tárterület-fiók mennyiségét is lehet becsült elsősorban a *"GetBlob"* és *"CopyBlob"* műveletek *"TotalEgress"* összegét megjeleníti.
- A tároló fiók írt adatok mennyiségét a megjeleníti a elsősorban a *"PutBlob"*, *"PutBlock"*, *"CopyBlob"* és *"AppendBlock"* műveletek *"TotalIngress"* összege is becsült.

A költség geo replikációs adatátviteli Blob-tároló fiókok is a becsült ennyi abban az esetben, ha egy GRS TS-GRS tároló fiókot vagy írt adatok használatával kell számítani.

> [AZURE.NOTE] Kiszámítása a meleg vagy izgalmas tároló réteg a költségek kapcsolatos részletesebb például kérjük, ajánljuk figyelmébe az *interaktív és látványos access rétegek melyek és hogyan kell meg use? melyiket* című Gyakori az [Azure tárhely, oldal árak](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Meglévő adatok áttelepítése

Egy Blob-tároló fiókot csak a továbbfejlesztett fájlblokkolás tárolására szolgáló speciális van, és a Hozzáfűzés BLOB. Általános célú tároló meglévő fiókok, amelyek lehetővé teszik a táblázatok, sorok, fájlok és a lemezt, valamint BLOB tárolhatja, nem alakítható Blob-tároló fiókokhoz. A tároló rétegek használatához be kell Blob-tároló új számlákat hozhat létre, és a meglévő adatok áttelepítése az újonnan létrehozott fiókok.
Az alábbi módszerekkel meglévő adatok áttelepítése Blob tároló fiókokat helyszíni tárolók, külső fél felhőalapú tárolási szolgáltatók vagy a meglévő általános célú tárterület-fiókjából Azure-ban:

#### <a name="azcopy"></a>AzCopy

AzCopy egy nagy teljesítményű adatok másolása, és Azure-tárhelyről készült Windows parancssori segédprogramot. Olyan adatokat másol Blob-tároló fiókját a meglévő általános célú tárterület-fiókjából, illetve a helyszíni tároló eszközök adatainak feltöltése a Blob-tároló fiókba AzCopy használhatja.

További részletekért olvassa el [a AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Mozgás adatforrástárban

Azure tároló elmozdulásnak adatforrástárban a .NET rendszerhez core adatok elmozdulásnak-keretrendszer AzCopy hatásköröket alapul. A tár a nagy teljesítményű, a megbízható és könnyen átadás adatműveleteket AzCopy hasonló lett tervezve. Lehetővé teszi, hogy a teljes előnyei a által biztosított szolgáltatások AzCopy az alkalmazás natív módon anélkül, hogy fut, és a figyeléshez AzCopy külső előfordulását foglalkozik.

További információra kíváncsi olvassa el a [Azure tároló mozgását adatforrástárban a .NET rendszerhez](https://github.com/Azure/azure-storage-net-data-movement) című témakört.

#### <a name="rest-api-or-client-library"></a>REST API-t vagy a tár ügyfél

Az adatok áttelepítése az egyik az Azure ügyfél tárak vagy a Azure tároló szolgáltatást REST API Blob-tároló fiókba egyéni alkalmazás létrehozása Azure tároló több nyelvet és platformok, például a .NET, Java, C++, Node.JS, PHP, fonetikus és Python ügyfélprogramok tárak biztosít. Az ügyfél-tárak például újraküldés logikája, a naplózás és a párhuzamos feltöltések speciális funkciókat kínál. Közvetlenül a REST API-t, amely lehetővé teszi a HTTP-/ HTTPS-kérések bármely nyelvre által hívható szemben is készíthet.

További információra kíváncsi olvassa el az [Azure Blob-tárolóhoz – első lépések](storage-dotnet-how-to-use-blobs.md)című témakört.

> [AZURE.NOTE] Titkosított ügyféloldali titkosítással BLOB a titkosítási kapcsolatos, a blob-ben tárolt metaadatokat tárolhatnak. Teljes mértékben fontos, hogy bármely másolás mechanizmusa biztosítania kell a blob-metaadatok és különösen a titkosítási kapcsolatos metaadatokat, megmarad. Ha másol a BLOB a metaadatok nélkül, a blob-tartalom nem beolvasható újra. Titkosítási kapcsolatos metaadatok kapcsolatban, lásd: [Azure tároló ügyféloldali titkosítást](storage-client-side-encryption.md).

## <a name="faqs"></a>Gyakori kérdések

1. **Meglévő vannak még rendelkezésre álló tárhely fiókok?**

    Igen, a meglévő tároló fiókok továbbra is elérhetők, és árak vagy funkció változatlan marad.  Nem rendelkezik az azt jelenti, hogy egy tároló réteg válassza ezeket, és nem lesz tiering funkciók a jövőben.

2. **Miért és mikor célszerű kezdhetem Blob-tároló fiókokkal?**

    BLOB-tároló fiókok BLOB tárolására szolgáló speciális vannak, és lehetővé számunkra, hogy új blob kötődnek szolgáltatások bevezetésére. Előre fog, Blob-tároló fiókokat az ajánlott módszereket BLOB, mint például hierarchikus tárhely jövőbeli funkciók tárolásához és tiering vezeti a fiók típusának alapján. Célszerű azonban legfeljebb, ha szeretné áttelepíteni, hogy az üzleti követelmények alapján.

3. **Konvertálhatom meglévő tárterület-fiókom Blob-tároló fiókhoz?**

    nem. BLOB-tároló fiók tároló fiók más típusú, és meg kell létrehozása új adatok és áttelepítéséhez a fenti című cikkben ismertetett módon.

4. **Tárolhatom a mindkét tároló rétegek ugyanazzal a fiókkal az objektumok?**

    A *"a hozzáférési szint"* attribútum, amely jelzi a tárhely réteg fiók szinten van beállítva, és a fiókhoz társított összes objektumát vonatkozik. A hozzáférési szint attribútum nem állítható-objektum szintjén.

5. **Megváltoztathatom-e a tárhely réteg Blob-tároló fiók?**

    igen. A tároló réteg módosítása a tárhely számlán a *"a hozzáférési szint"* attribútum megadásával tudja. A tároló réteg módosítása fiók tárolt összes objektum vonatkozni fog. A tárhely a meleg látványos réteg nem merülnek fel, amelyekre bármilyen díjakat, módosítása, a látványos, hogy menet közben módosítása merülnek fel, amelyekre a per GB-os költség minőségű a fiók adatait.

6. **Milyen gyakran módosíthatja a tárhely réteg Blob-tároló fiók?**

    Miközben azt nem engedélyezi a milyen gyakran lehet módosítani a tárhely réteg korlátozása, ne feledje, hogy a tárhely réteg módosítani a látványos, hogy menet merülnek fel, amelyekre jelentős díjak. A tároló réteg gyakran változnak nem javasoljuk.

7. **Lesz a BLOB a izgalmas tároló réteg eltérően a meleg tároló réteg is?**

    A meleg tároló réteg BLOB általános célú tároló fiókok a azonos időtartam, mint BLOB rendelkezik. A izgalmas tároló réteg BLOB van-e általános célú tárterület-fiókok (ezredmásodpercben) szerint BLOB hasonló késés.

    A izgalmas tároló réteg BLOB kissé elérhetősége szolgáltatás alacsonyabb (SLA) a tárolt a meleg tároló réteg BLOB-nál lesz. További információra kíváncsi olvassa el a [SLA tárolására](https://azure.microsoft.com/support/legal/sla/storage)című témakört.

8. **Tárolhatom lap BLOB és virtuális gép lemezre az Blob-tároló fiókok?**

    BLOB-tároló fiókok csak a továbbfejlesztett fájlblokkolás támogatja, és a Hozzáfűzés BLOB, és nem lap BLOB. Azure virtuális gép lemez készül, oldal BLOB, és emiatt Blob-tároló fiókok nem használható virtuális gép lemezt tárolásához. Azonban mindenre blokk BLOB egy Blob-tároló fiókban biztonsági másolatok virtuális gép lemezt tárolhatja.

9. **Fog kell módosíthatja a meglévő alkalmazások Blob-tároló fiókokat használ?**

    BLOB-tároló fiókok 100 %-os API egységes blokk általános célú tárterület-fiókokkal és BLOB Hozzáfűzés. Mindaddig, amíg az alkalmazás BLOB letiltani vagy hozzáfűző BLOB, és a [Tároló szolgáltatások REST API -val](https://msdn.microsoft.com/library/azure/dd894041.aspx) , vagy nagyobb a 2014-02 – 14-es verzióját használja, majd az alkalmazás csak kell működniük. Alkalmazás használatakor a protokoll régebbi verzióját, majd szüksége lesz annak érdekében, hogy mindkét tároló fiókok típusú zökkenőmentes együttműködés az új verzió használatára az alkalmazás frissítéséhez. Általánosságban elmondható hogy mindig a legújabb, függetlenül attól, melyik tárterület-fiók használata ajánlott írja be használja.

10. **Lesz-e a felhasználói felület módosítás?**

    BLOB-tároló fiókok nagyban hasonlítanak ahhoz a továbbfejlesztett fájlblokkolás tárolásához egy általános célú tároló fiókok BLOB hozzáfűző és támogatja az összes fontosabb Azure tárolására, beleértve a magas tartóssági és elérhetősége, méretezhetőség, teljesítmény és biztonsági funkciói. A szolgáltatások és Blob-tároló fiókokhoz bizonyos korlátozások, és annak, hogy milyen egyéb fenti beállításokkal tároló rétegek eltérő változatlan marad.

## <a name="next-steps"></a>Következő lépések

### <a name="evaluate-blob-storage-accounts"></a>Blob-tároló fiókok felmérése

[Régió szerint Blob-tároló fiókok elérhetőségének ellenőrzése](https://azure.microsoft.com/regions/#services)

[Az aktuális tároló fiókok használatát felmérése, mivel az Azure tárolási mértékek](storage-enable-and-view-metrics.md)

[Jelölje be a Blob tároló árak régió szerint](https://azure.microsoft.com/pricing/details/storage/)

[Árak jelölőnégyzet adatok átvitele](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Blob-tároló fiókok használatának megkezdése

[Első lépések az Azure Blob-tárolóhoz](storage-dotnet-how-to-use-blobs.md)

[Az adatok Azure-tárhelyről mozgatása](storage-moving-data.md)

[A AzCopy parancssori segédprogram az adatok átvitele](storage-use-azcopy.md)

[Tallózással keresse meg és feltárása a tárterület-fiókok](http://storageexplorer.com/)
