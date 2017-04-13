<properties
   pageTitle="Magas elérhetősége Azure-alkalmazások |} Microsoft Azure"
   description="Technikai áttekintés és tervezéséről és magas elérhetőség alkalmazások létrehozásába a Microsoft Azure részletesebb információt."
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

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Microsoft Azure-ra épülő alkalmazások magas elérhetősége

Egy könnyen hozzáférhető alkalmazás elérhetősége, betöltése és a függő szolgáltatásokat és a hardver ideiglenes hibák ingadozása elnyeli. Az alkalmazás továbbra is működnek-elfogadható felhasználói és rendszeres válasz szintű üzleti követelmények vagy alkalmazás szolgáltatói rendelkezést (SLA) által megadott.

##<a name="azure-high-availability-features"></a>Azure magas rendelkezésre állás funkciók

Azure számos beépített platform szolgáltatások, amelyek támogatják a könnyen hozzáférhető alkalmazást tartalmaz. Ez a szakasz ismerteti a főbb jellemzők részét. Átfogóbb elemzés a platform a [technikai útmutató Azure tűrőképessége](./resiliency-technical-guidance.md)című témakör tartalmaz.

###<a name="fabric-controller"></a>Háló vezérlő

Az Azure háló vezérlő rendelkezések, és figyeli a az Azure számítási példányok állapotát. A háló vezérlő ellenőrzi a hardver állapotát és a szoftver host és vendéghozzáférés gépi példányok. Ha azt észleli, hogy egy hiba, a virtuális példányok automatikusan áthelyezésével kényszeríti SLA. Hibafa és frissítésére tartományok további fogalmának támogatja a számítási SLA.

Ha több Felhőszolgáltatásba szerepkör példánya van telepítve, Azure ezekben az esetekben különböző hiba tartományok üzembe helyezése. Hibafa tartomány oszlopazonosító jobboldali tulajdonképpen egy másik hardver állvány ugyanabban a régióban. Hibafa-tartományok csökkentése annak a valószínűségét, hogy honosított hardver hiba szakítja meg az alkalmazás a szolgáltatás. A szám vagy a webes szerepkörök van rendelve hibafa-tartományok nem képes kezelni. A háló vezérlő dedikált források, amelyek külön Azure által üzemeltetett alkalmazásokból használja. 100 %-os üzemidőt rendelkezik, mert azt az Azure rendszer sejtmagjában szolgál. Figyeli és szerepkör-példányok kezeli hibafa tartományok.

Az alábbi ábra mutatja az Azure megosztott erőforrásokat, hogy a háló vezérlő üzembe helyezése, és különböző hiba tartományok kezeli.

