<properties 
    pageTitle="Függőségek, a kivételek és a végrehajtás időpontok Java web Apps alkalmazások figyelése" 
    description="Az alkalmazás az összefüggéseket a Java webhely figyelemmel kísérése terjeszteni" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Függőségek, a kivételek és a végrehajtás időpontok Java web Apps alkalmazások figyelése

*Alkalmazás háttérismeretek az előzetes verzióban.*

Ha telepítette [az alkalmazást az összefüggéseket a Java-webalkalmazást rendszereken][java], mélyebb hírcsatornájában, kód módosítások nélkül beszerzése a Java Agent is használhatja:


* **Függőségek:** Más összetevőket, többek között az alkalmazás módosít hívások adatait:
 * **Többi hívások** HttpClient OkHttp és RestTemplate (tavaszi) keresztül.
 * **Vgx.dll** hívások a Jedis ügyfél keresztül. Ha a hívás 10s-nél több időt vesz igénybe, a agent is beolvassa a hívás argumentumokat.
 * **[JDBC hívások](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, az Oracle DB vagy Apache Derby DB. "executeBatch" hívásokat a rendszer támogatja. MySQL és PostgreSQL Ha a hívás több időt vesz igénybe 10s, mint a agent jelenti a lekérdezéstervet. 
* **Kifogott kivételeket:** Kivételek a kód által kezelt adatok.
* **Módszer végrehajtás ideje:** Adott módszerek végrehajtásához szükséges időt adatait.

A Java agent használatához, akkor a kiszolgálón telepítenie. A web Apps alkalmazások kell rendszereken, és az [Alkalmazás az összefüggéseket Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Az alkalmazás az összefüggéseket ügynök Java telepítése

1. A számítógépen futó a Java-kiszolgálóhoz, [Töltse le a agent](https://aka.ms/aijavasdk).
2. Szerkessze a alkalmazás kiszolgáló indítási parancsfájlt, és adja hozzá a következő JVM:

    `javaagent:`*a ügynök üveg fájl teljes elérési útja*

    Ha például a Tomcat Linux gépen:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Indítsa újra a alkalmazáskiszolgáló.

## <a name="configure-the-agent"></a>A ügynökök beállítása

Hozzon létre egy nevű fájlt `AI-Agent.xml` , és helyezze el a ügynök üveg fájl ugyanabban a mappában.

Állítsa az XML-fájl tartalmát. A következő példa szerepeltetésére, illetve elhagyja a szolgáltatásokat, amelyet szerkeszteni. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Engedélyeznie kell az jelentések kivétel és az egyes módszerek módszer időzítését.

Alapértelmezés szerint `reportExecutionTime` IGAZ és `reportCaughtExceptions` hamis.

## <a name="view-the-data"></a>Az adatok megtekintése

Az alkalmazás az összefüggéseket erőforrás összesített távoli függőség és módszer végrehajtási időpont elem [alatt]jelenik meg a teljesítményét csempére[metrics]. 

Keresett függőség, kivétel és módszer jelentések különálló példányai, nyissa meg a [keresési][diagnostic]. 

[Felismerése függőség kérdések – megtudhatja, hogy](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Kérdések? Problémákat?

* Adatok nélkül? [Tűzfal a kivételek megadása](app-insights-ip-addresses.md)
* [Java – hibaelhárítás](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
