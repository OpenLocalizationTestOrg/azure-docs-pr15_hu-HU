<properties
   pageTitle="Útmutató gyorsítótárazás |} Microsoft Azure"
   description="Gyorsítótár-javítható a teljesítmény és méretezhetőség útmutatást."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/14/2016"
   ms.author="masashin"/>


# <a name="caching-guidance"></a>Gyorsítótár-útmutató

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

Olyan gyakori módszer, amely a teljesítmény és méretezhetőség: a rendszer fejlesztését célozza gyorsítótárazás. Mindezt ideiglenes gyors tárolóhoz közeli található a gyakran használt adatok másolása az alkalmazást. Ha a gyors adattárolás található közelebbi az alkalmazás, mint az eredeti forrás, majd gyorsítótárazás jelentősen javíthatja válaszidő-ügyfélalkalmazásokban adatok gyorsabban megtartásával.

Gyorsítótár esetén leghatékonyabb egy ügyfél-példány többször felolvassa ugyanazokat az adatokat, különösen akkor, ha az alábbi feltételek vonatkoznak az eredeti adatokat tároló:
- Az általában viszonylag statikus marad.
- Érdemes lassú és a gyorsítótár sebességének összehasonlítása.
- A magas szintű kérelem alá esik.
- Ha a hálózati késés okozhatják lassíthatja az access kéznél azt van.

## <a name="caching-in-distributed-applications"></a>Az elosztott alkalmazásokban gyorsítótárazás

Elosztott alkalmazások általában megvalósítása végezheti el a következő stratégiák esetén a gyorsítótár-adatok:

- A magánjellegű gyorsítótár, adatok tárolási helye helyi meghajtóra, amely az adott alkalmazás vagy szolgáltatás egy példánya fut a számítógépen használja.
- A megosztott gyorsítótár, ami több folyamatok és/vagy gépek elérhető gyakori adatforrásként szolgáló használatával.

Mindkét esetben gyorsítótárazás hajthatják ügyféloldali és/vagy a kiszolgálóoldali. A folyamat, amelyek a felhasználói felületet biztosít a rendszer, például egy webböngészőben vagy asztali alkalmazás által végzett ügyféloldali gyorsítótárazás.
Kiszolgálóoldali gyorsítótárazás befejeződött, a folyamat, amely a távolról futtatott üzleti szolgáltatásokat nyújt.

### <a name="private-caching"></a>A magánjellegű gyorsítótárazás

A gyorsítótár legalapvetőbb típusú egy memóriában áruházból. A címterület egyetlen folyamat tartott, és közvetlenül a folyamat futó kóddal érhető el. Ez a fajta gyorsítótár nagyon fontos eléréséhez. Statikus adatokat, mérsékelt mennyiségű tárolásához, mivel a gyorsítótár méretét a memória elérhető a számítógépen, a folyamat szolgáltatója hangerejének általában korlátozott egy rendkívül hatékony eszközöket is juthat.

További információt a memóriában fizikailag lehetőség, mint a gyorsítótárban van szüksége, ha a helyi fájlrendszerben írhat gyorsítótárban tárolt adatokat. Kisebb, mint a memóriában tartott adatok eléréséhez lesz, de továbbra is gyorsabb és megbízhatóbb, mint az adatok visszakeresése a hálózaton keresztül.

Ha több példányának egy alkalmazást, amely használja ezt a modellt, párhuzamosan fut, minden alkalmazás példány foglalja magában: saját eszközfüggetlen gyorsítótár tartó saját az adatok másolása

Érdemes a gyorsítótár az eredeti adatok bizonyos pontján pillanatképként múltbeli. Ha az adatok nem statikus, akkor valószínű különböző szolgáltatásalkalmazás-példányok azok gyorsítótárát az adatokat a különböző verzióival tartsa nyomva. Ezért ezekben az esetekben által elvégzett ugyanazon lekérdezés is eltérő eredményeket adnak, 1 ábrán látható módon.

![Az alkalmazás másik példányában egy memóriában gyorsítótár használata](media/best-practices-caching/Figure1.png)

_Ábra 1: Az alkalmazás másik példányában egy memóriában gyorsítótár használata_

### <a name="shared-caching"></a>A megosztott gyorsítótárazás

A megosztott gyorsítótár használatával is megszüntetheti aggályokat, hogy minden gyorsítótárát, amely akkor fordulhat elő, és a memóriában gyorsítótárazás adatok eltérhetnek. Megosztott gyorsítótárazás biztosítja, hogy más szolgáltatásalkalmazás-példányok kapni a azonos gyorsítótárban tárolt adatokat. Ezt nem megkeresésével a gyorsítótár külön helyen, általában egy külön szolgáltatás részét képező üzemeltetett, 2 ábrán látható módon.

![A megosztott gyorsítótár használata](media/best-practices-caching/Figure2.png)

_2. ábra: Egy megosztott gyorsítótár használata_

Egy fontos a megosztott gyorsítótárazási megközelítés előnye a méretezhetőség biztosít. Sok megosztott gyorsítótár-szolgáltatás kiszolgálók fürt használatával végrehajtását, és a szoftvert, az adatok elosztja a fürt átlátszó módon csatlakozást. Alkalmazás-példány egyszerűen kérést küld a gyorsítótár-szolgáltatás.
A gyorsítótárban tárolt adatokat a fürt helyének megállapítása az alapul szolgáló infrastruktúra feladata. A gyorsítótár további kiszolgálók hozzáadásával könnyen méretezheti.

Létezik a megosztott gyorsítótárazási megközelítés két legfontosabb hátránya:
- A gyorsítótár lassabban érhető el, mert azt már nem tartják helyileg minden alkalmazás példánnyal.
- Egy külön gyorsítótár-szolgáltatás végrehajtásához kötelező összetettsége hozzáadása előfordulhat, hogy a megoldás.

## <a name="considerations-for-using-caching"></a>Gyorsítótár használatával kapcsolatos szempontok

Az alábbi szakaszok részletesen ismertetik a tervezéséről és használatáról a gyorsítótár szempontjai.

### <a name="decide-when-to-cache-data"></a>Döntse el, hogy mikor kell az adatokat a gyorsítótárban

Gyorsítótár jelentősen javíthatja a teljesítményt, méretezhetőség és elérhetőség. Minél több adatot, amelyekhez és való ezeket az adatokat, annál nagyobb lesz gyorsítótárazás előnyei hozzáférést igénylő felhasználók minél nagyobb számát. Ennek az oka gyorsítótárazás csökkenti a késés és kérelem, amely az eredeti adatok áruházban egyidejű kérelmek nagy mennyiségű kezelésére van társítva.

Adatbázis például egyidejű kapcsolatok korlátozott számú lehet, hogy támogatja. Adatok beolvasása egy megosztott gyorsítótárból, azonban az alapul szolgáló adatbázis, nem pedig annak lehetővé teszi az adatok eléréséhez, akkor is, ha éppen elfogy a rendelkezésre álló kapcsolatok száma egy ügyfél alkalmazáshoz. Emellett az adatbázis nem érhető el, ha ügyfélalkalmazásokban előfordulhat, hogy tudja tartani az adatokat a gyorsítótárban lévő használatával továbbra is.

Fontolja meg az adatokat, olvassa el a gyakori el, de módosított ritkán (például az adatok, amely tartalmaz egy nagyobb részét olvasási műveleteket, mint az írási műveletek). Azonban nem javasoljuk a gyorsítótár használata a kritikus információk mérvadó áruházból. Ehelyett győződjön meg arról, hogy minden módosítás, hogy az alkalmazás nem engedheti meg magának elvesznek a egy állandó adattár mindig menti. Ez azt jelenti, hogy ha a gyorsítótár nem érhető el, az alkalmazás továbbra is működnek az adatokat tároló használatával, és nem vesznek el fontos információkat.

### <a name="determine-how-to-cache-data-effectively"></a>Adatok hatékony gyorsítótárban módjának meghatározása

A kulcs gyorsítótárat hatékony használatához meghatározásakor a gyorsítótár leginkább megfelelő adatokat, és a gyorsítótárazás, akkor a megfelelő időben helyezkedik el. Az adatokat a gyorsítótárban igény először azt veszi-alkalmazás bővíthető. Ez azt jelenti, hogy az alkalmazás csak egyszer az adatok beolvasása az adatok áruházból, és a későbbi access is teljesül, a gyorsítótár használatával.

Azt is megteheti a gyorsítótár is részben vagy egészben töltik adatokkal korábban, általában az alkalmazás indításakor (megközelítés rendezi a neve). Azonban nem feltétlenül tanácsos rendezi a nagyméretű gyorsítótárat, mivel ez a módszer is bevezetése az eredeti adatokat tároló hirtelen, nagy terhelés, az alkalmazás indításakor operációs rendszert futtató végrehajtásához.

Gyakran szokásai elemzésének is segít eldönteni, hogy szeretné-e teljesen vagy részben előre a gyorsítótár, illetve választhatja ki az adatokat a gyorsítótárban. Ha például hasznos lehet a gyorsítótárat a statikus felhasználói profiladatok rendezi, olyan ügyfeleknek, akik alkalmazással rendszeresen (talán minden nap), de nem az alkalmazás csak hetente használók számára.

Gyorsítótár általában jól adatokkal dolgozik, amely megváltoztatható vagy, ritkán változó. Többek között hivatkozás információt, például a termék és árak információkat egy e-kereskedelmi alkalmazást, vagy megosztott statikus források, amelyek költséges összeállításához. Egy vagy több adat töltődnek be a gyorsítótár alkalmazás indításakor minimalizálása érdekében erőforrásait igény szerint, és a teljesítmény javítása érdekében. Akkor is célszerű rendszeres frissítő hivatkozás háttér folyamat kell ahhoz, hogy a gyorsítótárban lévő adatok naprakész, vagy, amely a gyorsítótárat frissíti mikor hivatkozási adatok módosításokat.

Gyorsítótár érdemes kisebb dinamikus adatok a figyelmet bizonyos alóli kivételek ugyan (lásd a gyorsítótár erősen dinamikus adatok a jelen cikk további információ szakaszt). Az eredeti adatok rendszeresen megváltozik, amikor a gyorsítótárban lévő adatok válik elavult nagyon gyorsan, vagy a terhelést a a gyorsítótár szinkronizálása az eredeti adatokat tároló csökkenti a gyorsítótár hatékonyságát.

Ne feledje, hogy a gyorsítótár nem és a teljes adatok entitás. Például ha egy elemet egy többértékű objektum, például a nevet, címet és számla egyenlege bank ügyfél képvisel, ezeket az elemeket egy része maradhat statikus (például a név és a cím), míg mások (például a számla egyenlege) lehet, hogy több dinamikus. Ezekben az esetekben lehet hasznos lehet ahhoz, hogy az adatok statikus része gyorsítótár, és a többi adatot beolvasásához (vagy kiszámítása) szükség esetén.

Azt javasoljuk, hogy végezze el határozza meg, hogy szükség-e egy statisztikai sokaság előtti vagy igény szerinti betöltése a gyorsítótár vagy mindkettőt, tesztelése és az használatát elemzést teljesítményét. Döntés a és a az adatok használatát mintát kell alapján. Gyorsítótár kihasználtsági és a teljesítmény elemzése különösen fontos az alkalmazásokat, amelyek észlel nagy terhelés és erősen méretezhető kell lennie. Például erősen méretezhető helyzetekben értelme lehet a gyorsítótár csökkentheti a betöltés az adatokat tároló csúcs időpontokban rendezi.

Gyorsítótár is használható számítások ismétlődő, amíg fut az alkalmazás elkerülése érdekében. Ha egy művelet adatok átalakítások vagy elvégzi a számítást egy bonyolult, akkor a művelet eredményét a gyorsítótárban lévő mentheti. Ha ugyanazt a számítást később szükség, az alkalmazás egyszerűen meghallgathatja az eredmények a gyorsítótárból.

Az alkalmazások módosíthatók van a gyorsítótárban tárolt adatokat. Jó helyen jár javasoljuk, mint egy tranziens adattár bármikor sikerült eltűnik a gyorsítótár "témájú. Ne tároljon adatról csak; a gyorsítótárban Győződjön meg arról, hogy megmaradjanak az adatokat, valamint az eredeti adatok áruházból. Ez azt jelenti, hogy a gyorsítótár nem érhető el, ha Ön összezárása az adatvesztés hozzájuthat a szükséges.

