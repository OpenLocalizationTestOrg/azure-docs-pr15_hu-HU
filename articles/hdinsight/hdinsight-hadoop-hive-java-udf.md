<properties
pageTitle="Java felhasználó által definiált függvény (UDF) használata a struktúra HDInsight |} Microsoft Azure"
description="Megtudhatja, hogy miként hozhat létre és használhat egy Java felhasználó által definiált függvény (UDF) struktúrából a hdinsight szolgáltatásból lehetőségre."
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
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Használja a Java UDF a HDInsight struktúra

Struktúra az adatok HDInsight-használata nagyszerű, de néha szüksége van egy általánosabb nyelvi célból. Struktúrát hozhat létre a felhasználó által definiált függvényeket (UDF) programnyelven számos teszi lehetővé. A jelen dokumentum megtanulhatja a használatát egy Java UDF struktúrából.

## <a name="requirements"></a>Követelmények

* Az Azure előfizetéssel

* Egy HDInsight fürthöz (Windows vagy Linux-alapú)

    > [AZURE.NOTE] A dokumentumban a legtöbb lépéseket is működik fürt mindkettőt; azonban a lépéseket a lefordított UDF feltöltése a fürt, és indítsa el a fürt Linux-alapú kötődnek. Windows-alapú fürt használható információkra mutató hivatkozások állnak rendelkezésre.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7-es vagy újabb verzióját (vagy egy ezzel egyenértékű, például OpenJDK)

* [Apache maven tesztelése](http://maven.apache.org/)

* Egy egyszerű szövegszerkesztőben vagy Java IDE

    > [AZURE.IMPORTANT] Ha egy Linux-alapú HDInsight-kiszolgálót használ, de a Windows-ügyfél Python fájlok létrehozása, a szerkesztő kell használnia, amely LF egy sor befejezési használja. Ha nem biztos abban, hogy a szerkesztő LF vagy CRLF használja-e, című [Hibaelhárítás](#troubleshooting) lépései segédprogram használata a HDInsight fürt CR karakter eltávolításával kapcsolatban.

## <a name="create-an-example-udf"></a>Példa UDF létrehozása

1. A parancssorból a következő segítségével maven tesztelése új projekt létrehozása:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Ha a PowerShell használata esetén a paraméterek idézőjelbe kell helyezi el. Ha például `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Ez a hoz létre __exampleudf__, amely tartalmazza a maven tesztelése projekt nevű új könyvtárában.

2. A projekt létrehozása után a __exampleudf/src/próba__ könyvtár, a projekt; részeként létrehozott törlése nem lesz ebben a példában.

3. Nyissa meg a __exampleudf/pom.xml__és cseréje a meglévő `<dependencies>` bevitelhez a következőket:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Ezeket a bejegyzéseket adja meg a Hadoop és struktúra HDInsight 3.3 és 3.4 fürt részét képező verziója. Hadoop és struktúra HDInsight ellátni a [HDInsight-összetevő verziószámozás](hdinsight-component-versioning.md) dokumentumból verzióin információt találhat.

    Adja hozzá a `<build>` szakasz előtt a `</project>` sor végén található a fájlt. Ez a szakasz tartalmaznia kell a következő:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
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
            </plugins>
        </build>
    
    Hogyan hozhat létre a project ezeket a bejegyzéseket megadása Pontosabban verziója a projekt használó Java és hogyan hozhat létre egy uberjar a fürthöz telepítés.

    Miután a változtatás megtörtént, mentse a fájlt.

4. Nevezze át __exampleudf/src/main/java/com/microsoft/examples/App.java__ __ExampleUDF.java__, és kattintson a szerkesztő nyissa meg a fájlt.

5. Cserélje le a __ExampleUDF.java__ fájl tartalmát a következő, majd mentse a fájlt.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Ez végrehajtja a UDF karakterláncérték elfogadja, amely egy karakterlánc kisbetűsre változatát adja vissza.

## <a name="build-and-install-the-udf"></a>Továbbépítése és az UDF telepítése

1. A következő parancs segítségével állíthat össze, és az UDF csomag:

        mvn compile package

    A szolgáltatás összeállítása, majd az UDF csomag __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__be.

2. Használja a `scp` másolja át a fájlt a HDInsight fürt parancsot.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    A SSH felhasználói fiókokkal együtt a fürt __myuser__ cseréje __En_furtom nevű fürt__ cserélje le a csoport nevét. Ha korábban beállított jelszót biztonságossá tétele a SSH fiókot, a rendszer kéri adja meg a jelszót. Tanúsítvány használata esetén szüksége lehet használni a `-i` paraméter használatával adja meg a titkos kulcs fájlt.

3. Csatlakozás a fürt SSH használatával. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    A HDInsight SSH használja a további tudnivalókért lásd: a következő dokumentumokat.

    * [A HDInsight Linux, Unix vagy OS X Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [A Windows HDInsight Linux-alapú Hadoop SSH használata](hdinsight-hadoop-linux-use-ssh-windows.md)

4. A SSH munkamenetből másolhatja a üveg HDInsight-tárterületet.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Az UDF struktúrából használata

1. A következő segítségével indítsa el a Beeline ügyfél a SSH munkamenetet.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Ez a parancs feltételezi, hogy az alapértelmezett __rendszergazdai__ a bejelentkezési fiókjához használta a fürt.

2. Amikor odaér ehhez a `jdbc:hive2://localhost:10001/>` kéri, írja be a következőt a UDF struktúra vehet fel, és elérhetővé teheti azt a függvény.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Az UDF segítségével értékeket, amelyeket egy táblából, a kisbetű karakterláncok átalakítása.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    Ezzel az eszköz platform (Android, a Windows, iOS, stb.) jelölje ki a táblában, a karakterlánc átalakítása kisbetű, és megjelenítheti őket. A kimenet az alábbihoz hasonlóan fog megjelenni.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Következő lépések

Más módszerek a struktúra [A HDInsight struktúra használata](hdinsight-use-hive.md)című témakör tartalmaz.

Hive User-Defined funkciók, a struktúra wiki apache.org a [struktúra operátorok és a felhasználó által definiált függvényeket](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) szakasz lásd: további információt.
