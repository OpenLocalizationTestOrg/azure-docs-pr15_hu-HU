<properties 
    pageTitle="Szolgáltatás Bus párosított névtér |} Microsoft Azure"
    description="Párosított névtér végrehajtása részletek és a költség"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Párosított névtér végrehajtása részletek és a következmények költség

A [PairNamespaceAsync][] módszerrel, egy [SendAvailabilityPairedNamespaceOptions][] példány látható feladatokat az Ön nevében hajt végre. Mivel ott vannak költség előtt megfontolandó kérdések a funkció használata esetén, célszerű megérteni a felelősöket, hogy a várt a vezérlő viselkedése, amikor ez történik. Az API az Ön nevében a következő automatikus viselkedés folytat:

-   Tartalék sorban várakozó létrehozása.
-   Sorok vagy témakörök folytatott beszélgetést [MessageSender][] -objektum létrehozása.
-   Amikor egy üzenetben személyhez nem érhető el, küld üzeneteket a személyhez kísérlet észleli, hogy a szervezet elérhetővé ismét a ping parancsot.
-   Tetszés szerint a "üzenet szivattyúk" halmazának hoz létre, amely üzenetek áthelyezése a tartalék sorokból az elsődleges sorban várakozó.
-   Koordinálja az elsődleges és másodlagos [MessagingFactory][] példányainak záró vagy hibás.

Magas szintű, a funkció a következőképpen működik: Ha az elsődleges entitás megfelelő, nincs működését változásokról. Ha a [FailoverInterval][] időtartam letelik, és az elsődleges entitás látja nem sikeres küld egy nem tranziens [MessagingException][] vagy [TimeoutException][]után a következő történik:

1.  Az elsődleges személyhez műveletek le vannak tiltva, és a rendszer az elsődleges entitás pingelése, amíg a ping sikeresen kézbesítve küldje el.

2.  A véletlenszerű tartalék várólista be van jelölve.

3.  A kiválasztott tartalék várólista [BrokeredMessage][] objektumok résztvevőkhöz.

1.  Ha a kiválasztott tartalék várakozási sorban található a Küldés művelet sikertelen, a Forgatás lekért van, hogy a várakozási, és egy új sor be van jelölve. Minden feladónak [MessagingFactory][] példányon ismerje meg, a hiba.

Az alábbi ábra a sorozat jelzik. Első lépésként a feladó küld üzeneteket.

![Párosított névtér][0]

Az elsődleges várólista küldeni sikertelenség a feladó megkezdi üzeneteket küld egy véletlenszerűen kiválasztott tartalék várólista. Egyszerre egy ping tevékenység kezdődik.

![Párosított névtér][1]

Ezen a ponton a üzenetek továbbra is a másodlagos sorban és a elsődleges sorba nem lett kézbesítve. Miután az elsődleges várólista megfelelő újra, legalább egy folyamat futnia kell a Szifonos. A Szifonos továbbítja az üzeneteket különböző tartalék sorok a megfelelő cél személyek (sorban várakozó és témakörök).

![Párosított névtér][2]

Osztásának maradéka Ez a témakör azt ismerteti, hogy az adott részletek szerinti a darabot működéséről.

## <a name="creation-of-backlog-queues"></a>A sorok tartalék létrehozása

A [PairNamespaceAsync][] módszerrel átadott [SendAvailabilityPairedNamespaceOptions][] objektum azt jelzi, hogy használni kívánt tartalék sorok számát. Minden tartalék sor létrejön a következő tulajdonságok explicit módon beállított (minden más érték a [QueueDescription][] alapértelmezett értékek vannak beállítva):

| Elérési út                                   | [elsődleges névtér] / x-servicebus-átadási / [index] [index] [0 BacklogQueueCount) érték esetén |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 perc                                                                                             |
| EnableDeadLetteringOnMessageExpiration | Igaz                                                                                                 |
| EnableBatchedOperations                | Igaz                                                                                                 |

Ha például az első tartalék várólista létrehozott névtér **kontraktor** nevű `contoso/x-servicebus-transfer/0`.

A sorok létrehozásakor a kódját először ellenőrzi, hogy egy várakozási sora van-e. Ha a sor nem létezik, akkor a sor jön létre. A kód nem karbantartása "extra" tartalék sorban várakozó. Kifejezetten Ha az alkalmazást az elsődleges névtér **contoso** kéri, hogy az öt tartalék, de a elérési tartalék várólista sorban várakozó `contoso/x-servicebus-transfer/7` létezik, a felesleges tartalék várólista továbbra is megtalálható, de nem használható. A rendszer kifejezetten lehetővé teszi, hogy a felesleges tartalék sorban várakozó létezik, amely nem használhatók. A névtér tulajdonosaként bármely még nem használt/nemkívánatos tartalék sorban várakozó megtisztítása felelős. A döntés oka az, hogy szolgáltatás Bus nem tudja, hogy milyen célból léteznek a névtér sorban várakozó. Továbbá ha létezik ilyen nevű várólista, de nem felel meg a felvett [QueueDescription][], majd a okai az alapértelmezett működés módosítása az egyéni. Semmilyen garanciát a kód készítette a tartalék sorban várakozó módosítása. Győződjön meg arról, hogy a módosítások ellenőrzéséhez alapos.

## <a name="custom-messagesender"></a>Egyéni MessageSender

