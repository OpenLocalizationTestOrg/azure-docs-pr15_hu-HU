<properties
    pageTitle="HBase oktatóprogram: első lépések a Hadoop HBase |} Microsoft Azure"
    description="Kövesse a elkezdeni használni a HDInsight Hadoop Apache HBase HBase oktatóprogram. Táblák létrehozása az HBase rendszerhéj és a lekérdezés azokat struktúra használatával."
    keywords="Apache hbase, hbase, hbase felületén hbase oktatóprogram"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase oktatóprogram: Apache HBase használata a Windows-alapú Hadoop HDInsight az első lépések

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Megtudhatja, hogy miként HBase fürt létrehozása a HDInsight HBase táblázatok létrehozása és lekérdezés a táblák Apache struktúra használatával. HBase kapcsolatos általános tudnivalókért [áttekintése]című témakörben találhat HDInsight HBase[hdinsight-hbase-overview].

A dokumentum adatai a Windows-alapú HDInsight fürt konkrét. Windows-alapú fürt olvashat a lap tetején lévő tabulátorválasztó használatával válthat.

> [AZURE.NOTE] HBase (verzió 0.98.0) a Windows-alapú HDInsight csak akkor érhető el, HDInsight 3.1 fürt való használatra (Apache Hadoop és alapján fonal 2.4.0). Verzió információ [HDInsight által biztosított Hadoop fürt verziók újdonságai?][hdinsight-versions]

## <a name="before-you-begin"></a>Első lépések

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ebben az oktatóanyagban HBase megkezdése előtt a következőket kell rendelkeznie:

