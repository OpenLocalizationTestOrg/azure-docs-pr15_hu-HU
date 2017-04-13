<properties
   pageTitle="Naplógyűjtés Linux Azure diagnosztika segítségével |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogy miként állíthat be Azure diagnosztikai naplók gyűjt a rendszerű az Azure Service háló Linux fürtre."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Naplógyűjtés Azure diagnosztika segítségével

> [AZURE.SELECTOR]
- [A Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Az Azure Service háló fürt futtat, esetén célszerű az egy központi helyen összes csomópontjának naplógyűjtés. A naplók kellene egy központi helyen egyszerűen elemzése és hibáinak elhárításához, akár a szolgáltatások, az alkalmazás és a fürthöz. Töltse fel és naplógyűjtés egy úgy, hogy az Azure diagnosztika bővítménnyel feltöltések megjelenítése az Azure-tárolóhoz naplók. Olvassa el az események tárhelyről, és helyezze őket egy termék például [Rugalmas keresési](service-fabric-diagnostic-how-to-use-elasticsearch.md) vagy egy másik napló elemzése megoldás.

## <a name="log-sources-that-you-might-want-to-collect"></a>Log források, érdemes lehet összegyűjtése
- **Szolgáltatás háló naplók**: a platformon keresztül [LTTng](http://lttng.org) által kibocsátott és feltölteni a tárterület-fiókjába. Naplók lehet műveleti események és runtime események, amely a platform bocsát ki. Ezeket a naplókat, amely meghatározza a fürt jegyzék helyen tárolja. (A tárhely fiók részleteket a címke **AzureTableWinFabETWQueryable** keresni, és keresse meg a **StoreConnectionString**.)
- **Alkalmazás események**: a szolgáltatáskód által kibocsátott. Szöveges alapú naplófájlok – például LTTng ír naplózás megoldások is használhatja. További információ az LTTng dokumentációjában a nyomkövetés az alkalmazást.  


## <a name="deploy-the-diagnostics-extension"></a>A diagnosztikai bővítmény telepítése
Az első lépés a naplók gyűjteni, hogy az egyes a VMs a szolgáltatás háló fürt a diagnosztika bővítmény telepítése. A diagnosztikai bővítmény gyűjti össze a naplók a minden virtuális, és feltöltések megjelenítése az őket a tárterület-fiókjába, megadott feltételeknek. A lépések hogy használja-e az Azure portálja vagy az erőforrás-kezelő Azure függ.

A VMs a fürt fürt létrehozása részeként a diagnosztika bővítmény telepítéséhez **Diagnosztika** **On**értékre. Miután létrehozta a fürt, ez a beállítás nem módosítható a portál használatával.

Ezután állítsa be Linux Azure diagnosztika (LAD) gyűjthet a fájlokat, és helyezze el őket a tárterület-fiókba. Ez a folyamat magyarázza mint eset 3 ("töltse fel a saját naplófájlok") [Használatával LAD figyelésére és Linux VMs diagnosztizálása](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md)című témakörben. Ezt a folyamatot követve megkapja a halad való hozzáférés. A nyomkövetési naplók is feltölthet egy tetszés szerinti visualizer.

A diagnosztikai bővítmény Azure erőforrás-kezelő használatával is telepítheti. A folyamat Windows és Linux hasonlít, és [hogyan gyűjthetők össze az Azure diagnosztikai naplók](service-fabric-diagnostics-how-to-setup-wad.md)a Windows fürtre vonatkozóan ismertetését.

Tevékenységek kezelése csomagot, [Műveletek Management csomagja napló Analytics Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/)ismertetett módon is használhatja.

Ez a beállítás után a LAD agent figyeli, a megadott naplófájlok. Új sor mellett jelenik meg a fájlt, amikor létrehoz egy syslog bejegyzést a megadott tároló küldött.


## <a name="next-steps"></a>Következő lépések
Részletesebb milyen események kell vizsgálja meg közben kapcsolatos problémák elhárítása című cikkben talál részletes [LTTng dokumentáció](http://lttng.org/docs) és [LAD használatával](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
