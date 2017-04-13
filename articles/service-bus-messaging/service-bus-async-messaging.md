<properties 
    pageTitle="Aszinkron üzenetküldési szolgáltatás Bus |} Microsoft Azure"
    description="Aszinkron üzenetküldési szolgáltatás Bus leírását."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Aszinkron üzenetben mintázatok és elérhetőséget

Aszinkron üzenetküldés számos különböző módon hajtható végre. Sorok, a témakörök és az előfizetéseket Azure Service Bus támogatja az áruház és a továbbítási mechanizmusa asynchrony. Normál (szinkron) műveletben üzeneteket küldeni a súgótémaköröket, és a sorok és a sorok és az előfizetéseket érkező üzenetek fogadására. Alkalmazások, akkor ezek személyek mindig a rendelkezésre álló éppen függ. A szervezet állapot megváltozásakor körülmények között számos miatt szükséges oly módon, hogy egy korlátozott képességek entitás többségéhez is megfelelnek.

Alkalmazások általában aszinkron üzenetben mintázatok használható kommunikációs helyzetekben számos engedélyezéséhez. Készíthet alkalmazást, amelyben ügyfelek üzeneteket küldhet szolgáltatások, akkor is, ha nem fut a szolgáltatás. Alkalmazások kommunikáció, várólista felszakadásáig segítséget tapasztalatok szint a betöltés, mert a hely pufferelési kommunikáció. Végül egy egyszerű, de a hatékony terheléselosztó több számítógépen üzenetek terjesztésére elérheti.

Annak érdekében, hogy ezek a szervezetek elérhetősége, fontolja meg, amelyben a szervezetek is megjelennek nem érhető el, tartós üzenetkezelő rendszer különböző módokon. Általánosságban elmondható láthatja a szervezet elérhetetlenné azt írja be az alábbi módszerekkel alkalmazások:

- Nem lehet küldeni.

- Nem jelennek meg.

- Nem lehet személyek kezelése (létrehozása, beolvasásához, frissítése és törlése egységek).

- Nem lehet kapcsolódni a szolgáltatást.

Minden egyes e hibák a különböző hiba módok létezik, amelyek lehetővé teszik-alkalmazás továbbra is csökkentett képesség néhány szintjén vonhatnak. A rendszer, amelyet küldésére, de nem kap például rendelések továbbra is kaphatnak a felhasználók, de nem lehet feldolgozni azokat a rendelések. Ez a témakör azt ismerteti, hogy a lehetséges problémák, amelyek akkor fordulhat elő, és hogyan vannak szünteti meg azokat a problémákat. Szolgáltatás Bus vezetett megoldásokkal kapcsolatban, akkor be kell kiválaszthatják, amely számos, és ez a témakör is ismerteti, hogy az adott beállításánál megoldásokkal kapcsolatban használatára vonatkozó szabályokat.

## <a name="reliability-in-service-bus"></a>Megbízhatósága a szolgáltatás Bus

Többféle módon üzenetet, és a szervezet kapcsolatos problémák kezelése, és ezeket megoldásokkal kapcsolatban megfelelő használatára vonatkozó irányelveket. Az útmutatásokat megérteni, először látnia kell Mi a szolgáltatás Bus történhet. Azure rendszerek felépítésének miatt az összes ezeket a problémákat általában rövid életű. Magas szintű a különböző okok elérhetetlenség a következőképpen jelenik meg:

-   Szabályozásának egy külső rendszer szolgáltatás Bus függ. A tárhely és a számítási erőforrások interakciók szabályozásának fordul elő.

-   A probléma rendszer szolgáltatás Bus függ. Ha például egy adott részének tárterület is problémába.

-   Egyetlen alrendszerek szolgáltatás Bus hiba. Ebben az esetben a számítási csomópont egy következetes állapotba elérheti és kell indítani magát, okozó összes entitás terheléselosztó csomóponton szolgál. Ez a okozhat lassú üzenet feldolgozási rövid idő.

-   Az Azure adatközponthoz belül szolgáltatás Bus hiba. Ez a "Katasztrofális hiba" során, amelyeket a rendszer nem érhető el az ennyi perc vagy néhány órát.

> [AZURE.NOTE] A kifejezés **tároló** jelentheti Azure-tárhely és a SQL Azure.

Szolgáltatás Bus tartalmazza a jelen hibák megoldásokkal kapcsolatban. Az alábbi szakaszokból megtudhatja, hogy minden probléma és a megfelelő megoldásokkal kapcsolatban.

