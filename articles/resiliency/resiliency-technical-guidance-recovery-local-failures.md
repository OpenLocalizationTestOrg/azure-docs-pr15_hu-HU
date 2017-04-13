<properties
   pageTitle="Technikai útmutató: helyi hibaelhárítás Azure-ban |} Microsoft Azure"
   description="Szóló ismertetése és rugalmas, könnyen hozzáférhető tervezése, hibafa tervezett alkalmazások, valamint csak a helyi hibák belül Azure vészhelyreállítás megtervezése."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-local-failures-in-azure"></a>Azure tűrőképessége technikai útmutató: helyi hibaelhárítás Azure-ban

Van két elsődleges veszélyek alkalmazás elérhetősége:

* A hibát az eszközök, például meghajtók és a kiszolgálón
* Kritikus erőforrások, például számítási csúcs betöltés körülmények kibocsátása

Azure biztosít az erőforrás-kezelés, rugalmasság, terheléselosztás és ahhoz, hogy magas elérhetőségének e körülmények szétválasztás kombinációi. Ezek a funkciók közül automatikusan megtörténik az összes Azure szolgáltatás. Jó helyen jár egyes esetekben az alkalmazás fejlesztője kell végeznie néhány további munka hasznát vehetik.

##<a name="cloud-services"></a>Cloud Services

Azure Cloud Services áll egy vagy több webes vagy dolgozó szerepkörök gyűjteménye. Egy vagy több példányának egy szerepkört futhat. A konfiguráció határozza meg, hogy a példányainak száma. Szerepkör-példányok figyeli és kezelhetők a háló vezérlő összetevőt. A háló vezérlő észleli, és automatikusan válaszol a szoftverek és a hardver hibáit.

Minden szerepkör-példány saját virtuális gép (virtuális) fut, és a háló ellenőrzi a vendégként való bekapcsolódáshoz ügynök keresztül kommunikál. A vendégként való bekapcsolódáshoz agent erőforrás- és mértékek, beleértve a virtuális használatát, állapot, naplók, erőforrás-kihasználtság, kivétel és hiba feltételek gyűjti össze. A háló vezérlő lekérdezi a vendégként való bekapcsolódáshoz agent konfigurálható időközönként, és újraindul a virtuális, ha nem válaszol a vendégként való bekapcsolódáshoz agent. Abban az esetben hardver a társított háló vezérlő minden érintett szerepkör-példányok áthelyezi egy új hardver csomópontot, és átállítja a hálózaton van a forgalom átirányítása.

Ezeket a szolgáltatásokat igénybe, a fejlesztők biztosítania kell, az összes szolgáltatás szerepkörök ne tárolja a szerepkör-példányok állam. Állandó az összes adat helyett, tartós tárhelyről, például Azure tároló vagy Azure SQL-adatbázis is elérhető. Ebben a csoportban adhatja bármely szerepkörök kérések kezelésére. Azt is jelenti, hogy szerepkör-példányok látogathat bármikor következetlenségekre létrehozása a szolgáltatás tranziens vagy állandó állapotban nélkül.

A külső felhasználókkal történő a szerepkörök állapot tárolási megkövetelésének több következmények tartalmaz. Ez azt jelenti, például, hogy az Azure tároló tábla kapcsolódó összes módosítás módosítani kell a egy egyed-csoport tranzakció, ha lehetséges. Természetesen nem mindig lehetséges, hogy minden módosítás egyetlen tranzakció. Meg kell tennie annak érdekében, hogy szerepkör példány hibák nem problémákat okozhatnak az azok szakítják meg hosszan futó műveleteket az állandó a szolgáltatás állapota két vagy több frissítések átterjedő külön figyelmet. Ha másik szerepkört megpróbál ilyen művelet megismétléséhez, meg kell várhatóan sok, és kezelheti a az esetben, ha részben befejezésének a munka.

