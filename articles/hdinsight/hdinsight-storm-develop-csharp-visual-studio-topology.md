<properties
   pageTitle="A Visual Studio és C# Apache vihar topológiák |} Microsoft Azure"
   description="Megtudhatja, hogyan hozhat létre vihar topológiák C#: hozzon létre egy egyszerű word darab topológia a Visual Studióban for Visual Studio HDInsight eszközeivel."
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
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Fejleszthet olyan C# topológiák Apache vihar a HDInsight használatával Hadoop tools for Visual Studio

Megtudhatja, hogy miként hozhat létre, a C# vihar topológiában for Visual Studio HDInsight eszközeivel. Ebben az oktatóanyagban végigvezeti a folyamaton, vihar új projekt létrehozása a Visual Studióban, helyileg tesztelés és üzembe helyezése a HDInsight fürthöz egy Apache vihar.

Emellett megismerheti, hogyan hozhat létre, amelyek C# és -összetevők Java hibrid topológiák.

> [AZURE.IMPORTANT] A dokumentumban a lépéseket a Windows fejlesztői környezet a Visual Studio olyan van szüksége, miközben a lefordított projekt elküldhetők Linux vagy Windows-alapú HDInsight fürthöz. Csak Linux-alapú fürt után létrehozott 10/28/2016 támogatási SCP.NET topológiák.
>
> Linux-alapú fürt C# topológia használatához frissítenie kell a Microsoft.SCP.Net.SDK NuGet csomag verzióra 0.10.0.6 a projekt által használt vagy újabb verziójában. A csomag verziójának is egyeznie kell a főverzióját vihar telepítve van a hdinsight szolgáltatásból lehetőségre. Ha például a 3.3 és 3.4 HDInsight verzióján vihar vihar verzióját használja, míg a HDInsight 3.5-ös verzióját használja vihar 0.10.x 1.0.x.
> 
> A Linux-alapú fürt topológiák C# kell .NET 4.5 használja, és egyszínű használja a HDInsight fürt futtatásához. Elérhető összetevők többsége fog működni, azonban nézze meg az esetleges kompatibilitási problémák [Egyszínű kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) kívánt dokumentumot.

## <a name="prerequisites"></a>Előfeltételek

