<properties
pageTitle="Windows-alapú hdinsight áttelepítése Linux-alapú HDInsight |} Azure"
description="Megtudhatja, hogy miként telepítheti át a Windows-alapú HDInsight fürtre Linux-alapú HDInsight fürtre."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>A Windows-alapú HDInsight fürtre áttelepítése Linux-alapú fürthöz

Windows-alapú HDInsight egyszerűvé Hadoop használni a felhőben, miközben azt tapasztalhatja, hogy szüksége van-e a Linux-alapú fürtre kihasználhatja az eszközök és a megoldás szükséges technológiák. A Hadoop ökológiai számos dolgot kifejlesztett Linux rendszerek, és néhány nem feltétlenül vehető igénybe Windows-alapú HDInsight való használatra. Ezenkívül sok könyvek, videókat és más tananyag feltételezik, hogy rendszert futtat egy Linux Hadoop-használatakor.

A dokumentum részletesen a Windows és Linux hdinsight szolgáltatásból lehetőségre, és hogy miként telepítheti át a meglévő munkaterhelésekből Linux-alapú fürtre útmutatást közötti különbségeket.

> [AZURE.NOTE] HDInsight fürt az operációs rendszer a csomópontok a fürt Ubuntu hosszú távú támogatási (LTS) használata. HDInsight, a rendelkezésre álló Ubuntu verzióját, és más összetevő verziószámozás információkat olvashat olvassa el a [HDInsight összetevő verzióival](hdinsight-component-versioning.md)foglalkozó.

## <a name="migration-tasks"></a>Áttelepítési műveletek

Az általános munkafolyamat áttelepítése a következőképpen történik.

