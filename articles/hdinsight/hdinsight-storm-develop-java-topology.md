<properties
   pageTitle="Fejleszthet olyan Java-alapú topológiák Apache vihar |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre vihar topológiák Java: hozzon létre egy egyszerű word darab topológia."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Java-alapú topológiák Apache vihar és a HDInsight maven tesztelése egyszerű szószám alkalmazáshoz kidolgozása

Megtudhatja, hogy miként hozhat létre a egy Java-alapú topológia Apache vihar a HDInsight-maven tesztelése használatával. Azt ismerteti, azon a folyamaton, egy egyszerű szószám alkalmazás maven tesztelése és Java, ahol a topológia Java könyvjelzőnév létrehozása. Ezután megtanulhatja a megadása a topológia az neutronáramú keretrendszer használatával.

> [AZURE.NOTE] Neutronáramú keretében vihar 0.10.0 érhető el vagy újabb verziója. A HDInsight 3.3 és 3.4 vihar 0.10.0 érhető el.

Miután elkészült a lépéseket a dokumentumban, lehetősége van egy egyszerű topológia, amely a HDInsight Apache vihar telepítheti.

> [AZURE.NOTE] A [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount)az ehhez a dokumentumhoz létrehozott topológiák bejegyzett verziója.

##<a name="prerequisites"></a>Előfeltételek

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java Developer Kit (JDK) 7-es verzió</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven tesztelése</a>: maven tesztelése rendszer projekt build Java-projektekhez.

* Szövegszerkesztőben, például a Jegyzettömbben <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Sublime szöveg</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Másik lehetőségként használhatja például <a href="https://eclipse.org/" target="_blank">Holdas</a> integrált fejlesztői környezetet (IDE) (verzió Luna vagy újabb).

    > [AZURE.NOTE] A szerkesztő vagy IDE funkcióiról a maven tesztelése, hogy a dokumentum címzettjei nem lehet. A funkciók a szerkesztési környezet információt dokumentációjában a terméket használni.

##<a name="configure-environment-variables"></a>Változók környezet beállítása

Az alábbi környezeti változók Java- és a JDK telepítésekor adható meg. Jó helyen jár ellenőrizze, hogy vannak, és a rendszer a megfelelő értékeket tartalmaznak.

* A címtárhoz, amelyen telepítve van a (JRE) Java-futtatókörnyezet **JAVA_HOME** - kell mutatnia. Például a Unix vagy Linux terjesztési, meg kell kell egy értéknek hasonló `/usr/lib/jvm/java-7-oracle`. A Windows rendszerben azt szeretné, hogy hasonló érték`c:\Program Files (x86)\Java\jre1.7`

* **Elérési út** - kell tartalmaznia az alábbi elérési utak:

    * **JAVA_HOME** (vagy a megfelelő elérési út)

    * **JAVA_HOME\bin** (vagy a megfelelő elérési út)

    * A könyvtár, amelyen telepítve van a maven tesztelése

##<a name="create-a-new-maven-project"></a>Maven tesztelése új projekt létrehozása

A parancssorból a következő kód segítségével maven tesztelése **WordCount**nevű új projekt létrehozása:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

Ez hoz létre egy új könyvtár nevű **WordCount** az aktuális helyre, és egy egyszerű maven tesztelése projektet tartalmaz.

A **WordCount** könyvtár fogja tartalmazni az alábbiakat:

* **pom.XML**: a maven tesztelése projekt beállításait tartalmazza.

* **src\main\java\com\microsoft\example**: az alkalmazás kódot tartalmazza.

* **src\test\java\com\microsoft\example**: azt vizsgálja, az alkalmazás tartalmazza. Ez a példa azt nem létrehoznak vizsgálatok.

###<a name="remove-the-example-code"></a>A példa kód eltávolítása

Azt fog szervezni az alkalmazás, mert törlése a létrehozott tesztelése és az alkalmazás fájljait:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Tulajdonságok hozzáadása

