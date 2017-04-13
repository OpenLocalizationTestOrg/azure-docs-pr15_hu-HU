<properties
   pageTitle="Állapot ellenőrzése szolgáltatás háló |} Microsoft Azure"
   description="Az Azure Service háló rendszerállapot figyelése a modell, amely a fürt alkalmazások és szolgáltatások felügyelete bemutatása."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="introduction-to-service-fabric-health-monitoring"></a>Bevezetés a szolgáltatás háló rendszerállapot figyelése
Azure Service háló állapot modell, amely biztosít a multimédiás rugalmas és bővíthető állapot értékelése és a jelentéskészítő vezet be. A modell lehetővé teszi, hogy a közel-valós idejű nyomon követése a fürt és a benne lévő futó szolgáltatások állapotát. Egyszerű állapot információk beszerzése és javítása az iskolák esetleges kaszkádoltak, míg a tömeges kimaradások okozó problémák. A szokásos modell szolgáltatások küldeni a jelentések helyi véleményüket alapján, és az, hogy a információk megadására átlagos összesíti fürt szintű megtekintése.

Szolgáltatás háló összetevőket a multimédiás állapot modell segítségével jelentést az aktuális állapotát. Is használhatja ugyanazt a mechanizmusa jelentés állapota alkalmazásából. Kiváló minőségű állapota, amely jellemzi a egyéni feltételek jelentéskészítés fektetni, akkor ha észleli, és az aktív alkalmazás sokkal egyszerűbb problémák megoldása.

> [AZURE.NOTE] Az állapot alrendszer cím felügyelt frissítések szükséges lépések azt. Szolgáltatás háló biztosít, amelyek a teljes állásának, nincs leállás felügyelt alkalmazás és a fürt frissítések és a minimális felhasználói beavatkozás nélkül szeretné. A kitűzött célok eléréséhez a frissítés állapot alapján frissítési házirend konfigurálva, és lehetővé teszi, hogy folytassa, csak ha állapot megőrzi a kívánt küszöbértékek frissítésének ellenőrzi. Egyéb esetben a frissítés automatikusan visszaállítja vagy adjon rendszergazdák a problémák megoldása a névjegyadatok fel van függesztve. Többet szeretne tudni az alkalmazás frissítése, hogy [ismertető](service-fabric-application-upgrade.md)témakört is.

## <a name="health-store"></a>Állapot áruházból
Az állapot tároló szervezetek állapot kapcsolatos információ a partnerek és a kiértékelés fürt továbbra is. Egy szolgáltatás háló állandó biztosítja a jó elérhetőség és méretezhetőség állapot-nyilvántartó szolgáltatást, akkor lép érvénybe. Az állapot tároló része a **háló: / rendszer** alkalmazást, és érhető el, ha a csoportját fel és működik.

## <a name="health-entities-and-hierarchy"></a>Állapot személyek és a hierarchia
A rendszerállapot személyek egy logikai hierarchiája, amely jellemzi a kapcsolati és más szervezetek közötti függőségek vannak rendezve. A személyek és a hierarchia automatikusan készültek az állapot üzlet szolgáltatás háló összetevők kapott jelentések alapján.

A rendszerállapot személyek tükrözése a szolgáltatás háló személyek. (Például **Állapot Alkalmazás egyed** megfelel egy alkalmazás példány telepítve a fürt közben **állapota csomópontra-szervezet** megfelelő szolgáltatás háló csomópont.) Az állapot hierarchia rögzíti a rendszer szervezetek interaktív műveletet, és speciális állapot kiértékelés alapját. [Technikai áttekintés szolgáltatás háló](service-fabric-technical-overview.md)alapfogalmak szolgáltatás háló megismerheti. Alkalmazás bővebben lásd: [szolgáltatás háló alkalmazásmodell](service-fabric-application-model.md).

Az állapot személyek és a hierarchia lehetővé tenni a fürt és hatékonyan jelenteni, indítja, és figyeli alkalmazásokat. A rendszerállapot modell a fürt hány mozgó darab állapotának pontos, *részletes* ábrázolása biztosít.

![Állapot szervezetek.][1]
A rendszerállapot entitás alapján a szülő-gyermek kapcsolatok hierarchia vannak rendezve.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

A rendszerállapot személyek a következők:

