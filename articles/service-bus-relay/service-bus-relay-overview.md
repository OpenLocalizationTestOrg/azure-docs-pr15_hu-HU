<properties
    pageTitle="Szolgáltatás Bus továbbítási áttekintése |} Microsoft Azure"
    description="Szolgáltatás Bus továbbítási áttekintése."
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
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Szolgáltatás Bus továbbítási áttekintése

A szolgáltatás Bus fő összetevője központi (de nagyon terheléselosztás) *továbbítási* szolgáltatás, amely lehetővé teszi, hogy az Azure adatközponthoz, mind a saját helyszíni vállalati környezetben futtatott hibrid alkalmazásokat összeállítása.  A szolgáltatás Bus továbbítási számos különböző protokollokat és web services szabványokat támogat. Ide tartoznak, SOAP, WS-*, sőt akár a többi. A továbbítási szolgáltatáshoz úgy biztonságosan elérhetővé tenni a Windows Communication Foundation (WCF) szolgáltatásokat, amelyek anélkül, hogy a kapcsolatot a tűzfalon, nyissa meg a nyilvános felhőben, egy vállalati vállalati hálózaton belül lakik, illetve a cég hálózati infrastruktúrájába beavatkozni szükség szerint lehetővé teszi a hibrid alkalmazásokat. 

![Továbbítás fogalmak](./media/service-bus-relay-overview/sb-relay-01.png)

A továbbítási szolgáltatáshoz támogatja a hagyományos egyirányú üzenetküldés, üzenetküldés kérés/válasz és társközi üzenetküldés. Esemény-eloszlás internet-hatókör közzététel/előfizetés esetek és a hatékonyság növelése pontok közötti kétirányú szoftvercsatorna kommunikáció engedélyezése a is támogat. 

Szerkezetében relayed üzenetben a helyszíni szolgáltatásainak keresztül egy kimenő port a továbbítási szolgáltatáshoz csatlakozik, és létrehoz egy kétirányú szoftvercsatornát tartozik egy adott szinkronizálása címzett kapcsolati. Az ügyfél majd kommunikálni a helyszíni szolgáltatás üzeneteket küld a továbbítási szolgáltatáshoz célba juttatása szinkronizálása címét. A továbbítási szolgáltatáshoz fog majd "" küldött üzenetek továbbítására a helyszíni szolgáltatás már a hely a kétirányú szoftvercsatorna keresztül. Az ügyfél, a helyszíni szolgáltatás közvetlen kapcsolat nem szükséges, nem szükséges, hogy hol található a szolgáltatást, és a helyszíni szolgáltatás nem kell minden bejövő portokat nyitva a tűzfalon keresztül.

A kapcsolat a helyszíni szolgáltatásban és a továbbítási szolgáltatáshoz, használja a WCF "továbbítási" kötések-öt között kezdeményez. Bepillantás a színfalak mögé a továbbítási kötések célja, hogy a felhőben szolgáltatás Bus együttműködő WCF csatorna összetevőket hozhat létre új átviteli kötés elemekhez rendelése. 

## <a name="next-steps"></a>Következő lépések

A szolgáltatás Bus továbbítási olvashat az alábbi témakörökben.

- [Azure szolgáltatás Bus építészeti – áttekintés](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [A szolgáltatás Bus továbbítási szolgáltatáshoz használata](service-bus-dotnet-how-to-use-relay.md)

 