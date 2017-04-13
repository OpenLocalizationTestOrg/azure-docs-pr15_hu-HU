<properties
    pageTitle="A fejlesztők Azure köteg szolgáltatás áttekintése |} Microsoft Azure"
    description="Ismerje meg, hogy a köteg szolgáltatás és az API-hoz fejlesztési szempontból funkcióját."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/29/2016"
    ms.author="marsma"/>

# <a name="batch-feature-overview-for-developers"></a>A fejlesztők köteg szolgáltatás áttekintése

Ez a köteg Azure szolgáltatás fő összetevői áttekintése ölelik fel a elsődleges szolgáltatásai és információforrások, amelyek a köteg fejlesztők használatával nagyméretű párhuzamos összeállítása megoldások számítja ki.

E fejleszt elosztott számítási alkalmazás vagy szolgáltatás problémák közvetlen [REST API] [ batch_rest_api] hívásokat, vagy használ a [Köteg SDK](batch-technical-overview.md#batch-development-apis)közül, az erőforrások és a jelen cikkben ismertetett funkciók közül számos kell megadnia.

> [AZURE.TIP] A köteg szolgáltatás magasabb szintű bevezetés [Az Azure köteg alapjai](batch-technical-overview.md)című témakör tartalmaz.

## <a name="batch-service-workflow"></a>Köteg szolgáltatás munkafolyamat

A következő magas szintű munkafolyamat tipikus az alkalmazások és a párhuzamos munkaterhelésekből feldolgozásra a köteg szolgáltatást használó szolgáltatások szinte mindegyikét:

1. Töltse fel a **adatfájlok** , amelyet az [Azure tároló] feldolgozása[ azure_storage] fiókot. Köteg beépített támogatja az Azure Blob-tárolóhoz elérése, és a feladatok töltheti le a fájlok [csomópontok](#compute-node) számítja ki a feladatok futtatásakor.

2. Töltse fel a **alkalmazás fájljait** a feladatok fog futni. Ezek a fájlok bináris vagy parancsfájlok és azok függőségek, és a feladatokat a feladatok hajt végre. A feladatok töltheti le a tárterület-fiókjából ezeket a fájlokat, vagy a köteg [alkalmazáscsomagok](#application-packages) funkció használható az alkalmazás kezelése és telepítése.

3. Hozzon létre egy [készlet](#pool) számítási csomópontot. Egy készlet létrehozásakor adja meg a készlet, azok méretét és az operációs rendszer számítási csomópontok számának. Ha a projekt mindegyik tevékenysége fut, azt hozzá van rendelve a készlet csomópontját egyik végrehajtása.

4. Hozzon létre egy [feladatot](#job). A feladat kezeli a feladatok gyűjteménye. Minden feladat, ahol futtatható az adott Projektfeladatok adott készletbe társítani.

5. [Tevékenységek](#task) hozzáadása a projekthez. Minden tevékenység fut, az alkalmazás vagy parancsfájlt, az adatfájlok a tárterület-fiókjából letöltés feldolgozása feltöltött. Minden tevékenység befejeződött, ahogy azt az eredményt is feltölthet a Azure-tárolóhoz.

6. Feladat haladásuk figyelemmel kíséréséhez, és a tevékenység kimenet beolvashatja Azure tároló.

Az alábbi szakaszokból megtudhatja, hogy ezek és a más erőforrások: a köteget, amelyek lehetővé teszik a elosztott számítási forgatókönyv.

> [AZURE.NOTE] A köteg szolgáltatás használatát egy [Köteg fiók](batch-account-create-portal.md) szükséges. Is, szinte minden megoldás használhatja az [Azure tároló] [ azure_storage] tárhely és a beolvasás számlája. A köteg jelenleg támogatja a csak az **Általános célú** tárolási fiók típusára, az 5 [tárolási fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) az [Azure kapcsolatos tárolási fiókok](../storage/storage-create-storage-account.md)leírt módon.

## <a name="batch-service-resources"></a>Köteg szolgáltatás erőforrások

A következő forrásokhoz – partnerek, kiszámítania csomópontok, készletek, feladatok és tevékenységek – követel meg az összes a köteg szolgáltatást használó megoldások. Mások számára, például a projekt ütemezését és alkalmazáscsomagok, hasznos, de nem kötelező, funkcióját tartalmazza.

- [Fiók](#account)
- [Csomópont kiszámítása](#compute-node)
- [Készlet](#pool)
- [Feladat](#job)

  - [Feladat ütemezése](#scheduled-jobs)

- [Tevékenység](#task)

  - [Indítsa el a tevékenység](#start-task)
  - [Kezelő projektfeladat](#job-manager-task)
  - [Előkészítés és megjelenés projektfeladat](#job-preparation-and-release-tasks)
  - [Több elem példány tevékenység (MPI)](#multi-instance-tasks)
  - [Tevékenységfüggések](#task-dependencies)

- [Alkalmazáscsomagok](#application-packages)

## <a name="account"></a>Fiók

A köteg fiók egy egyedileg meghatározott egység belül a köteg szolgáltatás. Az összes feldolgozás társítva egy köteg fiókot. A köteg szolgáltatással műveletek végrehajtásakor szüksége is a fiók nevét, és a fiók kulcsok közül. Azt is megteheti, hogy [az Azure portálon Azure köteg fiókot létrehozni](batch-account-create-portal.md).

## <a name="compute-node"></a>Csomópont kiszámítása

Számítási csomópont-Azure virtuális gépre (virtuális), amelynek van hozzárendelve egy részét az alkalmazás terhelést feldolgozása. A csomópont méretének meghatározza, hogy hány Processzor magmintákat, a memóriahasználat kapacitás és a helyi rendszer fájlméret, amely a csomópontra kiosztva. A Windows vagy Linux rendszerhez csomópontok készletek Azure Cloud Services vagy a virtuális gépeken futó piactér képek használatával hozhat létre. További információt a következő [készlet](#pool) szakasz nézze meg ezeket a beállításokat.

Bármely végrehajtható vagy parancsfájlt, amely támogatja az operációs rendszer környezete a csomópont csomópontok futtatását is lehetővé teszi. Ide tartoznak a \*.exe, \*.cmd, \*.bat és PowerShell-parancsfájlokat Windows – és bináris, burkolatok és Python Linux a parancsfájlok.

Az összes kiszámítania köteg található csomópontok is tartalmazza:

- Szabványos [mappastruktúra](#files-and-directories) és kapcsolódó [környezeti változók](#environment-settings-for-tasks) az összefoglaló tevékenységek által elérhető.
- A hozzáférés vezérléséhez konfigurált **tűzfal** beállításokat.
- [Távoli hozzáférés](#connecting-to-compute-nodes) Windows (távoli asztali Protocol (RDP)) és a Linux (biztonságos rendszerhéj (SSH)) csomópontot.

## <a name="pool"></a>Készlet

Egy készlet, amelyet az alkalmazás futtat a csomópontok gyűjteménye. A készlet hozhatók létre manuálisan, vagy automatikusan a köteg szolgáltatás megadása elvégzendő munka. Létrehozhat és kezelheti a készletet, amely megfelel az erőforrás az alkalmazás. Egy készlet segítségével csak a köteg fiókot, amelyben létrehozták. A köteg fiók beállíthatja, hogy egynél több készlet.

Azure köteg készletek összeállítása a core számítási Azure platform fölött. Nagy terhelés, alkalmazások telepítése, adatok terjesztési, állapot ellenőrzése és rugalmas kiigazítás erőforráskészlethez tartozik (a[Méretezés](#scaling-compute-resources)) számítási található csomópontok számának lehetőséget biztosítanak.

Minden csomópont hozzáadott erőforráskészlethez tartozik egy egyedi nevet és IP-cím van-e hozzárendelve. A csomópont-készletből eltávolításakor az operációs rendszer vagy a fájlok elvégzett módosítások elvesznek, és nevét és IP-cím kiadott későbbi felhasználás céljából. Egy készlet csomópont elhagyásakor élettartama véget ért.

Amikor létrehoz egy készlet, a következő attribútumok adhatja meg:

- Csomópont **operációs rendszer** és **verziója** kiszámítása

    A csomópontok operációs rendszer a készlet kijelölésekor két lehetőség közül választhat: **Virtuális géphez beállításait** , illetve **Cloud Services beállításait**.

    Konfigurációval **virtuális gép** Linux és a Windows képe számítja ki a a [Microsoft Azure virtuális gépeken futó piactéren]csomópontok[vm_marketplace].
    Egy készlet virtuális géphez konfigurációs csomópontok tartalmazó létrehozásakor meg kell adnia a nem csak méretét csomópontok, de is a **virtuális géphez Képhivatkozás** és a köteg **csomópont ügynök Termékváltozat** telepítve legyen azon a csomópontok. Készlet tulajdonságokból megadásával kapcsolatos további tudnivalókért lásd: a [rendelkezést Linux Azure köteg készletek található csomópontok számítja ki](batch-linux-nodes.md).

    **Cloud Services konfigurálása** nyújt a Windows csomópontok *csak*számítja ki. A Cloud Services konfigurációs készletek elérhető operációs rendszerek az [Azure Vendég OS kiadásairól és SDK kompatibilitási mátrix](../cloud-services/cloud-services-guestos-update-matrix.md)jelennek meg. Egy készlet Cloud Services csomópontok tartalmazó létrehozásakor kell adja meg csak a csomópont méretének és a *Család OS*. Létrehozásakor a Windows készletek csomópontok számítja ki, akkor leggyakrabban Cloud Services.

    - Az *Operációs rendszer család* is határozza meg, hogy mely .NET-verziók telepített az operációs rendszerrel.
    - Cloud Services dolgozó szerepkörében, is megadhat egy *Operációs rendszer* szerint (dolgozó szerepkörök kapcsolatos további tudnivalókért című [értesítés, felhőalapú szolgáltatásokról](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) a [Cloud Services áttekintése](../cloud-services/cloud-services-choose-me.md)).
    - Dolgozó szerepkörök, azt javasoljuk, hogy adja meg, `*` esetében az *Operációs rendszer* , hogy a csomópontok automatikusan frissülnek, és nem szükséges gondoskodjon arról, hogy az újonnan munka nem végleges változatán. A fő használati eset egy bizonyos operációs rendszer verziója kijelölésére szolgáló nem kompatibilitási, amely lehetővé teszi, hogy tesztelés frissíteni kell a verzió engedélyezése előtt elvégzendő korábbi verziókkal való kompatibilitás érdekében. Adatérvényesítési, után az *Operációs rendszer* pedig a naprakésszé tehetők és az új OS kép is lehet telepítve – minden futó feladatok megszakadt és helyezte.

- **A csomópontok mérete**

    **Cloud Services konfigurálása** számítási csomópont méretű [Cloud Services méretben](../cloud-services/cloud-services-sizes-specs.md)jelennek meg. Köteg támogatja kivételével az összes Cloud Services méretű `ExtraSmall`.

    **Virtuális gép konfigurációs** csomópont méretű jelennek meg [az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) és [az Azure virtuális gépeken futó méretben](../virtual-machines/virtual-machines-windows-sizes.md) (Windows) számítja ki. Köteg támogatja kivételével az összes Azure virtuális méretű `STANDARD_A0` és azok prémium adathordozós (`STANDARD_GS`, `STANDARD_DS`, és `STANDARD_DSV2` adatsor).

    A számítási csomópont méretet kiválasztásakor fontolja meg a jellemzői és az alkalmazások futtatják a csomópontok követelményeinek. Szempontok, például hogy többszálas-e az alkalmazást, és azt fogyasztása memória segít a leginkább megfelelő és költséghatékony csomópont méretének határozza meg. Érdemes tipikus, jelölje be a egy csomópont méret feltételezve, hogy egy tevékenység csomóponton egyszerre futtatható. Jó helyen jár, akkor lehet, hogy több tevékenység (és ezért több szolgáltatásalkalmazás-példányok) [párhuzamosan futó](batch-parallel-node-tasks.md) kiszámítania csomópontok projekt végrehajtásakor. Ebben az esetben célszerű közös válassza a nagyobb igény szerint a párhuzamos feladat-végrehajtás igazodik csomópont nagyobb méretű. További információt a [tevékenység ütemezése házirend](#task-scheduling-policy) talál.

    A csomópontok erőforráskészlethez tartozik minden azonos méretű. Ha szándékozik alkalmazások futtatásához a különböző Rendszerkövetelmények és/vagy betöltése szintek, azt javasoljuk, hogy külön készletek használja.

- **Csomópontok számának cél**

    Az üzembe pedig a kívánt számítási csomópontok számának. Ezt nevezzük a *cél* , mert bizonyos esetekben a készlet esetleg nem éri el a kívánt számot a csomópontok. Egy készlet előfordulhat, hogy nem éri el a kívánt számát csomópontok eléri a [core kvóta](batch-quota-limit.md#batch-account-quotas) köteg fiókjának – Ha, vagy ha egy automatikus méretezés képletet, amely a maximális száma (lásd a következő "Méretezés házirend") csomópontok korlátozza a kvótáját alkalmazott.

- **Méretezési házirend**

    Mellett egy statikus számát megadó csomópontok, helyette írhat és az [Automatikus méretezés képlet](#scaling-compute-resources) alkalmazása erőforráskészlethez tartozik. A köteg szolgáltatás rendszeres kiértékeli a képletet, és megadja a különböző készlet, beosztások és megadható paraméterek tevékenység alapján készlet található csomópontok számának.

- **A tevékenységek ütemezési házirend**

    Az [egyes csomópontok max feladatok](batch-parallel-node-tasks.md) beállítás határozza meg a készlet belül minden számítási csomóponton párhuzamosan futtatható tevékenységek maximális száma.

    Az alapértelmezett beállítás, hogy egyszerre több feladat futtat a csomópontot, de van esetekről azt szeretné, hogy egynél több feladat egyidejű végrehajtása a csomóponton előnyös. A [Példa forgatókönyv](batch-parallel-node-tasks.md#example-scenario) tekintheti meg, hogyan előnyeivel egyes csomópontok több tevékenység [egyidejű csomópontjának tevékenységek](batch-parallel-node-tasks.md) cikkben talál.

    Megadhatja, hogy a *Kitöltés típusa* , amely meghatározza, köteg feladatok egyenletesen oldalpárokat erőforráskészlethez tartozik csomópontjait keresztül, vagy csomagok minden csomópont feladatok maximális száma a feladat kiosztása egy másik csomópont előtt.

- **A kapcsolati állapot** számítási csomópontok

    A legtöbb esetben a tevékenységek egymástól függetlenül működik, és nem kell kommunikálni egymással. Vannak azonban olyan néhány alkalmazást, amelyben feladatok tájékoztatnia, például [MPI esetek](batch-mpi.md).

    Beállíthatja, hogy az informatikai –**szártag kommunikációs**található csomópontok közötti kommunikációhoz erőforráskészlethez tartozik. Ha engedélyezve van a szártag kommunikációs, Cloud Services konfigurációs készletek található csomópontok portokra 1 100-nál nagyobb a többi tudnak kommunikálni, és virtuális géphez konfigurációs készletek nem korlátozza a forgalmat a minden olyan portot.

    Ne feledje, hogy szártag kommunikáció engedélyezése is hatással van a fürt található csomópontok helyzetének telepítési korlátozások miatt korlátozhatja a erőforráskészlethez tartozik található csomópontok maximális száma. Ha az alkalmazás nem igényel csomópontok közötti kommunikáció, a köteg szolgáltatást is lefoglalhat esetleg sok csomópontok készletébe a számos különböző fürt, és adatközpontokkal ahhoz, hogy nagyobb párhuzamos feldolgozási power.

- A számítási csomópontok **feladat indítása**

    *Indítsa el a tevékenység* választható minden csomóponton végrehajtja a csomópontra a készlet csatlakozik, és minden alkalommal, amikor egy csomópont újraindul vagy reimaged. A kezdő tevékenység esetén különösen hasznos számítási csomópontok előkészítése feladatok, például a számítási csomópontok a feladatok futó alkalmazások telepítése végrehajtását.

- **Alkalmazáscsomagok**

    Megadhatja az [alkalmazáscsomagok](#application-packages) a számítási csomópontok pedig a telepítéshez használni. Alkalmazáscsomagok adja meg, egyszerűbb telepítés és a verziószámozás, a feladatok futó alkalmazást. Minden csomópontot, amely az adott készletre csatlakozik, akkor adja meg a készlet alkalmazáscsomagok telepítve van, és minden alkalommal a csomópont indítani, vagy reimaged. Alkalmazáscsomagok által jelenleg nem támogatott Linux számítási csomóponton.

- **Hálózati konfigurációja**

    Adhatja meg a készlet számítja ki csomópontok Azure [virtuális hálózati (VNet)](../virtual-network/virtual-networks-overview.md) azonosítója létre kell hozni. Egy VNet tartalmazó, a program a készlet [hozzáadása egy készlet-fiókjába] a követelményei[ vnet] a köteg REST API-hivatkozás.

> [AZURE.IMPORTANT] Minden köteg fiók van egy alapértelmezett **Kvóta** , amely az korlátozza **magmintákat** (és ezért számítási csomópontok) egy köteg fiókban. Az alapértelmezett kvótáinak és útmutatást a [kvóta növelése](batch-quota-limit.md#increase-a-quota) (például a köteg fiókjában magmintákat maximális száma) a [kvótáinak és -korlátozások az Azure köteg szolgáltatás](batch-quota-limit.md)található. Ha már útvesztőjében kérő "Miért nem a készlet elérje a több mint csomópontok X?" a fő kvóta is okozhatja.

## <a name="job"></a>Feladat

A projekt a tevékenységek gyűjteménye. Hogyan kiszámítása végzi a készletben számítási csomóponton feladatainak kezeli.

- A feladat adja meg a **készlet** , amelyben a munka van fog futni. Minden feladat új készlet létrehozása, vagy egy készlet sok feladat használata. Egy készlet minden feladat munkaütemezés társított, illetve munkaütemezés társított összes feladatot hozhat létre.

- A választható **feladat prioritási**adhatja meg. A feladat, amely jelenleg folyamatban lévő feladatok-nál magasabb prioritású leadásakor a várakozási sorban található, akik előre a tevékenységek a kisebb prioritású feladatokhoz illeszteni a a magasabb prioritású projektre vonatkozóan a feladatokat. Feladatok az alsó fontos feladatok, amelyeken már nem előnyt vannak.

- Feladat **kényszerek** adja meg, bizonyos korlátozások a feladatokra használhatók:

    Beállíthatja, hogy **wallclock maximális időt**, hogy a feladat fut, hosszabb, mint a maximális wallclock idő megadott érték, ha a feladatot, és a tevékenység befejeződött.

    Köteg is észleli, és próbálkozzon újra a sikertelen feladatok. Kényszer, például hogy egy tevékenység *mindig* vagy *Soha ne* az újból megadható a **tevékenység újrapróbálkozások maximális száma** . Tevékenység újra próbálkozik, az azt jelenti, hogy a tevékenység helyezte-e, hogy újra kell futtatni.

- Az ügyfélalkalmazás tevékenységek hozzáadása a feladatot, vagy megadhatja a [kezelő projektfeladat](#job-manager-task). Kezelő projektfeladat, amely a feladat manager feladathoz pedig számítási csomópontját egyik futtatott a szükséges feladatokat, feladat létrehozásához szükséges információkat tartalmazza. A kezelő projektfeladat kezeli köteg – által kifejezetten azt az aszinkron, amint a feladat jön létre, és ha sikertelen újraindítása után. Kezelő projektfeladat *szükség* a feladatok, mert csak úgy lehet meghatározzák, hogy a feladatokat, mielőtt a feladat jön létre [az ütemezett feladat](#scheduled-jobs) által létrehozott.

- Alapértelmezés szerint feladatok maradnak aktív állapotú a projekt minden altevékenységét fejeződtek. Ez a probléma, hogy a feladat automatikusan leáll, amikor a projekt összes tevékenységek fejeződtek módosíthatja. A feladat **onAllTasksComplete** tulajdonság ([OnAllTasksComplete] [ net_onalltaskscomplete] a köteg .NET) való *terminatejob* automatikusan megszüntetése a feladatot, ha minden feladatainak befejeződött állapotú.

    Figyelje meg, hogy a köteg szolgáltatás úgy véli, *nem* szeretné, hogy a tevékenység befejeződött a tevékenységek egy feladatot. Ez a beállítás emiatt [manager projektfeladat](#job-manager-task)leggyakrabban használatos. Automatikus feldolgozás lemondási anélkül, hogy egy feladat-kezelő használni kívánt, ha meg kell kezdetben egy új feladat **onAllTasksComplete** tulajdonsága *noaction*, majd beállítása *terminatejob* csak a tevékenységek hozzáadása a feladat befejezése után.

### <a name="job-priority"></a>Feladat-prioritás

A köteg létrehozott feladatok prioritást rendelhet. A köteg szolgáltatás a Prioritás oszlop értékét a feladat használja a feladat ütemezése (Ez azonban nem [ütemezett feladat](#scheduled-jobs)is zavaros) a partner sorrendjét határozza meg. A prioritás az értékek-1000 1000,-1000 alatt a legalacsonyabb prioritású, és a legmagasabb 1000 alatt. A feladat prioritás frissítheti a [projekt tulajdonságainak módosítása] a[ rest_update_job] művelet (köteg REST) vagy a [CloudJob.Priority] módosításával[ net_cloudjob_priority] tulajdonság (köteg .NET).

Belül ugyanazzal a fiókkal magasabb prioritású feladatok van elsőbbségi ütemezési alsó fontos feladatok fölé. A feladat egy fiókot egy magasabb prioritású értékkel nincsenek elsőbbségi ütemezési fölé egy másik fiókba a kisebb prioritású értéket tartalmazó állást változtat.

Munka különböző készletek ütemezése független. Különböző-készletek közötti nem biztosítani, hogy egy újabb prioritás feladat ütemezve van, először üresjárati csomópontok nem éri el a társított készlet esetén. Az azonos készletben prioritás azonos szintű feladatok van ütemezik egyenlő lehetőségét.

### <a name="scheduled-jobs"></a>Ütemezett feladat

[A projekt ütemezését] [ rest_job_schedules] lehetővé teszi a köteg szolgáltatás belül ismétlődő feladatok létrehozásához. Munkaütemezés meghatározza, hogy a feladat futtatása, és a feladatok futtatandó leírásainak tartalmaz. Megadhatja, hogy az ütemezés – mennyi ideig és amikor az ütemtervet gyakorlatilag –, és milyen gyakran időszakban adott, létre kell hozni a feladatok időtartamának.

## <a name="task"></a>Tevékenység

Akkor egy feladat társított kiszámítása időegység egy feladatot. A csomóponton fusson. Tevékenységek végrehajtásához csomópont vannak rendelve, vagy addig, amíg a csomópont ingyenes sorban állnak. Egyszerűen helyezi, a tevékenység futtat egy vagy több program vagy a parancsfájlok a végezze el a munkát, szüksége van egy számítási csomópontot.

Tevékenység létrehozásakor adhatja meg:

- A **parancssorból** a tevékenység. Ez a parancssort az alábbiakon futtatható parancsfájl vagy az alkalmazás a számítási csomópontot.

    Fontos tudni, hogy a parancssorból valójában nem fut a rendszerhéj csoportban. Emiatt azt nem natív módon kihasználhatja rendszerhéj szolgáltatásokat, például a [környezeti](#environment-settings-for-tasks) kiterjesztett (ideértendők a `PATH`). Ezek a szolgáltatások előnyeit, meg kell hívnia a rendszerhéj a parancssorban – például által indítása `cmd.exe` Windows csomóponton vagy `/bin/sh` Linux:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Ha a feladatok kell az adott alkalmazás vagy parancsfájlt, amely nem szerepel a csomópont `PATH` vagy környezeti változók hivatkozást, a rendszerhéj kifejezetten a tevékenység parancssorban meghívásához.

- **Erőforrásfájl** feldolgoztatni adatokat tartalmazzák. Ezeket a fájlokat is automatikusan átmásolja a csomópontra egy **Általános célú** Azure tároló fiókban Blob-tárolóhoz a tevékenység parancssori végrehajtása előtt. További tudnivalókért lásd: a szakaszok [Indítsa el a feladatot](#start-task) , és a [fájlok és mappák](#files-and-directories).

- A **környezet változók** követel meg az alkalmazást. További információ című [tevékenységek környezeti beállításokat](#environment-settings-for-tasks) .

- A **kényszerek** alapján, amely a feladatot végre kell hajtani. A példában az a feladat futtatása, meg kell ismételni sikertelen tevékenység, a maximális száma engedélyezett maximális időt és a maximális időt, hogy a tevékenység munkakönyvtár fájlok megőrződnek.

- **Alkalmazáscsomagok** telepítéshez használni a számítási csomópontot, amelyen a tevékenység ütemezése futtatásához. [Alkalmazáscsomagok](#application-packages) adja meg, egyszerűbb telepítés és a verziószámozás, a feladatok futó alkalmazást. Tevékenységszintű alkalmazáscsomagok különösen hasznosak megosztott készlet környezetekben, ahol egy készlet különböző feladat futtatása, és pedig a program nem törli a feladat befejezésekor. Ha a feladat csomópontok-nál kevesebb feladatok a készletben, tevékenység alkalmazáscsomagok adatátvitel méretűre, mivel az alkalmazás csak a feladatok futtatása csomópontok telepíti.

Definiálása feladatokon kívül végre szeretne hajtani a csomópont kiszámítása az alábbi speciális feladatok is által biztosított a köteg szolgáltatás:

- [Indítsa el a tevékenység](#start-task)
- [Kezelő projektfeladat](#job-manager-task)
- [Projekt előkészítése és a fontos feladatok](#job-preparation-and-release-tasks)
- [Több elem példány feladatok (MPI)](#multi-instance-tasks)
- [Tevékenységfüggések](#task-dependencies)

### <a name="start-task"></a>Indítsa el a tevékenység

Társítása egy **feladat indítása** erőforráskészlethez tartozik, előkészítheti a rendszer a csomópontok. Például hajthatja végre a műveleteket, például a tevékenységek futó alkalmazások telepítése és háttérfolyamatok kezdve. A kezdő feladat fut. minden indításakor csomópontot, az mindaddig, amíg meg a készlet – beleértve a csomópont első esetén hozzáadni a készletet, és amikor újraindítja vagy reimaged

Egy elsődleges a kezdő tevékenység előnye, hogy tartalmazhat, az összes szükséges, állítsa be a számítási csomópontot, és telepítse az alkalmazást a feladat végrehajtásához szükséges információkat. Egy készlet található csomópontok számának növelése ezért egyszerűen, állítsa be az új csomópontot, és készen áll a tevékenységek fogad el őket szükséges információkat tartalmazó új cél csomópont számának – köteg már rendelkezik.

Mint bármilyen Azure köteg tevékenységet, megadhatja **az erőforrás-fájlok** listája [Azure-tárolóban]lévő[azure_storage], nemcsak a **parancssor** futtatható. Köteg először az erőforrás fájlokat másolja a csomópont Azure-tárhelyről, és ezután fut, a parancssorból. Egy készlet kezdő tevékenység a fájllista általában tartalmazza a tevékenység alkalmazás és a függőségét.

Azt azonban is lehetnek hivatkozási adatok a számítási csomópontra futó feladatok való használatra. Ha például a tevékenység kezdési parancssori tudta végrehajtani a `robocopy` művelet alkalmazások fájljai (ez volt megadott erőforrás-fájlként, és a csomópont letöltött) a kezdő tevékenység [Munkakönyvtár](#files-and-directories) a [megosztott mappára](#files-and-directories), és futtassa az MSI vagy `setup.exe`.

> [AZURE.IMPORTANT] A köteg jelenleg támogatja a *csak* az **Általános célú** tárterület-fiók típusára, az 5 [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) az [Azure kapcsolatos tároló fiókok](../storage/storage-create-storage-account.md)leírt módon. A köteg feladatok (beleértve a szokásos tevékenységek kezdési tevékenységek, projektfeladatok előkészítése és megjelenés projektfeladatok), amely *csak* az **Általános célú** tároló fiókok lakik erőforrásfájl kell megadnia.

A köteg szolgáltatás várja meg a kezdő feladat befejeződése után, figyelembe véve a csomópontot, készen áll az egyes tevékenységek általában célszerű, de adhatja meg.

Egy kezdő sikerül a művelet a számítási csomópontot, majd a csomópontjának állapota a hibát megfelelően frissül és a csomópont nincs társítva egyetlen tevékenységet sem. Tevékenység kezdési sikertelen lehet az erőforrás-fájlok másolása tárhelyről probléma esetén, vagy ha a folyamat hajtja végre a parancssor nullától különböző kilépési kódját adja eredményül.

Ha szeretne hozzáadni, vagy egy *meglévő* készlet kezdő feladata frissítése, újra kell indítani a számítási csomópontok csomópontok vonatkozni indítása a tevékenységek.

### <a name="job-manager-task"></a>Kezelő projektfeladat

Általában használata **manager projektfeladat** vezérlőelemre, illetve figyelheti a feladat végrehajtását – például hozhat létre, és a projekt a tevékenységek elküldése, meghatározása futtatni, és meghatározzák, hogy a munka befejeztével további feladatokat. Kezelő projektfeladat viszont nem korlátozott, a tevékenységekre. Bármilyen a feladat szükséges műveletek hajthatók végre teljes értékű feladatot. Például manager projektfeladat előfordulhat, hogy töltse le egy fájlt, amely a paraméter van megadva, elemezheti, hogy a fájl tartalmát, és ezek tartalma alapján további tevékenységek elküldése.

Kezelő projektfeladat előtt minden tevékenység el van indítva. Ez az alábbi szolgáltatásokat nyújtja:

- Automatikusan elküldése feladatként a köteg szolgáltatás által a feladatot létrehozásakor.

- A feladat feladatok végrehajtásához van ütemezve.

- A kapcsolódó csomópont az utolsó eltávolítjuk a készletből, amikor a készlet van folyamatban downsized.

- Az a projekt minden tevékenység befejezésére megszüntetésének is kötni.

- Amikor, újra kell indítani manager projektfeladat kapja a legmagasabb prioritásérték. Az üresjárati csomóponthoz nem érhető el, ha a köteg szolgáltatás lehet, hogy ér véget a készletben futtatásához vezető projektfeladat számára helyet szeretne csinálni más futó feladatok közül.

- Egy feladat manager projektfeladat nem elsőbbséget élveznek a feladatok más feladatok. Keresztül feladatokat csak a feladat szintű prioritásának betartják.

### <a name="job-preparation-and-release-tasks"></a>Projekt előkészítése és a fontos feladatok

Köteg előtti feladat végrehajtását beállítási nyújt a projekt előkészítése feladatok. Megjelenés projektfeladatok utáni feladatok karbantartási vagy karbantartása vannak.

- **Projekt előkészítése-feladat**: előkészítése projektfeladat futtat, a számítási csomópontjait, hogy mikorra van ütemezve feladatok, mielőtt az feladat műveleteket hajtja végre a rendszer. Az adatok másolása, hogy minden tevékenység meg van osztva, de a projekthez egyedi például előkészítése projektfeladat is használhatja.
- **Megjelenés projektfeladat**: a feladat befejezése megjelenés projektfeladat alábbiakon futtatható a készletben legalább egy tevékenység végrehajtott egyes csomópontot. A projekt előkészítése feladat által másolt adatok törlése, illetve a lehetőségre, és töltse fel a diagnosztikai naplóadatok, például megjelenés projektfeladat is használhatja.

Mind a projekt előkészítése és megjelenés feladatok lehetővé teszi, hogy adja meg a tevékenység meghívásakor futtatásához a parancssor. Szolgáltatások, mint a fájl letöltése, emelt végrehajtás, egyéni környezeti változók, maximális végrehajtási időtartam, kísérletek száma és fájl adatmegőrzési idő kínálnak.

Előkészítés és megjelenés projektfeladatok további tudnivalókért lásd: a [Futtatás előkészítése és a Befejezés projektfeladatok Azure köteg csomópontok számítja ki](batch-job-prep-release.md).

### <a name="multi-instance-task"></a>Feladat több példány

[Több elem példány tevékenység](batch-mpi.md) feladata csomóponton szeretné futtatni egynél több számítási egyidejű van beállítva. A több elem példány műveleteket engedélyezheti egy csoportja közös (például üzenet továbbítása felület (MPI)) egyetlen terhelési feldolgozása lefoglalt számítási csomópontok igénylő nagy teljesítményű számítógépes helyzeteket.

A köteg MPI feladatok fut a köteg .NET tár használatával részletes vitafórum tanulmányozza [használata több példány feladatok Azure köteg az üzenet továbbítása felület (MPI) alkalmazások futtatásához](batch-mpi.md).

### <a name="task-dependencies"></a>Tevékenységfüggések

[Tevékenység függések](batch-task-dependencies.md)magában foglalja a nevet, mint meg lehet adni, hogy egy tevékenység függ, hogy az előtt végrehajtása során más feladatok végrehajtására. Ez a funkció helyzetek, amelyben a "lefelé irányuló" tevékenység fogyasztása a kimenet egy "felsőbb szintű" tevékenység –, vagy egy felsőbb szintű feladatot hajt végre néhány inicializálni lefelé irányuló tevékenység szükséges támogatást nyújt. Ez a funkció használatához először engedélyeznie kell tevékenységfüggőségek a köteg feladaton. Ezután, az egyes tevékenységek, az attól függ, hogy egy másik számítógépre (vagy számos más), akkor adja meg, hogy a feladatokat, amelyek az adott tevékenység függ.

Tevékenységfüggőségek az alábbihoz hasonló esetek adhatja meg:

* *taskB* függ, hogy *taskA* (*taskB* nem kezdi végrehajtási *taskA* befejeztéig).
* *taskC* *taskA* és *taskB*is függ.
* *taskD* attól függ, hogy a tevékenységek *1* – *10*, például a tevékenységek tartomány végrehajtása előtt.

Nézze meg [az Azure köteg tevékenységfüggőségek](batch-task-dependencies.md) és a [TaskDependencies] [ github_sample_taskdeps] kód minta az [azure-köteg-minták] [ github_samples] GitHub tárházba további részletes információt ezt a beállítást.

## <a name="environment-settings-for-tasks"></a>Tevékenységek környezeti beállítások

Minden tevékenység hajtja végre a köteg szolgáltatás, amely akkor állítja be a számítási csomóponton környezeti változók hozzáférése van. Ide tartoznak a köteg szolgáltatás által meghatározott környezeti változók ([szolgáltatás által definiált][msdn_env_vars]) és az egyéni környezeti változók meghatározhatja, hogy a tevékenységekhez. Az alkalmazások és a feladatok végrehajtása parancsfájlok hozzáférést ezek a környezeti változók végrehajtás során.

A feladat vagy projekt szintjén egyéni környezeti változók beállíthatók úgy, hogy a *környezeti beállítások* tulajdonság, ezek entitás feltöltése. Például jelenik meg a [Projekt feladatban] [ rest_add_task] műveletet (köteg REST API-val) vagy a [CloudTask.EnvironmentSettings] [ net_cloudtask_env] és [CloudJob.CommonEnvironmentSettings] [ net_job_env] a köteg .NET tulajdonságait.

Az ügyfél-alkalmazás vagy szolgáltatás szerezheti be a tevékenység környezet változót is szolgáltatás által definiált és egyéni, a [tevékenység adatainak] használatával[ rest_get_task_info] művelet (köteg REST) vagy a [CloudTask.EnvironmentSettings] elérésével[ net_cloudtask_env] tulajdonság (köteg .NET). A számítási csomópont végrehajtása folyamatok érhessék ezek, és a többi környezeti változók a csomópontra, például az ismerős `%VARIABLE_NAME%` (Windows) vagy `$VARIABLE_NAME` (Linux) szintaxis.

A [csomópont környezeti változók számítja ki]egy teljes listát az összes környezeti szolgáltatás által definiált változó található[msdn_env_vars].

## <a name="files-and-directories"></a>Fájlok és mappák

Minden tevékenység tartalmaz egy *Munkakönyvtár* a melyik létrehozott nulla vagy több fájlok és mappák. Következő munkakönyvtár használható a program a tevékenység feldolgozásával adatokat és a kimeneti végrehajtott feldolgozásának által futtatott tárolásához. Fájlok és a tevékenység könyvtárak a tevékenység felhasználó tulajdonában.

A köteg szolgáltatás a *legfelső szintű címtár*teszi lehetővé a fájlrendszer csomóponton egy részét. Feladatok a legfelső szintű könyvtár érheti el hivatkozó a `AZ_BATCH_NODE_ROOT_DIR` környezeti. Környezet változók használatával kapcsolatos további tudnivalókért olvassa el a [tevékenységek környezeti beállítások](#environment-settings-for-tasks)című témakört.

A legfelső szintű könyvtár az alábbi címtár szerkezetet tartalmazza:

![Csomópont directory struktúrát kiszámítása][1]

- **megosztott**: ezt a címtárat *a csomóponton futó feladatok* olvasási/írási hozzáférést biztosít. Bármely tevékenység, amelyet a csomóponton futtat létrehozása, olvasása, módosítása, és ezt a címtárat a fájlok törlése. Feladatok érheti ezt a címtárat hivatkozó a `AZ_BATCH_NODE_SHARED_DIR` környezeti.

- **indítása**: ezt a címtárat a munkakönyvtár tevékenység kezdési használja. Az összes a csomópontra a kezdő tevékenység által letöltött fájl itt találhatók. A kezdő tevékenység létrehozása, olvasása, frissítése, és ezt a címtárat a fájlok törlése. Feladatok érheti ezt a címtárat hivatkozó a `AZ_BATCH_NODE_STARTUP_DIR` környezeti.

- **Feladatok**: könyvtárában az egyes tevékenységek a csomóponton futó jön létre. Hivatkozás hozzáférés a `AZ_BATCH_TASK_DIR` környezeti.

    Minden tevékenység címtáron belül a köteg szolgáltatás létrehoz egy munkakönyvtár (`wd`) által megadott amelynek egyedi elérési utat a `AZ_BATCH_TASK_WORKING_DIR` környezeti. Ezt a címtárat a tevékenység olvasási/írási hozzáférést biztosít. A tevékenység létrehozása, olvasása, módosítása, és ezt a címtárat a fájlok törlése. Ezt a címtárat tárolja a *RetentionTime* korlátozást, a tevékenység megadott érték alapján.

    `stdout.txt`és `stderr.txt`: ezeket a fájlokat a tevékenység végrehajtása során a tevékenység mappába kerülnek.

>[AZURE.IMPORTANT] A csomópont eltávolításakor a készletből *összes* a csomóponton tárolt fájlok törlődnek.

## <a name="application-packages"></a>Alkalmazáscsomagok

Az [alkalmazáscsomagok](batch-application-packages.md) szolgáltatása könnyű kezelés és a készletek számítási csomópontjának alkalmazások telepítéséhez. Töltse fel, és az alkalmazások futtatásához a feladatok, köztük a bináris és a támogatási fájlokat több verzióinak kezelése. Ezután automatikusan telepítheti egy vagy több ezeket az alkalmazásokat a számítási csomópontok a készletben.

Megadhatja, hogy a készlet és a tevékenység szintjén alkalmazáscsomagok. Adja meg a készlet alkalmazáscsomagok, amikor az alkalmazás telepíti, minden készletben csomópontot. Tevékenység alkalmazáscsomagok megadása esetén az alkalmazás csak az, hogy mikorra van ütemezve csomópontok telepíti a feladat feladatok, csak a a tevékenység parancssori futása előtt közül.

Köteg kezeli az alkalmazáscsomagok mentheti, és üzembe helyezése számítja ki a csomópontok, így a kód és a kezelés terhelést egyszerűsíthető Azure tároló használata részleteit.

Többet szeretne megtudni az alkalmazás csomag funkció megkereséséhez kivétele [alkalmazás környezet, amelyben az Azure köteg alkalmazáscsomagok](batch-application-packages.md).

>[AZURE.NOTE] Ha egy *meglévő* csoportba készlet alkalmazáscsomagok ad hozzá, újra kell indítani a számítási csomópontok az alkalmazáscsomagok telepítendő a csomópontok.

## <a name="pool-and-compute-node-lifetime"></a>Készlet és a számítási csomópont élettartam

A köteg Azure megoldás tervezésekor van, hogy a tervezés döntés arról, hogy miként és készletek jönnek létre, és mennyi ideig kiszámítására az adott készleteket található csomópontok tartják érhető el.

A dokumentumhasználat egyik végén minden feladat, amely elküldése készlet létrehozása, és a készlet törlése, amint feladatainak Befejezés végrehajtása. Ez maximalizálja kihasználtsági, mivel a csomópontok csak van rendelve, ha szükséges, és leállítása amint üresjárati is legyenek. Ez azt jelenti, hogy a feladat kiosztandó csomópontok kell várnia, azt is fontos tudni, hogy tevékenységek ütemezett végrehajtás, amint a csomópontok érhetők el külön-külön, kiosztva, és a kezdő feladat befejeződött. Köteg jelent *nem* várja meg, amíg megelőző feladat kiosztása a csomópontok erőforráskészlethez tartozik minden található csomópontok állnak rendelkezésre. Ezzel biztosíthatja, hogy az összes rendelkezésre álló csomópontok legnagyobb felhasználásának.

Végén más a dokumentumhasználat Ha feladatok azonnal elindul, akkor az a legmagasabb prioritásérték létrehozhat egy készlet időszakokra és elérhetővé teheti a csomópontok feladatok elküldése előtt. Ebben az esetben feladatok azonnal elkezdheti, de csomópontok üresjárati előfordulhat, hogy ülnie őket az egyes várakozás közben.

A Kombinált megközelítés változó, de a folyamatban lévő, terhelést kezelése a szokásos szolgál. Lehet, hogy több feladat küldött, de méretezheti erőforráskészlethez tartozik felfelé vagy lefelé a feladat megfelelően csomópontok számának betöltése (lásd: a [Méretezés erőforrások kiszámítása](#scaling-compute-resources) a következő szakaszban). Ezt megteheti bővítésében, aktuális terheltsége, vagy ezzel kapcsolatban beérkező, ha a betöltés jelezhető alapján.

## <a name="scaling-compute-resources"></a>Méretezési számítási erőforrások

Az [Automatikus méretezés](batch-automatic-scaling.md)akkor a köteg szolgáltatás dinamikusan a aszerint, hogy az aktuális terhelést és az erőforrás számítási igényektől használatát egy készletben számítási csomópontok számának módosítása. Ez a alkalmazást futtató, és csak a szükséges erőforrások használatával, azok a fölösleges felengedése teljes költsége alsó teszi lehetővé.

Lehetősége van engedélyezni az automatikus méretezés az [Automatikus méretezés képletet](batch-automatic-scaling.md#automatic-scaling-formulas) ír, és képletet társítása erőforráskészlethez tartozik. A köteg szolgáltatás a képletet használ, határozza meg a készlet csomópontok cél száma a következő méretezési intervallum (elvégezve konfigurálható tulajdonsággal). Készlet automatikus méretezési beállításokat is megadhat, hozza létre, vagy később méretezés meg a készlet engedélyezése. Frissítheti a méretezés engedélyező készletébe nagyítási beállítást is.

Példaként talán egy feladat szükséges, hogy nagyon sok végrehajtandó tevékenységek céljából. Méretezési képlet rendelhet a készlet, amely megadja a készletben várólistás feladatok aktuális száma és a feladatokat a feladat befejezése mértéke alapján csomópontok számának. A köteg szolgáltatás rendszeresen kiértékeli a képletet, és átméretezi a készlet terhelést alapján (csomópontok sok várólistás feladatok hozzáadása és eltávolítása a csomópontok nincs aszinkron vagy futó feladatok) és a képlet egyéb beállításokat.

Méretezési képlet a következő mérési módja miatt alapulhatnak:

- Statisztikai adatokat a megadott számú óra öt percenként gyűjtött **idő mértékek** alapulnak.

- **Erőforrás-mértékek** Processzor használatát, sávszélesség-használat, memóriahasználat és csomópontok számának alapulnak.

- **Tevékenység mértékek** alapján tevékenység állapotát, például az *aktív* (aszinkron), *futó*vagy *Befejezett*.

Ha az automatikus méretezés csökkenti a készletben számítási csomópontok számának, figyelembe kell vennie a csökkentése művelet időben futó feladatok kezelése. Ez igazodik köteg biztosít a képletekben elhelyezhető *csomópont felszabadítás lehetőséget* . Például megadhatja, hogy futó feladatok vannak azonnal leállítja, azonnal leállítja, és kattintson egy másik csomóponton végrehajtásához helyezte, vagy lehetővé tenni, hogy a Befejezés gombra, mielőtt a csomópont törlődik a készletből.

Automatikus méretezés céljával kapcsolatos további tudnivalókért olvassa el a [automatikusan számítja ki a méretezés az Azure köteg készletben csomópontok](batch-automatic-scaling.md)című témakört.

> [AZURE.TIP] Számítási erőforrás-kihasználtság maximalizálása, állítsa a végén található a feladat nulla, de engedélyezése futó feladatok befejezéséhez csomópontok cél száma.

## <a name="security-with-certificates"></a>A biztonsági tanúsítványokkal

A szokásos kell használnia a tanúsítványok titkosítása vagy bizalmas információkat a tevékenységek, például a [tárhely Azure-fiók]a kulcs visszafejtése[azure_storage]. Támogatásához telepíthető tanúsítványok csomópontot. Titkosított titkos kulcsok átadott parancssori paraméterek keresztül feladatokat, vagy a tevékenység források egyikét beágyazva, és a telepített tanúsítványok használható visszafejtése őket.

[Hozzáadás tanúsítványt] használja[ rest_add_cert] művelet (köteg REST) vagy a [CertificateOperations.CreateCertificate] [ net_create_cert] módszer (köteg .NET) tanúsítvány hozzáadása egy köteg fiókhoz. A tanúsítvány egy új vagy meglévő csoportba majd társíthat. Tanúsítvány társítva erőforráskészlethez tartozik, amikor a köteg szolgáltatás minden egyes készletben csomóponton telepíti a tanúsítványt. A köteg szolgáltatás telepítése a megfelelő tanúsítványok indításakor a csomópontot, egyetlen tevékenységet sem (beleértve a kezdő tevékenység és manager projektfeladat) elindítása előtt

Tanúsítványok ad hozzá egy *meglévő* készlet, ha a számítási csomópontok csomópontok alkalmazandó tanúsítványainak újra kell indítania.

## <a name="error-handling"></a>Hibakezelő

Előfordulhat, hogy belül a köteg megoldás tevékenység- és alkalmazás hibák kezeléséhez szükséges.

### <a name="task-failure-handling"></a>Tevékenység hiba kezelése
Tevékenység hibák ezekbe a kategóriákba sorolhatók:

- **Ütemezési hibák**

    Ha egy tevékenységhez van megadva fájlokat a továbbított valamilyen okból meghiúsul, "Ütemezési hiba" legördülő listában válassza a tevékenység.

    Ütemezési hiba akkor fordulhat elő, ha a tevékenység erőforrásfájl át lett helyezve, a tárterület-fiókkal már nem érhető el, vagy egy másik probléma történt, amely letiltja a fájlok sikeres másolása a csomópontra.

- **Alkalmazáshibák**

    A folyamat a tevékenység parancssori által megadott sikertelen is lehet. A folyamat nem sikerült, amikor a folyamat által a feladatot végrehajtható által visszaadott nullától különböző kilépési kódot tekinteni (lásd: a *tevékenység kilépési kódokat* a következő szakaszban).

    Alkalmazáshibák, a köteg automatikus újra a megadott számú alkalommal felfelé a tevékenység is beállíthatja.

- **Kényszer hibák**

    Beállíthatja, hogy megadja azt a maximális végrehajtási időtartamot, projekt vagy feladat, a *maxWallClockTime*kényszer. Ez akkor lehet hasznos, ha "lefagyott" tevékenységek megszakítása.

    Ha idő legnagyobb mennyiségű túllépve, a feladat *befejezése*van megjelölve, de a kilépési kódot értéke `0xC000013A` és a *schedulingError* mező be van-e megjelölve `{ category:"ServerError", code="TaskEnded"}`.

### <a name="debugging-application-failures"></a>Alkalmazáshibák hibakeresése

- `stderr`és`stdout`

    Végrehajtásakor az alkalmazások konzerv előfordulhat, hogy a diagnosztikai kimeneti problémák megoldásához használható. Az említett korábbi részében [fájlok és mappák](#files-and-directories), a köteg szolgáltatás ír szabványos kibocsátás és a kimeneti standard hiba `stdout.txt` és `stderr.txt` fájlokat a tevékenység címtárban a számítási csomópontra. Az Azure portálon vagy a köteg SDK egyik segítségével e fájlok letöltése elemére. Ha például ezek és más fájlok hibaelhárítási célból [ComputeNode.GetNodeFile] használatával meghallgathatja[ net_getfile_node] és [CloudTask.GetNodeFile] [ net_getfile_task] a köteg .NET tárban.

- **Tevékenység kilépési kódok**

    A korábban említett tevékenység megjelölést köteg szolgáltatás nem sikerült, ha a folyamat által a feladatot végrehajtható nullától különböző kilépési kódját adja eredményül. Egy tevékenység egy folyamat végrehajt, amikor a köteg feltölti a tevékenység kilépési kód tulajdonság, amelynek *vissza a kódot a folyamat*. Fontos, hogy ne feledje, hogy egy tevékenység kilépési kód **nem** határozza meg a köteg szolgáltatás – azt határozza meg a folyamat magát vagy az operációs rendszer, amelyen a folyamat végrehajtása.

### <a name="accounting-for-task-failures-or-interruptions"></a>Könyvelési tevékenység hibák vagy kiküszöbölése

Időnként előfordulhat, hogy nem feladatokat, vagy meg semmilyen. A tevékenység alkalmazás magát előfordulhat, hogy nem, a csomópontot, amelyen a tevékenység működik, és előfordulhat, hogy újra kell indítani, vagy a csomópont előfordulhat, hogy el kell távolítani a készlet átméretezés művelet során a készlet felszabadítás házirend értéke nélkül Várakozás a feladat befejezéséhez azonnal távolítsa el a csomópontok. Minden esetben a tevékenység is lehet automatikusan helyezte köteg által egy másik csomóponton végrehajtásához.

Lehetőség arra is egy szakaszos problémához, hogy egy tevékenység, lefagy vagy túl sokáig. Beállíthatja, hogy a tevékenység maximális végrehajtási ideje. Ha túllépik, a köteg megszakítja a tevékenység alkalmazás.

### <a name="connecting-to-compute-nodes"></a>Kapcsolódás a csomópontok kiszámítása

További hibakeresése során, és jelentkezzen be a számítási csomópontra távolról hibaelhárítási végezheti el. Az Azure portal segítségével töltse le a Windows csomópontok távoli asztali Protocol (RDP) fájl, és szerezze be a biztonságos rendszerhéj (SSH) kapcsolati adatok Linux csomópontok. Szintén ezt megteheti a köteg API-k – például a [Köteg .NET] használatával[ net_rdpfile] vagy [Köteg Python](batch-linux-nodes.md#connect-to-linux-nodes).

>[AZURE.IMPORTANT] RDP, illetve SSH csomópont szeretne csatlakozni, ha először létrehoz egy felhasználó a csomópontra. Ehhez használható az Azure portál [csomópontra felhasználói fiók felvétele] [ rest_create_user] a köteg REST API segítségével hívja fel a [ComputeNode.CreateComputeNodeUser] [ net_create_user] módszer a köteg .NET vagy a hívás a [add_user] [ py_add_user] módszer a köteg Python modulban.

### <a name="troubleshooting-bad-compute-nodes"></a>Hibaelhárítási "hibás" csomópontok számítja ki.

Helyzetek, amelyben a feladatokat nem működnek a köteg ügyfélalkalmazás vagy szolgáltatás is vizsgálja meg a metaadat-alapú sikertelen feladatot azonosítása egy nem megfelelően viselkedő csomópontot. Minden csomópontjának erőforráskészlethez tartozik egy egyedi Azonosítót kap, és a csomópontot, amely egy tevékenység fut. szerepel a tevékenység metaadatok. Miután azonosította, hogy probléma csomópontot, vele több műveletek hajthatók végre:

- **Indítsa újra a csomópontot.** ([REST][rest_reboot] | [.NET][net_reboot])

    A csomópont újraindítását is előfordul, hogy törölje a jelet be például a rögzített vagy lefagyott folyamatok rejtett problémák. Figyelje meg, hogy ha a készlet tevékenység kezdési vagy a projekt előkészítése projektfeladat használja, végrehajtás, amikor újraindítja az csomópontot.

- **A csomópont reimage** ([REST][rest_reimage] | [.NET][net_reimage])

    A csomóponton Ezzel újratelepíti az operációs rendszer. Akárcsak újraindítása csomópontot, indítsa el a feladatokat, és előkészítése projektfeladat után a csomópont van már reimaged futtassa újra a rendszer.

- **A csomópont a készletből eltávolítása** ([REST][rest_remove] | [.NET][net_remove])

    Időnként szükség a készletből távolítja el teljesen az csomópontot.

- **A tevékenységek ütemezési a csomóponton letiltása** ([REST][rest_offline] | [.NET][net_offline])

    A hatékony elég a csomópont "offline", hogy nincsenek további tevékenységek vannak rendelve, de a csomópont továbbra is lehetővé teszi, hogy fut, és a készlet. Ez lehetővé teszi, hogy a hibák okát további vizsgálat végrehajtása nélkül sikertelen tevékenység adatvesztés –, és a csomópont nélkül okoz a további tevékenységek hibák. A tevékenységek ütemezési a csomópontra, majd [Jelentkezzen be a távolról](#connecting-to-compute-nodes) a vizsgálja meg a csomópont eseménynaplók, és végezze el az egyéb hibaelhárítási például letilthatja. Amikor befejezte a vizsgálat, majd áttelepítheti a csomópont online állapotba, mivel a tevékenység ütemezése ([többi][rest_online] | [.NET][net_online]), vagy az egyéb műveletek, korábbiakban tárgyalt hajtsa végre.

> [AZURE.IMPORTANT] Az egyes műveletet – ebben a szakaszban ismertetett indítsa újra, reimage, törlése és letiltása tevékenységütemezést –, megadhatja, hogy a csomóponton futó feladatok kezelésének módja, amikor a művelet végrehajtása. Például tevékenységütemezést csomóponton a köteg .NET ügyfél tár használatával letiltásakor adhatja meg a [DisableComputeNodeSchedulingOption] [ net_offline_option] felsorolásos érték megadása a **lezáró** fut-e tevékenység **Requeue** többi csomóponton ütemezéshez tőlük vagy engedélyezés futó feladatok elvégzéséhez (**TaskCompletion**) művelet végrehajtása előtt.

## <a name="next-steps"></a>Következő lépések

- Haladjon végig, és egy minta köteg alkalmazást az [első lépések az Azure köteg tárat .NET](batch-dotnet-get-started.md)lépésről lépésre. Az oktatóprogram Linux számítási csomópontok terhelési futó [Python verzióját](batch-python-tutorial.md) is van.

- Töltse le és a [Köteg Explorer] összeállítása[ github_batchexplorer] minta projekt használatra, amíg a köteg alkalmazásokat fejleszt. A köteg Intézővel végezheti el az alábbi és:
  - Figyelje meg, és készletek, feladatok és a köteg fiók tevékenységeinek kezelése
  - Töltse le a `stdout.txt`, `stderr.txt`, és egyéb fájlokat a csomópontok
  - A csomópontok felhasználók létrehozása és távoli bejelentkezés RDP-fájlok letöltéséről

- Megtudhatja, hogyan hozhat [létre Linux számítási csomópontok készletek](batch-linux-nodes.md).

- Keresse fel a [Köteg Azure-fórum] [ batch_forum] MSDN webhelyen. A fórum tehetnek fel kérdéseket, egy jó helyen-e csak tanulási, vagy használja a köteg szakértője.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