- **Fürt**. A szolgáltatás háló fürtre állapotának jelöli. Fürt állapotjelentések feltételeket, amelyek befolyásolják a teljes fürt, és az egy vagy több sérült gyermekek nem maradt ismertetik. A hálózati szétválasztás vagy kommunikációs problémák miatt felosztása fürt az agy többek között.

- **Csomópontot**. A szolgáltatás háló csomópont állapotának jelöli. Csomópont állapotjelentések befolyásolja a csomópont funkciókat feltételek ismertetik. A szokásos befolyásolják, futó telepített szervezetek. Többek között csomópont ki a szabad terület (vagy egy másik gépi szintű tulajdonság, például a memóriahasználat, a kapcsolatok) van, illetve egy csomópont nem működik. A csomópont entitás azonosítja a csomópont nevét (karakterlánc).

- Az **alkalmazás**. Az állapot, az alkalmazás-példányok futtatása a fürt jelöli. Alkalmazás állapotjelentések írja le az alkalmazás általános állapotának befolyásoló feltételeket. Ezek nem lehet maradt meg az egyes gyermekek (services vagy a telepített alkalmazások). Többek között a végpontok közötti kölcsönhatás különböző szolgáltatások között az alkalmazásban. Az alkalmazás egyed azonosítja az alkalmazás nevét (URI).

- A **szolgáltatás**. A fürt futó szolgáltatás állapotát jelző jelöli. Szolgáltatás állapotjelentések ismertetik, amelyek befolyásolják a szolgáltatás állapotát jelző általános feltételek, és ezek nem kell maradt meg egy partíciót vagy kópia. A szolgáltatás konfiguráció (például a port vagy külső fájlmegosztás), amely az összes partíciót problémákat okoz a többek között. A szolgáltatás entitás azonosítja a szolgáltatás neve (URI).

- **Partition**. A szolgáltatás partíciót állapotának jelöli. Partition állapotjelentések feltételek, a teljes kópiakészlet befolyásoló ismertetik. Ha kópiák száma alatti cél száma, és amikor a partíciók kvórumvesztést például. A partíciók entitás azonosítja a partíciót azonosító (GUID).

- **Replika**. Állapot-nyilvántartó szolgáltatás kópia vagy egy állapot nélküli szolgáltatás példány állapotának jelöli. A legkisebb egység watchdogs és a rendszer összetevőit nyilvántartástól az alkalmazáshoz. Az állapot-nyilvántartó szolgáltatások, többek között jelentéskészítés, amikor azt nem műveletek formátumú másodlagos zónák, és amikor a replikáció nem a várt eljárás való replikáció elsődleges kópia tempóban. Egy állapot nélküli példány jelentheti is, amikor fogy a források, vagy a csatlakozási problémákat tapasztal van. A replika entitás azonosítjuk a azonosító (GUID) és a kópia vagy példány Azonosítót (hosszú).

- **DeployedApplication**. Egy *alkalmazást futtató csomóponton*állapotának jelöli. A telepített alkalmazások állapotjelentések alkalmazásával meghatározott feltételek a csomópontra, ugyanazon a csomóponton rendszerbe szolgáltatás csomagokra nem maradt ismertetik. Amikor az alkalmazás-csomag nem tölthető le, a csomóponton és az alkalmazás rendszerbiztonsági a csomóponton probléma beállítása esetén például. A telepített alkalmazások azonosítjuk alkalmazás (URI) és csomópont nevét (karakterlánc).

- **DeployedServicePackage**. A fürt futó szolgáltatás csomag állapotának jelöli. Azt ismerteti, hogy a feltételnek szolgáltatás csomag alkalmazásra nincs hatással a többi szolgáltatás csomagot az azonos alkalmazáshoz ugyanazon a csomóponton. Többek között a kód csomag a szolgáltatás, amely nem indítható el és a konfigurációs csomagja, amely nem tudja olvasni a. A telepített szolgáltatást csomag azonosítjuk alkalmazás nevét (URI), a csomópont nevét (karakterlánc) és a szolgáltatás nyilvánvalóan neve (karakterlánc).

