<properties
   pageTitle="Első lépések a szolgáltatás háló megbízható szereplők |} Microsoft Azure"
   description="Ebben az oktatóanyagban végigvezeti a létrehozás, hibakeresési és használ szolgáltatási háló megbízható szereplők egyszerű szereplő-alapú szolgáltatás üzembe helyezése."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Megbízható szereplők – első lépések

> [AZURE.SELECTOR]
- [C# Windows rendszeren](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Ez a cikk ismerteti, Azure Service háló megbízható szereplők alapjait, és részletes információt tartalmaz hozhat létre, hibakeresési és üzembe helyezné a Visual Studióban egyszerű megbízható szereplő alkalmazása keresztül.

## <a name="installation-and-setup"></a>Telepítés és beállítás
Mielőtt nekikezdene, győződjön meg arról, hogy a szolgáltatás háló fejlesztői környezet beállítása a számítógépre.
Ha szeretne állította be, [hogy miként állíthatja be a fejlesztői környezet](service-fabric-get-started.md)részletes útmutatást találhat.

## <a name="basic-concepts"></a>Alapfogalmak
Első lépések a megbízható szereplők, csak szüksége lehet áttekinteni néhány alapfogalmak:

 * **Szereplő szolgáltatás**. A szolgáltatás háló infrastruktúra telepített megbízható szolgáltatások megbízható szereplők csomagolása. Szereplő példányok aktiválva vannak, egy névvel ellátott szolgáltatás példány.
 
 * **Regisztráció szereplő**. Megbízható szolgáltatásokkal megbízható szereplő szolgáltatás kell a Service háló futtatókörnyezet regisztrálva. Ezenkívül a szereplő típus kell a szereplő futtatókörnyezet regisztrálva.
 
 * **Szereplő felület**. A szereplő felület erősen beírt nyilvános felhasználói felülete egy szereplő definiálása szolgál. A megbízható szereplő modell terminológia a szereplő felülettel, amely a szereplő érthető üzenetek és a folyamat típusú határozza meg. A szereplő felület "" (aszinkron) üzeneteket küldeni a szereplő más szereplők és ügyfélalkalmazásokban használják. Megbízható szereplők is végrehajtja a több felületek.
 
 * **ActorProxy osztály**. A ActorProxy osztály a szereplő felületen elérhetővé tett módszerek meghívásához ügyfélalkalmazások használják. A ActorProxy osztály két fontos funkciókat kínál:
    * Névfeloldás: képes keresse meg a szereplő a fürt (Keresés a csomópontra a jelenlegi fürt).
    * Hiba kezelésének: módszer meghívásához újra és újra megoldani a szereplő helyre, például a szereplő való helyezhetők át egy másik csomópontra a fürt igénylő hiba után.

Az alábbi szabályokat, amelyek szereplő felületek vonatkoznak megemlítése érdemes a következők:

- Túlterhelt nem szereplő felület módszereket.
- Módszerek a meg nem rendelkezhet szereplő felület, HIV, vagy választható paraméterek.
- Általános felületek nem támogatottak.

## <a name="create-a-new-project-in-visual-studio"></a>A Visual Studióban új projekt létrehozása
A frissítés telepítése után a szolgáltatás háló tools for Visual Studio, létrehozhat új projekttípusok. Az új projekttípusok vannak a **felhőben** kategóriában, az **Új projekt** párbeszédpanel.


![Szolgáltatás háló tools for Visual Studio - új projekt][1]

A következő párbeszédpanelen válassza ki a létrehozni kívánt projekt típusú.

![Szolgáltatás háló projektsablonok][5]

A HelloWorld projekthez a szolgáltatás háló megbízható szereplők szolgáltatás használata.

Miután létrehozta a megoldást, meg kell jelennie a következő szerkezet:

![Szolgáltatás háló projekt szerkezetének][2]

## <a name="reliable-actors-basic-building-blocks"></a>Megbízható szereplők egyszerű építőelemek

Egy tipikus megbízható szereplők megoldás három projektet tevődik össze:

* **Az alkalmazás a project (MyActorApplication)**. Ez az a projektet, amely az összes telepítés együtt szolgáltatások csomagok. Az alkalmazás kezelésére szolgáló a *ApplicationManifest.xml* és PowerShell parancsfájlok tartalmazza.

* **A felület-projektet (MyActor.Interfaces)**. Ez az a projektet, amely tartalmazza a szereplő felület definícióját. A MyActor.Interfaces projekt meghatározhatja a megoldást a szereplők által használt kapcsolódási. A szereplő felületek definiálható minden projekt bármely nevét, azonban a felület határozza meg a szereplő végrehajtása által megosztott szereplő szerződést, és az ügyfelek meghívása a szereplő, így általában célszerű határozhat meg egy külön szereplő végrehajtása, és több projekt megosztható összeállítás.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **A szereplő szolgáltatás project (MyActor)**. A projekt használta a szolgáltatás háló szolgáltatás, amely a szereplő tárolni fog az. A szereplő végrehajtásának tartalmaz. Egy szereplő végrehajtása egy eredetű, amely az alap típusból `Actor` és a MyActor.Interfaces projekt definiált valósít. Egy szereplő osztály is kell alkalmazhat, hogy elfogadja konstruktorban egy `ActorService` példány és egy `ActorId` átadja őket a következő `Actor` osztály. Ez a platform függőségek konstruktor függőség utasítások beszúrását tesz lehetővé.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

A szolgáltatás háló Runtime szolgáltatás típusú regisztrálni kell a szereplő szolgáltatást. Ahhoz, hogy a szereplő példánya futtatható a szereplő szolgáltatás a szereplő típusa is regisztrálni kell a szereplő szolgáltatással. A `ActorRuntime` regisztrációs módszer a munka szereplők hajt végre.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Ha, nyissa meg az új projektet a Visual Studióban, és csak egy szereplő definíció van, a program a kódot, amely a Visual Studio generál alapértelmezés szerint látható a regisztráció. Ha más szereplők a szolgáltatásban, kell a szereplő regisztráció felvétele segítségével:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] A szolgáltatás háló szereplők futtatókörnyezet bocsát ki néhány [események és a teljesítmény számláló kapcsolódó szereplő módszereket](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). A diagnosztikai és teljesítményét figyelve hasznosak.


## <a name="debugging"></a>Hibakeresése során

A szolgáltatás háló tools for Visual Studio támogatja a helyi számítógépre hibakeresése során. Egy hibakeresési folyamat elindíthatja szerezze meg az F5 billentyűt. Visual Studio épít (ha szükséges) csomagok. Az alkalmazás a helyi szolgáltatás háló fürt üzembe helyezése és a hibakereső.

A telepítés során megjelenik a **kimeneti** ablakban végrehajtását.

![Szolgáltatás háló hibakeresési kimeneti ablakban][3]


## <a name="next-steps"></a>Következő lépések
 - [A szolgáltatás háló platform megbízható szereplők használata](service-fabric-reliable-actors-platform.md)
 - [Szereplő állam kezelése](service-fabric-reliable-actors-state-management.md)
 - [Szereplő életciklusa és szemét gyűjtemény](service-fabric-reliable-actors-lifecycle.md)
 - [Szereplő API hivatkozási dokumentáció](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Minta kódot.](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
