<properties
   pageTitle="Első lépések a megbízható szolgáltatások |} Microsoft Azure"
   description="Bevezetés a Microsoft Azure Service háló alkalmazások létrehozása az állapot nélküli és állapot-nyilvántartó szolgáltatásaival."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Megbízható szolgáltatások – első lépések

> [AZURE.SELECTOR]
- [C# Windows rendszeren](service-fabric-reliable-services-quick-start.md)
- [Java Linux](service-fabric-reliable-services-quick-start-java.md)

Ez a cikk ismerteti, Azure szolgáltatás háló megbízható szolgáltatások alapjait, és részletes információt tartalmaz hozhat létre és üzembe helyezése egyszerű megbízható szolgáltatásalkalmazás Java nyelven írt keresztül.

## <a name="installation-and-setup"></a>Telepítés és beállítás
Mielőtt nekikezdene, győződjön meg arról, hogy a szolgáltatás háló fejlesztői környezet beállítása a számítógépre.
Ha állítsa be van szüksége, [első lépések a Mac](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md)megnyitásához.

## <a name="basic-concepts"></a>Alapfogalmak
Első lépések a megbízható szolgáltatásokat, csak szüksége lehet áttekinteni néhány alapfogalmak:

 - **Szolgáltatás típusa**: Ez a szolgáltatás Önnél. Azt határozza meg, írja be az osztály nyúló `StatelessService` és más kódot vagy használja ott, nevét és a verziószám függőségek.

 - **Elnevezett szolgáltatás példányának**: a szolgáltatás futtatásához hoz létre elnevezett példányát a szolgáltatás típusa sokkal hoz létre az a osztály típusú objektum példányok, mint. Szolgáltatás-példányok valójában a kézzel írt osztály objektum példányának. 

 - **Szolgáltatás host**: A hoz létre elnevezett szolgáltatás példányainak kell a szolgáltató belül futnak. A szolgáltatás állomás csak egy folyamat, ahol a szolgáltatás példányát futtatását is lehetővé teszi.

 - **Szolgáltatás regisztráció**: regisztráció mindent egy helyen megtalálhatja. A szolgáltatás típusa regisztrálva futtatásához engedélyezni szolgáltatás háló létrehozása példányát, akkor a szolgáltatás fogadó szolgáltatás háló futtatókörnyezet kell lennie.  

## <a name="create-a-stateless-service"></a>Hozzon létre egy állapot nélküli szolgáltatás

Az új szolgáltatás háló alkalmazás létrehozásával kell kezdenie. A szolgáltatás háló SDK Linux tartalmaz egy Yeoman nyilvántartás-készítő alkalmazás az állapot nélküli szolgáltatást szolgáltatás háló alkalmazáshoz állványon megadását. Indítsa el a következő Yeoman futtatásával parancsot:

```bash
$ yo azuresfjava
```

Kövesse az utasításokat követve hozzon létre egy **Megbízható állapot nélküli szolgáltatást**. Ebben az oktatóprogramban szerinti nevet az alkalmazás "HelloWorldApplication" és az "HelloWorld" szolgáltatást. Az eredményt fogja tartalmazni könyvtárak a `HelloWorldApplication` és `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>A szolgáltatás megvalósítása

Nyissa meg a **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Az osztály a szolgáltatás típusa határozza meg, és futtatását is lehetővé teszi, hogy minden kódot. A szolgáltatás API biztosít a kód két belépési pontról:

 - Egy bejegyzés nyílt pont módszert nevű `runAsync()`, ahol külön előkészületek nélkül használhatja tetszőleges-munkaterhelésekből, például hosszabb ideig futó számítási munkaterhelésekből végrehajtása.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - A kapcsolati belépési pontjához csatlakoztatható, ahol a kapcsolati egymást fedő megválasztott. Ez a hol kezdődhet kéréseket fogad a felhasználók és más szolgáltatásokhoz.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Ebben az oktatóanyagban azt összpontosít a `runAsync()` könyvviteli pontot. Ez a, ahol azonnal elkezdheti a kód futtatását.

### <a name="runasync"></a>RunAsync

A platform felhívja ezt a módszert, egy példányának szolgáltatás elhelyezett, és készen áll a végrehajtása esetén. A Megnyitás/Bezárás ciklus szolgáltatás példány akkor fordulhat elő, a szolgáltatás élettartama során sokszor egészének mérete. Ez akkor fordulhat elő, különféle okok, például:

- A rendszer a szolgáltatás példányainak terheléselosztás erőforrás lép.
- Hibák a kód fordul elő.
- Az alkalmazás- vagy frissíteni.
- Az alapul szolgáló hardver-üzemszünetek találkozik.

Ez az üzletifolyamat-tervező kezelik szolgáltatás háló tartani a szolgáltatást, könnyen hozzáférhető és megfelelően kiegyensúlyozott szerint.

#### <a name="cancellation"></a>A lemondás

Fontos, hogy a kód `runAsync()` leállíthatja a végrehajtás, amikor szolgáltatás háló értesítést kap. A `CompletableFuture` – által eredményül adott `runAsync()` megszakad, ha szolgáltatás háló van szüksége a szolgáltatás leállítása végrehajtása. A következő példa bemutatja, hogyan kezelheti a lemondás esemény: 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Regisztráció szolgáltatás

Szolgáltatás-típusok regisztrálva a szolgáltatás háló futtatókörnyezet kell lennie. A szolgáltatás típusa könyvjelzőnév a `ServiceManifest.xml` és a szolgáltatás osztály, amely `StatelessService`. A folyamat fő belépési pontjához szolgáltatás regisztrációs történik. Ebben a példában a folyamat fő belépési pontjához van `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Futtassa az alkalmazást

A Yeoman állványon tartalmaz egy gradle parancsfájlt, az alkalmazás összeállítása és üzembe parancsfájlok és az ENSZ bash – az alkalmazás telepítéséhez. Futtassa az alkalmazást, először összeállítása gradle az alkalmazást:

```bash
$ gradle
```

Ez a szolgáltatás háló alkalmazáscsomag, amely használ szolgáltatási háló Azure CLI telepíthető hoznak létre. A install.sh parancsfájl parancsok a szükséges Azure CLI alkalmazáscsomag telepítéséhez. Egyszerűen futtassa a install.sh telepítése:

```bask
$ ./install.sh
```
