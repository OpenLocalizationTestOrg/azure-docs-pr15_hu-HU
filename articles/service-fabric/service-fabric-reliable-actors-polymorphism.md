<properties
   pageTitle="A megbízható szereplők keretrendszer polimorfizmus |} Microsoft Azure"
   description="Hozzon létre hierarchiák .NET felületek és típusú megbízható szereplők keretében funkciók és az API-definíciók újra felhasználhatja."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>A megbízható szereplők keretrendszer polimorfizmus

A megbízható szereplők keretrendszer használatával objektumorientált Tervező használó szeretné ugyanazokat a technikákat számos szereplők összeállítása teszi lehetővé. Ezeket a módszereket egyik polimorfizmus, amely lehetővé teszi, hogy a típus, és további örökli felületek szülők általános. Öröklődés megbízható szereplők keretében általában követi a .NET modell néhány további korlátozásokkal.

## <a name="interfaces"></a>Kapcsolatok

A megbízható szereplők keretrendszer legalább egy felületet végre kell hajtania a szereplő típusának megadása kéri. Ez a felület használatával kommunikál a szereplők ügyfelek használható proxy osztály készítése szolgál. Kapcsolatok más felületek is öröklik mindaddig, amíg minden kívánt szereplő: által végrehajtott felület és az összes a szülők végül származtatása IActor. IActor az platform definiált alap kezelőfelület szereplők. Ezért a klasszikus polimorfizmus példa alakzatokkal előfordulhat, hogy valami hasonló látható:

![Az alakzat szereplők felület hierarchia][shapes-interface-hierarchy]


## <a name="types"></a>Típusai

Is létrehozhat hierarchiát szereplő típusú, amely az alap szereplő osztály a platform által biztosított származik. Alakzatok, ha lehet, hogy egy számrendszerben `Shape` típusát:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

A altípushoz `Shape` felülbírálhatja a következő módszerek.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Megjegyzés: a `ActorService` szereplő típusú attribútum. Az attribútum közli, hogy meg kell automatikusan szolgáltatást hozhat létre az ilyen típusú szereplők szolgáltatója megbízható szereplő keretében. Egyes esetekben előfordulhat, hogy kívánt hoz létre alap, amely kizárólag szánt funkciók megosztását altípus közül, és a program soha nem használta konkrét szereplők elindítását. Ezekben az esetekben kell használnia a `abstract` jelzi, hogy soha ne hozzon létre egy adott típusú alapján szereplő kulcsszó.


## <a name="next-steps"></a>Következő lépések

- Lásd: [hogyan megbízható szereplők keretében használja-e a szolgáltatás háló platform](service-fabric-reliable-actors-platform.md) megbízhatóságot, méretezhetőség a és egységes megadását.
- Tudjon meg többet a [szereplő életciklus](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
