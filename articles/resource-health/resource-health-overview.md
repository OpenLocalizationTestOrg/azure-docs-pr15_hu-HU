<properties
   pageTitle="Azure állapot erőforrás-áttekintés |} Microsoft Azure"
   description="Azure erőforrás állapot áttekintése"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure állapot erőforrás-áttekintés

Azure erőforrás állapot közzététele egyes Azure erőforrások állapotát jelző és az értekezletekre útmutató hibáinak elhárítása szolgáltatás. Helyén nem lehetséges, így közvetlenül elérheti a kiszolgálókon vagy infrastruktúra elemek felhőalapú környezetben az erőforrás egészség célja ügyfelek töltött hibaelhárítás, különösen a annak megállapítása, ha a probléma gyökerének határozza meg, az alkalmazás belül vagy belül az Azure platform esemény okozza töltött idő csökkentése az idő csökkentése érdekében.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Mi számít, hogy egy erőforrás, és hogyan működik az erőforrás állapot úgy dönt, hogy ha az erőforrás megfelelő-e? 
Erőforrás-e a felhasználók által létrehozott példány, szolgáltatás, például nyújtotta erőforrástípus: virtuális gépen, webalkalmazást vagy SQL-adatbázishoz. 

Erőforrás-állapot annak megállapításához, hogy egy erőforrás megfelelő-e az erőforrás-és/vagy a szolgáltatás által kibocsátott jelek támaszkodik. Fontos, hogy látható, hogy jelenleg erőforrás állapot fiókok csak az adott erőforrás egy állapotának írja be a más elemek, amelyek a teljes állapota hozzájárulhatnak nem veszi figyelembe. Például virtuális géphez, a számítási részének a infrastruktúra állapotának kitalálta, azaz a hálózati problémákkal nem jelenik meg az erőforrás állapot, kivéve, ha van egy feltüntetett szolgáltatás üzemszünetek jelentésekor ebben az esetben, akkor fog jelenik meg a lap tetején a szalagcímen keresztül. A jelen cikk további információt a szolgáltatás üzemszünetek lehetősége. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Miben különbözik az erőforrás-állapota az állapotjelző irányítópultja?

Erőforrás-állapot által megadott adatokkal finomabb, mint az állapotjelző irányítópultja által biztosított mi. SHD lehetnek még az olyan régióban szolgáltatás elérhető események kommunikál, miközben a erőforrás állapot közzététele egy adott erőforrás vonatkozó információkat, pl. nézetéhez eseményeket, amely hatással lehet a virtuális géphez, a megfelelő web App alkalmazásban és az SQL-adatbázis elérhetőségét. Például ha váratlanul újraindul csomópontot, amelynek virtuális gépeken futó a csomóponton futó ügyfelek lesz kapnak az az oka, miért időbeli volt a virtuális nem érhető el.   

## <a name="how-to-access-resource-health"></a>Hogyan érhető el az erőforrás állapota
Az erőforrás állapot elérhető szolgáltatások módon 2 erőforrás állapot eléréséhez.

### <a name="azure-portal"></a>Azure portál
Az erőforrás rendszerállapot lap az Azure-portálon az erőforrás a vonatkozó részletes információt tartalmaz, valamint javasolt műveleteket, amelyeket az aktuális állapota, az erőforrás függően változnak. Ez a lap nyújt a legjobb élmény lekérdezése az erőforrás állapot, amikor megkönnyíti a más erőforrások: a portálon belül a hozzáférést. Az említett előtt, az erőforrás rendszerállapot lap javasolt műveletek megadása az aktuális állapota függ:

* Megfelelő erőforrások: mivel nincs hatással lehetnek a erőforrás állapotának problémát talált a műveletek összpontosítanak ezzel megkönnyítve a hibaelhárítási folyamathoz. Ha például azt a hibaelhárítás lap, amely hogyan oldhatja meg a leggyakoribb problémákat ügyfelek arc útmutatást közvetlen hozzáférést biztosít.
* Sérült erőforrás: Azure okozta problémák, a lap jeleníti meg a Microsoft tart (vagy tett) műveletek az erőforrás visszaállításához. A problémákat okoz a felhasználó által kezdeményezett műveletek, a műveletek ügyfelei listája is igénybe vehet a lap lesz így megoldhatja a problémát, helyreállítása az erőforrás.  

Miután, jelentkeztek be az Azure-portálra, kétféleképpen az erőforrás rendszerállapot lap eléréséhez: 

###<a name="open-the-resource-blade"></a>Nyissa meg az erőforrás lap
Nyissa meg az erőforrás lap egy adott erőforrás. A beállítások lap, amely megnyitja az erőforrás lap mellett kattintson az erőforrás állapot lap megnyitása erőforrás állapota. 

