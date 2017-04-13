<properties 
    pageTitle="Bevezetés az Azure-gyorsítótár prémium vgx.dll réteg |} Microsoft Azure" 
    description="Útmutató: létrehozása és kezelése vgx.dll adatmegőrzési, vgx.dll fürtözés, és a prémium réteg Azure vgx.dll gyorsítótár-példányok VNET támogatása" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Az Azure-gyorsítótár prémium vgx.dll réteg – bevezetés
Azure vgx.dll gyorsítótár elosztott, felügyelt gyorsítótárat kialakíthatja jól méretezhető és válaszol alkalmazásokat az adatok erősen gyors hozzáférést biztosítva. 

Az új prémium réteg egy vállalati készen áll a réteg, amelyek tartalmazzák még a normál szintű-funkciók és az egyéb, például jobb teljesítményt, nagyobb munkaterhelésekből, vészhelyreállítás, importálás/exportálás és fokozott biztonság. Folytatni szeretné az olvasást, ha többet szeretne tudni a prémium gyorsítótár réteg további funkcióját.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Jobb teljesítményt és a normál vagy Basic réteg összehasonlítása
**Jobb teljesítményt vagy egyszerű keresztül Standard réteg.** A támogatás réteg gyorsítótárát hardveren, amelyet gyorsabb processzor és viszonyítva hatékonyabb biztosít a Basic vagy szabványos réteg telepítik. Prémium réteg gyorsítótárát nagyobb teljesítmény és az alsó késések van. 

**Az azonos méretű gyorsítótár átviteli képest szabványos réteg prémium feljebb kerül.** Például a egy 53 GB P4 átviteli (prémium) gyorsítótár 250K kérelmek mint 150 K C6 másodpercenként (normál).

Méret, átviteli és a prémium gyorsítótárát sávszélesség kapcsolatos további tudnivalókért olvassa el a [Azure vgx.dll gyorsítótár – gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) című témakört.

