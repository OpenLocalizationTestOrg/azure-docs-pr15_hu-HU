<properties
   pageTitle="Tűrőképessége feladatlista |} Microsoft Azure"
   description="Feladatlista az, hogy nyújt útmutatást tűrőképessége aggályokat tervezés során."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure tűrőképessége útmutatást: Tűrőképessége ellenőrzőlista

Az alkalmazás tűrőképessége tervezése és elő kell készítenie az fellépő hiba módok számos enyhítő. Tekintse át az alkalmazás tervezés, hogy rugalmasabb szemben a feladatlista elemeinek.

## <a name="requirements"></a>Követelmények

- **Az ügyfél elérhetősége követelmények meghatározása.** Az ügyfél elérhetősége követelményei összetevők fog rendelkezni az alkalmazásban, és ez hatással van az alkalmazás tervezés. Az ügyfél szerződés letöltése az alkalmazás minden szövegblokkot a elérhetősége célok, egyéb esetben a tervező nem teljesíti az ügyfelek elvárásainak. További információ című [definiálása az tűrőképessége igényeknek megfelelően alakíthatja](guidance-resiliency-overview.md#defining-your-resiliency-requirements) az [Azure rugalmassá alkalmazások tervezése](guidance-resiliency-overview.md) dokumentum.

## <a name="failure-mode-analysis"></a>Hiba mód elemzése

- **Az alkalmazás hibát mód analízis (FMA) végrehajtása.** FMA egy folyamat tűrőképessége létrehozásának korai tervezési szakaszában az alkalmazásba. A szervezeti céloknak való egy FMA a következők:  

    - Milyen típusú hibák céljával esetleg tapasztalható azonosítása 
    - Rögzítés a potenciális effektusok és a hiba az alkalmazás a különböző típusú hatását.
    - Azonosítsa a helyreállítási stratégiák. 

    További tudnivalókért lásd: [Azure rugalmassá alkalmazások tervezése: hiba mód analysis][fma].  

## <a name="application"></a>Alkalmazás

- **Több példányának services telepítése.** Szolgáltatások elkerülhetetlenül meghiúsul, és ha az alkalmazás függ, hogy egy példányát a szolgáltatás elkerülhetetlenül meghiúsul is. [Azure alkalmazás szolgáltatás](../app-service/app-service-value-prop-what-is.md)kiépítése több példányával, jelölje be az [Alkalmazás szolgáltatás tervezése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) , amely felajánlja a több példányon. Azure Felhőszolgáltatások állítsa be minden [több példányon](../cloud-services/cloud-services-choose-me.md#scaling-and-management)használni a szerepköröket. [Azure virtuális gépeken futó (VMs)](../virtual-machines/virtual-machines-windows-about.md), győződjön meg arról, hogy a virtuális architektúra tartalmazza-e több virtuális és minden egyes virtuális szerepel egy [elérhetőségének beállítása][availability-sets].   

- **Egy terheléselosztó segítségével kérések terjesztése.** Egy terheléselosztó az alkalmazás kérések megfelelő szolgáltatás példányaiban elosztja a Forgatás sérült példányok eltávolításával. Ha a szolgáltatás Azure alkalmazás szolgáltatás vagy az Azure Cloud Services használja, már meg terheléselosztása. Jó helyen jár Ha az alkalmazás Azure VMs, szüksége lesz egy terheléselosztó hozhatók létre. Az [Azure terheléselosztó](../load-balancer/load-balancer-overview.md) áttekintésben további információt. 

- **Állítsa be az Azure alkalmazás átjárók több példányon használni.** Attól függően, hogy az alkalmazás követelményeknek, az [Azure alkalmazás átjáró](../application-gateway/application-gateway-introduction.md) lehet jobban illeszkedik a alkalmazásszolgáltatások kérések terjesztése. Jó helyen jár az alkalmazás átjáró szolgáltatás egyetlen példányainak nem garantált által egy SLA, lehetséges, hogy az alkalmazás sikertelen lehet, ha az alkalmazás átjárópéldány nem sikerül. Egynél több közepes vagy nagy alkalmazás átjárópéldány zökkenőmentes elérhetőségét a [szolgáltatásiszint-szerződés](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)értelmében a szolgáltatás kiépítése.

- **A minden alkalmazás réteg használata elérhetőségének beállítása**. Helyezze a példányok egy [elérhetőségének beállítása] [ availability-sets] legalább egy virtuális példány kapcsolatot garantálja belül a [szolgáltatásiszint-szerződés](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/)feltételeit. A VMs megtalálhatók-e a egy elérhetőségének beállítása nincs garantálja védelme őket, és lehetséges, hogy az összes sikertelen lehet, vagy egyszerre frissülnek. 

- **Érdemes megfontolni az alkalmazás különböző több területre.** Ha egyetlen területére telepíti az alkalmazásokat, a ritka esetben az egész régió elérhetetlenné válik, az alkalmazás nem is érhetők el. Lehet, hogy ez az alkalmazás szolgáltatásiszint-szerződés értelmében fogadható el. Ha igen, érdemes megfontolni az alkalmazás és a szolgáltatások között több területre. Több területre telepítés használhatja az aktív-aktív mintát (való terjesztésére kérések több aktív példányok között) vagy egy aktív-passzív mintát (megőrzési egy "meleg" példány foglalás, abban az esetben, ha az elsődleges példány nem sikerül). Azt javasoljuk, hogy területi párban keresztül telepíti a az alkalmazásszolgáltatások több példányát. További tudnivalókért lásd: [üzleti folytonosságot és katasztrófa helyreállítási (BCDR): Azure párosított régiók](../best-practices-availability-paired-regions.md).

- **Hajtja végre tűrőképessége mintázatok távoli műveletek, ahol csak lehetséges.** Ha az alkalmazás távoli szolgáltatások közötti kommunikáció függ, kommunikáció elérési elkerülhetetlenül sikertelen lesz. Ha több hibák, sikerült az az alkalmazásszolgáltatások hátralévő megfelelő előfordulását kéréseivel túl sok. Több mintázatok vannak hasznos, beleértve az időtúllépés mintát, [ismételje meg a minta]gyakori hibák kezelésének[retry-pattern], a [megszakító] [ circuit-breaker] mintázatot, és másokkal. További tudnivalókért olvassa el a [Azure rugalmassá alkalmazások megtervezése](guidance-resiliency-overview.md#implementing-resiliency-strategies)című témakört. 

- **Autoscaling segítségével nő a betöltés válaszolni.** Ha az alkalmazás automatikusan nő betöltés méretezése nincs beállítva, akkor lehet, hogy az alkalmazás-szolgáltatások meghiúsul, ha az azok telítődjön felhasználói kérések. További információt talál az alábbiakat:

    - Általános: [méretezhetőség ellenőrzőlista](../best-practices-scalability-checklist.md) 
    - Azure alkalmazás szolgáltatás: [kézzel és automatikusan átméretezheti a példányok száma][app-service-autoscale]
    - [Automatikus méretezés egy felhőalapú szolgáltatásba hogyan] cloud Services:[cloud-service-autoscale]
    - Virtuális gépeken futó: [automatikus méretezése és virtuális gép skála állítja be.][vmss-autoscale]


- **Amikor csak lehetséges aszinkron műveletek végrehajtása.** Szinkronizált műveletek játíthatja erőforrások és a blokkolása egyéb műveleteket, miközben a hívó megvárja, amíg a folyamat befejezéséhez. Tervezze meg az alkalmazást, hogy minden lehetséges esetben aszinkron műveletekhez minden részét. Aszinkron programozás megvalósításáról C# a további tudnivalókért lásd: [aszinkron programozási aszinkron és kerülve][asynchronous-c-sharp].

- **Kezelővel Azure irányítja a forgalmat a alkalmazás különböző régiók.**  [Azure forgalom Manager] [ traffic-manager] hajtja végre a DNS-szintjén terheléselosztás és is irányítja a forgalmat a [forgalom útválasztás] alapján különböző régiók[ traffic-manager-routing] módot, adja meg, és az alkalmazás végpontok állapotának. 

- **Állítsa be és tesztelje a terheléselosztókkal és a menedzserek forgalom állapot szondákat.** Győződjön meg arról, hogy az állapot logika a kritikus részei a rendszer ellenőrzi, és állapota szondákat megfelelően válaszol.

    - Az állapot ellenőrzi az [Azure forgalom Managerhez] [ traffic-manager] és [Azure terheléselosztó] [ load-balancer] egy adott funkció szolgál. Az adatforgalom-parancsra az állapot vizsgálati határozza meg egy másik területére átadni. Egy terheléselosztó azt határozza meg, e, ha el szeretne távolítani egy virtuális Forgatás.      

    - A rendszerállapot végpont kritikus függőségek, hogy az azonos régión belüli van telepítve, és amelyek meghibásodása indítson el egy másik terület áttérni kell olvassa el a forgalom Manager vizsgálati.  

    - Az egy terheléselosztó az állapot végpontot jelentse a virtuális állapotának. Nem tartalmazza a más rétegek és a külső szolgáltatásokat. Ellenkező esetben a, amely akkor következik be, a virtuális kívüli hibát okoz a terheléselosztó, ha el szeretné távolítani a virtuális Forgatás.

    - Végrehajtási a rendszerállapot figyelése a alkalmazásban, olvassa el [Állapot végpont figyelése mintát](https://msdn.microsoft.com/library/dn589789.aspx).

- **Figyelje a külső szolgáltatásokra támaszkodik.** Az alkalmazás függőségek külső szolgáltatások vannak, ha where azonosítása, hogyan külső szolgáltatásokra történhet, és az alkalmazás a Mi az az hibák számába effektus lesz. Egy külső szolgáltatásnak előfordulhat, hogy nem része a figyelés és diagnosztika, ezért fontos, hogy jelentkezzen be a meghívásához őket, és a összehangolására őket az alkalmazás állapota a és a diagnosztikai naplózás egyedi azonosítója használatával. További információt a gyakorlati tanácsok a figyelés és diagnosztika, lásd: a [Figyelés és diagnosztika útmutatást] [ monitoring-and-diagnostics-guidance] dokumentumot.

- **Győződjön meg arról, hogy minden olyan külső felhasznált szolgáltatása egy SLA.** Ha egy külső szolgáltatásnak függ, hogy az alkalmazás, de a harmadik fél biztosít egy SLA formájában elérhetősége garanciát, az alkalmazás elérhetősége is nem lehet garantálni. A SLA célszerű csak az alkalmazás legalább elérhető részeként.


## <a name="data-management"></a>Adatok kezelése

- **A replikáció módszerek az alkalmazás adatforrások megértéséhez.** Az alkalmazás adatok kíván tárolni a különböző adatforrásokból, és más elérhetősége követelményeik vannak. A replikáció módszerek az egyes: Azure adattárolás felmérése, többek között a [Azure tároló replikációs](../storage/storage-redundancy.md) és [SQL adatbázis aktív Geo replikációs](../sql-database/sql-database-geo-replication-overview.md) ahhoz, hogy az alkalmazás adatok teljesülnek.

- **Győződjön meg arról, hogy nincs egyetlen felhasználói fiók gyártási és a biztonsági másolat adatokhoz való hozzáférés.** Az adatok biztonsági másolatok hordoznak, ha egy egyetlen felhasználói fiók gyártási és a biztonsági másolat források írási engedéllyel rendelkezik. Rosszindulatú felhasználók közben rendszeresen sikerült véletlenül töröl sikerült szándékosan törölni az adatok. Tervezze meg az alkalmazást az egyes felhasználói fiókok az engedélyek korlátozása, hogy csak a írási hozzáférést igénylő felhasználók rendelkezik írási jogosultsággal, és csak a termelési vagy biztonsági másolatot, de egyszerre.

- **Az adatforrás átveszi és visszavétele dokumentum folyamat, és tesztelje.** Abban az esetben, ha az adatforrás catastrophically nem sikerült egy emberi operátor kell kövesse az utasításokat a új adatforrás átadni csoportja. Ha dokumentált lépéseiben hibákat, operátorok egyike nem tudnak sikeres kövesse őket, és nem sikerül az erőforrás fölé. Rendszeresen tesztelje a utasítás lépéseket, ellenőrizze, hogy sikeresen átveszi és az adatforrás visszavétele operátor követni őket.

- **Az adatok biztonsági másolatok ellenőrzése.** Rendszeresen bizonyosodjon meg arról, hogy az adatok biztonsági másolatának mi várható parancsfájl futtatásával érvényesítése az adatok integritását, séma és lekérdezések. Nincs biztonsági problémákat, ha még nem hasznos lehet ahhoz, hogy az adatforrások visszaállítása pont. Jelentkezzen be, és a jelentés esetleges következetlenségekre, így a biztonsági másolat szolgáltatás javítható.

- **Fontolja meg inkább a tárhely fióktípus, amely geo felesleges.** Azure tárterület-fiókjában tárolt adatok mindig van replikált helyi meghajtóra. Vannak azonban több replikációs stratégiák közül választhat, amikor a tárterület-fiókkal már kiépítve. Jelölje ki az [Azure-olvasásra Geo felesleges tároló (TS-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) szemben a ritka eset alkalmazás adatok védelme, amikor egy teljes terület elérhetetlenné válik.

    > [AZURE.NOTE] VMs nem támaszkodhat TS-GRS replikációs visszaállítani a virtuális lemez (virtuális fájlok). Ehelyett az [Azure másolat]eszközzel[azure-backup].   

## <a name="operations"></a>Műveletek

- **Figyelés, és a gyakorlati tanácsok az alkalmazás riasztási hajtja végre.** Anélkül, hogy megfelelő figyelés, diagnosztika és riasztási nincs mód a hibák feltárása az alkalmazásban, és a hibák kijavításának céljából operátor figyelmeztetést nem. Ajánlott eljárások a további tudnivalókért lásd: a [Figyelés és diagnosztika útmutatást] [ monitoring-and-diagnostics-guidance] dokumentumot.

- **Mérje távoli hívási statisztikát, és elérhetővé teheti az adatokat az alkalmazás csoportnak.**  Ha nem nyomon követése és a valós idejű távoli hívásvezérlés statisztikai jelentést, és be egyszerűen olvassa el ezt az információt nyújt, a műveletek csoport nem lesz egy pillanatnyi betekintést az alkalmazáshoz állapota. Ha csak a mérték átlagos távoli hívásvezérlés idő, nem kell elegendő információt, jelenítse meg és a szolgáltatások a hibák. Összegzés távoli hívásvezérlés mértékek például késés, átviteli és a 99 és 95-ös percentilisek hibákat. A mérési módja miatt a Kihúzás belül minden PERCENTILIS előforduló hibák statisztikai elemzések végrehajtása

- **Nyomon követheti a tranziens kivételek és ismétlések számát egy megfelelő időkeret fölé.** Ha nem nyomon követése és tranziens kivételek figyelésére és újrapróbálkozások adott idő alatt, akkor lehet, hogy probléma vagy hiba elrejthető szerint az alkalmazás újrapróbálkozási logika. Ez azt jelenti, hogy ha a figyelés és naplózás csak megoldás sikeres vagy sikertelen az során, arra, hogy a műveletnek többször kell ismételni miatt kivételek rejtve marad. Kivételek növelése időbeli trend azt jelzi, hogy a szolgáltatás probléma meghiúsulhat. További tudnivalókért lásd: a [szolgáltatás speciális útmutatókat újra][retry-service-guidance].

- **Egy korai figyelmeztetés rendszer értesíti a operátor végrehajtása.** Azonosítsa a fő teljesítménymutatók jelölők az alkalmazás egészségi, például a kivételek ideiglenes (tranziens) és a távoli hívja fel a késés, és állítsa a megfelelő küszöbértéket az egyes őket. A küszöbértéket elérésekor értesítést küld műveletek. Ezek a küszöbértékek beállítása kapcsolatos problémák azonosítása, mielőtt kritikus válik, és helyreállítási válaszolni szinten.

- **A Megjelenés folyamat az alkalmazás a dokumentum.** Részletes fontos folyamat dokumentáció, nélkül operátor előfordulhat, hogy egy hibás frissítés telepítése vagy helytelenül állíthat be az alkalmazás beállításai. Jól meghatározása a Megjelenés folyamat dokumentum, és győződjön meg arról, hogy azt a teljes műveletek csoport számára elérhető. Az alkalmazás rugalmassá telepítési gyakorlati tanácsok a [rugalmassá telepítési] részletezi[ guidance-resilient-deployment] Tűrőképessége útmutató szakaszát.

- **Győződjön meg arról, hogy a csoport több személynek képzett-e figyelje az alkalmazást, és kézi helyreállítási lépések elvégzéséhez.** Ha csak egy egyetlen operátor a csapat figyelje az alkalmazást, és helyreállítási lépéseket elindításához képes felhasználók körét, az adott személy hiba egyetlen pont lesz. Több személy észlelése és javítása a képzése, és ellenőrizze, hogy mindig legyen legalább egy aktív bármikor.

- **Az alkalmazás telepítési folyamatot automatizálása.** Ha a műveletek munkatársak manuálisan a az alkalmazás telepítéséhez szükséges, emberi hiba okozhatják sikertelen történő telepítéséhez. Alkalmazások telepítésének automatizálása gyakorlati tanácsok a további tudnivalókért lásd: a [rugalmassá telepítési] [ guidance-resilient-deployment] Tűrőképessége útmutató szakaszát.

- **A Megjelenés folyamat alkalmazás elérhetősége maximalizálása tervezése.** Ha a Megjelenés folyamat szolgáltatások, a telepítés során kapcsolat nélküli módba van szüksége, az alkalmazás nem érhető el csak azokat az újrakapcsolódáskor. [Kék és zöld](http://martinfowler.com/bliki/BlueGreenDeployment.html) , és [engedje fel az Kanári](http://martinfowler.com/bliki/CanaryRelease.html) telepítési technikával használni a termelési alkalmazás telepítéséhez. Mind az alábbi eljárások magában foglalja a Megjelenés programkódot összegzik gyártási kód telepít, így a felhasználók megjelenés kód átirányítható abban az esetben egy gyártási kódot. További tudnivalókért lásd: a [rugalmassá telepítési] [ guidance-resilient-deployment] Tűrőképessége útmutató szakaszát.

- **Jelentkezzen be, és az alkalmazás telepítések naplózási.** Ha előkészített telepítési technikák, például a gyártási futó alkalmazás egynél több verziója lesz kék és zöld vagy canary kiadásokban. Ha a probléma a következő történjen, rendkívül fontos a problémát okozó az alkalmazás melyik verzióját. A lehető legnagyobb nyelvspecifikus információk rögzítése robusztus naplózás stratégia végrehajtása. 

- **Győződjön meg arról, hogy az alkalmazás nem működik az alábbi [Azure előfizetés korlátozza](../azure-subscription-service-limits.md).** Azure előfizetések korlátai bizonyos erőforrástípus, például az erőforrás csoportok számát, magmintákat számát, és a tárterület-fiókok rendelkezik.  Az alkalmazás igényeknek megfelelően alakíthatja meghaladják az Azure előfizetés korlátozások, ha egy másik Azure rendelkezés és előfizetési kellő erőforrások létrehozása van.

- **Győződjön meg arról, hogy az alkalmazás nem működik az alábbi [szolgáltatás – korlátozza](../azure-subscription-service-limits.md).** Egyéni Azure szolgáltatások van felhasználási korlátozások &mdash; például korlátozza a tárhely, átviteli, a kapcsolatok, a kérelmek egy második és a többi mértékek számát. Az alkalmazás sikertelen lesz, ha túl az ezek a korlátok források megkísérli. Ez a szolgáltatás szabályozási és a lehető legrövidebb leállás, érintett felhasználónál eredményez. 

    Attól függően, hogy az adott szolgáltatás és az alkalmazás igényeknek megfelelően alakíthatja, gyakran elkerülhető, ezek a korlátok (például egy másik árak réteg kiválasztása) méretezés vagy a méretezés kifelé (Hozzáadás új példányok).  

- **Tervezze meg az alkalmazás tárhelyre Azure tároló méretezhetőség és a teljesítmény célok alá.** Azure tárolására szolgál belül előre definiált méretezhetőség és a teljesítmény célok működik, így tervezése a csatlakozást tároló ezen célok belül az alkalmazást. Ha egy művelet túllépi a tárolók az alkalmazás tároló szabályozásának fog tapasztalni. A hiba kijavításához kiépítése további tárterület-fiókokat. Ha a tárterület-fiók által az alábbi, további Azure előfizetéseket kiépítése, és kattintson a további tárterület-fiókok nem kiépítése. További tudnivalókért lásd: [Azure tároló méretezhetőség és a teljesítmény célok](../storage/storage-scalability-targets.md).

- **Válassza ki a megfelelő virtuális az alkalmazást.** Mérje le a tényleges Processzor, memóriahasználat, a lemez és a gyártási a VMs I/O, és ellenőrizze, hogy a kijelölt virtuális mérete elegendő. Ha nem, az alkalmazás kapacitás problémák léphetnek fel, a VMs határai megközelíthető. Virtuális méretű a [méretét az Azure virtuális gépeken futó](../virtual-machines/virtual-machines-windows-sizes.md) dokumentumban részletesen.

- **Állapítsa meg, ha az alkalmazás terhelést stabil vagy hullámzó időbeli.** Ha a terhelést ingadozik adott idő alatt, használata Azure virtuális skála állítja automatikus méretezés a virtuális példányainak száma. Egyéb esetben be kell manuálisan növelheti vagy csökkentheti a VMs számát. További tudnivalókért lásd: a [Virtuális gép skála készletek áttekintése](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Jelölje ki a megfelelő szolgáltatási réteg Azure SQL-adatbázis.** Ha az alkalmazás Azure SQL-adatbázissal, győződjön meg arról, hogy kijelölte a megfelelő szolgáltatási réteg. Ha bejelöli az egy réteg, amely nem képes kezelni az alkalmazás adatbázis-tranzakción egység (DTU) követelmények, a program szabályozott az adatok felhasználásával. Jelölje ki a megfelelő szolgáltatáscsomagja a további tudnivalókért lásd: a [SQL-adatbázis beállítások és a teljesítmény: megértéséhez, hogy mi érhető el az egyes szolgáltatási réteg](../sql-database/sql-database-service-tiers.md) dokumentumot. 

- **Telepítés visszaállítás csomaggal rendelkezik.** Érdemes lehet, hogy az alkalmazás telepítési sikertelen sikerült, és már nem érhető el az alkalmazást. Térjen vissza az utolsó ismert jó verziót, és minimalizálhatja az állásidőt egy modullal tervezése. Lásd: a [rugalmassá telepítési] [ guidance-resilient-deployment] további információt a Tűrőképessége útmutatást dokumentum című szakaszát. 

- **Hozzon létre egy folyamat Azure támogatási személlyel.** Kapcsolatfelvétel a [támogatási Azure](https://azure.microsoft.com/support/plans/) folyamata értéke nincs beállítva, mielőtt kapcsolatba lépni a támogatási szükség, ha legrövidebb leállás fog kell meghosszabbítása, a támogatási folyamat első alkalommal valójában. Kapcsolatfelvétel a támogatási és a problémák escalating kezdettől fogva az alkalmazás tűrőképessége részeként a folyamat tartalmazzák.

- **Győződjön meg arról, hogy az alkalmazás több, mint a maximális száma előfizetésenként tárterület-fiókok nem-e használni.** Azure lehetővé teszi, hogy legfeljebb 200 tároló számláinak száma előfizetésenként. Ha az alkalmazás további tárterület-fiókokat, mint a jelenleg elérhető az előfizetését, akkor hozhat létre új előfizetést, és ott további tárterület-fiókok létrehozása. [További információért Azure előfizetés és szolgáltatás korlátozások, kvótákat, és](../azure-subscription-service-limits.md#storage-limits)megtekintése korlátozásokkal

- **Győződjön meg arról, hogy az alkalmazás nem haladja meg a virtuális gép lemezt méretezhetőség célok.** Egy IaaS Azure virtuális támogatja az adatok lemezt számos tényező, többek között a virtuális méretét és a tárhely fiók típusától függően a szám csatolása. Ha az alkalmazás meghaladja a méretezhetőség célok virtuális gép lemezen, további tárterület fiókok kiépítése és létrehozásához a virtuális gép van. További tudnivalókért lásd: az [Azure tároló méretezhetőség és teljesítménybeli célok] (... / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Teszt

- **Feladatátvétel és visszaállás az alkalmazás tesztelés végrehajtása** Ha még nem teljesen tesztelve feladatátvétel és visszaállás, nem lehet biztos, hogy az alkalmazásban a függő szolgáltatások lyncen vissza szinkronizált módon vészhelyreállítás során. Győződjön meg arról, hogy az alkalmazás függő szolgáltatások feladatátvétel és fail: a megfelelő sorrendben.

- **Hibafa-utasítások beszúrását az alkalmazás tesztelés végrehajtása** Az alkalmazás hibát okozhat számos más oka lehet annak, tanúsítvány érvényességi, például egy virtuális vagy adattárolási hibák rendszer-erőforrásokat kimerülése. Az alkalmazás tesztelése olyan környezetben, amelyek vagy valós hibáit kiváltó gyártási, a lehető legközelebb. Ha például tanúsítványok törlése, mesterségesen felhasználni a rendszer-erőforrásokat vagy tároló törlésekor. Ellenőrizze az alkalmazás képes lévő hibák, egyedül és kombinált diagramtípusokat visszaállításához. Ellenőrizze, hogy hibák nem propagálása vagy egymásra keresztül, a rendszer.

- **Gyártási mindkét szintetikus és valódi felhasználói adatok használata vizsgálatok futtatása:** Teszt és munkakörnyezeti azonosak ritkán, ezért fontos, kék és zöld vagy canary környezetet alakítana ki és az alkalmazás tesztelése gyártási. Lehetővé teszi, hogy az alkalmazás tesztelése gyártási valós terhelés alatt, és győződjön meg arról, hogy amikor teljesen telepítve várt módon fog működni.

## <a name="security"></a>Biztonsági

- **Alkalmazás szintű védelmet elosztott megtagadását szolgáltatás (DDoS) támadások elleni végrehajtása.** Azure szolgáltatások a hálózati rétegben DDos támadások ellen védett. Azonban Azure nem nyújtanak alkalmazásszintű támadások, mivel a true felhasználói kérések rosszindulatú felhasználói kérések megkülönböztetni nehezen. Alkalmazásszintű DDoS támadások elleni védelmét a további tudnivalókért lásd: a [Microsoft Azure hálózati biztonsági](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (PDF-fájl letöltése) "DDoS elleni védelem" című szakaszát.

- **Az alkalmazás erőforrások eléréséhez a legalacsonyabb jogosultság elve végrehajtása.** Az alkalmazás források az access az alapértelmezett kell a lehető. Jóváhagyás kombinálásával magasabb szintű engedélyek kiosztása. Alapértelmezés szerint az alkalmazás erőforrások túl alkalmazásszolgáltatásokra hozzáférés engedélyezése eredményezhet valaki szándékosan vagy véletlenül erőforrások törlése. Azure [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-built-in-roles.md) kezelésére felhasználóként biztosítja, azonban fontos, hogy más erőforrások: a saját engedélyeket rendszerek, például SQL Server rendelkező legalacsonyabb jogosultság engedélyeinek ellenőrzése. 

## <a name="telemetry"></a>Telemetriai

- **Jelentkezzen be a telemetriai adatokat, az alkalmazás az éles üzemi környezetben futása közben.** Robusztus telemetriai információk rögzítése, miközben az alkalmazást futtató az éles üzemi környezetben, vagy nem lesz, akkor elegendő információt diagnosztizálása a probléma okát, problémák, miközben meg van aktívan szolgáló felhasználók. További információ a naplózás érhető el gyakorlati tanácsokat érhető el a [Figyelés és diagnosztika útmutatást] [ monitoring-and-diagnostics-guidance] dokumentumot.

- **Naplózás az aszinkron minta használatával végrehajtása.** Ha naplózási műveletek szinkron, azok megakadályozhatja az alkalmazás kódját. Győződjön meg arról, hogy a naplózási műveletek aszinkron műveletként végrehajtását. 

- **Naplóadatok összehangolására szolgáltatás közötti együttműködést kívánja lehetővé.** Egy tipikus n szintű alkalmazásban a felhasználó kérésére több szolgáltatás határai előfordulhat, hogy bejárása. Például a felhasználó kérésére általában származik, a webes réteg átadni a vállalati réteg és az adatok réteg végül állandó. Összetettebb esetekben a felhasználó kérésére számos különböző szolgáltatások és az adatok áruházban kell juttatni. Győződjön meg arról, hogy a naplózás rendszer felel meg hívások szolgáltatás közötti együttműködést kívánja lehetővé, hogy az alkalmazás egész nyomon követhető a kérést.

##  <a name="azure-resources"></a>Azure erőforrások 

- **Az erőforrások kiépítése Azure erőforrás-kezelő sablonok használatával.** Erőforrás-kezelő sablonok megkönnyítik a automatizálása telepítések PowerShell, illetve az Azure CLI, amely egy megbízhatóbb telepítési folyamatot. További tudnivalókért lásd: [Azure erőforrás szolgáltatásának áttekintése][resource-manager].

- **Erőforrások érthető neveket ad.** Erőforrások érthető neveket biztosítva alkalmazva könnyebben elvégezhetők az adott erőforrás keresse meg és szerepének bemutatása. További tudnivalókért lásd: [ajánlott elnevezési szabályai Azure erőforrások](guidance-naming-conventions.md) 

- **Használat szerepköralapú hozzáférés - szerepalapú**. Azure erőforrások rendszerbe való hozzáférés szabályozása a RBAC használatával. RBAC lehetővé teszi, hogy engedélyt szerepköröket rendelhet, hogy a módosítások vagy véletlen törlését telepített erőforrásokhoz DevOps csapata tagjainak. További tudnivalókért lásd: [Ismerkedés a hozzáférés-kezelés az Azure-portálon](../active-directory/role-based-access-control-what-is.md) 

- **Kritikus erőforrások, például VMs erőforrás zárolások használata.** Erőforrás-zárolások megakadályozhatja, hogy a operátor erőforrás véletlen törlését. További információért témakörökben [zárolása a Azure-kezelő eszközzel](../resource-group-lock-resources.md) 

- **Területi párokká.** Két sorterület telepítésekor válasszon ugyanarra a regionális két régióban. Egy széles üzemszünetek megelőzve egy régió helyreállítási kívül minden pár van prioritása. Egyes szolgáltatások, például Geo felesleges tárhely párosított régió automatikus replikáció biztosítanak. További tudnivalókért lásd: [üzleti folytonosságot és katasztrófa helyreállítási (BCDR): Azure párosított régiók](../best-practices-availability-paired-regions.md) 

- **Erőforrás-csoportok rendezése függvény és a életciklus**.  Az általános erőforrás csoport információforrások, amelyek az azonos életciklus megosztása kell tartalmaznia. Ezzel megkönnyíti a telepítések kezelése törlése tesztet telepítések és jogokat access, az esélye annak, hogy egy éles üzemi véletlenül törölt vagy módosított csökkentése. Gyártási, fejlesztési, külön erőforrás-csoportok létrehozása, és tesztelje a környezetben. Több területre környezetben tegye elérhetővé az egyes régiókra vonatkozó erőforrások külön erőforrás csoportokba. Ezzel megkönnyíti az telepítsen újra egy régió más régió módosítása nélkül. 

## <a name="azure-services"></a>Azure szolgáltatások 

Az alábbi ellenőrzőlista elemek alkalmazása az egyes szolgáltatásokhoz Azure.

###  <a name="app-service"></a>Alkalmazás szolgáltatás 

- **Normál vagy prémium réteg használja.** Ezeket a réteg átmeneti tárolásra szolgáló helyek támogatja, és az automatikus biztonsági mentést. További információ a [Azure alkalmazás szolgáltatás csomagok meg szeretné vizsgálni áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) című témakörben találhat. 

- **Kerülje a méretezés felfelé vagy lefelé.** Ehelyett válassza ki a réteg és példány mérete, amely a teljesítmény követelményeknek tipikus betöltése, majd a [ki a méretezés](../app-service-web/web-sites-scale.md) csoportban a példányokat forgalma változások kezelése. Méretezés felfelé és lefelé kezdeményezheti az alkalmazás újraindítása.  

- **Alkalmazás beállításainak konfigurálása tárolni.** Alkalmazás beállításainak segítségével alkalmazás beállítások konfigurációs beállítások tartsa lenyomva az ujját. Adja meg a beállításokat az erőforrás-kezelő sablonok és használatáról a PowerShell, így alkalmazza őket az automatizált központi telepítés részeként, és frissítse a folyamat, amely megbízhatóbb. További tudnivalókért lásd: [konfigurálása web Apps alkalmazások Azure App szolgáltatásban](../app-service-web/web-sites-configure.md). 

- **Hozzon létre külön App milyen szolgáltatáscsomagok, gyártás-és a vizsgálat.** A éles üzemi helyek teszteléshez nem használja.  Alkalmazás szolgáltatás ugyanarra a csomagra belül minden alkalmazás megosztása az azonos virtuális példányok. Ha termelési és a próba-telepítések ugyanarra a csomagra helyezi, a éles üzemi negatív hatással lehet. Például betöltés vizsgálatok csökkentheti a éles webhelyet. Ha elhelyez egy külön csomagra szóló próba-telepítések, akkor elkülönítése őket a termelési verzióból.  

- **A webes API-khoz külön webalkalmazásokban**. Ha a megoldás előtér-webhely és a webes API-t is tartalmaz, fontolja meg az külön alkalmazás szolgáltatás alkalmazásokba decomposing őket. A tervezés megkönnyíti a megoldást felbontani terhelést szerint. Futtathatja a web app és az API-t a külön App milyen szolgáltatáscsomagok, így azok megadhat egymástól függetlenül. Ha már nincs szükség az, hogy a méretezhetőség szintjét először telepítheti az alkalmazások ugyanarra a csomagra szóló, és át őket külön csomagok később szükség esetén.

- **Kerülje az alkalmazás biztonsági szolgáltatás készítsen biztonsági másolatot az Azure SQL-adatbázisait.** Használja [az automatikus biztonsági mentés SQL-adatbázis][sql-backup]. Alkalmazás szolgáltatás biztonsági mentése az adatbázis egy SQL .bacpac fájlt, amely a költségek DTUs exportálja.  

- **Egy átmeneti tárolásra szolgáló tárolóhely üzembe.** Hozzon létre egy telepítési tárolóhely az előkészítés. Az átmeneti tárolásra szolgáló tárolóhely alkalmazás frissítéseket, és ellenőrizze a telepítés előtt éles lecserélése azt. Ez csökkenti az esélye annak gyártási hibás frissítés. Is biztosítható, hogy a összes előfordulását előtt éles cserélni folyamatban van bemelegíteni. Sok alkalmazás jelentős warmup és hideg kezdetének van. További tudnivalókért olvassa el a [átmeneti tárolására környezetekben Azure App szolgáltatásban web Apps alkalmazások beállítása](../app-service-web/web-sites-staged-publishing.md)című témakört. 

-  **Hozzon létre egy telepítési tárolóhely tartása a legutolsó helyes (LKG) telepítésben.** Amikor rendszerbe állítják frissítés gyártási, helyezze át az előző éles üzemi a LKG tárolóhely. Ezzel megkönnyíti az állítsa vissza a hibás telepítés. Probléma később Fedezze fel, ha gyorsan visszatérhet a LKG verziót. További tudnivalókért lásd: [Azure hivatkozás architektúra: egyszerű webalkalmazás](guidance-web-apps-basic.md). 

- **Diagnosztikai naplózás engedélyezéséhez**, többek között az alkalmazás naplózási és webkiszolgáló naplózásának. Figyelemmel kísérésére és a diagnosztikai naplózás fontos el. Lásd: a [diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Napló blob-tároló**. Ezzel megkönnyíti a összegyűjtése és elemezheti az adatokat. 

- **Naplók külön tárterület-fiók létrehozása.** Naplók és az alkalmazás adatok nem használja a tárhely ugyanazzal a fiókkal. Ez segít megelőzni naplózás az alkalmazás teljesítményének csökkentése. 

- **Teljesítmény figyelését.** Használja a monitor alkalmazás teljesítményének és terhelés alatt viselkedés például [Új Relic](http://newrelic.com/) vagy [Alkalmazás háttérismeretek](../application-insights/app-insights-overview.md) szolgáltatás felügyelete a teljesítmény.  Teljesítményét figyelve lehetővé teszi az alkalmazás valós idejű betekintést. Lehetővé teszi, hogy problémáinak diagnosztizálása és analízist kiváltóok-hibák. 


###  <a name="application-gateway"></a>Alkalmazás átjáró 

- **Építse legalább két példánya.** Üzembe helyezéséhez alkalmazás átjáró legalább két példánya. Egyetlen példány található egy egyetlen hiba. Két vagy több példány használata redundancia és méretezhetőség. Azt, hogy jogosult legyek az [SZOLGÁLTATÁSISZINT](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)-, engedélyeznie kell két vagy több közepes vagy nagy példányát. 

### <a name="azure-search"></a>Azure keresés

- **Egynél több kópia kiépítése.** Használjon legalább két kópiák olvasási magas rendelkezésre állás és az írási és olvasási magas rendelkezésre állás három.

- **Állítsa be a több elem régió telepítésekhez indexelő.** Ha több területre telepítéshez, fontolja meg az indexelő a folyamatosság beállításainak.

    - Ha az adatforrás geo replikált, mutasson a helyi adatok forrása kópia minden egyes területi Azure keresési szolgáltatás indexelő.  

    - Az adatforrás nem esetén geo replikált, mutasson a azonos adatforrás több indexelő, hogy az Azure keresési szolgáltatás több régióban folyamatosan és függetlenül indexelni az adatforrásból. További tudnivalókért lásd: [Azure keresési teljesítmény és optimalizálási kapcsolatos szempontok][search-optimization].

###  <a name="azure-storage"></a>Azure tárhely 

- **Az alkalmazás adatokat használja az olvasásra geo felesleges tároló (TS-GRS).** TS-GRS tároló replikálja az adatokat egy másodlagos területre, és a másodlagos régióból csak olvasási hozzáférést biztosít. Ha az elsődleges területen a tárhely üzemszünetek, az alkalmazás a másodlagos régióból olvasni az adatokat is. További tudnivalókért lásd: [Azure tároló replikáció](../storage/storage-redundancy.md).

- **A virtuális lemezt, prémium tároló beépülő modullal** További tudnivalókért lásd: [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md).

- **A várakozási tárolására biztonsági várólista létrehozása egy másik tartományban lévő.** Várólista tárolására a csak olvasható kópia korlátozott használatát, mert nem lehet sorba vagy elemek feldolgozásához. Ehelyett hozzon létre egy biztonsági várólista egy másik tartományban lévő tárterület-fiókban. A tároló üzemszünetek esetén az alkalmazás használhatja a biztonsági másolat várólista mindaddig, amíg az elsődleges régió elérhetővé újra. Úgy, hogy az alkalmazás továbbra is az új kérések is feldolgozása.  

###  <a name="documentdb"></a>DocumentDB 

- **Az adatbázis bizonyos területek között.** Több területre fiókkal a DocumentDB adatbázis tartalmaz, egy írási terület és a több olvasási területre. Ha a írási régióban hiba, egy másik kópiából erről. Az ügyfél SDK kezeli meg automatikusan. Az írás régió egy másik területére keresztül is sikertelen. További tudnivalókért lásd: az [adatok elosztás globálisan DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL-adatbázis 

- **Normál vagy prémium réteg használja.** A réteg adja meg, hogy már pont és az idő visszaállítása ponttal (35 nap). További tudnivalókért lásd: az [SQL-adatbázis beállítások és a teljesítmény](../sql-database/sql-database-service-tiers.md).

- **SQL-adatbázis naplózásának engedélyezése.** A naplózás használható diagnosztizálása rosszindulatú támadások vagy emberi hiba. További tudnivalókért olvassa el a [– első lépések az SQL-adatbázis Képletvizsgálat](../sql-database/sql-database-auditing-get-started.md)című témakört. 

- **Használja az Active Geo replikációs** Aktív Geo replikációs használatával hozzon létre egy olvasható másodlagos egy másik régióbeli.  Ha nem sikerül az elsődleges adatbázist, vagy egyszerűen csak meg kell offline állapotban, végezze el a manuális áttérni a másodlagos adatbázis.  Nem sikerül fölé, amíg a másodlagos adatbázis marad, csak olvasható.  További tudnivalókért lásd: az [SQL adatbázis aktív Geo-replikáció](../sql-database/sql-database-geo-replication-overview.md). 

- **Használat sharding**. Fontolja meg az adatbázis vízszintesen partition sharding használatát. Megadhatja, hogy az sharding hibafa elkülönítési. További tudnivalókért olvassa el a [Méretezés ki az Azure SQL-adatbázis](../sql-database/sql-database-elastic-scale-introduction.md)című témakört. 

- **Az emberi hiba visszaállításához használandó pont és az idő visszaállítása.**  Pont és az idő visszaállítása adja vissza az adatbázis egy korábbi állapotba idő. További tudnivalókért lásd: [az Azure SQL-adatbázisokkal automatikus adatbázis biztonsági mentése helyreállítása][sql-restore].

- **A szolgáltatás üzemszünetek lévő visszaállításához használandó geo-visszaállítása.** GEO-visszaállítja adatbázis geo felesleges biztonsági másolatból.  További tudnivalókért lásd: [az Azure SQL-adatbázisokkal automatikus adatbázis biztonsági mentése helyreállítása][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (egy virtuális fut)

- **Az adatbázis replikáció.** Az SQL Server mindig a elérhetősége csoportok használatával az adatbázis. Ha nem sikerül egy SQL Server-példányt, itt magas elérhetősége. További tudnivalókért lásd: [További információ...](guidance-compute-n-tier-vm.md) 

- **Az adatbázis biztonsági mentése**. Ha már használ [Azure biztonsági másolatot](https://azure.microsoft.com/documentation/services/backup/) készítsen biztonsági másolatot a VMs, fontolja meg [Azure biztonsági mentése az SQL Server-munkaterhelésekből DPM használatával](../backup/backup-azure-backup-sql.md). Az eljárás nem a szervezet egy biztonsági rendszergazdai szerepkört, és egy egyesített helyreállítás VMs és az SQL Server. [Az SQL Server felügyelt biztonsági mentése Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx)ellenkező esetben használja. 


###  <a name="traffic-manager"></a>Adatforgalom Manager 

- **Végezze el a manuális visszaállás.** A forgalom Manager feladatátvétel után végezze el a manuális visszaállás helyett automatikusan adatkapcsolat vissza. Vissza nem működnek, előtt győződjön meg róla, hogy az összes alkalmazás alrendszerek megfelelő.  Egyéb esetben hozhat létre olyan helyzet, ha az alkalmazás tükrözésekor másikba adatközpontokban között. További tudnivalókért lásd: az [Operációs rendszert futtató VMs több régióban](guidance-compute-multiple-datacenters.md).

- **A rendszerállapot vizsgálati végpont létrehozása**. Hozzon létre egy egyéni végpontot, hogy az alkalmazás általános állapotát jelző jelentések. Ez a forgalom Manager átadni, ha minden kritikus út nem sikerül, nem csak az előtér lehetővé teszi. A végpont térjen vissza egy HTTP kódszámú hiba jelenik meg, ha bármelyik kritikus függőség a sérült vagy nem érhető el. Nem hibajelentés az nem kritikus szolgáltatást, akkor jó helyen jár. Egyéb esetben a rendszerállapot vizsgálati feladatátvételt válthat nincs szükség esetén, téves létrehozása. További tudnivalókért lásd: a [forgalom Manager végpont figyelése és feladatátvételi](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuális gépeken futó 

- **Lehetőleg ne futtasson egy gyártási terhelést a egy egyetlen virtuális.** Egy egyetlen virtuális telepítés esetén nem rugalmassá a tervezett vagy nem tervezett karbantartás. Ehelyett akkor több VMs elérhetőségének beállítása vagy a virtuális skála megadása a előtt terheléselosztó. Hogy jogosult legyek az SZOLGÁLTATÁSISZINT-, legalább két virtuális gépeken futó belüli elérhetősége mindazon kell telepítését. További információ a [Virtuális gép skála készletek áttekintése](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)című témakörben találhat. 

- **Adja meg, hogy be, amikor Ön a virtuális kiépítése elérhetőségét.** Jelenleg nem tudja, hogy egy erőforrás-kezelő virtuális hozzáadása egy után a virtuális már kiépítve elérhetőségét. Amikor felvesz egy új virtuális egy meglévő elérhetővé beállítása, ügyeljen arra, hogy hozzon létre egy hálózati kártya a virtuális, és a hálózati kártya felvétele a háttéradatbázist cím gyűjtő a terheléselosztó. Egyéb esetben a terheléselosztó nem irányítja a forgalmat, hogy virtuális. 

- **Egyes alkalmazások réteg üzembe külön elérhetőségének beállítása.** Az N szintű alkalmazásokban nem kell helyezni VMs a különböző rétegek elérhetősége mindazon. Egy elérhetőségének beállítása VMs hibafa tartományokban (FDs) kerülnek, és frissítse a domains (UD). Azonban redundancia élvezheti a FDs és UDs, hogy minden virtuális elérhetősége megadása kell tennie az azonos ügyfél kérések kezelheti. 

- **Válassza a teljesítmény igényeknek megfelelően alakítható jobb virtuális méretét.** Amikor egy meglévő terhelést Azure helyez át, indítsa el a virtuális mérete, amely a legközelebb áll a helyszíni kiszolgálókhoz. Mérje le a tényleges terhelést részletez Processzor, a memória és a lemez IOPS teljesítményének, majd módosítsa a méretét, ha szükséges. Ez segít megelőzni, hogy az alkalmazás a vártnak felhőalapú környezetben úgy viselkednek. Ha több van szüksége, is érdemes szem előtt tartania minden méretét a hálózati kártya korlátját. 

- **Használja a prémium tároló rendszerindításának.** Azure prémium tároló nagy teljesítményű, alacsony-késés lemezt támogatja. További tudnivalókért lásd: [prémium tároló: Azure virtuális gép Munkaterhelésekből nagy teljesítményű tárterület](../storage/storage-premium-storage.md) válassza ki a virtuális méretét, amely támogatja a prémium tároló. 

- **Külön tárterület-fiók létrehozása az egyes virtuális.** Helyezze a VHD egy virtuális külön tárterület-fiókba. Ezzel az elemzéssel szerezze meg a tárterület-fiókok IOPS korlátozások elkerülése érdekében. További tudnivalókért lásd: [Azure tároló méretezhetőség és a teljesítmény célok](../storage/storage-scalability-targets.md). Azonban VMs nagyszámú telepít, ha a felhasználónevek tároló fiókok előfizetés korlátját. [Tárterületkorlátok](../azure-subscription-service-limits.md#storage-limits)látható.

- **A diagnosztikai naplók külön tároló fiók létrehozása**. A diagnosztikai naplók nem ugyanazzal a tárterület-fiókkal, a VHD zsugorítása a diagnosztikai naplózás befolyásolja a virtuális lemezt a IOPS írni. Helyi meghajtóra felesleges tároló (LRS) fióktulajdonos elegendő a diagnosztikai naplók. 

- **Alkalmazások telepítése adatok lemezen, nem az operációs rendszer lemezen.** Előfordulhat, hogy elér ellenkező esetben a lemez méretkorlátot. 

- **Azure biztonsági másolatot készítsen biztonsági másolatot VMs.** Biztonsági másolatok véletlen adatvesztés elleni védelem. További tudnivalókért lásd: [Azure VMs védelme a egy helyreállítási szolgáltatások tárolóból elemre](../backup/backup-azure-vms-first-look-arm.md).

- **A diagnosztikai naplók engedélyezése**, például egyszerű állapot mértékek, infrastruktúra naplók és [indítási diagnosztika][boot-diagnostics]. Indítási diagnosztika segíthetnek diagnosztizálása indítási hibát, ha nem indítható állapotba lekéri a virtuális. További tudnivalókért lásd: [Áttekintés az Azure a diagnosztikai naplók][diagnostics-logs].

- **A AzureLogCollector bővítmény használatára**. (Windows VMs csak.) Erre a mellékre Azure platform naplók összesíti, és nem távolról bejelentkezés a virtuális szereplő Azure-tárolóhoz feltölti őket. További tudnivalókért olvassa el a [AzureLogCollector bővítmény](../virtual-machines/virtual-machines-windows-log-collector-extension.md)című témakört.


###  <a name="virtual-network"></a>Virtuális hálózati 

- **Whitelist vagy blokk nyilvános IP-címek hozzáadása egy NSG alhálózathoz.** Rosszindulatú felhasználók hozzáférésének letiltása, vagy a hozzáférés engedélyezése csak a felhasználók, akik az alkalmazás jogosultsága van.  

- **Hozzon létre egy egyéni állapot vizsgálati.** HTTP vagy a TCP-tesztelheti a betöltés terheléselosztó állapot ellenőrzi. A virtuális HTTP-kiszolgálón fut, a HTTP-vizsgálati esetén egy jobb jelölő állapota állapotának mint a TCP-vizsgálati. Egy HTTP vizsgálati egy egyéni végpontot, hogy az alkalmazás, beleértve minden kritikus függőség általános állapotának jelentések használata. További információ az [Azure terheléselosztó áttekintése](../load-balancer/load-balancer-overview.md)című témakörben találhat.

- **Az állapot vizsgálati nem blokkolja.** A betöltés terheléselosztó állapot vizsgálati ismert IP-címet, 168.63.129.16 küldi. Nem blokkolja a forgalmat, illetve az IP-házirendek tűzfal vagy a hálózati biztonsági csoport (NSG) szabályok a. Az állapot vizsgálati blokkolása okozhat a terheléselosztó, ha el szeretné távolítani a virtuális Forgatás. 

- **Naplózás engedélyezése terheléselosztó.** A naplók hány VMs megjelenítése a háttéradatbázist nem kapnak miatt sikertelen vizsgálati válaszokat a hálózati forgalmának engedélyezésére. További tudnivalókért lásd: [Azure terheléselosztó napló elemzésének](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
