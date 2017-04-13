<properties
   pageTitle="Vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége |} Microsoft Azure"
   description="Műszaki tekintse át, és z információt a Microsoft Azure-ra épülő alkalmazások magas rendelkezésre állásának és katasztrófa helyreállítás alkalmazások megtervezése."
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

#<a name="disaster-recovery-and-high-availability-for-applications-built-on-microsoft-azure"></a>Vészhelyreállítás és a Microsoft Azure-ra épülő alkalmazások elérhetőséget

##<a name="introduction"></a>– Bevezetés

Ez a cikk az Azure-ban futó alkalmazások magas elérhetősége koncentrál. Magas elérhetőség általános stratégia is tartalmaz az vészhelyreállítás a funkcióival. Hibák és a felhőben katasztrófák megtervezése megköveteli a hibák gyorsan felismer. Ezután alkalmazhat, amely megfelel a hibatűrést az alkalmazás legrövidebb leállás vonatkozó stratégia. Ezenkívül figyelembe kell vennie az alkalmazás elviseli anélkül, hogy az üzleti hátrányos következményeit, mint a vissza az adatok elvesztését mértékét.

A legtöbb cégek tegyük fel, ideiglenes és nagyméretű hibák elő őket. Azonban előtt magának kérdésre válaszol, nem a vállalat Időzítéspróba ezeket a hibákat? Tesztelje a helyreállítás annak érdekében, hogy a helyes folyamatok már a helyükön adatbázisok? Valószínűleg nem. Ennek az oka sikeres vészhelyreállítás sok tervezése és ezek a folyamatok végrehajtásához architecting kezdődik. Számos más nem funkcionális követelmények, például biztonsági, hasonlóan a vészhelyreállítás ritkán kapja meg a terveznie elemzésre és az idő terhelés igényel. A legtöbb cégek is, nincs telepítve a felesleges kapacitással földrajzilag elosztott terület költségvetés. Tehát még alapvető fontosságú alkalmazások gyakran tartoznak megfelelő helyreállítási katasztrófára.

A felhő platformok, például az Azure, és adja meg a világon földrajzilag szétszórt régiók. Következő platformokon is biztosít, amelyek támogatják a elérhetősége és katasztrófa helyreállítási esetek számos funkciók. Most, minden misszió kritikus felhő alkalmazást kell adni a rendszer katasztrófa ellenőrzéshez figyelembe. Azure tűrőképessége és a helyreállítás szolgáltatásai számos beépített tartalmaz. Gondosan tanulmányozása platform funkciókról, és az alkalmazás stratégiák kiegészítése.

