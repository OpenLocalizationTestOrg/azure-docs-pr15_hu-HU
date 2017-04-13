<properties
    pageTitle="Maven tesztelése és Java HBase alkalmazás összeállítása, majd a HDInsight Linux-alapú telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként egy Apache HBase Java-alapú alkalmazás összeállítása, majd a Azure felhőben Linux-alapú HDInsight kell üzembe Apache maven tesztelése használatával."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Java Linux-alapú HDInsight (Hadoop) HBase használható alkalmazások létrehozásához használandó maven tesztelése

Megtudhatja, hogy miként hozhat létre és Java [Apache HBase](http://hbase.apache.org/) céljával készíthet Apache maven tesztelése használatával. Kattintson az alkalmazás használata Linux-alapú HDInsight fürt.

[Maven tesztelése](http://maven.apache.org/) pedig a szoftver projektvezetési megértési eszköz, amely lehetővé teszi, hogy a szoftver, a dokumentáció és Java-projektekhez jelentések készítése. Ebben a cikkben ismerkedhet használatához hozhat létre egy egyszerű Java-alkalmazást, hogy hoz létre, lekérdezések, és törli a Linux-alapú HDInsight fürthöz egy HBase táblát.

> [AZURE.NOTE] A dokumentum ismertetett lépések feltételezik, hogy a Linux-alapú HDInsight fürtre kell használnia. A Windows-alapú HDInsight fürtre használatáról további tudnivalókért lásd [Maven tesztelése használja a Windows-alapú HDInsight HBase használó Java-alkalmazások létrehozásához](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Követelmények

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 vagy újabb verzió

* [Maven tesztelése](http://maven.apache.org/)

* [Az Azure HDInsight Linux-alapú fürtre HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] A lépéseket a jelen dokumentum HDInsight fürt verzióival 3,2 3.3 és 3.4 teszteltük. Alapértelmezett példák megadott értékek HDInsight 3.4 fürt.

* **SSH és SCP ismerős**. SSH és SCP használata HDInsight további tájékoztatást talál a következő:

    * **Linux, Unix vagy OS X ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight Linux, az OS X vagy a Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windows-ügyfelek**: lásd: [Használata SSH a Linux-alapú Hadoop a HDInsight a Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>A projekt létrehozása

1. A parancssorból a fejlesztői környezetben, váltson könyvtárak arra a helyre, például a projekt létrehozása, amelyre `cd code/hdinsight`.

2. A paranccsal __mvn__ , amelyen telepítve van a maven tesztelése, készítése a projekthez a állványon.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Ezzel létrehoz egy új mappát az aktuális könyvtár (**hbaseapp** ebben a példában.) __artifactID__ paraméterben megadott nevű Ezt a címtárat fogja tartalmazni az alábbiakat:

    * __pom.XML__: A projekt objektummodelljéhez ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) a projekt létrehozásához használt adatok és a konfigurációs részletek tartalmazza.

    * __forrás__: a __fő/java/com/a microsoft/példák__ könyvtár, ahol az alkalmazás fog Szerző tartalmazó könyvtár.

3. Mivel ez nem használható ebben a példában a __src/test/java/com/microsoft/examples/apptest.java__ fájl törlése

##<a name="update-the-project-object-model"></a>A projekt objektummodelljéhez frissítése

1. A __pom.xml__ fájl szerkesztése és belül a következő kód hozzáadása a `<dependencies>` szakasz:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Ez azt jelenti az maven tesztelése, hogy a projekt __hbase-ügyfél__ verzió __1.1.2__kell lennie. Fordítási időben ez lesz lehet letölteni a alapértelmezett maven tesztelése tárat. Ha többet szeretne tudni a függőség a [Maven tesztelése központi tárházba keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) is használhatja.

    > [AZURE.IMPORTANT] A verziószám egyeznie kell a HDInsight fürthöz biztosított HBase verzióját. Az alábbi táblázat segítségével megkeresheti a megfelelő verziószáma.

  	| HDInsight fürt verziója | HBase verzió használatára |
  	| ----- | ----- |
  	| a 3,2 | 0.98.4-hadoop2 |
  	| 3.3 és 3.4. | 1.1.2 |

    További információt a HDInsight-verziók és -összetevők olvassa el a [Mik azok a HDInsight elérhető különböző Hadoop-összetevő](hdinsight-component-versioning.md)című témakört.

2. Ha egy HDInsight 3.3 vagy 3.4 fürt használja, az alábbi módon is hozzá kell adnia a `<dependencies>` szakasz:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Ezt fogja tölteni a phoenix-core összetevők szükségesek Hbase verzióval 1.1.x.

2. A következő kód hozzáadása a __pom.xml__ fájlt. Belül kell lennie a `<project>...</project>` címkéket a fájlban, például közötti `</dependencies>` és `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
            <resource>
              <directory>${basedir}/conf</directory>
              <filtering>false</filtering>
              <includes>
                <include>hbase-site.xml</include>
              </includes>
            </resource>
          </resources>
          <plugins>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
                        <version>3.3</version>
              <configuration>
                <source>1.7</source>
                <target>1.7</target>
              </configuration>
            </plugin>
            <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-shade-plugin</artifactId>
              <version>2.3</version>
              <configuration>
                <transformers>
                  <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                  </transformer>
                </transformers>
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
          </plugins>
        </build>

    Ezzel a egy erőforrás (__conf/hbase-site.xml__,) HBase konfigurációs információit tartalmazza.

    > [AZURE.NOTE] Konfigurációs értékek kód keresztül is beállíthatja. A megjegyzések __CreateTable__ , hogyan érheti el a következő példában látható.

    Ez konfigurálja a [Maven tesztelése fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven tesztelése árnyékolása beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/)is. A beépülő modul fordító a topológia összeállítására szolgál. A beépülő modul árnyékolása, hogy a licenc ismétlődések által maven tesztelése épül üveg csomagban használják. Használatos oka az, hogy az ismétlődő licenc fájlok hibát jelez a HDInsight fürt futásidőben. A maven tesztelése-árnyékolása-bővítmény használatával a `ApacheLicenseResourceTransformer` végrehajtása megakadályozza, hogy ezt a hibát.

    A maven tesztelése-árnyékolása-beépülő modul is eredményez uber üveg (vagy fat üveg,), amely tartalmazza az alkalmazás által igényelt összes függőségek.

3. Mentse a __pom.xml__ fájlt.

4. Hozzon létre új __conf__ a __hbaseapp__ címtárban nevű könyvtárában. Ez a konfigurációs adatok történő csatlakozás HBase tartása lesz.

5. A következő paranccsal másolja a vágólapra a HBase konfiguráció a HDInsight-kiszolgálóról a __conf__ címtárhoz. **USERNAME** cseréje a a SSH bejelentkezési nevét. **CLUSTERNAME** cserélje ki a HDInsight fürt neve:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Ha korábban beállított jelszót SSH fiókjának, a rendszer kéri adja meg a jelszót. Egy SSH kulcsot a fiókkal használata esetén szüksége lehet használni a `-i` paraméterrel adja meg a fő fájl elérési útját. A következő példa fogja tölteni a titkos kulcs a `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Az alkalmazás létrehozása

1. Nyissa meg a __hbaseapp/src/fő/java/com/a microsoft/példák__ könyvtár, és nevezze át a app.java fájlt __CreateTable.java__.

2. Nyissa meg a __CreateTable.java__ fájlt, és a meglévő tartalmat lecserélése a következőre:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Ez az a __CreateTable__ osztály, amely __személyek__ nevű táblázat létrehozása és feltöltéséről néhány előre definiált felhasználókkal.

3. Mentse a __CreateTable.java__ fájlt.

4. A __hbaseapp/src/fő/java/com/a microsoft/példák__ címtárban __SearchByEmail.java__nevű új fájl létrehozása. Ez a fájl tartalmát a következő használható:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    A __SearchByEmail__ osztály használható Query sorok használt e-mail címből. Mert reguláris kifejezésekkel szűrő használ, akkor megadhatja egy karakterlánc vagy egy kifejezéssel az osztály használata esetén.

5. Mentse a __SearchByEmail.java__ fájlt.

6. A __hbaseapp/src/fő/hava/com/a microsoft/példák__ címtárban __DeleteTable.java__nevű új fájl létrehozása. Ez a fájl tartalmát a következő használható:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Az osztály szolgál ez a példa megtisztítása letiltásával, és húzással a táblázatot az __CreateTable__ osztály által létrehozott.

7. Mentse a __DeleteTable.java__ fájlt.

##<a name="build-and-package-the-application"></a>Továbbépítése és az alkalmazás csomag

2. A __hbaseapp__ könyvtárból a következő paranccsal, amely tartalmazza az alkalmazás üveg fájl létrehozásához:

        mvn clean package

    Ez törli a bármely előző build eltérések, letöltések függőségek, amely még nem telepítette, majd hozza létre és az alkalmazás csomagok.

3. Ha befejezte a parancsot, a __hbaseapp/célhely__ könyvtár __hbaseapp-1.0-SNAPSHOT.jar__nevű fájlt tartalmazza.

    > [AZURE.NOTE] __hbaseapp-1.0-SNAPSHOT.jar__ fájl egy uber (más néven egy fat üveg) üveg, amely tartalmazza az alkalmazás futtatásához szükséges összes függőségek.

##<a name="upload-the-jar-file-and-run-jobs"></a>Töltse fel a üveg fájlt, és a feladatok futtatása

1. A következő segítségével töltse fel a üveg a HDInsight fürt. **USERNAME** cseréje a a SSH bejelentkezési nevét. **CLUSTERNAME** cserélje ki a HDInsight fürt neve:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Ez a fájl feltöltése az otthoni Directoryban SSH fiókját.

    > [AZURE.NOTE] Ha korábban beállított jelszót SSH fiókjának, a rendszer kéri adja meg a jelszót. Egy SSH kulcsot a fiókkal használata esetén szüksége lehet használni a `-i` paraméterrel adja meg a fő fájl elérési útját. A következő példa fogja tölteni a titkos kulcs a `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. A HDInsight fürthöz kapcsolatfelvétel SSH használatával. **USERNAME** cseréje a a SSH bejelentkezési nevét. **CLUSTERNAME** cserélje ki a HDInsight fürt neve:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Ha korábban beállított jelszót SSH fiókjának, a rendszer kéri adja meg a jelszót. Egy SSH kulcsot a fiókkal használata esetén szüksége lehet használni a `-i` paraméterrel adja meg a fő fájl elérési útját. A következő példa fogja tölteni a titkos kulcs a `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Miután létrejött, a következő segítségével HBase Java-alkalmazások használata új tábla létrehozása:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    Ezzel HBase __személyek__nevű új tábla létrehozása, és töltse ki az adatokat.

4. Ezután a következő használatával kereshetők az e-mail címét a táblában tárolt:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    A következő eredményeket kell megjelennie:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>A táblázat törlése

Amikor befejezte a példában, használja a következő parancsot a Azure PowerShell-munkamenetet a ebben a példában a __személyek__ megjelenítő táblázat törlése:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

