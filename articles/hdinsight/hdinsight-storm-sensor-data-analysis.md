<properties
   pageTitle="Apache vihar és HBase szenzoradatokat elemzése |} Microsoft Azure"
   description="Megtudhatja, hogy miként csatlakozhat egy virtuális hálózati Apache vihar. Segítségével vihar HBase folyamat szenzoradatokat esemény-központban, és a D3.js ábrázolása azt."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Apache vihar, esemény központi és a HDInsight (Hadoop) HBase érzékelő adatok elemzése 

Megtudhatja, hogy miként segítségével Apache vihar hdinsight szolgáltatásból lehetőségre a folyamat szenzoradatokat Azure esemény központból, tárolja azt be a HDInsight Apache HBase és ábrázolása azt D3.js fut, mint az Azure Web App használatával.

Az erőforrás-kezelő Azure-sablon a dokumentumban használt bemutatja, hogyan hozhat létre több Azure erőforrások erőforráscsoport. Pontosabban hoz létre egy Azure virtuális hálózat, két HDInsight fürt (vihar és HBase) és az Azure-Webappokban. A valós idejű webes irányítópult node.js végrehajtásának automatikusan telepíti, a web App alkalmazásban.

> [AZURE.NOTE] Az adatokat a dokumentum vagy a megadott, például a HDInsight 3.3 Linux-alapú és 3.4 fürt verziók teszteltük.

## <a name="prerequisites"></a>Előfeltételek

* Egy Azure-előfizetést. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Nincs szükség, egy meglévő HDInsight fürthöz a lépéseket a jelen dokumentum hoz létre az alábbi forrásokat:
    >
    > * Az Azure virtuális hálózaton
    > * A HDInsight fürthöz egy vihar (Linux alapú 2 dolgozó csomópontok)
    > * A HDInsight fürthöz egy HBase (Linux alapú 2 dolgozó csomópontok)
    > * Az Azure-Webappokban, amelyen a webes irányítópult