Ez a cikk a körvonal a szükséges architektúra kell ajánlott lépéseket katasztrófa-vásárlási az Azure környezetben. Kattintson a nagyobb üzleti folytonosságot folyamat is alkalmazhat. Üzleti folytonosságot terv káros feltételek műveletek folyamatos vonatkozó tudnivalókat foglaltuk össze. Ez lehet például leállított szolgáltatás vagy természeti katasztrófa, például egy vihar vagy power üzemszünetek technológiával hibát. Katasztrófák az alkalmazás tűrőképessége a nagyobb katasztrófa helyreállítási folyamat csak egy részhalmazát NIST dokumentumtartalom leírtak szerint: [Készenléti tervezési útmutató az információtechnológiai rendszerek](https://www.fismacenter.com/sp800-34.pdf).

Az alábbi szakaszok a hibák, technikákat, amelyekkel foglalkozni az, és architektúrákban, amely támogatja az alábbi eljárások különböző mértékű határozza meg. Ezt az információt nyújt a beviteli a katasztrófa helyreállítási folyamat és eljárásokat annak érdekében, hogy a katasztrófa helyreállítási stratégia működik megfelelően és hatékonyan.

##<a name="characteristics-of-resilient-cloud-applications"></a>Rugalmas felhő alkalmazások jellemzői

Egy jól szervezett alkalmazás képes hibák taktikai szinten lévő állhat ellen, és azt is elviseli stratégiai rendszerbeli hibák régió szintre. Az alábbi szakaszok a referenciacikk olyan kifejezéseket hivatkozni szeretne a dokumentumban rugalmassá felhőszolgáltatások számos tulajdonságát leírására határozza meg.

###<a name="high-availability"></a>Magas elérhetősége

Egy könnyen hozzáférhető felhő alkalmazás stratégiák felvegye a üzemszünetek a függőségek, például a felügyelt, a felhőben platform által kínált szolgáltatások hajtja végre. Ezt a megközelítést ellentétben lehetséges hibák a felhőben platform funkciók, lehetővé teszi, az alkalmazás továbbra is a várt funkcionális és nem működési rendszeres jellemzők mutatnak. Ez a csatorna 9 videó sorozat meg szeretné vizsgálni vonatkozik [Failsafe: rugalmassá felhő architektúrákban útmutatást](https://channel9.msdn.com/Series/FailSafe).

Ha az alkalmazás, figyelembe kell vennie egy képesség üzemszünetek valószínűsége. Emellett célszerű az ütközés egy üzemszünetek van az alkalmazás az üzleti szempontból előtt mély a végrehajtási stratégiák be van. Határidő nélküli figyelembe veszi az üzleti gyakorolt és szerezze meg a kockázat feltétel valószínűsége, végrehajtása drága és lehet esetleg felesleges.

Fontolja meg egy nagy elérhetőség autós hasonlóan. Még minőség modulok és kiváló műszaki nem akadályozza meg alkalmi hibák. Például a autós egy egyszerű, strukturálatlan autógumit kap, amikor a autós továbbra is fut, de csökkentett funkcionalitással működik. Ha a potenciális előfordulás tervezett is használhatja egy adott vékony kerettel tartalék abroncsok amíg el nem éri a javítás szalon. Bár a tartalék autógumit gyors sebesség nem teszi lehetővé, továbbra is működtetheti a gépjármű mindaddig, amíg a autógumit cserél. Hasonlóképpen egy felhőalapú szolgáltatásba, amely a tervek az adatvesztést funkciók megakadályozhatja, hogy az általában viszonylag kisebb problémák arra a teljes alkalmazás lefelé. Az IGAZ, ha a felhőbeli szolgáltatástól csökkentett funkcionalitással kell futtatnia.

Létezik néhány főbb jellemzők a könnyen hozzáférhető felhőszolgáltatásba: elérhetőség, méretezhetőség és hibatűrést. Ezek a jellemzők kapcsolatban áll egymással, de fontos megtudhatja, hogy minden, és hogyan azok szerkeszthetik azokat a megoldást az általános elérhető.

###<a name="availability"></a>Elérhetőség

Az elérhető alkalmazások úgy véli, az alapul szolgáló infrastruktúra és a függő szolgáltatások elérhetőségét. Választható alkalmazások redundancia és rugalmassá tervezés hiba egyetlen pontjai eltávolítása. Amikor Ön bővítése szempontok elérhetősége Azure-ban, fontos megértéséhez, amely a platform hatékony elérhetőségét a. Hatékony elérhetősége úgy véli, hogy a szolgáltatás rendelkezést SZOLGÁLTATÁSISZINT-minden függő szolgáltatás, és a teljes rendszer elérhetősége göngyölt hatásukat.

Rendszer elérhetősége egy, a rendszer tudják működtetéséhez időkeret százalékos mértéke. Például az Azure-ban webszolgáltatás vagy dolgozó szerepkörbe legalább két példányainak SLA elérhetősége is 99.95 százalék (Házon kívül 100 százalék). Azt nem mérjük a teljesítmény vagy a szolgáltatások ezeket a szerepköröket futó funkcióját. A felhőalapú szolgáltatás hatékony elérhetősége azonban is érinti a különböző SLA egyéb függő szolgáltatást. A további mozgó elemeket a rendszer belül, a további körültekintéssel végre kell hajtania alkalmazásához is eltolódhasson felel meg a elérhetőségét a végfelhasználók számára.

Fontolja meg az Azure szolgáltatás, amely Azure szolgáltatásokat használ a következő SLA: számítási Azure SQL-adatbázis és Azure-tárterületet.

|Azure szolgáltatás|SLA   |A lehetséges perc legrövidebb leállás/hónap (30 nap)|
|:------------|:-----|:----------------------------------------:|
|Számítási      |99.95 %|21,6 perc                              |
|SQL-adatbázis |99.99 %|4.3-as perc                              |
|Tárhely      |99.90 %|43.2 perc                              |

Meg kell terveznie, esetleg különböző időpontokban nyissa szolgáltatások. Egyszerűsített példánkban összesen hány perc lehet le az alkalmazás havonta 108 perc. Egy 30 napos hónap 43,200 perc összesen tartalmaz. 108 perc összesen hány perc a 30 napos hónap (43,200 perc).25 százalékában. Ezzel kap egy hatékony 99.75 készültségi elérhetőségét a felhőalapú szolgáltatáshoz.

Azonban az ebben a cikkben leírt elérhetősége technikákat javíthatja ki. Például ha tervez folytatja, ha az SQL-adatbázis nem érhető el az alkalmazást, eltávolíthatja, az alábbi egyenlettel. Ez lehet, hogy jelenti, hogy a az alkalmazás csökkentett funkciókhoz, fut, így is üzleti követelmények is célszerű figyelembe venni. Azure SLA listáját olvassa el a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/)című témakört.

###<a name="scalability"></a>Méretezhetőség:

Méretezhetőség közvetlenül az elérhetőség befolyásolja. Olyan alkalmazás, amely nagyobb terhelés alatt sikertelen már nem érhető el. Egységes eredménnyel, az elfogadható idő windows megnövelt igényeknek méretezhető alkalmazások képesek. Ha a rendszer méretezhető, vízszintesen vagy függőlegesen méretezés kezelése nő a betöltés egységes teljesítmény megőrzésével. Az alapfogalmak vízszintes méretezés hozzáadja (processzor, a memória és sávszélesség), azonos méretű további gépek függőleges méretezési nő közben a meglévő gépek méretét. Az Azure a számítási különböző gépi méretű kijelölésére szolgáló függőleges méretezési lehetőségek állnak rendelkezésre. A gép méretének módosítása azonban csak egy újbóli telepítés. Ezért a legtöbb rugalmas megoldások készültek vízszintes méretezését. Ennek oka számítási, különösen igaz növelheti egyszerűen futó bármely webes vagy dolgozó szerepkör-példányok száma. Ezen további előfordulásait kezelheti a PowerShell-parancsfájlokat, a kód vagy a webes Azure portál keresztül nagyobb forgalmat. Alapnaptár: Ez az adott felügyelt mértékek nő döntést. Ebben az esetben mértékek és a felhasználó működésére nem érheti terhelés alatt csökkentheti. A webes és dolgozó szerepkörök általában a külső felekkel bármely állam tárolni. Rugalmas terheléselosztás és a biztonságos kezelésre példány megszámolja módosításait, így. Vízszintes méretezés is jól együttműködik az Azure tárolására, például nem adja meg a függőleges méretezés többszintű lehetőségei szolgáltatások.

Felhőalapú környezetekben a méretezés-mennyiségű gyűjteménye kell tekinteni. Ez lehetővé teszi, hogy az alkalmazás a végfelhasználók átviteli szükségleteinek karbantartási rugalmas lesz. A méretezés megadott egységek könnyebben ábrázolása a webről és az alkalmazás kiszolgáló szintjén. Ennek oka az Azure már tartalmaz állapot nélküli számítási csomópontok webes és dolgozó szerepkörök keresztül. Hozzáadása több számítja ki a méretezés-mennyiségű a telepítéshez nem okoz bármely alkalmazás állam management hatásai, mert számítási skála-mennyiségű állapot nélküli. A tároló időosztás egységet felelős partíciót adatok kezelésére szolgáló (strukturált és strukturálatlan). Tárolási skála-egységek többek között az Azure-táblából partíciót Azure Blob-tárolóhoz és Azure SQL-adatbázis. Még több Azure tároló fiókok használatát közvetlen hatása van az alkalmazás méretezhetőség. Egy erősen méretezhető felhőalapú szolgáltatásba, amelyekkel beépíthetők a több tárolási skála egységek tervezése Például ha egy alkalmazást használ, relációs adatok, partition az adatokat számos SQL-adatbázisok között. Ha így lehetővé teszi, hogy a tároló egység skála rugalmas számítási modell lépést tartani. Azure tároló hasonlóan lehetővé teszi, hogy a számítási réteg átviteli igényeknek szándékos látványtervek igénylő rendszerek szétválasztás adatok. Gyakorlati tanácsok méretezhető felhőszolgáltatások tervezéséhez listáját olvassa el a [Gyakorlati tanácsok az Azure Cloud Services nagyszabású szolgáltatások tervezés](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)című témakört.

###<a name="fault-tolerance"></a>Hibatűrést

Alkalmazások kell feltételezik, hogy minden függő felhő lehetőséget is, és a program elküldi bizonyos pontján időben. A hiba alternatív alkalmazások észleli és lépések körül sikertelen elemeket, a folytatáshoz és egy adott időszakon belül a helytelen eredményt adnak. Ideiglenes (tranziens) hibák keresése céljából hibafa alternatív rendszer ismét házirend fog alkalmazhat. Az komoly hibák az alkalmazás problémáinak feltárása és átadni alternatív hardvereszközt vagy készenléti tervek mindaddig, amíg a hiba kijavításakor. Egy megbízható alkalmazást is megfelelően a hibát az egy vagy több szakaszok kezelése, majd folytassa a megfelelően működik-e. Hibafa alternatív alkalmazások legalább tervezés stratégiák, például redundancia, replikációs vagy csökkentett funkció használható.

##<a name="disaster-recovery"></a>Vészhelyreállítás

Egy felhőalapú környezetben működik a függő szolgáltatások vagy az alapul szolgáló infrastruktúra egy rendszeres üzemszünetek miatt előfordulhat, hogy megszűnik. Olyan feltételekkel üzleti folytonosságot terv elindítja a katasztrófa helyreállítási folyamat. Ezt a folyamatot általában magában foglalja mind műveletek személyzeti és automatizált eljárások annak érdekében, hogy aktiválja újra az alkalmazást a rendelkezésre álló területen. Ehhez az új területet az alkalmazás felhasználóinak, az adatok és szolgáltatások átadását. Biztonsági másolat vagy a folyamatban lévő replikáció használatával is jár.

Fontolja meg az előző hasonlóan, az azt jelenti, hogy egy egyszerű, strukturálatlan autógumit való a tartalék helyreállítása magas elérhetőségét képest. Viszont vészhelyreállítás lépésből áll a után egy autós összeomlik venni, ahol a autós már nem működik. Ebben az esetben a legjobb megoldás, hogy hatékonyan módosítása autók, hívja fel a utazási szolgáltatás vagy egy ismerősét, hogy megtalálja. Ebben az esetben ott fogják lesz egy hosszabb késve vissza utazik. További összetettsége javítása, és térjen vissza az eredeti gépjármű is van. Megegyező módon egy másik területére vészhelyreállítás bonyolult feladat, amely általában magában foglalja az egyes legrövidebb leállás és adatvesztést. Szeretné jobban megismerni, és Vészhelyreállítási stratégiák meghatározása felmérése, fontos, hogy két feltételek megadása: helyreállítási idő cél (RTO) és a helyreállítási pont cél (Készletben).

###<a name="recovery-time-objective"></a>Helyreállítási idő célkitűzés

A RTO az a maximális időt alkalmazás funkciókat kínál kiosztva. Ez az üzleti követelmények alapján, és az alkalmazás fontossága kapcsolódik. Kritikus üzleti alkalmazások kell egy kis RTO.

###<a name="recovery-point-objective"></a>Helyreállítási pont célkitűzés

A Készletben a elfogadható időkeret vesznek el adatok a helyreállítási folyamat miatt. Például egy órával a Készletben esetén meg kell teljesen biztonsági mentése vagy bizonyos adatok legalább óránként. Miután, jelenítse meg az alkalmazást, helyettesítő területen az adatok biztonsági másolatának hiányozhat egy óra adatok felfelé. RTO, például a kritikus alkalmazások Megcélozhat egy sok kisebb Készletben.

##<a name="checklist"></a>Feladatlista

Vegyük összegzés Ez a cikk (és a kapcsolódó cikkek [magas rendelkezésre állásának](./resiliency-high-availability-azure-applications.md) és Azure alkalmazások [vészhelyreállítás](./resiliency-disaster-recovery-azure-applications.md) ) szereplő főbb pontjairól. Ez az összefoglaló ellenőrzőlista elemek fontolja meg a saját elérhetősége és helyreállítási katasztrófára jár. Ezek a változást komoly kapcsolatos végrehajtási sikeres megoldást megszerezni kérő ügyfeleknek hasznos tanácsokat.

1. Minden alkalmazáshoz kockázatbecslés lebonyolítása, mivel minden is eltérő követelményeik vannak. Egyes alkalmazások több kritikus, mint a többi, és a költség parancsot választva tervezővel őket vészhelyreállítás extra igazolására.
1. Ezen információk segítségével az egyes alkalmazások RTO és Készletben határozza meg.
1. A hiba, az alkalmazás architektúra kezdődő tervezés.
1. Gyakorlati tanácsok a magas elérhetőségét, végrehajtja a kockázat, költség és összetettsége terheléselosztás közben.
1. Katasztrófa helyreállítási csomagok és folyamatok hajtja végre.
  * Fontolja meg a modul szintjét teljes mértékben a teljes felhő üzemszünetek átterjedő hibák.
  * Hozzon létre hivatkozást, és a tranzakció alapú adatok biztonsági stratégiák.
  * Válassza a több elem webhely katasztrófa helyreállítási architektúra.
1. Egy adott tulajdonosa katasztrófa helyreállítási folyamat, automatizálási és tesztelése megadása A tulajdonos kell kezelni, és saját a teljes folyamat.
1. A folyamatok dokumentálása, így azok könnyebben megismételhető. Bár egy tulajdonosa, több személynek láthatja, és kövesse a folyamatok vészhelyzet esetén.
1. Az eljárás végrehajtásához a munkatársak betanítása.
1. Normál katasztrófa szimulációk használata képzés és a folyamat érvényességi is.

##<a name="summary"></a>Összefoglalás

Amikor a hardver és az alkalmazások nem sikerül Azure belül, a technikák és őket kezelésére szolgáló stratégiák eltérnek hiba esetén a helyszíni rendszeren. A fő ennek oka, hogy felhő megoldások általában további függőségek van, amely egy Azure terület elosztva, és a felügyelt szolgáltatások külön infrastruktúra. Meg kell magas elérhetősége módszereket részleges hibák foglalkozik. Szigorúbb hibák, a katasztrófa esemény miatt esetleg kezeléséhez használja a Vészhelyreállítási stratégiák meghatározása.

Azure észleli és sok hibák kezeli, de a hibák, hogy az alkalmazás-specifikus stratégiák sokféle közül. Ki kell aktívan előkészítése és a hibák alkalmazások, szolgáltatások és adatok kezelése.

Az alkalmazás elérhetőségét és katasztrófa helyreállítási terv létrehozása, vegye figyelembe az alkalmazás hibát üzleti következményeit. A folyamatok, házirendek és visszaállíthatja a kritikus rendszerek, miután egy katasztrofális esemény időt vesz igénybe eljárások meghatározása, tervezés és lekötési. És miután a tervek, hogy nem állítható le. Meg kell rendszeresen elemezni, tesztelése és az folyamatosan javítása a tervek az alkalmazás portfólió, az üzleti igények és az elérhető technológiák alapján. Azure új szolgáltatásokat nyújt, és hatványra sokoldalú alkalmazásokat, hogy hibák törés létrehozása új problémáit.

##<a name="additional-resources"></a>További források

[Microsoft Azure-ra épülő alkalmazások magas elérhetősége](resiliency-high-availability-azure-applications.md)

[Microsoft Azure-ra épülő alkalmazások vészhelyreállítás](resiliency-disaster-recovery-azure-applications.md)

[Azure tűrőképessége technikai útmutató](resiliency-technical-guidance.md)

[Áttekintés: A felhő üzleti folytonosságot és az adatbázis vészhelyreállítás az SQL-adatbázis](../sql-database/sql-database-business-continuity.md)

[Az Azure virtuális gépeken futó SQL Server magas rendelkezésre állásának és katasztrófa helyreállítás](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md)

[FailSafe: Rugalmas felhő architektúrákban útmutatást](https://channel9.msdn.com/Series/FailSafe)

[Gyakorlati tanácsok a nagyméretű szolgáltatások Azure Cloud Services tervezése](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/)

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a vészhelyreállítás és Azure-alkalmazásokhoz magas elérhetősége egy cikksorozat része. A sorozat a következő cikk [Microsoft Azure-ra épülő alkalmazások magas elérhetőségét](resiliency-high-availability-azure-applications.md).