### <a name="throttling"></a>Szabályozása

A szolgáltatás Bus szabályozásának együttműködési üzenet ráta kezelését teszi lehetővé. Minden egyes szolgáltatás Bus csomópont sok entitás otthont ad. Processzor, a memóriahasználat, a tárhely és a más metszettel a rendszer minden e szervezetek teszi igényeivel. Ha bármelyik ezek metszettel észleli, hogy használatát meghaladja a definiált küszöbértékek, a szolgáltatás Bus megtagadhatja egy adott kérelem. A hívó kap egy [ServerBusyException][] és próbálkozások 10 másodperc után.

A kezelési, mint a kódot kell olvassa el a hiba és az üzenet bármely újrapróbálkozások megállítás legalább 10 másodpercig. A hiba akkor fordulhat elő, az ügyfél alkalmazás részei között, mert várhatóan, hogy egymástól függetlenül minden darab végrehajtja a az újraküldés logikája. A kód csökkentheti, mivel a szétválasztás várólista vagy témakör éppen szabályozott valószínűsége.

### <a name="issue-for-an-azure-dependency"></a>A probléma az Azure függőséget

Azure belül más összetevő szolgáltatásproblémák alkalmanként van. Például a rendszert, amelyet használ szolgáltatási Bus frissítésekor, hogy a rendszer ideiglenes sorolunk csökkentett képességeit. Az ilyen típusú problémák megkerüléséhez a szolgáltatás Bus rendszeresen megvizsgálja, és végrehajtja a megoldásokkal kapcsolatban. Az alábbi megoldásokkal kapcsolatban hatásai jelennek meg. Átmeneti problémák tárhely kezelése, például a szolgáltatás Bus alkalmazza, amely lehetővé teszi az üzenet küldése műveletek egységes munkára rendszer. A kezelési jellegét, miatt elküldött üzenet legfeljebb 15 percig is eltarthat jelennek meg a szóban forgó várólista vagy előfizetést, és készen áll a fogadási művelet. Általánosságban elmondható a legtöbb szervezetek nem fog tapasztalni a probléma. Azonban belül Azure Service Bus egységek számát tekintve a kezelési néha szükséges szolgáltatás Bus ügyfelek egy kisebb részét.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Egy egyetlen alrendszer szolgáltatás Bus hiba

Bármely alkalmazásban körülmények között nem következetes válhat szolgáltatás Bus egy belső összetevője okozhatja. Amikor szolgáltatás Bus Ez azt észleli, diagnosztizálása, hogy mi történt a támogatási alkalmazásból gyűjt adatokat. Miután az adatgyűjtés, az alkalmazás újraindítása a kísérlet egységes állapotra való visszatéréshez. Ez a folyamat meglehetősen gyorsan történik, és eredmény jelenik meg, hogy hányszor lefelé tipikus nem lesz elérhető a felfelé néhány perc alatt entitás sokkal rövidebb.

Ezekben az esetekben az ügyfélalkalmazás olyan [System.TimeoutException][] vagy [MessagingException][] kivétel hoz létre. Szolgáltatás Bus egy erre a problémára ügyfélprogram automatikus újrapróbálkozási logika formájában kezelési tartalmazza. Miután a periódus merülnek van, és nem kézbesítve az üzenet, feltárása a egyéb szolgáltatások, például [párosított névtér][]használatával. Párosított névtér más telepítés által okozott problémák ebben a cikkben ismertetett van.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Szolgáltatás Bus meghibásodása belül az Azure adatközponthoz

Az Azure adatközpontban hiba legvalószínűbb oka egy sikertelen frissítési telepítési szolgáltatás Bus vagy egy függő rendszer. Ahogy a platform amelyet tartalmaz, annak valószínűségét, hogy ilyen típusú hiba van csökken. Adatközponthoz hiba akkor is fordulhat elő, amely tartalmazza a következő okokból:

-   Elektromos üzemszünetek (power ellátási és eltűnik létrehozása power).
-   Kapcsolat (internetes oldaltörés az ügyfelek és Azure között).

Mindkét esetben természeti vagy végtelen katasztrófa okozza a problémát. Kerülhető meg, és győződjön meg arról, hogy továbbra is küldhet üzenetet, használhatja [párosított névtér][] ahhoz, hogy küldeni egy másik helyre, miközben az elsődleges helye lett megfelelő újra. További tudnivalókért lásd: [Gyakorlati tanácsok a szigetelő alkalmazások szolgáltatás Bus kimaradások és katasztrófák szemben][].