Maven tesztelése tulajdonságok nevű projektszintű értékek megadását teszi lehetővé. Adja hozzá a következő után a `<url>http://maven.apache.org</url>` sor:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Más szakaszokba most ábrázolásakor ezeket az értékeket. Ha például vihar összetevők verziójának megadásakor ábrázolásakor `${storm.version}` helyett kemény coding értéket.

##<a name="add-dependencies"></a>Függőségek hozzáadása

Mivel ez egy vihar topológia, fel kell vennie vihar összetevők függőség. Nyissa meg a **pom.xml** fájlt, és a következő kód hozzáadása a ** &lt;függőségek >** szakasz:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Fordítási időben maven tesztelése lépések elvégzésével keresheti meg **vihar-core** a maven tesztelése adattárban ezt az információt használja. Meg először a jelentést a tárat a helyi számítógépre. Ha a fájlokat nem létezik, azt fog töltse le őket a nyilvános maven tesztelése tárat, és tárolja őket a helyi tárat.

> [AZURE.NOTE] Értesítés a `<scope>provided</scope>` sor jelöltük szakaszában. Ez jelzi, ha nem szeretné **vihar-core** hozzunk létre, üveg fájlok, mivel a rendszer nyújthatja maven tesztelése. Ezzel a csomagok hoz létre egy kicsit kisebb lesz, és azt ellenőrzi, hogy az a vihar HDInsight fürt szereplő **vihar-core** bit használandó.

##<a name="build-configuration"></a>Konfigurációs összeállítása

Maven tesztelése bővítmények engedélyezése, hogy a projekt, például a projekt lefordított hogyan vagy hogy miként készítheti elő üveg fájlba build szakaszainak testreszabásához. Nyissa meg a **pom.xml** fájlt, és adja hozzá a következő kódot közvetlenül a fenti a `</project>` sor.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

Ez a szakasz hozzáadása a bővítmények, erőforrások és egyéb beépített konfigurációs beállítások lesz. A __pom.xml__ fájl teljes hivatkozást olvassa el a [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html)című témakört.

###<a name="add-plug-ins"></a>Bővítmények hozzáadása

A vihar topológiák a <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Végrehajtási maven tesztelése beépülő modul</a> akkor lehet hasznos, mivel lehetővé teszi, hogy a topológia egyszerűen helyi futtatásához a fejlesztői környezet. Az alábbi módon adja hozzá a `<plugins>` a **pom.xml** fájlt, amelyet fel szeretne venni a végrehajtási maven tesztelése beépülő modul szakasza:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Megjegyzendő, hogy a `<mainClass>` bejegyzést használja `${storm.topology}`. Azt nem adja meg, korábbi verziók tulajdonságok szakaszában (de lehet, hogy.) Ehelyett azt fogja állítsa ezt az értéket a parancssorból a fejlesztői környezet egy újabb lépésben a topológia forgalmi.

Egy másik hasznos beépülő modul van a <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache maven tesztelése fordító beépülő modul</a>, amellyel a fordítási beállítások módosítása. Elsődleges szükség ennek oka az, módosíthatja a Java verziót használó maven tesztelése a forrás- és célwebhelyek az alkalmazás. Verzió 1.7 szeretnénk.

Adja hozzá a következő a `<plugins>` a **pom.xml** fájlt tartalmazza a Apache maven tesztelése fordító bővítményt, és állítsa a forrás- és célwebhelyek verziók 1.7 szakaszát.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Erőforrások konfigurálása

A források részben lehetővé teszi, hogy nem kód erőforrások, például a topológiában összetevők szükséges konfigurációs fájlokat. Adja hozzá a következő ebben a példában a `<resources>` **pom.xml** fájl szakaszában.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Ez a lehetőség az erőforrások könyvtár a legfelső szintű a projekt (`${basedir}`) olyan hely, amely tartalmazza az erőforrásokat, és a __log4j2.xml__nevű fájlt tartalmazza. Ez a fájl konfigurálása információk be van jelentkezve, a topológia által szolgál.

##<a name="create-the-topology"></a>A topológia létrehozása

Egy Java-alapú vihar topológia mint függőség, ahol a kell a szerző által három összetevőt (vagy a hivatkozás).

* **Spouts**: beolvassa a külső adatok forrásokat, és az adatok adatfolyamok bocsát a topológia be.

