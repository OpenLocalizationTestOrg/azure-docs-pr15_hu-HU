<properties
   pageTitle="Megbízható szereplők események |} Microsoft Azure"
   description="A szolgáltatás háló megbízható szereplők események bemutatása."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Szereplő események
Szereplő események a szereplő legjobb mindent értesítést küld az ügyfelek ad lehetőséget. Szereplő események szereplő-ügyfél kapcsolati készültek, és nem használható szereplő-szereplő kommunikációra.

A következő kódtöredék megjelenítése szereplő események az alkalmazás használatáról.

Adja meg az események a szereplő által közzétett leíró felületet. A kapcsolat kell származnia a `IActorEvents` felület. Módszerek az argumentumokat kell lennie az [adatok szerződés szerializálható](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). A módszerek vissza kell térnie érvénytelenítése, eseményként értesítések egyirányú és ajánlott munkamennyiség.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Az események szereplő felületén a szereplő által közzétett az elemeket rekordként.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Ügyféloldali az eseménykezelő végrehajtása.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Az ügyfél a proxy a szereplő tesz közzé, az esemény létrehozása, és annak események előfizetés.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Feladatátadás, megelőzve a szereplő meghiúsulhat, fölé egy másik folyamat vagy csomópontot. A szereplő proxy az aktív előfizetések kezelése és automatikusan újra előfizetője őket. Az újbóli előfizetés intervallum keresztül szabályozhatja, hogy a `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API-val. Leiratkozás, használja a `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API-val.

Kattintson a szereplő egyszerűen a közzététel az események, amikor megtörténnek. Ha az esemény előfizetőinek, a szereplők futtatókörnyezet küld őket az értesítésre.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Következő lépések
 - [Szereplő rögzítve](service-fabric-reliable-actors-reentrancy.md)
 - [Szereplő diagnosztikai és a teljesítmény figyelése](service-fabric-reliable-actors-diagnostics.md)
 - [Szereplő API hivatkozási dokumentáció](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Minta kódot.](https://github.com/Azure/servicefabric-samples)