## <a name="paired-namespaces"></a>Párosított névtér

A [páronkénti névtér][] szolgáltatás lehetőséget támogat, amelyben a szolgáltatás Bus entitás vagy egy adatközpont belül telepítési elérhetetlenné válik. Miközben az esemény ritkán elosztott rendszerek továbbra is kell készíteni legrosszabb eset esetek kezelése. A szokásos Ez az esemény oka az, hogy néhány elem szolgáltatás Bus függ egy rövid érvényességi idejű problémát tapasztalja. Alkalmazás elérhetősége egy üzemszünetek megtartásához szolgáltatás Bus felhasználók segítségével két külön névtér, lehetőleg a külön adatközpontokban, az üzenetkezelési személyek szolgáltató. Ez a szakasz hátralévő használja az alábbi kifejezések:

-   Elsődleges névtér: A névtér, amellyel az alkalmazás hogyan kommunikáljon a küldési és fogadási műveletek.

-   Másodlagos névtér: A névtér, amely végpontként az elsődleges névtér biztonsági másolatot. Alkalmazás logika nem működik a névtér együtt.

-   Feladatátvevő intervallum: fogadja el a normál hibák, mielőtt az alkalmazás – az elsődleges névtér Váltás a másodlagos névtér idő mennyiségét.

Párosított névtér támogatási *Elérhetőség küldése*. Küldje el az elérhetőség megőrzi az azt jelenti, hogy üzeneteket küldeni. Elérhetőség küldése használja, az alkalmazás az alábbi követelmények kell megfelelnie:

1.  Az elsődleges névtér vannak csak érkező üzenetek.

2.  Egy adott várólista küldött üzenetek vagy témakör előfordulhat, hogy mire eljut az üzenetem megfelelő sorrendben.

3.  Ha az alkalmazás munkamenetek, munkamenet belüli üzenetek előfordulhat, hogy kapják meg üzeneteiket megfelelő sorrendben. Ez a munkamenetek rendes működését szünetet. Ez azt jelenti, hogy az alkalmazás használja-e a munkamenetek logikailag csoportüzenetekre. Az elsődleges névtér a munkamenet-állapot csak karbantartása.

4.  Egy munkamenetből üzenetek előfordulhat, hogy mire eljut az üzenetem sorrendje. Ez a munkamenetek rendes működését szünetet. Ez azt jelenti, hogy az alkalmazás használja-e a munkamenetek logikailag csoportüzenetekre.

5.  Az elsődleges névtér a munkamenet-állapot csak karbantartása.

6.  Az elsődleges várólista is online állapotba helyezni, és indítsa el az üzenetek fogadására, mielőtt a másodlagos várakozási sorban található összes üzenetet továbbít elsődleges várakozási.

Az alábbi szakaszokból megtudhatja, hogy az API-k, hogyan az API-k végrehajtását és példakódot jeleníti meg a szolgáltatását használja. Figyelje meg, hogy nincsenek-e ez a funkció társított számlázási következmények.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>A MessagingFactory.PairNamespaceAsync API

A páronkénti névtér funkció a [PairNamespaceAsync][] módszer a [Microsoft.ServiceBus.Messaging.MessagingFactory][] osztály tartalmazza:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

A feladat befejezésekor a névtér párosítás is teljes, és készen áll a bármely [MessageReceiver][], [QueueClient][], az jár el vagy [TopicClient][] [MessagingFactory][] példányával létrehozott. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] alap eredetű a különböző típusú párosításhoz elérhető [MessagingFactory][] objektummal. Az egyetlen származtatott osztály jelenleg egy névvel ellátott [SendAvailabilityPairedNamespaceOptions][], amely végrehajtja a Küldés rendelkezésre állási követelmények. [SendAvailabilityPairedNamespaceOptions][] konstruktorokat, amelyek egymással épülnek rendelkezik. Megjeleníti a legtöbb paraméterrel konstruktorának, a többi konstruktorok viselkedését megértéséhez.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Ezek a paraméterek jelentése a következő:

-   *secondaryNamespaceManager*: a másodlagos névtér, amely a [PairNamespaceAsync][] módszer segítségével állíthatja be a másodlagos névtér inicializált [NamespaceManager][] példány. A névtér kezelő használatos beszerzése a névtér sorban várakozó listáját, és győződjön meg arról, hogy a szükséges tartalék sorokat. Ha ezek a sorok nem létezik, a rendszer létrehozza őket. [NamespaceManager][] jogkivonat **kezelése** kérelem lehetősége van szükség.