## <a name="redis-data-persistence"></a>Adatok állandó vgx.dll
A prémium réteg lehetővé teszi, hogy továbbra is fennáll a Azure tároló fiók gyorsítótár adatait. Egy egyszerű/Standard gyorsítótár az összes adatot tároló csak a memóriában. Abban az esetben, ha mögöttes infrastruktúra problémák vannak adatvesztést is lehet. Azt javasoljuk, hogy funkcióval vgx.dll adatok megőrzése a prémium réteg adatvesztés tűrőképessége növeléséhez. Azure vgx.dll gyorsítótár Rekordadatbázis és (hamarosan) AOF lehetőséget kínál az [adatmegőrzési vgx.dll](http://redis.io/topics/persistence). 

Adatmegőrzési konfigurálásával kapcsolatos útmutatásért lásd: az [adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Vgx.dll fürthöz
Ha gyorsítótárát 53 GB-nál nagyobb létrehozása, illetve az shard adatokhoz több vgx.dll csomópontok keresztül, a vgx.dll fürtözés, amely érhető el a támogatási réteg is használhatja. Az egyes csomópontok magas elérhetőség Azure által felügyelt elsődleges/replika gyorsítótár két áll. 

**Vgx.dll fürtözés lépve maximális méret és az átvitel.** Átviteli növekszik lineárisan shards (csomópontok) a fürt számának növelése. Pl.. Ha a 10 shards P4 fürt létrehozásakor, akkor érhető el az átviteli 250K * 10 2,5 millió kérelmek másodpercenként =. Kérjük, olvassa el az [Azure-gyorsítótár Gyakori vgx.dll](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) méretét, átviteli és a prémium gyorsítótárát sávszélesség olvashat bővebben.

Az első lépések fürtözés, megtudhatja, [hogy miként fürtözőszoftverét prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Fokozott biztonság és elkülönítési

A Basic vagy normál réteg létrehozott gyorsítótárát nyilvános internetes érhetők el. A gyorsítótárban való hozzáférés korlátozva a hívóbetű alapján: Az a prémium réteg további biztosíthatja, hogy csak a megadott hálózati ügyfeleinek hozzáférhet a gyorsítótár. [Azure virtuális hálózati (VNet)](https://azure.microsoft.com/services/virtual-network/)vgx.dll gyorsítótár telepítheti. Használhatja az összes funkcióját alhálózat, hozzáférési szabályok, például VNet, és más funkciók további elérésének korlátozásához vgx.dll.

További információért megtudhatja, [hogy miként virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Az importálás/exportálás

Az importálás/exportálás egy Azure vgx.dll gyorsítótár adatok kezelése művelet, amely lehetővé teszi az adatok importálása az Azure vgx.dll gyorsítótár vagy adatok exportálása az Azure vgx.dll gyorsítótárból importálása és exportálása vgx.dll gyorsítótár-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótárból az Azure tárterület-fiókokban található lap blob. Lehetővé teszi, hogy a különböző Azure vgx.dll gyorsítótár-példányok közötti áttelepítéséhez, vagy a gyorsítótár használat előtt adatokkal.

Importálás a bármely vgx.dll bármelyik felhő vagy környezetre, beleértve a vgx.dll Linux rendszerhez, a Windows vagy bármely felhő például Amazon webszolgáltatásokhoz és mások futó operációs rendszert futtató kiszolgáló kompatibilis Rekordadatbázis fájlokat vgx.dll tárolt használható. Adatok importálása az ugyanígy gyorsítótár létre előre megadott adatokkal. Az importálási folyamat során Azure vgx.dll gyorsítótár Rekordadatbázis fájlok betölti Azure tárhelyről a memóriában, és a billentyűk majd beillesztése a gyorsítótár.

Exportálás az Azure-vgx.dll vgx.dll kompatibilis Rekordadatbázis fájlokat a gyorsítótárban tárolt adatokat exportálni teszi lehetővé. Ez a funkció használatával adatok áthelyezése egyik Azure vgx.dll gyorsítótár példányából, vagy egy másik vgx.dll kiszolgálóra. Az exportálási folyamat során ideiglenes fájl a virtuális, amelyen az Azure vgx.dll gyorsítótár-kiszolgálói példány jön létre, és a fájlt töltenek fel a kijelölt tárterület-fiókjába. Az exportálási művelet befejeztével vagy állapotban sikeres vagy sikertelen az ideiglenes fájlok törlődik.

További tudnivalókért lásd: [az adatimportálás és adatexportálás Azure vgx.dll gyorsítótárból](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Indítsa újra

A támogatás réteg indítsa újra a gyorsítótár igény szerinti egy vagy több csomópontjai teszi lehetővé. Lehetővé teszi az alkalmazás tűrőképessége hiba esetén próbálhatja ki. A következő csomópontokat is indítsa újra.

-   A gyorsítótár fő csomópont
-   A gyorsítótár kisegítő csomópont
-   Diaminta és a kisegítő csomópontok a gyorsítótár
-   Prémium gyorsítótárat a fürtözés használatakor is indítsa újra a fő, kisegítő vagy mindkét csomópont-gyorsítótár egyedi shards

További tudnivalókért olvassa el a [Indítsa újra](cache-administration.md#reboot) , és [Indítsa újra a gyakori kérdések](cache-administration.md#reboot-faq)című témakört.

## <a name="schedule-updates"></a>Frissítések ütemezése

Az ütemezett frissítés funkció lehetővé teszi, hogy kijelöli a gyorsítótár karbantartási az ablak. Ha a Karbantartás ablakban megadva, a minden vgx.dll kiszolgáló frissítése során az ablak történik. A Karbantartás ablakban kijelöl, jelölje ki a kívánt nap, és adja meg a karbantartás ablak indítsa el az egyes napokon óra. Ne feledje, hogy a karbantartás ablak idő UTC szerint megadva. 

További tudnivalókért olvassa el a [ütemezése](cache-administration.md#schedule-updates) és az [Ütemezés frissítések – gyakori kérdések](cache-administration.md#schedule-updates-faq)című témakört.

>[AZURE.NOTE] Csak vgx.dll során az ütemezett karbantartás ablak módosításai frissítések kiszolgáló. A Karbantartás ablakban nem vonatkozik az Azure és frissítések a virtuális operációs rendszerben.

## <a name="to-scale-to-the-premium-tier"></a>Ha át kívánja méretezni a prémium réteg való

A prémium réteg méretezéséhez egyszerűen válasszon egyet a prémium meghatározási kattintson a **Módosítás árak réteg** lap. A PowerShell és CLI prémium réteg a gyorsítótár is méretezheti. Lépésenkénti útmutatásért lásd: [hogyan skála Azure vgx.dll gyorsítótár](cache-how-to-scale.md) és [egy méretezési művelet automatizálása](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Következő lépések

Gyorsítótár létre, és ismerje meg az új réteg prémium szolgáltatásaihoz.

-   [Adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md)
-   [Virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-vnet.md)
-   [Prémium Azure vgx.dll gyorsítótár fürtözőszoftverét konfigurálása](cache-how-to-premium-clustering.md)
-   [Adatok importálása és exportálása hogyan Azure vgx.dll gyorsítótárból](cache-how-to-import-export-data.md)
-   [Azure vgx.dll gyorsítótár felügyelete](cache-administration.md)
  

