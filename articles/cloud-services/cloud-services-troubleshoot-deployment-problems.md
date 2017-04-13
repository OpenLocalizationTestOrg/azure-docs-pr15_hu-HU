<properties
 pageTitle="Felhőalapú szolgáltatás telepítési problémáinak elhárítása |} Microsoft Azure"
 description="Előfordulhat, hogy problémába egy felhőalapú szolgáltatásba Azure telepítésekor néhány gyakori problémák vannak. Ebben a cikkben néhány oldalt megoldásokat."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Felhőalapú szolgáltatás telepítési problémák elhárítása

Ha egy felhőalapú szolgáltatás alkalmazáscsomag Azure rendszerbe, a telepítési információ szerezhet be a **Tulajdonságok** ablak az Azure-portálon. Segít a felhőbeli szolgáltatástól kapcsolatos problémák elhárítása az ablaktábla a részleteinek is használhatja, és hozzá lehet adni ezt az információt, támogatja az Azure új támogatási kérelem megnyitásakor.

A **Tulajdonságok** ablaktábla megtalálhatja az alábbi képlettel történik:

* Az Azure-portálon kattintson a felhőalapú szolgáltatás üzembe helyezésével, **az összes beállításai**parancsra, és válassza a **Tulajdonságok parancsot**.
* Az Azure klasszikus portálon kattintson a felhőalapú szolgáltatás üzembe helyezésével, majd a **DASHBOARD**(a **fontos**) a lap jobb alsó sarkában található. Ne feledje, hogy nincs "Tulajdonságok" címke van az ablaktábla.

> [AZURE.NOTE] A **Tulajdonságok** ablaktábla tartalmát a vágólapra másolhatja az ablak jobb felső sarkában található ikonra kattintva.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Probléma: nem érhetők el a webhelyemre, de elindul a telepítő, és készen áll példányait szerepkör

A webhely URL-címe hivatkozásra a portálon látható nem része a port. A webhely alapértelmezett portja 80. Ha az alkalmazás futtatásához a különböző port van beállítva, hozzá kell adnia a helyes portszámot URL-CÍMÉT a webhely eléréséhez.

1. Az Azure-portálon kattintson a felhőalapú szolgáltatás üzembe helyezésével.
2. Az Azure portál **Tulajdonságok** ablaktáblájában jelölje be a portokat a szerepkör-példányok (a **Végpontok beviteli**).
3. Ha a port nem 80, adja hozzá a megfelelő port értéket az URL-címet az alkalmazás elérésekor. Alapértelmezettől port megadásához írja be az URL-címet, és egy kettőspontot (:), a portszámot, szóközök nélkül követ.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Probléma: A szerepkör-példányok újrahasznosított én bármi nélkül

Javító szolgáltatás automatikusan megtörténik, amikor Azure csomópontok problémát észlel, és ezért helyezi át szerepkör-példányok új csomópontot. Ez akkor fordulhat elő, amikor a szerepkör-példányok automatikusan újrafelhasználás jelenhet meg. Megtudhatja, ha szolgáltatás javító történt:

1. Az Azure-portálon kattintson a felhőalapú szolgáltatás üzembe helyezésével.
2. A **Tulajdonságok** ablaktáblában az Azure-portálra tekintse át az információkat, és meghatározza, hogy szolgáltatás javító történt, a szerepkörök újrafelhasználás a megfigyelt ideje alatt.

Szerepkörök fog is Lomtár nagyjából egyszer havonta host-OS és a vendégként való bekapcsolódáshoz-OS frissítések során.  
További tudnivalókért lásd: a blogbejegyzésből [Szerepkör példány újraindul esedékes operációs rendszer frissítése](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Probléma: e nem végezze el a virtuális felcserélése és hiba jelenik meg

Egy virtuális felcserélése visszacsatolás nem engedélyezett, ha a telepítés frissítése folyamatban van. A frissítések telepítési automatikusan történik esetén:

* Új vendég operációs rendszer érhető el, és az automatikus frissítések konfigurált.
* A szolgáltatás javító fordul elő.

Annak megállapítása, hogy az automatikus frissítés megakadályozza, hogy Ön egy virtuális felcserélése módon:

1. Az Azure-portálon kattintson a felhőalapú szolgáltatás üzembe helyezésével.
2. A **Tulajdonságok** ablaktáblában az Azure-portálra keresse meg az **állapot**értéket. Ha **készen**áll, akkor ellenőrizze megakadályozhatja, hogy a **legutóbbi művelet** tekintheti meg, ha egy nemrégiben történt, amely a virtuális felcserélése.
3. Az éles üzemi esetében ismételje meg az 1-2.
4. Ha az automatikus frissítés folyamat, várja meg a befejezés előtt végezze el a virtuális felcserélése.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Probléma: Körkörös lejátszás lépések, inicializálás, elfoglalt vagy leállítva között egy szerepkör-példány

Ez a feltétel azt jelzik, az alkalmazás kódot, csomagot vagy konfigurációs fájl probléma. Ebben az esetben kell láthatja az állapot módosítása minden néhány percet, és az Azure portálon is fel, hogy valamit, amit például **újrahasznosítás**, **elfoglalt**vagy **inicializálás**. Ez azt jelzi, hogy van valami probléma azzal, hogy az alkalmazás, a szerepkör-példány van megőrzési futtatását.

A probléma hibák elhárítása a további tudnivalókért tekintse meg az [Azure PaaS kiszámítania diagnosztikai adatok](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) és a [gyakori problémákra, amelyek következtében a Lomtár szerepkörök](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Probléma: Az alkalmazás nem működik

1. Az Azure-portálon kattintson a szerepkör-példányt.
2. Az Azure-portálra a **Tulajdonságok** ablaktáblában vegye figyelembe az alábbi feltételek a probléma megoldásához:
   * Ha a szerepkör-példány nemrégiben leállt (érdemes értékét **darab megszakítani**), a telepítés frissítése sikerült kell. Várjon megjelenítéséhez, ha a szerepkör-példány folytatja a saját maga működését.
   * Ha a szerepkör-példány **elfoglalt**, jelölje be a alkalmazás kód megtekintéséhez, ha a [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) esemény kezeli. Szükség lehet hozzáadni vagy javítani a kód, amely az esemény kezeli.
   * A diagnosztikai adatok és a következő blogbejegyzésben: [Azure PaaS kiszámítania diagnosztika adatok](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)hibaelhárítási esetek folyamatát.

>[AZURE.WARNING] Ha Ön Lomtár a felhőalapú szolgáltatásba, új a telepítéshez, a Tulajdonságok hatékony törlése a probléma az eredeti adatait.

## <a name="next-steps"></a>Következő lépések

További [cikkek hibaelhárítási](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) felhőszolgáltatások megtekintése

Felhőalapú szolgáltatás szerepkör problémák elhárítása az Azure PaaS számítógépes diagnosztika adatok használatával című témakörben talál [Kevin Williamson blogsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
