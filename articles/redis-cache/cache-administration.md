<properties 
    pageTitle="Azure vgx.dll gyorsítótár felügyelete |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure vgx.dll gyorsítótár újraindítása és az ütemezett frissítést például felügyeleti feladatok végrehajtása"
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
    ms.date="09/27/2016"
    ms.author="sdanie" />

# <a name="how-to-administer-azure-redis-cache"></a>Azure vgx.dll gyorsítótár felügyelete

Ez a témakör ismerteti, hogyan újraindítása és az Azure vgx.dll gyorsítótár-példányok frissítések ütemezése például felügyeleti feladatok elvégzéséhez.

>[AZURE.IMPORTANT] A beállítások és a jelen cikkben ismertetett funkciók prémium réteg gyorsítótárát csak elérhetők.


## <a name="administration-settings"></a>Közösségfelügyeleti beállítások

Azure vgx.dll gyorsítótár **felügyeleti** beállításai hajthat végre a következő rendszergazdai feladatokat a prémium gyorsítótár. Közösségfelügyeleti beállítások eléréséhez kattintson **az összes** vagy **beállításait** a gyorsítótár vgx.dll lap, és görgetéssel keresse **felügyelet** szakaszában kattintson a **Beállítások** lap.

![Felügyelete](./media/cache-administration/redis-cache-administration.png)