![Erőforrás rendszerállapot lap](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Súgó és támogatás lap
Nyissa meg a Súgó és támogatás a lap jobb felső sarokban a kérdőjel ikonra kattintva, majd válassza a Súgó + támogatás. 

**A felső navigációs sávon**

![Súgó + támogatás](./media/resource-health-overview/HelpAndSupport.png)

A csempére kattintva megnyílik az erőforrás állapot előfizetés lap, amely felsorolja az összes erőforrás az előfizetésben. Az egyes erőforrásokhoz nincs állapotát jelző ikon. Kattintson az egyes erőforrásokhoz az erőforrás rendszerállapot lap nyílik meg.

**Erőforrás-állapot csempe**

![Erőforrás-állapot csempe](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Mit jelent az erőforrás állapot állapot?
Vannak olyan előfordulhat, hogy az erőforrás 4 különböző egészségügyi állapotok.

### <a name="available"></a>Érhető el
A szolgáltatás nem észlelt problémák a platform, hogy az Erőforrás elérhetősége érintő sikerült. Egy zöld pipa ikonra ezt jelzi. 

![Erőforrás érhető el](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Nem érhető el

Ebben az esetben a szolgáltatást egy folyamatban lévő problémát talált a platform, hogy az erőforrás, például a csomópontra a virtuális futtató váratlanul indítani elérhető van érintő. Ez egy piros, figyelmeztető ikon jelzi. További információt a probléma szerepel az a lap középső részén együtt: 

1.  Milyen műveleteket Microsoft tart az erőforrás helyreállítása 
2.  A problémát, beleértve a várt megoldás időt részletes ütemterv
3.  Lista javasolt műveletek, felhasználók számára 

![Erőforrás nem érhető el.](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Nem érhető el: ügyfél által kezdeményezett
Az erőforrás egy ügyfél kérelmet, például az erőforrás leállítása vagy újraindítása kérő miatt nem érhető el. Ez a kék tájékoztató ikon jelzi. 

![Erőforrás a felhasználó által kezdeményezett művelet miatt nem érhető el](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Ismeretlen
A szolgáltatás nem kapott erőforrás információ a több, mint 5 percig tart. Ez egy szürke kérdőjel ikon jelzi. 

Fontos, hogy ne feledje, hogy ez nem a végleges jelzi, hogy van valami hiba történt az adott erőforráshoz, így kell végrehajtania fenti ajánlást:

* Ha az erőforrás meg a várt módon működik, de az állapot erőforrás állapot ismeretlen értékre van állítva, nincs problémák vannak, és számíthat az erőforrás frissíthető a megfelelő néhány perc múlva állapotát.
* Ha létezik, az erőforrás- és az állapot elérésével kapcsolatos problémákról erőforrás állapot ismeretlen értékre van állítva, ez a jövőben is okozhatja korai jelzi problémát és további nyomozásokban kell elvégezni mindaddig, amíg az állapot frissül, vagy a megfelelő, vagy a sérült

![Erőforrás-állapot ismeretlen.](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Szolgáltatás érintő események
Az erőforrás is hatással lehet egy folyamatban lévő szolgáltatás érintő eseményt, ha a fejléc az erőforrás rendszerállapot lap tetején jelenik meg. A szalagcímen kattintva megnyílik a az események lap, amely további információt a üzemszünetek jeleníti meg.

![Erőforrás-állapot egy SIE is hatással lehet](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Mit kell tudnom még az erőforrás-állapot?

### <a name="signal-latency"></a>Jel időtartama
A azt jelzi, hogy az erőforrás állapot hírcsatorna, a késleltetés legfeljebb 15 perc előfordulhat, hogy az erőforrás aktuális állapota állapotát és a tényleges elérhetőségét közötti eltéréseket okozhatják, amelyek. Fontos, hogy ez érdemes szem előtt tartani segít kiküszöbölheti a szükségtelen töltött időt vizsgáló lehetséges problémákat. 

### <a name="special-case-for-sql"></a>Az SQL speciális eset 
Erőforrás állapotjelentések nem az SQL server SQL-adatbázis állapotát. Ez az útvonal áttekintés nyújt további reálisan állapot kép, miközben igényel, hogy több összetevők és -szolgáltatások figyelembe kell venni az adatbázis állapotát jelző határozza meg. Az aktuális jel támaszkodik bejelentkezések az adatbázishoz, ami azt jelenti, hogy-adatbázisok mentsen normál bejelentkezések (tartalmazó egyebek mellett lekérdezés-végrehajtási kérések fogadása) az állapot állapot rendszeresen jelenik meg. Ha az adatbázis nem lett elérhető legalább 10 perc időszakra vonatkozóan, átkerül a állapota ismeretlen. Ezt jelenti, hogy az adatbázis nem érhető el, csak, hogy nincs jel van lett kibocsátott, mert nincs bejelentkezések elvégzését. Az adatbázis kapcsolódik, és a lekérdezés futtatása fog elhelyezése a jelek határozza meg, és az adatbázis állapotának frissítése szükséges.

## <a name="feedback"></a>Visszajelzés
Hogy mindig számára nyilvánosak, így visszajelzés és javaslatok! Küldjön nekünk [javaslatokat](https://feedback.azure.com/forums/266794-support-feedback). Ezenkívül is megtalál us [Twitter](https://twitter.com/azuresupport) , illetve az [MSDN-fórumok](https://social.msdn.microsoft.com/Forums/azure).
