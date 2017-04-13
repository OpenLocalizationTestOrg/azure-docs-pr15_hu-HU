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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Megbízható szereplők – első lépések

> [AZURE.SELECTOR]
- [C# Windows rendszeren](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Ez a cikk ismerteti, Azure Service háló megbízható szereplők alapjait, és végigvezeti Önt létrehozását, és egy egyszerű megbízható szereplő Java-alkalmazás telepítése.

## <a name="installation-and-setup"></a>Telepítés és beállítás
Mielőtt nekikezdene, győződjön meg arról, hogy a szolgáltatás háló fejlesztői környezet beállítása a számítógépre.
Ha állítsa be van szüksége, [első lépések a Mac](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md)megnyitásához.

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

## <a name="create-an-actor-service"></a>Hozzon létre egy szereplő szolgáltatás
Az új szolgáltatás háló alkalmazás létrehozásával kell kezdenie. A szolgáltatás háló SDK Linux tartalmaz egy Yeoman nyilvántartás-készítő alkalmazás az állapot nélküli szolgáltatást szolgáltatás háló alkalmazáshoz állványon megadását. Indítsa el a következő Yeoman futtatásával parancsot:

```bash
$ yo azuresfjava
```

Kövesse az utasításokat követve hozzon létre egy **Megbízható szereplő szolgáltatás**. Ebben az oktatóprogramban szerinti nevet az alkalmazás "HelloWorldActorApplication" és a szereplő "HelloWorldActor." A következő állványon jönnek létre:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Megbízható szereplők egyszerű építőelemek

Az alapfogalmak ismertetett lefordítása az egyszerű építőelemek egy megbízható szereplő szolgáltatás.

### <a name="actor-interface"></a>Szereplő felület

A felület definíció szereplő tartalmaz. A kapcsolat a szereplő szerződést, és az ügyfelek, a szokásos adjon meg egy helyen szereplő végrehajtása különböző értelme, és több más szolgáltatások vagy ügyfélalkalmazásokban megosztható, hívja fel a szereplő, szereplő végrehajtásához által megosztott határozza meg.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Szereplő szolgáltatás 
Ez a szereplő végrehajtása és a szereplő regisztráció programkódjának tartalmaz. A szereplő osztály a szereplő felület hajtja végre. Ez a, ahol a szereplő végzi a saját munka.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Szereplő regisztráció

A szolgáltatás háló Runtime szolgáltatás típusú regisztrálni kell a szereplő szolgáltatást. Ahhoz, hogy a szereplő példánya futtatható a szereplő szolgáltatás a szereplő típusa is regisztrálni kell a szereplő szolgáltatással. A `ActorRuntime` regisztrációs módszer a munka szereplők hajt végre.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Próba-ügyfél

Ez az egyszerű próba ügyfélalkalmazás futtatva külön-külön a szereplő szolgáltatás tesztelése szolgáltatás háló alkalmazásból. Íme egy példa, ahol a ActorProxy aktiválásához és a kommunikáció a szereplő példányok használható. Nem üzemelnek a szolgáltatásával.

### <a name="the-application"></a>Az alkalmazás 

Az alkalmazás csomagok végül a szereplő szolgáltatást, és előfordulhat, hogy felvette a jövőben közös telepítéshez más szolgáltatásokat. A *ApplicationManifest.xml* és a hely tulajdonosai a szereplő szolgáltatás csomag tartalmazza.

## <a name="run-the-application"></a>Futtassa az alkalmazást

A Yeoman állványon tartalmaz egy gradle parancsfájlt, az alkalmazás összeállítása és üzembe parancsfájlok és az ENSZ bash – az alkalmazás telepítéséhez. Futtassa az alkalmazást, először összeállítása gradle az alkalmazást:

```bash
$ gradle
```

Ez a szolgáltatás háló alkalmazáscsomag, amely használ szolgáltatási háló Azure CLI telepíthető hoznak létre. A install.sh parancsfájl parancsok a szükséges Azure CLI alkalmazáscsomag telepítéséhez. Egyszerűen futtassa a install.sh telepítése:

```bask
$ ./install.sh
```
