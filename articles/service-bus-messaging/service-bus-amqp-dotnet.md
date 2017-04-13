<properties 
    pageTitle="A .NET és AMQP 1.0 Bus szolgáltatás |} Microsoft Azure"
    description="A .NET szolgáltatás Bus használata AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>A .NET szolgáltatás Bus használata AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>A szolgáltatás Bus SDK letöltése

A szolgáltatás Bus SDK 2.1-es vagy újabb verzió AMQP 1.0 támogatás érhető el. Biztosíthatja, hogy a szolgáltatás Bus bittel letöltésével [NuGet][]által legújabb verziójával rendelkezik.

## <a name="configuring-net-applications-to-use-amqp-10"></a>.NET-alkalmazások használata AMQP 1.0 beállítása

Alapértelmezés szerint a szolgáltatás Bus .NET ügyfél tárat a szolgáltatás Bus szolgáltatás egy SOAP-alapú dedikált protokoll használatával kommunikál. AMQP 1.0 használata helyett alapértelmezés szerint a protokoll a szolgáltatás Bus kapcsolati karakterláncot, a közvetlen konfiguráció szükséges a a következő szakaszban leírt módon. Ez a módosítás nem alkalmazás kódja változatlan marad AMQP 1.0 használata esetén.

Az aktuális programcsomag létezik néhány API-szolgáltatások AMQP használata esetén nem támogatott. Nem támogatott funkciókról később jelennek meg a szakasz [nem támogatott funkciókat, korlátozások, és viselkedési különbségek](#unsupported-features-restrictions-and-behavioral-differences). A speciális konfigurációs beállítások egy részét is kell egy másik AMQP használata esetén.

### <a name="configuration-using-appconfig"></a>Konfigurációs App.config használatával

Tanácsos alkalmazások beállítások tárolni a App.config konfigurációs fájl használatával. Szolgáltatás Bus alkalmazások App.config tárolja a szolgáltatás Bus **ConnectionString** érték beállításokat használhatja. Egy példa App.config fájl az alábbi képlettel történik:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Értékét a `Microsoft.ServiceBus.ConnectionString` értéke a szolgáltatás Bus kapcsolati karakterlánc, amely állítsa be a szolgáltatás Bus kapcsolatot. Formátuma az alábbi képlettel történik:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Ha `[namespace]` és `SharedAccessKey` az [Azure portál][]kapott. További tudnivalókért olvassa el a [szolgáltatás Bus sorban várakozó – első lépések][]című témakört.

AMQP használatakor hozzáfűzése a kapcsolati karakterlánc a `;TransportType=Amqp`. Ez a jelölés arról tájékoztat, hogy az ügyfél tár AMQP 1.0 használ szolgáltatási Bus végezze el a kapcsolatot.

## <a name="message-serialization"></a>Üzenet szerializálási

Az alapértelmezett protokoll használata esetén az alapértelmezett szerializálási működés a .NET-ügyfél tár környezetbe [DataContractSerializer][] típusa szerializálni egy átviteli az ügyfél tár és a szolgáltatás Bus szolgáltatás közötti [BrokeredMessage][] -példányt. AMQP átviteli mód használatakor az ügyfél tár AMQP típus rendszert használ az [üzenet brokered][BrokeredMessage] AMQP üzenetbe szerializáláshoz. A szerializálási lehetővé teszi, hogy a kapott, és egy másik platformon, például egy Java-alkalmazást a JMS API használ szolgáltatási Bus eléréséhez esetleg futó fogadó alkalmazás értelmezi az üzenetet.

Amikor egy [BrokeredMessage][] példányt hoz létre a konstruktor az üzenet törzsében szolgáló paraméterként egy .NET-objektum adhat meg. Egyszerű típusokhoz AMQP csatolható objektumokat a szervezet szerializálni AMQP adattípusok. Ha az objektum nem közvetlenül képezhető le egy egyszerű AMQP típus; be Ez azt jelenti, hogy egy egyéni típusa határozza meg az alkalmazást, majd az objektum szerializált a [DataContractSerializer][]és a szerializált bájtok küldjük AMQP adatok üzenetben.

A .NET ügyfelek együttműködést elősegítő használata csak a használhatók, közvetlenül az üzenet törzsében AMQP típusait, amelyekről .NET típusokat. Az alábbi táblázat adott típusú és a AMQP típus rendszer a megfelelő hozzárendelés adatai.

| .NET-szervezet objektumtípus          | Megfeleltetett AMQP típusa                          | AMQP szervezet szakasz típusa                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| logikai                           | logikai érték                                   | AMQP érték                                                                                                                                                |
| bájt                           | ubyte                                     | AMQP érték                                                                                                                                                |
| ushort                         | ushort                                    | AMQP érték                                                                                                                                                |
| uint                           | uint                                      | AMQP érték                                                                                                                                                |
| ulong                          | ulong                                     | AMQP érték                                                                                                                                                |
| sbyte                          | bájt                                      | AMQP érték                                                                                                                                                |
| rövid                          | rövid                                     | AMQP érték                                                                                                                                                |
| int                            | int                                       | AMQP érték                                                                                                                                                |
| hosszú                           | hosszú                                      | AMQP érték                                                                                                                                                |
| Lebegtetés                          | Lebegtetés                                     | AMQP érték                                                                                                                                                |
| dupla                         | dupla                                    | AMQP érték                                                                                                                                                |
| decimális                        | decimal128                                | AMQP érték                                                                                                                                                |
| karakter                           | karakter                                      | AMQP érték                                                                                                                                                |
| Dátum és idő                       | időbélyeg                                 | AMQP érték                                                                                                                                                |
| Globálisan egyedi azonosítója                           | UUID                                      | AMQP érték                                                                                                                                                |
| byte]                         | bináris                                    | AMQP érték                                                                                                                                                |
| karakterlánc                         | karakterlánc                                    | AMQP érték                                                                                                                                                |
| System.Collections.IList       | lista                                      | AMQP érték: a gyűjteményben lévő elemeket csak lehet azokat az e táblázatban beállított.                                                             |
| System.Array                   | tömb                                     | AMQP érték: a gyűjteményben lévő elemeket csak lehet azokat az e táblázatban beállított.                                                             |
| System.Collections.IDictionary | térkép                                       | AMQP érték: a gyűjteményben lévő elemeket csak lehet azokat az e táblázatban beállított. Megjegyzés: csak a karakterlánc billentyűk használhatók.                        |
| URI                            | Karakterlánc leírt (lásd az alábbi táblázatban) | AMQP érték                                                                                                                                                |
| DateTimeOffset                 | Hosszú leírt (lásd az alábbi táblázatban)   | AMQP érték                                                                                                                                                |
| Időszak                       | Hosszú leírt (lásd az alábbi)         | AMQP érték                                                                                                                                                |
| Adatfolyam                         | bináris                                    | AMQP adatok (több is lehet). Az adatok szakaszok a nyers bájtok, olvassa el a adatfolyam objektumból tartalmazzák.                                                           |
| Más objektumokhoz                   | bináris                                    | AMQP adatok (több is lehet). Az objektum a DataContractSerializer vagy adja meg az alkalmazás szerializálót használó szerializált bináris tartalmazza. |