A rendszerállapot modell granularitása egyszerűen észleli és a problémák megoldása. Például a szolgáltatás nem válaszol, érdemes lehet azt jelzi, hogy az alkalmazás-példány megsérült. Jelentéskészítés, hogy szint a nem ideális, akkor jó helyen jár, mert a probléma előfordulhat, hogy nem kell érintő belül az alkalmazást a szolgáltatásokat. Ha adott partíciót mutat további információt a jelentést a sérült szolgáltatásba vagy egy adott gyermek partíciót kell alkalmazni. Az adatok automatikusan felületeken keresztül a hierarchia, és egy sérült partíciót lett látható szolgáltatások és alkalmazások szinten. Ez az összesítés segítséget nyújt a pinpoint és gyorsabban megoldani a legfelső szintű a probléma okát.

A rendszerállapot hierarchia szülő-gyermek kapcsolatok áll. A fürtre csomópontok és az alkalmazások tevődik össze. Alkalmazások-szolgáltatásokkal, és a telepített alkalmazások. A telepített alkalmazások szolgáltatás csomagok telepítette. Szolgáltatások rendelkeznek partíciót, és az egyes partíciók egy vagy több kópiák. Speciális kapcsolat csomópontok és telepített szervezetek között van. Ha az megsérült, ahogyan azt hozzá hitelesítésszolgáltató rendszer összetevője (Feladatátvevő-kezelő szolgáltatás) csomópontot, befolyásolja a telepített alkalmazások, a szolgáltatás csomagok és a kópiák telepítve rajta.

Az állapot hierarchia a legújabb állapotát, a rendszer a legújabb állapotjelentések alapján, amely olyan, szinte valós idejű adatokat jelöli.
Belső és külső watchdogs nyilvántartástól az azonos szervezetek, az alkalmazás-specifikus logika vagy egyéni felügyelt feltételek alapján. Felhasználói jelentések rendszer jelentések megtalálhatók.

Tervezze meg a jelentést, és állapota megválaszolása könnyebb a szolgáltatás hibakeresési, figyelésére és működik egy nagyméretű felhőszolgáltatásba tervezése során fektetni.

## <a name="health-states"></a>Állapot államok
Szolgáltatás háló ahhoz, hogy egy szervezet megfelelő-e vagy sem használja a három állapot állapota: az OK gombra, figyelmeztetés és hiba. Bármelyik jelentés az állapot tároló küldött államok egyik kell adnia. A rendszerállapot kiértékelés eredménye államok egyik.

A lehetséges [állapotok](https://msdn.microsoft.com/library/azure/system.fabric.health.healthstate) a következők:

- **OK**. A szervezet megfelelő. Nincs a hozzá tartozó gyermekwebhelyek (ha van) vagy a jelentett ismert problémák lépnek fel.

- **Figyelmeztetés**. A szervezet problémák elhárításához találkozik, de azt még nem sérült (például késések, de ne okozzanak működési problémák megoldásához még). Bizonyos esetekben a figyelmeztetés feltétel speciális beavatkozás nélküli szabhat magát, és hasznos lehet ahhoz, hogy a szükséges betekintést kap abba, hogy mi legyen a történésekkel kapcsolatban. Egyéb esetben a figyelmeztetés feltétel csökkenhet a felhasználói beavatkozás nélkül nagyon gyenge hálózati minőség probléma be.

- A **hiba**. A szervezet megsérült. Művelet javíthatja az személy állapota kell tenni, mert nem működik megfelelően.

- **Ismeretlen**. A szervezet nem létezik a rendszerállapot áruházból. Ez az eredmény szerezhető be a megosztott lekérdezésekből egyesítheti több összetevők származó találatok jelenjenek meg. Ugrás az **FailoverManager** és **HealthManager**, get-alkalmazás lista lekérdezés ugrik **ClusterManager** és **HealthManager**csomópont lista lekérdezés például kaphat. Ezeket a lekérdezéseket egyesítheti több rendszer összetevőit származó találatok jelenjenek meg. Ha egy másik rendszer összetevője nem ért, vagy a Szolgáltatásállapot áruházból eltávolította entitás ad vissza, az egyesített eredmény van-e az ismeretlen rendszerállapot.

## <a name="health-policies"></a>Állapot házirendek
Az állapot tároló állapot házirendek állapítsa meg, hogy a szervezet megfelelő a jelentések és a hozzá tartozó gyermekwebhelyek alapján vonatkozik.

> [AZURE.NOTE] Állapot házirendek a fürt jegyzék (értékeléséhez fürt- és állapot) vagy az alkalmazás jegyzék (az alkalmazás értékelése és a hozzá tartozó gyermekwebhelyek bármelyikét) adható meg. Állapot kiértékelés kérések is átadhatja az egyéni állapot kiértékelés házirendek, amelyet csak az adott kiértékelés.

Alapértelmezés szerint a szolgáltatás háló szigorú szabályainak (minden kell lennie megfelelő) a szülő-gyermek hierarchikus kapcsolat vonatkozik. Ha egy sérült esemény van még a gyermekek közül, a szülő sérült számít.

### <a name="cluster-health-policy"></a>Fürt állapot házirend
A rendszerállapot fürt és csomópont állapotok ki szeretné számítani a [fürt állapot házirend](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.aspx) használják. A házirend a fürt jegyzék definiálható. Ha nincs megadva, az alapértelmezett házirend (nulla megengedett hibák) használják.
Fürt házirend tartalmazza:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.considerwarningaserror.aspx). Itt adhatja meg, hogy kezelni a figyelmeztetés állapot jelentések hibaként állapot értékelés során. Alapértelmezett: hamis.

- [MaxPercentUnhealthyApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications.aspx). Itt adhatja meg az alkalmazást, amely sérült, mielőtt a fürt számít hiba megengedett maximális százalékos egységarányt.

- [MaxPercentUnhealthyNodes](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes.aspx). Adja meg a csomópontok, amely sérült, mielőtt a fürt számít hiba megengedett maximális százalékos egységarányt. Nagy fürt néhány csomópontok is mindig lefelé vagy ki a javításához, így ezt a százalékos értéket, amely elviselni kell konfigurálni.

- [ApplicationTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap.aspx). Az alkalmazás típusú állapot házirend térkép speciális alkalmazás típusának leírására fürt állapot értékelés során használható. Alapértelmezés szerint az összes alkalmazások erőforráskészlethez tartozik üzembe és a MaxPercentUnhealthyApplications kiértékelt. Ha bizonyos alkalmazás típusai eltérő kell kezelni, azok ki a globális készlet tehetők. Ehelyett kiértékelésének megfelelő százalékértéket a térkép alkalmazás típusú neveket társított szemben. Például a fürt vannak ezer különböző típusú alkalmazásokat, és néhány vezérlő szolgáltatásalkalmazás-példányok a speciális alkalmazás típusú. A vezérlő alkalmazások soha nem kell az a hiba. Megadhatja, hogy a globális MaxPercentUnhealthyApplications néhány hibák elviselni 20 %-át, de az alkalmazás type "ControlApplicationType" a MaxPercentUnhealthyApplications értéke 0. Ebben az esetben, ha a sok alkalmazások némelyike sérült, de a globális sérült százalékos alatt, a fürt volna ki kell értékelni figyelmeztető. Állapot állapot nincs hatással fürt frissítés, vagy hiba rendszerállapot figyelése más indított. De akár egy vezérlőelem-alkalmazás hiba fürt sérült, amely elindítja visszaállítása vagy felfüggeszti a fürt frissítés, attól függően, hogy a frissítés konfigurálása tenné.
Az alkalmazás-típusok érvényes leképezést szolgáltatásalkalmazás-példányok ki az alkalmazások globális készlete venni. Az alkalmazás típusú, az adott MaxPercentUnhealthyApplications leképezés használata alkalmazások száma alapján kiértékelésének. Az alkalmazás a többi a globális készletben és MaxPercentUnhealthyApplications kiértékelése történik.

Az alábbi példában egy olyan fürt jegyzék kivonat. Az alkalmazás típusú megfeleltetés bejegyzések megadásához előtag a paraméter neve "ApplicationTypeMaxPercentUnhealthyApplications-", az alkalmazás neve követ.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Alkalmazás-állapot házirend
Az [alkalmazás-állapot házirend](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.aspx) ismerteti, hogyan események és a gyermek-államok összesítési értékelése az alkalmazások és a gyermekek befejeződött. Az alkalmazás jegyzék, **ApplicationManifest.xml**, az alkalmazás csomagban definiálható. Ha nincs házirendek nincs megadva, szolgáltatás háló feltételezi, hogy a szervezet sérült, ha egy állapotjelentés vagy egy Gyermekszint a figyelmeztetés vagy hibaüzenet rendszerállapot a.
A konfigurálható házirendek a következők:

