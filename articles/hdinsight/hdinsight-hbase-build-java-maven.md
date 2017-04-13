<properties
pageTitle="Maven tesztelése HBase alkalmazás összeállítása, és a Windows-alapú HDInsight telepítése |} Microsoft Azure"
description="Megtudhatja, hogy miként Apache maven tesztelése használatával egy Apache HBase Java-alapú alkalmazás összeállítása, majd a Windows-alapú Azure hdinsight szolgáltatáshoz fürtre beállítaná őket."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"
tags="azure-portal"/>

<tags
ms.service="hdinsight"
ms.workload="big-data"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>A Windows-alapú HDInsight (Hadoop) HBase használó Java-alkalmazások létrehozásához használandó maven tesztelése

Megtudhatja, hogy miként hozhat létre és Java [Apache HBase](http://hbase.apache.org/) céljával készíthet Apache maven tesztelése használatával. Kattintson az alkalmazás használata Azure hdinsight szolgáltatáshoz (Hadoop).

[Maven tesztelése](http://maven.apache.org/) pedig a szoftver projektvezetési megértési eszköz, amely lehetővé teszi, hogy a szoftver, a dokumentáció és Java-projektekhez jelentések készítése. Ebben a cikkben megismerheti, hogyan egyszerű Java-alkalmazás létrehozása, amelyek segítségével, hoz létre, lekérdezéseket, és törli az Azure hdinsight szolgáltatáshoz fürt egy HBase táblát.

> [AZURE.NOTE] A dokumentum ismertetett lépések feltételezik, hogy a Windows-alapú HDInsight fürtre kell használnia. Linux-alapú HDInsight fürt használatával kapcsolatos további tudnivalókért lásd [Maven tesztelése használja a Linux-alapú HDInsight HBase használó Java-alkalmazások létrehozásához](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Követelmények

* [Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 vagy újabb verzió

* [Maven tesztelése](http://maven.apache.org/)


* [A Windows-alapú HDInsight fürtre HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] A lépéseket a jelen dokumentum HDInsight fürt verzióival 3,2 és 3.3 teszteltük. Alapértelmezett példák megadott értékek HDInsight 3.3 fürt.

##<a name="create-the-project"></a>A projekt létrehozása

1. A parancssorból a fejlesztői környezet, váltson könyvtárak arra a helyre, például a projekt létrehozása, amelyre `cd code\hdinsight`.

2. A paranccsal __mvn__ , amelyen telepítve van a maven tesztelése, készítése a projekthez a állványon.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Ez a parancs létrehoz egy mappát az aktuális helyre (**hbaseapp** ebben a példában.) __artifactID__ paraméterben megadott nevű Ezt a címtárat tartalmazza az alábbiakat:

    * __pom.XML__: A projekt objektummodelljéhez ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) a projekt létrehozásához használt adatok és a konfigurációs részletek tartalmazza.

    * __forrás__: a __main\java\com\microsoft\examples__ könyvtár, ahol az alkalmazás fog Szerző tartalmazó könyvtár.

3. Mivel az nem használatos ebben a példában a __src\test\java\com\microsoft\examples\apptest.java__ fájl törlése

##<a name="update-the-project-object-model"></a>A projekt objektummodelljéhez frissítése

1. A __pom.xml__ fájl szerkesztése és belül a következő kód hozzáadása a `<dependencies>` szakasz:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Ez a szakasz ez maven tesztelése, hogy a projekt __hbase-ügyfél__ verzió __1.1.2__kell lennie. Fordítási időben a függőség letölteni a alapértelmezett maven tesztelése tárat. Ha többet szeretne tudni a függőség a [Maven tesztelése központi tárházba keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) is használhatja.

    > [AZURE.IMPORTANT] A verziószám egyeznie kell a HDInsight fürthöz biztosított HBase verzióját. Az alábbi táblázat segítségével megkeresheti a megfelelő verziószáma.

  	| HDInsight fürt verziója | HBase verzió használatára |
  	| ----- | ----- |
  	| a 3,2 | 0.98.4-hadoop2 |
  	| 3.3. | 1.1.2 |

    További információt a HDInsight-verziók és -összetevők olvassa el a [Mik azok a HDInsight elérhető különböző Hadoop-összetevő](hdinsight-component-versioning.md)című témakört.

2. Ha egy HDInsight 3.3 fürthöz használja, az alábbi módon is hozzá kell adnia a `<dependencies>` szakasz:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    A függőséget fogja tölteni a phoenix-core összetevők Hbase verzió által használt 1.1.x.

2. A következő kód hozzáadása a __pom.xml__ fájlt. Ez a szakasz belül kell lennie a `<project>...</project>` címkéket a fájlban, például között `</dependencies>` és `</project>`.

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

    A `<resources>` szakasz konfigurálása egy erőforrás (__conf\hbase-site.xml__) HBase konfigurációs információit tartalmazza.

    > [AZURE.NOTE] Konfigurációs értékek kód keresztül is beállíthatja. A megjegyzések __CreateTable__ , hogyan érheti el a következő példában látható.

    Ez `<plugins>` szakasz konfigurálja [Maven tesztelése fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven tesztelése árnyékolása bővítményt](http://maven.apache.org/plugins/maven-shade-plugin/). A beépülő modul fordító a topológia összeállítására szolgál. A beépülő modul árnyékolása, hogy a licenc ismétlődések által maven tesztelése épül üveg csomagban használják. Használatos oka az, hogy az ismétlődő licenc fájlok hibát jelez a HDInsight fürt futásidőben. A maven tesztelése-árnyékolása-bővítmény használatával a `ApacheLicenseResourceTransformer` végrehajtása megakadályozza, hogy ezt a hibát.

    A maven tesztelése-árnyékolása-beépülő modul is eredményez uber üveg (vagy fat üveg), amely tartalmazza az alkalmazás által igényelt összes függőségek.

3. Mentse a __pom.xml__ fájlt.

4. Hozzon létre új __conf__ a __hbaseapp__ címtárban nevű könyvtárában. A __conf__ címtárban hozzon létre egy __hbase-site.xml__nevű fájlt. A fájl tartalmát a következő használható:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Ez a fájl betöltése a HBase konfiguráció egy HDInsight fürt lesz.

    > [AZURE.NOTE] Ez egy minimális hbase-site.xml fájlt, és benne a HDInsight fürt-bA minimális beállításainak.

3. Mentse a __hbase-site.xml__ fájlt.

##<a name="create-the-application"></a>Az alkalmazás létrehozása

1. Nyissa meg a __hbaseapp\src\main\java\com\microsoft\examples__ könyvtár, és nevezze át a app.java fájlt __CreateTable.java__.

2. Nyissa meg a __CreateTable.java__ fájlt, és a meglévő tartalmat cserélje ki a következő kódot:

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

4. A __hbaseapp\src\main\java\com\microsoft\examples__ címtárban __SearchByEmail.java__nevű új fájl létrehozása. Ez a fájl tartalmát használható a következő kódot:

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

6. A __hbaseapp\src\main\hava\com\microsoft\examples__ címtárban __DeleteTable.java__nevű új fájl létrehozása. Ez a fájl tartalmát használható a következő kódot:

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

1. Nyisson meg egy parancssort, és módosítsa a könyvtárak a __hbaseapp__ címtárhoz.

2. A következő parancsot használja, az alkalmazás tartalmazó üveg fájl létrehozásához:

        mvn clean package

    Ez törli a bármely előző build eltérések, letöltések függőségek, amely még nem telepítette, majd hozza létre és az alkalmazás csomagok.

3. Ha befejezte a parancsot, a __hbaseapp\target__ könyvtár a __hbaseapp-1.0-SNAPSHOT.jar__nevű fájlt tartalmazza.

    > [AZURE.NOTE] __hbaseapp-1.0-SNAPSHOT.jar__ fájl egy uber (más néven egy fat üveg) üveg, amely tartalmazza az alkalmazás futtatásához szükséges összes függőségek.

##<a name="upload-the-jar-file-and-start-a-job"></a>Töltse fel a üveg fájlt, és a feladat indítása

Módon sok fájl feltöltése a HDInsight fürt [Töltse fel az adatok HDInsight Hadoop feladatok](hdinsight-upload-data.md)leírt módon. Az alábbi lépésekkel Azure PowerShell-lel.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Miután való telepítéséről és konfigurálásáról Azure PowerShell, __hbase-runner.psm1__nevű új fájl létrehozása. Ez a fájl tartalmát a következő használható:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Ez a fájl két modulok tartalmazza:

    * __Hozzáadás-HDInsightFile__ - fájlok feltöltése a HDInsight használatával

    * __Kezdés-HBaseExample__ – a korábban létrehozott osztályok futtatásához használt

2. Mentse a __hbase-runner.psm1__ fájlt.

3. Azure PowerShell új ablak megnyitása, váltson az könyvtárak __hbaseapp__ , és futtassa a következő parancsot.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Módosítsa az elérési utat a korábban létrehozott __hbase-runner.psm1__ -fájl helyét. Ez a Azure PowerShell-munkamenetet regisztrálja a modul.

2. A következő parancs segítségével töltse fel a __hbaseapp-1.0-SNAPSHOT.jar__ a HDInsight fürt.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    __Hdinsightclustername__ cserélje le a HDInsight fürt nevét. A parancs a __hbaseapp-1.0-SNAPSHOT.jar__ feltölti az elsődleges tárolója a HDInsight fürt __Példa/kancsó__ helyét.

3. Után a fájlok feltöltése befejeződik, az alábbi kód használatával hozzon létre egy táblázatot a __hbaseapp__használata:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    __Hdinsightclustername__ cserélje le a HDInsight fürt nevét.

    Ez a parancs a HDInsight fürt __személyek__ nevű új táblát hoz létre. Ez a parancs nem látható minden kimeneti a konzol ablak.

2. A tábla bejegyzéseit keres, használja a következő parancsot:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    __Hdinsightclustername__ cserélje le a HDInsight fürt nevét.

    Ez a parancs az **SearchByEmail** osztály használja bármely sor, ahol a __contactinformation__ oszlop család és az __e-mailek__ oszlopban tartalmazza a karakterlánc __contoso.com__kereshet. A következő eredményeket kell megjelennie:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    __Fabrikam.com__ a használatával a `-emailRegex` értéket adja vissza az e-mailek mezőben a __fabrikam.com__ rendelkező felhasználók. A keresési kifejezés alapú normál szűrő használatával történik, mivel is megadhat reguláris kifejezéseket, például: __^ r__, mely eredménye tételeket, ahol az e-mailt a "r" betűvel kezdődik.

##<a name="delete-the-table"></a>A táblázat törlése

Amikor befejezte a példában, használja a következő parancsot a Azure PowerShell-munkamenetet a ebben a példában a __személyek__ megjelenítő táblázat törlése:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

__Hdinsightclustername__ cserélje le a HDInsight fürt nevét.

##<a name="troubleshooting"></a>Hibaelhárítás

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Kezdés-HBaseExample használata esetén nem várt eredmények sem eredménye

Használja a `-showErr` paraméter megtekintése a feladat futtatása során létrehozott standard hiba (STDERR).
