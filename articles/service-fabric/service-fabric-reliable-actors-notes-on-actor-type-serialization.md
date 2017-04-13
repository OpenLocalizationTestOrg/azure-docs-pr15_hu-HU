<properties
   pageTitle="Megbízható szereplők jegyzetek szereplő írja be a szerializációs |} Microsoft Azure"
   description="Azt ismerteti, hogy a szolgáltatás háló megbízható szereplők államok és felületek megadására használható szerializálható osztályok definiálása alapvető követelményei"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>A szolgáltatás háló megbízható szereplők jegyzetek írja be a szerializációs


Az argumentumok módszerek, a feladatok mindegyik módszernek az szereplő felületet kapott, és egy szereplő állam Manager tárolt objektumok találattípusok [Adatok szerződés szerializálható](https://msdn.microsoft.com/library/ms731923.aspx)kell lennie. Ez a meghatározott [szereplő esemény felületek](service-fabric-reliable-actors-events.md#actor-events)módszerek az argumentumokat is vonatkozik. (Szereplő esemény felület módszerek mindig vissza érvénytelenítése.)

## <a name="custom-data-types"></a>Egyéni adattípusok

Ebben a példában a következő szereplő kapcsolat határozza meg, amely visszaadja a nevű egyéni adattípus módszer `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

A felület impelemented által egy szereplő, az állapot Manager használó tárolására egy `VoicemailBox` objektum:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

Ebben a példában a `VoicemailBox` objektum szerializált esetén:
 - Az objektum küldeni egy szereplő példány és egy hívó között.
 - Az objektum az állapot kezelőjét, amennyiben lemezre állandó és más csomópontok replikált menti a program.
 
A megbízható szereplő keretrendszer DataContract szerializálási használja. Emiatt a egyéni data objects és a tagjaik fel kell vezetni **DataContract** és **DataMember** attribútumok, a kurzor

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Következő lépések
 - [Szereplő életciklusa és szemét gyűjtemény](service-fabric-reliable-actors-lifecycle.md)
 - [Szereplő időzítő és emlékeztetők](service-fabric-reliable-actors-timers-reminders.md)
 - [Szereplő események](service-fabric-reliable-actors-events.md)
 - [Szereplő rögzítve](service-fabric-reliable-actors-reentrancy.md)
 - [Szereplő polimorfizmus és objektumorientált tervezés mintázatok](service-fabric-reliable-actors-polymorphism.md)
 - [Szereplő diagnosztikai és a teljesítmény figyelése](service-fabric-reliable-actors-diagnostics.md)