Például érdemes megvizsgálni szolgáltatás, amely az adatok partíciók több üzlet keresztül. Ha egy dolgozó szerepkör megszakad, miközben meg van áthelyezése egy shard, a shard való áthelyezését nem befejeződhet. Vagy a áthelyezése egy másik dolgozó szerepkörönként, esetleg okoz a felesleges adatok vagy adatsérülés előfordulhat, hogy annak kezdete a kell ismételni. A problémák megelőzése érdekében hosszan futó műveleteket kell az alábbiak közül:

 * *Idempotent*: megismételhető, oldal effektusok nélkül. Ahhoz, hogy idempotent, egy hosszabb ideig futó művelet van, hogy ugyanazt az eredményt, függetlenül attól, hogy hány alkalommal program hajtsa végre, akkor is, ha azt megszakad a végrehajtás során.
 * *Fokozatosan újraindítható*: a legutóbbi helyétől hiba folytathatják. Ahhoz, hogy fokozatosan újraindítható, egy hosszabb ideig futó művelet kisebb atomi műveletek sorozata kell állnia. Azt is rögzítsen a meghívási folyamat tartós tárolására, az, hogy minden későbbi meghívási felveszi hol a megelőző tevékenység leállt.

Végezetül minden hosszan futó műveleteket érvényesíthetők többször amíg nem sikerül. Például a kiépítési művelet lehet helyezni egy Azure várólista, és ezután eltávolít a sorból dolgozó szerepkör által csak akkor, ha sikerül. Lehet, hogy a szemétgyűjtő létrehozásához szükséges műveletek megszakadt adatok karbantartása.

###<a name="elasticity"></a>Rugalmasság

A kezdeti minden szerepkörhöz futó példányok száma konfigurációs minden szerepkör határozza meg. A rendszergazdák minden szerepkör alapján várható betöltés két vagy több példányok futtatása kezdetben konfigurálja. De egyszerűen méretezheti szerepkör-példányok vagy lefelé használatát, minták módosítása. Ezt megteheti manuálisan az Azure-portálon, vagy a Windows PowerShell, a szolgáltatás felügyeleti API vagy harmadik fél eszközök használatával automatizálhatja a folyamat. További információért megtudhatja, [hogy miként Automatikus méretezéssel egy alkalmazást](../cloud-services/cloud-services-how-to-scale.md).

###<a name="partitioning"></a>Szétválasztás

Az Azure háló vezérlő kétféle partíciót használja:

* A szolgáltatás szerepkör példányok csoportokban frissítése egy *tartomány frissítése* használható. Azure több frissítése tartomány példányokat szolgáltatás üzembe helyezése. A helyi frissítés a háló vezérlő egy frissítés tartományban lévő összes előfordulását le életre, frissítésük és újraindítja őket a következő frissítés tartomány áthelyezése előtt. Ezt a módszert megakadályozhatja, hogy a teljes szolgáltatás nem érhető el a frissítési folyamat során.
* *Hibafa-tartomány* hardver- vagy hálózati hiba lehetséges pontjai határozza meg. Bármely szerepkört, amelynek egynél több példány a háló vezérlő biztosítja, hogy a példányok hibafa több tartományt, elszigetelt hardver hibák megakadályozása szolgáltatás megszakítása vannak elosztva. Hibafa-tartományok összes információk megjelenítése a kiszolgáló és fürt hibák szabályozza.

