<properties
   pageTitle="Helyi meghajtóra figyelése és Azure Service háló írása szolgáltatások diagnosztizálása |} Microsoft Azure"
   description="Megtudhatja, hogy miként figyelheti, és a szolgáltatások használata a Microsoft Azure Service háló helyi fejlesztési gépre írt diagnosztizálása."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Figyelése és a szolgáltatások egy helyi számítógép zónában fejlesztési beállítása diagnosztizálása


> [AZURE.SELECTOR]
- [A Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Figyelés, észlelését, diagnosztizálása és hibaelhárítási engedélyezése minimális állásidőt azzal, hogy a felhasználói felület folytatásához szolgáltatásokhoz. Figyelés és diagnosztika egy tényleges telepített üzemi környezetben kritikus. Egy hasonló modell elfogadása szolgáltatások fejlesztés során biztosítja, hogy a diagnosztikai folyamat munkakörnyezetben áthelyezésekor működik-e. Szolgáltatás háló egyszerűen zökkenőmentesen dolgozhat át egyetlen-gép helyi fejlesztési mátrixok és a valós életből gyártási fürt mátrixok diagnosztika végrehajtásához szolgáltatás fejlesztők számára.


## <a name="debugging-service-fabric-java-applications"></a>Hibakeresési szolgáltatás háló Java-alkalmazások

Java-alkalmazásokhoz [több naplózás keretek](http://en.wikipedia.org/wiki/Java_logging_framework) érhetők el. Mivel a `java.util.logging` az alapértelmezett beállítás JRE, a is használja a [github példák kódot](http://github.com/Azure-Samples/service-fabric-java-getting-started).  A következő megbeszélés megtudhatja, hogyan konfigurálása a `java.util.logging` keretrendszer. 
 
Java.util.logging használatával irányíthatja át az alkalmazás naplók memóriát, kimeneti adatfolyamok, konzolfájl vagy sockets. Az egyes az alábbi lehetőségek vannak alapértelmezett kezelők már megadott keretében. Létrehozhat egy `app.properties` fájl konfigurálása a fájl kezelő az alkalmazás az összes naplók átirányítása egy helyi fájl. 

A következő kódrészletet egy példa konfigurációs tartalmazza: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

A mappa által hivatkozott a `app.properties` léteznie kell, fájlt. Után a `app.properties` jön létre, módosítania kell a bejegyzés pont parancsprogramot is `entrypoint.sh` a a `<applicationfolder>/<servicePkg>/Code/` mappát, a tulajdonság `java.util.logging.config.file` való `app.propertes` fájlt. A bejegyzés a következő kódtöredékének hasonlóan kell kinéznie:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Ebben a konfigurációban a elforgatása módon gyűjtenek naplókban eredmények `/tmp/servicefabric/logs/`. A **%u** és **%g** engedélyezése hozhat létre több fájlt, a fájlnevekben mysfapp0.log, mysfapp1.log és így tovább. Alapértelmezés szerint nincs kezelője explicit módon van beállítva, ha a konzol kezelő van regisztrálva. A naplók syslog /var/log/syslog csoportban az egyik megtekintése.
 
További tudnivalókért lásd a [github példák kódot](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Következő lépések
Nyomkövetés ugyanazt kódot hozzáadni az alkalmazást a diagnosztika az alkalmazás egy Azure fürt is működik. Olvassa el az alábbi cikkekben megvitatása a különböző beállításokat, az eszközök és ismertetik, hogyan beállíthatja őket.
* [Hogyan gyűjthetők össze az Azure diagnosztikai naplók](service-fabric-diagnostics-how-to-setup-lad.md)
