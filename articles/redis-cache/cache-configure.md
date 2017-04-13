<properties 
    pageTitle="Azure vgx.dll gyorsítótár beállítása |} Microsoft Azure"
    description="Az alapértelmezett vgx.dll konfiguráció Azure vgx.dll gyorsítótár megismerni, és megtudhatja, hogy miként konfigurálható az Azure vgx.dll gyorsítótár-példányok"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="douge"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="08/25/2016"
    ms.author="sdanie" />

# <a name="how-to-configure-azure-redis-cache"></a>Azure vgx.dll gyorsítótár beállítása

Ez a témakör azt ismerteti, hogyan ellenőrizheti és frissítheti az adatokat az Azure vgx.dll gyorsítótár-példányok, és a alapértelmezett vgx.dll beállítása Azure vgx.dll gyorsítótár-példányok foglalkozik.

>[AZURE.NOTE] Prémium gyorsítótár szolgáltatásainak használata és konfigurálása a további tudnivalókért lásd: [adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md), [prémium Azure vgx.dll gyorsítótár fürtözőszoftverét beállításáról](cache-how-to-premium-clustering.md)és [konfigurálásáról prémium Azure vgx.dll gyorsítótár virtuális hálózat támogatása](cache-how-to-premium-vnet.md).

## <a name="configure-redis-cache-settings"></a>Vgx.dll gyorsítótár beállításainak konfigurálása

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure vgx.dll gyorsítótár Ez a témakör a **Beállítások** lap a következő beállításokat.

![Gyorsítótár beállításai vgx.dll](./media/cache-configure/redis-cache-settings.png)