- [ConsiderWarningAsError](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.considerwarningaserror.aspx). Itt adhatja meg, hogy kezelni a figyelmeztetés állapot jelentések hibaként állapot értékelés során. Alapértelmezett: hamis.

- [MaxPercentUnhealthyDeployedApplications](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications.aspx). Adja meg a telepített alkalmazások, amely sérült előtt az alkalmazás hibát számít megengedett maximális százalékos egységarányt. Ez a százalékérték fölé, hogy az alkalmazás éppen az üzembe helyezése a a fürt csomópontok számának sérült telepített alkalmazások számát elosztjuk. A számítási elviselni egy hiba csomópontok kisebb számok felfelé kerekít. Alapértelmezett százalékos: nulla.

- [DefaultServiceTypeHealthPolicy](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy.aspx). Itt adhatja meg az alapértelmezett szolgáltatás típusa állapot szabályt, amely az alkalmazás az összes szolgáltatás alapértelmezett házirend lecseréli.

- [ServiceTypeHealthPolicyMap](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap.aspx). A szolgáltatás állapota házirendek szolgáltatást típusonként térképe biztosít. Ezek a házirendek az alapértelmezett szolgáltatás típusa állapot házirendek az egyes megadott szolgáltatás: helyére. Például ha egy alkalmazást egy állapot nélküli átjáró szolgáltatás típusa, és az állapot-nyilvántartó motor szolgáltatás típusa, beállíthatja a kiértékelés az állapot házirendeket másképp. Miután megadta a házirend-szolgáltatás típusonként, kaphatnak a pontosabban szabályozhatja a szolgáltatás állapotát jelző a.

### <a name="service-type-health-policy"></a>Szolgáltatás típusa állapot házirend
A [szolgáltatás típusa állapot házirend](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.aspx) meghatározza az értékelése és a szolgáltatások és -szolgáltatások gyermekeinek összesítése. A házirend tartalmazza:

- [MaxPercentUnhealthyPartitionsPerService](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice.aspx). Itt adhatja meg a sérült partíciót megengedett maximális százalékos egységarányt, mielőtt számít, hogy a szolgáltatás megsérült. Alapértelmezett százalékos: nulla.

- [MaxPercentUnhealthyReplicasPerPartition](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition.aspx). Adja meg a sérült kópiák megengedett maximális százalékos egységarányt, előtt partíciót sérült számít. Alapértelmezett százalékos: nulla.

