<properties
    pageTitle="Üzenetkezelés áttekintése szolgáltatás Bus |} Microsoft Azure"
    description="Szolgáltatás Bus üzenetküldés: rugalmas adatok kézbesítési a felhőben"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Szolgáltatás Bus üzenetküldés: rugalmas adatok kézbesítési a felhőben

Microsoft Azure Service Bus olyan megbízható információk kézbesítési szolgáltatás. Ez a szolgáltatás célja, így könnyebben kommunikációt. Ha két vagy több fél szeretné, hogy exchange-adatok, egy kommunikációt szükségük van. Szolgáltatás Bus brokered vagy a külső kommunikációt. Hasonlít a fizikai világ postai szolgáltatás. Postai megkönnyítik a nagyon levelek és kézbesítési garanciákkal, különféle csomagok különböző típusú elküldése a világ bármely pontjára.

Hasonló a levelek kézbesítése postai szolgáltatás, szolgáltatás Bus nem rugalmas információk kézbesítési a feladó és a címzett egyaránt. Az üzenetben szolgáltatás biztosítja, hogy az adatok kézbesítve van, akkor is, ha a két fél soha nem mindkét online egyszerre, vagy ha az azok nem érhetők el a pontos egy időben. Ezzel a módszerrel üzenetküldés hasonlít küld levelet, közben nem brokered kommunikációs hasonlít úgy, hogy a hívni (vagy - hívás várakozási és a hívó ID, amely rengeteg, például brokered üzenetküldés előtt kell hívni használatának).

Az üzenet feladója is megszabhatja kézbesítési jellemzők, például a tranzakciókat, ismétlődő észlelése, időalapú lejárati és kötegelés számos. Ezek a minták van, valamint a postai analógiákat: ismételje meg a kézbesítési, a szükséges aláírás, a cím módosítása vagy a visszahívás.

Szolgáltatás Bus két különböző üzenetben mintázatok támogatja: *továbbítási* és *brokered üzenetküldés*.

## <a name="service-bus-relay"></a>Szolgáltatás Bus továbbító

A szolgáltatás Bus [továbbítási](../service-bus-relay/service-bus-relay-overview.md) összetevője számos különböző protokollokat és Web services szabványokat támogató központi (de nagyon terheléselosztás) szolgáltatás. Ide tartoznak, SOAP, WS-*, sőt akár a többi. A [továbbítási szolgáltatáshoz](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) számos különböző továbbítási kapcsolat lehetőséget, és közvetlen-társközi kapcsolatok egyeztetni, amikor lehetséges segítséget. Szolgáltatás Bus .NET fejlesztők számára, akik használják a Windows Communication Foundation (WCF), mind a teljesítmény és használhatóság kapcsolatban van optimalizálva, és a továbbítási szolgáltatáshoz, SOAP és a többi felületeken keresztül teljes hozzáférést biztosít. Ez teszi lehetővé bármely SOAP vagy a többi fejlesztői környezet szolgáltatás Bus integrálása.

A továbbítási szolgáltatáshoz támogatja a hagyományos egyirányú üzenetküldés, üzenetküldés kérés/válasz és társközi üzenetküldés. Esemény-eloszlás Internet-hatókör ahhoz, hogy a is támogat közzététel előfizetés forgatókönyveket és szoftvercsatorna kétirányú kommunikációt a hatékonyság növelése pontok közötti. Szerkezetében relayed üzenetben a helyszíni szolgáltatásainak keresztül egy kimenő port a továbbítási szolgáltatáshoz csatlakozik, és létrehoz egy kétirányú szoftvercsatornát tartozik egy adott szinkronizálása címzett kapcsolati. Az ügyfél majd kommunikálni a helyszíni szolgáltatás üzeneteket küld a továbbítási szolgáltatáshoz célba juttatása szinkronizálása címét. A továbbítási szolgáltatáshoz fog majd "" küldött üzenetek továbbítására a helyszíni szolgáltatás már a hely a kétirányú szoftvercsatorna keresztül. Az ügyfél, a helyszíni szolgáltatás közvetlen kapcsolat nem szükséges, sem van szükség, hogy hol található a szolgáltatást, és a helyszíni szolgáltatás nem kell minden bejövő portokat nyitva a tűzfalon keresztül.

Kezdeményez a kapcsolat a helyszíni szolgáltatásban és a továbbítási szolgáltatáshoz között a WCF "továbbítási" kötések-öt használ. Bepillantás a színfalak mögé a továbbítási kötések megfeleltetése átviteli kötési eleme célja, hogy a felhőben szolgáltatás Bus együttműködő WCF csatorna összetevőket hozhat létre.

Szolgáltatás Bus továbbítási számos előnyt kínál, de megköveteli a kiszolgáló és az ügyfél mindkét online annak érdekében, hogy az üzenetek küldése és fogadása egy időben. Ez nem optimális, amelyben a kérések nem lehet a szokásos élettartamú HTTP stílusú kommunikációhoz, sem pedig az ügyfelek, amely csak alkalmanként csatlakozás böngészők, például mobilalkalmazások, és így tovább. Támogatja a leválasztott kommunikációs üzenetküldés brokered, és a saját előnyökkel jár; ügyfelek és kiszolgálók lehet csatlakozni, ha szükséges, és végezze el a műveletek aszinkron módon.

## <a name="brokered-messaging"></a>Üzenetkezelés brokered

A továbbítás rendszer alkalmazással szemben [brokered üzenetküldés](service-bus-queues-topics-subscriptions.md) lehet olyan szerint aszinkron, vagy "temporally leválasztott." Gyártók (feladók) és a fogyasztói (címzett) nem kell lenniük online egy időben. Az üzenetkezelési infrastruktúra mindaddig, amíg a igénybe fél fogadása őket készen áll a biztos, hogy tárolja az "ügynök" üzenetek (például egy várólista). Lehetővé teszi a megszakítását, vagy önkéntes; elosztott alkalmazás összetevői Ha például karbantartási, vagy egy összetevő összeomlik miatt, nélkül érintő a teljes rendszerben. Ezenkívül a fogadó alkalmazás csak lehet a nap, például egy készlet rendszer, amely csak a munkanap végén futtatásához szükséges bizonyos időszakokban online állapotba.

A szolgáltatás Bus brokered üzenetben infrastruktúra fő összetevői sorok, a témakörök és az előfizetéseket.  Az elsődleges különbség, hogy a témakörök közzététel/előfizetés összetett tartalom-alapú továbbítás és a kézbesítés logika, beleértve a küld a címzetteknek több használható funkciókat támogatja. Ezek az összetevők új aszinkron üzenetküldési forgatókönyvek, például időbeli szétválasztás engedélyezése, közzététel és előfizetés és terheléselosztás. További információt a következő üzenetben szervezetek lásd: a [szolgáltatás Bus sorban várakozó, témák, és előfizetések](service-bus-queues-topics-subscriptions.md).

A továbbítás infrastruktúra és a brokered csevegési funkció WCF és a .NET-keretrendszer programozóknak és többi keresztül kapja.

## <a name="next-steps"></a>Következő lépések

További információk a szolgáltatás Bus üzenetben, a következő témakörökben olvashat:

- [Szolgáltatás Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
- [Szolgáltatás Bus sorban várakozó, témák és előfizetések](service-bus-queues-topics-subscriptions.md)
- [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)
- [Szolgáltatás Bus témakörök és előfizetések használatával](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
