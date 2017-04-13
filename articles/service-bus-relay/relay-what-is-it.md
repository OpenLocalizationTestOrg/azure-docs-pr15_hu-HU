<properties
    pageTitle="Mi az Azure továbbítási? | Microsoft Azure"
    description="Azure továbbítási áttekintése"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Mi az Azure továbbítási?

Azure továbbítási szolgáltatáshoz úgy biztonságosan elérhetővé tenni a szolgáltatásokat, anélkül, hogy a kapcsolatot a tűzfalon, nyissa meg a nyilvános felhőben, egy vállalati vállalati hálózaton belül lakik, illetve a cég hálózati infrastruktúrájába beavatkozni szükség szerint lehetővé teszi a hibrid alkalmazásokat. Továbbítás számos különböző protokollokat és web services szabványokat támogat.

A továbbítási szolgáltatáshoz támogatja a hagyományos egyirányú kérés/válasz és társközi forgalmat. Esemény-eloszlás internet-hatókör közzététel/előfizetés esetek és a hatékonyság növelése pontok közötti kétirányú szoftvercsatorna kommunikáció engedélyezése a is támogat. 

Adatok továbbítani az átvitel szerkezetében helyszíni szolgáltatásainak keresztül egy kimenő port a továbbítási szolgáltatáshoz csatlakozik, és tartozik egy adott szinkronizálása címzett kapcsolati kétirányú szoftvercsatornát létrehoz. Az ügyfél majd kommunikálni a helyszíni szolgáltatás küld a forgalmat a továbbítási szolgáltatáshoz célba juttatása szinkronizálása címét. A továbbítási szolgáltatáshoz fog majd "közvetítése" adatokat a helyszíni szolgáltatásnak az egyes ügyfelek hozzárendelve kétirányú szoftvercsatornát keresztül. Az ügyfél, a helyszíni szolgáltatás közvetlen kapcsolat nem szükséges, nem szükséges, hogy hol található a szolgáltatást, és a helyszíni szolgáltatás nem kell minden bejövő portokat nyitva a tűzfalon keresztül.

A fő videofunkcióinak elemek továbbítási által biztosított kétirányú, nem pufferelt kommunikációs TCP-szerű szabályozásának, a végpont feltárás, a kapcsolat állapotának hálózati közötti együttműködést kívánja lehetővé, és végpont biztonsági átfedett. A továbbítási funkciók hálózaton szinten integrációs technológiák, például, hogy akkor is kell hatóköre az egyetlen alkalmazásból egyik végpontját egyetlen számítógépen, a VPN-technológia sokkal tolakodó, ahogy azt támaszkodik a hálózati környezet megváltoztatása közben a VPN eltérnek.

Azure továbbítási két funkciókkal rendelkezik:

1. [Hibrid kapcsolatok](#hybrid-connections) - többplatformos forgatókönyvek engedélyezése megnyitott szabványos webes Sockets használja

2. [WCF jelfogók](#wcf-relays) - használja a Windows Communication Foundation engedélyezése a távoli lépésről hívások

Hibrid kapcsolatok és a WCF relék mindkét engedélyezze a biztonságos kapcsolatot assests megtalálható a vállalati vállalati hálózaton belül. Az egymás alatti szolgáltatás használata az adott igényeinek részletes alatti függ:

|                                    | WCF-továbbító | Hibrid kapcsolatok |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **.NET core**                      |           |         x          |
| **.NET-keretrendszer**                 |     x     |         x          |
| **A JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Szabványos alapú protokollhoz**  |           |         x          |
| **Több RPC Programing modellek** |           |         x          |
*<sub>Általános elérhetőség szerint</sub>

## <a name="hybrid-connections"></a>Hibrid kapcsolatok

Azure továbbítási hibrid kapcsolatok lehetőség a meglévő továbbítási funkciók, amelyek lehetnek egy biztonságos, a Megnyitás-protocol alakulása végrehajtott bármely platformon és bármilyen nyelven, amely tartalmaz egy egyszerű WebSocket képesség, tartalmazó kifejezetten az WebSocket API közös böngészők. Hibrid kapcsolatok HTTP és WebSockets alapul.

## <a name="wcf-relays"></a>WCF jelfogók

A WCF-továbbító a teljes .NET keretrendszer (NETFX) a és a WCF-működik. A kapcsolat a helyszíni szolgáltatásban és a továbbítási szolgáltatáshoz, használja a WCF "továbbítási" kötések-öt között kezdeményez. Bepillantás a színfalak mögé a továbbítási kötések célja, hogy a felhőben szolgáltatás Bus együttműködő WCF csatorna összetevőket hozhat létre új átviteli kötés elemekhez rendelése.

## <a name="service-history"></a>Szolgáltatási előzmények

Hibrid kapcsolatok transplants az előbbi egyaránt nevű "BizTalk szolgáltatások" szolgáltatás, amely az Azure Service Bus WCF továbbítási lett épül. Az új hibrid kapcsolatok funkció kiegészíti a meglévő WCF-továbbító, és ezeket a két szolgáltatás lehetőségeket akkor léteznek egymás melletti a továbbítási szolgáltatáshoz tartozó a közeljövőben; a oszthatnak meg átjárókat közös, de más módon különböző megvalósítás.

WCF továbbítási a régi továbbítási kínáló, előfordulhat, hogy vevőknek már használata az WCF programing adatmodelleket.

## <a name="next-steps"></a>A következő lépéseket:

- [Továbbítás – gyakori kérdések](relay-faq.md)
- [Névtér létrehozása](relay-create-namespace-portal.md)
- [Első lépések a .NET rendszerhez](relay-hybrid-connections-dotnet-get-started.md)
- [Csomópont – első lépések](relay-hybrid-connections-node-get-started.md)