- Az alábbi verziókra Visual Studio

    - Visual Studio 2012 a [4-es frissítése](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [frissítése a 4-es](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Közösség](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 vagy [Visual Studio 2015-Közösség](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 vagy újabb verzió

- HDInsight Tools for Visual Studio: lásd a [HDInsight Tools for Visual Studio használatának első lépései](hdinsight-hadoop-visual-studio-tools-get-started.md) telepítse és állítsa be a HDInsight tools for Visual Studio.

    > [AZURE.NOTE] HDInsight Tools for Visual Studio nem támogatottak a Visual Studio Express

-   A HDInsight fürthöz Apache vihar: [első lépések a HDInsight Apache vihar](hdinsight-apache-storm-tutorial-get-started.md) a lépések fürt létrehozásához.

## <a name="templates"></a>Sablonok

A HDInsight Tools for Visual Studio adja meg az alábbi sablonok:

| Projekt típusa | Bemutató |
| ------------ | ------------- |
| Vihar alkalmazás | Egy üres vihar topológia projekten |
| Az SQL Azure-író minta vihar | Hogyan kell írni az Azure SQL-adatbázishoz |
| Vihar DocumentDB olvasó minta | Azure DocumentDB beolvasása |
| Vihar DocumentDB író minta | Hogyan kell írni az Azure DocumentDB |
| Vihar EventHub olvasó minta | Azure esemény hubok beolvasása |
| Vihar EventHub író minta | Hogyan kell írni az Azure esemény hubok |
| Vihar HBase olvasó minta | A HDInsight fürt HBase beolvasása |
| Vihar HBase író minta | Hogyan kell írni a HDInsight fürt HBase |
| Vihar hibrid minta | Java összetevő használata |
| Vihar minta | Egy egyszerű word darab topológiája |

> [AZURE.NOTE] A HBase olvasó és író minták a HBase REST API segítségével HDInsight fürtre, nem a HBase Java API-HBase kommunikálni.

A lépéseket a dokumentumban, az egyszerű vihar alkalmazás projekt típusát hozhat létre egy új topológiája fogja használni.

## <a name="create-a-c-topology"></a>Hozzon létre egy C# topológiája

1. Ha, még nem telepítette a HDInsight-eszközök a legújabb Visual Studio, olvassa el a [HDInsight Tools for Visual Studio használatának első lépései](hdinsight-hadoop-visual-studio-tools-get-started.md)című témakört.

2. Nyissa meg a Visual Studióban, és válassza a **fájl** > **Új**, majd a **Projekt**.

3. Az **Új projekt** képernyőn bontsa ki a **telepítendő** > **sablonok**és választó **hdinsight szolgáltatásból lehetőségre**. A sablonok listából válassza ki a **Vihar alkalmazást**. A képernyő alján írja be az alkalmazás neve **WordCount** .

    ![kép](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. A projekt létrehozását követően rendelkeznie kell a következő fájlokat:

    - **Program.cs**: a topológia beállítása a projekthez határozza meg. Figyelje meg, hogy alapértelmezés szerint egy alapértelmezett topológia, amely egy spout és egy rögzített áll jön létre.

    - **Spout.cs**:-például spout, amely a véletlen számok bocsát ki.

    - **Bolt.cs**:-például rögzített őrzi meg a spout által kibocsátott számok számának.

    Projekt létrehozása részeként a legújabb [SCP.NET csomagok](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) tölti le a NuGet.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

A következő szakaszokban a projekt módosítja egy egyszerű WordCount alkalmazásba.

### <a name="implement-the-spout"></a>A spout megvalósítása

1. Nyissa meg a **Spout.cs**. Külső forrásból származó adatok topológiában elolvasására spouts szolgálnak. A fő összetevők egy spout a következők:

    - **NextTuple**: vihar hívja meg, amikor új kockára elhelyezése a spout engedélyezve van.

    - **Nyomon követés** (csak a tranzakció alapú topológia): a topológia az elküldött e spout kockára más összetevőket által kezdeményezett nyugták kezeli. Egy sor bármelyik eleme FELISMERVE lehetővé teszi, hogy tudja, hogy azt feldolgozása sikeresen megtörtént által feldolgozott összetevőket a spout.

    - **Nem sikerül** (csak a tranzakció alapú topológia): kockára, amely vannak fail feldolgozási más összetevőket a topológia kezeli. Ezzel a megoldással újra elhelyezése a sor bármelyik eleme, úgy, hogy újra feldolgozott lehetőséget.

2. A **Spout** osztály tartalmának lecserélése a következőre. Ezzel létrehoz egy spout, amely véletlenszerűen bocsát ki a mondatok a topológia be.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Szánjon egy kis időt olvassa el a Mit jelent ez a kód megérteni a megjegyzéseket.

### <a name="implement-the-bolts"></a>A csapszegek megvalósítása

1. A meglévő **Bolt.cs** fájl törlése a projekt.

2. A **Megoldás Explorer**, kattintson a jobb gombbal a projektet, és válassza a **Hozzáadás** > **új elemet**. A listából jelölje be a **Rögzített vihar**, és adja meg a **Splitter.cs** neveként. Ismételje meg ezt a második rögzített závárzatú elnevezett **Counter.cs**létrehozásához.

    - **Splitter.cs**: egy mondatot kettéválaszthatja-e az egyes szavakat, és bocsát ki egy új adatfolyam szavak vég hajtja végre.

    - **Counter.cs**: egy rögzített, amely minden szó megszámolja és bocsát ki egy új adatfolyam szavak és az egyes szavak számának hajtja végre.

    > [AZURE.NOTE] Ezeket a csapszegek egyszerűen írási és olvasási adatfolyamok, de egy vég forrásokhoz, például egy adatbázist, vagy a szolgáltatás kommunikáció is használhatja.

3. Nyissa meg a **Splitter.cs**. Megjegyzés:, hogy rendelkezik-e a módszer csak egy alapértelmezés szerint: **végrehajtás**. Ha a rögzített kap egy sor bármelyik eleme feldolgozásra Link. Ebben az esetben a következőképpen olvasható folyamat kockára bejövő és kimenő kockára elhelyezése.

4. Az **elválasztó** osztály tartalmának cserélje ki a következő kódot:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Szánjon egy kis időt olvassa el a Mit jelent ez a kód megérteni a megjegyzéseket.

5. Nyissa meg a **Counter.cs** , és az osztályjegyzetfüzet tartalma lecserélése a következőre:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Szánjon egy kis időt olvassa el a Mit jelent ez a kód megérteni a megjegyzéseket.

### <a name="define-the-topology"></a>A topológia meghatározása

Spouts és csapszegek diagramját, amely meghatározza, hogy hogyan átfolyása az adatokat az összetevők között vannak rendezve. Ez a topológia a diagram esetében az alábbi képlettel történik:

![hogyan vannak rendezve az összetevők képe](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

A mondatok a spout, a vannak kibocsátott, az elválasztó rögzített előfordulását szállítandó. Az elválasztó rögzített a mondatok oszlik a szavakat, amelyek a számláló rögzített való oszlanak.

A szavak száma a számláló példány helyileg tárolt, mert szeretnénk győződjön meg arról, hogy adott szavakat flow ugyanazon számláló rögzített példányára, így egy adott szót szerinti nyomon követést csak egy példánya van. De az elválasztó rögzített, az valójában mindegy, melyik rögzített kap mely mondatot, így egyszerűen szeretnénk egyensúly mondatok betöltése azokban az esetekben keresztül.

Nyissa meg a **Program.cs**. A fontos **GetTopologyBuilder**, a topológia beküldött meghatározásához vihar használt található. **GetTopologyBuilder** tartalmának cserélje ki a korábban leírt topológia végrehajtásához a következő kódot:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Szánjon egy kis időt olvassa el a Mit jelent ez a kód megérteni a megjegyzéseket.

## <a name="submit-the-topology"></a>A topológia elküldése

1. A **Megoldás Explorer**kattintson a jobb gombbal a projektet, és válassza a **Küldés gombot a HDInsight vihar**.

    > [AZURE.NOTE] Ha a rendszer kéri, írja be a bejelentkezési adatok Azure előfizetését. Ha egynél több előfizetése van, jelentkezzen be az adott, amely tartalmazza a vihar HDInsight fürt.

2. Jelölje ki a vihar HDInsight fürt a **Vihar fürt** legördülő listában, és válassza a **Küldés gombra**. Ha a beküldött sikeres a **kimeneti** ablakban figyelheti.

3. Amikor a topológia sikeres elküldését, a fürt **Vihar topológiák** jelenjenek meg. Jelölje ki a listában a futó topológia kapcsolatos információk megtekintése a **WordCount** topológiában.

    > [AZURE.NOTE] Is megtekintheti, **Vihar topológiák** **Kiszolgáló**Intézőből: **Azure**kibontása > **HDInsight**, kattintson a jobb gombbal egy vihar HDInsight fürt, és válassza a **Nézet vihar topológiák**.

    A hivatkozások a spouts vagy csapszegek használatával ezeket az összetevőket kapcsolatos információk megtekintése. Egyes kijelölt elemek új ablakban fog megnyílni.

4. Kattintson a **Topológia összegzés** nézet **törlése** le szeretné állítani a topológia.

    > [AZURE.NOTE] Vihar topológiák tovább futnak mindaddig, amíg vannak inaktiválták őket, vagy a fürt törlődik.

## <a name="transactional-topology"></a>Tranzakció alapú topológiája

Az előző topológia nem tranzakció alapú. A topológia összetevőit valósítja bármely funkciók ismétlése üzeneteket, ha feldolgozása nem sikerül egy olyan összetevővel a topológiában. Új projekt létrehozása egy példa tranzakció alapú topológia, és jelölje be a projekt típusa **Vihar minta** .

Tranzakció alapú topológiák az alábbi módon támogatja az adatok ismétlés végrehajtása:

- **Metaadatok gyorsítótárazása**: A spout kell tárolni az adatokat, hogy az adatok beolvasott, és ha hiba lép fel újra kibocsátott kibocsátott metaadatait. Mivel az adatokat a minta által kibocsátott kicsi, minden egyes sor bármelyik eleme nyers adatait egy szótár ismétléses vannak tárolva.

- **Nyomon követés**: a topológia minden rögzített felhívhatja `this.ctx.Ack(tuple)` ack, hogy sikeresen van-e egy sor bármelyik eleme feldolgozott. Esetén minden csapszegek korrektúrák sor bármelyik eleme, a `Ack` meghívott a spout metódusát. Ezzel a spout lehet eltávolítani az ismétlés gyorsítótárazott adatokat, mivel az adatok teljesen feldolgozása.

- **Nem sikerül**: minden rögzített felhívhatja `this.ctx.Fail(tuple)` jelzi, hogy feldolgozás rekordhoz sikertelen volt. A hiba propagálás a a `Fail` a spout, ahol a sor bármelyik eleme lehet újból használatával metódusát gyorsítótárazott metaadatok.

- **Sorozat azonosító**: vezérlés egy sor bármelyik eleme, ha a sorrend azonosítója adható meg. Egy érték, amely azonosítja a sor bármelyik eleme (Ack és Fail) ismétléses feldolgozásra kell. Például a spout a **Vihar minta** projekt használja az alábbi adatokat vezérlés:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Ez a bocsát ki egy új rekord, amely tartalmazza az alapértelmezett adatfolyam egy mondatban a sorozat ID értékkel **lastSeqId**található. Ebben a példában a minden sor bármelyik eleme kibocsátott **lastSeqId** egyszerűen nő.

Ahogyan az a **Vihar minta** projekt, hogy összetevő tranzakció alapú-e lehet futásidőben beállítása konfigurációjának.

## <a name="hybrid-topology"></a>Hibrid topológiája

HDInsight tools for Visual Studio is használható hibrid topológiák, ahol bizonyos összetevői C# és más alkalmazásokat Java létrehozásához.

Egy példa hibrid topológia új projekt létrehozása, és jelölje ki a **Vihar hibrid minta**. Ezzel kapcsolatot hozott létre, amely tartalmazza, amelyek bemutatják a következő több topológiák teljesen megjegyzésként minta:

- **Java spout** és **C# rögzített**: **HybridTopology_javaSpout_csharpBolt** meghatározott

    - A könyvjelzőnév tranzakció alapú verziót **HybridTopologyTx_javaSpout_csharpBolt**

- **C# spout** és **Java rögzített**: **HybridTopology_csharpSpout_javaBolt** meghatározott

    - A könyvjelzőnév tranzakció alapú verziót **HybridTopologyTx_csharpSpout_javaBolt**

        > [AZURE.NOTE] Ez a verzió is bemutatja, hogyan lehet Clojure kód használata szövegfájlból Java-összetevő.

A projekt elküldésekor használt a topológia közötti váltáshoz egyszerűen helyezze át a `[Active(true)]` a topológia, mielőtt elküldené a fürthöz használni kívánt utasítást.

> [AZURE.NOTE] Szükséges összes Java-fájl részeként ehhez a projekthez a **JavaDependency** mappában találhatók.

Vegye figyelembe a következőket hozhat létre, és egy hibrid topológiája elküldése:

- Hozzon létre egy új példányát az Java osztály spout vagy rögzített **JavaComponentConstructor** kell használható.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** használandó szerializálni adatok a JSON Java objektumból Java összetevők ki.

- A kiszolgáló a topológiát elküldésekor a **Java-fájl elérési utak**megadása kell használnia a **További beállítások** lehetőséget. A megadott elérési út Java órájához tartalmazó üveg fájlokat tartalmazó könyvtár kell lennie.

### <a name="azure-event-hubs"></a>Azure esemény hubok

SCP.Net verzió 0.9.4.203 vezet be egy új osztály és módszer kifejezetten használata a rendezvény központi Spout (egy Java spout, amely beolvassa az esemény központból.) A topológia, amely használja ezt a spout létrehozásakor tegye az alábbiakat:

- **EventHubSpoutConfig** osztály: hoz létre egy objektumot, amely tartalmazza az adatokat a spout-összetevő

- **TopologyBuilder.SetEventHubSpout** módszer: a topológia hozzáadja az esemény központi Spout összetevőt

> [AZURE.NOTE] Ezek megkönnyítése használata a rendezvény központi Spout más Java-összetevő-nél, miközben továbbra is használja a CustomizedInteropJSONSerializer szerializálni a spout készített adatok.

## <a id="configurationmanager"></a>ConfigurationManager használata

ConfigurationManager mellőzése amely értékre a rögzített és spout elemeket. Ez egy null mutató kivétel vezet. Ehelyett a konfiguráció beállítása a projekthez átadott be a vihar topológia kulcs/érték párban topológia környezetében. Konfigurációs értékek használna, minden összetevő kell beolvashatja őket a környezetből inicializálni során.

A következő kódot bemutatja, hogyan lehet beolvasni a következő értékeket:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Ha egy `Get` módszer való visszatéréshez egy példányát a összetevő, gondoskodnia kell arról, hogy mindkét továbbítja a `Context` és `Dictionary<string, Object>` a konstruktor paramétereit. Az alábbi példa az alap `Get` megadott megfelelően átadja a következő értékeket:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>SCP.NET frissítése

Legutóbbi verzióitól SCP.NET támogatja a csomag frissítés NuGet keresztül. Ha egy új frissítés érhető el, egy frissítésről tájékoztató értesítést kap. Ha manuálisan szeretné ellenőrizni a frissítést, hajtsa végre ezeket a lépéseket:

1. A **Megoldás Explorer**kattintson a jobb gombbal a projektet, és válassza a **NuGet csomagok kezelése**.

2. A csomag kezelőjétől válassza a **frissítések**. Ha a frissítés érhető el, akkor lesz látható. A **frissítés** gombra kattintva telepítheti a csomagnak.

> [AZURE.IMPORTANT] Ha a projekt egy csomag frissítések NuGet nem használó SCP.NET korábbi verzióiban készült, frissítse az új verzió az alábbi lépéseket kell elvégeznie:
>
> 1. A **Megoldás Explorer**kattintson a jobb gombbal a projektet, és válassza a **NuGet csomagok kezelése**.
> 2. A **Keresés** mezőben keres, és majd hozzáadásához **Microsoft.SCP.Net.SDK** a projekthez.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="null-pointer-exceptions"></a>NULL mutató kivételek

A C# topológiában egy Linux-alapú HDInsight fürthöz használatakor szög, és összetevők, olvassa el a konfigurációs beállítások futásidőben esetleg null mutató kivételek ConfigurationManager használó spout. Ez akkor fordul elő, mivel az adatokat a betöltött tartomány nem az az összeállítás a projektet tartalmazó.

A konfiguráció beállítása a projekthez a topológia környezetben kulcs/érték párban a vihar topológia átadott van, és a szótár objektumból, amikor a program kitölti értékekkel a összetevők átadott tudja visszaszerezni.

Az alábbi példa bemutatja a konfigurációs értékek betöltése a topológia környezetből, olvassa el ezt a dokumentumot [ConfigurationManager](#configurationmanager) szakaszát.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

C# topológiája egy Linux-alapú HDInsight fürthöz használatakor előfordulhatnak a következőt:

    System.TypeLoadException: Failure has occurred while loading a type.

Ez általában következik be, bináris használata esetén ez nem kompatibilis a .NET, amely támogatja az egyszínű.

A HDInsight Linux-alapú fürt gondoskodnia kell arról, hogy a projekt használja-e a .NET 4.5 lefordított bináris.


### <a name="test-a-topology-locally"></a>Tesztelje a topológia helyi meghajtóra

Bár egyszerűen a topológia üzembe fürtre, egyes esetekben előfordulhat, tesztelje a topológia helyi meghajtóra. Kövesse az alábbi lépéseket futtatásához, és tesztelje a példa topológia ebben az oktatóanyagban a fejlesztői környezet a helyi meghajtóra.

> [AZURE.WARNING] Helyi tesztelése csak akkor működik, az egyszerű, C# csak topológiák. Helyi vizsgálata hibrid topológiák vagy több adatfolyamok, topológiák hibákat fog kapni, ne használja.

1. A **Megoldás Explorer**kattintson a jobb gombbal a projektet, és válassza a **Tulajdonságok parancsot**. A projekt tulajdonságait módosítsa a **kimeneti típus** **Konzol**alkalmazás.

    ![kimeneti típus](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Ne feledje, hogy a **kimeneti típus** módosítása **Osztály** tárban, mielőtt beállítaná a topológia fürt.

2. A **Megoldás Explorer**, kattintson a jobb gombbal a projektet, majd válassza a **Hozzáadás** > **Új elemet**. Jelölje ki az **Osztályjegyzetfüzet** , és írja be a **LocalTest.cs** osztály neveként. Végül kattintson a **Hozzáadás**gombra.

3. Nyissa meg a **LocalTest.cs** , és adja hozzá a következő **használatával** utasítás tetején:

        using Microsoft.SCP;

4. Használható az **LocalTest** osztály tartalmát a következő:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Szánjon egy kis időt olvassa el a kód megjegyzéseket. Kód **LocalContext** segítségével a fejlesztői környezet összetevők futtatása, és azt továbbra is fennáll a adatfolyam háttérösszetevőre, hogy a helyi meghajtón szövegfájlokból között.

5. Nyissa meg a **Program.cs** , és adja hozzá a **fő** módszer a következőket:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Mentse a módosításokat, majd kattintson az **F5 billentyűt** , vagy jelölje be a **hibakeresési** > **Hibakeresési indítsa el** a projekt indításához. Egy console ablak kell helyezni, és jelentkezzen állapot vizsgálatok előrehaladását. **Végzett vizsgálatot** megjelenése után nyomja le az ENTER tetszőleges billentyűt lenyomva zárja be az ablakot.

7. A **Windows Intézőben** keresse meg a könyvtár, például a projektet tartalmazó **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Ezt a címtárat nyissa meg a **elemre**, és válassza a **hibakeresési**. Meg kell jelennie a fájlok, amelyeket a tesztek futtatásakor: sentences.txt counter.txt és splitter.txt. Minden szöveg fájl megnyitása, és nézze meg az adatokat.

    > [AZURE.NOTE] Ezeket a fájlokat a decimális érték tömbként karakterlánc típusú adatok megőrződnek. Ha például \[[97,103,111]] a **splitter.txt** fájl a word "és".

Bár a tesztelés egy egyszerű szót tartalmazó cellákat számlálnia helyileg alkalmazása meglehetősen trivial, a tényleges érték származik, amikor egy összetett topológia, amely a külső adatforrások kommunikál, vagy hajt végre komplex adatelemzés. Ha egy projekten dolgozik, akkor előfordulhat, hogy kell beállítania töréspontok és lépés a kód a összetevők kapcsolatos problémák azonosítása.

> [AZURE.NOTE] Győződjön meg arról, értékre a **Projekt típusa** **Osztálytár** egy vihar HDInsight fürt mielőtt.

### <a name="log-information"></a>Naplóadatok

Akkor is egyszerűen származó adatok naplózása a topológia összetevők használatával `Context.Logger`. Ha például a következő hoz létre egy tájékoztató naplóbejegyzés:

    Context.Logger.Info("Component started");

A naplózott adatok megtekintheti a **Hadoop szolgáltatás naplófájl**, amely megtalálható a **Kiszolgáló Explorer**. Bontsa ki a bejegyzést a vihar HDInsight fürt számára, majd bontsa ki a **Hadoop szolgáltatás naplója**. Végül jelölje be a naplófájl megtekintéséhez.

> [AZURE.NOTE] A naplók tárolja a fürt által használt Azure tárterület-fiókjában. Ha egy másik előfizetést, mint a jelentkezett a Visual Studio, jelentkezzen be az előfizetést, az Ez az információ megtekintése tárterület-fiókot tartalmazó szeretne.

### <a name="view-error-information"></a>Hibaüzenet adatainak megtekintése

A futó topológiában előfordult hibák megtekintéséhez kövesse az alábbi lépéseket:

1. A **Kiszolgáló Intéző**kattintson a jobb gombbal a vihar HDInsight fürt, és válassza a **Nézet vihar topológiák**.

2. **Spout** és **Bolts**az **Utolsó hiba** oszlop lesz az utolsó hiba, hogy rendelkezik-e olvashat.

3. Jelölje ki a **Spout** vagy **Rögzített azonosítója** a felsorolt hibás összetevőhöz. A részletek lapon megjelenő további információ a hibáról szerepelni fognak a lap alján a **hibák** című részére.

4. További információkhoz juthat, jelölje ki a rajzlapra a vihar dolgozó naplóban talál az utolsó néhány perc **végrendeleti végrehajtó** szakaszából olyan **portot** .

## <a name="next-steps"></a>Következő lépések

Most, hogy rendelkezik megtanulta kidolgozása üzembe helyezése a Visual Studio vihar topológiák a HDInsight csoportban, és megtudhatja, hogyan [esemény-központban Azure hdinsight szolgáltatáshoz a vihar a folyamat események](hdinsight-storm-develop-csharp-event-hub-topology.md)hogyan.

Példa egy C# topológia, amellyel adatfolyamok osztja több adatfolyam megjelenítését olvassa el a [C# vihar példa](https://github.com/Blackmist/csharp-storm-example)című témakört.

C# topológiák létrehozásával kapcsolatos további tudnivalók felderítésére, látogasson el a [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

További módszerek a hdinsight szolgáltatásból lehetőségre, és további vihar HDinsight mintákon talál a következő:

**A Microsoft SCP.NET**

* [SCP programozási útmutató](hdinsight-storm-scp-programming-guide.md)

**A HDInsight Apache vihar**

- [Üzembe helyezéséhez és a Apache vihar a HDInsight topológiák figyelése](hdinsight-storm-deploy-monitor-topology.md)

- [Példa a HDInsight vihar topológiát](hdinsight-storm-example-topology.md)

**A HDInsight Apache Hadoop**

- [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

- [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

- [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

**A HDInsight Apache HBase**

- [A HDInsight HBase – első lépések](hdinsight-hbase-tutorial-get-started.md)
