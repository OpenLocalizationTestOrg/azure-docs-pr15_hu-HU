<properties
    pageTitle="Adatforgalom Manager - forgalmat útválasztási módszerek |} Microsoft Azure"
    description="Ez a cikk segít megérteni a forgalom kezelő által használt eltérő forgalmat útválasztási módszerek"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Adatforgalom Manager forgalom útválasztás módszerek

Azure forgalom Manager három forgalom útválasztás módszer annak a hálózati forgalmat a különböző szolgáltatás végpontok támogatja. Forgalom Manager minden DNS-lekérdezés kap a forgalom útválasztás módszer vonatkozik. A forgalom útválasztás módszer határozza meg, hogy melyik végpont visszaadva a DNS-válasz.

Az Azure erőforrás-kezelő támogatási forgalom Manager eltérő kifejezéseket, mint a klasszikus telepítési modellt használja. A következő táblázat mutatja az erőforrás-kezelő és klasszikus kifejezéseket közötti különbségeket:

| Erőforrás-kezelő kifejezés | Klasszikus kifejezés |
|-----------------------|--------------|
| Adatforgalom útválasztás módszer | Terheléselosztási módszer |
| Prioritás módszer | Feladatátvevő módszer |
| Súlyozott módszer | Ciklikus módszer |
| Teljesítmény módszer | Teljesítmény módszer |

Ügyfeleink visszajelzései alapján számúra módosítása a referenciacikk olyan kifejezéseket áttekinthetővé javítása és a gyakori félreértést csökkentése. Nem funkció nincs különbség.

Háromféleképpen forgalom útválasztási érhető el az adatforgalom-kezelőben:

- **Prioritás:** Jelölje ki a "Prioritása", ha meg szeretné használni egy elsődleges szolgáltatás végpontjának minden forgalom, és adja meg a biztonsági másolatok, abban az esetben, ha az elsődleges vagy a biztonsági másolat végpontokat nem érhetők el.
- **Súlyozott:** Select "súlyozott" Ha forgalom elosztása végpontok halmazának szeretne, vagy egyenletesen vagy súlyok, amely szerint adja meg.
- **Teljesítmény:** Jelölje be a "Teljesítmény", amikor végpontjait a különböző földrajzi helyek és azt szeretné, hogy a végfelhasználók számára, hogy a "legközelebbi" végpont értelmez a legmagasabb hálózati késés használatára.