* **Bolts**: feldolgozás spouts vagy más csapszegek által kibocsátott adatfolyamok hajt végre, és bocsát ki egy vagy több adatfolyam megjelenítését.

* **Topológia**: hogyan a spouts csapszegek vannak rendezve, és adja meg a bejegyzés pontot a topológia az határozza meg.

###<a name="create-the-spout"></a>A spout létrehozása

Állítsa be a külső adatforrások követelményei csökkentéséhez a következő spout egyszerűen véletlen mondatok bocsát ki. A módosított verziójával egy spout a [vihar-Starter példák](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)biztosított.

> [AZURE.NOTE] Példa egy spout, amely beolvassa a külső adatforrásból olvassa el az alábbi példák:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java):-például spout, amely beolvassa a Twitteren
>
> * [Vihar-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): egy spout, amely beolvassa az Kafka

A spout hozzon létre egy új fájlt **RandomSentenceSpout.java** nevű az **src\main\java\com\microsoft\example** Directoryban és tartalmát a következő használni:

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Szánjon egy kis időt olvassa el a kód megjegyzések a spout működésének megértése.

> [AZURE.NOTE] Bár ezt a topológia csak egy spout használ, mások számos különböző forrásokból származó adatok hírcsatorna a topológia lehetnek.

###<a name="create-the-bolts"></a>A csapszegek létrehozása

Csapszegek kezelni az adatfeldolgozás. Ez a topológia van még két csapszegek:

* **SplitSentence**: Kettéválaszthatja-e az egyes szavakat az **RandomSentenceSpout** által kibocsátott mondatok.

* **WordCount**: megszámolja az egyes word történt, hány alkalommal.

> [AZURE.NOTE] Csapszegek bármit betűhíven, például számítást, adatmegőrzési vagy külső összetevők beszélgetésben.

Két új fájlok létrehozása, **SplitSentence.java** és **WordCount.Java** az **src\main\java\com\microsoft\example** Directoryban. A fájlok tartalma használható a következő:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Szánjon egy kis időt olvassa el a kód megjegyzések minden rögzített működésének megértése.

###<a name="define-the-topology"></a>A topológia meghatározása

A topológia a spouts kötelékek és bolts közös diagramját, amely meghatározza, hogy hogyan átfolyása az adatok között az összetevők be. Párhuzamos útmutatók vihar használó belül a fürt összetevő példányok létrehozásakor is tartalmaz.

Egyszerű diagram létrehozása a topológia összetevőinek a diagram a következő:

