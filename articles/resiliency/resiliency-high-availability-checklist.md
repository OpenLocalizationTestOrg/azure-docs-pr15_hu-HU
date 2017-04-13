<properties
   pageTitle="Magas elérhetősége feladatlista |} Microsoft Azure"
   description="Beállítások és a műveleteket, amelyeket sokat tehet győződjön meg arról, hogy egy rövid feladatlista fejlesztjük az Azure az alkalmazás elérhetőségét."
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

#<a name="high-availability-checklist"></a>Magas elérhetősége ellenőrzőlista
A nagy előnyei Azure egyik lehetővé teszi, hogy az alkalmazás segítségével az a felhő elérhetőség (és méretezhetőség). Győződjön meg arról, ezeket a beállításokat a legtöbb készít, az alábbi ellenőrzőlista segítséget nyújt az egyes fő infrastruktúra alapvető való annak biztosítására, hogy az alkalmazások rugalmassá jelenti. 

>[AZURE.NOTE] Az alábbi javaslatokat többsége dolgot, amely az alkalmazás bármikor végrehajtható, és így kiválóan alkalmasak "gyors javítások". A legjobb hosszú távú megoldást gyakran-alkalmazás tervezés, az a felhő épített magába foglalja.  Az egy ellenőrzőlistát a következő (további lépések Tervezés területek, olvassa el az [Elérhetőség feladatlista](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Használja forgalom Manager elé az erőforrások?
Adatforgalom Manager segít használatával, a forgalmat internet több Azure régiók és Azure és a helyszíni helyeket. Ennek számos oka, beleértve a késés és az elérhetőség az teheti meg. Ha azt szeretné, hogyan forgalom Manager segítségével növelje a tűrőképessége, és húzza szét a forgalom több területre a további információkhoz juthat, olvassa el az [Operációs rendszert futtató VMs több adatközpont esetén a magas elérhetőség Azure](../guidance/guidance-compute-multiple-datacenters.md).

__Mi történik, ha Ön nem használja a forgalom Manager?__ Ha forgalom Manager elé az alkalmazás nem használ, akkor korlátozódik egyetlen területére az erőforrásokhoz tartozó. Ez a méretezés korlátozza, növeli a felhasználóknak, akik nem a kiválasztott területi közelébe késése és csökkenti a védelmét, amíg a terület szintű a szolgáltatási zavarok.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Kiküszöbölhető egyetlen virtuális számítógépen használja bármely szerepkör?
Jó tervezés elkerülhető hiba egyetlen bármely pontján. Az összes szolgáltatás Tervező fontos (helyszíni, vagy a felhőben), de a felhőben különösen akkor hasznos, átméretezheti méretezhetőség és bár méretezés kifelé (hozzáadása a virtuális gépeken futó) tűrőképessége méretezés (használatával egy nagyobb teljesítményű virtuális gép) helyett. Ha azt szeretné, megtudhatja, hogy további információt a méretezhető alkalmazás tervezés, olvassa el a [Microsoft Azure-ra épülő alkalmazások magas elérhetősége](resiliency-high-availability-azure-applications.md).

__Mi történik egy szerepkört egyetlen virtuális gép esetén?__ Egy egyetlen számítógépre hiba egyetlen pontot, és nem érhető el az [Azure virtuális gép szolgáltatási szint szerződés](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). A legjobb esetekben az alkalmazás csak finom működik, de ez nem egy rugalmas tervezés, az Azure virtuális gép SLA és hiba növeli az esélye annak, ha valami legrövidebb leállás nem sikerül egyetlen bármely pontján nem vonatkozik.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Az alkalmazás internetes VMs elé egy terheléselosztó használja?
Terheléselosztókkal lehetővé teszi a bejövő forgalom az alkalmazás kiterjed tetszőleges számú gépek között. Akkor is hozzáadása és eltávolítása gépek a terheléselosztó a bármikor, amely működik jól a virtuális gépeken futó (és az automatikus méretezés bíró virtuális gép skála) lehetővé teszi a forgalom és a virtuális sikertelen növekedését egyszerűen kezelése. Ha szeretne többet megtudni a terheléselosztókkal, olvassa el az [Azure betöltése terheléselosztó – áttekintés](../load-balancer/load-balancer-overview.md) és a [több VMs a méretezhetőség és elérhetőség az Azure futtatása](../guidance/guidance-compute-multi-vm.md).

__Mi történik, ha nem használ egy terheléselosztó elé az internetes VMs?__ Egy terheléselosztó nélkül nem fogja tudni méretezése (hozzáadása több virtuális gépeken futó), és csak ha át kívánja méretezni (a webes elérésű virtuális gép méretének növelése). Egyetlen hiba, hogy a virtuális gép pont is nézzen. Akkor is kell DNS-kódot az figyelje meg, ha egy internetes gépi megszakadt, és újra feleltesse meg az új számítógépre indulhat elfoglalt helyét a DNS-bejegyzés írása.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Azok az elérhetőség segítségével állítja az állapot nélküli alkalmazás és a webhely-kiszolgálókon?
A gép illusztráló egy elérhetőségének beállítása ugyanabban a alkalmazás rétegben ellenőrzi az Azure virtuális SLA használatára jogosult a VMs. Az elérhetőség beállítása is részét biztosítja, hogy a gép üzembe-e (azaz különböző host gépek, amely a különböző időpontokban telepítve) különböző frissítési tartományok és hiba (vagyis egy közös power forrás és a hálózati megosztása host gépek váltás) tartományok. Anélkül, hogy egy elérhetőségének beállítása, a VMs host ugyanarra a gépre is található, és így előfordulhat, hogy egyetlen pont hiba, amely nem jelenik meg. Ha azt szeretné, megtudhatja, hogy további információt az elérhetőség készletek használata VMs elérhetőségének növelésével, olvassa el a [virtuális gépeken futó elérhetőségének kezelése](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Mi történik, ha Ön nem használja az elérhetőség webkiszolgálón és állapot nélküli alkalmazások beállítása?__ Nem használja az elérhetőség beállítása azt jelenti, hogy nem tudja az Azure virtuális SLA előnyeit. Azt is jelenti, hogy az alkalmazás a réteg gépek sikerült minden kapcsolat nélküli módba frissítés az állomásgép (a gépen, amelyen a VMs esetén) vagy egy közös hardver hiba esetén.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Használja a virtuális gépi skála beállítása (VMSS) az állapot nélküli alkalmazást vagy a webes kiszolgálók?
Jó méretezhető és rugalmassá Tervező VMSS győződjön meg arról, hogy akkor is méretezés az alkalmazás az egy réteg gépek számát (például a webes réteg) használja. VMSS lehetővé teszi, hogy megadhatja, hogy az alkalmazás réteg méretezze át (kiszolgálók hozzáadása vagy eltávolítása a kiválasztott feltételek alapján). Ha azt szeretné, megtudhatja, hogy hogyan Azure virtuális gép skála beállítása segítségével növelje a forgalom kiugrásainak megfelelő erősítése olvashat, olvassa el a [Virtuális gép skála készletek áttekintése](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Mi történik, ha nem us egy virtuális gép méretarányra beállított az állapot nélküli webkiszolgáló-alkalmazás?__ Anélkül, hogy egy VMSS az Ön számára, ha át kívánja méretezni korlátozás nélküli és optimalizálása az erőforrásokhoz használt korlátozza. A tervezés, ahonnan hiányzik VMSS méretezési hibafüggvény további kóddal (vagy kézzel) kezelendő rendelkező tartalmaz. Egy VMSS hiánya azt is jelenti, hogy az alkalmazás nem egyszerűen hozzáadása és eltávolítása (függetlenül a méretarány) gépek segít kezelje a forgalom nagy kiugrásainak megfelelő (például promóció során, vagy ha a webhely/alkalmazás/termék Ugrás vírus).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Használja prémium tárterület és a külön tároló fiókok az egyes a virtuális gépeken futó?
A gyártási virtuális gépeken futó prémium tároló használandó a legjobb. Ezeken kívül gondoskodnia kell arról, hogy, használjon külön tárhely a virtuális gépeken (a kisüzemi telepítésekhez igaz. Nagyobb, amelyeket később újra felhasználhat telepítésekhez tároló fiókok, de több gépekhez egy terheléselosztás kell végrehajtania annak érdekében, hogy vannak kiegyensúlyozott a frissítés tartományok és a réteg az alkalmazás, hogy). Ha azt szeretné, megtudhatja, hogy további tájékoztatást az Azure tároló teljesítmény és méretezhetőség, olvassa el a [Microsoft Azure tárhely a teljesítmény és méretezhetőség feladatlista](../storage/storage-performance-checklist.md).

__Mi történik, ha nem használ külön tároló fiókok virtuális gépeken?__ Egy tárterület-fiókot, például a sok erőforrást található egy egyetlen hiba. Habár számos védelem és Azure tároló tűrőképessége szolgáltatásai, egyetlen hiba pont értéke nem lehet egy jó tervezés. Ha például sérülése jogokkal első szeretné, hogy a fiók Tárhelykorlát érvényes találat van, vagy egy [IOPS korlátozza](../azure-subscription-service-limits.md#virtual-machine-disk-limits) eléréséig, tárterület-fiókba való virtuális gépeken futó vannak hatással. Emellett a szolgáltatási zavarok, amely hatással van a tárhely bélyegző, amely tartalmazza az adott tárolási fiók esetén lehet több virtuális gépeken futó hatással.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Használja a terheléselosztó, illetve az alkalmazás minden réteg közötti várólista?
A terheléselosztókkal vagy a sorok között az alkalmazás minden réteg lehetővé teszi az alkalmazás minden réteg egyszerűen méretezni, egyszerűen és függetlenül. Ezek a késés, összetettsége, alapján technológiák között kell választania, és a terjesztési (azaz hogy meg vannak elosztása az alkalmazás) van szüksége. Az általános sorban várakozó gyakran magasabb időtartama és összetettsége hozzáadása viszont előnyei rugalmasabb alatt, és lehetővé teszi, hogy az alkalmazás terjesztése fölé nagyobb területeket (például: területek között). Ha szeretné, hogy meg, hogy a belső terheléselosztókkal vagy a sorok, olvassa el a [belső betöltése terheléselosztó – áttekintés](../load-balancer/load-balancer-internal-overview.md) és [Azure sorban várakozó és szolgáltatás Bus sorban várakozó - összehasonlítás és ellentétben](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)használatáról további információt.

__Mi történik, ha Ön nem használja egy terheléselosztó vagy várólista az alkalmazás minden réteg között?__ Nincs terheléselosztó, vagy várólista az alkalmazás minden réteg között nehezen méretezni az alkalmazást, felfelé vagy lefelé, és a betöltés terjesztése több számítógépen. Nem ezzel vezethet keresztül, vagy a kiépítési az erőforrások és a kockázat legrövidebb leállás és gyenge felhasználói felület, ha a forgalmat és rendszerhibák váratlan változásokat.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Az SQL-adatbázisok aktív geo replikációs használ? 
Aktív Geo replikációs lehetővé teszi a ugyanazon vagy másik, a régiók 4 olvasható másodlagos adatbázisok beállítása. Másodlagos adatbázisokat a szolgáltatási zavarok vagy az elsődleges adatbázishoz való csatlakozáshoz nem érhetők el. Ha szeretne többet megtudni az SQL-adatbázis aktív geo replikációs, olvassa el [áttekintése: SQL adatbázis aktív Geo replikációs](../sql-database/sql-database-geo-replication-overview.md).
 
 __Mi történik, ha Ön nem használja az active geo-replikáció az SQL-adatbázisok?__ Aktív geo-ismétlések nélkül, ha az elsődleges adatbázis minden eddiginél offline állapotba kerül (tervezett karbantartás, a szolgáltatási zavarok, hardver hiba, stb.) az adatbázis-alkalmazás lesz offline mindaddig, amíg a megfelelő állapot hozhat át az elsődleges adatbázis online állapotba. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Használja a gyorsítótár (Azure vgx.dll gyorsítótár) elé az adatbázisok?
Ha az alkalmazás egy adatbázis nagy terhelés van, amennyiben az adatbázis hívások többsége olvasása, az alkalmazás sebességének növeléséhez, és az adatbázis a betöltés csökkentése elé az adatbázis számára, ezeket a műveleteket olvasási gyorsítótárazási réteg végrehajtása. Az alkalmazás sebességének növeléséhez, és az adatbázis terhelés (tehát növekvő is képes kezelni méretarányának) csökkentése gyorsítótárazási réteg az adatbázis elé írva. Ha azt szeretné, ha többet szeretne megtudni az Azure vgx.dll gyorsítótár, olvassa el a [gyorsítótár útmutatást](../best-practices-caching.md).
 
 __Mi történik, ha Ön nem használja a gyorsítótár elé az adatbázis?__ Ha az adatbázis gépi hatékony elég a forgalom terhelés kezelésére helyezi el rajta, majd az alkalmazás válaszol, mint normál, abban az esetben, ha ez azt jelenti, hogy alsó terhelés meg fog kell fizet a egy adatbázis számítógépre, amely több költséges, mint szükséges. Ha az adatbázis gépi nem hatékony elég a terhelés kezelésére, akkor a program ekkor elkezdi gyenge felhasználói felület, majd tapasztal az alkalmazással (időtartam időkorlátok és esetleg szolgáltatás legrövidebb leállás).
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Akikkel kapcsolatba a Microsoft Azure támogatási Ha is meg fog jelenni a magas skála esemény?
Azure támogatási segítséget nyújtanak a szolgáltatás korlátozások kezeléséhez tervezett nagy forgalmat eseményekkel (például új termék indítást vagy speciális ünnepek) növelése. Azure-támogatás is lehet segít kapcsolatba szakértői ki tekintse át a tervező a fiók csoporthoz, és segítséget nyújtanak segítséget találhat magas skála esemény igényekhez a legjobb megoldás. Ha azt szeretné, megtudhatja, hogy Azure ügyfélszolgálatát olvashat, olvassa el a [Azure támogatja a gyakori kérdések](https://azure.microsoft.com/support/faq/).

__Mi történik, ha nem ügyfélszolgálatot Azure magas szintű esemény?__ Ha nem a kommunikáció, vagy megtervezése, a nagy forgalmat eseményeket, kockázat szerezze meg bizonyos [Azure szolgáltatások korlátozza](../azure-subscription-service-limits.md) , és így létrehozása a gyenge felhasználói felület (vagy ami még rosszabb, legrövidebb leállás) a rendezvény alatt. Építészeti ellenőrzések és a kommunikáció, akik előre túlfeszültség segíthetnek e kockázatok csökkentésében.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Használja a tartalom kézbesítési hálózati (Azure CDN) a webes elérésű BLOB-tároló és statikus eszközök elé?
A CDN segítségével előnyeinek betöltés ki a kiszolgáló által a CDN POP/él helyeken található a világon a tartalom gyorsítótárazás. Késés csökkentése és méretezhetőség növelése, csökkentheti a kiszolgáló betöltése, ezt megteheti és megtagadását service(DOS) támadások elleni védelem stratégia részeként. Ha szeretné, hogy Azure CDN használatát a tűrőképessége növelése és csökkentése a vevő késés további információkhoz juthat, olvassa el a [áttekintése, az Azure tartalom kézbesítési hálózati (CDN)](../cdn/cdn-overview.md).

__Mi történik, ha Ön nem használja az a CDN?__ Ha nem használ a CDN kattintson az összes az ügyfél forgalom megtalálható közvetlenül az erőforrások az. Ez azt jelenti, hogy a kiszolgálókat, ami csökkenti a méretezhetőség magasabb terhelések találhat. Emellett az ügyfelekkel merülhetnek fel újabb késések, CDNs a világon helyekre, amelyek valószínűleg közelebbi oda az ügyfelekre felajánl.

##<a name="next-steps"></a>A következő lépéseket:
Ha szeretne olvasni olvashat az magas elérhetőség, az alkalmazások megtervezése olvassa el a [Microsoft Azure-ra épülő alkalmazások magas elérhetőségét](resiliency-high-availability-azure-applications.md).
