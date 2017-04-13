<properties
    pageTitle="Azure App szolgáltatásban alkalmazásainak figyelése"
    description="Megtudhatja, hogy miként figyelheti az alkalmazások Azure App szolgáltatásban az Azure portál használatával."
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>Útmutató: Azure App szolgáltatásban alkalmazások figyelése

[Alkalmazás-szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=529714) az [Azure-portálon](https://portal.azure.com)beépített felügyeleti funkciókat biztosít.
Ide tartoznak, az azt jelenti, hogy tekintse át a **kvótáinak** és **mérőszámok** alkalmazás, valamint az alkalmazás szolgáltatáscsomagja, **Figyelmeztetések** és páros **Méretezés** ezeket a mértékek alapján automatikusan beállítani.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Kvóták ismertetése és mérőszámok

### <a name="quotas"></a>Kvóták

Alkalmazások üzemeltetett App szolgáltatásban a források segítségével bizonyos *korlátozások* vonatkoznak. A korlátozások a alkalmazással társított **alkalmazás szolgáltatáscsomagja** határozza meg.

Ha az alkalmazás **ingyenes** , vagy **megosztott** tervben üzemelteti, majd az erőforrások használható az alkalmazás a vonatkozó korlátok által definiált **kvóták**.

Ha az alkalmazás a egy **egyszerű**, **normál** vagy **prémium verzió** csomagot, majd a korlátok fájlkiszolgálón található az erőforrások használhatják a **méret** (kicsi, közepes, nagy) és a **példányok száma** (1, 2, 3,...) a **alkalmazás szolgáltatáscsomagja**állítja be.

**Szabad** - és **a megosztott** alkalmazás **kvóták** a következők:

* **CPU(Short)**
   * Ehhez az alkalmazáshoz a 3 perc időköz engedélyezett Processzor összege. Ez a kvóta újra minden 3 perc állítja be.
* **CPU(Day)**
   * Ehhez az alkalmazáshoz nap engedélyezett Processzor mennyiségét. Ez a kvóta újra a éjfél UTC 24 óránként állítja be.
* **A memória**
   * Ez az alkalmazás számára engedélyezett memória mennyiségét.
* **A sávszélesség**
   * Ehhez az alkalmazáshoz nap engedélyezett kimenő sávszélesség mennyiségét.
   Ez a kvóta újra a éjfél UTC 24 óránként állítja be.
* **Formázáshoz**
   * Engedélyezett tároló mennyiségét.

Az alkalmazások **egyszerű**, **normál** és **prémium** csomag is alkalmazható csak kvóta **formázáshoz**.

További információt az adott kvóták, korlátai és a másik alkalmazás szolgáltatás termékváltozatok elérhető szolgáltatások megtalálható itt: [Azure előfizetési szolgáltatás korlátok](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kvóta kényszerítési

Ha egy alkalmazás, a használatát a meghaladja a **Processzor (rövid)**, **Processzor (nap)**, vagy kattintson az alkalmazás **sávszélesség** -kvóta leáll mindaddig, amíg a kvóta akkor állítja be újra. A megadott időszakban az összes bejövő felkérést egy **HTTP 403**eredményezi.
![][http403]

Az alkalmazás **memória** kvóta túllépik, ha az alkalmazás újraindítása lesz.

Ha a **fájlrendszer** kvóta kimerítve van, majd bármelyik írási művelet sikertelen lesz, ilyenek például naplók írás.

Kvóták növelhető és az alkalmazás eltávolítja a alkalmazás díjcsomagjától frissítésével.

### <a name="metrics"></a>Mértékek

**Mértékek** az alkalmazást, vagy alkalmazás szolgáltatás terv viselkedés információt nyújtanak.

Az **alkalmazás**a rendelkezésre álló mérési módja miatt a következők:

* **Átlagos válaszidő**
   * Az alkalmazás az ms kérések kiszolgálására az átlagos ideje.
* **Átlagos memória-munkakészlet**
   * Az alkalmazás által használt MIB-adatbázisból a memória átlagos összege.
* **Processzor idő**
   * Az alkalmazás által elfogyasztott másodpercben Processzor összege. További információt a metrikus lásd: [Processzor idő viewben Processzor százaléka](#cpu-time-vs-cpu-percentage)
* **Az adatok**
   * Az alkalmazás MIB-adatbázisból a bejövő sávszélességre összege.
* **Adatok meg**
   * Az alkalmazás MIB-adatbázisból a kimenő sávszélességre összege.
* **HTTP 2xx**
   * A HTTP-állapotkód így kérelmek száma > = 200, de < 300.
* **HTTP 3xx**
   * A HTTP-állapotkód így kérelmek száma > = 300, de < 400.
* **HTTP 401**
   * HTTP 401 állapotkódot így kérelmek száma.
* **HTTP 403**
   * HTTP 403 állapotkód így kérelmek száma.
* **HTTP 404**
   * HTTP 404-es állapotkód így kérelmek száma.
* **HTTP 406**
   * HTTP 406 állapotkód így kérelmek száma.
* **HTTP 4xx**
   * A HTTP-állapotkód így kérelmek száma > = 400, de < 500.
* **HTTP-kiszolgáló hibák**
   * A HTTP-állapotkód így kérelmek száma > = 500, de < 600.
* **Memória-Munkakészlet**
   * A MIB-adatbázisból a által használt memória jelenlegi mérete.
* **Kérések**
   * Az eredményül kapott HTTP állapotkód függetlenül kérések száma.

Az **alkalmazás szolgáltatáscsomagja**a rendelkezésre álló mérési módja miatt a következők:

>[AZURE.NOTE] Alkalmazás szolgáltatás terv mértékek csak az **egyszerű**, **normál** és **prémium** Termékváltozat a csomagok érhetők el.

* **Processzor százalékos**
   * A terv minden példányára használt átlagos Processzor.
* **A memória százalékos**
   * Az átlag memóriahasználat a terv minden példányára.
* **Az adatok**
   * A terv minden példányára használt átlagos bejövő sávszélességet.
* **Adatok meg**
   * Az átlag, a terv minden példányára használt sávszélesség kimenő.
* **Lemez hossza**
   * Átlagos száma olvasási és írási váró kérelmek tárolón. Egy nagy lemez várólista hossza olyan alkalmazás, amely miatt túlzott lemeztevékenység víruskeresők is lassíthatják feltüntetése.
* **HTTP várólista hossza**
   * HTTP-kérelmeket, kattintson a sor előtt, hogy teljesülnek ülnie kellett átlagos száma. A magas vagy növekvő HTTP várólista hossza terv nagy terhelés alatt a jelenség.

### <a name="cpu-time-vs-cpu-percentage"></a>Processzor idő viewben Processzor százaléka
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Vannak olyan 2 mértékek processzorhasználata tükröző. **Processzor időt** és a **százalékos Processzor**

**Processzor** ideje is **ingyenes** vagy **megosztott** tervek, mivel azok közül a által használt Processzor perc könyvjelzőnév alkalmazások esetében hasznos lehet.

**Processzor százalékos** azonban akkor lehet hasznos-alkalmazások **egyszerű**, **normál** és **premium** tervekben üzemeltetett azokat meg kell arányosan, és a mérőszám egy jó feltüntetése az általános használatát összes előfordulását keresztül.

##<a name="metrics-granularity-and-retention-policy"></a>Mértékek beállítási lehetőséget és az adatmegőrzési

Az alkalmazás és az alkalmazás szolgáltatáscsomagja mértékek jelentkezve, és a szolgáltatás a következő részletességek és az adatmegőrzési házirendek összesíti:

 * **Perc** Granularitás mértékek megmaradnak **48** óra
 * **Óra** Granularitás mértékek megmaradnak **30** nap
 * **Napi** Granularitás mértékek **90** napig őrződnek meg

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Figyelés kvótáinak és mérőszámok az Azure-portálon.

A különböző **kvóták** és **Mértékek** , az alkalmazások az [Azure-portálon](https://portal.azure.com)érintő állapotának áttekintése

![][quotas]
**Kvóták** megtalálható a beállítások >**kvóták**. A UX lehetővé teszi, hogy a áttekintése: (1) kvóták nevét, (2) a alaphelyzetbe intervallum, (3) a jelenlegi korlát és (4) jelenlegi értékét.

![][metrics]
**Mértékek** lehet közvetlenül az erőforrás lap az access. Testre is szabhatja a diagram által: (1) **kattintson** rá, és a **Szerkesztés diagram**kijelölése (2).
További lehetőségek módosíthatja **időtartomány**(3), (4) **Diagramtípus**és (5) **Mértékek** megjelenítéséhez.  

További információ a mértékek itt talál: [Monitor szolgáltatás mértékek](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Az értesítések és az Automatikus méretezéssel
Mértékek az alkalmazás vagy szolgáltatás alkalmazás csomagjával legfeljebb csatlakoztatott is figyelmezteti, további tudnivalók, akkor olvassa el az [értesítések fogadása](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Az egyszerű, normál vagy magasabb szintű alkalmazás szolgáltatás tervek üzemeltetett App szolgáltatás alkalmazások **Automatikus méretezéssel**támogatja. Ebben a csoportban adhatja állíthatja be az alkalmazás szolgáltatás terv mértékek figyelése és növelheti vagy csökkentheti a további erőforrások kezeléséről, szükség szerint példányok száma szabályokat vagy mentése pénz túlzott rendelkezés esetén az alkalmazás. További tudnivalók az automatikus méretezés itt talál: [hogyan méretarányra](../monitoring-and-diagnostics/insights-how-to-scale.md) és a [Gyakorlati tanácsok az Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md) Itt

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

## <a name="whats-changed"></a>Mi változott
* Módosítása egy segédvonalat a webhelyekre alkalmazás szolgáltatáshoz lásd: [Azure alkalmazás szolgáltatás, és a hatás a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
