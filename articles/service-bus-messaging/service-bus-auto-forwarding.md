<properties 
    pageTitle="Automatikus továbbítási szervezetek üzenetküldési szolgáltatás Bus |} Microsoft Azure"
    description="Lánc várólista vagy-előfizetésre másik várólista vagy a témakör bemutatja, hogyan."
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
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Az automatikus továbbítási láncolási szolgáltatás Bus személyek

Az *automatikus-továbbítás* funkció lehetővé teszi a várólista vagy -előfizetésre másik várólista vagy ugyanazt a névteret részét képező témakör lánc. Automatikus továbbítási engedélyezve van, a szolgáltatás Bus automatikusan jelennek meg, amelyek az első várólista vagy -előfizetésre (forrás) kerülnek eltávolítja, és a helyezi őket a második várólista vagy témakör (cél). Figyelje meg, hogy az üzenet küldése a cél személyhez közvetlenül is lehetséges. Ezenkívül még nem lánc alvárólisták, például egy deadletter várólista másik várólista vagy témakört is.

## <a name="using-auto-forwarding"></a>Automatikus továbbítási használatával

Automatikus továbbítási engedélyezheti a [QueueDescription][] [SubscriptionDescription][] objektumok és a forrás, ahogy a következő példában a [QueueDescription.ForwardTo][] vagy [SubscriptionDescription.ForwardTo][] tulajdonságok megadásával.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

A cél entitás léteznie kell, a forrás személy létrehozásakor. Ha a célként megadott egységek nem létezik, a szolgáltatás Bus kivétel, amikor a rendszer kéri a forrás személy létrehozása adja eredményül.

Ha egy adott témakör automatikus továbbítási is használhatja. Szolgáltatás Bus korlátozza a 2000 [adott témában előfizetések száma](service-bus-quotas.md) . További előfizetéseket rendezhetők a másodlagos szintű témakörök létrehozásával. Figyelje meg, hogy akkor is, ha Ön nem köti szolgáltatás Bus korlátozása előfizetések számú, a második szintű témakörök hozzáadása a teljes átvitel javítható a témakör.

![Automatikus továbbítási eset][0]

Automatikus-átirányítás segítségével az üzenetek feladóihoz a vevők decouple. Például érdemes megvizsgálni egy ERP rendszer, amely három modulok áll: a feldolgozási sorrendben Készletkezelés és vevő kapcsolatok kezelése. Az egyes modulok hoz létre, amelyek a megfelelő témakörbe sorbaállítva üzeneteket. Anna és Péter üzletkötőként, amely érdekli, az összes üzenetre, amelyet az ügyfeleknek vonatkoznak. Azokat az üzeneteket fogadni, Anna és Péter minden hozzon létre egy személyes várólista és előfizetés minden ERP témakört, amely a várakozási sorban található összes üzenetek automatikus továbbítása a.

![Automatikus továbbítási eset][1]

Anna állapotba szabadság, saját személyes várólista helyett a ERP témakört, ha tölti. Ebben az esetben egy értékesítési képviselő nem kapta meg az esetleges üzeneteket, mert a ERP témakörök közül egyik sem minden eddiginél elérje a kvóta.

## <a name="auto-forwarding-considerations"></a>Automatikus-átirányítás előtt megfontolandó kérdések

Ha a célként megadott egységek halmozott sok üzenetet, és a kvóta meghaladja, vagy a cél entitás le van tiltva, a forrás személy hozzáadja az üzeneteket a [kézbesítetlen](service-bus-dead-letter-queues.md) mindaddig, amíg a nincs a rendeltetési hely (vagy a szervezet újraengedélyezheti). Azokat az üzeneteket, külön kell fogadása, és feldolgozni azokat a halottlevél az élő halottlevél, továbbra is.

Láncolás közös sok előfizetést az összetett témakör beszerzése témakörök tartalmaznak, ajánlott, hogy rendelkezik-e mérsékelt számos különböző előfizetések első szintű című témakör és a másodlagos szintű témakörök a sok előfizetést. Például 20 előfizetések, mindegyiket módszere során 200 előfizetések, a másodlagos szintű-témakörben az első szintű témakör lehetővé teszi, hogy az első szintű témakör 200 előfizetésekkel-nál magasabb átviteli minden második szintű-témakörben az 20 előfizetések módszere során.

Szolgáltatás Bus egy műveletet minden továbbított üzenet váltók stb. Például az üzenet elküldése az mindegyiket automatikus továbbítás üzenetek témakört, vagy másik várólista konfigurált 20 előfizetések témakörben terheli 21 műveletek, ha minden első szintű előfizetés az üzenet a másolatot kap.

Szeretne létrehozni, amely a másik várólista vagy témakör módszere van során előfizetést, az előfizetés létrehozója kell rendelkeznie engedélyek **kezelése** a forrás- és a cél entitás. A forrás témakör **küldése** engedélyeinek csak üzeneteket küld a forrás témakör szükséges.

## <a name="next-steps"></a>Következő lépések

Automatikus továbbítási részletes információt az alábbi témakörökben hivatkozás:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Szolgáltatás Bus teljesítménybeli fejlesztések kapcsolatos további információért olvassa el a [Partitioned üzenetküldés szervezetek][]című témakört.

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Particionált üzenetben személyek]: service-bus-partitioning.md