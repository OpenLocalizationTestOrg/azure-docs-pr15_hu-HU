<properties 
    pageTitle="Árak és a számlázási szolgáltatás Bus |} Microsoft Azure"
    description="Árak struktúra szolgáltatás Bus áttekintése."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Árak és a számlázási szolgáltatás Bus

Az egyszerű, normál és [Premium](service-bus-premium-messaging.md) rétegek szolgáltatás Bus lehetősége. Megadhatja, hogy az egyes szolgáltatás Bus szolgáltatás névtere létrehozott egy szolgáltatási réteg, és a réteg kijelölés keresztül belül, a létrehozott összes entitás vonatkozik.

>[AZURE.NOTE] Aktuális szolgáltatás Bus árak részletes információt talál az [Azure Service Bus árak, oldal](https://azure.microsoft.com/pricing/details/service-bus/)és a [Szolgáltatás Bus – gyakori kérdések](service-bus-faq.md#service-bus-pricing).

Szolgáltatás Bus használ a következő két méter sorban várakozó és témakörök/előfizetések:

1. **Üzenetkezelés műveletek**: API-hívások ellen várólista vagy témakör/előfizetési szolgáltatás végpontok jelenti. Ez a állapotjelzője küldött vagy fogadott számlázható használatát az sorban várakozó és témakörök/elsődleges mértékegységként üzenetek helyére kerülnek.

2. **Kapcsolatok brokered**: definiált ellen sorban várakozó, témakörök vagy előfizetések egy adott egy óra mintavételnél időszakban nyissa meg az állandó kapcsolatok maximális számát. A kijelző csak alkalmazza a szabványos réteg, további kapcsolatok is megnyithatók (korábban kapcsolatok voltak korlátozva előfizetésenként várólista/témakör/100) névleges kapcsolati díjköteles.

A **szokásos** réteg vezet be, a sorok és a témakörök/előfizetések, így mennyiségi-alapú kedvezményére 80 %-a legmagasabb szintű használatát végrehajtott műveletek osztott árak. Egy szabványos réteg alap díjat $ 10 havonta, amely lehetővé teszi, hogy külön költség nélkül havonta legfeljebb 12,5 millió műveleteket is van.

A **prémium** réteg biztosít az erőforrás elkülönítési Processzor- és rétegben, hogy az egyes ügyfelek terhelést fut elkülönítési. Ez az erőforrás-tároló *egység üzenetküldés*neve. Minden egyes prémium névtér felosztása legalább egy üzenetben egység. 1, 2 vagy 4 üzenetben egységek minden szolgáltatás Bus prémium névtér vásárolhat. Egy egyetlen terhelést vagy a szervezet több üzenetben egységek is kiterjedhet, és az üzenetben egységek számát bármikor módosíthatók lesz, bár számlázási 24 órás vagy napi ráta díjak. A szolgáltatás Bus-alapú megoldás kiszámíthatóvá és megismételhető teljesítmény eredménye. Nemcsak a teljesítmény kiszámítható, és érhető el, de azt is gyorsabb. A tárhely motor Azure esemény hubok bevezetett épül Azure Service Bus prémium üzenetküldés. Prémium csevegést, a csúcs teljesítmény sokkal gyorsabb, mint a szokásos réteg.

Figyelje meg, hogy a szabványos alap díjat terheli csak egyszer Azure előfizetésenként havonta. Ez azt jelenti, hogy miután létrehozott egy egységes vagy prémium réteg szolgáltatás Bus névtér, lesz létrehozhatnak annyi további normál vagy prémium réteg névtér van szüksége, hogy ugyanazon Azure előfizetését, a további dátumalapértékhez díjakat nélkül.

November 1, 2014-es előtt létrehozott minden meglévő szolgáltatási Bus névtér automatikusan kerültek be a szabványos réteg. Ezzel biztosíthatja, hogy továbbra is van hozzáférésük az összes szolgáltatás Bus jelenleg elérhető szolgáltatások. Ezt követően az [Azure klasszikus portal][] segítségével vissza léptetheti, ha szükségesnek látja az egyszerű a réteg.

Az alábbi táblázat összefoglalja a Basic és a normál/Premium rétegek funkcionális eltérései.

|Lehetőség|Egyszerű|Normál/prémium verzió|
|---|---|---|
|Sorok|igen|igen|
|Ütemezett üzenetek|igen|igen|
|Témakörök/előfizetések|nem|igen|
|Jelfogók|nem|igen|
|Tranzakciók|nem|igen|
|Ismétlődések megszüntetése|nem|igen|
|Munkamenetek|nem|igen|
|Nagyméretű üzenetek|nem|igen|
|ForwardTo|nem|igen|
|SendVia|nem|igen|
|Brokered kapcsolatok (része)|Szolgáltatás Bus névtér 100|Azure előfizetésenként 1000|
|Brokered kapcsolatok (a hozzárendelések engedélyezett)|nem|Igen (számlázható)|

## <a name="messaging-operations"></a>Üzenetek műveletek

Az új árak modell részeként sorban várakozó és témakörök/előfizetés számlázási megváltoztatása. Ezek a szervezetek vannak áttérés a számlázási egy üzenet számlázás egy műveletet. Bármely ellen egy várólista vagy témakör/előfizetési szolgáltatás végpontjának API-hívás "Művelet" hivatkozik. Ide tartoznak a kezelés, a Küldés/fogadás és a munkamenet állam műveletek.

|Művelet|Leírás|
|---|---|
|Kezelése|Hozzon létre, Olvasás, frissítés, törlése (CRUD) sorban várakozó vagy témakörök/előfizetések szemben.|
|Csevegés|Üzenetek küldése és fogadása a sorok vagy témakörök/előfizetések.|
|A munkamenet-állapot|Az első, vagy a munkamenet-állapot beállítása a várakozási vagy témakör/előfizetés.|

A következő árak voltak hatékony November 1, 2014-es indítása:

|Egyszerű|Költség|
|---|---|
|Műveletek|$0,05 per millió műveletek|

|Normál|Költség|
|---|---|
|Alap költség|10 USD/hónap|
|Először 12,5 millió műveletek/hónap|Beépített|
|12,5-100 milliós műveletek/hónap|$0,80 per millió műveletek|
|100 milliós - 2500 millió műveletek/hónap|($ 0,50) millió műveletek|
|Több mint 2500 millió műveletek/hónap|$0,20 per millió műveletek|

|Támogatás|Költség|
|---|---|
|Napi|állandó kamatláb üzenet egységnyi $11.13|

## <a name="brokered-connections"></a>Brokered kapcsolatok

*Brokered kapcsolatok* , amely magában foglalja a "folyamatosan csatlakoztatott" feladók/címzett sorban várakozó, témakörök vagy előfizetések ellen sok ügyfél használatát mintázatok kiterjesztve azt. Folyamatosan csatlakoztatott feladók/vevők, azokat, amelyek csatlakozhat AMQP vagy a HTTP-nem nullával fogadása időtúllépése (például HTTP hosszú lekérdezési). HTTP küldő és az azonnali időkorlát nem eredményeznek brokered kapcsolatok.

Sorok és témakörök/előfizetések korábban legfeljebb 100 egyidejű kapcsolatok egy URL-CÍMÉT. A jelenlegi számlázási rendszer eltávolítja a sorok és témakörök/előfizetések-URL korlátját, és alkalmazza a kvótáinak és a szolgáltatás Bus névtér és Azure előfizetés szinten brokered kapcsolatok mérési.

Az egyszerű réteg tartalmazza, és szigorúan korlátozott, 100 brokered kapcsolatok per szolgáltatás Bus névtér. Az egyszerű réteg kapcsolatok száma feletti elutasításra kerül. A szokásos réteg eltávolítja a névtér korlát, és megszámolja, összesített brokered kapcsolat használata végig az Azure előfizetést. A szokásos réteg Azure előfizetésenként 1000 brokered kapcsolatok lesz jogosult külön költség (túl az alap ingyenesen) nélkül. Használata több, mint 1000 brokered kapcsolatok összesen Standard szintű szolgáltatás Bus névtér az Azure-előfizetésben fognak számlát kapni osztott időközönként, az alábbi táblázatban látható módon.

|Brokered kapcsolatok (normál réteg)|Költség|
|---|---|
|Az első 1000/havi|Alap ingyenesen részét képező|
|1000-100 000/hónap|$0,03 / kapcsolat/hó|
|500 000 100 000/hónap|$0,025 / kapcsolat/hó|
|500 000/hónap keresztül|$0.015 / kapcsolat/hó|

>[AZURE.NOTE] 1000 brokered kapcsolatok bekerülnek a szabványos üzenetben réteg (keresztül az alap ingyenesen), és minden sorban várakozó, témák és belül a társított Azure előfizetés az előfizetések megosztható.

<br />

>[AZURE.NOTE] Számlázási egyidejű kapcsolatok maximális száma alapján, és egyenletesen típusú, havonta 744 óra óránként alapján.

|Réteg prémium verzió
|---|
|A támogatás réteg nem terheli brokered kapcsolatok.|

Brokered kapcsolatok kapcsolatos további tudnivalókért lásd: Ez a témakör a [Gyakori kérdések](#faq) című szakaszában.

## <a name="relay"></a>Továbbítás

Csak a szokásos réteg névtér jelfogók érhetők el. Egyéb esetben jelfogók vonatkozó árak, és a kapcsolat változatlan marad. Ez azt jelenti, hogy jelfogók továbbra is ráterheljük a megadott üzenet (nem műveletek), és közvetítése óra.

|Árak közvetítése|Költség|
|---|---|
|Továbbítás óra|a 100 továbbítási óránként 0,10 $|
|Üzenetek|minden 10 000 üzenetek 0,01 $|

## <a name="faq"></a>GYAKORI KÉRDÉSEK

### <a name="how-is-the-relay-hours-meter-calculated"></a>Hogyan számítja ki a továbbítási óra állapotjelzője?

[Ebben a témakörben](service-bus-faq.md#how-is-the-relay-hours-meter-calculated)talál.

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Mik azok a kapcsolatok brokered, és hogyan végezze el tudom első az előfizetést őket?

Brokered kapcsolat definíciója az alábbiak egyikét:

1. AMQP származó ügyfélről szolgáltatás Bus várólista vagy témakör/előfizetésre.

2. Az üzenet szolgáltatás Bus témakör vagy egy fogadási időtúllépés nullánál nagyobb értéket tartalmazó várólista HTTP a hívást.

A szolgáltatás Bus díjai, amelyekre a található mennyiség (a szabványos réteg 1000) egyidejű brokered kapcsolatok maximális száma. Csúcsok óránkénti kombinálásával mért, egy adott hónapban 744 óra elosztja szerint egyenletesen típusú, és adja meg a havi számlázási időszak alatt. A egyenletesen óránkénti csúcsok összegét szemben a számlázási időszak végén található mennyiség (1000 brokered kapcsolatok / hó) érvényes.

Példa:

1. 10 000 eszközökhöz egyetlen AMQP kapcsolaton keresztül csatlakozik, és parancsok fogad a szolgáltatás Bus tematikus. Az eszközök telemetriai eseményeket egy esemény hubhoz küldje el. Ha az összes eszközön 12 óra naponta csatlakozni, a következő adatforgalmi költségek alkalmazása a (mellett az egyéb szolgáltatás Bus témakör díjak): 10 000 kapcsolatok *12 órás* 31 nap / 744 = 5000 brokered kapcsolatok. A havi támogatás 1000 brokered kapcsolatok után volna fel 0,03 $ a $120 összesen brokered kapcsolatot egy sebessége 4000 brokered kapcsolatok.

2. 10 000 eszközökön keresztül HTTP, a nullától időtúllépése megadása a szolgáltatás Bus sorból üzeneteket fogadni. Ha az összes eszközön 12 óra mindennap csatlakozni, látni fogja a következő adatforgalmi költségek (mellett az egyéb szolgáltatás Bus díjak): 10 000 HTTP fogadása kapcsolatok *napi 12 órával* 31 nap / 744 óra = 5000 brokered kapcsolatok.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Végezze el brokered adatforgalmi költségek alkalmazása sorban várakozó és témakörök/előfizetések?

igen. Vannak olyan nincs adatforgalmi költségek küldésének események HTTP, függetlenül attól, rendszerek és eszközök küldése számát használják. Brokered adatforgalmi költségek események fogadása a HTTP használatával nagyobb nullánál, más néven "hosszú lekérdezési," időtúllépés hoz létre. AMQP kapcsolatok miatt brokered adatforgalmi költségek, függetlenül attól, hogy a kapcsolatok használata érdekli küldéséhez és fogadásához. Fontos tudni, hogy 100 brokered kapcsolat engedélyezve van-e egy egyszerű névtér költség nélkül. Az is engedélyezett az Azure előfizetés brokered kapcsolatok maximális száma. Az első 1000 brokered kapcsolatok minden, szabványos névtér az Azure-előfizetésben keresztül is tartalmaz, külön költség nélkül (túl az alap ingyenesen). Mivel ezek juttatások elég sok szolgáltatás – üzenetküldési forgatókönyvek terjed ki, brokered adatforgalmi költségek általában csak lesz szükség, ha azt tervezi, hogy az ügyfelek; nagyszámú AMQP vagy HTTP hosszú lekérdezési használja Ha például hatékonyabb esemény streaming elérése vagy több eszközzel vagy szolgáltatásalkalmazás-példányok kétirányú kommunikációt engedélyezése.

## <a name="next-steps"></a>Következő lépések

- A szolgáltatás Bus árak, lásd: az [Azure Service Bus árak, oldal](https://azure.microsoft.com/pricing/details/service-bus/).

- Olvassa el az [Szolgáltatás Bus gyakran ismételt kérdések](service-bus-faq.md#service-bus-pricing) néhány gyakori gyakori kérdések a szolgáltatás bus árak, és a számlázási körül.

[Azure klasszikus portál]: http://manage.windowsazure.com