Az alábbi táblázat szolgáltatás Bus üzenetküldés kvóta információkat. Esemény hubok korlátai szerepelnek az alábbi táblázatban, de esemény hubok részletesebb információt, olvassa el a [Esemény hubok árak](https://azure.microsoft.com/pricing/details/event-hubs/)című témakört. Árak információt és a többi szolgáltatás Bus kvóták a a [Szolgáltatás Bus árak](https://azure.microsoft.com/pricing/details/service-bus/) áttekintése című témakörben találhat.

|Kvóta neve|Hatókör|Típus|Vezérlő viselkedése, amikor túllépve|Érték|
|---|---|---|---|---|
| Azure előfizetésenként névtér maximális száma|Namespace|Statikus|További névtér későbbi kérelmek a portálon elutasításra kerül.|100|
|A várakozási/témakör mérete|Személy|Definiált várólista témakör létrehozás után.|Bejövő üzenetek elutasításra kerül, és kivételt fogadja a hívó kóddal.|1, 2, 3, 4 vagy 5 GB.<br /><br />Ha a [szétválasztás](service-bus-partitioning.md) engedélyezve van, a maximális várólista/témakör mérete 80 GB.|
|A névtér egyidejű kapcsolatok száma|Namespace|Statikus|Ezután valaki további kapcsolatok elutasításra kerül, és kivételt fogadja a hívó kóddal. További műveletek nem beleszámítanak egyidejű TCP-kapcsolatot.|NetMessaging: 1000<br /><br />AMQP: 5000|
|A várakozási/témakör/előfizetés entitás egyidejű kapcsolatok száma|Személy|Statikus|Ezután valaki további kapcsolatok elutasításra kerül, és kivételt fogadja a hívó kóddal. További műveletek nem beleszámítanak egyidejű TCP-kapcsolatot.|A környezettől per névtér egyidejű kapcsolatok határa.|
|Egyidejű száma egy várólista/témakör/előfizetés entitás kérelem érkezik|Személy|Statikus|További fogadása kérelmek elutasításra kerül, és a kivételt fogadja a hívó kóddal. Ez a kvóta vonatkozik, a kombinált egyidejű száma fogadási műveletek témához kapcsolódó összes előfizetésekben.|5000|
|A témakörök és sorok szolgáltatás névtere / szám|Rendszerbeli|Statikus|Ezután valaki egy új témakör vagy a szolgáltatás névtere sor elrejtésével elutasításra kerül. Emiatt az [Azure portálon][]keresztül konfigurált, ha hibaüzenet jön létre. Ha hív kezelésének API, kivételt megkapta a hívó kódot.|10 000<br /><br />Témakörök, valamint a szolgáltatás névteret sorok száma legfeljebb 10 000 kell lennie.<br/>Ez nem alkalmazható prémium, az összes entitás particionálva vannak.|
|Szolgáltatás névtere per particionált témakörök/sorok száma|Rendszerbeli|Statikus|Ezután valaki egy új particionált témakör vagy a szolgáltatás névtere sor elrejtésével elutasításra kerül. Emiatt az [Azure portálon][]keresztül konfigurált, ha hibaüzenet jön létre. A felügyeleti API hívását, ha egy **QuotaExceededException** kivételt megkapta a hívó kódot.|Egyszerű és a normál rétegek - 100<br />Prémium - 1000<br/><br />Minden particionált várólista vagy a témakör a függvény összeszámolja 10 000 szervezetek névtér használati kvóta felé.|
|Bármely üzenetben entitás elérési maximális mérete: várólista vagy témakör|Személy|Statikus|-|260 karakter|
|Bármely üzenetben entitás neve maximális mérete: névtér, előfizetés, előfizetés szabály vagy esemény központi|Személy|Statikus|-|50 karakter|
|Esemény hubok esemény maximális mérete|Rendszerbeli|Statikus|-|256 KB|
|Üzenet mérete a várakozási/témakör/előfizetés entitás|Rendszerbeli|Statikus|Bejövő üzeneteket, amelyekre az alábbi kvóták elutasításra kerül, és a kivétel fogadja a hívó kóddal|Üzenetek maximális mérete: ([Normál réteg](../articles/service-bus/service-bus-premium-messaging.md)) 256KB és 1 MB ([prémium réteg](../articles/service-bus/service-bus-premium-messaging.md)). <br /><br />**Megjegyzés:** Rendszer általános, hogy ezt a korlátot az általában kicsit kisebb.<br /><br />Az élőfej maximális méret: 64KB<br /><br />Élőfej tulajdonságok tulajdonság között a maximális száma: **bájt/int MaxValue**<br /><br />Tulajdonság tulajdonság között a maximális mérete: nincs explicit korlát. Az élőfej maximális méret szerint korlátozott.|
|A várakozási/témakör/előfizetés entitás üzenetméret tulajdonság|Rendszerbeli|Statikus|Egy **SerializationException** kivétel jön létre.|Üzenetek maximális tulajdonság minden tulajdonság mérete 32K. Az összes tulajdonságok összesített mérete nem haladhatja meg a 64K. Ez a cikk a [BrokeredMessage](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.aspx), amelynek mindkét teljes fejlécének felhasználó tulajdonságainak, valamint a rendszer tulajdonságai (például [– sorszám –](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.sequencenumber.aspx), [felirat](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.label.aspx), [MessageId](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)és stb.).|
|A témakör egy előfizetések száma|Rendszerbeli|Statikus|Ezután valaki hozhat létre a témakörben további előfizetések elutasításra kerül. Emiatt a portálon keresztül konfigurált, ha hibaüzenet jelenik meg. Ha az adatkezelési API hívását kivételt megkapta a hívó kódot.|2000|
|SQL-szűrők / témakör száma|Rendszerbeli|Statikus|A további szűrők című témakör létrehozása későbbi kérelmek elutasításra kerül, és kivételt megkapja a hívó kódot az.|2000|
|A témakör egy korrelációs szűrők száma|Rendszerbeli|Statikus|A további szűrők című témakör létrehozása későbbi kérelmek elutasításra kerül, és kivételt megkapja a hívó kódot az.|100 000|
|SQL-szűrők/műveletek mérete|Rendszerbeli|Statikus|A további szűrők létrehozása későbbi kérelmek elutasításra kerül, és kivételt fogadja a hívó kóddal.|Szűrési feltétel karakterlánc maximális hossza: 1024 (1-K).<br /><br />Szabály művelet szöveg maximális hossza: 1024 (1-K).<br /><br />Egy szabály művelet kifejezések maximális száma: 32.|
|Névtér, várólista vagy témakör eső [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) szabályok száma|Egységek, névtere|Statikus|További szabályok létrehozásához további kérések elutasításra kerül, és a kivételt fogadja a hívó kóddal.|Szabályok maximális száma: 12. <br /><br /> Szabályok szolgáltatás Bus névteret konfigurált összes sorban várakozó és az adott névtér témakörök vonatkoznak.

[Azure portál]: https://portal.azure.com