![Hibafa tartománybeli egyszerűsített nézete](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Frissítési tartományok hibafa tartományok függvény hasonlít, de támogatja-e a hibák, hanem frissítések. Egy frissítési tartománya példány elkülönítésének határozza meg, hogy mely egy adott szolgáltatás példányok frissülni fognak egy pontján időben logikai időegység. A szolgáltatott szolgáltatás üzembe alapértelmezés szerint öt frissítési tartományok határozzák meg. A szolgáltatás-definíciós fájl az adott értéket is módosíthatja. Tegyük fel például, hogy nyolc példányát a webes szerepkör. Két példánya három frissítési tartományok és az egyik frissítési tartomány két példánya lesz. Azure frissítés sorrendjét határozza meg, de a frissítési tartományok számának alapul. További információt a frissítési tartományok olvassa el a [frissítése egy felhőalapú szolgáltatásba](../cloud-services/cloud-services-update-azure-service.md)című témakört.

###<a name="features-in-other-services"></a>Az egyéb szolgáltatásokat lehetőségei

Magas számítási elérhetősége támogató platform szolgáltatások használatán kívül az Azure magas rendelkezésre állás, szolgáltatások ágyaz a más szolgáltatásokhoz. Például az Azure tároló összes blob táblázat adatok, és a várakozási három replikáit tárolja. Is lehetővé teszi a biztonsági másolatok BLOB és táblázatok tárolása a másodlagos terület geo replikációs lehetőséget. A Azure tartalom kézbesítési hálózat lehetővé teszi, hogy gyorsítótárba helyezhető redundancia és a méretezhetőség a világon BLOB. Azure SQL-adatbázis a, valamint a több replikáit tárolja.

Mellett a [technikai útmutató Tűrőképessége](https://aka.ms/bctechguide) cikksorozat olvassa el a [Gyakorlati tanácsok az Azure Cloud Services nagyszabású szolgáltatások tervezés](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) . Ezek a dokumentumok mélyebb vitafórum az Azure platform elérhetősége funkciók nyújtanak.

Bár a Azure lehetővé teszi, hogy több funkciók, amelyek támogatják a magas elérhetőségét, fontos, hogy azok korlátai ismertetése:

- A számítási a Azure garantálja, hogy a szerepkörök elérhető és működik, de azt nem tudja, hogy ha az alkalmazás futó és túlterhelt.
- Azure SQL-adatbázis adatainak van replikált szinkron a régión belüli. Megadhatja, hogy az aktív geo replikációs, amely lehetővé teszi, hogy legfeljebb négy adatbázis további másolatok ugyanabban a régióban (vagy a különböző régiók). Ezek az adatbázis-kópiák sem pont és az idő biztonsági másolatok. SQL-adatbázisait adja meg a biztonsági funkciók pont idejét. További információk az SQL adatbázis pont igény funkcióiról, olvassa el az [Idő visszaállítása Azure SQL adatbázis pontra](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Azure tárolására a táblázat és blob-adatok replikált van egy másik területére alapértelmezés szerint. Azonban nem érhetők el a replikák mindaddig, amíg a Microsoft úgy dönt, hogy a másik webhely átadni. Régió feladatátvétel csak egy hosszabb régió szintű a szolgáltatási zavarok esetén, és nincs semmilyen SLA geo átváltó alkalommal. Is fontos tudni, hogy minden olyan adatsérülésekkel gyorsan oldalpárokat a kópiákkal.

Az alábbi okai lehetnek meg kell kiegészítése az platform elérhetősége szolgáltatások elérhetősége az alkalmazás-specifikus funkcióival. Az alkalmazás-specifikus elérhetősége szolgáltatásai a blob pillanatkép funkció pont és az idő blob-adatok biztonsági másolatait létrehozásához.

###<a name="availability-sets-for-azure-virtual-machines"></a>Elérhetőség az Azure virtuális gépeken futó állítja be.

Ez a cikk a legtöbb platformot használja a szolgáltatás (PaaS) modellben felhőszolgáltatások koncentrál. Vannak azonban olyan is adott elérhetőségi szolgáltatások az Azure virtuális gépeken futó, amely infrastruktúrát használ szolgáltatási (IaaS) modellben. A virtuális gépeken futó magas elérhetősége eléréséhez elérhetőségének beállítása kell használnia. Az elérhetőség beállítása hibafa és frissítés tartományok hasonló funkcióval szolgál. Belül az elérhetőség Azure oly módon, hogy megakadályozza, hogy a honosított hardver hibák és karbantartási tevékenységek lefelé az összes csoport a gép arra a virtuális gépeken futó helyezi. Elérhetőség beállítása az Azure SLA virtuális gépeken futó elérhetőségét eléréséhez szükséges.

Az alábbi ábra ábrázolhatók az két elérhetőségének beállítása adott csoport webes és az SQL Server virtuális gépeken futó, illetve.

![Elérhetőség az Azure virtuális gépeken futó állítja be.](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Az előző ábrán az SQL Server telepítve, és virtuális gépeken futó. Eltér az előző hozzászólásra az Azure SQL-adatbázissal, amely magában foglalja a felügyelt szolgáltatásként adatbázis.

##<a name="application-strategies-for-high-availability"></a>Alkalmazás stratégiák magas elérhetősége

A legtöbb alkalmazás stratégiák magas elérhetősége magában foglalja a redundancia vagy a merevlemez függőségeket alkalmazásösszetevők eltávolítását. Alkalmazás tervezés során Azure vagy külső szolgáltatásokra támaszkodik kapcsolata időről-időre állásidőt hibatűrést támogatnia kell. Az alábbi szakaszok ismertetik a felhőalapú szolgáltatások elérhetőségét növelése alkalmazás mintázatok.

###<a name="asynchronous-communication-and-durable-queues"></a>Aszinkron kommunikáció és tartós sorban várakozó

Fontolja meg aszinkron kommunikációját módszerektől összekapcsolt szolgáltatások elérhetősége az Azure alkalmazások növeléséhez. Ez a mintázat írási üzenetek tároló sorban várakozó vagy a későbbi feldolgozásra Azure Service Bus sorok. Ha a sorba írja meg az üzenetet, vezérlő azonnal az üzenet feladójának adja eredményül. Az alkalmazás egy másik réteg feldolgozása, általában egy dolgozó szerep végrehajtani üzenet kezeli. Ha a dolgozó szerepkör megszakad, az üzenetek a várakozási sorban található gyűjteniük, mindaddig, amíg a feldolgozás szolgáltatás visszaáll. Mindaddig, amíg a sor érhető el, nem az előtér-küldő és az üzenet processzor között nincs közvetlen függőség. Így nem kötelező, amely egy átviteli szűk elosztott alkalmazásokban párhuzamos szolgáltatási hívásokat.

Ez változata használja Azure tároló (BLOB, táblázatokat, sorban várakozó) vagy a szolgáltatás Bus sorban várakozó feladatátvevő helyként sikertelen adatbázis hívások. Másik szolgáltatásra (például SQL Azure-adatbázis) alkalmazáson belül szinkron hívás például többször meghiúsul. Előfordulhat, hogy adatokat szerializálni tartós tárolóba. Újabb valamikor esetén a szolgáltatásra vagy az adatbázis vissza online, az alkalmazás is újra a kérelem elküldése tárhelyről. A modellben különbség, hogy a köztes helyre nem része a állandó az alkalmazás munkafolyamat. Csak a hiba esetek használatos.

Mindkét esetben aszinkron kommunikáció és köztes tárolási megakadályozása leállított háttéradatbázist szolgáltatás leállítása a teljes alkalmazás. Sorok egy logikai közvetítő lesz. További kiválasztása a megfelelő várólista szolgáltatást, olvassa el [Azure sorban várakozó Azure Service Bus sorban várakozó – képest és ellentétben](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Hibafa észlelése, és próbálkozzon újra logikája

Egy könnyen hozzáférhető alkalmazás Tervező van szó, újra logika kód segítségével biztonságos kezelését szolgáltatásra van átmenetileg nem működnek. Az [Ideiglenes (tranziens) hiba kezelésének alkalmazás letiltása](https://msdn.microsoft.com/library/hh680934.aspx), a Microsoft Patterns és eljárásokat csoport által fejlesztett alkalmazások fejlesztői számára a folyamat segítséget nyújt. A "tranziens" szó azt jelenti, hogy egy ideiglenes feltétel, amely csak viszonylag rövid ideig tart. Ez a cikk környezetében tranziens hibák kezelése része a könnyen hozzáférhető-alkalmazások fejlesztése. Átmeneti feltételek többek között szakaszos hálózati hibák és az adatbázis-kapcsolat elvész.

Az ideiglenes (tranziens) hiba kezelésének alkalmazás blokkokból álló egyszerűsített módja a szeretné kezelni a hibák, a kód biztonságos módon. Az alkalmazás elérhetőségét javítása robusztus tranziens hibafa-kezelési logika hozzáadását felhasználhatja azt. A legtöbb esetben újrapróbálkozási logika rövid megszakadása kezeli, és a feladó és címzett helyreáll egy vagy több sikertelen próbálkozás után sem. A sikeres kísérletek próbálkozásra általában ugrik észrevétlen alkalmazás felhasználóinak.

A fejlesztők az újraküldés logikája kezelésére szolgáló három lehetőség van: egyre növekvő tendenciát mutat, a rögzített intervallum és exponenciális. Növekményes vár előtt már minden újra növekvő lineárisan (például 1, 2, 3 és 4 másodperc). Rögzített intervallum az egyes kísérletek (például 2 másodpercig) között eltelt idő ugyanannyi várakozik. Egy további véletlen lehetőségre az exponenciális vissza ki várakozik már újrapróbálkozások között. Azonban a exponenciális viselkedés (például 2, 4, 8 és 16 másodperc) használja.

A magas szintű stratégia a kód van:

1. Adja meg a újrapróbálkozási stratégia és a házirend.
1. Próbálja meg a műveletet, amely egy ideiglenes (tranziens) hibát eredményezi.
1. Ideiglenes (tranziens) hiba lép fel, ha meghívása a újrapróbálkozási házirend.
1. Ha nem sikerül az összes újrapróbálkozások, elfog a végleges kivétel.

Az újraküldés logikája tesztelheti annak érdekében, hogy újrapróbálkozások egymást követő műveletek nem eredményeznek egy szeretne hosszú késedelem szimulált hibák. Ehhez a mielőtt úgy dönt, hogy sikertelen lesz a teljes feladatot.

###<a name="reference-data-pattern-for-high-availability"></a>Hivatkozási adatok mintájának magas elérhetősége

Hivatkozás egy alkalmazás csak olvasható adatok adata. Ezeket az adatokat az üzleti környezetben, amelyeken belül az alkalmazás tranzakció alapú adatok létrehoz egy üzleti művelet során biztosít. A hivatkozási adatok pont és az idő függvény adata tranzakció alapú. Emiatt a integritását függ, hogy a hivatkozási adatok pillanatképe a tranzakció időben. Némileg laza definícióját, de azt kell elegendő, itt a célra.

Hivatkozás egy alkalmazás környezetében adata az alkalmazás működéséhez szükséges. A saját alkalmazások létrehozása és kezelése a hivatkozási adatok; mesteradat-kezelés (MDM) rendszerek gyakran végre ezt a funkciót. Ezek a rendszerek a hivatkozási adatok életciklusára felelősek. Hivatkozási adatok közé termékkatalógus, a fő alkalmazottak, fő részből és berendezések diaminta. Hivatkozási adatok származik a szervezeten kívüli például irányítószámok vagy díjak adó is. Stratégiák a hivatkozási adatok elérhetőségének növelésével nehezen általában kevesebb mint tranzakció alapú adatokhoz. Hivatkozás adatokat tartalmaz az az előnye, hogy főleg megváltoztatható.

Azure webes és dolgozó szerepkörökkel önálló futásidőben hivatkozási adatok felhasználása az alkalmazás és a hivatkozási adatok üzembe helyezésével teheti meg. Ha a helyi tároló méretét lehetővé teszi, hogy ilyen telepítésen, ez az egy ideális állam. Beágyazott-adatbázisok (SQL, NoSQL) és az XML-fájlok helyi fájlrendszerben rendszerbe állított segítséget nyújt az Azure számítási skála egységek autonómiáját. Adatok frissítése az egyes szerepkör újratelepítés anélkül, hogy egy mechanizmusa azonban rendelkeznie kell. Ehhez jelölje ki a hivatkozási adatok egy felhőalapú tárolási végpontra (például Azure Blob-tárolóhoz vagy SQL-adatbázis) valamilyen frissítés. Kód hozzáadása a szerepkörökhöz letölti a az adatok frissítését be a számítási csomópontok szerepkör indításkor. Azt is megteheti vegye fel a kódot, amely lehetővé teszi, hogy a rendszergazda a kényszerített letölteni a szerepkör-példányok be.

Elérhetőség növeléséhez a szerepkörök tartalmaznia kell egy sor olyan hivatkozási adatok abban az esetben, ha tárterületet nem működik. Egyszerű hivatkozás adatokat tartalmazó indítása mindaddig, amíg a tárhely erőforrás lesz elérhető a frissítések keresése a szerepkörök szolgáltatás lehetővé teszi.

![Önálló számítási csomópontok magas alkalmazás elérhetősége](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

A minta egy szempontokat szüksége telepítési és indítási sebességét a szerepköröket. Ha üzembe helyezése, illetve nagy mennyiségű adatot hivatkozás indításkor letöltése, ezzel új telepítések vagy szerepkör-példányok léptetőnyíl vezérlőelem szükséges idő mennyiségét növelésével. Lehet, hogy egy külső tárolása függően helyett az azonnal elérhető hivatkozási adatok gyakorló minden szerepkör autonómiáját az elfogadható útján.

###<a name="transactional-data-pattern-for-high-availability"></a>Magas elérhetősége tranzakció alapú adatok mintájának

Tranzakció alapú adata az adatokat, hogy az alkalmazás létrehoz egy vállalati környezetben. Üzleti folyamatok, amely az alkalmazás és a hivatkozási adatok, amely támogatja az alábbi eljárások számának együttes adata tranzakció alapú. Példák a tranzakció alapú adatokat is elhelyezhet, rendelések, a speciális szállítási közleményeinek, a számlák és az ügyfél Ügyfélkapcsolat-kezelés (CRM) lehetőséget. Az így létrehozott tranzakció alapú adatokat fog adni, hogy külső rendszerek rögzítésére vagy további feldolgozásra.

Ne feledje, hogy adatokat módosíthatja a rendszer az adatok felelősek belül hivatkozás. Emiatt tranzakció alapú adatok mentenie kell a hivatkozási pontot idejét adatkörnyezet úgy is, hogy a szemantikai konzisztencia minimális külső függőségeket. Például érdemes megvizsgálni az Eltávolítás a katalógusról termék néhány hónapok megrendelés teljesítése után. A legjobb módszer is ágyazhat be annyi hivatkozás adatkörnyezet szerinti a tranzakciók. Ez megőrzi a tranzakció társított szemantikáját, még akkor is, ha a hivatkozási adatok módosítása után a tranzakció rögzíthető.

A korábban említett, laza kapcsoló és aszinkron kommunikáció használó architektúrákban működhet magasabb szintű elérhetőségét. Ez a tranzakció alapú adatokhoz, valamint igaznak, de végrehajtása összetettebb. Hagyományos tranzakció alapú vélemények általában az adatbázist a tranzakció biztosító támaszkodhat. Köztes rétegek bevezetésére meg, amikor az alkalmazás kódja helyesen kell kezelni a különféle rétegeket elegendő egységesebb és tartóssági biztosítja az adatok.

Az alábbi lépések, amelyek a rögzítés tranzakció alapú adatok elválasztja a feldolgozás munkafolyamat ismerteti:

1. Webes számítási csomópont: bemutató adatokra.
1. Külső tároló: köztes tranzakció alapú adatokat is menteni.
1. Webes számítási csomópont: a végfelhasználói tranzakció befejezéséhez.
1. Webes számítási csomópont: bejegyzett tranzakció alapú együtt az adatokat is, az összefoglaló adatok környezetben küldése ideiglenes tartós tárhely, amely kiszámíthatóan választ ad.
1. Webes számítási csomópont: a tranzakció végfelhasználói kiegészítésének jel.
1. Háttér csomópont számítja ki: a tranzakció alapú adatok kibontása, utáni feldolgozása, ha szükséges, és küldje el az utolsó tárolási helyére a jelenlegi rendszerben.

A következő ábrán a tervezés egy lehetséges alkalmazásának Azure által üzemeltetett felhőszolgáltatásában.

![Magas laza összekapcsolási elérhetősége](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Az előző ábrán a szaggatott nyilak jelzik aszinkron feldolgozása. Az előtér-webes szerepkör nem áll a aszinkron feldolgozás tudatában. Ez a végleges célhelyét alapján az aktuális rendszer tranzakció tárolására vezet. Az időtartam, amely a aszinkron modell vezet be, hogy a tranzakció alapú adatok nem érhető azonnal lekérdezés. Ezért szeretné menteni a gyorsítótár minden egyes tranzakció alapú adatok mértékegység van szüksége, vagy egy felhasználói foglalkozáson, ahol a felhasználói felület azonnali értekezlet szüksége van.

A webes szerepkör az önálló infrastruktúra többi. Az elérhetőség profilt a webes szerepkört, és az Azure várólista és nem a teljes infrastruktúra. Túl nagy elérhetőségét ezt a megközelítést lehetővé teszi, hogy a webes szerepkört, ha át kívánja méretezni vízszintesen, függetlenül a háttéradatbázist tároló. A magas rendelkezésre állás modell is hatással a közgazdasági műveletek. További összetevőit Azure sorban várakozó és dolgozó szerepkörök például havi használati költségek hatással lehet.

Ne feledje, hogy az előző diagram látható tranzakció alapú adatok leválasztott eljárás egy végrehajtását. Vannak olyan sok lehetséges megvalósítás. Az alábbiakban néhány alternatívája:

 * A webes szerepkört, és a tárhely várólista között dolgozó szerepkörbe előfordulhat, hogy helyét.
 * A szolgáltatás Bus várólista egy Azure tároló várólista helyett használható.
 * Lehet, hogy a rendeltetési Azure tároló vagy egy másik adatbázist szolgáltatóval.
 * Adja meg az azonnali gyorsítótárazási követelmények után a tranzakció Azure gyorsítótár használható a webes rétegben.

###<a name="scalability-patterns"></a>Méretezhetőség mintázatok

Fontos tudni, hogy közvetlenül a méretezhetőség a felhőalapú szolgáltatás hatása a elérhetősége. Nagyobb betöltés okoz a szolgáltatás nem reagál, a felhasználó benyomást esetén, hogy az alkalmazás nem működik. Kövesse a várt alkalmazás betöltése és a későbbi várakozásoknak alapján méretezhetőség vonatkozó gyakorlati tanácsokat. A legnagyobb skála sok szempontok, például egy több tárterületet fiókhoz, keresztül több adatbázis megosztása és a gyorsítótár-stratégiák és felhasználása magába foglalja. Meg szeretné vizsgálni a működésébe ezeket a mintázatok olvassa el a [Gyakorlati tanácsok az Azure Cloud Services nagyszabású szolgáltatások tervezés](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)című témakört.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [vészhelyreállítás és a Microsoft Azure-ra épülő alkalmazások elérhetőséget](./resiliency-disaster-recovery-high-availability-azure-applications.md)egy cikksorozat része. A sorozat a következő cikk [Microsoft Azure-ra épülő alkalmazások vészhelyreállítás](./resiliency-disaster-recovery-azure-applications.md).
