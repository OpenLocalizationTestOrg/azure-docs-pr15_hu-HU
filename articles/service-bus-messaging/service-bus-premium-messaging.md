<properties
    pageTitle="Szolgáltatás Bus prémium verzió és az szokásos rétegek áttekintése árak |} Microsoft Azure"
    description="Szolgáltatás Bus prémium verzió és az szokásos"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Szolgáltatás Bus Premium és a szokásos üzenetben rétegek 

Üzenetküldési szolgáltatás Bus, amely magában foglalja üzenetküldés például sorban várakozó személyek és témaköreit, üzenetküldés rich lehetőségek kombinálásával vállalati közzététele-előfizetés a felhőben skála szemantikáját. Szolgáltatás Bus üzenetküldés szolgál a kapcsolati gerinchálózathoz sok bonyolult felhő megoldások.

A szolgáltatás Bus üzenetküldés a *prémium* réteg címek közös felhasználói kérések skála, a teljesítmény és a kulcsfontosságú alkalmazások elérhetősége körül. Habár a szolgáltatás beállítása majdnem azonos, szolgáltatás Bus üzenetküldés az alábbi két rétegek készült különböző használati eset szolgálnak.

Magas szintű eltérések a program kiemeli az alábbi táblázatban.

| Támogatás                               | Normál                       |
|---------------------------------------|--------------------------------|
| Nagy teljesítmény                       | Változó átviteli            |
| Kiszámíthatóan teljesítmény               | Változó időtartama               |
| Kiszámíthatóan árak                   | Díj folyamatos rendezését változó árak |
| Ha át kívánja méretezni mezőjében a terhelést lehetősége | A #HIÁNYZIK                            |
| Üzenet mérete > 256KB                  | Üzenet mérete 256KB          |

Processzor- és rétegben erőforrás elkülönítési **Szolgáltatás Bus prémium üzenetküldés** biztosít, hogy az egyes ügyfelek terhelést fut elkülönítési. Ez az erőforrás-tároló *egység üzenetküldés*neve. Minden egyes prémium névtér felosztása legalább egy üzenetben egység. 1, 2 vagy 4 üzenetben egységek minden szolgáltatás Bus prémium névtér vásárolhat. Egy egyetlen terhelést vagy a szervezet több üzenetben egységek is kiterjedhet, és az üzenetben egységek számát bármikor módosíthatók lesz, bár számlázási 24 órás vagy napi ráta díjak. A szolgáltatás Bus-alapú megoldás kiszámíthatóvá és megismételhető teljesítmény eredménye.

Nemcsak a teljesítmény kiszámítható, és érhető el, de azt is gyorsabb. A tárhely motor [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/)bevezetett épül üzenetküldési szolgáltatás Bus prémium verzióban. Prémium csevegést, a csúcs teljesítmény sokkal gyorsabb, mint a szokásos réteg a.

## <a name="premium-messaging-technical-differences"></a>Prémium üzenetküldés műszaki eltérések

Az alábbiakban néhány eltérések Premium és a szokásos üzenetben rétegek.

### <a name="partitioned-queues-and-topics"></a>Particionált sorban várakozó és témakörök

Az üzenetküldés prémium particionált sorban várakozó és témakörök által támogatott, de nem fog működni szolgáltatás Bus üzenetküldés ahogy a szokásos és Basic rétegek megegyező módon. Prémium üzenetküldés mely nem használja az SQL egy adatokat tároló, és már nem rendelkezik a lehetséges erőforrás verseny társított megosztott platformot. Emiatt szétválasztás már nem szükséges. Ezenkívül partíciót számának prémium 2 partíciók szabványos küldése a 16 partíciókból megváltozott. Két partíciót problémákat biztosítja az elérhetőség, és a prémium futtatókörnyezet környezetben megfelelőbb szám. Szétválasztás kapcsolatos további tudnivalókért olvassa el a [Partitioned sorban várakozó és témakörök](service-bus-partitioning.md)című témakört.

### <a name="express-entities"></a>Express-szervezetek

Egy teljesen elszigetelt futtatókörnyezet prémium üzenetküldés fut, mert kifejezett szervezetek nem támogatottak a prémium névtér. A sürgős szolgáltatással kapcsolatos további tudnivalókért lásd: a [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) tulajdonságot.

## <a name="next-steps"></a>Következő lépések

További információk a szolgáltatás Bus üzenetben, a következő témakörökben olvashat:

- [Azure Service Bus prémium (blogbejegyzés) üzenetküldés bemutatása](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Azure Service Bus prémium (Channel9) üzenetküldés bemutatása](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Szolgáltatás Bus üzenetküldés – áttekintés](service-bus-messaging-overview.md)
- [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)
