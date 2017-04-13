## <a name="receive-messages-with-apache-storm"></a>Apache vihar üzenetek fogadása

[**Apache vihar**](https://storm.incubator.apache.org) elosztott valós idejű számítási rendszer egyszerűbbé teszi az adatok határolatlan adatfolyamok megbízható feldolgozása. Ez a szakasz megtudhatja, hogy hogyan egy esemény hubok vihar spout esemény hubok események fogadni. Apache vihar használ, szétoszthatja események több folyamatok csomópontjai kiszolgálón keresztül. Vihar esemény hubok integrációja leegyszerűsíti esemény felhasználási átlátszó ellenőrzésipont meghívási folyamat Pápaszemes Zookeeper telepítéssel, állandó pontjainak kezelése, és a párhuzamos esemény hubok kap.

Esemény hubok kapcsolatban további tudnivalókat mintázatok kap, az [esemény hubok – áttekintés][]című témakörben találhat.

Ebben az oktatóanyagban [HDInsight vihar][] telepítés megtalálható az esemény hubok spout már elérhető használja.

1. Kövesse a hozzon létre egy új HDInsight fürt és távoli asztali keresztül csatlakozhat [HDInsight vihar – első lépések](../articles/hdinsight/hdinsight-storm-overview.md) az eljárást.

2. Másolás a `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` a helyi fejlesztői környezet fájlt. Az események-vihar-spout tartalmaz.

3. A következő parancsot használja a csomag telepítéséhez a helyi tárolóba maven tesztelése. Ez lehetővé teszi, hogy vegye fel a vihar projektben, a leendő hivatkozásként.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Holdas, hozzon létre egy új maven tesztelése projekt (kattintson a **fájl** **Új**, majd a **Projekt**).

    ![][12]

5. Jelölje ki a **munkaterület helyének használatát**, majd kattintson a **Tovább** gombra

6. Jelölje ki a **maven tesztelése-archetype-quickstart útmutató** archetype, majd kattintson a **Tovább** gombra

7. A **Csoportazonosító** és **ArtifactId**, majd kattintson a **Befejezés gombra**

8. Az **pom.xml**, adja meg a következő függőségeket a `<dependency>` csomópontot.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Az **src** mappában hozzon létre egy **Config.properties** nevű fájlt, és másolja a vágólapra az alábbi tartalmat, az alábbi értékek helyettesítése:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    **Eventhub.receiver.credits** érték határozza meg, hogy hány események rendszer kötegekbe foglalja őket történő vihar felengedése előtt. Az egyszerűség kedvéért ebben a példában 10 értéket állítja be. Gyártási akkor általában értékre kell állítani a magasabb értékek; Ha például 1024.

10. Hozzon létre egy új osztály **LoggerBolt** nevű a következő kódot:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    A vihar rögzített naplózza a kapott események tartalmát. Ez egyszerűen meghosszabbítható egy tárhelyszolgáltatáshoz kockára tárolni. A [HDInsight érzékelő analysis oktatóprogram] használja ezt a megközelítést be HBase tárolja az adatokat.

11. A következő kódot **LogTopology** nevű osztály létrehozása:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Az osztály létrehoz egy új esemény hubok spout, a tulajdonságok használata a konfigurációs fájl instatiate azt. Fontos, hogy ne feledje, hogy ez a példa annyi spouts feladatok az esemény-központban partíciót számát az adott esemény elosztót által megengedett legnagyobb párhuzamos használatához.

<!-- Links -->
[Esemény hubok – áttekintés]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight vihar]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight érzékelő analysis oktatóprogram]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png