-   [Támogatási és hibaelhárítási beállítások](#support-amp-troubleshooting-settings)
-   [Általános beállítások megadása](#general-settings)
    -   [Tulajdonságok](#properties)
    -   [Hívóbetűk](#access-keys)
    -   [Speciális beállítások](#advanced-settings)
    -   [Gyorsítótár Advisor vgx.dll](#redis-cache-advisor)
-   [Méretarány-beállításai](#scale-settings)
    -   [Réteg árak](#pricing-tier)
    -   [Vgx.dll fürt mérete](#cluster-size)
-   [Adatok kezelése beállításai](#data-management-settings)
    -   [Adatok állandó vgx.dll](#redis-data-persistence)
    -   [Az importálás/exportálás](#importexport)
-   [Közösségfelügyeleti beállítások](#administration-settings)
    -   [Indítsa újra](#reboot)
    -   [Frissítések ütemezése](#schedule-updates)
-   [Diagnosztikai beállítások](#diagnostics-settings)
-   [Hálózati beállítások](#network-settings)
-   [Erőforrás-beállítások kezelése](#resource-management-settings)

## <a name="support--troubleshooting-settings"></a>Támogatási és hibaelhárítási beállítások

A **támogatás + hibaelhárítási** szakasz a beállítások a beállítások a gyorsítótár-val kapcsolatos problémák szükséges.

![Támogatás + – hibaelhárítás](./media/cache-configure/redis-cache-support-troubleshooting.png)

Kattintson a **diagnosztizálása és problémáinak megoldása** kell adni a gyakori problémák és stratégiák a megoldásukat.

Kattintson a **tevékenység naplója** a gyorsítótár végrehajtott műveletek megtekintéséhez. Akkor is használhatja szűrés más erőforrások, ez a nézet kibontásához. Naplókat használata a további tudnivalókért lásd: [események megtekintése és egyéb naplókat](../monitoring-and-diagnostics/insights-debugging-with-events.md) és [erőforrás-kezelő műveletek naplózási](../resource-group-audit.md). Azure vgx.dll gyorsítótár eseményeinek figyelése a további tudnivalókért lásd: a [műveletek és a riasztások](cache-how-to-monitor.md#operations-and-alerts)lehetőséget.

**Erőforrás-állapot** se nézze, amikor az erőforrás, és jelzi, hogy ha a várt módon fut. Többet szeretne tudni az Azure erőforrás állapot szolgáltatás [Azure erőforrás állapot áttekintése](../resource-health/resource-health-overview.md)című témakörben találhat.

>[AZURE.NOTE] Erőforrás állapot jelenleg nem jelentheti a virtuális hálózatban üzemeltetett Azure vgx.dll gyorsítótár-példányok állapotának. További tudnivalókért lásd: [összes gyorsítótár-szolgáltatás működnek a gyorsítótárat a egy VNET szolgáltatója?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)

Kattintson az **új támogatási kérelem** kattintva nyissa meg a gyorsítótár támogatási kérelmet.

## <a name="general-settings"></a>Általános beállítások megadása

Az **Általános** területen beállításai lehetővé teszik megnyitásához és a következő a gyorsítótár beállításainak konfigurálása.

![Általános beállítások megadása](./media/cache-configure/redis-cache-general-settings.png)

-   [Tulajdonságok](#properties)
-   [Hívóbetűk](#access-keys)
-   [Speciális beállítások](#advanced-settings)
-   [Gyorsítótár Advisor vgx.dll](#redis-cache-advisor)

### <a name="properties"></a>Tulajdonságok

Kattintson a **Tulajdonságok** a gyorsítótár, beleértve a gyorsítótár végpont és portokat kapcsolatos információk megtekintése elemre.

![Vgx.dll gyorsítótár tulajdonságai](./media/cache-configure/redis-cache-properties.png)

### <a name="access-keys"></a>Hívóbetűk

Kattintson a **hívóbetűk** megtekintéséhez, vagy a hívóbetűk a gyorsítótár újragenerálása gombra. Billentyűk az ügyfelek, a gyorsítótár csatlakoztatása az állomásnév és a **Tulajdonságok** lap portok használják.

![Gyorsítótár hívóbetűk vgx.dll](./media/cache-configure/redis-cache-manage-keys.png)






### <a name="advanced-settings"></a>Speciális beállítások

Kattintson a **Speciális beállítások** lap a az alábbi beállítások konfigurálása

-   [Access-portok](#access-ports)
-   [Maxmemory-házirend és maxmemory fenntartott](#maxmemory-policy-and-maxmemory-reserved)
-   [Keyspace értesítések (Speciális beállítások)](#keyspace-notifications-advanced-settings)


### <a name="access-ports"></a>Access-portok

-SSL access új gyorsítótárát az alapértelmezés szerint le van tiltva. Ahhoz, hogy az-SSL port, **csak az SSL keresztül hozzáférés engedélyezése** a **Speciális beállítások lap** a **nem** gombra, és kattintson a **Mentés**gombra.

![Vgx.dll gyorsítótár Access-portok](./media/cache-configure/redis-cache-access-ports.png)

### <a name="maxmemory-policy-and-maxmemory-reserved"></a>Maxmemory-házirend és maxmemory fenntartott

A **Speciális beállítások** lap **Maxmemory házirend** és **maxmemory fenntartott** beállítást állítsa be a memória házirendek a gyorsítótár. A **maxmemory** beállítást konfigurálja a gyorsítótár eviction házirendjét, és **maxmemory fenntartott** beállítja a nem gyorsítótár folyamatok foglalt memóriát.

![Gyorsítótár Maxmemory házirend vgx.dll](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory házirend** lehetővé teszi, a következő eviction házirendek közül választhat.

-   ideiglenes-lru – Ez az alapértelmezett érték.
-   allkeys-lru
-   ideiglenes véletlen
-   allkeys véletlen
-   a ttl ideiglenes
-   noeviction

Maxmemory házirendek kapcsolatos további tudnivalókért olvassa el a [Eviction házirendek](http://redis.io/topics/lru-cache#eviction-policies)című témakört.

A **maxmemory fenntartott** beállítás memória nem gyorsítótár műveletek, például a replikáció feladatátvételkor fenntartott MB állítja be. Azt is használható a magas töredezettség megtartásával esetén. Állítsa ezt az értéket lehetővé teszi, hogy a vgx.dll kiszolgáló egységesebb, amikor a betöltés változik. Ez az érték magasabb-munkaterhelésekből, amely nehéz vannak írja be kell állítani. Ezeket a műveleteket fenntartott memória esetén nem érhető el a gyorsítótárban lévő adatok tárolásának.

>[AZURE.IMPORTANT] A **maxmemory fenntartott** beállítás csak akkor érhető el, szabványos, és prémium gyorsítótárát.

### <a name="keyspace-notifications-advanced-settings"></a>Keyspace értesítések (Speciális beállítások)

A **Speciális beállítások** lap értesítések konfigurálása keyspace vgx.dll. Keyspace értesítések engedélyezi az ügyfél, ha értesítést szeretne kapni az egyes események végrehajtásakor.

![Speciális beállítások gyorsítótár vgx.dll](./media/cache-configure/redis-cache-advanced-settings.png)

>[AZURE.IMPORTANT] Keyspace értesítések és az **értesítés keyspace-események** beállítás csak akkor érhetők szabványos, és prémium gyorsítótárát.

További információért [Vgx.dll Keyspace értesítés](http://redis.io/topics/notifications)jelenik meg. A minta kód a [Helló, világ](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) minta [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) fájl megtekintéséhez.

<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Gyorsítótár Advisor vgx.dll

A **javaslatok** lap javaslatok a gyorsítótár jeleníti meg. Normál működés közben nem javaslatok jelennek meg. 

![Javaslatok](./media/cache-configure/redis-cache-no-recommendations.png)

Ha bármelyik feltétel fordul elő, a műveletek, például magas memóriahasználat, a hálózati sávszélesség vagy a kiszolgáló betöltése a gyorsítótár során, értesítés jelenik meg a **Gyorsítótár vgx.dll** lap.

![Javaslatok](./media/cache-configure/redis-cache-recommendations-alert.png)

További információ a **javaslatok** lap található.

![Javaslatok](./media/cache-configure/redis-cache-recommendations.png)

Ezek a **Gyorsítótár vgx.dll** lap szakaszait [Figyelés diagramok](cache-how-to-monitor.md#monitoring-charts) és [diagramok használatát](cache-how-to-monitor.md#usage-charts) a mértékek figyelheti meg.

Minden egyes árak réteg ügyfélkapcsolatok, a memória és a sávszélesség-különböző korlátozások tartalmaz. Ha a gyorsítótár tartós időszakra vetített állapotával megközelíti a maximális kapacitása ezeket a mértékek, ajánlást jön létre. Mértékek és a **javaslatok** eszköz szab korlátai kapcsolatos további tudnivalókért lásd: az alábbi táblázatban.

| Gyorsítótár metrikus vgx.dll      | További információ                                                  |
|-------------------------|---------------------------------------------------------------------------|
| Hálózati sávszélesség-használat | [Gyorsítótár teljesítmény - sávszélességre](cache-faq.md#cache-performance) |
| Kapcsolódó ügyfelek       | [Alapértelmezett vgx.dll kiszolgáló konfigurálása - maxclients](#maxclients)            |
| Kiszolgáló betöltése             | [Használatát diagramok - kiszolgáló terhelését vgx.dll](cache-how-to-monitor.md#usage-charts)  |
| Memóriahasználat            | [Gyorsítótár teljesítmény - méret](cache-faq.md#cache-performance)                |

A gyorsítótár frissítéséhez kattintson a **Frissítés most** módosítsa a [réteg árak](#pricing-tier) , és a gyorsítótár méretezni. Egy árak réteg kiválaszthatja a további tudnivalókért lásd: [milyen vgx.dll gyorsítótár felkínálása és a méret érdemes használnom?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-settings"></a>Méretarány-beállításai

A **Méretezés** szakasz a beállítások eléréséhez és a következő a gyorsítótár beállításainak konfigurálása teszi lehetővé.

![Hálózati](./media/cache-configure/redis-cache-scale.png)

-   [Réteg árak](#pricing-tier)
-   [Vgx.dll fürt mérete](#cluster-size)

### <a name="pricing-tier"></a>Réteg árak

Kattintson a **árak réteg** megtekintése vagy módosítása a árak réteg a gyorsítótár. További tájékoztatást a méretezés tájékozódhat [skála Azure vgx.dll gyorsítótár](cache-how-to-scale.md).

![Réteg árak gyorsítótár vgx.dll](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>
### <a name="redis-cluster-size"></a>Vgx.dll fürt mérete

Kattintson a **(Előzetes verzió) vgx.dll fürt méretét** az futó fürt méretének módosítása prémium gyorsítótárat fürtözés engedélyezve van.

>[AZURE.NOTE] Ne feledje, hogy az Azure-gyorsítótár prémium vgx.dll szint megjelenik az általános elérhetővé, miközben a vgx.dll fürt méret funkció jelenleg előzetes verzióban.

![Vgx.dll fürt mérete](./media/cache-configure/redis-cache-redis-cluster-size.png)

A fürt méretének módosításához a csúszkával vagy a **Shard száma** mezőben írja be egy 1 és 10 közé számot, és mentéséhez kattintson **az OK** gombra.

>[AZURE.IMPORTANT] Vgx.dll fürtözés lehetőség csak a prémium gyorsítótárát. További információért megtudhatja, [hogy miként fürtözőszoftverét prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-clustering.md).


## <a name="data-management-settings"></a>Adatok kezelése beállításai

Az **adatok kezelése** szakaszban a megadott beállítások eléréséhez és a következő a gyorsítótár beállításainak konfigurálása teszi lehetővé.

![Adatok kezelése](./media/cache-configure/redis-cache-data-management.png)

-   [Adatok állandó vgx.dll](#redis-data-persistence)
-   [Az importálás/exportálás](#importexport)

### <a name="redis-data-persistence"></a>Adatok állandó vgx.dll

Kattintson az **adatok adatmegőrzési vgx.dll** engedélyezése, letiltása és adatok megőrzése a prémium gyorsítótár beállítása.

![Adatok állandó vgx.dll](./media/cache-configure/redis-cache-persistence-settings.png)

Ahhoz, hogy az adatmegőrzési vgx.dll, válassza a **engedélyezett** ahhoz, hogy Rekordadatbázis (vgx.dll adatbázis) biztonsági mentése lehetőséget. Adatmegőrzési vgx.dll kikapcsolásához válassza a **Letiltva**lehetőséget.

Adja meg a biztonsági másolat időközt, jelölje be a **Biztonsági mentés gyakoriságának** a legördülő listából. Választási lehetőségek **15 percet**, **30 percig**, **60 perc**, **6 órával**, **12 órás**és **24 óra**tartalmazzák. Az intervallum elindul számlálás az előző biztonsági művelet sikeresen befejeződött, és a program, amikor egy új biztonsági másolatot kezdeményezett után.

Kattintson a **Tárterület-fiók** , jelölje be a tárterület-fiók használata, és jelölje ki az **elsődleges kulcs** vagy **másodlagos kulcsa** a **Tárhely kulcs** legördülő használja. Ugyanabban a régióban, mint a gyorsítótár kell választania egy tárterület-fiókot, és egy **Prémium tárterület** -fiókkal ajánlott, mert magasabb átviteli prémium tároló. A tárhely billentyűt az adatmegőrzési fiók jön, bármikor újra kell választania a kívánt kulcsot a **Tárhely kulcs** legördülő.

Kattintson **az OK** gombra az adatmegőrzési konfiguráció mentéséhez.

>[AZURE.IMPORTANT] Adatok adatmegőrzési vgx.dll prémium gyorsítótárát csak érhető el. További információért megtudhatja, [hogy miként adatmegőrzési prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-persistence.md).

### <a name="importexport"></a>Az importálás/exportálás

Az importálás/exportálás egy Azure vgx.dll gyorsítótár adatok kezelése művelet, amely lehetővé teszi az adatok importálása az Azure vgx.dll gyorsítótár vagy adatok exportálása az Azure vgx.dll gyorsítótárból importálása és exportálása vgx.dll gyorsítótár-adatbázis (Rekordadatbázis) pillanatkép prémium gyorsítótárból az Azure tárterület-fiókokban található lap blob. Lehetővé teszi, hogy a különböző Azure vgx.dll gyorsítótár-példányok közötti áttelepítéséhez, vagy a gyorsítótár használat előtt adatokkal.

Importálás a bármely vgx.dll bármelyik felhő vagy környezetre, beleértve a vgx.dll Linux rendszerhez, a Windows vagy bármely felhő például Amazon webszolgáltatásokhoz és mások futó operációs rendszert futtató kiszolgáló kompatibilis Rekordadatbázis fájlokat vgx.dll tárolt használható. Adatok importálása az ugyanígy gyorsítótár létre előre megadott adatokkal. Az importálási folyamat során Azure vgx.dll gyorsítótár Rekordadatbázis fájlok betölti Azure tárhelyről a memóriában, és a billentyűk majd beillesztése a gyorsítótár.

Exportálás az Azure-vgx.dll vgx.dll kompatibilis Rekordadatbázis fájlokat a gyorsítótárban tárolt adatokat exportálni teszi lehetővé. Ez a funkció használatával adatok áthelyezése egyik Azure vgx.dll gyorsítótár példányából, vagy egy másik vgx.dll kiszolgálóra. Az ideiglenes fájlok jön létre a virtuális exportálási folyamat során, hogy tárolja az Azure vgx.dll gyorsítótár server-példányt, és a fájlt töltenek fel a kijelölt tárterület-fiókjába. Az exportálási művelet befejeztével vagy állapotban sikeres vagy sikertelen az ideiglenes fájlok törlődik.

>[AZURE.IMPORTANT] Az importálás/exportálás prémium réteg gyorsítótárát csak érhető el. További információt és útmutatást olvassa el a [Azure vgx.dll gyorsítótárban lévő adatok importálása és exportálása](cache-how-to-import-export-data.md)című témakört.


## <a name="administration-settings"></a>Közösségfelügyeleti beállítások

A beállítások **felügyelete** szakaszában hajtsa végre a következő rendszergazdai feladatokat a prémium gyorsítótár teszi lehetővé. 

![Felügyelete](./media/cache-configure/redis-cache-administration.png)

-   [Indítsa újra](#reboot)
-   [Frissítések ütemezése](#schedule-updates)

>[AZURE.IMPORTANT] Ebben a szakaszban a beállítások csak érhetők el prémium réteg gyorsítótárát.

### <a name="reboot"></a>Indítsa újra

A **Indítsa újra** a lap lehetővé teszi, hogy indítsa újra a gyorsítótár egy vagy több csomópontot. Lehetővé teszi az alkalmazás tűrőképessége hiba esetén próbálhatja ki.

![Indítsa újra](./media/cache-configure/redis-cache-reboot.png)

Fürtözés engedélyezve van a prémium gyorsítótárat Ha, mely shards indítani a gyorsítótár választhat.

![Indítsa újra](./media/cache-configure/redis-cache-reboot-cluster.png)

Indítsa újra az egy vagy több csomópontok a gyorsítótár, válassza ki a kívánt csomópontokat, és kattintson az **Újraindítás**gombra. Fürtözés engedélyezve van a prémium gyorsítótárat Ha, indítsa újra, és kattintson a **Indítsa újra**a shard(s) kijelölése Néhány perc múlva a kijelölt csomópont indítsa újra a rendszert, és olyan biztonsági online néhány perccel később.

>[AZURE.IMPORTANT] Indítsa újra a rendszert a prémium réteg gyorsítótárát csak érhető el. További információt és útmutatást talál az [Azure vgx.dll gyorsítótár felügyelete – indítsa újra](cache-administration.md#reboot).

### <a name="schedule-updates"></a>Frissítések ütemezése

A **frissítések ütemezése** lap lehetővé teszi, hogy a gyorsítótár server-frissítései vgx.dll karbantartási az ablak. 

>[AZURE.IMPORTANT] Ne feledje, hogy a Karbantartás ablakban csak az vgx.dll kiszolgáló frissítéseket, és nem minden Azure frissítések, vagy frissíti a VMs, amely a gyorsítótár szolgáltató operációs rendszer.

![Frissítések ütemezése](./media/cache-configure/redis-schedule-updates.png)

Karbantartási ablak megadásához jelölje be a kívánt nap, és adja meg a karbantartás ablak kezdő óra minden nap, és kattintson az **OK gombra**. Ne feledje, hogy a karbantartás ablak idő UTC szerint megadva. 

>[AZURE.IMPORTANT] Frissítések ütemezése prémium réteg gyorsítótárát csak érhető el. További információt és útmutatást olvassa el a [Azure vgx.dll gyorsítótár administration - frissítések ütemezése](cache-administration.md#schedule-updates)című témakört.

## <a name="diagnostics-settings"></a>Diagnosztikai beállítások

A **diagnosztikai** szakasz diagnosztika beállítása a vgx.dll gyorsítótár teszi lehetővé.

![Diagnosztikai](./media/cache-configure/redis-cache-diagnostics.png)

Kattintson a **Diagnosztika lehetőségre** , [a tárterület-fiókok konfigurálása](cache-how-to-monitor.md#enable-cache-diagnostics) gyorsítótár diagnosztika tárolja.

![Gyorsítótár diagnosztika vgx.dll](./media/cache-configure/redis-cache-diagnostics-settings.png)

Kattintson a **Mértékek vgx.dll** [Mértékek megtekintése](cache-how-to-monitor.md#how-to-view-metrics-and-customize-charts) a gyorsítótár és a **riasztási szabályok** [riasztási szabályokat állíthat be](cache-how-to-monitor.md#operations-and-alerts).

További információt a Azure vgx.dll gyorsítótár diagnosztika megtudhatja, [hogy miként figyelheti az Azure vgx.dll gyorsítótár](cache-how-to-monitor.md).


## <a name="network-settings"></a>Hálózati beállítások

A **hálózat** csoportban a beállítások eléréséhez és a következő a gyorsítótár beállításainak konfigurálása teszi lehetővé.

![Hálózati](./media/cache-configure/redis-cache-network.png)

>[AZURE.IMPORTANT] Virtuális hálózati beállítások csak érhetők el prémium gyorsítótárát fiókoknál VNET támogatásával gyorsítótár létrehozása során. További információkat a prémium gyorsítótárat a VNET támogatja, és annak beállítások frissítése, megtudhatja, [hogy miként virtuális hálózat támogatása prémium Azure vgx.dll gyorsítótár beállítása](cache-how-to-premium-vnet.md).

## <a name="resource-management-settings"></a>Erőforrás-beállítások kezelése

![Erőforrás-kezelés](./media/cache-configure/redis-cache-resource-management.png)

A **címkék** csoport segítséget nyújt az erőforrások rendszerezheti. További tudnivalókért lásd: [az Azure erőforrások rendszerezéséhez használata címkék](../resource-group-using-tags.md).

A **Zárolás** szakasz zárolása egy előfizetés, erőforráscsoport vagy erőforrás meg, hogy más felhasználók a szervezet véletlen törlését vagy kritikus erőforrások módosítása teszi lehetővé. További információért témakörökben [zárolása az Azure erőforrás-kezelő](../resource-group-lock-resources.md).

A **felhasználók** szakaszban támogatást nyújt az Azure Portal segítségével a szervezetek felel meg az access követelményeknek, egyszerűen és pontosan a szerepköralapú hozzáférés-szerepalapú. További tudnivalókért lásd: [szerepköralapú hozzáférés-vezérlés az Azure-portálra](../active-directory/role-based-access-control-configure.md).

Jelölje be a **sablon exportálása** létre, és a későbbi telepítéshez telepített erőforrásait sablon exportálása. További tudnivalókat a sablonok témakörökben [Deploy Azure erőforrás-kezelő sablonokkal](../resource-group-template-deploy.md).

## <a name="default-redis-server-configuration"></a>Alapértelmezett vgx.dll kiszolgáló beállítása

Új Azure vgx.dll gyorsítótár-példányok van beállítva az alábbi alapértelmezett vgx.dll konfigurációs értékeket.

>[AZURE.NOTE] Ebben a szakaszban a beállítások használatával nem lehet megváltoztatni a `StackExchange.Redis.IServer.ConfigSet` módot. Ha ez a módszer egy ebben a szakaszban a parancsok neve, az alábbihoz hasonló van kivétel:  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>Bármely konfigurálható, például a **max-memória-házirend**-értékek konfigurálható az Azure portálon vagy a parancssor eszközök például Azure CLI vagy PowerShell keresztül.

|A beállítás|Alapérték|Leírás|
|---|---|---|
|adatbázisok|16|Az alapértelmezett szám az adatbázisok 16, de beállíthatja, hogy egy másik számot a árak réteg alapján. <sup>1</sup> az alapértelmezett adatbázis DB 0, választhat egy másik nevet a kapcsolat alap használatához `connection.GetDatabase(dbid)` dbid esetén közé eső szám `0` és `databases - 1`.|
|maxclients|Függ, hogy a árak réteg<sup>2</sup>|Az engedélyezett egyszerre kapcsolódó ügyfelek maximális száma. A korlát elérésekor vgx.dll fog zárja be az új kapcsolatokat küldése "ügyfelek számára elérhető maximális száma" hiba történt.|
|maxmemory-házirend|ideiglenes-lru|Hogyan vgx.dll választja ki mit maxmemory (a a kijelölt a gyorsítótár létrehozásakor kínáló gyorsítótár méretét) elérésekor törlendő Maxmemory házirendet a beállítás nem. Azure vgx.dll gyorsítótár az alapértelmezett beállítás a billentyűk eltávolítja az elévülési LRU algoritmus használatával az ideiglenes-lru. Ez a beállítás az Azure-portálon beállíthatók. További tudnivalókért lásd: [Maxmemory-házirend és maxmemory fenntartott](#maxmemory-policy-and-maxmemory-reserved).|
|a minták maxmemory|3|LRU és minimális TTL (élettartam) algoritmusok nem pontos algoritmusok, de hatványsorral algoritmusok (annak érdekében, hogy mentse a memória), így kijelölhet, valamint a minta mérete ellenőrzéséhez. Az alapértelmezett példány vgx.dll fog jelölje be a három kulcsok és válasszon a lehetőségek, amely kisebb utoljára használt.|
|LUA határidő|5000|Max-végrehajtási idő ezredmásodpercben Lua parancsfájl. A végrehajtás maximális időt elérésekor vgx.dll naplózza visszaállítása parancsfájl végrehajtása után a legnagyobb idő engedélyezett és a program ekkor elkezdi hibával lekérdezések megválaszolása.|
|LUA-esemény-korlát|500|Ez a parancsprogram esemény várólista maximális méretét.|
|ügyfél-kimeneti-pufferelési-korlát normalclient-kimeneti-pufferelési-korlát pubsub|0 0 032mb 8mb 60|Az ügyfél kimeneti pufferelési korlátai kényszeríthet ki, hogy a program nem adatok beolvasása a kiszolgálóról kellő valamilyen okból (gyakori oka az, hogy nem lehet Pub/Sub ügyfél felhasználni az üzeneteket, amilyen gyorsan lehet a publisher kereshetőségét) ügyfelek leválasztó használható. További tudnivalókért olvassa el a [http://redis.io/topics/clients](http://redis.io/topics/clients)című témakört.|

<a name="databases"></a>
<sup>1</sup> A korlátját `databases` eltérő minden Azure vgx.dll gyorsítótár réteg árak és állítja a gyorsítótár létrehozását. Ha nincs `databases` beállítás van megadva gyorsítótár létrehozása során az alapértelmezett érték 16.

-   Egyszerű és a szokásos gyorsítótárát
    -   C0 (250 MB) gyorsítótár - legfeljebb 16 adatbázisok
    -   C1 (1 GB) gyorsítótár - legfeljebb 16 adatbázisok
    -   A C2 (2,5 GB) gyorsítótár - legfeljebb 16 adatbázisok
    -   C3 (6 GB) gyorsítótár - legfeljebb 16 adatbázisok
    -   C4 (13 GB) gyorsítótár - 32 adatbázisok felfelé
    -   C5 (26 GB) gyorsítótár - 48 adatbázisok felfelé
    -   C6 (53 GB) gyorsítótár - legfeljebb 64 adatbázisok
-   Prémium gyorsítótárát
    -   P1 (6 GB - 60 GB) – legfeljebb 16 adatbázisok
    -   P2 (13 GB - 130 GB) – 32 adatbázisok felfelé
    -   P3 (26 GB - 260 GB) – 48 adatbázisok felfelé
    -   P4 (53 GB - 530 GB) – legfeljebb 64 adatbázisok
    -   Az összes prémium gyorsítótárát vgx.dll fürthöz engedélyezve van - vgx.dll fürt csak támogatja a 0-adatbázis használata tehát a `databases` korlátozni bármely prémium gyorsítótár engedélyezett vgx.dll fürthöz lényegében 1 és a [Select](http://redis.io/commands/select) parancs nem engedélyezett. További tudnivalókért lásd: [kell bármilyen módosítást végez az ügyfélalkalmazás fürtözést használ?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)


>[AZURE.NOTE] A `databases` lehet, hogy a beállítás konfigurált csak a gyorsítótár létrehozása során, és csak az PowerShell, CLI vagy más management ügyfelek. Példa a `databases` PowerShell használatával gyorsítótár létrehozása során lásd: a [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).


<a name="maxclients"></a>
<sup>2</sup> `maxclients` minden Azure vgx.dll gyorsítótár réteg árak eltérő.

-   Egyszerű és a szokásos gyorsítótárát
    -   C0 (250 MB) gyorsítótár - legfeljebb 256 kapcsolatok
    -   C1 (1 GB) gyorsítótár - legfeljebb 1000 kapcsolatok
    -   A C2 (2,5 GB) gyorsítótár - legfeljebb 2000 kapcsolatok
    -   C3 (6 GB) gyorsítótár - legfeljebb 5 000 kapcsolatok
    -   C4 (13 GB) gyorsítótár - legfeljebb 10 000 kapcsolatok
    -   C5 (26 GB) gyorsítótár - 15 000 kapcsolatok felfelé
    -   C6 (53 GB) gyorsítótár - legfeljebb 20 000 kapcsolatok
-   Prémium gyorsítótárát
    -   P1 (6 GB - 60 GB) – 7,500 kapcsolatok felfelé
    -   P2 (13 GB - 130 GB) – 15 000 kapcsolatok felfelé
    -   P3 (26 GB - 260 GB) – legfeljebb 30 000 kapcsolatok
    -   P4 (53 GB - 530 GB) – legfeljebb 40 000 kapcsolatok

## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Az Azure vgx.dll gyorsítótár nem támogatja a parancsok vgx.dll

>[AZURE.IMPORTANT] Mivel a konfigurálása és Azure vgx.dll gyorsítótár példányainak kezelése a Microsoft kezeli az alábbi parancsok le vannak tiltva. Ha megpróbál meghívja őket hasonló hibaüzenetet kap `"(error) ERR unknown command"`.
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  BEÁLLÍTÁSOK
>-  HIBAKERESÉSI
>-  ÁTTELEPÍTÉSE
>-  MENTÉS
>-  LEÁLLÍTÁS
>-  SLAVEOF
>-  FÜRT – fürt írási parancsai le vannak tiltva, de csak olvasható fürt parancsok engedélyezettek.

Parancsok vgx.dll kapcsolatos további tudnivalókért olvassa el a [http://redis.io/commands](http://redis.io/commands)című témakört.

## <a name="redis-console"></a>Konzol vgx.dll

Parancsok biztonságosan kiadását az Azure vgx.dll gyorsítótár-példányok használata a **Vgx.dll konzol**, amely normál érhető el, és prémium gyorsítótárát.

>[AZURE.IMPORTANT] A vgx.dll konzol VNET, fürtözés, nem működik, és más, mint 0 adatbázisok. 
>
>-  [VNET](cache-how-to-premium-vnet.md) – Ha a gyorsítótár egy VNET része, csak a VNET-ügyfélprogramok hozzáférhetnek a gyorsítótár. Mivel a vgx.dll konzol a vgx.dll cli.exe ügyfél, amelyek nem szerepelnek a VNET VMs is használ, nem tud kapcsolódni a gyorsítótár.
>-  [Fürtképzés](cache-how-to-premium-clustering.md) – a vgx.dll konzol a vgx.dll cli.exe ügyfélnek, amely nem támogatja az adott időben fürtözés használja. A vgx.dll cli segédprogram a [stabil](http://redis.io/download) ág a vgx.dll tárat a GitHub, amikor lépések egyszerű támogatási hajtja végre a `-c` válthat. További információt [a fürthöz egyenlő](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) lássanak [http://redis.io](http://redis.io) a [fürt oktatóprogram vgx.dll](http://redis.io/topics/cluster-tutorial).
>-  A vgx.dll konzol adatbázis 0, minden alkalommal, amikor elküld egy parancs új kapcsolatot létesít. Nem használhatja a `SELECT` parancsot, jelölje be a másik adatbázist, mert az adatbázis alaphelyzetbe 0, minden parancs. Vgx.dll parancsokat, például egy másik adatbázis módosításához futtatásával kapcsolatos tudnivalókat talál [hogyan is futtatása vgx.dll parancsok?](cache-faq.md#how-can-i-run-redis-commands)

A vgx.dll konzol használatához kattintson a **Gyorsítótár vgx.dll** lap a **konzolt** .

![Konzol vgx.dll](./media/cache-configure/redis-console-menu.png)

Parancsok szemben a gyorsítótár-példány kibocsátása, egyszerűen írja be a kívánt parancsot a konzolba.

![Konzol vgx.dll](./media/cache-configure/redis-console.png)

Azure vgx.dll gyorsítótár le vannak tiltva vgx.dll parancsok listájában című előző [Vgx.dll parancsok az Azure vgx.dll gyorsítótár nem támogatott](#redis-commands-not-supported-in-azure-redis-cache) . Parancsok vgx.dll kapcsolatos további tudnivalókért olvassa el a [http://redis.io/commands](http://redis.io/commands)című témakört. 

## <a name="move-your-cache-to-a-new-subscription"></a>A gyorsítótár áthelyezése új előfizetéshez

Áthelyezheti a gyorsítótár új előfizetéshez **áthelyezése**gombra kattintva.

![Gyorsítótár vgx.dll áthelyezése](./media/cache-configure/redis-cache-move.png)

Erőforrások egy erőforrás csoportból a másikra és áthelyezése egy előfizetésből között a további tudnivalókért lásd [Új erőforráscsoport vagy-előfizetésre erőforrások áthelyezése](../resource-group-move-resources.md).

## <a name="next-steps"></a>Következő lépések
-   Vgx.dll parancsok használata a további tudnivalókért lásd: [hogyan is futtatása vgx.dll parancsok?](cache-faq.md#how-can-i-run-redis-commands).