![Áttelepítési munkafolyamat-diagramokhoz.](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Olvassa el a módosításokat, amelyeket szükség lehet a meglévő munkafolyamat, feladatok, stb áttelepítéséhez Linux-alapú fürtre megértéséhez a dokumentum egyes szakaszainak.

2.  A próba és a minőséggel garancia környezet Linux-alapú fürt létrehozása. Linux-alapú fürt létrehozásával kapcsolatos további tudnivalókért lásd: [létrehozása Linux-alapú fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-provision-linux-clusters.md).

3.  Az új környezet meglévő feladatok, adatforrások és mosdók másolja. Lásd: az adatok másolása a próba környezet szakaszban további információt.

4.  Teszteléssel érvényességi kattintva győződjön meg arról, hogy a feladatokat a az új fürt a várt módon működnek-e.

Ellenőrizte, hogy minden a várt módon működik, miután az áttelepítés tervezze. A legrövidebb leállás alatt végezze el az alábbi műveletek.

1.  Készítsen biztonsági másolatot a fürt csomópontok tárol tranziens adatokat. Ha például közvetlenül egy központi csomóponton tárolt adatok van.

2.  A Windows-alapú fürt törléséhez.

3.  A Windows-alapú fürt használt azonos alapértelmezett adatok tárolót használó Linux-alapú fürt létrehozása. Ez lehetővé teszi az új fürt, folytassa a munkát a termelési meglévő adatok alapján.

4.  Biztonsági másolat tranziens az adatok importálása.

5.  Kezdés feladatok/továbbra is használja az új fürt feldolgozása.

### <a name="copy-data-to-the-test-environment"></a>Másolja az adatokat a tesztkörnyezetben

Számos módszerrel másolni az adatokat, és a feladatok, azonban a fentebb ismertetett két módon a legegyszerűbben a közvetlenül áthelyezni a fájlokat a próba fürtre.

#### <a name="hdfs-dfs-copy"></a>Az elosztott fájlrendszer Fájlrendszerhez másolása

A Hadoop Fájlrendszerhez paranccsal közvetlenül adatainak másolása a tárterület a meglévő gyártási fürt tárolt adatait egy új próba fürt, az alábbi lépésekkel.

1. A tárterület-fiók és az alapértelmezett tároló információk keresése a meglévő fürt. Ehhez használja az alábbi Azure PowerShell parancsprogramot.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Kövesse a dokumentumban a HDInsight hozzon létre egy új tesztkörnyezetben létrehozása Linux-alapú fürt. Állítsa le a fürt létrehozása előtt, és helyette válassza a **Választható beállítási**.

3. A választható beállítási lap kattintson a **Csatolt tárterület-fiókokhoz**.

4. Válassza a **Hozzáadás a tárhely billentyűt**, és amikor a rendszer kéri, válassza ki a tárterület-fiókot, a PowerShell-parancsprogramot, az 1 által visszaadott. Kattintson a **Jelölje be** a bezárásuk minden egyes lap. Végezetül hozza létre.

5. A csoport létrehozását követően használatával kapcsolódik hozzá **SSH.** Ha nem ismeri a HDInsight SSH használja, az alábbi cikkekben egyike látható.

    * [Windows-ügyfelek Linux-alapú HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Linux-alapú HDInsight Linux rendszerhez, a Unix és a Mac-ügyfelek SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

6. A SSH munkamenetből paranccsal a következő fájlok másolása a csatolt tárterület-fiókból az új alapértelmezett tároló fiók. A tároló és a fiókadatok az PowerShell-parancsprogramot, az 1 által visszaadott tároló és a fiók cserélje ki. Adatok elérési útja cserélje le a fájl elérési útját adatokat.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Ha a címtár-szerkezet adatokat tartalmazó nem létezik a tesztkörnyezetben, hozhat létre a következő parancs használatával.

        hdfs dfs -mkdir -p /new/path/to/create

    A `-p` kapcsoló lehetővé teszi, hogy az elérési út az összes könyvtárak létrehozását.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Közvetlen másolás tároló Azure BLOB-között

Azt is megteheti, előfordulhat, hogy használni kívánt a `Start-AzureStorageBlobCopy` Azure PowerShell-parancsmag másolása BLOB-tároló fiókok HDInsight-Ön kívüli között. További tudnivalókért lásd: hogyan kezelheti az Azure PowerShell használatá Azure adathordozós Azure BLOB szakaszában.

##<a name="client-side-technologies"></a>Ügyféloldali technológiák

Az általános ügyféloldali technológiák, például [Azure PowerShell-parancsmagok](../powershell-install-configure.md), [Azure CLI](../xplat-cli-install.md) vagy a [.NET SDK Hadoop-](https://hadoopsdk.codeplex.com/) továbbra is a Linux-alapú fürt ugyanúgy működik, ahogy azok azonosak-OS fürt mindkettőt REST API-khoz is támaszkodhat.

##<a name="server-side-technologies"></a>Kiszolgálóoldali technológiák

Az alábbi táblázat a áttelepítése kiszolgálóoldali összetevők, amelyek a Windows adott ismerteti.

| Ha ezt a technológiát használja... | Ezt a műveletet. |
| ----- | ----- |
| **A PowerShell** (kiszolgálóoldali parancsfájlok, beleértve a parancsfájl-műveletek fürt létrehozása során) | Újraírása Bash parancsfájlokat. Parancsfájl-műveletek a [parancsprogram-műveleteinek testreszabása Linux-alapú HDInsight](hdinsight-hadoop-customize-cluster-linux.md) és a [parancsprogram művelet fejlesztési Linux-alapú HDInsight-](hdinsight-hadoop-script-actions-linux.md)témakörben talál. |
| **Azure CLI** (kiszolgálóoldali parancsfájlok) | Noha az Azure CLI Linux, azt nem származnak előre telepítve van a HDInsight fürt központi csomóponton. Ha a kiszolgálóoldali parancsfájlok van szüksége, olvassa el a [telepítse az Azure CLI](../xplat-cli-install.md) Linux-alapú platformokon telepítéséről további információt. |
| **.NET-összetevők** | .NET nem teljesen támogatott Linux-alapú HDInsight fürt. A HDInsight Linux-alapú vihar fürtök 10/28/2017 támogatási után létrehozott C# vihar topológiák az SCP.NET keretrendszer használatával. A .NET rendszerhez további támogatási belekerül jövőbeli frissítéseit. |
| **Win32-összetevők vagy más csak Windows technológia** | Útmutató függ, hogy az összetevő vagy technológia; Előfordulhat, hogy tudja megkeresni Linux kompatibilis verziója, vagy egy alternatív megoldás keresheti meg és az összetevő átírásának szüksége lehet. |

##<a name="cluster-creation"></a>Csoport létrehozása

Ez a szakasz ismerteti fürt létrehozása közötti különbségek.

### <a name="ssh-user"></a>SSH felhasználói

HDInsight fürt Linux-alapú **Biztonságos rendszerhéj (SSH)** a protokollt használja a fürt csomópontok távoli eléréséhez. Távoli asztali a Windows-alapú fürt eltérően a legtöbb SSH ügyfelek nem adja meg a grafikus kezelőfelület, hanem inkább itt egy parancssori, amely lehetővé teszi, hogy a fürt parancsai futtathatók. Egyes ügyfelek (például [MobaXterm](http://mobaxterm.mobatek.net/),) mellett egy távoli parancssori grafikus fájl rendszer böngészőben adja meg.

Csoport létrehozása során meg kell adnia egy SSH felhasználó és a **jelszavát** vagy **nyilvános kulcs tanúsítvány** hitelesítési.

Használata ajánlott nyilvános kulcs tanúsítvány, mivel az biztonságosabb, mint a jelszót használja. Hitelesítés aláírt nyilvános és titkos kulcs két létrehozása, majd a csoport létrehozásakor kezeléséről a nyilvános kulcshoz működik. A kiszolgálón SSH használatával csatlakozik, a titkos kulcs az ügyfél hitelesítési a kapcsolatot biztosít.

A HDInsight SSH használja a további tudnivalókért lásd:

- [Windows-ügyfelek HDInsight SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

- [HDInsight Linux, Unix és OS X ügyfelektől érkező SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Fürt testreszabása

**Parancsfájl-műveletek** Linux-alapú fürt használt Bash parancsfájl kell írni. Parancsfájl-műveletek csoport létrehozása során használható, miközben Linux-alapú fürtre vonatkozóan azokat is után fürt felfelé testreszabási végrehajtásához használt és működik. További információt a [testreszabása Linux-alapú HDInsight parancsprogram-műveleteinek](hdinsight-hadoop-customize-cluster-linux.md) és a [parancsfájl művelet fejlesztési Linux-alapú HDInsight-](hdinsight-hadoop-script-actions-linux.md)témakörben talál.

Egy másik testreszabási funkció **betöltő**. A Windows fürt így további tárak használata helyének megadásához a struktúra. Fürt a létrehozás után tárakban használhatók automatikusan struktúra lekérdezések használata nélkül `ADD JAR`.

Betöltő Linux-alapú fürt esetében nem ezt a lehetőséget. Ehelyett művelettel parancsprogram [hozzáadása struktúra](hdinsight-hadoop-add-hive-libraries.md)tárakban fürt létrehozása során dokumentált.

### <a name="virtual-networks"></a>Virtuális hálózatok

Windows-alapú HDInsight fürtök klasszikus virtuális hálózatokkal csak munka, míg Linux-alapú HDInsight fürt erőforrás-kezelő virtuális hálózatok szükségesek. Források a Linux-HDInsight fürt, csatlakoznia kell klasszikus virtuális hálózatban, tanulmányozza a következő [Klasszikus virtuális hálózat erőforrás-kezelő virtuális hálózathoz csatlakozik](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Azure virtuális hálózatok használata HDInsight konfigurációs követelményei a további tudnivalókért lásd: [kiterjesztése HDInsight funkciók egy virtuális hálózaton keresztül](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Kezelés és figyelése

A webes a Windows-alapú HDInsight, például a korábbi vagy fonal Felületet, előfordulhat, hogy használt előkészíthetik számos Ambari keresztül érhető el. A Ambari struktúra nézet emellett webböngészőn keresztül struktúra lekérdezések futtatása lehetőséget. A Ambari webes felület Linux-alapú fürt https://CLUSTERNAME.azurehdinsight.net a érhető el.

Ambari használatával kapcsolatos további tudnivalókért lásd: a következő dokumentumokat:

- [Webes Ambari](hdinsight-hadoop-manage-ambari.md)

- [Ambari REST API-val](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Ambari értesítések

Ambari egy figyelmeztető rendszer, hogy meg tudja mondani a potenciális problémákról a fürthöz tartalmaz. Figyelmeztetés jelenik meg a Ambari webes felület piros vagy sárga bejegyzésként azonban is meghallgathatja őket az REST API-k.

> [AZURE.IMPORTANT] Ambari értesítések jelzik, hogy nem *lehet* a probléma, hogy van-e *van* probléma. Ha például jelenhet meg, hogy HiveServer2 nem érhető el, értesítés annak ellenére, hogy a szokásos módon érhető el.
>
> Sok riasztások megvalósítása szolgáltatás intervallum-alapú lekérdezéseket, és várjon egy adott időkereten belüli választ. A figyelmeztetést nem feltétlenül jelenti, hogy a szolgáltatás nem működik, hogy csak az, hogy nem találatokat is visszaadjon a várt időkereten belüli időpontra.

Az általános, gondolja végig, akár jelzést van már előforduló hosszabb ideig, akár tükrözi, amely a korábban kapcsolatban a fürthöz előtt műveleteket végezhet a felhasználó problémáit.

##<a name="file-system-locations"></a>Rendszer helye

A Linux fürt fájlrendszer kártyaformátumban Windows-alapú HDInsight fürt másképp. Az alábbi táblázat segítségével gyakran használt keresése a fájlokat.

| Keresés végrehajtandó feladat | Található... |
| ----- | ----- |
| Konfiguráció | `/etc`. Ha például`/etc/hadoop/conf/core-site.xml` |
| Naplófájlok | `/var/logs` |
| Hortonworks adatok Platform (HDP) | `/usr/hdp`. Vannak olyan két könyvtárak található ebben az esetben, amely a jelenlegi HDP verzióval (például `2.2.9.1-1`,) és `current`. A `current` címtár szimbolikus fájlok és mappák verzió szám könyvtárában található hivatkozásokat tartalmaz, és áll rendelkezésre, a HDP fájlok elérése a verzió óta kényelmesen szám a HDP verzió frissítése módosítása. |
| hadoop-streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Általánosságban elmondható Ha ismeri annak a fájlnak a nevét, segítségével a következő parancsot a SSH munkamenetből keresse meg a fájl elérési útja:

    find / -name FILENAME 2>/dev/null

Helyettesítő karakterek és a fájlnevet is használhatja. Ha például `find / -name *streaming*.jar 2>/dev/null` ad vissza az elérési út "folyamatos" a fájl neve részeként szót tartalmazó üveg fájlokon.

##<a name="hive-pig-and-mapreduce"></a>Struktúra, Malac és MapReduce

Malac és MapReduce munkaterhelésekből nagyon hasonlít a Linux-alapú fürt – fő különbséget, ha távoli asztali segítségével csatlakozzon a Windows-alapú fürthöz feladatok futtatásához használandó SSH Linux-alapú fürt együtt.

- [Malac használata SSH](hdinsight-hadoop-use-pig-ssh.md)

- [SSH MapReduce használata](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>A struktúra

A következő diagramon útmutatást a struktúra munkaterhelésekből áttelepítéséhez nyújt.

| A Windows-alapú használható... | A Linux-alapú... |
| ----- | ----- |
| **Szerkesztő struktúra** | [Ambari struktúra nézetében](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Ahhoz, hogy Tez | Tez is az alapértelmezett végrehajtás motor Linux-alapú fürtre vonatkozóan, ezért a már nincs szükség a set-utasítást. |
| CMD fájlokat vagy a kiszolgálón, a struktúra feladat részeként meghívott parancsfájlok | parancsfájlok Bash használata |
| `hive`a távoli asztali parancs | [Beeline](hdinsight-hadoop-use-hive-beeline.md) vagy [egy SSH munkamenetből struktúra](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Vihar

| A Windows-alapú használható... | A Linux-alapú... |
| ----- | ----- |
| Vihar irányítópult | A vihar irányítópult nem érhető el. Lásd: [a Linux-alapú HDInsight Deploy és kezelheti a vihar topológiák](hdinsight-storm-deploy-monitor-topology-linux.md) topológiák elküldése módjai |
| Vihar felhasználói felület | A felhasználói felület vihar https://CLUSTERNAME.azurehdinsight.net/stormui címen érhető el |
| Visual Studio szeretne létrehozni, üzembe helyezését és C# vagy hibrid topológiák kezelése | Visual Studio létrehozása, üzembe helyezését és C# (SCP.NET) vagy Linux-alapú vihar a létrehozása után 10/28/2017 HDInsight fürt a hibrid topológiák kezelésére használható. |

##<a name="hbase"></a>HBase

A Linux-alapú fürt HBase znode fölérendelt van `/hbase-unsecure`. Be kell ezt a konfigurációban Java jegyzetlapon natív HBase Java API-t használó alkalmazások.

Lásd: [a HBase Java-alapú alkalmazás összeállítása](hdinsight-hbase-build-java-maven.md) egy példa ügyfél, amely akkor állítja be ezt az értéket.

##<a name="spark"></a>A külső

A külső fürt volt a Windows-fürt villámnézetben; a Megjelenés külső azonban csak Linux-alapú fürt számára érhető el. Nincs áttelepítési útvonalat a Windows-alapú külső előzetes fürtre Linux-alapú külső megjelenés fürtre nem.

##<a name="known-issues"></a>Ismert problémák

### <a name="azure-data-factory-custom-net-activities"></a>Azure Data Factory egyéni .NET tevékenységek

Azure Data Factory egyéni .NET tevékenységek jelenleg nem támogatottak a HDInsight Linux-alapú fürt. Ehelyett kell használni az alábbi lehetőségek közül választhat egyéni tevékenységek végrehajtásához a ADF folyamat részeként.

-   Azure köteg készlet .NET tevékenységek végrehajtása. [Egyéni tevékenységeknek az Azure Data Factory során](../data-factory/data-factory-use-custom-activities.md#AzureBatch) használata Azure köteg csatolt szolgáltatás szakaszában talál

-   A tevékenység MapReduce tevékenységként végrehajtása. [Adatok gyári MapReduce programok meghívása](../data-factory/data-factory-map-reduce.md) talál további információt.

### <a name="line-endings"></a>Sorvégződések

A Windows-alapú rendszerek sorvégződések az általános használja a CRLF, a Linux-alapú rendszerek LF használata. Konzerv, vagy amelyet mellőzni, adatok, mellettük CRLF sorvégeket, ha, előfordulhat, hogy módosítania kell a gyártók vagy vonzóbbak lehetnek a LF sor végződés.

Ha például Azure PowerShell használatá lekérdezés HDInsight a Windows-alapú fürthöz ad vissza CRLF adatokat. Egy Linux-alapú fürthöz ugyanazon lekérdezés LF ad eredményül. Sok esetben ez nem számít, az adatok fogyasztói azonban a Linux-alapú fürtre áttelepítése előtt meg kell vizsgálni.

Ha közvetlenül a csomóponton Linux-fürt (például a struktúra és MapReduce feladat használt Python parancsfájl) végrehajtott parancsfájlok, a sor-végződés mindig célszerű használni LF. CRLF használata esetén, jelenhet meg hibák futtatja a parancsfájlokat a Linux-alapú fürthöz.

Ha tudja, hogy a parancsfájlok nem beágyazott CR karakteres karakterláncot tartalmaz, a tömeges is módosíthatja a sorvégződések, az alábbi módszerek egyikével:

-   **Tervezi a fürt feltöltése parancsprogramok esetén**, az alábbi PowerShell-kimutatások használatával előtt állítsa be a sorvégződések a CRLF LF a parancsfájl feltöltése a fürt.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Van-e a fürt által használt tárolására, amely még parancsfájlok**, segítségével a következő parancsot a Linux-alapú fürt egy SSH munkamenetből a parancsfájl módosításával.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Következő lépések

-   [Megtudhatja, hogy miként Linux-alapú HDInsight fürt létrehozása](hdinsight-hadoop-provision-linux-clusters.md)

-   [Csatlakozás SSH használata Windows ügyfélről Linux-alapú fürthöz](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Csatlakozás SSH használatával Linux rendszerhez, a Unix vagy a Mac ügyfélprogramból Linux-alapú fürthöz](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Ambari használatával Linux-alapú fürt kezelése](hdinsight-hadoop-manage-ambari.md)
