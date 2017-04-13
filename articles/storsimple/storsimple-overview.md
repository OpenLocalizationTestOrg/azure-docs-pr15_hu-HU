<properties 
   pageTitle="Mi az StorSimple? | Microsoft Azure" 
   description="Ismerteti StorSimple tiering, az eszköz, virtuális eszköz, szolgáltatások és tároló kezelése lapon, és a StorSimple használt kulcsfontosságú kifejezések vezet be." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 sorozat: hibrid felhőalapú tárolási megoldást

## <a name="overview"></a>– Áttekintés

Üdvözli a Microsoft Azure StorSimple, egy tároló tevékenységek között a helyszíni eszközök és a Microsoft Azure felhőbeli tárhelyről kezelő integrált tárolás megoldás. StorSimple egy hatékony költséghatékony és könnyen kezelhető tároló területen hálózati (SAN) megoldást, amely megszünteti a problémák és a vállalati tárolási és adatvédelem társított költségek számos. Azt a saját StorSimple 8000 sorozat eszközt használ, integrálódik a felhőszolgáltatások és összes vállalati tárolására, beleértve a felhőbeli tárhelyről zökkenőmentes nézetének nyújt a felügyeleti eszközök. (A a Microsoft Azure webhelyen közzétett StorSimple telepítési információ StorSimple 8000 sorozat eszközök érinti. Ha egy StorSimple 5000/7000 sorozat készüléket használ, olvassa el [StorSimple súgó](http://onlinehelp.storsimple.com/).)

StorSimple használja a [tárhely tiering](#automatic-storage-tiering) tárolt adatok kezelése különböző adathordozóra keresztül. A jelenlegi munkakészlet tárolt helyszíni a (SSD), ritkábban használatos adatokat tárolja a merevlemez-meghajtók (HDDs), és adatok archiválása tolódik a felhőben. Ezenkívül StorSimple deduplication és használja a tömörítési Rövidítse le a tárhely, hogy az adatok fogyasztása. További információért lépjen [Deduplication és a tömörítést](#deduplication-and-compression). Más kulcs kifejezések és a használt StorSimple 8000 sorozat dokumentációjában fogalmak meghatározását lépjen a [StorSimple terminológia](#storsimple-terminology) Ez a cikk végén.

StorSimple frissítés 2 mint *helyileg kiemelt* győződjön meg arról, hogy az elsődleges adatok maradjon helyi az eszközre, és nem az első csoportba tartozó a felhőbe azonosíthatja a megfelelő kötet. Ez lehetővé teszi, hogy futtatni, amely érzékenyek a felhőben késés, például SQL és virtuális gép munkaterhelésekből, a helyi meghajtóra a kiemelt kötet, miközben továbbra is hozzáférhet a felhőhöz biztonsági másolatának munkaterhelésekből. Helyileg rögzített kötet kapcsolatos további tudnivalókért olvassa el a [kötet kezelése a StorSimple kezelő szolgáltatás használata](storsimple-manage-volumes-u2.md)című témakört. 

2 frissítés is hozhat létre StorSimple virtuális eszközök, amelyek az alacsony késések és Azure prémium tároló által biztosított nagy teljesítményű előnyeit. StorSimple prémium virtuális eszközök kapcsolatos további tudnivalókért lásd: [Deploy és kezelése az Azure virtuális StorSimple eszköz](storsimple-virtual-device-u2.md). Az Azure prémium tárolási kapcsolatos további tudnivalókért kattintson a [prémium tárolási: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

Nemcsak a-tároló kezelése lapon StorSimple adatok védelmi funkciókkal engedélyezése hozhat létre az igény szerinti ütemezett biztonsági másolatok, és ezután tárolja őket helyileg vagy a felhőben. Biztonsági másolatok formájában növekményes pillanatfelvételek, ami azt jelenti, hogy is létrehozott, és gyorsan vissza kell venni. Felhőalapú pillanatképek katasztrófa helyreállítási helyzetekben különösen fontos lehet, mert a csere másodlagos tároló rendszerek (például a szalag biztonsági másolat), és lehetővé teszi az adatok visszaállítása az adatközpont, illetve alternatív webhelyek szükség esetén.

![videó ikon](./media/storsimple-overview/video_icon.png) A videó a Microsoft Azure StorSimple rövid útmutatást talál.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>Miért érdemes használni a StorSimple?

Az alábbi táblázat néhány főbb előnyöket a Microsoft Azure StorSimple biztosít.

| A szolgáltatás | Előny |
|---------|---------|
|Áttetsző integrációja | Microsoft Azure StorSimple iSCSI protokoll segítségével adatokat tároló létesítményekhez alapfeladatokat hivatkozás. Ellenőrzi, hogy az az adatközpont, a felhőben tárolt adatok, vagy a távoli kiszolgáló jelenik meg egyetlen helyen tárolja.|
|Csökkentett tárolási költségek|Microsoft Azure StorSimple osztja ki a megfelelő helyi és felhőbeli tárhelyről aktuális igényeinek kielégítéséhez, és kibővíti a felhőbeli tárhelyről csak akkor, ha szükséges. További csökkenti tárhelyre és a költség kiküszöbölésével ugyanazokat az adatokat (deduplication) felesleges verziója és a tömörítési segítségével.|
|Egyszerűsített tárhely kezelése|Microsoft Azure StorSimple rendszert biztosít felügyeleti eszközök, amelyek segítségével konfigurálhatja és kezelheti a tárolt adatok a helyszíni a távoli kiszolgáló és a felhőben. Ezenkívül kezelése biztonsági mentése és visszaállítása a funkciók a Microsoft Management Console (MMC) beépülő modulból. StorSimple StorSimple kezelési és adat védelme a SharePoint-kiszolgálókon tárolt tartalom meghosszabbítása használható külön, a választható felületet biztosít. |
|Korszerűbb helyreállítási és megfelelőség|Microsoft Azure StorSimple kiterjesztett helyreállítás idő nincs szükség. Ehelyett, visszaállítja az adatokat, szükség esetén. Ez azt jelenti, hogy a minimális zavarok továbbra is a szokásos műveletek. Ezenkívül házirendek adja meg a biztonsági másolat ütemezésüket és az adatmegőrzés beállítható.
|Adatok mobilitás|Töltenek fel a Microsoft Azure felhőszolgáltatások adatok helyreállítás és az áttelepítés céljából más helyekről érhetők el. Ezenkívül virtuális gépeken futó (VMs) fut a Microsoft Azure virtuális eszközök StorSimple tartományi StorSimple is használhatja. A VMs használhatja virtuális eszközök vizsgálati vagy helyreállítási célokra tárolt adatok eléréséhez.|
|Egyéb felhő szolgáltatók támogatása |A StorSimple 8000 sorozat szoftverrel frissítése 1 vagy újabb támogatja az Amazon S3 Erőforrásrekordok, HP és OpenStack cloud services, valamint a Microsoft Azure. (Továbbra is szüksége lesz a Microsoft Azure tároló fiók eszköz céljából.) További információért lépjen [a frissítés 1.2-es újdonságai](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Üzleti folytonosságot | Frissítés, 1 vagy újabb verzió egy új áttelepítési szolgáltatás, amely lehetővé teszi, hogy a StorSimple 5000-7000 sorozat felhasználói adataik áttelepítése StorSimple 8000 sorozat eszköz biztosít.|
|Elérhetőség az Azure kormányzati portálon | Az Azure kormányzati portálon 1 vagy újabb StorSimple frissítés érhető el. További tudnivalókért lásd: [a helyszíni StorSimple eszköz a kormányzati portálon Deploy](storsimple-deployment-walkthrough-gov.md).|
|Adatok védelme és elérhetősége | A StorSimple 8000 sorozat frissítés 1 vagy újabb zóna felesleges tároló (ZRS), támogatja a helyben felesleges tároló (LRS) és Geo felesleges tároló (GRS) mellett. [Ez a cikk a Azure tároló redundancia beállítások](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS részletekért olvassa el.|
|Kritikus alkalmazások támogatása | StorSimple frissítés 2 azonosíthatja a helyi meghajtóra, kiemelt megfelelő kötet. Ez a képesség biztosítja, hogy nem kritikus alkalmazások által igényelt adatok többszintű a felhőbe. Helyi meghajtóra rögzített kötet sem felhő késések vagy csatlakozási problémákat tapasztal. Helyileg rögzített kötet kapcsolatos további tudnivalókért olvassa el a [kötet kezelése a StorSimple kezelő szolgáltatás használata](storsimple-manage-volumes-u2.md)című témakört.|
|Kis késés és a nagy teljesítményű | StorSimple frissítés 2 hozhat létre virtuális eszközök kihasználhatja a nagy teljesítményű kis késés szolgáltatások Azure prémium-tárterületét. StorSimple prémium virtuális eszközök kapcsolatos további tudnivalókért lásd: [Deploy és kezelése az Azure virtuális StorSimple eszköz](storsimple-virtual-device-u2.md).|

![videó ikon](./media/storsimple-overview/video_icon.png) [Ebben a videóban](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) áttekintést a StorSimple 8000 sorozat funkcióit és előnyeit.

## <a name="storsimple-components"></a>StorSimple összetevők

A Microsoft Azure StorSimple megoldást tartalmazza az alábbiakat:

- **Microsoft Azure StorSimple eszköz** – a helyszíni hibrid tárolási tartalmazó tömbök SSD és HDDs, felesleges vezérlők és funkciók automatikusan átveszi együtt. A vezérlők tiering, úgy, hogy éppen használt (vagy meleg) adatok helyi tárolás (az eszköz vagy a helyszíni kiszolgálót), a kisebb a gyakran használt adatok áthelyezése a felhőbe közben tárhely kezelése
- **Virtuális eszköz StorSimple** – néven is ismert StorSimple a virtuális készülék: az az StorSimple eszköz architektúrája másolásához szoftver verziójának és a tényleges hibrid tárolóeszközre a legtöbb képességeit. Egyetlen csomópont-Azure virtuális gépen fut, a StorSimple virtuális eszközt. Prémium virtuális eszközök, amelyek Azure prémium tároló kihasználhatja rendelkezésre állnak a frissítés 2 és újabb.
- **StorSimple kezelő szolgáltatás** – az Azure klasszikus portálon, amellyel a StorSimple vagy StorSimple virtuális eszköz kezelése egyetlen webhely felületről kiterjesztését. A StorSimple kezelő szolgáltatás használatával és szolgáltatások kezelése, megtekintése és eszközök kezeléséhez, figyelmeztetések megtekintése, kötet, kezelése és megtekintése és létrehozása és kezelése biztonsági házirendek a biztonságimásolat-katalógus.
- **A Windows PowerShell-StorSimple** – a parancssor StorSimple eszközök kezeléséhez használható. A Windows PowerShell-StorSimple funkciók, amelyek lehetővé teszik StorSimple eszköze regisztrálni, a hálózati kapcsolat beállítása mobileszközön, bizonyos típusú frissítések telepítéséhez, kapcsolatos hibák elhárítása az eszköz a támogatási munkamenethez elérésével, és módosítsa a eszköz állapotot tartalmaz. Érheti el a Windows PowerShell-StorSimple a soros konzol csatlakozás vagy távelérési a Windows PowerShell használatával.
- **Azure PowerShell StorSimple parancsmagok** – a Windows PowerShell-parancsmagok, amelyek lehetővé teszik a parancssorból szolgáltatói és az áttelepítési feladatok automatizálása gyűjteményét. További információt a StorSimple Azure PowerShell-parancsmagok lépjen a [parancsmagjai – referencia](https://msdn.microsoft.com/library/dn920427.aspx).
- **StorSimple pillanatkép Manager** – a beépülő modult, amely a mennyiségi csoportok és a Windows kötet árnyék szolgáltatás használja az alkalmazás egységes biztonsági másolatok készítése. Ezeken kívül biztonsági ütemterveket és adatfeliratsor hozhat létre és távolíthat el kötet StorSimple pillanatkép Manager is használhatja. 
- **SharePoint-StorSimple kártya** – a Microsoft Azure StorSimple tárolási és adatvédelem a SharePoint Server-farmokon, miközben StorSimple tároló megtekinthető és a SharePoint – központi felügyelet portálról kezelhető átlátszó nyúló eszközben.

Az alábbi ábrán a Microsoft Azure StorSimple architektúrája és -összetevők áttekintést nyújt.

![StorSimple architektúra](./media/storsimple-overview/overview-big-picture.png)

Az alábbi szakaszok ismertetik, minden összetevő részletesebben, és ismertetik hogyan a megoldást adatok rendezése, foglal helyet, illetve elősegíti tároló kezelése lapon, és az adatok védelme. Az utolsó témakör néhány fontos kifejezések és StorSimple összetevők és a kezelés fogalmak definícióinak.

## <a name="storsimple-device"></a>StorSimple eszköz

A Microsoft Azure StorSimple eszköz a helyszíni hibrid tároló tömb elsődleges tárterület és a rajta tárolt adatok iSCSI hozzáférést biztosító. Felhőbeli tárhelyről való kommunikáció kezeli, és lehetővé teszi a biztonság és a Microsoft Azure StorSimple megoldást tárolt adatok titkosságának.

A StorSimple eszköz SSD és merevlemez-meghajtók HDDs, valamint fürtképző és az automatikus feladatátvevő támogatása tartalmazza. Egy megosztott processzor, a megosztott tárhely és a két tükrözött vezérlő tartalmaz. Minden egyes vezérlő az alábbi lehetőségeket nyújtja:

- A host számítógéppel való kapcsolat
- A helyi hálózat (LAN) csatlakozhat legfeljebb hat hálózati portok
- Hardveres figyelése
- Nem ideiglenes elérésű memória (NVRAM), amely megőrzi az adatait, még akkor is, ha megszakad a power
- Fürt-tartsa szem előtt, hogy a frissítések minimális kezelheti a szoftverfrissítések a kiszolgálón egy Feladatátvevőfürt frissítése vagy a nincs hatással a szolgáltatáselérhetőség
- Szolgáltatás, mely függvények, például magas elérhetősége kezeléséről, valamint a kis méretre bármely káros effektusok, amelyek fordulhat elő, ha egy merevlemez vagy SSD sikertelen, vagy a háttéradatbázist fürtre fürt offline állapotban

Csak egy vezérlő olyankor bármely pontján időben. Ha nem sikerül az aktív vezérlő, a második vezérlő automatikusan aktívvá válik. 

További információért lépjen [StorSimple hardver-összetevő és állapotát](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>StorSimple virtuális eszköz

StorSimple hozhat létre a virtuális eszköz másolásához a architektúrája és a tényleges hibrid tárolóeszközre képességeit. Egyetlen csomópont-Azure virtuális gépen fut, a StorSimple virtuális eszköz (más néven a StorSimple virtuális készülék). (A virtuális eszköz csak létrehozhatja az Azure virtuális gépen. Nem hozhatók létre egyet StorSimple eszköz vagy a helyszíni kiszolgálón.) 

A virtuális eszköz az alábbi szolgáltatásokat foglalja magában:

- Úgy viselkednek, mint a hagyományos készülék, és azt is kínálhatnak iSCSI felületet virtuális gépeken futó a felhőben. 
- Tetszőleges számú virtuális eszközök létrehozása a felhőben, és kapcsolja be- és kikapcsolása, ha az szükséges. 
- Helyszíni környezetekbe vészhelyreállítás, fejlesztés és tesztelési forgatókönyvek szimulálhatja segítséget, és segíthetnek a biztonsági mentés elemszintű lekérés. 

A frissítés 2 és újabb a StorSimple virtuális eszköz érhető el két modellt: a 8010 eszköz (korábbi nevén az 1 100 modell) és a 8020 eszközt. A 8010 eszköznek 30 TB maximális kapacitása. A 8020 lehetőséget, és Azure prémium tároló előnyeit 64 TB maximális kapacitással rendelkezik. (Helyi rétegek, a Azure prémium tároló tárolja az adatokat a SSD mivel szabványos tároló tárolja az adatokat a HDDs.) Figyelje meg, hogy fiókkal kell rendelkeznie az Azure prémium tároló prémium tároló használatára. Prémium tároló kapcsolatos további tudnivalókért kattintson a [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

Többet szeretne tudni a StorSimple virtuális eszközt, kattintson a [Deploy és kezelése az Azure virtuális StorSimple eszköz](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple kezelő szolgáltatás

Microsoft Azure StorSimple webes felhasználói felületet (StorSimple kezelő szolgáltatás), amely lehetővé teszi, hogy központilag adatközponthoz kezelése és a felhő tároló biztosít. A StorSimple kezelő szolgáltatás használatával végezze el az alábbi műveleteket:

- StorSimple eszközök rendszer beállításainak konfigurálása
- Konfigurálása és StorSimple eszközök biztonsági beállításainak kezelése.
- Felhőalapú hitelesítő adatok és a tulajdonságok beállítása.
- Konfigurálása és kezelése a kiszolgálón kötet.
- Állítsa be a mennyiségi csoportokat.
- Kevésbé részletes adatok és visszaállítását.
- Teljesítmény figyelését.
- Tekintse át a rendszerbeállítások, és lehetséges problémák azonosítására.

A StorSimple kezelő szolgáltatás segítségével hajtsa végre a rendszer lefelé idő, például a kezdeti telepítés és a frissítések telepítését igénylő kivételével az összes felügyeleti feladatát.

További tudnivalókért olvassa el az [használata a StorSimple kezelő szolgáltatás felügyelete StorSimple eszközére](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>A Windows PowerShell StorSimple használata

A Windows PowerShell-StorSimple létrehozása és kezelése a Microsoft Azure StorSimple szolgáltatás és állíthatja be és figyelése StorSimple eszközökön használható parancssori felületet biztosít. A Windows PowerShell-alapú parancssori felülettel, amely tartalmazza az StorSimple eszköz kezelésére szolgáló dedikált parancsmagok. Funkciók, amelyek lehetővé teszik a Windows PowerShell-StorSimple foglalja magában:

- Regisztráljon az eszközt.
- Állítsa be a hálózati kapcsolaton eszközön.
- Bizonyos típusú frissítések telepítése.
- Az eszköz a támogatási munkamenethez elérése hibaelhárítása.
- Az eszköz állapotának módosítása

Elérheti a Windows PowerShell-StorSimple (a közvetlenül csatlakozik az eszköz gazdaszámítógép) a soros konzolról vagy távolról távelérési a Windows PowerShell használatával. Figyelje meg, hogy bizonyos StorSimple tevékenységek kezdeti eszköz regisztráció, például a Windows PowerShell csak a soros konzolon végezhető. 

További információért lépjen [Használata a Windows PowerShell használata az eszközön való StorSimple](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple-parancsmagok

Az Azure PowerShell StorSimple parancsmag, amelyek lehetővé teszik a parancssorból szolgáltatói és az áttelepítési feladatok automatizálása Windows PowerShell-parancsmagok gyűjteménye. További információt a StorSimple Azure PowerShell-parancsmagok lépjen a [parancsmagjai – referencia](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>StorSimple pillanatkép Manager

Pillanatkép felügyelője StorSimple egy Microsoft Management Console (MMC) beépülő modul egységes, létrehozásához használható helyi biztonsági másolatait pont és az idő és a felhő adatokat. A beépülő modul futtatja a Windows Server-alapú állomáson. Pillanatkép kezelője StorSimple használható:

- Állítsa be, biztonsági mentése és törlése a kötet.
- Állítsa be a mennyiségi csoportok annak érdekében, hogy a biztonsági mentésben adatok egységesek alkalmazás.
- Biztonsági házirendek kezelése, így adatainak előre meghatározott időközönként a biztonsági mentésben és kijelölt helyen tárolt (helyben vagy a felhőben).
- Vissza a kötet, és az egyes fájlokat.

Biztonsági mentés másként pillanatfelvételek, amely csak a módosításokat rögzítése, mivel az utolsó pillanatkép felvételének és távol kevesebb mint teljes biztonsági másolatok helyet igényel, rögzítésének. Biztonsági másolat ütemtervek létrehozása, vagy azonnali biztonsági másolatokat készíthet, szükség szerint. Emellett használhatja StorSimple pillanatkép Manager adatmegőrzési házirendek létrehozásához, hogy hány pillanatfelvételek a program menti vezérlő. Adatok visszaállítása egy biztonsági másolat, StorSimple pillanatkép Manager lehetővé teszi, hogy később szükség esetén a katalógus helyi vagy felhőalapú pillanatképek közül választhat. 

Ha a katasztrófa történik, vagy visszaállíthatja az adatokat egy másik okból van szüksége, ha StorSimple pillanatkép Manager visszaállítja elő van rá szükség szerint. Adatok helyreállítása, hogy, állítsa le a teljes rendszerben visszaállít egy fájlt, a csere berendezések vagy műveletek áthelyezése egy másik webhelyre közben nincs szükség.

További tudnivalókért kattintson a [StorSimple pillanatkép Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePoint-StorSimple kártya

Microsoft Azure StorSimple a StorSimple kártya tartalmaz a SharePoint, átlátszó StorSimple tárolására és adatok védelmi funkciókkal SharePoint server-farmokon nyúló választható összetevője. A kártya működik-e a távoli Blob-tároló (RBS) szolgáltató és az SQL Server RBS szolgáltatás lehetővé teszi a áthelyezése BLOB-kiszolgálón készít biztonsági másolatot a Microsoft Azure StorSimple rendszer. Microsoft Azure StorSimple majd BLOB adatait tárolja helyileg vagy a felhőben használatát alapján.

A SharePoint-StorSimple kártya programból kezelheti a SharePoint – központi felügyelet portálon. Ezért SharePoint-kezelés központi marad, és minden tároló úgy tűnik, hogy a SharePoint-farmban.

További információért lépjen [SharePoint StorSimple kártya](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Tárhely kezelése technológiák
 
Mellett a dedikált StorSimple eszköz virtuális eszköz és más összetevő a Microsoft Azure StorSimple a következő szoftver technológiákat használja az adatok gyors hozzáférést biztosít, és csökkentheti a tárterület-felhasználás:

- [Az automatikus tároló tiering](#automatic-storage-tiering) 
- [Ha vékony kiépítése](#thin-provisioning) 
- [Deduplication és tömörítés](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Az automatikus tároló tiering

Microsoft Azure StorSimple automatikus elrendezése adatainak logikai rétegek aktuális használatát, kor és adatokra mutató kapcsolat alapján. Miközben a felhőbe automatikusan átkerül a kisebb aktív és inaktív adatok legaktívabb-adatok helyi meghajtóra, tárolja. Az alábbi ábra szemlélteti tároló eljárás.
 
![Tárterület-meghatározási StorSimple](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Ha engedélyezni szeretné a gyorselérési, StorSimple tárolja nagyon aktív (nagy) adatokat SSD az StorSimple eszközben. Tárolja az adatokat, amely alkalmanként (adatok meleg) kattintson az eszköz vagy a adatközponthoz kiszolgálók HDDs. Ugrás inaktív adatok biztonsági, és az adatok tartja meg az archiválás és megfelelőség céljából a felhőbe. 

>[AZURE.NOTE] A frissítés 2 vagy újabb megadhatja, hogy egy helyben, kiemelt mennyiségi, ebben az esetben az adatok marad, a helyi eszközt, és nem többszintű a felhőbe. 

StorSimple igazítása újrarendezi adatok és a tárterület-hozzárendelése nem szokásai módosítása. Például információk válhat kevésbé aktív idővel. Fokozatosan kevésbé aktív lesz, mint azt az áttelepített SSD HDDs, majd a felhőben. Azonos adatokat újra aktívvá válik, ha vissza szeretne a tárolóeszközre kerül át.

A tároló tiering folyamat akkor fordul elő, az alábbi képlettel történik:

1. A rendszergazda állítja be a Microsoft Azure felhőalapú tárolási fiókot.
2. A rendszergazda használja a soros konzol és a (az Azure klasszikus portálon fut) StorSimple-kezelő szolgáltatás konfigurálása az eszköztől és a fájl kiszolgálói kötet és az adatok védelme házirendek létrehozása. A helyszíni gépek (például egy fájlkiszolgálókhoz) az internetes kis számítógép rendszer felület használata (iSCSI) elérni az StorSimple eszközt.
3. StorSimple első lépésként az eszköz gyors SSD rétegben tárolja az adatokat.
4. A SSD réteg megközelíti a beosztását, mint StorSimple deduplicates tömöríti a legrégebbi adatblokk, és a merevlemez réteg helyezi őket.
5. A merevlemez réteg az alábbi módszerek kapacitás StorSimple titkosítja a legrégebbi adatblokkok és biztonságos küldi el a Microsoft Azure tároló fiók HTTPS.
6. Microsoft Azure hoz létre az adatok több kópiák az az adatközpont és a távoli adatközpont, biztosítva, hogy az adatok katasztrófa esetén állíthatók helyre. 
7. Ha a fájl kiszolgálói a felhőben tárolt adatokat kér, StorSimple zökkenőmentes adja vissza azt, és másolatának tárolja az StorSimple eszköz SSD rétegben.

### <a name="thin-provisioning"></a>Ha vékony kiépítése

Vékony kiépítési, amelyen a rendelkezésre álló tárhely megjelenik haladja meg a fizikai erőforrások virtualizációs technológia. Előre lefoglalására elegendő tárhely, hanem StorSimple használja a vékony kiépítési lefoglalhat csak elég hely az aktuális követelményeknek. Felhőbeli tárhelyről rugalmas jellegét ezt a megközelítést megkönnyíti, mert StorSimple növelheti vagy csökkentheti a felhőbeli tárhelyről változó igényeinek kielégítéséhez. 

>[AZURE.NOTE] Helyi meghajtóra rögzített kötet nem vékonyan kiépített környezetet. Egy helyi csak mennyiségi rendelt tárolási egészében már kiépítve hangerejének létrehozásakor.

### <a name="deduplication-and-compression"></a>Deduplication és tömörítés

Microsoft Azure StorSimple deduplication és az adatok tömörítést használja tovább csökkentse a tárhelyre.

Deduplication csökkenti a teljes tárolt adatok megadása redundancia kiküszöbölésével tárolt adatok. Amikor adatokat, StorSimple figyelmen kívül hagyja az adatok nem változik, és csak a változásokat rögzíti. Ezeken kívül StorSimple csökkenti a tárolt adatok azonosító, és a felesleges adatok eltávolításával. 

>[AZURE.NOTE] A helyi meghajtóra rögzített kötet adatok nem deduplicated vagy tömörítve. Azonban biztonsági másolatok helyileg rögzített mennyiségének deduplicated, és tömörítve.

## <a name="storsimple-workload-summary"></a>Összefoglaló StorSimple terhelést

A támogatott StorSimple feladatok összefoglalását alatt található táblázatos.

| Eset                  | Terhelést                | Támogatott |  Korlátozások                                  | Verzió              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Közös munka             | Fájl megosztása            | igen       |                                                | Az összes verzió         |
| Közös munka             | Az elosztott fájlrendszer megosztása| igen       |                                                | Az összes verzió         |
| Közös munka             | A SharePoint              | Igen *      |Csak a helyi meghajtóra rögzített kötet támogatott      | Frissítés, 2 vagy újabb verzió   |
| Archiválás                  | Egyszerű fájl archiválása   | igen       |                                                | Az összes verzió         |
| Virtualizációs            | Virtuális gépeken futó        | Igen *      |Csak a helyi meghajtóra rögzített kötet támogatott      | Frissítés, 2 vagy újabb verzió   |
| Adatbázis                  | SQL                     | Igen *      |Csak a helyi meghajtóra rögzített kötet támogatott      | Frissítés, 2 vagy újabb verzió   |
| Videó felügyelete        | Videó felügyelete      | Igen *       |Támogatott funkciók csak a terhelést a kitűzött célja StorSimple eszköz| Frissítés, 2 vagy újabb verzió   |
| Biztonsági másolat                    | Elsődleges cél biztonsági mentése | Igen *      |Támogatott funkciók csak a terhelést a kitűzött célja StorSimple eszköz| 3 és újabb frissítése |
| Biztonsági másolat                    | Másodlagos cél biztonsági mentése | Igen *      |Támogatott funkciók csak a terhelést a kitűzött célja StorSimple eszköz| 3 és újabb frissítése |

*Igen & #42; -Megoldás irányelvek és korlátozások vonatkoznak.*

A következő feladatok StorSimple 8000 sorozat eszközök által nem támogatott. Ha telepíti a StorSimple, ezek munkaterhelésekből eredményezi beállításai nem támogatottak.

-  Orvosi imaging
-  Exchange
-  VDI KÖRNYEZETBEN
-  Az Oracle
-  SAP –
-  Nagy adatok
-  Tartalom terjesztése
-  Indítsa el a SCSI

Az alábbiakban felsoroljuk a támogatott StorSimple infrastruktúra összetevők. 

| Eset | Terhelést      | Támogatott |  Korlátozások                                 | Verzió      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Általános  | Express-továbbítására | igen       |                                               |Az összes verzió |
| Általános  | DataCore FC   | Igen *       |Támogatott DataCore SANsymphony            | Az összes verzió |
| Általános  | ELOSZTOTT FÁJLRENDSZER REPLIKÁCIÓS SZOLGÁLTATÁSA          | Igen *      |Csak a helyi meghajtóra rögzített kötet támogatott     | Az összes verzió |
| Általános  | Az indexelés      | Igen *       |Többszintű kötet, csak metaadatok indexelés támogatott (nincs adat).<br>Helyi meghajtóra rögzített kötet a teljes indexelés támogatott.| Az összes verzió |
| Általános  | A víruskereső    | Igen *       |Többszintű kötet csak a beolvasott képet, kattintson a Megnyitás és a Bezárás támogatott.<br> Helyi meghajtóra rögzített kötet teljes körű keresés funkció használható.| Az összes verzió |

*Igen & #42; -Megoldás irányelvek és korlátozások vonatkoznak.*

## <a name="storsimple-terminology"></a>StorSimple kifejezések 

Mielőtt a Microsoft Azure StorSimple megoldást üzembe helyezné, javasoljuk, hogy olvassa el az alábbi fogalmak és definíciók.

### <a name="key-terms-and-definitions"></a>Fő fogalmak és definíciók

| Kifejezés (rövidítést vagy rövidítése) | Leírás |
| ------------------------------ | ---------------- |
| az Access vezérlő rekord (ACR)    | A Microsoft Azure StorSimple eszközön határozza meg, hogy mely állomások csatlakozhat hozzá egy mennyiségi társított rekordot. A meghatározása a iSCSI alapuló minősített neve (IQN-neve), a hosts (a ACR található), amely az StorSimple eszköz csatlakozik.|
| AES-256                        | Egy 256 bites Advanced Encryption Standard (AES) algoritmus adatok titkosítására helyezi át, és az a felhő. |
| Felosztás egység méretét (Ausztráliai)     | A lehető legkevesebb lemezterületet kiosztható fájl tartása a Windows rendszer fájl. Ha a fájl mérete nem páros többszörösére fürt méretét, plusz térközt kell használni a fájlt (legfeljebb fürt méretének legközelebbi többszörösére) tartsa elveszik szóközt és a merevlemez-meghajtó töredezettség eredményezi. <br>A javasolt Ausztráliai Azure StorSimple kötet 64 KB oka deduplication algoritmusokról a jól működik.|
| automatikus tárolását tiering      | Kevésbé aktív adatok automatikus áthelyezése a SSD HDDs, majd a réteg a felhőben, és segítségével, így a központi kezelőfelületen összes tárhely kezelése.|
| biztonsági másolat katalógus | Biztonsági másolatok, a használt alkalmazás típusa általában kapcsolatos gyűjteménye. Ebben a gyűjteményben a biztonsági másolat katalógusoldal a StorSimple kezelő szolgáltatás felhasználói felület jelenik meg.|
| biztonsági másolat katalógus fájl             | Az adatbázis biztonsági StorSimple pillanatkép Manager jelenleg tárolt elérhető pillanatképek listáját tartalmazó fájlt. |
| biztonsági másolat házirend                   | A kötet, írja be a biztonsági mentés és egy ütemtervet, amely lehetővé teszi, hogy készítsen biztonsági másolatokat előre meghatározott időközönként listáját.|
| nagyméretű bináris objektumok (BLOB)    | Egy adatbázis-kezelő rendszer egy egységként tárolt bináris adatok gyűjteménye. BLOB jellemzően képek, hanganyagok és multimédiás objektumokat, bár néha bináris végrehajtható programkódot BLOB-ként tárolja.|
| Hitelesítési protokoll (CHAP) | A partner, a kapcsolatok, a partner, a jelszó vagy a titkos megosztása alapján hitelesíti protokoll. CHAP lehet egy- vagy kölcsönös. Egyirányú CHAP, az a cél hitelesíti egy kezdeményező. Kölcsönös CHAP és elő kell készítenie, hogy a célhely hitelesíteni a kezdeményező, hogy a kezdeményező hitelesíti a cél. | 
| adatfeliratsor                          | A mennyiségi másolatát. |
|A felhőben, az egy réteg (CaaT)          | A felhő tároló integrált, az egy réteg belül a tárterület-architektúra, hogy az összes tároló úgy tűnik, hogy egy vállalati tároló hálózat részét.|
| felhőalapú szolgáltató (CSP)   | A felhőalapú szolgáltatások számítások szolgáltató.|
| felhőalapú pillanatfelvétel                 | A felhőben tárolt adatok mennyiségi pont és az idő másolata. Felhőalapú pillanatkép megegyezik egy másik, a helyszínen tároló rendszeren replikált pillanatkép. Felhőalapú pillanatképek különösen hasznosak katasztrófa helyreállítási helyzetekben.|
| felhőalapú tárolási titkosítási kulcs   | A jelszó vagy a felhőben az eszköz által küldött titkosított adatok eléréséhez az StorSimple eszköz által használt kulcsot.|
| fürt tudatában frissítése         | Szoftverfrissítések a kiszolgálón egy Feladatátvevőfürt kezelése az, hogy a frissítések minimális vagy szolgáltatáselérhetőség nincs hatással.|
| DataPath                       | Összekapcsolt adatkezelési műveletek hajthatók végre funkcionális egységek gyűjteménye.|
| inaktiválása                     | Egy állandó műveletet, amely a StorSimple eszköz és a kapcsolódó felhőalapú szolgáltatás közötti kapcsolat megszakad. Az eszköz a felhőben pillanatképek ezt a folyamatot után továbbra is, és is klónozva vagy vészhelyreállítás használható.|
| lemezen tükrözése                 | A merevlemez külön logikai lemez mennyiségének replikációs folyamatos rendelkezésre állásának meghajtók valós időben.|
| dinamikus lemez tükrözése         | Dinamikus lemezen logikai lemez mennyiségének replikáció.|
| dinamikus lemezen                  | Lemez mennyiségi formátumot használó tárolhatják és kezelhetik az adatok több fizikai lemezre a logikai Manager (LDM). Dinamikus lemezen is lehet nagyítani adja meg a több szabad helyre.|
| Bővített lemezt olyan (EBOD) ház | A Microsoft Azure StorSimple eszköz további tárterületet extra merevlemez-meghajtó lemezen tartalmazó egy másodlagos ház.|
| FAT kiépítése               | A hagyományos tárolási kiépítési mely tárolóban lévő térközt oszlik alapján várható igényeinek (és általában túl az aktuális van szükség). Lásd még: *Vékony kiépítési*.|
| merevlemez-meghajtó (merevlemez)          | Adatok tárolására elforgatása fémtartályban helyezkednek el használó meghajtóra.|
| hibrid felhőbeli tárhelyről           | A tároló architektúra, beleértve a felhőbeli tárhelyről helyi és a helyszínen erőforrásokat használó.|
| Internetes kis Computer System Interface (iSCSI) | Az Internet Protocol IP-alapú tároló hálózati szabvány adatok tárolására szolgáló berendezésekről vagy létesítményekhez csatolható.|
| iSCSI kezdeményező                 | A szoftver összetevő, amely lehetővé teszi, hogy a host Windows rendszert futtató egy külső iSCSI-alapú tárolási hálózathoz való csatlakozáshoz.|
| iSCSI minősített neve (IQN)      | Egy egyedi nevet, amely azonosítja az iSCSI-tároló vagy kezdeményező.|
| iSCSI-tárolóhoz                    | A szoftver összetevő, ahol a központi iSCSI lemezre alrendszerek tárolási hálózatokban.|
| élő archiválás                  | Tárterület megközelítés, amelyben archiválás adatok érhető el a mindig (ez nem tárolja nem a helyszínen történő szalagos, például). Microsoft Azure StorSimple használja az élő archiválás.|
|helyi meghajtóra rögzített mennyiségi | az eszközön található, és a felhőbe soha nem többszintű kötet. |
| helyi pillanatfelvétel                  | Pont és az idő másolatának mennyiségi tárolt adatokra van Microsoft Azure StorSimple az eszközre.|
| Microsoft Azure StorSimple      | Egy adatközponthoz tároló készülék és a szoftvert, ahol a szervezetek informatikai kihasználhatja a felhőbeli tárhelyről adatközponthoz tárhely, mintha álló hatékony megoldás. StorSimple egyszerűbbé teszi az adatok védelme és adatok kezelése közben költségek csökkentése. A megoldás összesíti az elsődleges tárhely, archiválás, biztonsági mentése és katasztrófa helyreállítási (DR) keresztül zökkenőmentes integráció a felhőben. SAN tárterület és a felhőbeli adatkezelési egy vállalati szintű platformon kombinálásával StorSimple eszközök engedélyezése a sebességét, egyszerűség és megbízhatóságára tárolási kapcsolatos összes igényeknek.|
| A Power és hűtési modul (PCM)  | A power éppen a Kellékek és a hűtőventilátor, ezért a név Power álló, és hűtési modul StorSimple eszköze hardver összetevői. Az elsődleges eszköz ház két 764W PCMs rendelkezik, mivel a EBOD ház két 580W PCMs.|
| elsődleges ház               | Az alkalmazás platform-vezérlőket tartalmazó StorSimple eszköz fő ház.|
| helyreállítási idő cél (RTO)   | Mielőtt egy üzleti folyamat vagy a rendszer a rendszer arra kell fordított idő legnagyobb mennyiségű teljesen helyreáll katasztrófa után.| 
|egymás után csatolt SCSI (Társítások)       | A típus merevlemez-meghajtók (merevlemez).|
| szolgáltatás adatok titkosítási kulcs     | Egy kulcsot minden új StorSimple eszközre, amely a StorSimple kezelő szolgáltatás regisztrálja elérhető. A konfigurációs adatok átvitele a StorSimple kezelő szolgáltatás és az eszköz között nyilvános kulccsal titkosított, és kattintson a titkos kulccsal eszközön fejthető. Szolgáltatás adatok titkosítási kulcs lehetővé teszi, hogy a szolgáltatás a titkos kulcs az visszafejtés juthat.|
| szolgáltatás regisztrációs kulcs        | Egy kulcsot, amelyek segítségével regisztrálhatja a StorSimple eszköz StorSimple Manager szolgáltatással, hogy úgy tűnik, további adatkezelési műveletek az Azure klasszikus portálon.|
| Kis Computer System Interface (SCSI) | Fizikailag számítógépek kapcsolódik, és haladnak adatok közöttük szabványát csoportja.|
| egyszínű állam meghajtó (SSD)         | Nincs mozgó részek; tartalmazó lemezen Ha például egy USB-meghajtóra.|
| tárterület-fiók                 | Access hitelesítő adatait egy adott felhő szolgáltató csatolja a tárterület-fiókjába.| 
| SharePoint-StorSimple kártya| A Microsoft Azure StorSimple összetevő StorSimple tárolási és az adatok védelme a SharePoint server-farmokon átlátszó nyúló.|
| StorSimple kezelő szolgáltatás      | Az Azure klasszikus portálon, amely lehetővé teszi, hogy a helyszíni Azure StorSimple és a virtuális eszközök kezelése kiterjesztését.|
| StorSimple pillanatkép Manager     | A Microsoft Management Console (MMC) beépülő modul biztonsági mentési és visszaállítási művelet a Microsoft Azure StorSimple kezelésére szolgáló.|
| biztonsági másolat készítése                     | Ez a szolgáltatás lehetővé teszi, hogy a felhasználót, hogy a kötet egy interaktív biztonsági másolat készítése. Egy másik mód véve egy kézi biztonsági mentést, nem pedig a véve egy definiált házirenden keresztül az automatikus biztonsági másolat mennyiségig.|
| Ha vékony kiépítése               | A módszer, amellyel a rendelkezésre álló tárhely tároló rendszerek használják a hatékonyság optimalizálása. A vékony kiépítési tárolására felosztása több felhasználó minden felhasználó által megadott bármikor szükséges minimális terület alapján között. Lásd még: *fat kiépítési*.|
| tiering | Logikai csoportok alapján az aktuális használatát, kor és adatokra mutató kapcsolat adatainak elrendezését. StorSimple automatikus rendezése a fenti adatok.   |
| mennyiségi                          | Logikai tárolási területek meghajtók formában. A host, beleértve a talált iSCSI és StorSimple eszköz való csatlakoztatva kötet StorSimple kötet felelnek meg.|
 | mennyiségi tároló                | Egy csoportosítási kötet és a rájuk vonatkozó beállításokat. Minden kötet StorSimple eszköze mennyiségi tárolók vannak csoportosítva. Mennyiségi tároló beállítások közé tartoznak, tárterület fiókok, a kapcsolódó titkosítás billentyűkkel cloud küldött adatok és a felhő érintő műveletek sávszélességre titkosítási beállításait.|
| mennyiségi csoport                    | A StorSimple pillanatkép Manager mennyiségi csoport gyűjteménye biztonsági feldolgozás megkönnyítése érdekében konfigurált mennyiségének.|
| Kötet árnyék szolgáltatás (VSS)| A Windows Server operációs rendszer szolgáltatás, amely elősegíti az alkalmazás konzisztencia kommunikáció növekményes pillanatképek létrehozásának koordinálása VSS-et használó alkalmazások által. VSS biztosítja, hogy az alkalmazások esetén ideiglenesen inaktív pillanatképek vesznek.|
| A Windows PowerShell StorSimple használata | A Windows PowerShell-alapú parancssor működik, és az StorSimple eszköz kezelésére szolgál. A Windows PowerShell egyszerű funkcióinak részét megőrzésével a kapcsolatnak további dedikált parancsmagokról felé StorSimple eszköz kezelése szolgálják.|


## <a name="next-steps"></a>Következő lépések

Tudjon meg többet [StorSimple biztonsági](storsimple-security.md).




 

 
