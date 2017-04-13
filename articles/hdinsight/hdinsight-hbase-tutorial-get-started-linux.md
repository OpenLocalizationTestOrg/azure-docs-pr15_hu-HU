<properties
    pageTitle="HBase oktatóprogram: első lépések a Hadoop Linux-alapú HBase fürt |} Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase oktatóprogram: elkezdeni használni a HDInsight Linux-alapú Hadoop Apache HBase 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Megtudhatja, hogy miként egy HBase fürthöz létrehozása a HDInsight HBase táblázatok létrehozása és lekérdezési táblák struktúra használatával. HBase kapcsolatos általános tudnivalókért [áttekintése]című témakörben találhat HDInsight HBase[hdinsight-hbase-overview].

A dokumentum adatai Linux-alapú HDInsight fürt alkalmazásra. Windows-alapú fürt olvashat a lap tetején lévő tabulátorválasztó használatával válthat.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban HBase megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- A [biztonságos Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Access-ellenőrzési követelmények

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>HBase fürt létrehozása

Az alábbi eljárás egy erőforrás-kezelő Azure-sablon segítségével verzió 3.4 Linux-alapú HBase fürt és a függő alapértelmezett Azure tárterület-fiók létrehozása. Az eljárás és más fürt létrehozási módjai használt paraméterek, című cikkben talál részletes [létrehozása Linux-alapú Hadoop fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-provision-linux-clusters.md).

1. Kattintson az alábbi képen a az Azure-portálon nyissa meg a sablont. A sablon egy nyilvános blob-tárolóban található. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Az **egyéni telepítési** lap az adja meg az alábbiakat:

    - **Előfizetés**: jelölje be a fürt létrehozásához használt Azure előfizetését.
    - **Erőforráscsoport**: hozzon létre egy új Azure az erőforrás-kezelés csoport vagy egy meglévőt használ.
    - **Hely**: Adja meg az erőforráscsoport helyét. 
    - **Fürtnév**: Adja meg a létrehozandó HBase fürt nevét.
    - **Fürt felhasználónév és jelszó**: az alapértelmezett bejelentkezési neve **felügyeleti**.
    - **SSH felhasználónév és jelszó**: az alapértelmezett felhasználónév szó **sshuser**.  Nevezze át.
     
    További paramétereket nem kötelező.  
    
    Minden csoport egy Azure Blob-tároló fiók függőség tartalmaz. Után fürt törléséhez az adatok marad, a tárterület-fiókjában. Fürt alapértelmezett tároló fiók neve "store" hozzáfűzi a fürt nevét. A sablon változói szakasz szoftveresen kötött.
        
3. Jelölje ki a **lehet elfogadja a fenti feltételek mellett**, és kattintson a **vásárlás**. Fürt létrehozása körülbelül 20 percet vesz igénybe.


>[AZURE.NOTE] Egy HBase fürthöz a rekord törlése után, létrehozhat egy másik HBase fürt a azonos alapértelmezett blob-tároló használatával. Az új fürt átveszi az eredeti fürt létrehozott HBase táblákat. Inkonzisztenciák elkerülése érdekében azt javasoljuk, hogy a HBase táblák tiltsa le a fürt törlése előtt.

## <a name="create-tables-and-insert-data"></a>Táblázatok létrehozása és adatok beillesztése

Csatlakozás HBase fürt és HBase rendszerhéj HBase táblázatok létrehozása, beszúrása és lekérdezés adatok SSH is használhatja. SSH használatával kapcsolatos további tudnivalókért lásd [Használata SSH a Linux-alapú Hadoop a HDInsight Linux rendszerhez, a Unix, vagy az OS X](hdinsight-hadoop-linux-use-ssh-unix.md) és [Használatának SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

A legtöbben az adatokat a táblázatos formátum jelenik meg:

![Táblázatos adatok HDInsight HBase][img-hbase-sample-data-tabular]

A HBase, amely egy BigTable végrehajtása ugyanazokat az adatokat néz ki:

![HDInsight HBase bigtable adatok][img-hbase-sample-data-bigtable]

Fog több értelme után a következő lépéseket.  


**A HBase rendszerhéj használata**

1. SSH futtassa a következő parancsot:

        hbase shell

4. Hozzon létre egy HBase kéthasábos családok:

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

    Ugyanezt az eredményt, a Keresés paranccsal, mivel csak egy sor jelenik meg.

    A HBase táblázat séma kapcsolatos további tudnivalókért lásd a [HBase séma Tervező – bevezetés][hbase-schema]. További HBase parancsok is elérhetők, a témakörben [Apache HBase útmutató][hbase-quick-start].

6. Kilépés a rendszerhéj

        exit



**Adatok betöltése tömeges a partnerek HBase táblázatba**

HBase többféle módszer áll rendelkezésére az adatok betöltése táblákba tartalmazza.  További tudnivalókért olvassa el a [tömeges betöltése](http://hbase.apache.org/book.html#arch.bulk.load)című témakört.


Egy nyilvános blob-tárolóhoz, egy adatfájl – minta feltöltötte *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Az adatokat tartalmazó fájl tartalmát a következő:

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

> [AZURE.NOTE] Ez az eljárás a partnerek HBase táblát, az utolsó eljárás létrehozott használja.

1. SSH, futtassa a következő parancsot az adatfájl StoreFiles és Dimporttsv.bulk.output által megadott relatív elérési utat a tár átalakítása:.  Ha HBase rendszerhéj, lépjen ki a kilépési parancs segítségével.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Futtassa az adatokat a HBase tábla /example/data/storeDataFileOutput feltölteni a következő parancsot:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Nyissa meg a HBase rendszerhéj, és a Keresés paranccsal a tartalom listában.



## <a name="use-hive-to-query-hbase"></a>Lekérdezés HBase struktúra használatával

A lekérdezés HBase táblában található adatok struktúra használatával. Ez a szakasz táblát hoz létre struktúra, megfelelteti a HBase táblázatot, és használja a lekérdezést a HBase táblázatban.

1. Nyissa meg a **gitt**, és csatlakozzon a fürthöz.  Olvassa el az előző eljárás útmutatót.
2. Nyissa meg a struktúra rendszerhéj.

       hive
3. Futtassa a következő HiveQL-struktúra táblázat, amely a HBase tábla létrehozása. Győződjön meg arról, hogy a mintatáblázat az oktatóprogram korábbi részében hivatkozik a HBase rendszerhéjával, ez az utasítás futtatása előtt létrehozott.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Futtassa a következő HiveQL a lekérdezést a HBase tábla:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>HBase REST API-k használata Curl használata

> [AZURE.NOTE] Használatakor Curl vagy bármely más többi kommunikáció a WebHCat, a felhasználónév és jelszó megadásával a HDInsight fürt rendszergazda hitelesítést végezni a kérelmeket. A csoport nevét is: az egységes erőforrás azonosító (URI) használja a kérelem küld a kiszolgáló részeként kell használnia.
>
> Ebben a szakaszban a parancsok **felhasználónév** cserélje ki a felhasználót, hogy a fürthöz hitelesítést végezni, és **jelszó** cserélje ki a felhasználói fiók jelszava. **CLUSTERNAME** cserélje le a csoport nevére.
>
> [Alapszintű hitelesítés](http://en.wikipedia.org/wiki/Basic_access_authentication)keresztül védett a REST API-t. Mindig készítsen kérések érdekében győződjön meg arról, hogy a hitelesítő adatok biztonságos elküldi a kiszolgáló biztonságos HTTP (HTTPS) használatával.

1. A parancssorból a következő parancs segítségével ellenőrizze, hogy tud csatlakozni a HDInsight fürthöz:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    A válasz, a következőhöz hasonló kell megjelennie:

        {"status":"ok","version":"v1"}

    Ez a parancs használt paraméterek a következők:

    * **-u** – a felhasználónevet és jelszót hitelesíti a kérést.
    * **-G** - jelzi, hogy a GET kérésekben.

2. A következő parancsot használja szeretné megjeleníteni a meglévő HBase táblázatokat:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. A következő parancs használatával hozzon létre egy új HBase táblázatot két oszlop család:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    A séma JSon formátumban megadva.

4. A következő parancsot használatával Beszúrás bizonyos adatok:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Meg kell base64 kódolása a d – Váltás a megadott értékeket.  Az a exmaple:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: személyes: neve
    - Sm9obiBEb2xl: János Dole

    [hamis billentyűs sor](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) beszúrása (kötegelt) érték több teszi lehetővé.

5. A következő parancsot használja az első sor:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

HBase többi kapcsolatos további tudnivalókért olvassa el a [Apache HBase útmutató](https://hbase.apache.org/book.html#_rest)című témakört.

## <a name="check-cluster-status"></a>Fürt állapotának ellenőrzése

A HDInsight HBase fürt figyelemmel kísérésére alkalmazásban a webhely felhasználói Felületet. A webhely felhasználói felületének használata esetén kérheti statisztika vagy a régiók információt.

SSH is használható helyi kérések, például a webes kérelmek a HDInsight fürthöz átjáró. A kérés majd átirányítja az erőforrás, mintha az csomóponton HDInsight központi volna származik. További tudnivalókért lásd: [Használata SSH a Linux-alapú Hadoop meg a Windows HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Egy SSH tunneling munkamenet létrehozásához**

1. Nyissa meg a **gitt**.  
2. Ha egy SSH kulcsot a létrehozási folyamat során a felhasználói fiók létrehozásakor, jelölje be a titkos kulcs használatához a fürthöz hitelesítés során a következő lépés kell végrehajtania:

    **Kategória**bontsa ki a **kapcsolat**, bontsa ki a **SSH**, és válassza a **hitelesítés**parancsra. Végül kattintson a **Tallózás gombra** , majd jelölje ki a .ppk fájlt, amely tartalmazza a titkos kulcs.

3. Kattintson a **kategória** **munkamenetből**gombra.
4. Egyszerű lista elemei közül a gitt munkamenet képernyő írja be az alábbi értékeket:

    - **Állomásnév**: a SSH címét a HDInsight server a Host name (vagy IP-cím) mezőbe. A SSH cím a csoport nevét, majd **-ssh.azurehdinsight.net**. Ha például *en_furtom nevű fürt-ssh.azurehdinsight.net*.
    - **Port**: 22. A ssh az elsődleges headnode portjához 22.  
5. A párbeszédpanel bal oldalán a **kategória** csoportban bontsa ki a **kapcsolatot**, bontsa ki a **SSH**, és kattintson a **alagutak**parancsra.
6. Adja meg az alábbi adatokat az beállításai SSH port átirányításának űrlap szabályozása:

    - **Forrásport** - továbbítani szeretné az ügyfél portjához. Ha például 9876.
    - **Dinamikus** - lehetővé teszi, hogy dinamikus SOCKS-proxy útválasztás.
7. A beállítások hozzáadása a **Hozzáadás** gombra.
8. Kattintson a **Nyissa meg** a párbeszédpanel alján SSH származó megnyitásához.
9. Amikor a rendszer kéri, jelentkezzen be a kiszolgáló-SSH fiókot használ. Ez egy SSH munkamenetet, és a alagutas engedélyezése.

**A Ambari használatával zookeepers teljes Tartománynevét megkeresése**

1. Tallózással keresse meg azt a https://<ClusterName>.azurehdinsight.net/.
2. Kétszer adja meg a fürt felhasználó fiók hitelesítő adatait.
3. Kattintson a bal oldali menü **zookeeper**.
4. Kattintson a három a **ZooKeeper kiszolgáló** hivatkozásra, a összesítő listából.
5. Másolja a **Hostname (állomásnév)**. Ha például zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Egy ügyfélprogram (Firefox) és fürt állapotának ellenőrzése**

1. Nyissa meg a Firefox böngészőt.
2. Kattintson a **Menü megnyitása** gombra.
3. Kattintson a **Beállítások**gombra.
4. Kattintson a **Speciális kategóriára**, kattintson a **hálózat**elemre, és kattintson a **Beállítások**lehetőséget.
5. Válassza a **manuális beállítást**.
6. Írja be az alábbi értékeket:

    - **A Host SOCKS**: localhost
    - **Port**: használhatja a gitt SSH használjanak konfigurált ugyanazt a portot.  Ha például 9876.
    - **ZOKNI v5**: (be van jelölve)
    - **Távoli DNS**: (be van jelölve)
7. Kattintson az **OK gombra** a módosítások mentéséhez.
8. Tallózással keresse meg azt a http://&lt;egy ZooKeeper a teljes Tartománynevét >: 60010/mesteralakzat-állapotát.

A magas elérhetősége fürtre találhatja meg az aktuális aktív HBase fő csomópont, amelyen a webhely felhasználói felületének mutató hivatkozást.

##<a name="delete-the-cluster"></a>A csoport törlése

Inkonzisztenciák elkerülése érdekében azt javasoljuk, hogy a HBase táblák tiltsa le a fürt törlése előtt.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Következő lépések

A HDInsight HBase oktatóprogram megtanulta, hogyan hozhat létre egy HBase fürthöz, és hogyan hozhat létre a táblák, és megtekintheti az adatokat az HBase rendszerhéj táblákat. Emellett megtanulta azt HBase táblában található adatok struktúra lekérdezés használatáról és a HBase C# REST API-k használata-HBase táblázat létrehozása és adatok beolvasása a táblából.

További tudnivalókért lásd:

- [HDInsight HBase áttekintése][hdinsight-hbase-overview]: HBase egy Apache, Megnyitás-forrást, NoSQL adatbázist, amelybe elérésű és erős konzisztencia semistructured strukturált és strukturálatlan adatokat nagy mennyiségű Hadoop-ra épülő.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