Az [Azure szolgáltatás szintű szerződést (SLA)](https://azure.microsoft.com/support/legal/sla/) garantálja, hogy két vagy több webes szerepkör példányok különböző hiba van telepítve, és frissítse a tartomány, amikor szintjüket külső connectivity az esetek legalább 99.95 százalékában. Frissítés tartományok eltérően nincs mód a sorszám hibafa-tartományok. Azure automatikusan lefoglalja hibafa tartományok és szerepkör-példányok elosztja őket. At minden szerepkör legalább első két példánya különböző hiba kerül, és annak érdekében, hogy legalább két példánya bármely szerepkört megfelel-e a SLA tartományok frissítése. Ez a következő diagramon jeleníti meg.

![Hibafa tartománybeli egyszerűsített nézete](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-1.png)

###<a name="load-balancing"></a>Terheléselosztás

Az összes bejövő forgalmat webes szerepkörbe áthaladt állapot nélküli terheléselosztó, amely szétosztja ügyfél kérelmek a szerepkör-példányok között. Egyes szerepkör-példányok nincs nyilvános IP-címeket, és nem az internetről közvetlenül címmel rendelkező. Webes szerepkörök állapot nélküli, úgy, hogy az ügyfél kérésének szerepkör példányokban lehet továbbítani. A [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck.aspx) esemény akkor következik be, minden 15 másodperc. Ezzel jelezheti, hogy kapni az adatforgalom készen áll-e a szerepkört, vagy hogy elfoglalt, és ki a terheléselosztó típust kell tenni.

##<a name="virtual-machines"></a>Virtuális gépeken futó

Azure virtuális gépeken futó platform eltér, mint (PaaS) szolgáltatás nagy elérhetősége viszonyított több szempontból szerepkörök számítja ki. Bizonyos esetekben el kell végeznie a magas rendelkezésre állásának további munkát.

###<a name="disk-durability"></a>Lemezen tartóssági

Szerepkör-példányok PaaS, eltérően virtuális gép-meghajtón tárolt adatok állandó akkor is, ha a virtuális gép át van helyezve a következő. Azure virtuális gépeken futó használja az Azure-tárolóban lévő BLOB virtuális lemezt megtalálható. Azure tároló elérhetősége jellemzői miatt egy virtuális gép meghajtón tárolt adatok érhető el is rendkívül.

Ne feledje, hogy D meghajtó (a Windows VMs) Ez a szabály alól kivételt képeznek. D meghajtó a állvány tartalmazó kiszolgálón történik; a virtuális a ténylegesen tárolóhely, és az adatok elvesznek, ha a rendszer a virtuális. Csak az ideiglenes tárolási D meghajtó szól. Linux, az Azure "általában" (de nem mindig) közzététele a helyi ideiglenes lemez /dev/sdb blokk eszközként. Gyakran csatlakoztatva van az Azure Linux ügynök azok /mnt/resource vagy /mnt csatlakozási pontok (/etc/waagent.conf keresztül állítható be).

###<a name="partitioning"></a>Szétválasztás

Azure natív módon megérti, a rétegek PaaS alkalmazásban (webes szerepkör és dolgozó szerepkör), és így megfelelően terjesztheti őket hiba és frissítés tartományok. Viszont a réteg egy infrastruktúra szolgáltatást (IaaS) alkalmazásként manuálisan definiálni kell elérhetőségének beállítása keresztül. Elérhetőség készletek szükség egy SLA IaaS csoportban.

![Elérhetőség Azure virtuális gépeken futó beállítása](./media/resiliency-technical-guidance-recovery-local-failures/partitioning-2.png)

Az előző ábrán a Internet Information Services (IIS) réteg (amely működik, mint a web app réteg), és a SQL réteg (amely működik, mint egy adatok réteg) elérhetősége különböző csoportjaihoz vannak rendelve. Ez biztosítja, hogy van-e minden réteg összes előfordulását hardver redundancia virtuális gépeken futó azáltal, hogy hibafa tartományok, és hogy teljes rétegek nem veszik frissítés során.

###<a name="load-balancing"></a>Terheléselosztás

A VMs kell rendelkeznie a forgalom elosztva őket, ha egy adott TCP- és UDP-végpontot keresztül szeretne kell csoportosítani a VMs-alkalmazás és a betöltés egyenleg. További tudnivalókért olvassa el a [terheléselosztás virtuális gépeken futó](../virtual-machines/virtual-machines-linux-load-balance.md)című témakört. Ha a VMs beviteli kapja meg más forrásból (például várólista mechanizmusa), akkor egy terheléselosztó nincs szükség. A terheléselosztó egy egyszerű állapot-ellenőrzés segítségével határozza meg, hogy kell-e forgalom küldeni a csomópontra. Akkor is hozhat létre saját szondákat alkalmazás kötött egészségügyi mértékek, amelyek meghatározzák, hogy a virtuális kell kapniuk a forgalom végrehajtásához.

##<a name="storage"></a>Tárhely

Azure tároló az eredeti tartós adatokat Azure szolgáltatás. Blob, tábla, várólista és virtuális lemezterület biztosít. Egy egyetlen adatközponthoz belül magas rendelkezésre állás replikációs és erőforrás-kezelés használ. Az Azure tároló elérhetősége SLA biztosítja, hogy legalább az idő 99,9 százaléka:

* Helytelenül formázott kérések szeretne hozzáadni, frissítés, az Olvasás és a törlés adatokat fog sikerült és megfelelően dolgozható fel.
* Tárterület-fiókok fog rendelkezni az internetes átjáró kapcsolatot.

###<a name="replication"></a>A replikáció

Azure tároló különböző meghajtókon lévő összes adatot több példányban megtartásával végig a régión belüli teljesen független tárolóhely alrendszerek lehetővé teszi az adatok tartóssági. Adatok szinkron replikált-e, és minden másolatban lekötött előtt az írás nyugtázza van. Azure tároló egységesek erősen, ami azt jelenti, hogy Olvasás vannak biztosítani a legutóbbi írások megfelelően. Ezeken kívül adatok másolatait folyamatosan felismerése és kijavítása bit rothadás, egy gyakran overlooked veszélyt tárolt adatok integritását beolvasott.

Szolgáltatások kedvezménye való replikáció csak az Azure tároló használatával. A szolgáltatás Fejlesztőeszközök nem kell a további munkához helyreállítani a helyi meghibásodása.

###<a name="resource-management"></a>Erőforrás-kezelés

Legfeljebb 500 TB (a az előző legfeljebb 200 TB volt) után május 2014-es verziójában létrehozott tároló fiókok is nő. További terület szükség, ha több tárterület-fiók használata alkalmazások kell megtervezni.

###<a name="virtual-machine-disks"></a>Virtuális gép lemez

A virtuális gép lemez az Azure tárolására, így azonos tartósság és méretezhetőség tulajdonságok Blob-tárolóhoz, oldal blob-ként vannak tárolva. A tervezés teszi az adatok egy virtuális gép lemezen állandó, még akkor is, ha a virtuális rendszert futtató kiszolgáló nem működik, és újra kell indítani a virtuális egy másik kiszolgálón.

##<a name="database"></a>Adatbázis

###<a name="sql-database"></a>SQL-adatbázis

Azure SQL-adatbázis adatbázis szolgáltatásként segítségével. Alkalmazások gyors kiépítése, adatok és a lekérdezés relációs adatbázisok beszúrása lehetővé teszi. A jól ismert SQL Server szolgáltatást és funkciót, sok közben a hardver, konfigurációs, javítása és tűrőképessége terhet abstracting biztosít.

>[AZURE.NOTE] Azure SQL-adatbázis nem nyújt az SQL Server egy az egyhez szolgáltatás eltérés áll fenn. Az követelmények – egy egyedi módon van igazodó felhő alkalmazások (skála rugalmas, a karbantartási költségek csökkentése, és így tovább szolgáltatásként adatbázis) más-más szabálykészletet teljesítéséhez van szolgál. További tudnivalókért lásd: [Válassza az SQL Server lehetőség felhő: (PaaS) Azure SQL-adatbázis vagy SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

####<a name="replication"></a>A replikáció

Azure SQL-adatbázis csomópont szintű hiba beépített erősítése biztosít. Az adatbázis összes írások automatikusan replikált két vagy több hátteret csomópontok kvórum jóváhagyás technika keresztül. (Az elsődleges és másodlagos legalább egy igazolnia kell a tevékenység íródott a tranzakciónaplója, mielőtt a tranzakció sikeres tekinteni, és adja eredményül.) Csomópont sikertelen, ha az adatbázis automatikus átvétele a másodlagos replikák egyikére. Ennek hatására az ügyfélalkalmazásokban egy tranziens kapcsolat megszakítás. Emiatt összes Azure SQL-adatbázis ügyfelek tranziens kapcsolat kezelése valamilyen kell végrehajtása. További tudnivalókért lásd: a [szolgáltatás speciális útmutatókat újra](../best-practices-retry-service-specific.md).

####<a name="resource-management"></a>Erőforrás-kezelés

Minden adatbázis létrehozásakor van beállítva egy felső méretkorlátot. Jelenleg elérhető maximális mérete 1 TB (méret korlátai feltételek alapján a szolgáltatási réteg, olvassa el a [szolgáltatási rétegek és a teljesítmény szintjei Azure SQL-adatbázisait](../sql-database/sql-database-resource-limits.md#service-tiers-and-performance-levels). Adatbázis találatok maximális felső méretét, elutasítja a további Beszúrás vagy a frissítés parancsot. (Lekérdezése adatok és törlésére is továbbra is lehetséges)

Adatbázis-belül Azure SQL-adatbázis használja egy háló erőforrások kezelésére. Azonban helyett egy háló vezérlő, akkor a csengetés topológiában hibák feltárása. Minden kópia fürt két szomszédok van, és a felelős érzékeli, ha az azok nyissa a. Kópia megszakad, annak szomszédok indítja el a konfigurálás ügynök hozza létre azt egy másik számítógépen. Motor szabályozásának kapja meg, győződjön meg arról, hogy egy logikai kiszolgáló nem túl sok erőforrást használata a számítógépen vagy haladja meg a gép fizikai korlátai.

###<a name="elasticity"></a>Rugalmasság

Ha az alkalmazás a többet 1 TB adatbázis van szüksége, akkor egy méretezési megoldások kell hajtja végre. Akkor méretezése az Azure SQL-adatbázis manuálisan szétválasztás, más néven sharding, több SQL-adatbázisok közötti adatok alapján. A méretezési megoldások elérése szinte lineáris költség NÖV a méretarány lehetőséget nyújt. Rugalmas NÖV vagy kapacitásának igény is növekedjen növekményes költségeket, mivel az adatbázisok számlázva vannak, az átlagos tényleges méret napi használt nem alapuló maximális mérete alapján.

##<a name="sql-server-on-virtual-machines"></a>A virtuális gépeken futó SQL Server

Az SQL Server (2014 vagy újabb verzió) a Azure virtuális gépeken futó telepítésével kihasználhatja az SQL Server hagyományos elérhetősége funkciói. Ezek a funkciók AlwaysOn rendelkezésre állási csoportok és az adatbázis-tükrözéshez tartalmazza. Ne feledje, hogy Azure VMs, a tárhely és a hálózat egy helyszíni nem virtualizálva informatikai infrastruktúrát eltérő működési jellemzői. Egy nagy elérhetősége/vészhelyreállítás Azure-ban (HA/DR) az SQL Server-megoldás sikeres végrehajtása szükséges, hogy megérteni a különbségeket és tervezése a megoldás, hogy beleférjenek őket.

###<a name="high-availability-nodes-in-an-availability-set"></a>Magas rendelkezésre állás csomópontok egy elérhetőségének beállítása

Ha magas rendelkezésre állás megoldást Azure-ban, a magas rendelkezésre állás csomópontok helyezhet külön hibafa tartományok és frissítése a tartományok beállítása a Azure elérhetősége is használhatja. Világosság, hogy az elérhetőség be kapcsolva az Azure fogalmat. Célszerű a legjobb kell követve győződjön meg arról, hogy az adatbázisok elérhetők valóban nagyon, akár AlwaysOn rendelkezésre állási csoportok, adatbázis-tükrözéshez vagy mást. Ha Ön nem követendő gyakorlat, bizonyára hamis feltételezve, hogy a rendszer könnyen hozzáférhető csoportban. De a valóságban a csomópontok összes végződhet egyidejű mivel amikor megtörténnek, ugyanabban a tartományban hibafa Azure régióban helyét.

Ennek ellenére nem alkalmazhatók a napló szállítási. Szolgáltatásként katasztrófa helyreállítási győződjön meg, hogy a kiszolgálókat külön Azure régióban futnak. Ezek a régiók a meghatározás, a külön hibafa tartományok is.

Az Azure Cloud Services VMs csoportházirenden keresztül a klasszikus portál el szeretné helyezni az elérhetőség ugyanazok telepítenie kell őket az ugyanazt a felhőalapú szolgáltatást. Azure erőforrás-kezelővel (az aktuális portálra) rendszerbe VMs nincs ezt a korlátozást. A klasszikus portál Azure felhőszolgáltatásában VMs rendszerbe, csak az ugyanazt a felhőalapú szolgáltatást csomópontok elérhetősége mindazon vehet részt. Ezenkívül a felhőalapú szolgáltatások VMs annak érdekében, hogy azok IP-címei tartják követően szolgáltatás javító virtuális ugyanabba a hálózatba kell lennie. Ezzel a DNS frissítése megszakadása elkerülhető.

###<a name="azure-only-high-availability-solutions"></a>Ha csak Azure: magas rendelkezésre állás megoldások

Az SQL Server-adatbázisok Azure-ban magas rendelkezésre állás megoldást is AlwaysOn rendelkezésre állási csoportok vagy az adatbázis-tükrözéshez használatával.

Az alábbi ábra bemutatja, hogyan AlwaysOn rendelkezésre állási csoportok fut a Azure virtuális gépeken futó architektúráját. Ez az ábra témával, [magas rendelkezésre állásának és a helyreállítás a Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)a részletes cikkre lett származik.

![Rendelkezésre állási csoportok AlwaysOn a Microsoft Azure-ban](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-1.png)

A AlwaysOn sablonnal az Azure-portálon is automatikusan egy AlwaysOn rendelkezésre állási csoportok telepítési-végpont a Azure VMs is kiépítése. További tudnivalókért lásd: [SQL Server AlwaysOn kínáló Microsoft Azure portál gyűjteményben](https://blogs.technet.microsoft.com/dataplatforminsider/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery/).

Az alábbi ábra bemutatja, hogy az adatbázis tükrözése a Azure virtuális gépeken futó. A részletes témakörben [magas rendelkezésre állásának és a helyreállítás a Azure virtuális gépeken futó SQL Server](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)is lett venni.

![Adatbázis-tükrözéshez Microsoft Azure-ban](./media/resiliency-technical-guidance-recovery-local-failures/high_availability_solutions-2.png)

>[AZURE.NOTE] Mindkét architektúrákban egy tartományvezérlőnek szükség. Azonban adatbázis tükrözés, lehetősége egy tartományvezérlőnek fölöslegessé kiszolgálói tanúsítványok használatával.

##<a name="other-azure-platform-services"></a>Más Azure platform szolgáltatások

Alkalmazások programhoz készült Azure platform funkciók helyi hibák helyreállítása élvezhetik. Egyes esetekben az elérhetőség az adott forgatókönyvet növeléséhez adott műveletek hajthatók végre.

###<a name="service-bus"></a>Szolgáltatás Bus

Csökkentésében átmenetileg nem működik az Azure Service Bus ellen, fontolja meg egy tartós ügyféloldali várólista létrehozása. Ezzel ideiglenesen használja egy másik, a helyi tároló mechanizmusa, amelyek nem adhatók hozzá a szolgáltatás Bus várakozási sorban található üzenetek tárolásához. Az alkalmazás megadhatja, hogy hogyan kezelheti az ideiglenes tárolt üzeneteket, miután visszaállította a szolgáltatás. További tudnivalókért lásd: [Gyakorlati tanácsok a teljesítmény javítása használ szolgáltatási Bus brokered üzenetküldés](../service-bus-messaging/service-bus-performance-improvements.md) és [Szolgáltatás Bus (Vészhelyreállítás)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services).

###<a name="hdinsight"></a>Hdinsight szolgáltatáshoz

Az adatok Azure hdinsight szolgáltatáshoz tartozó Azure Blob-tárolóban lévő alapértelmezés szerint vannak tárolva. Azure tároló Blob-tárolóhoz magas rendelkezésre állás és tartóssági tulajdonságainak megadása Több-csomópont feldolgozása, amely Hadoop MapReduce feladatok van társítva a egy tranziens Hadoop elosztott fájl rendszer (hdfs) lehetőségre, amely már kiépítve, amikor HDInsight van szükség, akkor fordul elő. Egy MapReduce feladatot kapott eredmények is tárolja az Azure Blob-tárolóhoz, alapértelmezés szerint, hogy a feldolgozott adatok tartós és erősen továbbra is elérhető marad a Hadoop fürt leépítjük után. További tudnivalókért olvassa el a [HDInsight (Vészhelyreállítás)](./resiliency-technical-guidance-recovery-loss-azure-region.md#other-azure-platform-services)című témakört.

##<a name="checklists-for-local-failures"></a>Ellenőrzőlisták a helyi hibák

###<a name="cloud-services"></a>Cloud Services

  1. Tekintse át a jelen dokumentum Cloud Services szakaszát.
  2. Állítsa be az egyes szerepkör legalább két példánya.
  3. Továbbra is fennáll, tartós tárolására, a szerepkör-példányok nem állapotát.
  4. A StatusCheck esemény megfelelően kezeli.
  5. Kapcsolódó módosítások tördelése a tranzakciókat, amikor csak lehetséges.
  6. Győződjön meg róla, hogy dolgozói szerepkör feladatok idempotent és újraindítható.
  7. Folytassa a hívás a műveletek, amíg meg nem sikerül.
  8. Fontolja meg autoscaling stratégiák.

###<a name="virtual-machines"></a>Virtuális gépeken futó

  1. Tekintse át a jelen dokumentum virtuális gépeken futó szakaszát.
  2. Ne használja a D meghajtó állandó tárhelyként használható.
  3. Egy szolgáltatási réteg gépek csoportosíthatja az elérhetőség beállítása.
  4. Konfigurálja a terheléselosztás és a választható szondákat.

###<a name="storage"></a>Tárhely

  1. Tekintse át a dokumentum tárolási szakaszát.
  2. Több tárhely-fiók használatával adatok vagy sávszélesség meghaladja kvóták.

###<a name="sql-database"></a>SQL-adatbázis

  1. Tekintse át a jelen dokumentum SQL-adatbázis szakaszát.
  2. Ideiglenes (tranziens) hibák kezelése újrapróbálkozási házirend végrehajtása.
  3. Méretezési stratégia szétválasztás/sharding használja.

###<a name="sql-server-on-virtual-machines"></a>A virtuális gépeken futó SQL Server

  1. Olvassa el a jelen dokumentum szakaszát virtuális gépeken futó SQL Server.
  2. Kövesse az előző javaslatok virtuális gépeken futó.
  3. Az SQL Server magas elérhetősége funkciókat, például AlwaysOn használja.

###<a name="service-bus"></a>Szolgáltatás Bus

  1. Tekintse át a jelen dokumentum szolgáltatás Bus szakaszát.
  2. Érdemes létrehozni egy tartós ügyféloldali várólista másolatként.

###<a name="hdinsight"></a>Hdinsight szolgáltatáshoz

  1. Tekintse át a jelen dokumentum HDInsight szakaszát.
  2. Nincsenek további elérhetősége lépések elvégzése nem szükséges helyi hibák.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [technikai útmutató Azure tűrőképessége](./resiliency-technical-guidance.md)sorozat része. A sorozat a következő cikk [helyreállítása a régió szintű a szolgáltatási zavarok](./resiliency-technical-guidance-recovery-loss-azure-region.md).