### <a name="cache-highly-dynamic-data"></a>Gyorsítótár erősen dinamikus adatok

Amikor egy állandó adattárhoz információ gyorsan változó tárolja, azt is ír elő egy terhelést a rendszer. Például érdemes megvizsgálni folyamatosan jelentések állapot vagy valamilyen egyéb mérési eszközt. Ha egy alkalmazás nem gyorsítótár ezeket az adatokat, hogy szinte mindig lesz elavult a gyorsítótárban lévő adatok alapján, az azonos szempont igaz, ha tárolásához és retrieving ezt az információt az adatok áruházból lehet. A szükséges időt mentéséhez és az adatok beolvasása akkor előfordulhat, hogy megváltoztak.

Például ez esetben fontolja meg a dinamikus adatok tárolásának közvetlenül az állandó adattár helyett a gyorsítótár kiürítése a előnyeit. Ha az adatok nem kritikus, és nem kell-e a naplózás, majd nem számít alkalmi módosítása elvész.

### <a name="manage-data-expiration-in-a-cache"></a>A gyorsítótárban lévő adatok elévülési kezelése

A legtöbb esetben a gyorsítótár tartott adata adatok az eredeti adatok áruházban tartott másolatát. Az adatok az eredeti adatok áruházban meg lett gyorsítótárazott, okozó elavult válik a gyorsítótárban tárolt adatokat után megváltozik. Számos gyorsítótárazási rendszer lehetővé teszi az adatok lejár, és az időszak, amelynek adatok elavultak lehetnek csökkentésére gyorsítótár beállítása.

Ha lejár a gyorsítótárban tárolt adatokat, a gyorsítótárból törlődik, és az alkalmazás kell az adatok beolvasásához az eredeti adatok áruházból (azt is halasztani az újonnan beolvasása adatok gyorsítótárba). Beállíthatja, hogy egy alapértelmezett jelszólejárati házirendet a gyorsítótár beállításakor. A sok gyorsítótár-szolgáltatás akkor is úgy is, az érvényességi időszakának az egyes objektumokhoz Ha tárolja őket programozás útján a gyorsítótárban lévő.
Néhány gyorsítótárát engedélyezze, hogy adja meg az érvényességi időszakának, abszolút érték vagy az elemet, ha ez nem érhető el az adott idő alatt a gyorsítótárból eltávolítjuk okozó csúsztatható értékként. Ez a beállítás felülírja bármely gyorsítótár szintű jelszólejárati házirendjének, de csak a megadott objektumoknál.

> [AZURE.NOTE] Fontolja meg az érvényességi időszakának a gyorsítótár és gondosan benne az objektumokat. Ha használni szeretné túl rövid, objektumok túl gyorsan lejár, és csökkenti a gyorsítótár használatának előnyeit. Ha túl hosszú az időszak, akkor az adatok elévültek kockázat.

Lehetőség arra is, hogy a gyorsítótár előfordulhat, hogy Kitöltés felfelé Ha adatok túl hosszú ideig maradjanak rezidens engedélyezett. Ebben az esetben új elemek hozzáadása a gyorsítótár minden olyan kérések okozhat a kényszerített eltávolítjuk eviction enged az egyes elemeket. Gyorsítótár-szolgáltatás általában kizárása adatok alapon legkevésbé használatos nemrégiben (LRU), de általában a házirend felülbírálása és megakadályozhatja, hogy az elemek eltávolítandó. Jó helyen jár akkor fogad el ezt a módszert, ha Ön kockázat meghaladja a gyorsítótár rendelkezésre álló memória. Elem hozzáadása a gyorsítótár megpróbálja alkalmazás kivétellel leállhat meghiúsul.

Néhány gyorsítótárazási megvalósítás feltétlenül nyújt további eviction házirendek. Többféle eviction házirendek. Ezek a következők:
- A legutóbb használt házirend (az elvárásoknak, hogy az adatok nem lesz szükség újra).
- Először az első out házirend (legrégebbi adatokat először kizárt).
- Egy a indítóval kezdeményezett eseményeket (például az adatok módosítását) alapuló explicit eltávolító házirend.

### <a name="invalidate-data-in-a-client-side-cache"></a>Egy ügyféloldali gyorsítótárban lévő adatok érvényteleníti

Van ügyféloldali a gyorsítótárban tárolt adatokat általában tekintendő kívüli felügyelete a szolgáltatása, amely az adatokat az ügyfél számára. A szolgáltatás nem közvetlenül kényszerítése hozzáadásához vagy az adatok eltávolítása a gyorsítótárból ügyféloldali ügyfél.

Ez azt jelenti, hogy az ügyfél, amelyet használ egy megfelelően konfigurált gyorsítótár elavult információk használja továbbra is lehetséges. Például a jelszólejárati házirendekről a gyorsítótár nem szabályszerű ügyfél elavult információkat, amelyek az adatokat az eredeti adatforrás megváltozásakor helyi gyorsítótárban használható.

Webalkalmazás adatok szolgál a HTTP-kapcsolaton keresztül készítésekor, implicit módon kényszerítheti (például a böngészőben vagy web proxy) a webes ügyfél-ból a legfrissebb információkat. Ehhez egy erőforrás frissítése után az adott erőforrás URI megváltoztatásával. Webes ügyfelek általában egy erőforrás az URI ügyféloldali gyorsítótár kulcsként használatához, ha megváltozik a URI, a webes ügyfélprogram figyelmen kívül hagyja a bármelyik korábban gyorsítótárazott erőforrás verziója, és helyette beolvassa az új verzió.

## <a name="managing-concurrency-in-a-cache"></a>A gyorsítótár feldolgozási kezelése

Több példányának egy alkalmazást kell megosztania gyorsítótárát gyakran készült. Egyes alkalmazások példány elolvashatja és a gyorsítótárban lévő adatok módosítása. Ennek az azonos verzió-ellenőrzési problémák merülnek fel, bármelyik megosztott adatok áruházzal is vonatkoznak a gyorsítótár. Olyan helyzet, ha egy alkalmazást kell tartott a gyorsítótárban lévő adatok módosítása célszerű annak érdekében, hogy az alkalmazás egy példánya által végzett frissítések nem írják felül a egy másik példányában által végzett módosításokat.

Attól függően, hogy az adatok jellegét, és annak valószínűségét, hogy az ütközések is elfogadják feldolgozási két megközelítés egyikét:

- __Optimista.__ Az alkalmazás azonnal előtt az adatok frissítését, ellenőrzi, hogy akár a gyorsítótárban lévő adatok megváltozott beolvasása történt. Ha az adatok még mindig ugyanaz, a változást tehetők. Az alkalmazás, akkor frissítésének eldöntése a rendelkezik. (Ez döntés meghajtók üzleti logikai funkcióinak lesz az alkalmazás-specifikus.) Ezt a megközelítést alkalmas esetek, amikor hol frissítések ritka, vagy ha ütközések valószínűleg nem történik.
- __Pesszimista.__ Amikor beolvassa az adatokat, az alkalmazás zárolja a gyorsítótárban meg, hogy egy másik példányában módosításához. Ez a folyamat biztosítja, hogy ütközések nem fordulhat elő, de azok is megakadályozhatja a többi folyamat ugyanazokat az adatokat kell példányát. Pesszimista feldolgozási hatással lehetnek a méretezhetősége megoldást, és csak a műveletek rövid életű ajánlott. Lehet, hogy ezt a megközelítést megfelelő olyan esetekben, ahol ütközések akkor használják majd szívesen, különösen akkor, ha egy alkalmazás frissíti a gyorsítótárban lévő több elem és gondoskodnia kell arról, hogy ezek a változások következetesen alkalmazzák.

### <a name="implement-high-availability-and-scalability-and-improve-performance"></a>Jó elérhetőség és méretezhetőség alkalmazásához és a teljesítmény javítása

Kerülje a gyorsítótár, az elsődleges tárházba adatok; Ez az a szerepkör az eredeti adatokat tároló, amelyhez a gyorsítótár van kitöltve. Az eredeti adatokat tároló feladata biztosítása az adatok az adatmegőrzési.

Ügyeljen arra, hogy nem kritikus függőségek a rendelkezésre álló egy megosztott gyorsítótár-szolgáltatás vezetnek a megoldásokban. Az alkalmazások kell folytathatják működik, ha a megosztott gyorsítótárat a szolgáltatás nem érhető el. Az alkalmazás nem kell lefagy vagy nem sikerül folytatni szeretné a gyorsítótár-szolgáltatás várakozás közben.