- **A Microsoft Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Visual Studio 2013-as vagy újabb **munkaállomás** : útmutatásért lásd: a [Visual Studio telepítése](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase fürt létrehozása

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Egy HBase fürthöz létrehozása az Azure portál használatával**

1. Jelentkezzen be az [Azure portál][azure-management-portal].
2. Kattintson az **Új** gombra, vagy **+** a bal felső sarkában, és kattintson a **adatok + Analytics**, **hdinsight szolgáltatásból lehetőségre**.
3. Írja be az alábbi értékeket:

    - **Fürt neve** - írjon be egy nevet a fürt azonosításához.
    - **Fürt típus** - választó **HBase**.
    - **Operációs rendszer** - választó **Windows**.  Fürt Linux-alapú HBase létrehozása című témakörben [HBase oktatóprogram: elkezdeni használni a HDInsight (Linux) Hadoop Apache HBase](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Verzió** – HBase verziójú adja.
    - **Előfizetés** - válassza az Azure előfizetés a fürt létrehozására használható.
    - **Erőforráscsoport** - hozzon létre egy új Azure erőforráscsoport, vagy jelöljön ki egy meglévőt. További információ a [Azure erőforrás-kezelő áttekintése](azure-resource-manager/resource-group-overview.md) című témakörben találhat.
    - **Hitelesítő adatok** - Windows-alapú fürt, létrehozhat egy fürt felhasználó (más néven HTTP-felhasználó, HTTP webes szolgáltatás felhasználó) és a távoli asztali felhasználó. **Távoli asztal engedélyezése** a távoli asztali felhasználói hitelesítő adatok hozzáadása gombra. A következő szakaszban RDP szükséges.
    - **Adatforrás** - hozzon létre egy új Azure tárterület-fiókot, vagy jelöljön ki egy meglévő Azure tárterület-fiókot a fürt használandó alapértelmezett fájlrendszer. Az alapértelmezett fiók tárhelye hol található a fürt hely határozza meg. Az alapértelmezett tárterület-fiók és a fürt kell közös keresse meg az azonos adatközpont.
    - **Csomópont árak rétegek** - jelölje ki a HBase fürt régió kiszolgálók száma

        > [AZURE.WARNING] A magas HBase szolgáltatások elérhetőségét, létre kell hoznia legalább tartalmazó fürt **három** csomópontot. Ezzel biztosíthatja, hogy egy csomópont megszakad, ha a HBase adatok régiók elérhető többi csomóponton.

        > Ha HBase vannak tanulási, mindig válassza az 1 értéket a fürt méretét, és törölje a a fürt minden használat csökkentheti a költség után.

    - **Választható beállítási** - konfigurálása Azure virtuális hálózat parancsprogram-műveletek konfigurálásához és további tárterület-fiókot.

4. Kattintson a **létrehozása**gombra.

>[AZURE.NOTE] Egy HBase fürthöz a rekord törlése után, létrehozhat egy másik HBase fürt alapértelmezett tároló ugyanazzal a fiókkal és a alapértelmezett blob-tárolóhoz. Az új fürt átveszi az eredeti fürt létrehozott HBase táblákat. Inkonzisztenciák elkerülése érdekében azt javasoljuk, hogy a HBase táblák tiltsa le a fürt törlése előtt.

## <a name="create-tables-and-insert-data"></a>Táblázatok létrehozása és adatok beillesztése

Jelenleg nincsenek két módon HBase eléréséhez. Ez a szakasz ismerteti azokat a HBase rendszerhéjával. A következő szakaszban bemutatja, hogy a .NET SDK használatával.

Legtöbben az adatok a táblázatos formátumban jelenik meg:

![Táblázatos adatok hdinsight-hbase][img-hbase-sample-data-tabular]

A HBase, amely egy BigTable végrehajtása ugyanazokat az adatokat néz ki:

![hdinsight hbase bigtable adatok][img-hbase-sample-data-bigtable]

Fog több értelme után a következő lépéseket.  

**A HBase rendszerhéj használata**

1. RDP segítségével csatlakozzon a HBase fürthöz a hdinsight szolgáltatásból lehetőségre. A RDP című cikkben olvashat [a HDInsight az Azure-portálon kezelése a Hadoop fürt][hdinsight-manage-portal].
2. A RDP-munkamenet belül kattintson a **Hadoop parancssori** parancsikont az asztalon.
3. Nyissa meg a HBase rendszerhéj:

        cd %HBASE_HOME%\bin
        hbase shell

4. Hozzon létre egy HBase két oszlop család:

        create 'Contacts', 'Personal', 'Office'
        list
5. Szúrja be az adatokat:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase rendszerhéj][img-hbase-shell]

6. Egyetlen sor első

        get 'Contacts', '1000'

    A beolvasott képet paranccsal, mert csak egy sor, látni fogja ugyanazt az eredményt.

    A Hbase táblázat séma kapcsolatos további tudnivalókért lásd a [HBase séma Tervező – bevezetés][hbase-schema]. További HBase parancsok is elérhetők, a témakörben [Apache HBase útmutató][hbase-quick-start].


6. Kilépés a rendszerhéj

        exit

**Adatok betöltése tömeges a partnerek HBase táblázatba**

HBase többféle módszer áll rendelkezésére az adatok betöltése táblákba tartalmazza. További tudnivalókért olvassa el a [tömeges betöltése](http://hbase.apache.org/book.html#arch.bulk.load)című témakört.


Egy nyilvános blob-tárolóhoz, egy adatfájl – minta feltöltötte wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Az adatokat tartalmazó fájl tartalmát a következő:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Hozzon létre egy szövegfájlt, és töltse fel a fájlt a saját tárterület-fiókjába, ha azt szeretné. A című témakör nyújt tájékoztatást [Töltse fel az adatok HDInsight Hadoop feladatok][hdinsight-upload-data].

> [AZURE.NOTE] Ez az eljárás a az utolsó eljárás létrehozott névjegyek HBase táblát használ.

1. A RDP-munkamenet belül kattintson a **Hadoop parancssori** parancsikont az asztalon.
2. A címtár módosítása:

        cd %HBASE_HOME%\bin

3. A következő parancsot átalakítása az adatfájl StoreFiles és Dimporttsv.bulk.output által megadott relatív elérési utat a tár:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Futtassa az adatokat a HBase tábla /example/data/storeDataFileOutput feltölteni a következő parancsot:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Nyissa meg a HBase rendszerhéj, és a Keresés paranccsal a tartalom listában.



## <a name="use-hive-to-query-hbase-tables"></a>Struktúra lekérdezési HBase táblázatok használata

A struktúra HBase tárolt adatok is lekérdezhetők. Ez a szakasz táblát hoz létre struktúra, megfelelteti a HBase táblázatot, és használja a lekérdezést a HBase táblázatban.

**A fürt irányítópult megnyitása**

1. Tallózással keresse meg **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Írja be a Hadoop felhasználói fiók felhasználónevét és jelszavát. Az alapértelmezett felhasználóneve **rendszergazdai** jelszó pedig a létrehozási folyamat során megadott. Ekkor megnyílik egy új böngészőlapon.
6. Kattintson a **Szerkesztő struktúra** , a lap tetején. A struktúra szerkesztő néz ki:

    ![Az irányítópult fürt hdinsight szolgáltatásból lehetőségre.][img-hdinsight-hbase-hive-editor]

**A struktúra lekérdezések futtatása**

1. Írja be a következő HiveQL parancsfájl szerkesztő struktúra, és kattintson a **Küldés gombra** a HBase tábla megfelelteti struktúra táblázatot szeretne létrehozni. Győződjön meg arról, hogy létrejönnek-e a mintatáblázat, ez az utasítás futtatása előtt HBase rendszerhéjával az oktatóprogram korábbi részében hivatkoznak.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Várja meg, addig, amíg **a **kitölteni**értesítéseket** .

2. Írja be a következő HiveQL parancsfájl szerkesztő struktúra, és kattintson a **Küldés**gombra. A struktúra lekérdezés szereplő adatokat a HBase kérdezi le:

        SELECT count(*) FROM hbasecontacts;

4. Olvassa be a struktúra lekérdezés eredményeit, kattintson a **Részletek** hivatkozásra a **Feladat munkamenet** ablakban a feladat befejezésekor futtatása. Lesznek csak egy feladat kimenő fájl, mert a HBase táblázatba helyezi el egy rekordot.




**A kimeneti fájl tallózásához.**

1. A lekérdezés konzolban kattintson a **Fájl-tallózó**elemet.
2. Kattintson a Azure tárterület-fiókot, amellyel az alapértelmezett fájlrendszer a HBase fürt.
3. Kattintson a HBase fürt nevére. Az alapértelmezett Azure tároló fiók tároló fürt nevét használja.
4. A **felhasználó**elemre, és kattintson a **rendszergazda**. (Ez a Hadoop felhasználónév.)
6. Kattintson a feladat nevére, és a **Legutóbbi módosítás** időpontja, amely megfelel a időpontot, amikor a struktúra VÁLASSZA a lekérdezés futtatásakor.
4. Kattintson a **stdout**. Mentse a fájlt, és nyissa meg a fájlt a Jegyzettömbben. Egy kimenő fájl lesz.

    ![HDInsight HBase struktúra szerkesztő fájl böngészőben][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Az ügyfél .NET HBase REST API-tárral

Le kell töltenie a HBase REST API-ügyfél tárat a .NET rendszerhez, a GitHub és hozza létre a projekt, így a HBase .NET SDK is használhatja. Az alábbi eljárás tartalmazza a tevékenységhez tartozó utasításokat.

1. Hozzon létre egy új C# Visual Studio Windows asztali konzol alkalmazást.
2. **Eszközök**gombra kattintva nyissa meg a NuGet csomag Manager konzolt > **NuGet csomag Manager** > **Csomag kezelője konzol**.
3. A következő NuGet parancsot a konzolban:

        Install-Package Microsoft.HBase.Client

5. Adja hozzá a következő **használata** kimutatásokban a fájl tetején:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Cserélje ki a **fő** függvény a következő:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Az első három változók beállíthatja a **fő** függvényt.
8. Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.

## <a name="check-cluster-status"></a>Fürt állapotának ellenőrzése

A HDInsight HBase fürt figyelemmel kísérésére alkalmazásban a webhely felhasználói Felületet. A webhely felhasználói felületének használata esetén kérheti statisztika vagy a régiók információt.

A webhely felhasználói felületének megnyitásához RDP be a fürt, és ezután kattintson a HMaster információ webes felületének parancsikont az asztalon vagy kell az alábbi URL-cím használata a böngészőben:

    http://zookeeper[0-2]:60010/master-status

A magas elérhetősége fürt hivatkozás az aktuális aktív HBase fő csomópontra, amelyen a webhely felhasználói felületének találhatók.

##<a name="delete-the-cluster"></a>A csoport törlése
Inkonzisztenciák elkerülése érdekében azt javasoljuk, hogy a HBase táblák tiltsa le a fürt törlése előtt.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Mi az következő?
A HDInsight HBase oktatóprogram megtanulta, hogyan hozhat létre egy HBase fürthöz, és hogyan hozhat létre a táblák, és megtekintheti az adatokat az HBase rendszerhéj táblákat. Emellett megtanulta, hogy hogyan lekérdezéssel struktúra HBase táblák és a HBase C# REST API-k használata-HBase táblázat létrehozása és adatok visszakeresése a táblázatot az adatok.

További tudnivalókért lásd:

- [HDInsight HBase áttekintése][hdinsight-hbase-overview].
HBase egy Apache, Megnyitás-forrást, NoSQL adatbázist, amelybe elérésű és erős konzisztencia semistructured strukturált és strukturálatlan adatokat nagy mennyiségű Hadoop-ra épülő.
- [Hozzon létre HBase fürt Azure virtuális hálózatot][hdinsight-hbase-provision-vnet].
Virtuális hálózati integrációval HBase fürt telepíthető az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül a HBase is kommunikálhatnak.
- [A HDInsight konfigurálása HBase replikációs](hdinsight-hbase-geo-replication.md). Megtudhatja, hogy miként HBase replikáció konfigurálása két Azure adatközpontokkal keresztül.
- [A HDInsight HBase a Twitteren sentiment elemzése][hbase-twitter-sentiment].
Megtudhatja, hogy miként végezze el a valós idejű [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) nagy adatok HDInsight a Hadoop fürt HBase használatával.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