-   [Indítsa újra](#reboot)
-   [Frissítések ütemezése](#schedule-updates)

## <a name="reboot"></a>Indítsa újra

A **Indítsa újra** a lap lehetővé teszi, hogy indítsa újra a gyorsítótár egy vagy több csomópontot. Lehetővé teszi az alkalmazás tűrőképessége hiba esetén próbálhatja ki.

![Indítsa újra](./media/cache-administration/redis-cache-reboot.png)

Fürtözés engedélyezve van a prémium gyorsítótárat Ha, mely shards indítani a gyorsítótár választhat.

![Indítsa újra](./media/cache-administration/redis-cache-reboot-cluster.png)

Indítsa újra a gyorsítótár egy vagy több csomópontot, válassza ki a kívánt csomópontokat, majd kattintson a **Indítsa újra**. Fürtözés engedélyezve van a prémium gyorsítótárat Ha, indítsa újra, és kattintson a **Indítsa újra**a shard(s) kijelölése Néhány perc múlva a kijelölt csomópont indítsa újra a rendszert, és olyan biztonsági online néhány perccel később.

Ügyfélalkalmazások gyakorolt, indítsa újra a csomópont függően változik.

-   **Fő** – Ha a fő csomópont újraindítása után, Azure vgx.dll gyorsítótár átvétele replika csomópontot, és elősegíti a minta. A feladatátvételkor, amelyben a kapcsolatok a gyorsítótár meghiúsulhat rövid időköz lehetnek.
-   **Kisegítő** – a kisegítő csomópont újraindításkor általában nem gyorsítótár-ügyfeleknek nincs hatással.
-   **Mind a mesterlapok és a kisegítő** – Ha mindkét gyorsítótár csomópontok vannak indítani, az összes adat elvesznek a gyorsítótár és a gyorsítótár fail: kapcsolatok mindaddig, amíg az elsődleges csomópontok ismét online állapotba kerül. [Adatok állandó](cache-how-to-premium-persistence.md)állította be, ha a legújabb biztonsági mentés esetén a gyorsítótár ismét online állapotba kerül vissza. Ne feledje, hogy minden olyan gyorsítótár írások, hogy mikor történt a legújabb biztonsági mentés után elvesznek.
-   **A prémium gyorsítótár fürtözés engedélyezve van a csomópont** - amikor újraindítja az a csomópont prémium-gyorsítótár fürtözés engedélyezve van a, a működése ugyanaz, mint amikor újraindítja csomópont nem csoportosított-gyorsítótár.


>[AZURE.IMPORTANT] Indítsa újra a rendszert a prémium réteg gyorsítótárát csak érhető el.

## <a name="reboot-faq"></a>Indítsa újra a gyakori kérdések

-   [Melyik csomópont kell indítani az alkalmazás kipróbálása?](#which-node-should-i-reboot-to-test-my-application)
-   [Törölje a jelet az ügyfélkapcsolatok a gyorsítótár is újraindítás?](#can-i-reboot-the-cache-to-clear-client-connections)
-   [Elvesznek a adatok a gyorsítótárból, ha a számítógép újraindítása tenni?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
-   [A PowerShell, CLI vagy más felügyeleti eszközök gyorsítótár is újraindítás?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)
-   [Milyen árak tiers újraindítás funkciók használhatók?](#what-pricing-tiers-can-use-the-reboot-functionality)


### <a name="which-node-should-i-reboot-to-test-my-application"></a>Melyik csomópont kell indítani az alkalmazás kipróbálása?

A hiba ellen, az alkalmazás a gyorsítótár elsődleges csomópontjának tűrőképessége teszteléséhez indítsa újra a **Diaminta** csomópontot. Tesztelje a tűrőképessége alkalmazásának a hiba ellen, a másodlagos csomópontot, indítsa újra a **kisegítő** csomópontot. Ha tesztelni szeretné az összes hiba ellen, az alkalmazás a gyorsítótár tűrőképessége, indítsa újra **mindkét** csomópontot.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Törölje a jelet az ügyfélkapcsolatok a gyorsítótár is újraindítás?

Igen, ha indítsa újra a gyorsítótár minden ügyfélkapcsolatok nincs bejelölve. Ez lehet hasznos, abban az esetben az összes ügyfelet használt kapcsolatok, például egy logikai hiba vagy az ügyfélalkalmazás egy hiba miatt. Minden egyes árak réteg van a másik [ügyfél kapcsolati korlátozások](cache-configure.md#default-redis-server-configuration) a különböző méretű, és ezek a korlátok elérésekor, ha nincs több ügyfélkapcsolatok elfogadott. A gyorsítótár újraindítása biztosítja az összes ügyfelet kapcsolat törléséhez.

>[AZURE.IMPORTANT] Az ügyfélkapcsolatok egy logikai hiba vagy az ügyfél kódban hiba miatt használ, ne feledje, hogy StackExchange.Redis automatikusan újracsatlakozik után a vgx.dll csomópont vissza online állapotban. Ha a mögöttes probléma továbbra is fennáll, az ügyfélkapcsolatok folytatja használhatók fel.

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Elvesznek a adatok a gyorsítótárból, ha a számítógép újraindítása tenni?

Ha a **fő** - és a **kisegítő** csomópontok újraindítja az összes adat a gyorsítótárban lévő (vagy az adott shard prémium gyorsítótárat használata engedélyezve fürtözés) elvész. [Adatok állandó](cache-how-to-premium-persistence.md)állította be, ha a legújabb biztonsági mentés visszaállítja esetén a gyorsítótár ismét online állapotba kerül. Ne feledje, hogy minden olyan gyorsítótár írások után a biztonsági másolat készült előfordult elvesznek.

Ha csak egy csomópontok újraindítja, általában adatvesztés nem történik, de továbbra is lehet, hogy. A példa: Ha újraindítása után a fő csomópontot, és a gyorsítótár írási már folyamatban van, a gyorsítótár írási adatainak elvész. Egy másik példa az adatok elvesztését lenne, ha újraindítja az egyik csomópontra, és a másik csomópont lépjen egy hiba miatt egyszerre történik. Az adatok elvesztését lehetséges oka kapcsolatos további tudnivalókért lásd [Mi történt a vgx.dll az adataimat?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md).

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>A PowerShell, CLI vagy más felügyeleti eszközök gyorsítótár is újraindítás?

Igen, PowerShell tanulmányozza [a vgx.dll gyorsítótár indítsa újra](cache-howto-manage-redis-cache-powershell.md#to-reboot-a-redis-cache).

### <a name="what-pricing-tiers-can-use-the-reboot-functionality"></a>Milyen árak tiers újraindítás funkciók használhatók?

Indítsa újra a rendszert csak a réteg árak támogatás érhető el.

## <a name="schedule-updates"></a>Frissítések ütemezése

A **frissítések ütemezése** lap lehetővé teszi, hogy a gyorsítótár karbantartási az ablak. Ha a Karbantartás ablakban megadva, a minden vgx.dll kiszolgáló frissítése során az ablak történik. Ne feledje, hogy a Karbantartás ablakban csak az vgx.dll kiszolgáló frissítéseket, és nem minden Azure frissítések, vagy frissíti a VMs, amely a gyorsítótár szolgáltató operációs rendszer.

![Frissítések ütemezése](./media/cache-administration/redis-schedule-updates.png)

Karbantartási ablak megadásához jelölje be a kívánt nap, és adja meg a karbantartás ablak kezdő óra minden nap, és kattintson az **OK gombra**. Ne feledje, hogy a karbantartás ablak idő UTC szerint megadva. 

>[AZURE.NOTE] Az alapértelmezett karbantartási ablak frissítések 5 óra. Ez az érték nem konfigurálható az Azure portálról, de a PowerShell használatával beállíthatja a `MaintenanceWindow` paramétere: a [New-AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx) parancsmag. További tudnivalókért lásd: [is lehet felügyelt PowerShell, CLI vagy más felügyeleti eszközök ütemezett frissítést?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Frissítések: gyakori kérdések ütemezése

-   [Amikor tegye frissítések fordulhat elő, ha a frissítési ütemezés szolgáltatás nem használhatom?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
-   [Milyen típusú frissítéseket során az ütemezett karbantartás ablak elvégzett?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
-   [Is lehet a PowerShell, CLI vagy más felügyeleti eszközök felügyelt ütemezett frissítést?](#can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools)
-   [Milyen árak tiers ütemezés frissítések funkciók használhatók?](#what-pricing-tiers-can-use-the-schedule-updates-functionality)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Amikor tegye frissítések fordulhat elő, ha a frissítési ütemezés szolgáltatás nem használhatom?

Ha nem adja meg a karbantartás ablak, frissítések bármikor tehetők.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Milyen típusú frissítéseket során az ütemezett karbantartás ablak elvégzett?

Csak vgx.dll során az ütemezett karbantartás ablak módosításai frissítések kiszolgáló. A Karbantartás ablakban nem vonatkozik az Azure és frissítések a virtuális operációs rendszerben.

### <a name="can-i-managed-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Is lehet a PowerShell, CLI vagy más felügyeleti eszközök felügyelt ütemezett frissítést?

Igen, az ütemezett frissítést, az alábbi PowerShell-parancsmag használatával kezelheti.

-   [Get-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763835.aspx)
-   [Új AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763834.aspx)
-   [Új AzureRmRedisCacheScheduleEntry](https://msdn.microsoft.com/library/azure/mt763833.aspx)
-   [Eltávolítás-AzureRmRedisCachePatchSchedule](https://msdn.microsoft.com/library/azure/mt763837.aspx)

### <a name="what-pricing-tiers-can-use-the-schedule-updates-functionality"></a>Milyen árak tiers ütemezés frissítések funkciók használhatók?

Frissítések ütemezése csak a réteg árak támogatás érhető el.

## <a name="next-steps"></a>Következő lépések

-   Ismerkedjen meg több [Azure vgx.dll gyorsítótár prémium réteg](cache-premium-tier-intro.md) szolgáltatást.