Az alkalmazás emiatt a gyorsítótár-szolgáltatás az elérhető észleli és visszatérés az eredeti adatok áruházból, ha a gyorsítótár nem érhető el kell készíteni. A [minta megszakító](http://msdn.microsoft.com/library/dn589784.aspx) akkor hasznos, ebben az esetben kezelésére. A szolgáltatás, ahol a gyorsítótár állíthatók helyre, és amint elérhetővé válik a gyorsítótár adni, adatok olvasás az eredeti adatok mentése, például a [gyorsítótár-függetlenül mintát](http://msdn.microsoft.com/library/dn589799.aspx)stratégia követő űrlap.

Azonban előfordulhat, hogy egy méretezhetőség hatása a rendszer, ha az alkalmazás visszavált az eredeti adatokat tároló esetén a gyorsítótár átmenetileg nem érhető el.
Az adatokat tároló helyreállítása folyamatban, miközben az eredeti adatokat tároló sikerült kell az adatokat, így időtúllépései kéréseivel swamped, és nem sikerült a kapcsolatok.

Fontolja meg egy helyi, személyes gyorsítótár végrehajtása egy alkalmazást, és a megosztott gyorsítótár szolgáltatásalkalmazás-példányok hozzáférő minden példányában. Amikor az alkalmazás tölt le egy elemet, azt is ellenőrizni először a helyi gyorsítótár, majd kattintson a megosztott gyorsítótár, és végül az eredeti adatokat tárolja. Helyi gyorsítótár feltölthető legyen az adatok megosztott gyorsítótár vagy az adatbázisban, ha a megosztott gyorsítótár nem érhető el.

Ezt a módszert megakadályozhatja, hogy a helyi gyorsítótár túl elévültek a megosztott gyorsítótár részletez ügyeljen konfiguráció szükséges. Jó helyen jár a helyi gyorsítótár működik-e egy pufferelési, ha a megosztott gyorsítótár nem érhető el. A 3-as szám a struktúra jeleníti meg.

![A helyi, személyes gyorsítótár használata egy megosztott gyorsítótár](media/best-practices-caching/Caching3.png)
_ábra 3: egy helyi, személyes gyorsítótár használata egy megosztott gyorsítótár_

Néhány gyorsítótár-szolgáltatás támogatásához nagy gyorsítótárát, amely általában viszonylag élettartamú adatok tárolására, adja meg a olyan magas rendelkezésre állás beállítás, amely automatikusan átveszi hajtja végre, ha a gyorsítótár elérhetetlenné válik. Ezt a megközelítést magában foglalja a szokásos, a gyorsítótárban tárolt adatreplikálás egy elsődleges gyorsítótár-kiszolgáló másodlagos gyorsítótár-kiszolgálón tárolt, és váltás a másodlagos kiszolgáló, ha az elsődleges kiszolgáló nem sikerül vagy kapcsolat elvész.

Ha csökkenteni szeretné a késés, amely írás több helyre van társítva, a másodlagos kiszolgáló a replikáció előfordulhat, hogy tapasztalható aszinkron adatokat a gyorsítótárban írni az elsődleges kiszolgálón. Ezt a megközelítést végigvezeti a lehetőségét, hogy néhány a gyorsítótárban lévő adatok elveszhetnek hiba esetén, de ezeket az adatokat a arányát kell lennie kis és összehasonlítása a gyorsítótár teljes méretét.

Ha egy megosztott gyorsítótár túl nagy, a gyorsítótárban tárolt adatokat partition között a csomagváltás kérelem csökkentése és méretezhetőség javítása csomópontok hasznos lehet. Sok megosztott gyorsítótárát támogatja a dinamikusan hozzáadása (és eltávolítása) csomópontot, és az adatok visszaállás partíciót keresztül. Ezt a megközelítést előfordulhat, hogy foglalnak magukban fürtözés, amelyben csomópontok gyűjteménye bemutatják ügyfélalkalmazásokban, a zökkenőmentes, egyetlen gyorsítótár. Belső azonban az adatokat a rendszer szétosztja között egy előre definiált eloszlás stratégia, hogy a betöltés egyenletesen követő csomópontok között. Az [adatok particionáló útmutató](http://msdn.microsoft.com/library/dn589795.aspx) a Microsoft webhelyének a lehetséges particionáló stratégiák további információt tartalmaz.

Fürtözés is növelheti a gyorsítótár elérhetőségét. Nem sikerül egy csomópontot, a gyorsítótár maradéka esetén továbbra is elérhető.
Fürtözés gyakran használt replikációs és feladatátvételi együtt. Minden csomópont replikálhatók, és a replika gyorsan online állapotba hozhatók Ha nem sikerül egy a csomópontot.

Számos további, és az írási műveletek valószínűleg egyetlen adatértékeket vagy objektumok. Jó helyen jár időnként szükség lehet tárolni, vagy nagy mennyiségű adattal gyorsan beolvasásához.
Ha például rendezi a gyorsítótárban jelenthet száz vagy elemek ezer írása a gyorsítótár. Az alkalmazások lehet beolvasni a kapcsolódó elemek nagyszámú a gyorsítótárból a kérésben részeként is kell.

Sok nagyméretű gyorsítótárát adja meg a következő célokra köteg műveletek. Lehetővé teszi, hogy egy ügyfélalkalmazás csomag egyetlen kérelem-elemek nagy mennyiségű felfelé, és a csökkenti a kis kérések nagyszámú elvégzéséhez tartozó.

## <a name="caching-and-eventual-consistency"></a>Gyorsítótár és az esetleges konzisztencia

A gyorsítótár-függetlenül mintázat a munkát az alkalmazás a gyorsítótár feltöltő példányának hozzáféréssel kell rendelkeznie a legtöbbet a legutóbbi és következetes verziójára az adatokat. A rendszer, amely az esetleges konzisztencia (például egy replikált adatokat tároló) Ez nem lehet az esetet.

Az alkalmazás egy példánya esetleg módosítása egy elemet, és érvénytelenítik az adott elem gyorsítótárazott verzióját. Előfordulhat, hogy az alkalmazás egy másik példányát megpróbálja olvassa el ezt az elemet a gyorsítótárból, így azt az adatokat tároló beolvassa az adatokat, és hozzáadja őket a gyorsítótár mért sikertelen a gyorsítótár-találatok, okozó. Azonban az adatokat tároló nem lett teljesen szinkronizálva a kópiákkal, ha az alkalmazás-példány sikerült olvassa el és a gyorsítótár a régi értéket.

Adatok konzisztencia kezelésével kapcsolatos további tudnivalókért lásd: az [adatok konzisztencia alapozó](http://msdn.microsoft.com/library/dn589800.aspx) lapon a Microsoft webhelyén.

### <a name="protect-cached-data"></a>Gyorsítótárban tárolt adatok védelme

A gyorsítótár-szolgáltatás függetlenül használni, fontolja meg kell tartani az illetéktelen hozzáférés a gyorsítótárban lévő adatok védelmét. Van két fő vonatkozik:

- Az a gyorsítótárban lévő adatok védelme
- Az adatok védelme folyik át a gyorsítótár és az alkalmazás, amely használja a gyorsítótár között

A gyorsítótárban lévő adatok védelme érdekében a gyorsítótár-szolgáltatás hitelesítési módszer van szükség az, hogy alkalmazásokat adja meg a következőket, előfordulhat, hogy végrehajtása:
- Mely identitások érheti el a gyorsítótárban lévő adatok.
- Milyen műveleteket (olvasási és írási) ezek identitások végrehajtásához áll rendelkezésre.

Csökkentheti felsőbb, amely van társítva olvasásra és írásra az adatokat, identitás írási és/vagy olvasási hozzáférést a gyorsítótár, hogy identitás használhatja-e az adatokat a gyorsítótárban lévő megadása után.

Ha korlátozhatja a hozzáférést a gyorsítótárban tárolt adatokat részhalmazának van szüksége, tegye a következők valamelyikét:

- A gyorsítótár felosztása partíciót (használatával különböző gyorsítótár-kiszolgálók), és csak hozzáférést identitások a partíciók, amely kell tenni használni.
- Egyes Alkészlet adatainak titkosítására különböző használatával, és küldje el a titkosítási kulcsok csak az azonosítók, amely az egyes Alkészlet hozzáféréssel kell rendelkeznie. Ügyfélalkalmazás előfordulhat, hogy továbbra is a gyorsítótárban lévő adatok beolvasásához, de csak akkor tudja visszafejteni az adatokat, amelynek a billentyűk rendelkezik.

Az adatok védelme kell, folyik át a gyorsítótár és kijelentkezés is. Ehhez, attól függenek, a hálózati infrastruktúrát, hogy az ügyfél-alkalmazások használata a gyorsítótár biztonsági szolgáltatásait. Ha a gyorsítótár alkalmazása a ügyfélalkalmazásokban tároló a szervezeten belül helyszíni kiszolgálót használ, majd a hálózat magát a elkülönítési nem megkövetelheti további teendője. Ha a gyorsítótár távolról található, és a TCP- vagy HTTP-kapcsolat szükséges (például az interneten) nyilvános hálózaton keresztül, fontolja meg az SSL végrehajtása.

## <a name="considerations-for-implementing-caching-with-microsoft-azure"></a>Microsoft Azure gyorsítótárazás végrehajtási kapcsolatos szempontok

Azure az Azure vgx.dll gyorsítótár biztosít. Ez a Megnyitás, amely az Azure adatközpontban szolgáltatásként vgx.dll gyorsítótár-végrehajtását. Gyorsítótárazási szolgáltatás bármely Azure alkalmazásból is elérhető, hogy az alkalmazás alkalmazása egy felhőalapú szolgáltatásba, egy webhelyen vagy egy Azure virtuális gép belül biztosít. Gyorsítótárát, amely rendelkezik a megfelelő hívóbetű ügyfélalkalmazásokban is osztozhat.

Azure vgx.dll gyorsítótár nagy teljesítményű gyorsítótárazási megoldást, ahol az elérhetőség, méretezhetőség és biztonsági. Ez általában szolgáltatásként egy vagy több kijelölt gépek között. Kísérel meg minél több információt azt is, gyors hozzáférést biztosítani a memóriában tárolni. Ez a felépítés adja meg a kis késés és magas átviteli lassú bemeneti és kimeneti műveletet végre kell csökkentésével szolgál.

 Azure vgx.dll gyorsítótár a különböző API-khoz ügyfélalkalmazásokban által használt számos kompatibilis. Ha már használja az Azure vgx.dll gyorsítótár futó helyszíni meglévő alkalmazások, a az Azure vgx.dll gyorsítótár egy kész áttelepítési útvonalat a felhőben gyorsítótárazás biztosít.

> [AZURE.NOTE] Azure is tartalmaz az felügyelt gyorsítótár-szolgáltatás. Ez a szolgáltatás az Azure Service háló gyorsítótár motor alapul. Lehetővé teszi, hogy hozzon létre egy elosztott gyorsítótáron módszerektől párosított alkalmazások megosztható. A gyorsítótár-Azure adatközpontban rendszerű nagy teljesítményű kiszolgálókon helyezkedik el.
Ezt a beállítást azonban már nem ajánlott, és csak a meglévő alkalmazások használatához épített támogatási megadva. Az összes új fejlesztés használja inkább az Azure vgx.dll gyorsítótár.
>
> Ezenkívül Azure támogatja a szerepkör a gyorsítótár-a. Ez a szolgáltatás lehetővé teszi, hogy hozzon létre egy adott, ha egy felhőalapú szolgáltatásba gyorsítótárat.
A gyorsítótár üzemelteti egy webhely vagy dolgozó szerepkör példányát, és csak az ugyanazon felhőalapú szolgáltatás telepítési egység részeként működő szerepkörök is elérhető. (Telepítési időegység a egy felhőalapú szolgáltatásba, egy adott területre telepített példányokat szerepkör). A gyorsítótár csoportosított van, és a szerepkör belül a gyorsítótár üzemeltető ugyanazon telepítési egység összes előfordulását gyorsítótár fürt részévé válnak. Ezt a beállítást azonban már nem ajánlott, és csak a meglévő alkalmazások használatához épített támogatási megadva. Az összes új fejlesztés használja inkább az Azure vgx.dll gyorsítótár.
>
> Azure felügyelt gyorsítótár-szolgáltatás és az Azure szerepkör a gyorsítótár is vannak jelenleg nyár kerül piacra elavulása a 2016 November 16.
Áttelepítése a az Azure vgx.dll gyorsítótár e elavulása előkészítése az ajánlott. További tudnivalókért látogassa meg a   [Mi az Azure vgx.dll gyorsítótár felkínálása és milyen méretű érdemes használnom?](redis-cache/cache-faq.md#what-redis-cache-offering-and-size-should-i-use) a Microsoft webhelyén.


### <a name="features-of-redis"></a>Vgx.dll lehetőségei

 Több, mint egy egyszerű gyorsítótár-kiszolgáló vgx.dll. Az elosztott a memóriában adatbázis való egy teljes körű parancs, amely támogatja a számos, gyakori alkalmazási területek biztosít. Ezek a jelen dokumentum szakasz használatával vgx.dll gyorsítótárazás témakörben olvashat. Ez a szakasz nélkül összefoglalja, ahol vgx.dll fontosabb funkciói.

### <a name="redis-as-an-in-memory-database"></a>Egy a memóriában adatbázisként vgx.dll

Vgx.dll egyaránt támogatja a olvasási és írási műveletek. Vgx.dll, a rendszer meghibásodása, akár egy helyi pillanatkép fájlban, vagy csak hozzáfűző naplófájlban rendszeres tárolt írások védhetők. Ez nem a helyzet a sok gyorsítótárát (amely kell figyelembe venni átmeneti adatokat tárolja).

 Az összes írások aszinkron, és olvasásra és írásra adatok az ügyfelek nem blokkolja. Amikor vgx.dll indítás futtatása, azt beolvassa az adatokat a pillanatfelvétel vagy napló fájlt, és a memóriában gyorsítótár Egyenletszerkesztővel használja. További tudnivalókért lásd [adatmegőrzési vgx.dll](http://redis.io/topics/persistence) a vgx.dll webhelyen.

> [AZURE.NOTE] Vgx.dll nem garantálja, hogy minden írás Katasztrofális hiba esetén a program menti, de a legrosszabb adatok érdemes csupán néhány másodpercet elveszhetnek. Ne feledje, hogy a gyorsítótár nem szánt mérvadó adatforrásként használni, és az alkalmazások használata a gyorsítótár annak érdekében, hogy kritikus mentett adatok sikeres egy megfelelő adatokat tároló feladata. További információ a [gyorsítótár-függetlenül mintázat](http://msdn.microsoft.com/library/dn589799.aspx)témakörben talál.

#### <a name="redis-data-types"></a>Adattípusok vgx.dll

Vgx.dll egy kulcs-érték áruház, ahol értékeket tartalmazhat típusok egyszerű vagy összetett adatszerkezet tiltva, listák, például és állítja be. Ezek az adattípusok atomi műveletek halmazának támogat. Billentyűk lehetnek állandó vagy címkézett time-to-live, mely pontján a billentyű és a megfelelő érték automatikusan törlődjenek a gyorsítótár korlátozott. További információt a vgx.dll kulcsok és értékek látogassa meg a [adattípusok és vízkivételeket vgx.dll bemutatása](http://redis.io/topics/data-types-intro) a vgx.dll webhelyen.

#### <a name="redis-replication-and-clustering"></a>A replikáció vgx.dll és fürtözés

Fő/alárendelt replikációs rendelkezésre állásának és átviteli karbantartása vgx.dll támogatja. Írja be egy vagy több alárendelt csomópontok replikált műveletek vgx.dll fő csomópontra. Olvasás műveleteket is kell, melyet a diaminta vagy a beosztottak valamelyikét.

Egy hálózati partíciót megelőzve beosztott is visszaadhassa a kért adatokat, és majd átlátszó szinkronizálnia a fő, ha a kapcsolat helyreállt. További részleteket látogasson el a [replikáció](http://redis.io/topics/replication) a vgx.dll webhelyen.

Vgx.dll is biztosít fürtözés, amely lehetővé teszi, hogy átlátszó adatok partition be shards kiszolgálóin, és húzza szét a betöltés. Ez a funkció javítja a méretezhetőség, mivel bővíthető új vgx.dll kiszolgálók, és az adatokat, mint a gyorsítótár méretét újraparticionálja növeli.

Ezenkívül minden kiszolgáló a fürt replikálhatók fő/alárendelt replikációs használatával. Ezzel biztosíthatja, elérhetőségét a fürt minden csomópontjának keresztül. Fürtképzés és sharding kapcsolatos további tudnivalókért látogassa meg a [Vgx.dll fürt oktatóprogram lap](http://redis.io/topics/cluster-tutorial) a vgx.dll webhelyen.

### <a name="redis-memory-use"></a>Memóriahasználat vgx.dll

Egy vgx.dll gyorsítótár korlátja függvényt, a rendelkezésre álló az állomáson források függ. Egy vgx.dll kiszolgáló konfigurálásakor memória használhatja azt a maximális mérete is megadhat. Egy kulcsot, hogy az elévülési időt, amely után automatikusan eltávolítása a gyorsítótárból vgx.dll gyorsítótárban is beállítható. Ez a funkció is megakadályozzák a memóriában gyorsítótár kitöltés régi vagy elavult adatokkal.

Memória megtelik, mint vgx.dll is automatikusan kizárása kulcsok és értékeik számos házirendeket követve. Az alapértelmezett LRU (legalább a legutóbb használt fájlok), de más házirendek például véletlenszerűen kizárása billentyűk vagy eviction teljesen kikapcsolására (a, mely elemek hozzáadása a gyorsítótár sikertelen, ha a teljes eset kísérlet) is kijelölhet. A lapon, [Mint egy LRU gyorsítótár vgx.dll használatával](http://redis.io/topics/lru-cache) több információt biztosít.

### <a name="redis-transactions-and-batches"></a>Tranzakciók és kötegekben vgx.dll

Vgx.dll lehetővé teszi, hogy egy ügyfélalkalmazás küldhetik adatainak olvasása és írása atomi tranzakcióként gyorsítótár műveletek sorozata. A műveletben minden parancs futtatása egymás után garantált, és nincs más egyidejű ügyfelek által kibocsátott parancsok fog interwoven, közöttük.

Azonban ezek nem igaz tranzakciók relációs adatbázisból kell elvégeznie őket. Két szakasz – tranzakció feldolgozás áll az első akkor, ha a parancsok aszinkron vannak, és a második pedig a parancsok futtatásakor. A parancs várólista szakaszában a parancsok, a tranzakció alkotó nyújtja az ügyfélnek. Ha valamilyen hiba lép fel ezen a ponton (például szintaktikai hiba vagy helytelen számú paraméter) majd vgx.dll megtagadja a teljes tranzakció feldolgozása és figyelmen kívül hagyja, hogy.

A futtatási fázisban vgx.dll minden várólistás parancs végrehajtása sorrendben. Parancs nem sikerül a fázisban, ha vgx.dll a következő várólistás parancs továbbra is fennáll, és nincs visszaállíthatja a már futó parancsok hatásait. Az egyszerűsített űrlapon tranzakció segítséget nyújt a teljesítményét karbantartása és teljesítményét a kérelem okozó problémák elkerülése érdekében.

Vgx.dll optimista zárolási konzisztencia fenntartása segítésére űrlap hajtja végre. Részletes információt a tranzakciókat, és a vgx.dll zárolása látogasson el a [Tranzakciók lapon](http://redis.io/topics/transactions) a vgx.dll webhelyen.

Vgx.dll is támogat kérések nem tranzakció alapú kötegelés. Az ügyfél által használt parancsok küldése vgx.dll kiszolgálón vgx.dll protokoll lehetővé teszi, hogy az ügyfél küldje el a tevékenységek sorozatának a kérésben részeként. Ez segít a hálózaton csomag töredezettség csökkentése érdekében. Ha a köteg feldolgozása, minden parancs történik. Ha ezek a parancsok között hibás, azok elutasításra kerül (ez nem történik, a tranzakciók), de a további parancsok fog történni. Nincs kapcsolatban a sorrendben, amelyben a parancsokat a köteg dolgozza garancia is van.

### <a name="redis-security"></a>Biztonsági vgx.dll

Vgx.dll összpontosít tisztán adatok gyors hozzáférést biztosító, és egy megbízható környezet csak megbízható ügyfélalkalmazások úgy is elérhető belül futnak lett tervezve. A jelszó-hitelesítést alapján korlátozott biztonsági modell vgx.dll támogatja. (Ajánlatos hitelesítési eltávolításhoz, bár nem javasoljuk ezt.)

Az összes hitelesített ügyfelek megosztása globális ugyanazt a jelszót, és ugyanazokat az erőforrásokat hozzáféréssel rendelkeznek. Ha átfogóbb bejelentkezési biztonsági, be kell állítani a saját biztonsági réteg a vgx.dll kiszolgáló elé, és kéréseket a további rétegen kellene haladnia.. Vgx.dll kell nem közvetlenül kitenni nem megbízható vagy nem hitelesített ügyfelek.

Tiltsa le őket, vagy átnevezhetők (és csak a Yammerhez ügyfelek közöljük az új nevek), az access parancsok korlátozhatja.

Vgx.dll közvetlenül nem támogatja a titkosítást, bármilyen így összes kódolást kell elvégeznie ügyfélalkalmazások. Ezenkívül vgx.dll nem nyújt átviteli biztonsági bármilyen. Adatok védelme biztonsági szerint folyik át a hálózaton van szüksége, azt javasoljuk az SSL-proxyn végrehajtása.

További tudnivalókért látogassa meg a [biztonsági vgx.dll](http://redis.io/topics/security) a vgx.dll webhelyen.

> [AZURE.NOTE] Azure vgx.dll gyorsítótár saját biztonsági réteg, amelyen keresztül az ügyfélgépek hogyan kapcsolódnak biztosít. Az alapul szolgáló vgx.dll kiszolgálók nem érhetők el a nyilvános hálózati.

### <a name="using-the-azure-redis-cache"></a>Az Azure vgx.dll gyorsítótár használata

Az Azure vgx.dll gyorsítótár vgx.dll terméket futtató kiszolgálók kiszolgálókon tárolt az Azure adatközponthoz; a hozzáférést biztosít. a hozzáférés-vezérlés és az adatvédelmet homlokzati szerint működik. A gyorsítótár az Azure kezelőportálja segítségével is hozhatók létre. A portál számos előre definiált konfiguráció esetén egy dedikált szolgáltatás, amely támogatja az SSL kommunikáció (az adatvédelem) és a fő/alárendelt replikáció 99,9 % elérhetősége 250 MB-os le egy SLA gyorsítótár (elérhetőség garanciáit) megosztott hardver futó varianciaanalízis ismétlések nélkül futtatott 53GB gyorsítótárból alaptartományban.

Az Azure adatkezelési portál használatával, is konfigurálja a gyorsítótár a eviction házirendet, és a gyorsítótárban való hozzáférés szabályozása adja hozzá a felhasználókat a szerepkörökhöz adni. Tulajdonos, a közös munka, olvasó. Ezeket a szerepköröket határozza meg, hogy a tagok végezhető műveletek. Például a tulajdonos szerepkör tagjai a gyorsítótár (ideértve a biztonság), és annak tartalmának teljes hozzáféréssel rendelkeznek, vonatkozó munkatársi szerepkörök tagjai, olvassa el, és írja be a gyorsítótárban lévő adatok és a olvasó szerepkör tagjai csak meghallgathatja adatok a gyorsítótárból.

A legtöbb felügyeleti feladatok megtörténik az Azure adatkezelési portálon keresztül, és emiatt számos a felügyeleti vgx.dll normál verzióját elérhető parancsok nem érhetők el, a konfigurációs programozás útján, módosítása és leállítása vgx.dll kiszolgáló konfigurálása további slaves, illetve kényszerített lemezre mentése adatokat.

Az Azure adatkezelési portálon megtalálhatók a kényelmes grafikus megjelenítheti, amely lehetővé teszi, hogy figyelheti a gyorsítótár teljesítményét. Például megtekintheti az éppen létrehozott kapcsolatok száma, kérések száma végre hangerejének olvasása és írása, és a gyorsítótár számát és a gyorsítótár mért sikertelen találatok találatok. Használata ezt az információt a meghatározhatja, hogy a gyorsítótár hatékonyságát, és ha szükséges egy másik konfigurációt váltani, vagy eviction házirend módosítása. Ezenkívül figyelmeztetések, amelyek e-mailt küldeni a rendszergazdával, ha egy várt tartományon kívül esik egy vagy több kritikus mértékek hozhat létre. Például a gyorsítótár mért sikertelen találatok száma meghaladja a egy megadott értéket az utolsó órában, ha a rendszergazda sikerült értesítést, előfordulhat, hogy a gyorsítótár túl kicsi vagy adatokat is lehet eltávolítandó túl gyorsan.

Figyelheti a Processzor, a memória és a hálózati használatát a gyorsítótár is.

További információk és ábrázoló létrehozása és konfigurálása az Azure vgx.dll gyorsítótár példák látogasson el a lap [Lap Azure vgx.dll gyorsítótár körül](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) az Azure blogjában.

## <a name="caching-session-state-and-html-output"></a>Gyorsítótárazási munkamenet és HTML kimenet

Ha, épület ASP.NET webalkalmazások, hogy futtatni Azure webes szerepkörök használatával, mentheti munkamenet állapot adatai, és a HTML-kimeneti az Azure vgx.dll gyorsítótár. A munkamenet-állapot szolgáltató Azure vgx.dll gyorsítótár lehetővé teszi, hogy a munkamenet információ megosztása között ASP.NET-webalkalmazások másik példányát, és a webes farm esetekben, ahol ügyfél-kiszolgáló affinitás nem érhető el, és a gyorsítótár-munkamenet adatok a memóriában nem lenne megfelelő nagyon hasznos.

A munkamenet állapot-szolgáltató használata az Azure vgx.dll gyorsítótár kézbesíti számos előnyét, például:

- Azt is megoszthatja a munkamenet-állapot példányokat egy ASP.NET webalkalmazás, továbbfejlesztett méretezhetőség, mert sok egyik ablaktábláról a másikra
- Az azonos munkamenet-állapot adatai ellenőrzött, egyidejű hozzáférést támogatja a több olvasók és egyetlen írót, és
- A tömörítési memóriafoglalási és hálózati teljesítmény javítása használhatja.

További információt keresse fel a Microsoft webhelyének a [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótár](redis-cache/cache-aspnet-session-state-provider.md) lapra.

> [AZURE.NOTE] Nem használható a munkamenet-állapot szolgáltató Azure vgx.dll gyorsítótár kívül az Azure környezetben futtatott ASP.NET-alkalmazásokat. A várakozási férnek hozzá a gyorsítótár az Azure-Ön kívüli kizárható a teljesítmény előnyei a gyorsítótár-adatok.

Hasonlóképpen a Weblapkimeneti gyorsítótár szolgáltató Azure vgx.dll gyorsítótár lehetővé teszi, hogy a HTTP-válaszokat ASP.NET-webalkalmazások által generált. A Weblapkimeneti gyorsítótár-szolgáltató használata Azure vgx.dll gyorsítótár javíthatja a válaszidő az alkalmazásokat, amelyek összetett HTML kimenet; jeleníti meg szolgáltatásalkalmazás-példányok létrehozása hasonló válaszok fel, hogy a megosztott kimeneti töredékek a gyorsítótár, hanem az létrehozása a HTML, kimeneti határolni használja.  További információt keresse fel a Microsoft webhelyének a [ASP.NET kimeneti gyorsítótár-szolgáltató Azure vgx.dll gyorsítótár](redis-cache/cache-aspnet-output-cache-provider.md) lapon.

### <a name="azure-redis-cache"></a>Azure vgx.dll gyorsítótár

Azure vgx.dll gyorsítótár-Azure adatközponthoz futtatott vgx.dll kiszolgálók hozzáférést biztosít. A hozzáférés-vezérlés és az adatvédelmet homlokzati szerint működik. A gyorsítótár az Azure portálon is hozhatók létre.

A portál számos előre elkészített beállítások. SSL kommunikáció (az adatvédelem) és a fő/alárendelt replikáció 99,9 % elérhetősége le egy megosztott hardver futó (elérhetőség garanciáit) varianciaanalízis ismétlések nélkül 25 0 MB-gyorsítótár-SLA támogató ezek tartomány dedikált szolgáltatásként fut 53 GB gyorsítótárból.

Az Azure portál használata esetén is konfigurálja az eviction házirendet a gyorsítótár, és a gyorsítótárban való hozzáférés szabályozása adja hozzá a felhasználókat a szerepkörök, feltéve hogy.  Ezek a szerepkörök, amely meghatározza, hogy a tagok végrehajtható műveletek, a tulajdonos, a közös munka és az olvasó tartalmazza. Például a tulajdonos szerepkör tagjai a gyorsítótár (ideértve a biztonság), és annak tartalmának teljes hozzáféréssel rendelkeznek, vonatkozó munkatársi szerepkörök tagjai, olvassa el, és írja be a gyorsítótárban lévő adatok és a olvasó szerepkör tagjai csak meghallgathatja adatok a gyorsítótárból.

A legtöbb felügyeleti feladatok az Azure portálon keresztül hajt végre. Emiatt sok vgx.dll normál verzióját felügyeleti parancsok nem érhetők el, és a konfigurációs programozás útján módosítása, állítsa le a vgx.dll kiszolgáló, további beosztott konfigurálása vagy adatok kényszerített mentsen a lemezre.

Az Azure portálon megtalálhatók a kényelmes grafikus megjelenítheti, amely lehetővé teszi, hogy figyelheti a gyorsítótár teljesítményét. Megtekintheti például arról, végrehajtását kérések száma, olvasása és írása, hangerejének kapcsolatok száma és a gyorsítótár találatok és a gyorsítótár mért sikertelen találatok száma. Használja ezt az információt, akkor a gyorsítótár hatékonyságának meghatározására és szükség szerint egy másik konfigurációt szeretne váltani, vagy eviction házirend módosítása.

Ezenkívül figyelmeztetések, amelyek e-mailt küldeni a rendszergazdával, ha egy várt tartományon kívül esik egy vagy több kritikus mértékek hozhat létre. Érdemes lehet például riasztást, a rendszergazda Ha gyorsítótár mért sikertelen találatok száma meghaladja a egy megadott értéket az elmúlt egy órában, mert azt jelenti, lehet, hogy a gyorsítótár túl kicsi vagy adatok előfordulhat, hogy kell eltávolítandó túl gyorsan szeretne.

Figyelheti a Processzor, a memóriahasználat, és a hálózati használatát a gyorsítótár is.

További információk és ábrázoló létrehozása és konfigurálása az Azure vgx.dll gyorsítótár példák látogasson el a lap [Lap Azure vgx.dll gyorsítótár körül](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) az Azure blogjában.

## <a name="caching-session-state-and-html-output"></a>Gyorsítótárazási munkamenet és HTML kimenet

Ha szeretne összeállítani ASP.NET-webalkalmazásokat, hogy futtatni Azure webes szerepkörök használatával, mentheti a munkamenet adja meg az Azure vgx.dll gyorsítótárban HTML kimenet és információk. A munkamenet-állapot szolgáltató Azure vgx.dll gyorsítótár lehetővé teszi, hogy a munkamenet információ megosztása között ASP.NET-webalkalmazások másik példányát, és a webes farm esetekben, ahol ügyfél-kiszolgáló affinitás nem érhető el, és a gyorsítótár-munkamenet adatok a memóriában nem lenne megfelelő nagyon hasznos.

A munkamenet állapot-szolgáltató használata az Azure vgx.dll gyorsítótár kézbesíti számos előnyét, például:

- ASP.NET webalkalmazások példányainak sok tartalmazó megosztási munkamenet állapota.
- Továbbfejlesztett méretezhetőség biztosítása.
- Több olvasók és egy egyetlen írót támogató ellenőrzött, egyidejű hozzáférést az azonos munkamenet-állapot adatai.
- A memóriahasználat, és a hálózati teljesítmény javítása tömörítés használatával.

További tudnivalókért látogassa meg a [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótára](redis-cache/cache-aspnet-session-state-provider.md) a Microsoft webhelyén.

> [AZURE.NOTE] Ne használjon a munkamenet-állapot szolgáltató Azure vgx.dll gyorsítótár kívül az Azure környezetben futtatott ASP.NET-alkalmazásokat. A várakozási férnek hozzá a gyorsítótár az Azure-Ön kívüli kizárható a teljesítmény előnyei a gyorsítótár-adatok.

Hasonlóképpen a weblapkimeneti gyorsítótár szolgáltató Azure vgx.dll gyorsítótár lehetővé teszi, hogy a HTTP-válaszokat ASP.NET-webalkalmazások által generált. A weblapkimeneti gyorsítótár-szolgáltató használata Azure vgx.dll gyorsítótár javíthatja válaszidő az alkalmazásokat, amelyek összetett HTML kimenet jeleníti meg. Hasonló válaszok készítése szolgáltatásalkalmazás-példányok fel, hogy a megosztott kimeneti töredékek a gyorsítótár, hanem az létrehozása a HTML, kimeneti határolni használatát. További tudnivalókért látogassa meg a [ASP.NET kimeneti gyorsítótár-szolgáltató Azure vgx.dll gyorsítótára](redis-cache/cache-aspnet-output-cache-provider.md) a Microsoft webhelyén.

## <a name="building-a-custom-redis-cache"></a>Egyéni vgx.dll gyorsítótár létrehozása

Azure vgx.dll gyorsítótár működik-e egy homlokzati mögöttes vgx.dll kiszolgálókhoz. Jelenleg támogatja a konfigurációk készletét, de nem nyújt fürtözéshez vgx.dll. Ha szüksége van egy speciális konfigurációs, amely nem vonatkozik az Azure vgx.dll gyorsítótár (például 53 GB-nál nagyobb gyorsítótár) össze, és saját vgx.dll kiszolgáló állomás Azure virtuális gépeken futó használatával.

Ennek oka a potenciálisan bonyolult folyamat létrehozása több VMs használni kívánt alárendelt és csomópontok, ha azt szeretné, való replikáció megvalósítása kell. Ezenkívül szeretne fürt létrehozása, majd szükségességének több diaminta és alárendelt kiszolgálók. A minimális csoportosított topológiát, ahol az elérhetőség és méretezhetőség magas fokú magában foglalja a legalább hat VMs három fő/alárendelt kiszolgálók (fürt legalább három fő csomópontokat tartalmaznia kell) pár rendezve.

Minden fő/alárendelt pár együttes bezárása kell elhelyezni a késleltetés csökkentése érdekében. Azonban minden egyes készletben pár futhat különböző Azure adatközpont esetén olvassa el a különböző régiók, ha másnak szeretné, keresse meg a gyorsítótárban tárolt adatokat közelébe az alkalmazásokat, amelyek valószínűleg használatához. A Microsoft webhelyének [A egy CentOS Linux virtuális Azure-ban futó vgx.dll](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) lap végigvezeti egy példa szemlélteti build, és állítsa be az Azure virtuális futtatott vgx.dll csomópontot.

[AZURE.NOTE] Felhívjuk a figyelmét arra, hogy ha a saját vgx.dll gyorsítótár ilyen alkalmazza, akkor felelősek figyelés, kezelése és a szolgáltatás biztonságossá tétele.

## <a name="partitioning-a-redis-cache"></a>Egy vgx.dll gyorsítótár szétválasztás

A gyorsítótár szétválasztás magában foglalja a gyorsítótár felosztása több számítógépei között. Ez a struktúra lépve számos előnye egyetlen gyorsítótár-kiszolgálóval, például:

- A-gyorsítótár létrehozása sokkal nagyobb, mint egy kiszolgálón tárolhatja.
- Adatok terjesztése elérhetősége javítására kiszolgálóin. Ha egy kiszolgáló nem sikerül vagy elérhetetlenné válik, az adatokat, hogy tart fenn, nem érhető el, de az adatok hátralévő kiszolgálókon továbbra is elérhető. A gyorsítótár ez nem kulcsfontosságú mivel a gyorsítótárban tárolt adatokat csak az adatokat, hogy az adatbázis tárolási tranziens példányát. Gyorsítótárazott adatokat kiszolgálón van, amely elérhetetlenné válik gyorsítótárba helyezhető egy másik kiszolgálón helyette.
- A betöltés terjesztése kiszolgálóin, ezáltal a teljesítmény és méretezhetőség javítására.
- Geolocating adatok zárja be a felhasználóknak, hogy hozzáférhessenek, így csökken a késés.

A gyorsítótár a leggyakoribb szétválasztás formátuma sharding. Ez a stratégia összes partíciót (vagy shard) saját jobb vgx.dll gyorsítótárat. Adatok egy adott partíciót átirányításához sharding logika, amelyek használatával az alábbi módszerek sokféle terjesztése az adatok használatával. A [minta Sharding](http://msdn.microsoft.com/library/dn589797.aspx) sharding végrehajtási további információt tartalmaz.

A vgx.dll gyorsítótár szétválasztás végrehajtásához hajthatók végre az alábbi eljárások egyikét:

- _Kiszolgálóoldali lekérdezés útválasztás._ Ez a módszer az ügyfélalkalmazás egy kérést küld a gyorsítótár (valószínűleg a legközelebbi kiszolgáló) alkotó vgx.dll kiszolgálók. Minden vgx.dll kiszolgáló, amely leírja, hogy rendelkezik, és más kiszolgálókon arról, hogy melyik partíciót található információkat is tartalmaz, a partíciót metaadatok tárolja. A vgx.dll kiszolgálón vizsgálja meg az ügyfél kérése. Helyi meghajtóra megoldható, ha azt végez a kért műveletet. Egyéb esetben továbbítja a megfelelő kiszolgálón megjelenítheti a kérést. A modell vgx.dll fürtözés megvalósítani, és a vgx.dll webhelyen [fürt oktatóprogram vgx.dll](http://redis.io/topics/cluster-tutorial) lapon részletesebb leírása. Vgx.dll ürtözés ügyfélalkalmazásokban elemre átlátszóvá, és további vgx.dll kiszolgálók anélkül, hogy be újra az ügyfelek adhatók hozzá a fürt (és az adatok újbóli particionálva).

- _Ügyféloldali szétválasztás._ Ebben a modellben az ügyfélalkalmazás tartalmaz logikájának (valószínűleg abban a formában, tár) kérések irányítja a megfelelő vgx.dll kiszolgálóhoz. Ezt a módszert kínál Azure vgx.dll gyorsítótár. Több Azure vgx.dll gyorsítótárát (egy, az egyes adatok partíciók) létrehozása, és a kérések irányítja a megfelelő gyorsítótárba ügyféloldali logikájának végrehajtása. Ha a particionálási séma (Ha további Azure vgx.dll gyorsítótárát létrehozásakor, például) változik, ügyfélalkalmazásokban lehet kell kell újrakonfigurálni.

- _A proxykiszolgáló segítik szétválasztás._ Ez a rendszer az alkalmazások küldés közvetítő proxyszolgáltatás megérti, hogy hogyan particionálva van-e az adatokat, amely arra kéri, és a kérelem irányítja a megfelelő ügyfél vgx.dll kiszolgáló. Ez a módszer is kínál Azure vgx.dll gyorsítótár; a proxyszolgáltatás az Azure felhőszolgáltatásba-ként hajthatók végre. Ezt a megközelítést igényel egy további szintet összetettsége megvalósítását a szolgáltatást, és kérések tarthat végrehajtásához ügyféloldali szétválasztás történő használata helyett.

Az oldal [particionálására: hogyan felosztása több vgx.dll példányok között adatok](http://redis.io/topics/partitioning) a vgx.dll a webhely információt tartalmaz a további vgx.dll a szétválasztás végrehajtása.

### <a name="implement-redis-cache-client-applications"></a>Gyorsítótár ügyfélalkalmazásokban vgx.dll megvalósítása

Vgx.dll támogatja az ügyfél-alkalmazások számos programozási nyelven. Ha a .NET-keretrendszer használatával hoz létre új alkalmazást, a javasolt megközelítés a StackExchange.Redis ügyfél tárat. A tár modellel a .NET-keretrendszer objektum abstracts vgx.dll kiszolgálóhoz való csatlakozás, parancsok küldése és fogadása a válaszokat részleteit. Érhető el a Visual Studio NuGet csomag. Ez a tár egy Azure vgx.dll gyorsítótár vagy egyéni vgx.dll gyorsítótár egy virtuális is is használhatja.

Egy vgx.dll kiszolgálóhoz való csatlakozáshoz használhatja a statikus `Connect` módszer a `ConnectionMultiplexer` osztály. A kapcsolatot, ezzel a módszerrel hoz létre az célja, hogy az ügyfélalkalmazás élettartama felhasználható, és több egyidejű szálak használhatja ugyanazt a kapcsolatot. Nem szinkronizálódnak, és kapcsolat bontása, minden alkalommal, amikor vgx.dll műveletet hajt végre, mert ez csökkenthetik a teljesítményt.

Megadhatja, hogy a kapcsolat paramétereket, például a a vgx.dll állomás címét és jelszavát. Azure vgx.dll gyorsítótár használja, ha a jelszó vagy a elsődleges és másodlagos kulcsfontosságú, Azure vgx.dll gyorsítótár az Azure adatkezelési portál használatával létrehozott.

Miután csatlakozott a vgx.dll kiszolgáló, a fogópont a vgx.dll adatbázishoz, amely végpontként a gyorsítótár szerezhet be. A vgx.dll kapcsolat biztosítja a `GetDatabase` ehhez módot. Ezután elemek beolvashatja a gyorsítótár és tárolja az adatokat a gyorsítótárban lévő használatával a `StringGet` és `StringSet` módszereket. Ezeket a módszereket paraméterként kulcs számíthat, és lépjen vissza az elemet, vagy a gyorsítótárban egyező értéket tartalmazó (`StringGet`), vagy vegye fel az elemet a gyorsítótár kulccsal (`StringSet`).

Attól függően, hogy a vgx.dll kiszolgáló helyét sok művelet előfordulhat, hogy merülnek fel néhány késés kérelmének eljussanak a kiszolgálón, mialatt a választ függvény ad vissza, az ügyfél számára. A StackExchange tár ez aszinkron a módszereket, akkor marad válaszol ügyfélalkalmazásokban érdekében közzététele számos. Ezeket a módszereket a .NET-keretrendszer az [Aszinkron mintát feladatalapú](http://msdn.microsoft.com/library/hh873175.aspx) támogatást.

A következő kódrészletet látható nevű metódus `RetrieveItem`. A gyorsítótár-függetlenül minta alapján vgx.dll és a StackExchange tár megvalósítása mutatja. A módszer fő karakterláncérték tart, és próbálja meg megszerezni a megfelelő elemre a vgx.dll gyorsítótárból hívja fel a `StringGetAsync` módszert (az aszinkron verzióját `StringGet`).

Ha az elem nem található, a mögöttes adatok forrása használja a beolvasása a `GetItemFromDataSourceAsync` módszer (ami egy helyi módszert, és nem része a StackExchange tár). Nem majd adja hozzá a gyorsítótár használatával a `StringSetAsync` módszer, így tudja visszaszerezni gyorsabban legközelebb.

```csharp
// Connect to the Azure Redis cache
ConfigurationOptions config = new ConfigurationOptions();
config.EndPoints.Add("<your DNS name>.redis.cache.windows.net");
config.Password = "<Redis cache key from management portal>";
ConnectionMultiplexer redisHostConnection = ConnectionMultiplexer.Connect(config);
IDatabase cache = redisHostConnection.GetDatabase();
...
private async Task<string> RetrieveItem(string itemKey)
{
    // Attempt to retrieve the item from the Redis cache
    string itemValue = await cache.StringGetAsync(itemKey);

    // If the value returned is null, the item was not found in the cache
    // So retrieve the item from the data source and add it to the cache
    if (itemValue == null)
    {
        itemValue = await GetItemFromDataSourceAsync(itemKey);
        await cache.StringSetAsync(itemKey, itemValue);
    }

    // Return the item
    return itemValue;
}
```

A `StringGet` és `StringSet` módszerek nem állnak beolvasása vagy a karakterlánc értékek tárolására korlátozott. Minden olyan elem, amely bájtok számát tömbként szerializált tudnak venni. .NET-objektum mentése van szüksége, ha szerializálni a bájt adatfolyamként és használható a `StringSet` módszer a gyorsítótár írni.

Hasonlóképpen, erről objektum a gyorsítótárból használatával a `StringGet` módot és a .NET objektumként deszerializálása azt. A következő kód szemlélteti a IDatabase felület bővítmény módszerei halmazának (a `GetDatabase` vgx.dll kapcsolat metódusát adja eredményül egy `IDatabase` objektum), és az írási és olvasási módszerekről az alábbi használó minta kód egy `BlogPost` a gyorsítótár-objektumot:

```csharp
public static class RedisCacheExtensions
{
    public static async Task<T> GetAsync<T>(this IDatabase cache, string key)
    {
        return Deserialize<T>(await cache.StringGetAsync(key));
    }

    public static async Task<object> GetAsync(this IDatabase cache, string key)
    {
        return Deserialize<object>(await cache.StringGetAsync(key));
    }

    public static async Task SetAsync(this IDatabase cache, string key, object value)
    {
        await cache.StringSetAsync(key, Serialize(value));
    }

    static byte[] Serialize(object o)
    {
        byte[] objectDataAsStream = null;

        if (o != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream())
            {
                binaryFormatter.Serialize(memoryStream, o);
                objectDataAsStream = memoryStream.ToArray();
            }
        }

        return objectDataAsStream;
    }

    static T Deserialize<T>(byte[] stream)
    {
        T result = default(T);

        if (stream != null)
        {
            BinaryFormatter binaryFormatter = new BinaryFormatter();
            using (MemoryStream memoryStream = new MemoryStream(stream))
            {
                result = (T)binaryFormatter.Deserialize(memoryStream);
            }
        }

        return result;
    }
}
```

A következő kódrészlet szemlélteti nevű metódus `RetrieveBlogPost` bővítmény módszerekben használó írási és olvasási egy szerializálható `BlogPost` a gyorsítótár követően a gyorsítótár-függetlenül mintázat objektum:

```csharp
// The BlogPost type
[Serializable]
private class BlogPost
{
    private HashSet<string> tags = new HashSet<string>();

    public BlogPost(int id, string title, int score, IEnumerable<string> tags)
    {
        this.Id = id;
        this.Title = title;
        this.Score = score;
        this.tags = new HashSet<string>(tags);
    }

    public int Id { get; set; }
    public string Title { get; set; }
    public int Score { get; set; }
    public ICollection<string> Tags { get { return this.tags; } }
}
...
private async Task<BlogPost> RetrieveBlogPost(string blogPostKey)
{
    BlogPost blogPost = await cache.GetAsync<BlogPost>(blogPostKey);
    if (blogPost == null)
    {
        blogPost = await GetBlogPostFromDataSourceAsync(blogPostKey);
        await cache.SetAsync(blogPostKey, blogPost);
    }

    return blogPost;
}
```

Vgx.dll támogatja parancs adatcsatorna-használat Ha ügyfélalkalmazás több aszinkron kérést küld. Vgx.dll is át a multiplex jeleket a kérések azonos kapcsolat helyett érkeznek és válaszol a parancsok szigorú sorrendben.

Ezt a megközelítést csökkentheti a késés azáltal, hogy a hatékonyabb használatát a hálózat segítségével. A következő kódrészletet az látható, amely a két vevők adatait egyidejűleg. A kód elküld két kérelmeket, és végrehajtja a néhány más feldolgozás (nem jelenik meg) előtt vár az eredmények. A `Wait` módszer a gyorsítótár-objektum hasonlít a .NET-keretrendszer `Task.Wait` metódus:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
var task1 = cache.StringGetAsync("customer:1");
var task2 = cache.StringGetAsync("customer:2");
...
var customer1 = cache.Wait(task1);
var customer2 = cache.Wait(task2);
```

A Microsoft webhelyének lapjain [Azure vgx.dll gyorsítótár dokumentáció](https://azure.microsoft.com/documentation/services/cache/) ügyfélalkalmazásokban, amelyekkel az Azure vgx.dll gyorsítótár szövegalkotás további információt tartalmaz. További információt az [Egyszerű használat lapon](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) a StackExchange.Redis webhelyen érhető el.

Az azonos webhely lapjának [folyamatok és multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) aszinkron műveletek és az adatcsatorna-használat vgx.dll és a StackExchange tár további információt tartalmaz.  Ebben a cikkben a gyorsítótár vgx.dll, a következő szakaszban példákat mutat be néhány a speciális módszerek van vgx.dll a gyorsítótárban tárolt adatokat is alkalmazható.

## <a name="using-redis-caching"></a>Vgx.dll gyorsítótárazás használatával

A legegyszerűbb vgx.dll aggályokat gyorsítótárazás használata kulcs – érték párokká ahol az érték, amely tartalmazhat bináris adatokat tetszőleges hosszúságú karakterláncra nem értelmezett. (Lényegében egy tömb karakterláncként kezelhető bájtok számát szó). Ebben az esetben ez a cikk korábbi részében szakasz megvalósítása vgx.dll gyorsítótár ügyfélalkalmazásokban lett szemlélteti.

Figyelje meg, hogy billentyűkkel is nem értelmezett adatokat tartalmaznak, ezért a kulcs bináris információkat is használhatja. A hosszabb a kulcs, azonban a nagyobb hely időt vesz igénybe tárolni, és a hosszabb időt vesz igénybe keresési hajtanak végre. Használhatósági és karbantartás Kezeléstechnikai a keyspace gondosan tervezése és értelmes (de nem részletes) billentyűk használata.

Például használja a strukturált kulcsok, például "ügyfél: 100" a billentyűt a vevő az azonosító 100, hanem egyszerűen csak a "100". Ez a program lehetővé teszi, hogy könnyen megkülönböztetni tároló különböző adattípusú értékeket. Ha például is használhatja a kulcs "rendelések: 100" ábrázolására a rendelés azonosítója 100 az a billentyűt.

Egydimenziós bináris karakterláncok, kivéve vgx.dll kulcs-érték két érték tartsa is további adatait, beleértve a listák, akkor állítja be (rendezett és rendezetlen), és kivonatolja strukturált. Vgx.dll biztosít átfogó parancs, amely ilyen is kezelheti, és ezek a parancsok közül számos csak a .NET-keretrendszer alkalmazások keresztül egy ügyfél-tárba, például StackExchange számára elérhető. A webhely vgx.dll lapjának [adattípusok és vízkivételeket vgx.dll bemutatása](http://redis.io/topics/data-types-intro) áttekintést kaphat részletesebb e típusok és a műveleteket hajthat végre rajtuk használható parancsokat.

Ebben a részben néhány gyakori használata esetben az alábbi adattípusok és parancsok foglalja össze.

### <a name="perform-atomic-and-batch-operations"></a>A számtani atomi és köteg műveletek

A megadott értékeket atomi get-set műveletsort vgx.dll támogatja. Ezeket a műveleteket, távolítsa el a lehetséges fajta veszélyek külön ismertetjük `GET` és `SET` parancsokat. Az elérhető műveletek a következők:

- `INCR`, `INCRBY`, `DECR`, és `DECRBY`, amely egész numerikus adatokat értékek atomi növelés és csökkentése műveletek hajthatók végre. A StackExchange tár Ez a túlterhelt a `IDatabase.StringIncrementAsync` és `IDatabase.StringDecrementAsync` , hajtsa végre ezeket a műveleteket, és visszatér az eredményül kapott értékkel, a gyorsítótárban tárolt módszereket. A következő kódrészletet bemutatja, hogyan használja az alábbi módszerek egyikét:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  await cache.StringSetAsync("data:counter", 99);
  ...
  long oldValue = await cache.StringIncrementAsync("data:counter");
  // Increment by 1 (the default)
  // oldValue should be 100

  long newValue = await cache.StringDecrementAsync("data:counter", 50);
  // Decrement by 50
  // newValue should be 50
  ```

- `GETSET`, amely beolvassa az értéket, amely egy kulcs van társítva, és értékre változtatja meg egy új. A StackExchange tár ezt a műveletet keresztül elérhetővé teszi a `IDatabase.StringGetSetAsync` módot. A kódtöredéket az alábbi példa ezt a módszert. Ez a kód, amely a "adatok: a számláló" az előző példában a kulcs van társítva az aktuális értékét adja eredményül. Kattintson rá visszaállítja a kulcs értékét nulla, az összes azonos művelet részeként:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  string oldValue = await cache.StringGetSetAsync("data:counter", 0);
  ```

- `MGET`és `MSET`, amelynek vissza vagy módosítása az értékhalmaz karakterlánc egyetlen műveletként. A `IDatabase.StringGetAsync` és `IDatabase.StringSetAsync` módszerek túlterhelt támogatja ezt a funkciót, az alábbi példában látható módon:

  ```csharp
  ConnectionMultiplexer redisHostConnection = ...;
  IDatabase cache = redisHostConnection.GetDatabase();
  ...
  // Create a list of key-value pairs
  var keysAndValues =
      new List<KeyValuePair<RedisKey, RedisValue>>()
      {
          new KeyValuePair<RedisKey, RedisValue>("data:key1", "value1"),
          new KeyValuePair<RedisKey, RedisValue>("data:key99", "value2"),
          new KeyValuePair<RedisKey, RedisValue>("data:key322", "value3")
      };

  // Store the list of key-value pairs in the cache
  cache.StringSet(keysAndValues.ToArray());
  ...
  // Find all values that match a list of keys
  RedisKey[] keys = { "data:key1", "data:key99", "data:key322"};
  RedisValue[] values = null;
  values = cache.StringGet(keys);
  // values should contain { "value1", "value2", "value3" }
  ```

Az egyetlen vgx.dll tranzakción több műveletet is szakaszban leírt módon a vgx.dll tranzakciók és -lapokat a cikk korábbi részében is összevonhatja. A StackExchange tár tranzakciók keresztül támogatja a `ITransaction` felület.

Létrehozhat egy `ITransaction` objektum használatával a `IDatabase.CreateTransaction` módszert. A tranzakció parancsok meghívásához által biztosított módszerekkel a `ITransaction` objektumot.

A `ITransaction` felület módszerek csoportja, amely hasonlít a elérhető hozzáférést biztosít a `IDatabase` felület, azzal a különbséggel, hogy minden módszer aszinkron. Ez azt jelenti, hogy csak azok végrehajtani, amikor a `ITransaction.Execute` módszer hivatkoznak. A által visszaadott érték a `ITransaction.Execute` módszer jelzi, hogy a tranzakció sikeres volt-e létre (igaz), vagy ha (hamis) sikertelen volt.

A következő kódrészletet azt szemlélteti, hogy lépésekben és csökkenti két számláló ugyanazon tranzakció részeként:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
ITransaction transaction = cache.CreateTransaction();
var tx1 = transaction.StringIncrementAsync("data:counter1");
var tx2 = transaction.StringDecrementAsync("data:counter2");
bool result = transaction.Execute();
Console.WriteLine("Transaction {0}", result ? "succeeded" : "failed");
Console.WriteLine("Result of increment: {0}", tx1.Result);
Console.WriteLine("Result of decrement: {0}", tx2.Result);
```

Ne feledje, hogy a tranzakciók vgx.dll eltérően a relációs adatbázisok tranzakciók. A `Execute` módszer egyszerűen várakozási sorba kell futtatni a tranzakció alkotó minden parancs, és bármelyiküket hibás esetén kattintson a tranzakciók le van állítva. Ha a minden parancs van már sikerült aszinkron, minden parancs aszinkron fog futni.

Ha nem sikerül a parancsok, a többi feldolgozás továbbra is. Ellenőrizze, hogy parancsot sikerült van szüksége, ha nem a parancs az eredmények tulajdonságával az **eredmény** a megfelelő feladat kell lehívni a fenti példában látható módon. Az **eredmény** tulajdonság olvasásakor blokkolja a hívó szál a tevékenység befejeztéig.

További tudnivalókért lásd: a [tranzakciók vgx.dll](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) lapon a StackExchange.Redis webhelyen.

Köteg műveletek végrehajtásakor is használhatja a `IBatch` StackExchange tár felület. A kapcsolat egy sor olyan módszereket hasonlóak elérhető hozzáférést biztosít a `IDatabase` felület, azzal a különbséggel, hogy minden módszer aszinkron.

Hoz létre egy `IBatch` objektum használatával a `IDatabase.CreateBatch` módot, és majd futtassa a köteg használatával a `IBatch.Execute` módszer, az alábbi példában látható módon. Ez a kód egyszerűen szöveges értékként, lépésekben és csökkenti az előző példában az azonos számláló állít be, és megjeleníti az eredményt:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
IBatch batch = cache.CreateBatch();
batch.StringSetAsync("data:key1", 11);
var t1 = batch.StringIncrementAsync("data:counter1");
var t2 = batch.StringDecrementAsync("data:counter2");
batch.Execute();
Console.WriteLine("{0}", t1.Result);
Console.WriteLine("{0}", t2.Result);
```

Fontos, ha meg szeretné érteni, hogy nem kedvelik-e egy tranzakció, ha egy Köteg parancs nem sikerül egy hibás, mert a további parancsok továbbra is működni. A `IBatch.Execute` metódus nem ad vissza a megoldás sikeres vagy sikertelen jelét.

### <a name="perform-fire-and-forget-cache-operations"></a>Hajtsa végre a fire, majd elfelejti gyorsítótár-műveletek

Támogatja fire vgx.dll és műveletek elfelejti jelölők parancs használatával. Ebben az esetben az ügyfél egyszerűen elindítja a műveletet, de nem áll, az eredmény érdekében, és nem megvárja, amíg a hátralévő parancsot. Az alábbi példában megtudhatja, hogy miként végezze el a INCR parancs, mint egy fire, és elfelejtette műveletet:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
await cache.StringSetAsync("data:key1", 99);
...
cache.StringIncrement("data:key1", flags: CommandFlags.FireAndForget);
```

### <a name="specify-automatically-expiring-keys"></a>Adja meg a automatikusan elévülési kulcsok

Egy elem vgx.dll gyorsítótárban tárolt, is megadhat egy időkorlát, amely után az elem automatikusan törlődnek a gyorsítótárból. Is is lekérdezhetők mennyire több időt kulcs van még mielőtt lejárna használatával a `TTL` parancsot. Ez a parancs érhető el StackExchange alkalmazások használatával az `IDatabase.KeyTimeToLive` módot.

A következő kódrészletet 20 másodperces elévülési időt meg egy kulcsot, és a hátralévő élettartam a kulcs lekérdezés szemlélteti:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration time of 20 seconds
await cache.StringSetAsync("data:key1", 99, TimeSpan.FromSeconds(20));
...
// Query how much time a key has left to live
// If the key has already expired, the KeyTimeToLive function returns a null
TimeSpan? expiry = cache.KeyTimeToLive("data:key1");
```

Is beállíthatja, hogy a lejárati idő egy adott dátum és idő parancsával elévülési, amely érhető el a StackExchange tárban, mint a `KeyExpireAsync` módszer:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Add a key with an expiration date of midnight on 1st January 2015
await cache.StringSetAsync("data:key1", 99);
await cache.KeyExpireAsync("data:key1",
    new DateTime(2015, 1, 1, 0, 0, 0, DateTimeKind.Utc));
...
```

> _Tipp:_ Manuális eltávolíthatja egy elemet a gyorsítótárból az DEL, mint a StackExchange tár keresztül érhető el parancs használatával a `IDatabase.KeyDeleteAsync` módot.

### <a name="use-tags-to-cross-correlate-cached-items"></a>A gyorsítótárban lévő elemek keresztté-correlate címkék használata

Egy vgx.dll be kapcsolva, hogy egy kulcs megosztása több elem gyűjteménye. Létrehozhat egy sor a SADD parancs használatával. Az elemek egy halmaz a SMEMBERS parancs használatával meghallgathatja. A StackExchange tár végrehajtja a SADD parancsot a `IDatabase.SetAddAsync` módját, valamint a SMEMBERS parancs a `IDatabase.SetMembersAsync` módot.

Új készletek létrehozása a SDIFF (beállítása különbség), a SZINTER (beállítása metszet) és a SUNION (beállítása Unió) parancsok használatával meglévő készletek kombinálhatja is. A StackExchange tár ezeket a műveleteket a egyesíti a `IDatabase.SetCombineAsync` módot. Ezt a módszert első paraméterrel a set-végrehajtandó műveletet.

A következő kódtöredék megjelenítése hogyan beállítása gyorsan tárolásához és kapcsolódó elemgyűjteményeket beolvasása hasznos lehet. Ez a kód használja a `BlogPost` megvalósítása vgx.dll gyorsítótár ügyfélalkalmazásokban Ez a cikk a korábbi szakaszban ismertetett típusa.

A `BlogPost` objektum négy mezőt tartalmazza – Azonosítóját, a címet, a rangsorolási pontszám és a címkék gyűjteménye. Az első kódtöredéket az alábbi megjeleníti a mintaadatokat, amellyel C# listájának feltöltése `BlogPost` objektumok:

```csharp
List<string[]> tags = new List<string[]>()
{
    new string[] { "iot","csharp" },
    new string[] { "iot","azure","csharp" },
    new string[] { "csharp","git","big data" },
    new string[] { "iot","git","database" },
    new string[] { "database","git" },
    new string[] { "csharp","database" },
    new string[] { "iot" },
    new string[] { "iot","database","git" },
    new string[] { "azure","database","big data","git","csharp" },
    new string[] { "azure" }
};

List<BlogPost> posts = new List<BlogPost>();
int blogKey = 0;
int blogPostId = 0;
int numberOfPosts = 20;
Random random = new Random();
for (int i = 0; i < numberOfPosts; i++)
{
    blogPostId = blogKey++;
    posts.Add(new BlogPost(
        blogPostId,               // Blog post ID
        string.Format(CultureInfo.InvariantCulture, "Blog Post #{0}",
            blogPostId),          // Blog post title
        random.Next(100, 10000),  // Ranking score
        tags[i % tags.Count]));   // Tags--assigned from a collection
                                  // in the tags list
}
```

A címkék tárolhatja a az egyes `BlogPost` objektum egy beállított vgx.dll gyorsítótárban, és minden egyes készletben társítása azonosítója a `BlogPost`. Ez lehetővé teszi, hogy egy alkalmazást, ha meg szeretné találni a címkék, amelyek egy adott blogbejegyzés tartoznak. Engedélyezheti a keresés irányának Ellentétesre változtatásához, és keresse meg az összes blogbejegyzések, amely egy adott címke megosztása, egy másik beállítása a blogbejegyzéseket, a címke azonosítója, a kulcs hivatkozó betöltő hozhat létre:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
// Tags are easily represented as Redis Sets
foreach (BlogPost post in posts)
{
    string redisKey = string.Format(CultureInfo.InvariantCulture,
        "blog:posts:{0}:tags", post.Id);
    // Add tags to the blog post in Redis
    await cache.SetAddAsync(
        redisKey, post.Tags.Select(s => (RedisValue)s).ToArray());

    // Now do the inverse so we can figure how which blog posts have a given tag
    foreach (var tag in post.Tags)
    {
        await cache.SetAddAsync(string.Format(CultureInfo.InvariantCulture,
            "tag:{0}:blog:posts", tag), post.Id);
    }
}
```

Szerkezetek engedélyezése számos, gyakori lekérdezések nagyon hatékonyan végezheti el. Például keresése és az összes blogbejegyzés 1 jelennek meg a címkék megjelenítése:

```csharp
// Show the tags for blog post #1
foreach (var value in await cache.SetMembersAsync("blog:posts:1:tags"))
{
    Console.WriteLine(value);
}
```

Talál, amelyek megegyeznek a blog összes címkék 1 és blog bejegyzés 2 közzé végrehajtásával beállítása metszet, az alábbi képlettel történik:

```csharp
// Show the tags in common for blog posts #1 and #2
foreach (var value in await cache.SetCombineAsync(SetOperation.Intersect, new RedisKey[]
    { "blog:posts:1:tags", "blog:posts:2:tags" }))
{
    Console.WriteLine(value);
}
```

És az adott címkét tartalmazó összes blogbejegyzések talál:

```csharp
// Show the ids of the blog posts that have the tag "iot".
foreach (var value in await cache.SetMembersAsync("tag:iot:blog:posts"))
{
    Console.WriteLine(value);
}
```

### <a name="find-recently-accessed-items"></a>Keresés a legutóbb használt elemek

A leggyakrabban a legutóbb használt elemek kereséséhez, akkor egy közös feladatot, számos más alkalmazáshoz szükséges. Blogkezelő webhely szükség lehet például a legutóbb olvasási blogbejegyzések kapcsolatos információk megjelenítéséhez.

Ez a funkció alkalmazhat vgx.dll lista használatával. Vgx.dll listáját tartalmazza, hogy ugyanazt a kulcsot megosztása több elem. A lista működik-e egy kettős várólista. Elem vagy a lista végére a LPUSH (a bal oldali leküldéses) és a RPUSH (jobb leküldéses) parancsokkal leküldéses. Elemek a LPOP és RPOP parancs használatával meghallgathatja a vagy a lista végére. Egy sor olyan elemeket is parancsaival LRANGE és ablaktáblák térhet vissza.

Az alábbi kódtöredék bemutatják, hogyan végezheti el ezeket a műveleteket a StackExchange tár használatával. Ez a kód használja a `BlogPost` az előző példákban megismert típusát. Blogbejegyzésbe van olvasni a felhasználó által a `IDatabase.ListLeftPushAsync` módszer verembe küldi a blogbejegyzés azt a listát, a "blog:recent_posts" vgx.dll gyorsítótár kulcs van társítva az alakzatot a cím.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:recent_posts";
BlogPost blogPost = ...; // Reference to the blog post that has just been read
await cache.ListLeftPushAsync(
    redisKey, blogPost.Title); // Push the blog post onto the list
```

További blogbejegyzések olvasásához, ugyanabban a listában az alakzatot a címben elküldte azokat. A listában a sorrendben, amelyben a címek hozzáadott szerint van rendezve. A legutóbb olvasási blogbejegyzések vannak felé a lista bal szélén. (Ugyanazt a blogbejegyzés többször olvasható, akkor van több bejegyzést a listában.)

A legutóbb olvasási bejegyzések címének használatával is megjelenítése a `IDatabase.ListRange` módot. Ez a módszer megnyitja a kulcsot, amely tartalmazza a listában, a kiindulási pont és a végpontot. A következő kódot beolvassa a 10 blogbejegyzések (elemek 0-tól 9-ig) bal szélső végén található a listában a módosult:

```csharp
// Show latest ten posts
foreach (string postTitle in await cache.ListRangeAsync(redisKey, 0, 9))
{
    Console.WriteLine(postTitle);
}
```

Megjegyzendő, hogy a `ListRangeAsync` mód nem elemek eltávolítása a listából. Ehhez használja a `IDatabase.ListLeftPopAsync` és `IDatabase.ListRightPopAsync` módszereket.

Meg, hogy a lista növekvő határozatlan ideig, időnként a lista körülvágja is selejtezett elemek. Az alábbi kódrészletet megtudhatja, hogy miként eltávolítja a teljes, de az öt bal szélső elemet a listából:

```csharp
await cache.ListTrimAsync(redisKey, 0, 5);
```

### <a name="implement-a-leader-board"></a>Kitöltés fal megvalósítása

Alapértelmezés szerint az elemek egy halmaz nem tartják meghatározott sorrendben követik egymást. A ZADD parancs használatával hozhat létre egy rendezett sorozata (a `IDatabase.SortedSetAdd` a StackExchange tárban módszer). Az elemek egy numerikus érték, a parancs paraméterként megadva pontszám nevű használatával vannak rendezve.

A következő kódrészletet ad olyan rendezett listát a blogbejegyzés címét. Ebben a példában mindegyik blogbejegyzés tartalmaz egy pontszám mezőt, amely tartalmazza a blogbejegyzés besorolását.

```csharp
ConnectionMultiplexer redisHostConnection = ...;
IDatabase cache = redisHostConnection.GetDatabase();
...
string redisKey = "blog:post_rankings";
BlogPost blogPost = ...; // Reference to a blog post that has just been rated
await cache.SortedSetAddAsync(redisKey, blogPost.Title, blogpost.Score);
```

A blog bejegyzés címek és értékek növekvő sorrendben pontszámhoz használatával meghallgathatja a `IDatabase.SortedSetRangeByRankWithScores` módszer:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(redisKey))
{
    Console.WriteLine(post);
}
```

> [AZURE.NOTE] A StackExchange tár is biztosít a `IDatabase.SortedSetRangeByRankAsync` módszer, amely pontszámhoz sorrendben adja vissza az adatokat, de nem ad vissza az eredmények.

Is elemek csökkenő sorrendben eredmények beolvasásához, és további paramétereket megadásával a visszaadott elemek számának korlátozása a `IDatabase.SortedSetRangeByRankWithScoresAsync` módot. A következő példa a címek és a felső 10 rangsorolt blogbejegyzések az értékek megjelenítése:

```csharp
foreach (var post in await cache.SortedSetRangeByRankWithScoresAsync(
                               redisKey, 0, 9, Order.Descending))
{
    Console.WriteLine(post);
}
```

A következő példában a `IDatabase.SortedSetRangeByScoreWithScoresAsync` módszerrel is használhatja az elemeket, amelyeket vissza azokat, amelyek egy adott pontszámhoz esik korlátozni tartomány:

```csharp
// Blog posts with scores between 5000 and 100000
foreach (var post in await cache.SortedSetRangeByScoreWithScoresAsync(
                               redisKey, 5000, 100000))
{
    Console.WriteLine(post);
}
```

### <a name="message-by-using-channels"></a>Üzenet csatornák használatával

Kívül működő adatainak gyorsítótárban, a vgx.dll kiszolgáló üzenetküldést a nagy teljesítményű publisher/előfizetői mechanizmusa keresztül biztosít. Csatorna ügyfélalkalmazásokban feliratkozhat, és más alkalmazások és szolgáltatások üzenetek közzéteheti a csatornához. Alkalmazások előfizetés ekkor megjelenik az alábbi üzenetek és lehet feldolgozni azokat.

Az ELŐFIZETÉS parancs ügyfélalkalmazásokban fizessen elő a csatornák használatával vgx.dll biztosít. Ez a parancs neve egy vagy több csatornák, amelyen az alkalmazás fogad üzenetek vár. A StackExchange tár magában foglalja a `ISubscription` felülettel, amely lehetővé teszi a .NET-keretrendszer-alkalmazások előfizetés, és közzéteheti a csatornák gombot.

Létrehozhat egy `ISubscription` objektum használatával a `GetSubscriber` a vgx.dll kiszolgálóhoz való csatlakozás metódusát. Használatával a csatorna üzenetek meghallgatása, majd a `SubscribeAsync` az objektum metódusát. Az alábbi példa szemlélteti "üzenetek: blogPosts" nevű csatorna fizessen elő:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
await subscriber.SubscribeAsync("messages:blogPosts", (channel, message) =>
{
    Console.WriteLine("Title is: {0}", message);
});
```

Az első paraméterként a `Subscribe` módja a csatorna nevét. Ez a név gyorsítótár billentyűk által használt azonos szabályokat követi. A név tartalmazhat bináris adatokat, bár tanácsos segíthetnek abban, hogy a jó teljesítményt és karbantartási követelmények viszonylag rövid, jelentéssel bíró karakterláncok használatával.

Megjegyzés: is, hogy a csatornák által használt névtér elkülönülnek billentyűk által használt. Ez azt jelenti, hogy lehet csatornák és a billentyűket, amelyek lehet ugyanaz a neve, bár ez nehezebben megőrzéséhez teheti az alkalmazás kódját.

A második paraméter egy művelet meghatalmazott. Ez a delegált aszinkron fut, amikor egy új üzenet jelenik meg a csatornát. Ebben a példában a (az üzenet tartalmazza a blogbejegyzés címét) konzolon egyszerűen az üzenetet jelenít meg.

Csatorna közzététele, az alkalmazás a paranccsal vgx.dll közzététele. A StackExchange tár biztosít a `IServer.PublishAsync` módszer művelet végrehajtásához. A következő kódrészletet szemlélteti, hogyan lehet az "üzenet: blogPosts" csatorna üzenet közzététele:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
...
BlogPost blogpost = ...;
subscriber.PublishAsync("messages:blogPosts", blogPost.Title);
```

Több pontok ismernie kell a közzététel/előfizetés mechanizmusa kapcsolatban:

- Több előfizetők feliratkozhat ugyanazt a csatornát, és az összes kapnak az adott csatornához közzétett üzenetek.
- Előfizetők csak azok előfizetett közzétett üzenetek fogadni. Csatornák nem puffereltek, és van közzétéve, egy üzenet, a vgx.dll infrastruktúra verembe, az üzenetet küldi minden előfizető, és eltávolítja azt.
- Alapértelmezés szerint előfizetők abban a sorrendben, amelyben küldött érkező üzeneteket. Az üzenetek és számos előfizetők és közzétevők nagyszámú erősen aktív rendszerben garantált szekvenciális üzenet kézbesítésének is csökkentheti a rendszer teljesítményét. Ha minden üzenet független és lényegtelen a sorrendben, a vgx.dll rendszer, amelyek segíthetnek válaszidő javítható párhuzamos feldolgozása engedélyezheti. Érhet el ezt a StackExchange ügyfél állítsa a kapcsolat az előfizetői hamis által használt a PreserveAsyncOrder:

```csharp
ConnectionMultiplexer redisHostConnection = ...;
redisHostConnection.PreserveAsyncOrder = false;
ISubscriber subscriber = redisHostConnection.GetSubscriber();
```

## <a name="related-patterns-and-guidance"></a>Kapcsolódó minták és útmutató

A következő mintát is fontos a példa a Ha az alkalmazásokban gyorsítótárazás:

- [Gyorsítótár-függetlenül mintázat](http://msdn.microsoft.com/library/dn589799.aspx): ezt a minta leírja, hogy miként igény adatok betöltése egy adatok áruházából gyorsítótárat. A minta szintén gondoskodhat tartott a gyorsítótárban lévő adatok és az adatok az eredeti adatok áruházban közötti összhangot megőrzéséhez.
- A [minta Sharding](http://msdn.microsoft.com/library/dn589797.aspx) végrehajtására, amikor tárolásához és nagy mennyiségű adattal eléréséhez méretezhetőség javítása érdekében vízszintes szétválasztás információt tartalmaz.

## <a name="more-information"></a>További információ

- A Microsoft webhelyének [MemoryCache osztály](http://msdn.microsoft.com/library/system.runtime.caching.memorycache.aspx) lapján
- A Microsoft webhelyének [Azure vgx.dll gyorsítótár dokumentáció](https://azure.microsoft.com/documentation/services/cache/) lapján
- A Microsoft webhelyének a [Azure vgx.dll gyorsítótár – gyakori kérdések](redis-cache/cache-faq.md) lapjára
- A Microsoft webhelyének [konfigurációs modell](http://msdn.microsoft.com/library/windowsazure/hh914149.aspx) lapján
- A Microsoft webhelyének [Aszinkron mintát feladatalapú](http://msdn.microsoft.com/library/hh873175.aspx) lapján
- A StackExchange.Redis GitHub repó [folyamatok és multiplexers](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md) lapján
- Az [adatmegőrzési vgx.dll](http://redis.io/topics/persistence) vgx.dll webhely lapjának
- A webhely vgx.dll [replikációs lap](http://redis.io/topics/replication)
- A webhely vgx.dll [fürt oktatóprogram vgx.dll](http://redis.io/topics/cluster-tutorial) lapjának
- A [particionálására: hogyan felosztása több vgx.dll példányok között adatok](http://redis.io/topics/partitioning) vgx.dll webhely lapjának
- A webhely vgx.dll [Használatával vgx.dll, mint egy LRU gyorsítótár](http://redis.io/topics/lru-cache) lapjának
- A [tranzakciók](http://redis.io/topics/transactions) lapon a vgx.dll webhelyen
- A [biztonsági vgx.dll](http://redis.io/topics/security) lapon a vgx.dll webhelyen
- A [Lap körül Azure vgx.dll gyorsítótár](https://azure.microsoft.com/blog/2014/06/04/lap-around-azure-redis-cache-preview/) lapon a Azure-blog
- A Microsoft webhelyének [A egy CentOS Linux virtuális Azure-ban futó vgx.dll](http://blogs.msdn.com/b/tconte/archive/2012/06/08/running-redis-on-a-centos-linux-vm-in-windows-azure.aspx) lapján
- A Microsoft webhelyének [ASP.NET munkamenet állapot-szolgáltató Azure vgx.dll gyorsítótár](redis-cache/cache-aspnet-session-state-provider.md) lapján
- A Microsoft webhelyének [ASP.NET kimeneti gyorsítótár-szolgáltató Azure vgx.dll gyorsítótár](redis-cache/cache-aspnet-output-cache-provider.md) lapján
- Az [Adattípusok és vízkivételeket vgx.dll egy bemutatása](http://redis.io/topics/data-types-intro) vgx.dll webhely lapjának
- Az [Egyszerű használat](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Basics.md) StackExchange.Redis webhely lapjának
- A StackExchange.Redis repó [tranzakciók vgx.dll](https://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Transactions.md) lapján
- Az [adatok particionáló útmutató](http://msdn.microsoft.com/library/dn589795.aspx) a Microsoft webhelyén
