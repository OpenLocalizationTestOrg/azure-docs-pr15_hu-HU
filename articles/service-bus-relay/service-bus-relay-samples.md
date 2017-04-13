<properties 
    pageTitle="Szolgáltatás Bus továbbítási minták áttekintése |} Microsoft Azure"
    description="Kategorizálja és szolgáltatás Bus továbbítási minták mutató hivatkozásokat tartalmazó ismerteti."
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
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-samples"></a>Szolgáltatás Bus továbbítási minták

A szolgáltatás Bus továbbítási minták bemutatják a [szolgáltatás Bus továbbítási](https://azure.microsoft.com/services/service-bus/)főbb szolgáltatásait. Ez a cikk kategorizálására és a minták érhető el, amely hivatkozásokat tartalmaz az egyes ismerteti.

>[AZURE.NOTE] Szolgáltatás Bus minták nincs telepítve vannak a SDK csomagjában talál. A minták letöltéséhez látogasson el az [Azure SDK minták lapjára](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Ezenkívül van egy szolgáltatás Bus továbbítási minták frissített csoportját [Itt](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (kezdve az írás, azok is nem jelen cikkben ismertetett).  

Üzenetek minták [szolgáltatás Bus üzenetküldés minták](../service-bus-messaging/service-bus-samples.md)című témakör tartalmaz.

## <a name="service-bus-relay"></a>Szolgáltatás Bus továbbító

Az alábbi példák bemutatják, hogyan írhat, így a szolgáltatás Bus továbbítási szolgáltatáshoz.

Figyelje meg, hogy a továbbítási minták egy kapcsolati karakterláncot, a szolgáltatás Bus névtér eléréséhez szükséges.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Kapcsolati karakterlánc Azure Service Bus beszerzése

1. Jelentkezzen be az [Azure-portálon](http://portal.azure.com).

1. A bal oldali oszlopban kattintson a **Szolgáltatás Bus**.

1. Kattintson a nevére a listában a névtér.

3. A névtér lap kattintson a **megosztott access-házirendek**elemre.

4. Kattintson a **megosztott access-házirendek** lap **RootManageSharedAccessKey**.

6. Másolja a vágólapra a kapcsolati karakterlánc.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>A Windows Server szolgáltatás Bus beszerzése a kapcsolati karakterlánc

1. Futtassa az alábbi PowerShell-parancsmagot:

    ```
    get-sbClientConfiguration
    ```

2. A kapcsolati karakterlánc illessze be a minta a App.config fájlt.

## <a name="service-bus-relay"></a>Szolgáltatás Bus továbbító

A minták, amelyek bemutatják a szolgáltatás Bus továbbítási.

### <a name="getting-started"></a>Első lépések

|Minta neve|Leírás|Minimális SDK verziója|Elérhetőség|
|---|---|---|---|
|[Üzenetkezelés továbbítását: Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Bemutatja, hogyan Azure Service Bus ügyfélszámítógép és a szolgáltatás futtatásához. Ez a példa szolgáltatás Bus programozás útján állítja be. Csak a környezet és a biztonsági információk konfigurációs fájl vannak tárolva.|1.8|Microsoft Azure Service Bus|
|[Üzenetben hitelesítés továbbítását: A megosztott titkos](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Bemutatja, hogyan lehet egy kibocsátó nevet és a kibocsátó titkos használja a szolgáltatás Bus hitelesítést végezni.|1.8|Microsoft Azure Service Bus|
|[Üzenetek hitelesítési továbbítását: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Bemutatja, hogyan kattintva jelenítse meg az ügyfél felhasználóhitelesítés nem igénylő HTTP szolgáltatásainak.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Bemutatja, hogyan kell használni a **WebHttpRelayBinding** kötés bináris adatok használata a webes programozási modell.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: NetTcp továbbítását](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Bemutatja, hogyan kell használni a **NetTcpRelayBinding** kötés.|1.8|Microsoft Azure Service Bus|

### <a name="exploring-features"></a>Funkcióinak felfedezése

A minták, amelyek bemutatják a különféle szolgáltatás Bus továbbítási funkciókat.

|Minta neve|Leírás|Minimális SDK verziója|Elérhetőség|
|---|---|---|---|
|[Üzenetben hitelesítés továbbítását: Egyszerű WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Bemutatja, hogyan lehet egy egyszerű webes jogkivonat hitelesítő adatok használata a szolgáltatás Bus hitelesítést végezni. A minta hasonlít a visszhang mintát, néhány módosítást. Kifejezetten az alábbi példa összeadja a működés az ServiceHost (szolgáltatás) és a ChannelFactory (ügyfél) alkalmazások.|1.8|Microsoft Azure Service Bus|
|[Üzenetküldés továbbítását: Terheléselosztási](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus több vevők üzeneteknek. Azt mutatja, hogy egy egyszerű szolgáltatás kommunikáció a **NetTcpRelayBinding** kötés keresztül egy ügyfél több példányban|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: Nettó esemény](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Bemutatja, hogy a **NetEventRelayBinding** kötés segítségével a Microsoft Azure Service Bus.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: WS2007Http munkamenethez](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Bemutatja, hogy megbízható-munkamenetek engedélyezve van a **WS2007HttpRelayBinding** kötés használata. Azt is megtudhatja, hogy miként szolgáltatás Bus hitelesítő adatok megadása a konfigurációs fájl programozás útján helyett.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Bemutatja, hogyan lehet a **WS2007HttpRelayBinding** kötés használata üzenet biztonsági biztonságossá tétele végpont üzenetek továbbra is az ügyfelek szolgáltatás Bus hitelesíteni igénylő közben.|1.8|Microsoft Azure Service Bus|
|[Üzenetküldés továbbítását: Metaadatcsere](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Bemutatja, hogyan kattintva jelenítse meg a metaadat-alapú végpont, amely a továbbítási kötés használja. A következő továbbítási kötések **metaadatcserét** támogatott: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**és **WS2007HttpRelayBinding**.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: NetTcp közvetlen](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Bemutatja, hogyan támogatja a **hibrid** csatlakozási mód, amely először egy relayed kapcsolatot létesít, és ha lehetséges, automatikusan átvált közvetlen ügyfél és a szolgáltatás közötti kapcsolat **NetTcpRelayBinding** kötése konfigurálása.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: NetTcp MsgSec felhasználónév](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Bemutatja, hogyan kell használni a **NetTcpRelayBinding** kötés az üzenet biztonsági.|1.8|Microsoft Azure Service Bus|
|[Üzenetben kötések továbbítását: Nettó Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Bemutatja, hogyan jelenítik meg, és felhasználása a szolgáltatás végpontjának a **NetOnewayRelayBinding** kötés segítségével.|1.8|Microsoft Azure Service Bus|
|[Üzenetek kötések továbbítását: WS2007Http egyszerű](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Bemutatja, hogy a **WS2007HttpRelayBinding** kötés segítségével. Bemutatja, hogy az olyan egyszerű szolgáltatás, amely nincs biztonsági beállításokat használ, és nem igénylő ügyfelek hitelesítést végezni.|1.8|Microsoft Azure Service Bus|

## <a name="next-steps"></a>Következő lépések

Az alábbi témakörökben szolgáltatás Bus elvi áttekintésével.

- [Szolgáltatás Bus továbbítási – áttekintés](service-bus-relay-overview.md)
- [Szolgáltatás Bus architektúra](../service-bus-messaging/service-bus-architecture.md)
- [Szolgáltatás Bus alapjai](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)