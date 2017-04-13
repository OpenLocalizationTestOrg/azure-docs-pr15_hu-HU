<properties
  pageTitle="Azure és Linux virtuális tároló |} Microsoft Azure"
  description="Azure Standard és a prémium tárhely a Linux virtuális gépeken futó mutatja be."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure és Linux virtuális tárolása

Azure tároló a felhőalapú tárolási megoldást tartóssági, elérhetőségét és méretezhetőség az igényeknek megfelelően ügyfeleikkel támaszkodó modern alkalmazásokhoz.  Mellett, amely lehetővé teszi a fejlesztők számára új eseteket támogató nagyméretű alkalmazások, Azure tárterület is biztosít a tárhely foundation az Azure virtuális gépeken futó.

## <a name="azure-storage-standard-and-premium"></a>Azure tárolási: Szabványos és prémium verzió

Azure virtuális szabványos tároló lemezt vagy prémium tároló lemezt épül fel.  Amikor a portál segítségével válassza ki a virtuális Váltás a szabványos, és a premium megtekintése alapjai képernyőn legördülő listában.  Az alábbi kép kiemeli az adott váltása menü.  Ha állítottuk SSD, csak prémium tároló engedélyezett VMs jelenik meg, hogy be SSD által támogatott összes meghajtók.  Állítottuk be a merevlemezen, amikor a szabványos tároló engedélyezve VMs biztonsági frissítésjelző lemezmeghajtók fog megjelenni, VMs mögött SSD prémium tároló együtt.

  ![képernyő1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

Az egy virtuális létrehozásakor a `azure-cli` választhat normál és prémium keresztül virtuális méretének kiválasztásakor az `-z` vagy `--vm-size` cli jelző.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Hozzon létre egy virtuális a cli a szabványos adathordozós virtuális

A cli jelző `-z` Standard_A1 választ ki az A1 egy szabványos tároló alapul Linux virtuális.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Hozzon létre egy virtuális a cli prémium formában

A cli jelző `-z` Standard_DS1 választ ki a DS1 prémium tárolási alapul Linux virtuális.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Szabványos tárhely

Azure szabványos tárhely a tárhely alapértelmezett típusa.  Szabványos tároló lép hatályba költség, miközben továbbra is performant.  

## <a name="premium-storage"></a>Prémium tárhely

Azure prémium tároló biztosítja e/O-igényes munkaterhelésekből futó virtuális gépeken futó nagy teljesítményű, alacsony-késés lemezt támogatása. Virtuális gép (virtuális) lemezt prémium tároló használó adattárolásra (SSD). Az alkalmazás virtuális lemez telepítheti át az Azure prémium tároló előnyeit, a sebesség és a teljesítmény lemezt a következő állapotba.

Kiemelt tároló szolgáltatásai:

- Prémium tároló lemez: Azure prémium tároló támogatja a DS, DSv2 vagy GS sorozat Azure VMs lehet csatolni virtuális lemezt.

- Prémium lap Blob: A prémium tároló lap Azure BLOB, tartsa lenyomva az ujját állandó lemezen az Azure virtuális gépeken futó (VMs) használt támogatja.

- Premium helyileg felesleges tárterület: Egy prémium tárterület-fiók csak helyileg felesleges tároló (LRS) a replikáció lehetőségként támogatja és továbbra is az adatok másolatait három egyetlen terület belül.

- [Prémium tárhely](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Prémium tároló támogatott VMs

Prémium tároló támogatja DS sorozat, DSv2 sorozathoz GS adatsort, és Fs-sorozat Azure virtuális gépeken futó (VMs). Normál és a prémium tároló lemezt VMs támogatott prémium adathordozós is használhatja. De nem használhat prémium tárolási lemezre virtuális sorozat, amelyek nem kompatibilis prémium tárolási.

Az alábbiakban prémium adathordozós azt érvényesített Linux terjesztését.

| Ki. | Verzió                 | Támogatott Kernel    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5 6.6, a 6,7, 7.0-s, 7.1. | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Tárhely

Azure tárhelyet biztosít fájlmegosztások a felhőben, a normál kis-és Középvállalatok protokoll használatával. Azure fájlokkal áttelepítheti a vállalati alkalmazások fájlkiszolgálókhoz az Azure támaszkodó. Azure-ban futó alkalmazások egyszerűen csatlakoztathat fájlmegosztások az Azure virtuális gépeken futó Linux operációs rendszert futtató. És a tárhely a legújabb verzióját is lehet csatlakoztatni fájlmegosztás, amely támogatja a kis-és Középvállalatok 3.0 helyszíni alkalmazásból.  Mivel fájlmegosztások kis-és Középvállalatok megosztások, szokásos fájlrendszert API-khoz keresztül elérhetők legyenek.

Tárhely épül ugyanazt a technológiát Blob, a táblázat és a várólista tárolására, így tárhelyet biztosít az elérhetőség, tartóssági, méretezhetőség és a tárhely Azure platform beépített geo redundancia. Fájlt tároló teljesítmény tárolók és korlátai kapcsolatos részletekért lásd: Azure tároló méretezhetőség és a teljesítmény célok.

- [Azure fájltároló használata Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Meleg tárhely

Az Azure meleg tárolási réteg gyakran használt adatok tárolására van optimalizálva.  Meleg tároló blob tárolja alapértelmezett tároló típusát.

## <a name="cool-storage"></a>Izgalmas tárhely

Az Azure izgalmas tároló réteg, amely a ritkán használt és élettartamú adatok tárolására van optimalizálva. Példa használati eset izgalmas tárolására biztonsági másolatokat, médiatartalom, tudományos adatok, megfelelőség és archiválás adatok tartalmazzák. Az általános bármely ritkán használt adata izgalmas tároló tökéletes csatlakozni kívánó.

|                             | Réteg meleg tárhely      | Izgalmas tároló szint     |
|:----------------------------|:---------------------:|:---------------------:|
| Elérhetőség                | 99,9 %                 | 99 %-os                   |
| Elérhetőség (TS-GRS olvasás) | 99.99 %                | 99,9 %                 |
| Használati költségek               | Nagyobb tárolási költségek  | Alsó tárolási költségek   |
|                             | Alsó access          | Magasabb szintű hozzáférés         |
|                             | és tranzakció költségek | és tranzakció költségek |


## <a name="redundancy"></a>A redundancia

A Microsoft Azure tárolási fiók adatainak mindig replikált, tartóssági és magas elérhetőség az Azure tárolási SLA tranziens hardver hibák ellenére is az értekezlet biztosítása érdekében.

Amikor létrehoz egy tárterület-fiókkal, választania kell az alábbi replikációs lehetőségek közül:

- Helyi meghajtóra felesleges tároló (LRS)
- Zóna felesleges tároló (ZRS)
- GEO felesleges tároló (GRS)
- Olvasás-elérés geo felesleges tároló (TS-GRS)

### <a name="locally-redundant-storage"></a>Helyi meghajtóra felesleges tárhely

Helyi meghajtóra felesleges tároló (LRS) replikálja belül a régió, amelyben a tárterület-fiók létrehozása az adatok. Tartóssági maximalizálása, hogy minden kérelme ellen adatokat tároló fiókban van replikált háromszor. Ezek a három replikák külön hibafa tartományok és a frissítési tartományok tárolnak.  Kérés lekérdezése sikeresen csak egyszer az összes három másolatnál íródott

### <a name="zone-redundant-storage"></a>Zóna felesleges tárhely

Két vagy három létesítményekhez, egy egyetlen régión belüli vagy a két régió keresztül LRS-nál magasabb tartóssági nyújtó között is zóna felesleges tároló (ZRS) replikál az adatok. Ha a tárterület-fiók ZRS engedélyezve van, az adatok tartós, még akkor is, ha a hiba történt a lehetőségek közül.

### <a name="geo-redundant-storage"></a>GEO felesleges tárhely

GEO felesleges tároló (GRS) replikálja az adatok egy másodlagos terület, amely több száz mérföld az elsődleges régió hagyta. Ha a tárterület-fiók GRS engedélyezve van, az adatok még akkor is, ha egy teljes területi üzemszünetek vagy az elsődleges régió nem helyreállítható katasztrófa tartós.

### <a name="read-access-geo-redundant-storage"></a>Olvasás-elérés geo felesleges tárhely

Olvasás-elérésű geo felesleges tárolás (TS-GRS) teljes méretűre nagyítása elérhetőségét a tárhely fiók, hogy csak olvasható hozzáférhetővé tenné másodlagos hely, mellett a replikáció GRS által megadott két sorterület át az adatokat. Abban az esetben, ha az adatok az elsődleges régióban érhető el, az alkalmazás adatok erről a másodlagos területről.

-Mély merülési az Azure tároló redundancia lásd:

- [Azure tároló replikációs](../storage/storage-redundancy.md)

## <a name="scalability"></a>Méretezhetőség:

Azure tároló nagymértékben méretezhető megnyitva, tárolhatja és tudományos, pénzügyi elemzés, és a multimédia-alkalmazások által igényelt terabájt támogatja az adatok nagy jelenik meg az adatok több száz folyamata. Vagy a kis mennyiségű adat szükséges a kisvállalati webhelyet tárolhat. Tartoznak az igényeinek, ahol kifizeti csak az adatokat tárolja. Azure tároló jelenleg tárolja az egyedi ügyfél-objektumok trillions tízesre, és kérelmek egy második milliónyi átlagosan kezeli.

Szabványos tároló fiókok: szabványos tároló fióknak 20 000 IOPS teljes kérések maximális sebesség. A teljes IOPS összes a virtuális gép merevlemezeken egy szabványos tárterület-fiókban nem haladhatja meg ezt a korlátot.

Prémium verzió tároló használata: A prémium tároló fióknak 50 GB/s teljes maximális sebesség mértéke. A teljes kapacitásának összes virtuális lemezt keresztül nem haladhatja meg ezt a korlátot.

## <a name="availability"></a>Elérhetőség

Azt garantálja, hogy legalább 99.99 % (99,9 %-os izgalmas hozzáférési szint) az idő, azt fogja sikeresen összehívások adatainak beolvasása olvasási hozzáférést-Geo felesleges tároló (TS-GRS) fiókok, feltéve, hogy a másodlagos régió, az adatok beolvasása az elsődleges régió sikertelen kísérletek újból vannak.

Azt garantálja, hogy legalább 99,9 % (99 %-os izgalmas hozzáférési szint) az idő, azt fogja sikeresen összehívások olvasható az adatok helyileg felesleges tárterület (LRS), a zóna felesleges tárterület (ZRS) és a fiókok Geo felesleges tárterület (GRS).

Azt garantálja, hogy legalább 99,9 % (99 %-os izgalmas hozzáférési szint) az idő, azt fogja sikeresen összehívások helyileg felesleges tároló (LRS), a zóna felesleges tároló (ZRS), és a Geo felesleges tároló (GRS) fiókok és olvasási hozzáférést-Geo felesleges tároló (TS-GRS) fiókok adatokat írni.

- [Azure SLA tárolására](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Területek

Azure általában elérhető a világon 30 régióban, és rendelkezik bejelentve a terv 4 további terület. Földrajzi kiterjesztett oka az Azure prioritást lehetővé teszi, hogy ügyfeleink nagyobb teljesítmény elérése érdekében, és hogy támogatja a követelmények és a vonatkozó adatok helye beállítások.  Németország azures legújabb régió indítására van.

A Microsoft Cloud németországi tartalmaz egy eltérő beállítás a Microsoft Cloud már elérhető szolgáltatások hatóságok, innováció megnövelt lehetőségeit és economic NÖV erősen szabályozott partnerek és ügyfelek létrehozásával Németország, az Európai Unió és az Európai ingyenes kereskedelmi társítási (EFTA).

Felhasználói adatok Magdeburgban és Frankfurt, az alábbi új adatközpont esetén egy adatok a meghatalmazott, T – rendszerek nemzetközi, egy független német cég és német Telekom leányvállalat felügyelete alatt kezeli. A Microsoft kereskedelmi felhőszolgáltatások ezek adatközpont esetén szabályok kezelése német adatok igazodjon, és adjon ügyfelek további lehetőségeket, hogy hol és hogyan adatfeldolgozás.


- [Azure régiók térkép](https://azure.microsoft.com/regions/)

## <a name="security"></a>Biztonsági

Biztonsági funkciók, amelyek közös engedélyezése a fejlesztők számára biztonságos alkalmazások átfogó Azure tárhelyet biztosít. A tároló fiókon védhetők szerepköralapú hozzáférés-vezérlés és Azure Active Directory. A védett adatok között egy alkalmazást, és Azure ügyféloldali titkosítást, a HTTPS vagy a kis-és Középvállalatok 3.0 használatával. Adatok beállítható, hogy automatikusan titkosítása Azure tárolóhoz írásakor tárolási szolgáltatás titkosítási (SSE) segítségével. Operációs rendszer és az adatok lemezre virtuális gépeken futó által használt adhatja meg az Azure lemezre titkosítással titkosítható. A data objects Azure-tárolóban lévő delegált eléréséhez lehet adni a megosztott Access aláírások használata.

### <a name="management-plane-security"></a>Adatkezelési sík biztonsági

A kezelő sík áll, az erőforrások használt tárterület-fiókjának kezelését. Ebben a részben fogja megvitatjuk, az erőforrás-kezelő Azure telepítési modell és szerepköralapú hozzáférés szerepalapú használatáról a tárterület-fiók elérésének szabályozása. A tároló fiók kulcsok, és hogyan újragenerálása őket kezelésével kapcsolatban is előadás.

### <a name="data-plane-security"></a>Adatok sík biztonsági

Ebben a részben áttekintjük a tárterület-fiókjában BLOB, a fájlokat, a sorok és a táblázatok, például a tényleges data objects hozzáférést megosztott Access aláírások használatával és az Access-házirendek tárolt. Témákat vesszük sorra szolgáltatói Társítások és a fiók szintű Társítások. Azt is megtalálja, egy adott IP-cím (vagy az IP-címek) való hozzáférés korlátozása, hogy miként szeretné korlátozni a használt HTTPS protokollt, illetve vissza szeretné vonni az Access megosztott aláírás nem várja meg, hogy lejárjon.

## <a name="encryption-in-transit"></a>A hálózaton átvitt titkosítás:

Ez a szakasz ismerteti a védik az adatokat, amikor azt át és elhalványítása Azure tároló. Megvitatjuk fogja HTTPS és Azure fájlmegosztások a kis-és Középvállalatok 3.0 által használt titkosítással ajánlott használatát. Azt a fog is ajánljuk figyelmébe az ügyféloldali titkosítás, amely lehetővé teszi az adatok titkosítása, mielőtt tárolóba ügyfélalkalmazás át és visszafejteni az adatokat, után átkerül tároló ki.

## <a name="encryption-at-rest"></a>A többi titkosítás:

Tárterület szolgáltatás titkosítási (SSE), és hogyan kapcsolhatja be tárolási fiókot, a továbbfejlesztett fájlblokkolás BLOB, oldal BLOB, így és Azure tárolóhoz írásakor az automatikusan titkosított BLOB hozzáfűző előadás. Azt is néznének hogyan Azure merevlemez-titkosítás használatára, és ismerje meg a alapvető különbség és a lemez titkosítást és SSE ügyféloldali titkosítást és eseteit. Megnézi fog röviden, az Amerikai Egyesült Államok kormányzati számítógépek a FIPS.

- [Azure tároló biztonsági útmutató](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Megtakarítási költség

- [Tárterület költség](https://azure.microsoft.com/pricing/details/storage/)

- [Költség tárterület-kalkulátor](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Tárterületkorlátok

- [Tárterületkorlátok szolgáltatás](../azure-subscription-service-limits.md#storage-limits)