Küldi el, ha minden üzenet feldolgozzuk a szokásos módon viselkedik, amikor minden működik, és amikor dolog, amit "oldaltörés." irányítja át a tartalék sorban várakozó belső [MessageSender][] objektum Nem tranziens hiba fogadásakor időzítőt kezdődik. A [FailoverInterval][] tulajdonság értékét, ameddig nem sikeres üzenetküldés álló [időszak][] lejárta után a feladatátvételi be kell kapcsolni. Ezen a ponton a következő műveleteket fordulhat elő, ha minden:

- Ping tevékenység végrehajtja a minden [PingPrimaryInterval][] kattintva ellenőrizze, hogy a szervezet nem érhető el. Miután létrejött a feladat végrehajtásához, az összes közvetlenül a szervezet használó ügyfél-kód elindul, az elsődleges névtér az új üzenetek küldése.

- Jövőbeli kérések szeretne küldeni, hogy bármely más feladóktól érkező ugyanazon entitás a [BrokeredMessage][] küldött módosítani kell, hogy a tartalék várólista ülnie eredményez. A módosítás egyes tulajdonságai eltávolítja a [BrokeredMessage][] objektum és máshol tárolja őket. A következő tulajdonságokat nincs bejelölve, és a hozzáadott új aliast, szolgáltatás Bus és a SDK üzenetek egyenletesen engedélyezése csoportban:

| Régi tulajdonság neve       | Új tulajdonság neve |
|-------------------------|-------------------|
| Munkamenet-azonosító               | x – az ms-munkamenet-azonosító    |
| Élettartam              | az x-ms-élettartam   |
| ScheduledEnqueueTimeUtc | x – az ms-elérési út         |

Az eredeti cél elérési útja is x-ms-path nevű tulajdonság tárolja az üzeneten belül. A tervezés lehetővé teszi, hogy sok entitás való együttműködéshez egyetlen tartalék várólista üzeneteket. A Tulajdonságok vissza az Szifonos fordítja.

Az egyéni [MessageSender][] objektum is problémába, amikor üzenetek megközelíthető a 256 KB-os korlát és feladatátvevő be kell kapcsolni. Az egyéni [MessageSender][] objektum minden sorban várakozó és témakörök üzenetek tartalék sorok közös tárolja. Az objektum vegyes közös belül az tartalék sorban várakozó sok elsődleges származó üzeneteket. Kezelheti a terheléselosztás, nem tudja, hogy egymást sok ügyfél között, a SDK csomagjában talál véletlenszerűen választja ki az egyik tartalék várakozási az egyes [QueueClient][] vagy [TopicClient][] hoz létre a kódot.

## <a name="pings"></a>Ping

Ping üzenet egy üres [BrokeredMessage][] annak [ContentType][] tulajdonság beállítása alkalmazás/vnd.ms-servicebus-ping és az 1 másodperc [élettartam][] értéket. Ez a ping rendelkezik egy speciális jellemzők szolgáltatás Bus: a kiszolgáló soha nem biztosítja a ping, amikor bármelyik hívó kér egy [BrokeredMessage][]. Így Önnek nem kell megtudhatja, hogy miként és figyelmen kívül. Minden személy (egyedi várólista vagy témakör) ügyfelenként [MessagingFactory][] példányonként fog pingkérést küldött, nem érhető el kell mutatniuk. Alapértelmezés szerint ez történik percenkénti. Ping üzeneteket minősülnek normál szolgáltatás Bus üzeneteket, és sávszélesség és az üzenetek díjai eredményezhet. Az üzenetek leállítása amint az ügyfelek észleli, hogy a rendszer nem érhető el,

## <a name="the-syphon"></a>A Szifonos

Az alkalmazás legalább egy végrehajtható aktívan futnia kell a Szifonos. A Szifonos végrehajtása hosszú szavazás kap, amelyek 15 percig tart. Minden entitás érhetők el, és 10 tartalék sorban várakozó van, amikor az alkalmazás a Szifonos üzemeltető felhívja a 40 alkalommal óránként, 960 időpontok napi és 30 nap múlva 28800 időpontok a fogadási művelet. Amikor a Szifonos van aktívan üzenetek áthelyezése a tartalék az az elsődleges sorba, a minden üzenet élmény a következő díjat (a üzenetméret és sávszélesség szabványos tarifa szerint valamennyi szakaszában):

1.  Küldje el a tartalék.

2.  A tartalék fogadni.

3.  Az elsődleges küldje el.

4.  Az elsődleges fogadni.

## <a name="closefault-behavior"></a>Bezárás/hibafa viselkedése

Tárolja a Szifonos, egyszer az elsődleges és másodlagos [MessagingFactory][] hibáit, vagy nincs megnyitva, annak a partnernek nélkül alkalmazáson belül is alatt álló hibával/bezárja, majd a Szifonos észleli ezt az állapotot a Szifonos jogszabályok. Ha a többi [MessagingFactory][] nincs megnyitva, 5 másodpercek, a Szifonos a még nyitva [MessagingFactory][]fog hiba.

## <a name="next-steps"></a>Következő lépések

[Aszinkron üzenetben mintázatok és elérhetőséget][] szolgáltatás Bus aszinkron üzenetküldés részletes információkat talál. 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Időszak]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [Élettartam]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Aszinkron üzenetben mintázatok és elérhetőséget]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