- [MaxPercentUnhealthyServices](https://msdn.microsoft.com/library/azure/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices.aspx). Adja meg a sérült szolgáltatások megengedett maximális százalékos egységarányt, mielőtt számít, hogy az alkalmazás megsérült. Alapértelmezett százalékos: nulla.

Az alábbi példában egy olyan alkalmazás jegyzék kivonat:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Állapot értékelése
Felhasználók és az automatizált szolgáltatások bármikor tudja kiértékelni bármely entitás állapota. Egy adott személy állapota értékeléséhez, az állapot áruházból összesítések összes állapot jelentéseket a szervezet és értékeli ki minden hozzá tartozó gyermekwebhelyek (ha alkalmazható). Az állapot összesítési algoritmus állapot házirendek, adja meg, hogyan kell kiértékelni állapotjelentések és összesítéséhez gyermek állapot államok (ha alkalmazható) módját használja.

### <a name="health-report-aggregation"></a>Állapot jelentés összesítése
Egy személy beállíthatja, hogy több állapotjelentések különböző közötti (rendszer összetevőit vagy watchdogs) különböző tulajdonságok által küldött. Az összesítés különösen használja a a társított állapot házirendeket, alkalmazása vagy fürt állapot házirend ConsiderWarningAsError tagja. ConsiderWarningAsError Itt adhatja meg, hogyan figyelmeztetések ki szeretné számítani.

A összesített rendszerállapot a *legrosszabb* állapotjelentések, a entitás váltja ki. Ha legalább egy hiba állapotjelentés, a összesített rendszerállapot program hibát.

![Hibákról szóló jelentéseket a állapota a jelentés összesítést.][2]

A rendszerállapot entitás, amely tartalmaz egy vagy több hibát állapotjelentések hibaként kiértékeli. Ugyanez igaz a lejárt állapotjelentés, függetlenül attól, az állapot állapotába.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Ha nincs hibajelentések és egy vagy több figyelmeztetések, a összesített rendszerállapot figyelmeztetés vagy a hiba, attól függően, hogy a ConsiderWarningAsError házirend jelölőre.

![Állapot jelentés összesítési figyelmeztetés jelentés és ConsiderWarningAsError hamis.][3]

Állapot jelentés összesítési figyelmeztetés jelentés és ConsiderWarningAsError (alapértelmezett) hamis értékre van állítva.

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Child állapot összesítése
Egy személy összesített állapotának tükrözi, a gyermek állapot államok (ha alkalmazható). A gyermek állapot államok összesítése algoritmust használja az állapot házirendek alkalmazandó entitás függően.

![Gyermek szervezetek állapot összesítést.][4]

Alárendelt összesítő állapot házirendek alapján.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Az állapot tároló kiértékelése az alárendelt kifejezésekkel, miután sérült gyermekek konfigurált maximális százalékos egységarányt alapján állapot állapotukat összesíti azt. Ez a százalék a személy és a gyermek függően házirend származik.

- Az összes alárendelt kifejezésekkel OK Államokban van, a gyermek összesíti rendszerállapot esetén az OK gombra.

- Ha gyermekek OK és a figyelmeztetés Államokban van, a gyermek összesíti rendszerállapot figyelmeztetés van.

- Gyermekek veszi figyelembe a maximális százalékos aránya a sérült gyermekek hiba állapotok esetén a összesített rendszerállapot program hibát.

- Ha a hiba államok, a gyermekek tiszteletben sérült gyermekek maximális százaléka, a összesített rendszerállapot figyelmeztetés van.

## <a name="health-reporting"></a>Állapot jelentése
Rendszer-összetevők, a rendszer háló alkalmazások és a belső és külső watchdogs jelentheti szolgáltatás háló szervezetek szemben. A közötti ellenőrizze a megfigyelt személyek, a megadott feltételeknek megfelelő figyelése azok állapotát jelző *helyi* meghatározását. Globális állam, illetve összesített adatok vizsgálata nem szükséges. A kívánt működése egyszerű közötti és nem összetett szervezetek, tekintse meg számos dolgot előállítani alapján kapcsolatos információk küldése kell rendelkeznie.

A rendszerállapot áruház egészségügyi adatok elküldéséhez a reporter kell a szóban forgó entitás azonosítása és állapot-jelentés létrehozása. A jelentés ezután lehet küldeni a API [FabricClient.HealthClient.ReportHealth](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient_members.aspx)használatával, Powershellen keresztül, vagy többi keresztül.

### <a name="health-reports"></a>Állapotjelentések
Az egyes a szervezetek a fürt [állapotjelentések](https://msdn.microsoft.com/library/azure/system.fabric.health.healthreport.aspx) az alábbi információkat tartalmazzák:

- **SourceId forrásazonosító**. Egy karakterlánc, amely az állapot esemény bejelentő egyedileg kell azonosítania.

- A **szervezet azonosítója**. A szervezet a jelentés alkalmaztunk azonosítja. [Írja be a szervezet](service-fabric-health-introduction.md#health-entities-and-hierarchy)alapján különbözik:

  - A fürt. Nincs lehetőség.

  - Csomópontot. Csomópont nevét (karakterlánc).

  - Az alkalmazás. Alkalmazás nevét (URI). Az alkalmazás a fürt telepített példányának nevét jelenti.

  - A szolgáltatás. A szolgáltatás neve (URI). A szolgáltatás példány telepítve a fürt nevét jelenti.

  - Partition. Partition azonosítója (GUID). Partition egyedi azonosítója jelöl.

  - Kópia. Az állapot-nyilvántartó szolgáltatás kópia vagy az állapot nélküli szolgáltatás példány azonosítója (INT64).

  - DeployedApplication. Az alkalmazás neve (URI) és a csomópont nevét (karakterlánc).

  - DeployedServicePackage. Az alkalmazás neve (URI), a csomópont nevét (karakterlánc) és a szolgáltatás cikkét (karakterlánc) nevet.

- A **tulajdonságot**. *Karakterlánc* (nem rögzített számbavétel), amely lehetővé teszi, hogy az állapot esemény egy adott tulajdonság entitás kategorizálásához bejelentő. Ha például reporter A jelentést küldhet a Node01 "tároló" tulajdonságot, és a reporter B állapotának jelentést küldhet a Node01 "kapcsolódási" tulajdonság állapotának. Az állapot áruházban ezeket a jelentéseket a Node01 entitás külön állapot események számít.

- **Leírás**. Egy karakterlánc, amely lehetővé teszi a részletes információkat az állapot esemény reporter. **SourceId forrásazonosító**, **tulajdonság**és **HealthState** teljesen illik a jelentést. A leírás hozzáadása a jelentés adatai olvasható. A szöveg megkönnyíti a rendszergazdák és a felhasználók az állapotjelentés megértéséhez.

- **HealthState**. Egy [számbavétel](service-fabric-health-introduction.md#health-states) , amely bemutatja állapotának a jelentést. Az elfogadott értékei az OK gombra, a figyelmeztetés és a hiba.

- **Élettartam**. Egy időszak, amely jelzi, hogy mennyi ideig tartott a állapotjelentés érvényes. **RemoveWhenExpired**kapcsolódik ahhoz, lehetővé teszi az állapot tároló tudni, hogy miként lejárt események ki szeretné számítani. Alapértelmezés szerint a értéke végtelen, és a jelentés érvényes örökkévalóság.

- **RemoveWhenExpired**. Logikai érték. Ha igaz, a lejárt állapotjelentés a rendszer automatikusan törli az állapot áruházat, és a jelentés nem hatással lehet entitás állapot kiértékelés. A jelentés érvényes idő csak az adott időszakra vonatkozóan, és nem szükséges bejelentő kifejezetten ürítse használ. Jelentések törlése az állapot áruházból is használják (például egy figyelő módosul, és leállítja a jelentések az előző forrás- és tulajdonság küldése). Elküldheti a egy rövid élettartam együtt RemoveWhenExpired be minden olyan előző állapotát az állapot áruházból törölje a jelentést. Ha az érték hamis értékre van állítva, a lejárt jelentés állapot értékeléséhez hibát kell kezelni. A hamis érték azt jelzi az állapot Store, hogy a forrás jelentse rendszeresen meg ezt a tulajdonságot. Ha nem, majd kell lennie egy hiba lépett fel a figyelő. A figyelő állapot rögzíthető az esemény hibaként figyelembe vételével.

- **– Sorszám –**. Pozitív egész számnak kell lennie, egyre növekvő, hogy, akkor a jelentések sorrendjének jelenti. Akkor használja az állapot tároló elavult jelentések miatt késések hálózati vagy egyéb problémák késői fogadott feltárása. Jelentés nem fogadja el, ha a sorrend értéke kisebb vagy egyenlő a leggyakrabban nemrégiben alkalmazza az azonos személy, a forrás és a tulajdonság szám. Ha nincs megadva, a sorszám automatikusan létrejön. Csak a állam áttűnések jelentésekor elhelyezni a sorszám szükség. Ebben az esetben a forrás van szüksége, ne feledje, hogy mely akkor küldi el a jelentések és a feladatátvevő helyreállítás lévő információkat.

E négy darab információk – SourceId forrásazonosító, a szervezet azonosítója, a tulajdonság és a HealthState – szükségesek minden állapotjelentés. A SourceId azonosító karakterlánc nem engedélyezett induljon az előtag "**rendszeren.**", a rendszer számára fenntartott jelentést. Ugyanaz a személy csak egy jelentést az azonos forrás- és a tulajdonság nincs. Felülbírálása jelentések forrás és a tulajdonságok egy vagy több, egymással, vagy az állapot ügyféloldalon (Ha a rendszer kötegekbe foglalja őket), vagy állapotát jelző tárolni, oldal. A csere alapuló sorszámok; újabb jelentések (amikor a magasabb sorszámok) régebbi jelentések helyére.

### <a name="health-events"></a>Állapot-események
Belső az állapot tároló továbbra is [állapot eseményeket](https://msdn.microsoft.com/library/azure/system.fabric.health.healthevent.aspx), a jelentések és további metaadatokat az összes adatot tartalmazó. A metaadatok tartalmaz, a jelentés az állapot ügyfélnek adott és a módosítás dátuma kiszolgálóoldalon idejét. Az állapot események [állapot lekérdezés](service-fabric-view-entities-aggregated-health.md#health-queries)által visszaadott.

A hozzáadott metaadatokat tartalmaz:

- **SourceUtcTimestamp**. Az idő a jelentésben megadott az állapot-ügyfél (egyezményes világidő).

- **LastModifiedUtcTimestamp**. Az idő, a jelentés a kiszolgálóoldalon (egyezményes világidő) utolsó módosítás dátuma.

- **IsExpired**. Jelző akár a jelentést, ha a lekérdezés végrehajtása az állapot üzlet lejárt. Esemény is járt le, csak akkor, ha RemoveWhenExpired hamis. Az esemény ellenkező esetben nem lekérdezés által visszaadott, és törlődik a áruházból.

- **LastOkTransitionAt**, **LastWarningTransitionAt**, **LastErrorTransitionAt**. Az OK gombra vagy hibaüzenettel áttűnések legutóbb. Ezeket a mezőket az esemény állam áttűnések adja meg a állapotát jelző előzményeit.

Az állapot áttűnés mezők végezhesse munkáját figyelmeztetések vagy a "korábbi" állapot adatai használható. Például: lehetővé teszik alkalmazási helyzetek:

- Ha egy tulajdonság, egy figyelmeztetés/hiba az X perc több, mint a riasztás. A feltétel, egy ideig ellenőrzése elkerülhető ideiglenes feltételek értesítéseket. Például ha a rendszerállapot van már figyelmeztetés öt percnél hosszabb értesítés el is értelmezhető (HealthState == figyelmeztető és -LastWarningTransitionTime > 5 perc).

- Csak a feltételeket, amelyek módosultak a legutóbbi riasztási X perc. Jelentés az adott időben előtt már a hibát, ha azt figyelmen kívül hagyhatja, mert már korábban lett adatbitjei.

- Ha egy tulajdonság, egy figyelmeztetés és hiba váltása van, állapítsa meg, mennyi ideig várjon sérült (Ez azt jelenti, hogy nincs rendben). Például egy értesítés, ha a tulajdonság még nem megfelelő öt percnél hosszabb el is értelmezhető (HealthState! = az OK gombra, és most - LastOkTransitionTime > 5 perc).

## <a name="example-report-and-evaluate-application-health"></a>Példa: Jelentést, és az alkalmazás állapot felmérése
Az alábbi példa az alkalmazás a Powershellen keresztül állapotjelentés küldése **háló: / WordCount** **MyWatchdog**forrásból. A rendszerállapot jelentés az állapot tulajdonság "elérhetősége" hiba állapotát, a végtelen élettartam állapotú információkat tartalmazza. Kattintson az alkalmazás állapot, mely eredménye összesíti állapot állam hibák és a jelentett állapot-események a listában, az állapot események kérdezi le.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Állapot modell használatát
A rendszerállapot modell lehetővé teszi, hogy felhőszolgáltatások és a mögöttes szolgáltatás háló platform, ha át kívánja méretezni, mivel a figyelés és állapot meghatározás meg vannak osztva a különböző monitorok a fürt belül.
Más rendszerekből egyetlen központi szolgáltatás, van az összes *esetleg* hasznos információt szolgáltatások által kibocsátott elemzi a fürt szintjén. Ez a módszer a méretezhetőség hátráltatja. Azt is nem teszi lehetővé, problémák és a lehető kiváltóok közelébe, a potenciális problémákra azonosításához nagyon konkrét információk összegyűjtéséhez.

A rendszerállapot modell erősen figyelemmel kísérésére és a diagnosztikai, kiértékelésének fürt és az alkalmazás és a megfigyelt frissítések használják. Más szolgáltatások egészségügyi adatok használatával végezze el az automatikus javítási fürt állapot előzmények összeállítása és értesítések bizonyos körülmények hiba.

## <a name="next-steps"></a>Következő lépések
[Szolgáltatás háló állapotjelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

[Hibaelhárításhoz rendszer állapotjelentések](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hogyan lehet jelenteni, és ellenőrizze a szolgáltatás állapota](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Adja hozzá egyéni szolgáltatás háló állapotjelentések](service-fabric-report-health.md)

[Figyelésére és szolgáltatások helyileg diagnosztizálása](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Szolgáltatás háló alkalmazás frissítése](service-fabric-application-upgrade.md)