| .NET-típus      | Megfeleltetett AMQP leírt típusa                                                                                              | Jegyzetek                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Időszak       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nem támogatott szolgáltatások, a korlátozás és viselkedési különbségek

A szolgáltatás Bus .NET API az alábbi funkciók jelenleg nem támogatottak AMQP használata esetén:

-   Tranzakciók

-   Átadás cél küldése

Vannak még kis eltérések a szolgáltatás Bus .NET API viselkedését a AMQP, és összehasonlítása az alapértelmezett protokoll használata esetén:

-   Az [így időtúllépés történt][] tulajdonságot a rendszer figyelmen kívül hagyja.

-   `MessageReceiver.Receive(TimeSpan.Zero)`mint alkalmazása `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Üzenetek befejezése zárolása tokenek által csak az üzenetet fogadó kezdetben Beérkezett üzenetek szerint végezheti el.

## <a name="controlling-amqp-protocol-settings"></a>AMQP protokoll beállításainak szabályozására

A .NET API-k elérhetővé tenni a AMQP protokoll viselkedésének szabályozásához számos beállítás:

-   **MessageReceiver.PrefetchCount**: a kezdeti hitel hivatkozás alkalmazott szabályozza. Az alapértelmezett érték 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: AMQP keret maximális mérete a kapcsolat egyeztetése során felajánlott vezérlők nyissa meg az időt. Az alapértelmezett érték 65 536 bájt.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Ha átvitelek batchable, ezt az értéket határozza meg, hogy a legnagyobb késleltetést dispositions küldésének. Alapértelmezés szerint feladók/vevők örökli. Egyes feladó és címzett felülbírálhatják az alapértelmezett, amelyet 20 ezredmásodperc.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: szabályozza, hogy AMQP kapcsolatot létesít SSL-kapcsolaton keresztül. Az alapértelmezett érték **Igaz**.

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el az alábbi hivatkozások:

- [Szolgáltatás Bus AMQP – áttekintés]
- [Szolgáltatás Bus particionálva sorban várakozó és témakörök AMQP 1.0 támogatása]
- [A Windows Server szolgáltatás Bus AMQP]

  [Első lépések a szolgáltatás Bus sorban várakozó]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [Így időtúllépés történt]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portál]: https://portal.azure.com
[Szolgáltatás Bus AMQP – áttekintés]: service-bus-amqp-overview.md
[Szolgáltatás Bus particionálva sorban várakozó és témakörök AMQP 1.0 támogatása]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[A Windows Server szolgáltatás Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
