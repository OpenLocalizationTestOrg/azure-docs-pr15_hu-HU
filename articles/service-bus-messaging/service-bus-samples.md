<properties 
    pageTitle="Áttekintés szolgáltatás Bus üzenetküldés minták |} Microsoft Azure"
    description="Kategorizálja, és a minták, amely hivatkozásokat tartalmaz az egyes üzenetküldési szolgáltatás Bus ismerteti."
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

# <a name="service-bus-messaging-samples"></a>Szolgáltatás Bus üzenetben minták

A szolgáltatás Bus üzenetben minták bemutatják a [szolgáltatás Bus üzenetküldés](https://azure.microsoft.com/services/service-bus/) (felhőalapú szolgáltatás) és a [Windows Server szolgáltatás Bus](https://msdn.microsoft.com/library/dn282144.aspx)főbb szolgáltatásait. Ez a cikk kategorizálására és a minták érhető el, amely hivatkozásokat tartalmaz az egyes ismerteti.

>[AZURE.NOTE] Szolgáltatás Bus minták nincs telepítve vannak a SDK csomagjában talál. A minták letöltéséhez látogasson el az [Azure SDK minták lapjára](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Ezenkívül van egy minták üzenetküldési szolgáltatás Bus frissített halmazára [Itt](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (kezdve az írás, azok is nem jelen cikkben ismertetett).  

Továbbítás minták [szolgáltatás Bus továbbítási minták](../service-bus-relay/service-bus-relay-samples.md)című témakör tartalmaz.

## <a name="service-bus-messaging"></a>Szolgáltatás Bus üzenetküldés

Az alábbi példák bemutatják, hogyan írhat szolgáltatás Bus üzenetküldés használó alkalmazások.

Figyelje meg, hogy a üzenetben minták egy kapcsolati karakterláncot, a szolgáltatás Bus névtér eléréséhez szükséges.

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

### <a name="getting-started-samples"></a>Első lépések minták

Ezek a minták egyszerű üzenetben jellemző.

|Minta neve|Leírás|Minimális SDK verziója|Elérhetőség|
|---|---|---|---|
|[Első lépések: Sorban várakozó üzenetküldés](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus segítségével üzenetek küldése és fogadása a sorból.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Első lépések: Üzenetkezelés témakörök](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Bemutatja, hogyan küldhet és fogadhat üzeneteket több előfizetéssel a témakör a Microsoft Azure Service Bus használatával.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|

### <a name="exploring-features"></a>Funkcióinak felfedezése

Az alábbi példák bemutatják, szolgáltatás Bus különféle szolgáltatásait.

|Minta neve|Leírás|Minimális SDK verziója|Elérhetőség|
|---|---|---|---|
|[HTTP-Token szolgáltatók](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Bemutatja a HTTP/többi ügyfél szolgáltatás Bus hitelesítése, milyen különböző módokon.|2.1|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Szolgáltatás Bus HTTP-ügyfél](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Bemutatja, hogyan üzenetek üzenetek küldése és fogadása a HTTP/többi keresztül szolgáltatás Bus.|2.3.|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Szolgáltatás Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Bemutatja, hogyan üzenetek automatikus továbbítása várólista, előfizetés vagy deadletter várólista áthelyezése egy másik várólista vagy témakört. Azt is bemutatja, hogyan üzenet küldése egy várólista vagy egy átviteli sorba e témakör.|2.3.|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: WCF-csatorna munkamenet minta](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus Windows Communication Foundation (WCF) a csatornák használata. A minta WCF csatornák keresztül szolgáltatás Bus várólista üzenetek küldése és fogadása a használatát mutatja be. A minta a szolgáltatás Bus munkamenet és a kommunikáció a munkamenet-ábrázolja.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Üzenetkezelés brokered: tranzakciók](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Bemutatja, hogyan lekötött kötegenként műveletek üzenetküldés biztosítása érdekében a Microsoft Azure Service Bus üzenetküldési szolgáltatások tranzakció hatókör használandó atomically.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Adatkezelési műveletek többi használatával](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Bemutatja, hogyan lehet többi használ szolgáltatási Bus adatkezelési műveletek hajthatók végre.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Erőforrás-szolgáltató REST API-hoz](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Bemutatja, hogyan kell használni az új szolgáltatás Bus RDFE REST API-k névtér és üzenetben szervezetek kezelése.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: WCF-munkamenet minta](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus WCF szolgáltatás modellt használja. A minta a WCF szolgáltatás modell kommunikáció munkamenet-alapú szolgáltatás Bus várólista elvégezheti a használatát mutatja be.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Kérés válasz](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus és a kérés/válasz funkciót. A minta egyszerű ügyfelek és a kiszolgálón keresztül egy szolgáltatás Bus várólista kommunikáció mutatja.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: halottlevél](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus és az üzenetek "halottlevél" funkciókat. A minta egy egyszerű feladó és a kommunikáció szolgáltatás Bus várólista keresztül címzett látható.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Üzenetek határolni.](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus üzenet halasztás szolgáltatásával. A minta egy egyszerű feladó és a kommunikáció szolgáltatás Bus várólista keresztül címzett látható.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Munkamenet üzenetek](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus és a munkamenet üzenetküldés funkciókat. A minta látható egyszerű küldő és kommunikáció szolgáltatás Bus várólista keresztül.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Kérés válasz témakör](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus témakörök és előfizetések kérés/válasz minta végrehajtásához. A minta egyszerű ügyfelek és a kommunikáció a szolgáltatás Bus tematikus keresztül kiszolgálók mutatja.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Kérés válasz várólista](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus és a kérés/válasz funkciót. A minta egyszerű ügyfelek és a két szolgáltatás Bus várólista keresztüli kommunikáció kiszolgálók mutatja.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Ismétlődő észlelési](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Bemutatja, hogyan használhatja a Microsoft Azure Service Bus ismétlődő üzenet észlelése az sorban várakozó. Két sorban várakozó egy engedélyezve van a duplikált elemek észlelése és más egy ismétlődő észlelése nem hoz létre.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Aszinkron üzenetküldés](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus segítségével üzenetek küldése és fogadása aszinkron a sorból. A várakozási sorban található biztosít a feladó és címzett tetszőleges számú leválasztott, aszinkron kommunikációját (itt egy egyetlen címzett).|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Speciális szűrők](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Bemutatja, hogyan kell használni a Microsoft Azure Service Bus közzététel/előfizetés speciális szűrők. Azt hoz létre a témakör és a 3 előfizetések különböző szűrő definíciókat, üzeneteket küld a témakör és előfizetések összes üzenetet kap.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Brokered üzenetküldés: Az üzenetek előzetes lehívása](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Bemutatja, hogyan lehet a Microsoft Azure Service Bus üzenetek előzetes lehívása funkcióval. Bemutatja, hogy az fogadási után az üzenetek előzetes lehívása funkció használatáról.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|

## <a name="service-bus-reference-tools"></a>Szolgáltatás Bus hivatkozás eszközök

Az alábbi példák bemutatják, a szolgáltatás különböző más szolgáltatásokat.

|Minta neve|Leírás|Minimális SDK verziója|Elérhetőség|
|---|---|---|---|
|[Szolgáltatás Bus Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|A szolgáltatás Bus Intéző lehetővé teszi, hogy a szolgáltatás Bus szolgáltatás névteret kapcsolódni, és üzenetben szervezetek kezelésére egyszerűen módon. Az eszköz speciális szolgáltatások, például az importálás/exportálás funkciók és az azt jelenti, hogy tesztelje üzenetben személyek és a továbbítási szolgáltatások biztosít.|1.8|Microsoft Azure Service Bus; A Windows Server szolgáltatás Bus|
|[Engedélyezés: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Ez a példa bemutatja, hogyan hozhat létre és kezelhet szolgáltatás identitások a Microsoft Azure Active Directory hozzáférés-vezérlés (más néven vezérlő szolgáltatást vagy ACS) szolgáltatás Bus való használatra.|A #HIÁNYZIK|Microsoft Azure Service Bus|

## <a name="next-steps"></a>Következő lépések

Az alábbi témakörökben szolgáltatás Bus elvi áttekintésével.

- [Szolgáltatás Bus üzenetküldés – áttekintés](service-bus-messaging-overview.md)
- [Szolgáltatás Bus architektúra](service-bus-architecture.md)
- [Szolgáltatás Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
