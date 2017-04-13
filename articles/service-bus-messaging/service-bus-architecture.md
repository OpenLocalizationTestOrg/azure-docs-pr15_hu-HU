<properties 
    pageTitle="Szolgáltatás Bus architektúra |} Microsoft Azure"
    description="Az üzenet és a továbbítási Azure Service Bus architektúráját feldolgozása ismerteti."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Szolgáltatás Bus architektúra

Ez a cikk ismerteti az üzenetet, illetve a továbbítás Azure Service Bus architektúráját feldolgozása.

## <a name="service-bus-scale-units"></a>Szolgáltatás Bus skála mennyiség

Szolgáltatás Bus *skála mennyiség*szerint vannak rendezve. Egy időosztás egységet telepítési időegység és tartalmazza az összes szükséges összetevő elindul, és a szolgáltatás. Egyes régiókra egy vagy több szolgáltatás Bus skála egységek üzembe helyezése.

A szolgáltatás Bus névtér van rendelve egy időosztás egységet. Az időosztás egységet kezeli a szolgáltatási Bus szervezetek diagramtípusokat: jelfogók és brokered üzenetben személyek (sorban várakozó, témák, az előfizetések). A szolgáltatás Bus időosztás egységet az alábbi összetevőket áll:

- **Átjáró csomópontok csoportja.** Átjáró csomópontok hitelesítést végezni, bejövő felkérést, és a továbbítási kérések kezelésére. Az egyes átjáró csomópontok egy nyilvános IP-címmel rendelkezik.

- **Ügynök csomópontok üzenetküldés csoportja.** Üzenetek ügynök csomópontok összehívások szóló üzenetben szervezetek.

- **Egy átjáró pontra.** Az átjáró tároló minden személy e időosztás egységet definiált adatait tartalmazza. Az átjáró tároló SQL Azure-adatbázis fölött áll rendelkezésre.

- **Több üzenetben tárolja.** Üzenetek tárolja az üzeneteket az összes sorban várakozó, téma és az e időosztás egységet definiált előfizetések tartsa. Az összes előfizetés adatokat is tartalmaz. Kivéve, ha engedélyezve van a [particionált üzenetküldés szervezetek](service-bus-partitioning.md) , a várólista vagy témát van hozzárendelve egy üzenetben áruházból. Az előfizetések tárolja a szülő-témakör azonos üzenetben tárolva. Szolgáltatás Bus [Prémium üzenetküldés](service-bus-premium-messaging.md), kivéve az üzenetben áruházak fölött SQL Azure-adatbázisokhoz szerepelni fog.

## <a name="containers"></a>Tárolók

Minden egyes üzenetek entitás adott tároló van-e hozzárendelve. A tároló egy logikai szerkezetet, amely pontosan egy üzenetben tároló való tároló összes vonatkozó adatokat tároló használja. Minden egyes tároló van hozzárendelve egy üzenetben ügynök csomópontot. A szokásos üzenetküldés ügynök csomópontok-nél több tárolók vannak. Ennélfogva minden üzenetben ügynök csomópont betölti több tárolók. Tárolók egy üzenetben ügynök csomópontra eloszlását, hogy az összes üzenetben ügynök csomópontok egyaránt töltődnek be vannak rendezve. A betöltés mintázat változásokat (például a tárolók keresése nagyon elfoglalt egyike), vagy ha egy üzenetben ügynök csomópont válik átmenetileg nem érhető el, ha a tárolók vannak osztja el az üzenetben ügynök csomópontok.

## <a name="processing-of-incoming-messaging-requests"></a>Bejövő üzenetek kérelmek feldolgozása

Amikor egy ügyfél szolgáltatás Bus kérelem küld, a Azure terheléselosztó irányítja valamelyik, az átjáró csomópontot. Az átjáró csomópont engedélyezi a kérést. A kérés érint egy üzenetben entitás (várólista, témakör, előfizetést), ha az átjáró csomópontot a szervezet az átjáró áruházban keres, és határozza meg, hogy mely üzenetek tárolása a szervezet található. Ez majd keres melyik üzenetben ügynök csomópont jelenleg szolgál ez a tároló, és a kérést küld az adott üzenetben ügynök csomópontot. Az üzenetben ügynök csomópontot a kérést, és frissíti a tároló áruházban entitás állapotát. Az üzenetben ügynök csomópont ezután a választ az átjáró csomópontra, amelynek vissza az ügyfél, amely az eredeti kérés ki a megfelelő választ küld üzenetet küld.

![Bejövő üzenetek kérelmek feldolgozása](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Bejövő továbbítási kérelmek feldolgozása

Amikor egy ügyfél szolgáltatás Bus kérelem küld, a Azure terheléselosztó irányítja valamelyik, az átjáró csomópontot. Ha a kérelem listening kérést, a az átjáró csomópontot egy új továbbítás hoz létre. Ha egy adott továbbítási csatlakozási kérelem a kérést, az átjáró csomópontot a az átjáró csomópontra, amely a továbbítási tulajdonosa a csatlakozási kérelem továbbítja. Az átjáró csomópontot, amely a továbbítási tulajdonosa szinkronizálása kérést küld a listening ügyfél, a figyelő az átjáró csomópontra a csatlakozási kérelem fogadott ideiglenes csatorna létrehozásához.

Ha a továbbítási kapcsolat jön létre, az ügyfelek üzenetváltásra képes a átjáró csomópontot, amellyel a szinkronizálása keresztül.

![Bejövő továbbítási kérelmek feldolgozása](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Következő lépések

Most, hogy az, hogy elolvasta szolgáltatás Bus architektúra kezdéshez áttekintése kövesse az alábbi hivatkozásokat:

- [Szolgáltatás Bus üzenetküldés – áttekintés](service-bus-messaging-overview.md)
- [Szolgáltatás Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
- [Aszinkron üzenetben megoldást használ szolgáltatási Bus sorban várakozó](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