Minden forgalom Manager profilok kísérni végpont állapot és az automatikus végpont feladatátvevő. További tudnivalókért lásd: a [Forgalom Manager végpont figyelés](traffic-manager-monitoring.md). Egyetlen forgalom Manager profil csak egy forgalom útválasztási módszer használható. Választhat egy másik forgalom útválasztási módszer a profil bármikor. Változások történnek egy percen belül, és nincs legrövidebb leállás merül fel. Adatforgalom útválasztás módszerek kombinálhatók beágyazott forgalom Manager profilok használatával. A beágyazási lehetővé teszi, hogy az összetett és rugalmas forgalom útválasztás konfigurációk, amely nagyobb, összetett alkalmazások igényeinek. További információ című témakör tartalmaz [beágyazott forgalom Manager](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Prioritás forgalom útválasztás módszer

Egy szervezet gyakran szeretne egy vagy több biztonsági szolgáltatások telepítésével, abban az esetben, ha az elsődleges szolgáltatás megszakad a szolgáltatásai megbízhatóság szükséges. A "Priority" forgalom útválasztás módszerrel Azure ügyfelek Ez feladatátvevő minta egyszerűen végrehajtásához.

![Azure forgalom Manager "Prioritása" forgalom útválasztás módszer][1]

A forgalom Manager profil szolgáltatás végpontok elsőbbségi listáját tartalmazza. Alapértelmezés szerint az adatforgalom-kezelő minden forgalom küld az elsődleges (legnagyobb prioritású) végpontot. Az elsődleges végpontot nem érhető el, ha azt a forgalom Manager forgalom az második végpontot. Ha az elsődleges és másodlagos végpontokat nem érhetők el, a forgalom Ugrás a harmadik, és így tovább. Elérhetőség a végpont (engedélyezhető vagy tiltható le) a beállított állapotot, és a folyamatban lévő végpont figyelemmel kísérése alapul.

### <a name="configuring-endpoints"></a>Végpont beállítása

Az Azure erőforrás-kezelő, akkor a kifejezetten az tulajdonságával "prioritása" minden végpont végpont prioritásának beállítása. Ez a tulajdonság értéke 1 és 1000 közötti. Kisebb értékeket magasabb prioritást jelenítik meg. Végpontokhoz nem oszthat meg a prioritás értékei. A tulajdonság beállításával nem kötelező. Ha elhagyják, a végpontok sorrend alapján alapértelmezett kiemelt használják.

A klasszikus felülettel végpont prioritás az implicit módon van beállítva. A prioritás a sorrendben, amelyben a végpontok jelennek meg a profil definíció alapul.

## <a name="weighted-traffic-routing-method"></a>Súlyozott forgalom útválasztás módszer

A "Súlyozott" forgalom útválasztás módszerrel forgalom egyenletes elosztása vagy egy előre definiált súlyozási használni.

![Azure forgalom Manager az "Súlyozott" forgalom útválasztás módszer][2]

Súlyozott forgalom útválasztás módszer, a vonalvastagságot minden végpontra a forgalom Manager profil konfigurációs hozzárendelése. Vonalvastagság értéke 1 és 1000 egész szám. Ez a paraméter megadása nem kötelező. Elhagyása esetén értéke a forgalom menedzserek használ egy alapértelmezett vastagságának "1".

Az egyes DNS-lekérdezés kapott forgalom Manager véletlenszerűen elérhető zárólap választja ki. Annak valószínűsége, zárólap kiválasztása az összes rendelkezésre álló végponton rendelt súlyok alapul. Az összes végpontok eredménye egy páros forgalom terjesztési azonos vonalvastagság használata. Nagyobb vagy kisebb súlyok használata bizonyos végpontok eredményezi, ezeket gyakran több vagy kevesebb vissza kell a DNS-válaszokat a végpontok.

A súlyozott módszer néhány hasznos forgatókönyveket teszi lehetővé:

- Fokozatos alkalmazás frissítése: a forgalom átirányítása az új végpont százalékos lefoglalhat és fokozatosan növelje a forgalmat a 100 %-os időbeli.
- Azure alkalmazás áttelepítés: profil létrehozása az Azure és a külső végpontok. Az új végpontokat inkább a végpontok vastagságának módosítása
- További kapacitás felhő felszakítási: gyorsan bontsa ki a helyszíni példányban a felhőbe történő mögött forgalom Manager profil helyezésével. További kapacitás a felhőben van szüksége, amikor hozzáadása vagy további végpontok engedélyezése, és adja meg, milyen forgalom részét minden végpontra ugrik.

Az új Azure portál súlyozott forgalom útválasztás konfigurációja támogatja. A klasszikus portálon nem állítható be a súly. Az erőforrás-kezelő és Azure PowerShell CLI és a REST API-k klasszikus változatának súlyok is beállítható.

Ha meg szeretné érteni, hogy az ügyfél által használt DNS-névfeloldás rekurzív DNS-kiszolgálók és ügyfelek által gyorsítótárban DNS válaszok fontos. Ez a gyorsítótár is hatással súlyozott forgalom terjesztését. Ha a szám rekurzív DNS-kiszolgálók és ügyfelek túl nagy méretű, forgalom terjesztési a várt módon működik. Azonban rekurzív DNS-kiszolgálók és ügyfelek értéke kisebb, amikor gyorsítótárazás is jelentősen ferdeség a forgalom eloszlás.

Közös használati esetek a következők:

- Fejlesztés és tesztelés környezetben
- Alkalmazások – kommunikáció
- Alkalmazások célja egy keskeny felhasználói alapul, hogy a közös rekurzív DNS-infrastruktúrát (például a vállalat proxyn keresztül csatlakozik az alkalmazottak) megosztása

A DNS-gyorsítótár effektusok megegyeznek az összes DNS-alapú forgalmat útválasztási rendszerek nem csak a Azure forgalom Manager. Egyes esetekben kifejezetten az a DNS-gyorsítótár kiürítése rendelkezhetnek megoldás. Egyéb esetben alternatív forgalom útválasztás módszert lehet megfelelőbb.

## <a name="performance-traffic-routing-method"></a>Teljesítmény forgalom útválasztás módszer

Két vagy több helyen lévő telepíti át a földgömb javíthatja adatforgalom arra a helyre, "a legközelebb esik", sok alkalmazás növeléséhez. A "Teljesítmény" forgalom útválasztás módszer biztosítja ezt a lehetőséget.

![Azure forgalom Manager "Teljesítmény" forgalom útválasztás módszer][3]

A "legközelebbi" végpont nem feltétlenül legközelebbi mért távolságot földrajzi. Ehelyett a "Teljesítmény" forgalom útválasztás módszer hálózati késés mérése a legközelebbi végpont határozza meg. Forgalom Manager tart fenn egy internetes késés táblázat nyomon követni oda-vissza időt IP-címtartományok és az egyes Azure adatközponthoz között.

A forrás IP-címe a bejövő DNS-kérés az internetes késés táblázat forgalom Manager keres. Forgalom Manager elérhető zárólap az Azure adatközpontban, hogy rendelkezik-e IP-címtartományokat a legalacsonyabb időtartam, majd adja meg a DNS-válasz végpontra választja ki.

[Forgalom Manager működése](traffic-manager-how-traffic-manager-works.md)leírtak forgalom Manager nem kapja meg a DNS-lekérdezések közvetlenül az ügyfelek számára. Ehelyett a DNS-lekérdezések származnia a rekurzív DNS-szolgáltatás, hogy az ügyfelek használatára konfigurált. Emiatt az IP-cím azt határozza meg, a legközelebbi"végpont nem az ügyfél IP-cím, de a rekurzív DNS-szolgáltatás IP-címe. A gyakorlatban IP-cím egy jó proxy az ügyfél számára.

Adatforgalom Manager rendszeresen frissíti az internetes késés táblázat a globális Internet és az új Azure régiók változásainak. Azonban alkalmazás teljesítményének függ valós idejű változatok a betöltés az interneten keresztül. Egy adott szolgáltatás végpontjának terheltsége nem figyelje, teljesítmény forgalom-routing. Azonban zárólap nem érhető el, ha forgalom Manager nem létezik DNS válaszokban.

Megjegyzés: a címzett pontok:

- Ha a profil több, ugyanabban a Azure régióban végpontok tartalmaz, majd forgalom Manager egyenletesen forgalom elosztja az adott régióban elérhető végpontjait. Ha azt szeretné, hogy egy másik forgalom terjesztési egy régión belüli, a [beágyazott forgalom Manager profilok](traffic-manager-nested-profiles.md)is használhatja.

- Ha egy adott Azure tartományban lévő összes engedélyezett végponton csökkent vannak, forgalom Manager forgalom elosztja az összes rendelkezésre álló végpontok helyett a legközelebbi végpont. A logikai megakadályozza, hogy a kaszkádolt hiba fordul elő, a legközelebbi végpont nem túlterhelés. Ha szeretné az előnyben részesített feladatátvevő sorának meghatározása, [beágyazott forgalom Manager profilok](traffic-manager-nested-profiles.md)használata

- Amikor a módszerrel a teljesítmény forgalom útválasztási külső végpontok vagy beágyazott végpontok kell adja meg azokat a végpontok helyét. Válassza ki a legközelebb áll a központi telepítés Azure területhez tartozik. Helyekre az értékeket a Internet késés tábla által támogatott.

- A választ ki a végpont algoritmus mérvadó. Az azonos ügyféltől ismételt DNS-lekérdezések irányítja át az azonos végpontot. Általában ügyfelek különböző rekurzív DNS-kiszolgálók utazás közben használt. Az ügyfél továbbítása egy másik végpontra történhet. Továbbítás is hatással lehet frissítések az internetes késés táblázatra. Emiatt a teljesítmény forgalom útválasztás módszerrel nem garantálja, hogy egy ügyfél mindig van-e irányítva az azonos végpontot.

- Az Internet késés táblázat változásakor Észreveheti, hogy egyes ügyfelek irányítja át egy másik végpontot. A útválasztási módosítás pontosabb aktuális késés adatok alapján. A frissítések fontosak megőrzéséhez a teljesítmény forgalom-routing pontosságát, az internetről folyamatosan követi.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként használja a [forgalom Manager végpont figyelése](traffic-manager-monitoring.md) magas rendelkezésre állás alkalmazások fejlesztői számára

Megtudhatja, hogyan hozhat [létre a forgalom Manager profilok](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