-   *messagingFactory*: a másodlagos névtér az [MessagingFactory][] -példányt. A [MessagingFactory][] objektum küldeni, és a [EnableSyphon][] tulajdonság értéke **Igaz**, ha a hibaüzenetek közül tartalék használják.

-   *backlogQueueCount*: létrehozása tartalék sorok számát. Ezt az értéket kell legalább 1. Amikor üzeneteket küld a tartalék, végre a következő sorban várakozó véletlenszerűen kiválasztott. Ha az érték 1, majd csak egy várólista minden eddiginél használható. Amikor ez történik, és a egy tartalék várakozási sorban található hibákat hoz létre, az ügyfél nem tud próbálkozzon egy másik tartalék várólista és meghiúsulhat küldi el az üzenetet. Azt javasoljuk, hogy az érték beállítása néhány nagyobb értéket, és az alapértelmezett 10 értéket. Attól függően, hogy mennyi adatot a kérelem küld napi nagyobb vagy kisebb értéket módosíthatja. Minden tartalék sor legfeljebb 5 GB üzenetek tárolhat.

-   *failoverInterval*: ameddig, akkor fogad el az elsődleges névtér hibák előtt bármely egyetlen entitás Átkapcsolás a másodlagos névtér az idő mennyiségét. A szervezet-entitás kombinálásával feladatátvétel. A szolgáltatás Bus különböző található csomópontok gyakran élő egyetlen névteret szervezetek. Egy személy hibába nem jelenti azt egy másik hibát. Beállíthatja, hogy ezt az értéket a [System.TimeSpan.Zero][] szeretne áttérni a másodlagos közvetlenül azután, hogy az első, nem tranziens hiba. Az feladatátvevő óra elindításának hibák bármely [MessagingException][] , amelyben a [IsTransient][] tulajdonság értéke hamis vagy egy [System.TimeoutException][]. Más kivételeket, például [UnauthorizedAccessException][] ne okozzanak feladatátvételt, mert azt jelzik, hogy az ügyfél nem megfelelően van konfigurálva. Egy [ServerBusyException][] ne okozzon feladatátvevő, mert a megfelelő mintája, hogy csak 10 másodperc, majd küldje el az üzenetet újra.

-   *enableSyphon*: jelzi, hogy a adott párosítás kell is syphon a másodlagos névtér vissza az elsődleges névtér az üzeneteit. Az általános küldésére alkalmazások kell állítsa ezt az értéket **hamis**; üzenetek fogadására alkalmazások kell állítsa ezt az értéket **Igaz**. Ennek oka gyakran nincsenek az üzenetek feladóihoz-nál kevesebb üzenetet fogadó. Attól függően, hogy a vevők számát megadhatja, hogy egy egyetlen alkalmazásból példány Szifonos feladatok kezelésére. Sok címzett használatával vetít számlázási minden tartalék sor.

A kód használatához hozzon létre egy elsődleges [MessagingFactory][] -példányt, egy másodlagos [MessagingFactory][] -példányt, egy másodlagos [NamespaceManager][] példány és egy [SendAvailabilityPairedNamespaceOptions][] példány. Lehet, hogy a hívás egyszerűen, a következőket:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Ha befejezte a feladatot a [PairNamespaceAsync][] módszer által visszaadott, minden van állítva, és használatra kész. Mielőtt a feladatot ad vissza, előfordulhat, hogy nem befejezése összes háttér szükséges munka a párosításhoz működnek megfelelően. Eredményt adja akkor nem indult el üzenetek küldésére, amíg a tevékenység adja eredményül. Ha bármilyen hiba történt, például a hibás hitelesítő adatokat, vagy nem lehet védőfalat létrehozni a tartalék sorokat a kivételek fog kell elő, ha a tevékenység befejeződött. A tevékenység ad vissza, ha győződjön meg arról, hogy a sorok sem talált a [BacklogQueueCount][] tulajdonság a [SendAvailabilityPairedNamespaceOptions][] példányon vizsgálata által létrehozott. Az előző kód, a művelet a következőképpen jelenik meg:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Következő lépések

Most, hogy megtanulta aszinkron üzenetküldési szolgáltatás Bus alapjait, olvassa el a [páronkénti névtér][]olvashat bővebben.

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Gyakorlati tanácsok a szigetelő alkalmazások szolgáltatás Bus kimaradások és katasztrófák ellen]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [párosított névtér]: service-bus-paired-namespaces.md