* [Node.js](http://nodejs.org/): ennek segítségével a fejlesztői környezet a helyi meghajtóra az webes irányítópult megtekintése.

* [Java- és a JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): a vihar topológia fejlesztéséhez használható.

* [Maven tesztelése](http://maven.apache.org/what-is-maven.html): létre és állíthat össze a projekt használt.

* [Mely számjegy](http://git-scm.com/): Töltse le a projekt GitHub használt.

* Egy __SSH__ ügyfél: a HDInsight Linux-alapú fürt használja. A HDInsight SSH használja a további tudnivalókért lásd: a következő dokumentumokat.

    * [A Windows ügyfélről HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [HDInsight Linux rendszerhez, a Unix vagy a Mac ügyfélprogramból SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Is kell rendelkeznie a hozzáférést a `scp` parancs, amellyel a fájlok másolása a helyi fejlesztői környezet és a HDInsight fürt SSH használata között.

## <a name="architecture"></a>Architektúra

![architektúra diagramja](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Ebben a példában az alábbi összetevőket áll:

* **Azure esemény hubok**: érzékelőktől gyűjtött adatokat tartalmazza. Ebben a példában az alkalmazások van, feltéve, hogy létrehozza az adatokat.

* **A HDInsight vihar**: biztosít az adatok valós idejű feldolgozás esemény központból.

* **A HDInsight HBase**: biztosítja a NoSQL állandó adattár adatok feldolgozása vihar után.

* **Azure virtuális hálózati szolgáltatás**: lehetővé teszi, hogy a vihar HDInsight a és a HDInsight fürt HBase közötti biztonságos kommunikáció.

    > [AZURE.NOTE] A virtuális hálózati nem a HBase fürtre vonatkozóan a nyilvános átjáró felett megjelenő megegyezik a Java HBase ügyfélprogram API használatához szükséges. HBase és vihar fürt telepítése a azonos virtuális hálózatba lehetővé teszi, hogy a vihar fürt (vagy bármely más rendszer a virtuális hálózaton) így közvetlenül elérheti HBase ügyfélprogram API segítségével.

* **Irányítópult-webhely**: egy minta irányítópulton, amelyek a diagramok valós idejű adatok.

    * A webhely alkalmazása a Node.js, hogy bármilyen ügyfél operációs rendszeren teszteléshez futtatását is lehetővé teszi, vagy Azure webhelyekre telepíthető.

    * Valós idejű kommunikációs a vihar topológiája és a webhely között [socket.IO](http://socket.io/) használatos.

        > [AZURE.NOTE] Ez a egy végrehajtása részletesen. Minden kommunikációs keretrendszer, például nyers WebSockets vagy SignalR is használhatja.

    * Az adatok, a webhely küldött graph [D3.js](http://d3js.org/) szolgál.

> [AZURE.IMPORTANT] Két fürt szükségesek, mint nincs semmilyen módszerrel nem támogatott hozhat létre egy HDInsight fürt vihar és HBase.

A topológia használatával a [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) osztály esemény központi adatokat olvas, és adatot ír be HBase a [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) osztály használatával. A webhely való kommunikáció végezhető el [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).

Az alábbiakban megtalálja a topológia folyamatábrája.

![Topológiai diagram](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Ez egy, a topológia nagyon egyszerűsített nézete. Futásidőben minden összetevő példányának létrejön az összes partíciót az olvasott, esemény-központban. Ezekben az esetekben vannak elosztva a fürt csomópontját, és adatok továbbítás közöttük az alábbi képlettel történik:
>
> * Adatok az elemző a spout terheléselosztása.
> * Az irányítópult és HBase az elemző adatainak Eszközazonosítót, csoportosítva, hogy mindig flow ugyanarra az eszközre az üzenetek és a azonos összetevőt.

### <a name="topology-components"></a>Topológia összetevők

* **EventHub Spout**: A spout Apache vihar verzió 0.10.0 megadott vagy újabb.

    > [AZURE.NOTE] Az esemény hubok spout ebben a példában egy vihar HDInsight fürt verzióját 3.3 vagy 3.4 szükséges. Esemény hubok használatát a HDInsight vihar régebbi verziójával további tudnivalókért lásd [Azure esemény hubok vihar a hdinsight szolgáltatásból lehetőségre a folyamat eseményeit](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: az adatok, amelyek a spout által kibocsátott nyers JSON, és alkalmanként egynél több esemény kibocsátott egyszerre. A rögzített bemutatja, hogyan kell olvasni az adatokat a spout által kibocsátott, és egy több mezőket tartalmazó rekord mint elhelyezése egy új adatfolyam.

* **DashboardBolt.java**: Ez bemutatja, hogyan lehet a Socket.io ügyfél tár Java segítségével adatokat küld valós idejű a webes irányítópult.

Ebben a példában az [neutronáramú](https://storm.apache.org/releases/0.10.0/flux.html) keretrendszer, a topológia definíció lévő YAML fájlokat. Kétféle:

* __nincs-hbase.yaml__ - használja ezt a fájlt a fejlesztői környezet a topológia tesztelésekor. Mivel az nem tud hozzáférni a HBase Java API, az a fürt abban a virtuális hálózaton kívüli HBase összetevők, nem használja.

* __a hbase.yaml__ - használja ezt a fájlt, a topológia a vihar fürthöz telepítésekor. Mivel a HBase fürt virtuális ugyanabba a hálózatba futása HBase összetevők használ.

## <a name="prepare-your-environment"></a>A környezet előkészítése

Mielőtt használatba ebben a példában, létre kell hoznia egy esemény beolvassa a vihar topológia Azure hubhoz.

### <a name="configure-event-hub"></a>Esemény központi konfigurálása

Esemény központi az ebben a példában az adatforráshoz. Az alábbi lépésekkel használatával hozzon létre egy új esemény hubon keresztül csatlakozott.

1. Az [Azure portálon](https://portal.azure.com)válassza az **+ Új** -> __Internetes a dolog, amit__ -> __Esemény hubok__.

2. Kattintson a __Namespace létrehozása__ lap hajtsa végre az alábbi műveleteket:

    1. Írja be egy __nevet__ a névtér.
    2. Jelölje ki a árak réteg. __Egyszerű__ is elegendő, ebben a példában.
    3. Jelölje ki az Azure __előfizetés__ használni.
    4. Jelölje ki a meglévő erőforrás csoportba, vagy hozzon létre egy újat.
    5. Válassza ki a __helyet__ az esemény-központban.
    6. Jelölje ki a __PIN-kód irányítópult__, és kattintson a __Létrehozás__gombra.

3. Ha a létrehozási folyamat befejeződik, a névtér az esemény hubok lap jelenik meg. További lehetőségek válassza az __+ Add esemény központi__. Az __Esemény központi létrehozása__ lap írjon be egy nevet a __sensordata__ , és válassza a __Create__. Az alapértelmezett értékeket a többi mezőt hagyja.

4. Jelölje ki a névtér esemény hubok lap, __Esemény hubok__. Jelölje ki a __sensordata__ .

5. Az esemény központi sensordata-lap kattintson a __megosztott access-házirendek__. A __+ Hozzáadás__ hivatkozást kövesse az alábbi házirendeket alkalmazhatja:


  	| Házirend neve | Követelések |
  	| ----- | ----- |
  	| eszközök | Küldés |
  	| vihar | Meghallgatása |

5. Jelölje ki a házirendet, és jegyezze fel az __Elsődleges kulcs__ értékét. Mindkét házirendek jövőbeli lépésben szüksége lesz az értéket.

## <a name="download-and-configure-the-project"></a>Töltse le és állítsa be a projekt

A következő segítségével töltse le a projekt GitHub.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

A parancs befejeződése után a következő címtár-struktúra választania kell:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] A dokumentum nem válassza minden részlet, a kód; Ez a példa tartalmazza jó helyen jár a kód teljesen Megjegyzéssel ellátva.

Nyissa meg a **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** fájlt, és adja hozzá a következő sort a esemény központi adatokat:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] Ez a példa feltételezi, hogy használt-e __vihar__ a házirendet, amely tartalmaz egy __meghallgatása__ igénylése nevet, és, hogy az esemény központi __sensordata__nevű.

 Mentse a fájlt, ez az információ hozzáadása után.

## <a name="compile-and-test-locally"></a>Állíthat össze, és tesztelje a helyi meghajtóra

Tesztelés, mielőtt el kell indítani az irányítópulton a kimenet: a topológia megtekintése, és készítése adatok tárolására az esemény-központban.

> [AZURE.IMPORTANT] A topológia HBase összetevője nincs bekapcsolva, amikor tesztelése helyi meghajtóra, mint a Java API a HBase fürt nem érhető el a a fürt tartalmazó Azure virtuális hálózaton kívüli.

### <a name="start-the-web-application"></a>A webes alkalmazás indítása

1. Nyissa meg egy új parancssor vagy a terminált, majd könyvtárak módosítása az **Irányítópult hdinsight-eventhub – például**a következő parancsot használja a függőségeket, szükség szerint a webes alkalmazás telepítése:

        npm install

2. A következő parancs használata a webes alkalmazás indítása:

        node server.js

    Meg kell jelennie a következőhöz hasonló üzenet:

        Server listening at port 3000

2. Nyisson meg egy webböngészőt, és adja meg **http://localhost:3000 /** címként. Meg kell jelennie egy lapot, az alábbihoz hasonló:

    ![Irányítópult webes](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Hagyja a parancssor vagy Terminálszolgáltatások nyitva. Tesztelés, után használhatja a Ctrl-C le szeretné állítani az érintett webkiszolgálóra.

### <a name="start-generating-data"></a>Indítsa el az adatok létrehozása

> [AZURE.NOTE] Ebben a szakaszban szereplő lépéseket Node.js használja, így bármely platformon használható. Más nyelv Példák című témakörben talál a **SendEvents** Directoryban.

1. Nyissa meg egy új parancssor, rendszerhéj vagy terminált, könyvtárak átállítása **hdinsight-eventhub – például/SendEvents/nodejs**, majd a függőségeket, szükség szerint az alkalmazás telepítése a következő parancsot használja:

        npm install

2. Nyissa meg a **app.js** fájlt egy szövegszerkesztőben, és adja meg a korábbi szerezte be esemény központi adatokat:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] Ez a példa feltételezi, hogy használt __sensordata__ nevet az esemény központi és __eszközök__ a házirendet, amely tartalmaz egy __küldése__ igénylése nevet.

2. A következő paranccsal illessze be az új tételeket az esemény-központban:

        node app.js

    Ekkor megjelennek a kimeneti több sornyi esemény központi küldött adatokat tartalmazzák. Az alábbihoz hasonló jelennek meg:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Indítsa el a topológia

2. Nyissa meg egy új parancssort, rendszerhéj vagy __TemperatureMonitor hdinsight-eventhub – például__terminált, és új könyvtárak, és a következő parancs használatával indítsa el a topológia:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Ha a PowerShell használata esetén használja a következő:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Ha egy Linux/Unix/OS X rendszeren, és [a fejlesztői környezet a vihar telepítve](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)van, az alábbi parancsok inkább is használhatja:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Ez a parancs elindítja a helyi módban __nem-hbase.yaml__ fájlban definiált topológiában. A __dev.properties__ fájlban lévő értékeket adja meg az esemény hubok a kapcsolat adatait. Miután elindult, a topológia esemény központból beolvassa a bejegyzéseket, és elküldése az irányítópulton a helyi számítógépen futó. Meg kell jelennie a vonalak jelennek meg a webes irányítópult, az alábbihoz hasonló:

    ![az adatok irányítópult](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Az irányítópult futása közben használja a `node app.js` esemény hubok új adatokat küldeni az előző lépéseket parancsot. A hőmérsékleti értékek véletlenszerűen jönnek létre, mert az a diagram, frissítenie kell az hőmérsékleti nagy változásait szemléltetik.

    > [AZURE.NOTE] Kell lennie a __Hdinsight-eventhub – például/SendEvents/Nodejs__ címtárban használata esetén a `node app.js` parancsot.

3. Annak ellenőrzése, hogy ez hogyan működik, után állítsa le a topológia, használja a Ctrl + C billentyűkombinációt. Ctrl + C billentyűkombinációt is használhatja a helyi webkiszolgáló is le.

## <a name="create-a-storm-and-hbase-cluster"></a>Vihar és HBase fürt létrehozása

Annak érdekében, hogy futtassa a topológia hdinsight szolgáltatásból lehetőségre, és ahhoz, hogy a HBase rögzített, akkor létre kell hoznia egy új vihar fürt és HBase fürt. Ez a szakasz lépéseit az [erőforrás-kezelő Azure-sablon](../resource-group-template-deploy.md) használatával hozzon létre egy új Azure virtuális hálózati és egy vihar és HBase fürt a virtuális hálózaton. A sablon is az Azure-Webappokban hoz létre, és üzembe helyezése az Irányítópulton egy példányát a mikrofonba.

> [AZURE.NOTE] Virtuális hálózatot, hogy a topológia a a vihar fürthöz közvetlenül kommunikálni tudjanak a HBase fürt a HBase Java API használja.

Az erőforrás-kezelő sablon a dokumentumban használt __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__a nyilvános blob-tárolóban található.

1. A következő gombra kattintva jelentkezzen be az Azure, és nyissa meg az erőforrás-kezelő sablon az Azure-portálon.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. A **Paraméterek** lap az adja meg az alábbiakat:

    ![HDInsight paraméterei](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: Ez az érték lesz a vihar alap nevét, és HBase fürtök. Például __hdi__ bevitelét és hoz létre __vihar-hdi__ nevű vihar fürt egy __hbase-hdi__nevű HBase fürthöz.
    * __CLUSTERLOGINUSERNAME__: A rendszergazdai felhasználónevével a vihar és HBase fürtre vonatkozóan.
    * __CLUSTERLOGINPASSWORD__: a vihar és HBase fürt rendszergazdai jelszavát.
    * __SSHUSERNAME__: A SSH felhasználói számára a vihar és HBase fürt létrehozásához.
    * __SSHPASSWORD__: a SSH a vihar és HBase fürt felhasználó jelszavát.
    * __Hely__: az régiót, amelynek a fürt jön létre.
    
    Kattintson __az OK__ gombra a paraméterek mentéséhez.
    
3. Az __erőforráscsoport__ szakasz használatával hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt.

4. __Erőforrások csoport helye__ legördülő menüben jelölje ki az ugyanazon a helyen közben a __hely__ paraméter-e jelölve.

5. Jelölje ki a __jogi feltételeket__, és válassza a __Create__.

6. Végül jelölje be a __PIN-kód irányítópult__ , és válassza a __Létrehozás__. Körülbelül 20 percet fog tartani a fürt létrehozásához.

Az erőforrások létrehozása után a lesz irányítva, a egy az erőforráscsoport fürt és webes irányítópult tartalmazó lap.

![Erőforrás csoport lap vnet és fürt](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] A HDInsight fürt azoknak a program __Vihar-BASENAME__ és __hbase-BASENAME__, ahol a BASENAME megadott a sablon nevét. A nevek használandó újabb lépéseket a fürt való csatlakozáskor. Tartsa szem előtt, hogy a webhely neve; az irányítópult __basename-irányítópult__-e. Fogja használni a később az irányítópulton megtekintésekor.

## <a name="configure-the-dashboard-bolt"></a>Az irányítópult rögzített konfigurálása

Adatok az irányítópulton a web App rendszerbe küldhet módosítania kell a következő sort a fájlban __dev.properties__ :

    dashboard.uri: http://localhost:3000

Módosítás `http://localhost:3000` való `http://BASENAME-dashboard.azurewebsites.net` , és mentse a fájlt. __BASENAME__ lecseréli az előző lépésben megadott alap nevére. Az erőforráscsoport korábban létrehozott az irányítópulton jelölje ki, és megtekintheti az URL-CÍMÉT is használhatja.

## <a name="create-the-hbase-table"></a>A HBase táblázat létrehozása

Annak érdekében, hogy tárolja az adatokat a HBase, azt először létre kell hoznia egy táblát. Általában előre létrehozni kívánt információforrások, amelyek vihar kell írni, próbál létrehozni, a vihar topológiában belül az erőforrások eredményezhet, elosztott példányok, a kód ugyanaz az erőforrás létrehozni. Az erőforrások kívül a topológia létrehozása, és csak használata vihar olvasási/írási és az elemzést.

1. SSH segítségével csatlakozzon a HBase fürthöz a SSH felhasználói és a csoport létrehozása során a sablonba megadott jelszót használja. Ha például csatlakozás használatával a `ssh` parancsot, és használja a következő szintaxist:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    Ez a parancs cserélje ki a megadott létrehozásakor a csoportját, és __BASENAME__ megadott Alap nevű SSH felhasználónév __felhasználónév__ . Amikor a rendszer kéri, írja be jelszavát a SSH felhasználó.

2. A SSH munkamenetből indítsa el a HBase rendszerhéj.

        hbase shell
    
    Ha a rendszerhéj betöltött, látni fog egy `hbase(main):001:0>` kérdés.

3. Az HBase rendszerhéj írja be a következő parancsot a szenzoradatokat tároló tábla létrehozása:

        create 'SensorData', 'cf'

4. Győződjön meg arról, hogy a táblázat hozott létre a következő parancsot:

        scan 'SensorData'
        
    Ez megjeleníti az információkat hasonlóan, mint az alábbi példa, jelezve, hogy nincsenek-e 0 sorok a táblázatban.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Írja be az alábbi módon lépjen ki a HBase rendszerhéj:

        exit

## <a name="configure-the-hbase-bolt"></a>A HBase rögzített konfigurálása

Írják HBase a vihar fürt, meg kell adnia a HBase rögzített a HBase fürt konfigurációs adatokkal. A művelet legegyszerűbben a __hbase-site.xml__ letöltése a fürt, és adja hozzá a projekthez. Meg kell is vegye ki a megjegyzésjeleket a __pom.xml__ fájlban több függőségek, amely a vihar-hbase összetevő és függőség betöltése.

> [AZURE.IMPORTANT] Is le kell töltenie a vihar HDInsight fürt 3.3 vagy 3.4 fürt; a megadott vihar-hbase.jar fájl Ez a verzió lefordított HBase használata 1.1.x, amelyet az HDInsight 3.3 és 3.4 fürt HBase. Vihar-hbase összetevő használ a máshol, ha azt elleni HBase régebbi verzióját is össze kell állítani.

### <a name="download-the-hbase-sitexml"></a>Töltse le a hbase site.xml

A parancssorból SCP segítségével töltse le a __hbase-site.xml__ fájlt a fürt. A következő példában a megadott létrehozásakor a csoportját, és __BASENAME__ korábban megadott Alap nevű SSH felhasználó __felhasználónév__ cserélje. Amikor a rendszer kéri, írja be jelszavát a SSH felhasználó. Cserélje le a `/path/to/TemperatureMonitor/resources/hbase-site.xml` az ebben a TemperatureMonitor projekt fájl elérési útját.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

A megadott elérési utat a __hbase-site.xml__ letöltése.

### <a name="download-and-install-the-storm-hbase-component"></a>Töltse le és telepítse a vihar-hbase-összetevő

1. A parancssorból SCP segítségével töltse le a __vihar-hbase.jar__ fájlt a vihar fürt. A következő példában a megadott létrehozásakor a csoportját, és __BASENAME__ korábban megadott Alap nevű SSH felhasználó __felhasználónév__ cserélje. Amikor a rendszer kéri, írja be jelszavát a SSH felhasználó.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Nevű fájl letöltése `storm-hbase-####.jar`, ahol ### a fürt vihar verziószám. Jegyezze le ezt a számot, később használják.

2. A következő paranccsal az összetevő telepítéséhez a helyi maven tesztelése tárházából a fejlesztői környezet be. A csomag kereséséhez, amikor a projekt összeállítása maven tesztelése szolgáltatás lehetővé teszi. Csere __####__ a verziószámú, a fájl nevét tartalmazza.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Ha a PowerShell használata esetén használja az alábbi parancsot:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>A projektben a vihar-hbase összetevő engedélyezése

1. Nyissa meg a __TemperatureMonitor/pom.xml__ fájlt, és törölje a következő sort:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Csak a következő két sorok; törlése Törölje a sorok közötti őket.
    
    Több összetevők szükségesek, amikor HBase a hbase rögzített használatával kommunikál lehetővé teszi.

2. Keresse meg a következő sorokat, majd a csere __####__ a letöltött fájl vihar-hbase verziószámmal.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] A verziószám egyeznie kell a használt történő helyi maven tesztelése tárházba az összetevő telepítésekor maven tesztelése használ ezt az információt a összetevő betöltése, a projekt készítésekor verziója.

2. Mentse a __pom.xml__ fájlt.

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Építse fel, csomag és a megoldás telepítse hdinsight szolgáltatáshoz

A fejlesztői környezetben kövesse az alábbi lépéseket a vihar fürt szeretne telepíteni, a vihar topológiában.

1. A __TemperatureMonitor__ címtárból segítségével a következő parancs végrehajtása egy új build és üveg csomag készítése a projekthez:

        mvn clean compile package

    Ezzel létrehoz egy fájlt, **TemperatureMonitor-1.0-SNAPSHOT.jar** nevű a projekt **cél** könyvtárában található.

2. Scp segítségével a __TemperatureMonitor-1.0-SNAPSHOT.jar__ fájl feltöltése a vihar fürthöz. A következő példában a megadott létrehozásakor a csoportját, és __BASENAME__ korábban megadott Alap nevű SSH felhasználó __felhasználónév__ cserélje. Amikor a rendszer kéri, írja be jelszavát a SSH felhasználó.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Eltarthat néhány percig, amíg feltölteni a fájlt, mint több MB méretű legyen.

    Felhasználhatja a scp feltölteni a __dev.properties__ fájlt az esemény hubok és az irányítópult eléréséhez használt információkat tartalmaz.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. A fájlok fel van töltve, miután csatlakozni a fürt SSH használatával.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. A SSH munkamenetből a következő parancs használatával indítsa el a topológia.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Ez a parancs elindítja a topológiát, a topológia meghatározása a __hbase.yaml a__ fájlban, és a konfigurációs értékek használata a __dev.properties__ fájlt.

3. Miután a topológia elindult, nyissa meg a böngészőt az Azure a közzétett webhelyre, majd használja a `node app.js` adatok küldése a esemény központi parancsot. Meg kell jelennie a webes irányítópult frissíteni az információkat jeleníthet meg.

    ![Irányítópult](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>HBase adatainak megtekintése

Miután elküldte a topológia segítségével adatokat `node app.js`, kövesse az alábbi lépéseket HBase kapcsolódhat, és ellenőrizze, hogy az adatok került-e a korábban létrehozott táblázatot.

1. SSH segítségével csatlakozzon a HBase fürthöz.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. A SSH munkamenetből indítsa el a HBase rendszerhéj.

        hbase shell
    
    Ha a rendszerhéj betöltött, látni fog egy `hbase(main):001:0>` kérdés.

2. A tábla sorainak megjelenítése:

        scan 'SensorData'
        
    Ez célszerű adatok visszaadására a következőhöz hasonló jelezve, hogy nincsenek-e 0 sorok a táblázatban.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] A beolvasott képet művelet a táblázat csak legfeljebb 10 sorát adja vissza.

## <a name="delete-your-clusters"></a>A fürt törlése

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

A fürt, a tárhely és a web app egyidejű törléséhez törölje az értékgörbét tartalmazó erőforráscsoport.

## <a name="next-steps"></a>Következő lépések

Most már megtanulta azt vihar használata adatainak beolvasása esemény hubok, tárolja azt be HBase és Socket.io és D3.js használatával jelenítse meg az információkat egy külső irányítópulton.

* A HDInsight vihar topológiák további példákat a következő cikkekben talál:

    * [Példa a HDInsight vihar topológiát](hdinsight-storm-example-topology.md)

* Apache vihar kapcsolatos további tudnivalókért lásd: a [Apache vihar](https://storm.incubator.apache.org/) webhelyet.

* A HDInsight HBase kapcsolatos további tudnivalókért olvassa el a a [HBase HDInsight áttekintése](hdinsight-hbase-overview.md)című témakört.

* Socket.io kapcsolatos további tudnivalókért lásd: a [socket.io](http://socket.io/) webhelyet.

* D3.js kapcsolatos további tudnivalókért lásd: [D3.js - adatok-alapú dokumentumok](http://d3js.org/).

* Java topológiák létrehozásával kapcsolatban további tudnivalókért lásd: [kidolgozása Java topológiák Apache vihar a HDInsight](hdinsight-storm-develop-java-topology.md).

* .NET topológiák létrehozásával kapcsolatban további tudnivalókért lásd [a Visual Studio segítségével HDInsight Apache vihar topológiát kidolgozása C#](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