![az ábrán az látható, a spouts és csapszegek szerint rendezve](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

A topológia végrehajtásához a **src\main\java\com\microsoft\example** könyvtár **WordCountTopology.java** nevű új fájl létrehozása. A fájl tartalmát használható a következő:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Szánjon egy kis időt olvassa el a kód megjegyzéseket megtudhatja, hogy hogyan a topológia definiált és a fürt majd elé.

###<a name="configure-logging"></a>Naplózás beállításai

Vihar információk naplózása Apache Log4j használja. Ha nem adja meg a naplózás, a topológia sok diagnosztikai adatok, amelyek lehetnek nehezen olvasható lesz elhelyezése. Szabályozhatja, hogy milyen be van jelentkezve, hozzon létre egy __log4j2.xml__ az __erőforrások__ könyvtárban nevű fájlt. A következő használja a fájl tartalmát.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

Ezzel egy új naplózó az __com.microsoft.example__ osztály, amelyek tartalmazzák még a összetevők ebben a példában topológiában. A szintje pedig nyomkövetési-e a naplózó és fog rögzíthet bármilyen naplóadatokat a topológiában összetevők által kibocsátott. Ha tekinti meg újra a kód ehhez a projekthez, észre fogja, hogy csak a WordCount.java fájl végrehajtja a naplózás. egyes szavak számának naplózza.

A `<Root level="error">` szakasz beállítja a legfelső szintű naplózást (a szolgáltatás nem a __com.microsoft.example__,) a hiba adatait csak.

> [AZURE.IMPORTANT] Ez jelentősen csökkenti a adatokat naplózza a fejlesztői környezet a topológia tesztelésekor, való eltávolítás nem elő, amikor a gyártási fürthöz hibakeresési információkat. Ha csökkenteni szeretné az adatokat, is meg kell adni a konfigurációban a fürthöz benyújtott hamis hibakeresési. Lásd: a dokumentum egy példát a WordCountTopology.java kódot. 

Log4j naplózásának konfigurálásával kapcsolatos további tudnivalókért olvassa el a [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html)című témakört.

> [AZURE.NOTE] 0.10.0 használja Log4j vihar verzió 2.x. Vihar régebbi verzióit használják Log4j 1.x alapértelmezettől eltérő napló konfigurációhoz használt. Régebbi konfigurációja tudnivalókért olvassa el a [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat)című témakört.

##<a name="test-the-topology-locally"></a>Tesztelje a topológia helyi meghajtóra

Miután menti a fájlokat, a következő paranccsal tesztelje a topológia helyi meghajtóra.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Futtatja, a topológia indítási információkat jelenít meg. Akkor a következőhöz hasonló a sorok megjelenítéséhez, mondat a spout által kibocsátott és a csapszegek által feldolgozott kezdődik.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

A naplózás a WordCount rögzített által kibocsátott megnézve azt láthatja, hogy "és" 113 hányszor volt kibocsátott. A count továbbra is a feljebb szeretne lépni, amíg a topológia fut, mert a spout folyamatosan ugyanazt a mondatok bocsát ki.

Is lesz egy 5 második intervallum szavak kibocsátási és megszámolja között. Ennek oka az, hogy az adatok csak létrehozása, amikor egy osztásjelek sor bármelyik eleme megérkezik, és kéri, hogy ilyen kockára csak érkeznek 5 másodpercenként alapértelmezés szerint a __WordCount__ összetevő van beállítva.

## <a name="convert-the-topology-to-flux"></a>A topológia átalakítása neutronáramú

Neutronáramú egy új keretrendszer vihar 0.10.0, amely lehetővé teszi, hogy külön végrehajtása konfiguráció számára érhető el. A összetevők (Csapszegek és spouts,) továbbra is Java definiálva, de a topológia definiált YAML fájl használatával.

A YAML fájlt az összetevők, a topológia használandó határozza meg, hogyan átfolyása az adatok őket, és milyen értékeket kell használni az összetevők inicializálásakor között. A projekt tartalmazó, ha azokat, vagy használhat egy külső YAML fájlt, a topológia indításakor üveg fájl részeként YAML fájl tartalmazhat.

1. Helyezze át a projekt ki a __WordCountTopology.java__ fájlt. A korábban a meghatározás, a topológia, de azt nem használja az neutronáramú.

2. Az __erőforrások__ címtárban __topology.yaml__nevű új fájl létrehozása. A következő használja a fájl tartalmát.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Szánjon egy kis időt, olvassa el a, és mit jelent az egyes szakaszait, és hogyan kapcsolódik a __WordCountTopology.java__ fájl Java-alapú definition ismertetése.

3. A következő módosításokat az __pom.xml__ fájlt.

    * A következő új függés hozzáadása a `<dependencies>` szakasz:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Adja hozzá a következő beépülő modult, hogy a `<plugins>` szakaszban. A beépülő modul egy csomag (üveg fájl) beállítása a projekthez kibocsátása kezeli, és bizonyos adott átalakítások vonatkozik neutronáramú a csomag létrehozásakor.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

    * A __végrehajtási-maven tesztelése-bővítmény__ a `<configuration>` szakaszban, sorszámának megadása `<mainClass>` való `org.apache.storm.flux.Flux`. Ebben a csoportban adhatja neutronáramú kezeléséhez, a topológia futása azt futtassa helyileg fejlesztésében.

    * Az a `<resources>` szakaszt, és adja meg az alábbi módon a `<includes>`. Ide tartoznak a YAML fájlt, amely meghatározza a topológia a projekt részeként.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Tesztelje a neutronáramú topológia helyi meghajtóra

1. A következő használatával állíthat össze, és hajtsa végre a neutronáramú topológia maven tesztelése használatával.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Ha a PowerShell használata esetén használja a következő:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Ha egy Linux/Unix/OS X rendszeren, és [a fejlesztői környezet a vihar telepítve](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html)van, az alábbi parancsok inkább is használhatja:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    A `--local` paramétert a topológia módban fut helyi a fejlesztői környezet. A `-R /topology.yaml` paraméter használja a `topology.yaml` fájl erőforrás megadásához a topológia üveg fájlból.

    Futtatja, a topológia indítási információkat jelenít meg. Akkor a következőhöz hasonló a sorok megjelenítéséhez, mondat a spout által kibocsátott és a csapszegek által feldolgozott kezdődik.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    Lesznek 10 második késleltetést kötegenként naplózott adatok között a `topology.yaml` fájl továbbítja az értéket `10` a WordCount összetevő létrehozásakor. Ez az osztásjelek sor bármelyik eleme késleltetés intervallumát 10 másodperc állítja.

2.  Másolat készítése a `topology.yaml` projektből fájlt. Hívás hasonló `newtopology.yaml`. A fájlban, keresse meg a következő szakaszt, és értékének módosítása `10` való `5`. Ez a művelet az 5-ös 10 másodperc szószámok kibocsátás kötegenként közötti időtartamot.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. A topológia futtatásához használja a következő parancsot:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Vagy ha vihar a a Linux/Unix/OS X fejlesztői környezet:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Módosítása a `/path/to/newtopology.yaml` elérési útját az előző lépésben létrehozott newtopology.yaml fájlt. Ez a parancs a newtopology.yaml használja a topológia-definíciót. Mivel azt nem tartalmaz a `compile` paraméter maven tesztelése felhasználja a projektet egy előző lépésekben verzióját.

    Miután a topológia elindul, meg kell figyelje meg, hogy a kibocsátott kötegekben között eltelt idő newtopology.yaml értéke megfelelően megváltozott. Úgy láthatja, hogy módosíthatja a konfigurációs fájl YAML keresztül anélkül, hogy a topológia fordítani.

Vannak olyan, hogy neutronáramú biztosít, amelyek nem az itt tárgyalt, például a paraméterek alapján YAML fájlban változó helyettesítő átadott futásidőben vagy környezeti változók számos más szolgáltatást. Ezek és más funkciók neutronáramú keretrendszer további tudnivalókért lásd: [neutronáramú (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident a magas szintű hardverabsztrakciós vihar által biztosított. Állapot-nyilvántartó feldolgozás támogat. Az elsődleges Trident előnye, hogy azt is garantálni, hogy beírja a topológia üzenetek csak egyszer dolgozza fel. Ez a nehéz nyers Java topológiát, mely garantálja a cég üzenetek legalább egyszer dolgozható elérése érdekében. Is további különbség, például a beépített összetevők csapszegek helyett használható. Erre valójában csapszegek teljesen kevésbé általános összetevők, például a szűrőket, előrejelzések és funkciók helyett.

Trident alkalmazások maven tesztelése projektek használatával hozhat létre. Ez a cikk korábbi részében bemutatott használható az azonos alapvető lépések – csak a különbség a kódot. Trident is nem (jelenleg) használható neutronáramú keretében.

Trident kapcsolatos további tudnivalókért olvassa el a <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API – áttekintés</a>című témakört.

Egy példa Trident alkalmazás című témakörben [Twitter trendfigyelésre témakörök a Apache vihar a hdinsight szolgáltatásból lehetőségre](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Következő lépések

Hogyan hozhat létre egy vihar topológia Java használatával megtanulta azt. Most megtudhatja, hogy miként:

* [Üzembe helyezéséhez és kezeléséhez Apache vihar topológiák a hdinsight szolgáltatáshoz](hdinsight-storm-deploy-monitor-topology.md)

* [Fejleszthet olyan C# topológiák Apache vihar a Visual Studio segítségével hdinsight szolgáltatáshoz](hdinsight-storm-develop-csharp-visual-studio-topology.md)

[Példa topológiák vihar a HDInsight-](hdinsight-storm-example-topology.md)felkeresésével további példa vihar topológiák is megkeresheti.
