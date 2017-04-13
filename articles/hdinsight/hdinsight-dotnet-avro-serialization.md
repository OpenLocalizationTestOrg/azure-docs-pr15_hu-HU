<properties
    pageTitle="A Microsoft Avro Library adatok szerializálni |} Microsoft Azure"
    description="Megtudhatja, hogyan Azure hdinsight szolgáltatáshoz használja nagy adatok szerializálni Avro."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Adatok az a Microsoft Avro Library Hadoop szerializálni

Ez a témakör bemutatja használatáról a <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro tár</a> objektumok és más adatstruktúrák szerializálni adatfolyamok be annak érdekében, hogy a memóriahasználat, adatbázis vagy fájl marad meg őket, továbbá a hogyan deszerializálásához, az eredeti objektumok visszaállításához.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
A <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro tár</a> végrehajtja a Apache Avro szerializálási adatrendszerrel a Microsoft.NET környezetben. Apache Avro biztosít szerializálási egy kompakt bináris data interchange formátum. Nyelvi interoperability köt nyelvi-felismerése nélkül sémára definiálása <a href="http://www.json.org" target="_blank">JSON</a> használ. Több nyelv eredményezi adatok egy másik is olvashatók. Jelenleg a C, C++, C#, Java, PHP, Python és fonetikus támogatottak. A formátum részletes információkat az <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro specifikációja</a>találhatók. Figyelje meg, hogy az aktuális verzióját a Microsoft Avro Library nem támogatja ezt a beállítást a távoli hívások (RPC) részét.

A Avro rendszer objektum szerializált ábrázolása két részből áll: séma és a tényleges érték. A Avro séma nyelvi beállítástól független adatmodell a JSON szerializált váltakozó ismerteti. Bemutató egymás melletti a bináris adatokat megjelenítő. Ahhoz, hogy nincs érték költségek, gyors szerializálási és a megjelenítő kis tétele az egyes objektumokra problémákat külön értendő a bináris ábrázolás a séma lehetővé teszi.

##<a name="the-hadoop-scenario"></a>A Hadoop eset
Apache Avro szerializálási formátum sokan használják Azure hdinsight szolgáltatáshoz és a többi Apache Hadoop-környezetben. Avro Hadoop MapReduce feladat összetett adatstruktúra ábrázolásához kényelmesen biztosít. A formátum (Avro tároló objektumfájl) Avro fájlok verziójához meg a támogatja a elosztott MapReduce programozási modell. A fő, amely lehetővé teszi, hogy a terjesztési funkció, hogy a fájlokat is, hogy egy beszerzése a fájl bármely pontjára, és olvasási indítása az egy adott szövegrészt értelemben "táblázat felosztása".

##<a name="serialization-in-avro-library"></a>Szerializálási Avro tárban
A .NET-tár Avro az szerializálási objektumok kétféle módon támogatja:

- **tükröződés** – a JSON séma típusok az adatok automatikusan elkészült szerződést attribútumok használhatók, amelyekről szeretne .NET típusú.
- **Általános rekord** : Ha nincs .NET típusokat bemutató ahhoz a séma az adatok használhatók, amelyekről nem jelöli a [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) osztály rekordban explicit módon megadva az A JSON séma.

Az adatok séma ismert író és az adatfolyam olvasó, az adatok lehet küldeni a séma nélkül. Azokban az esetekben egy Avro objektumfájl tároló használata esetén a séma van tárolva a fájlt. További paramétereket, például az adatok tömörítést használt kodek adható meg. Forgatókönyvekben részletesebben tagolt és az alábbi kód példák mutatják be.


## <a name="install-avro-library"></a>Avro tár telepítése

Az alábbiak szükségesek a tár telepítése előtt:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET-keretrendszer 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 vagy újabb verzió)

Figyelje meg, hogy a Newtonsoft.Json.dll függőség töltse le a rendszer automatikusan a a Microsoft Avro Library telepítése mellé. Ez az eljárás a következő szakaszban megadva.


A Microsoft Avro Library szolgáló NuGet csomag keresztül az alábbi eljárás a Visual Studio is telepítve van meghatározva:

1. Jelölje ki a **Projekt** lap -> **... NuGet csomagok kezelése**
2. Keresse meg az **Online keresés** mezőben a "Microsoft.Hadoop.Avro".
3. Kattintson a **Microsoft Azure hdinsight szolgáltatáshoz Avro tár**melletti **telepítése** gombra.

Megjegyzendő, hogy a Newtonsoft.Json.dll (> = 6.0.4) függőség is töltse le a rendszer automatikusan a a Microsoft Avro Library.

Érdemes keresse fel a <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro tár kezdőlapja</a> az aktuális kibocsátási megjegyzések olvasható.


A Microsoft Avro Library forráskód érhető el a <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro tár a Kezdőlap lap</a>.

##<a name="compile-schemas-using-avro-library"></a>A sémák segítségével Avro tárban fordítása

A Microsoft Avro Library egy kód generációs segédprogram, amely lehetővé teszi, hogy a korábban beállított JSON séma alapján automatikusan C# típusok létrehozása tartalmazza. A kód generációs segédprogram van meghatározva, mint egy bináris végrehajtható nem, de egyszerűen beépíthető keresztül az alábbi lépésekkel:

1. Töltse le a legújabb HDInsight SDK forráskód .zip kiterjesztésű <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET-Hadoop SDK</a>. (Kattintson a **Letöltés** ikonra, nem **tölthető le** lapon.)

2. Kivonat a HDInsight SDK csomagjában talál egy mappába a .NET-keretrendszer 4 a számítógépen telepítve, és csatlakozik az internethez, töltse le a szükséges függőség NuGet csomagok. Az alábbi azt feltételezi, hogy forráskódot kicsomagolta C:\SDK.

3. Nyissa meg a mappa C:\SDK\src\Microsoft.Hadoop.Avro.Tools, és futtassa a build.bat. (A fájl visszahívja MSBuild a 32 bites terjesztési .NET-keretrendszer. Ha szeretné, hogy a 64 bites verzióját használja, szerkeszthetők build.bat, a fájlban megjegyzései.) Győződjön meg arról, hogy a sikeres összeállítása. (Bizonyos rendszereken MSBuild alakulhat figyelmeztetések. Ilyen figyelmeztetések nem vonatkoznak a segédprogram mindaddig, amíg nem build hibák történnek.)

4. A lefordított segédprogram C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools található.


Megismerkedhet a parancssori szintaxist, át a mappát, ahonnan a kód generációs segédprogram végre az alábbi parancsot:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Ha tesztelni szeretné a segédprogramot, a minta JSON sémafájl forráskódot ellátni a C# osztályok hozhat létre. Hajtsa végre az alábbi parancsot:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Ez az aktuális könyvtár fájlok két C# kapcsol értendő: SensorData.cs és Location.cs.

A logika használó a kód generációs segédprogram konvertálásakor a JSON séma C# típusra, című cikkben talál részletes GenerationVerification.feature C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc található fájl.

Felhívjuk a figyelmét arra, hogy névtér kiolvasott-e a JSON séma, a fájlt, az előző bekezdés említett ismertetett logika használatával. A séma kiolvasott névtér élveznek függetlenül a segédprogram parancssorban /n paraméterrel megadva. Ha azt szeretné, a névtér a séma tartalmazott felülbírálása, /nf paraméter használatával. Minden névtér átállítása a SampleJSONSchema.avsc my.own.nspace, például végre az alábbi parancsot:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>A minták
Ebben a témakörben leírt hat példák bemutatják, a Microsoft Avro Library által támogatott különböző forgatókönyvek. A Microsoft Avro Library bármilyen adatfolyam végezhető lett tervezve. Ezek a példák az adatok memória adatfolyamok, hanem fájl adatfolyamok, illetve az egyszerűség és egységesebb adatbázisok n. A pontos forgatókönyv követelményeknek, adatforrás és mennyiségi, korlátozások a teljesítmény és egyéb tényezők munkakörnyezetben megközelítést függ.

Az első két példák bemutatják, hogyan lehet szerializálni és adatok deszerializálni memória adatfolyam puffer a tükröződés és az általános rekordok használatával. A két ilyenkor séma az olvasóknak és szövegírói közösen tekinti a sávon.

A harmadik és negyedik példák bemutatják, hogyan lehet szerializálni és deszerializálni adatokat tároló fájlok Avro objektum használatával. -Avro tároló fájlban tárolt adatok, a séma mindig tárolt vele, mert a séma meg kell osztania deszerializáláshoz.

Az első négy példák tartalmazó mintában a <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure mintakódok</a> webhelyről letölthető.

Az ötödik példában látható egy egyéni tömörítési kodek használata Avro objektum tároló fájlok hogyan. Ebben a példában a kódot tartalmazó minta az <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure mintakódok</a> webhelyről letölthető.

A hatodik minta megtudhatja, hogy hogyan Avro szerializálási Azure Blob-tárolóhoz feltölteni az adatokat, és ezután elemezheti az egy HDInsight (Hadoop) fürthöz-struktúra használatával. Az <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure mintakódok</a> webhelyről letölthető.

Az alábbiakban a témakörben tárgyalt hat minták mutató hivatkozások:

 * <a href="#Scenario1">**A tükröződés szerializálási**</a> - típusok használhatók, amelyekről a JSON séma az adatok automatikusan elkészült szerződést attribútumok.
 * <a href="#Scenario2">**Általános rekorddal szerializálási**</a> – a JSON séma explicit módon megadva rekord ha nincs .NET-típus nem érhető el, a tükröződés.
 * <a href="#Scenario3">**Objektum tároló fájlokkal a tükröződés szerializálási**</a> – a JSON séma automatikusan beépített és a szerializált adat-Avro objektum tároló fájlként együtt megosztott.
 * <a href="#Scenario4">**Objektum tároló fájlokkal általános rekorddal szerializálási**</a> – a JSON séma kifejezetten előtt szerializálásának megadott és megosztott együtt az adatokat egy Avro objektum tároló fájlként.
 * <a href="#Scenario5">**Objektum tároló fájlok egy egyéni tömörítési kodek használatával szerializálási**</a> – a példa Avro objektum tároló fájl létrehozása a Deflate adatok tömörítés kodek testre szabott .NET végrehajtásának mutatja.
 * <a href="#Scenario6">**Avro használatával feltölteni az adatokat a Microsoft Azure HDInsight szolgáltatáshoz**</a> – a példa azt szemlélteti, hogy Avro szerializálási hogyan kommunikáljon a HDInsight szolgáltatás. Ez a példa futtatásához egy aktív Azure előfizetés és Azure hdinsight szolgáltatáshoz fürthöz hozzáférés szükséges.

###<a name="Scenario1"></a>Minta 1: Szerializálási a tükröződés

A JSON séma típusok automatikusan épül fel a Microsoft Avro Library keresztül tükröződés az adatok használhatók, amelyekről C# objektumok attribútumainak szerződést. A Microsoft Avro Library létrehoz egy [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) azonosítja a mezőket, hogy használhatók, amelyekről.

Ebben a példában objektumok (egy tag **hely** exportálva **SensorData** osztály) vannak szerializálásának memória adatfolyam szeretne, és a követő viszont deszerializálni van. Az eredmény az első példányt meggyőződni arról, hogy a helyreállított **SensorData** objektum megegyezik az eredeti kerül összehasonlításra.

Ebben a példában a séma tekinti megosztott az olvasóknak és szövegírói, között, így a Avro tároló formátum nem kötelező. Példa szerializálni és adatok deszerializálni memória puffer a tükröződés – segítségével a tároló formátum megosztásakor a séma kell lennie az adatokkal olvassa el a <a href="#Scenario3">szerializálási tükröződés objektum tároló fájlok használata</a>című témakört.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Minta 2: Szerializálási általános rekorddal

A JSON séma kifejezetten megadható általános rekordban, amikor a tükröződés nem használható, mert az adatok nem jeleníthetők meg az adatok szerződést .NET osztályok keresztül. Ez a módszer akkor általában lassabb tükröződés használatával. Ebben az esetben a séma az adatokat is lehet dinamikus, azaz nem ismert fordítási időben. Kifejezett vesszővel elválasztott értékek (CSV) fájlok, amelynek séma nem ismeretlen, amíg a Avro formátumra futásidőben transzformált adatok dinamikus forgatókönyv a rendezés látható.

Ebben a példában látható, hogyan hozhat létre és használhat egy [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) pontos megadásához az JSON séma, hogyan töltse ki az adatokat, majd szerializálni és deszerializálni azt. Az eredmény az első példányt meggyőződni arról, hogy a helyreállított rekord azonos-e az eredeti kerül összehasonlításra.

Ebben a példában a séma tekinti megosztott az olvasóknak és szövegírói, között, így a Avro tároló formátum nem kötelező. Egy példa bemutatja, hogyan szerializálni és adatok deszerializálni memória puffer szóló általános rekordot a használatával az objektum tároló formázása, ha a séma szerepelnie kell a szerializált adatokat tartalmazó című témakörben az <a href="#Scenario4">objektum tároló fájlokkal általános rekorddal szerializálási</a> példa.


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>3 minta: Szerializálási tükröződés objektum tároló fájlok és szerializálási használata

Ebben a példában hasonlít az alkalmazási példát, az <a href="#Scenario1">első példát</a>, ahol a séma implicit módon megadva a tükröződés. A különbség az, hogy itt, a séma nem tekinti, amely azt deserializes az olvasót ismert. A **SensorData** objektumok használhatók, amelyekről és azok implicit módon megadott séma egy Avro tároló objektumfájl jelöli az [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály tárolja.

Az adatok szerializált ebben a példában a [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) és a deszerializált [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Az eredmény majd összehasonlítja a kezdeti példányok identitás biztosítása érdekében.

A tároló objektumfájl adatainak tömörített keresztül az alapértelmezett [**Deflate**] [ deflate-100] tömörítés kodek a .NET-keretrendszer 4. Lásd az ebben a témakörben megismerheti a [**Deflate**] újabb és kiváló verzióját szeretné használni az <a href="#Scenario5">ötödik példa</a> [ deflate-110] tömörítés kodek .NET-keretrendszer 4.5 érhető el.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Minta 4: Objektum tároló fájlok és a szerializálási használatának általános rekorddal szerializálási

Ebben a példában hasonlít az alkalmazási példát, a <a href="#Scenario2">második példában</a>, amelyben a séma explicit módon megadva az JSON. A különbség az, hogy itt, a séma nem tekinti, amely azt deserializes az olvasót ismert.

A próba adatkészlet keresztül egy kifejezetten definiált JSON séma [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objektumok listája az gyűjti, és ezt követően tárolni az objektum tároló fájlokban az [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) osztály jelöli. Ezt a fájlt tároló létrehoz egy író, amely az adatokat, és nem szeretné, majd mentette a fájlba memória adatfolyam tömörített, szerializálni. A [**Codec.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) az olvasót létrehozására használható paraméterrel, hogy az adatok nem lesznek tömörítve.

Az adatok majd fájlból olvasni és objektumok gyűjteménybe deszerializálni. Ebben a gyűjteményben a kezdeti bejegyzéslista Avro kattintva erősítse meg, hogy ezek megegyeznek összehasonlítja.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Minta 5: Szerializálási egy egyéni tömörítési kodek objektum tároló fájlok használata

Az ötödik példában látható egy egyéni tömörítési kodek használata Avro objektum tároló fájlok hogyan. Ebben a példában a kódot tartalmazó minta az [Azure mintakódok](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) webhelyről letölthető.

A [Avro specifikációja](http://avro.apache.org/docs/current/spec.html#Required+Codecs) lehetővé teszi, hogy egy tetszőleges tömörítés kodek (mellett a **Null** és **Deflate** alapértelmezés szerint) használatát. Ez a példa egy teljesen új kodek, például (az [Avro specifikációja](http://avro.apache.org/docs/current/spec.html#snappy)a támogatott választható kodek említett) Snappy nem végrehajtó. Azt megtudhatja, hogy hogyan a .NET-keretrendszer 4.5 végrehajtása a [**Deflate**] [ deflate-110] kodek, amely magában foglalja a [zlib](http://zlib.net/) tömörítés tár alapértelmezett .NET-keretrendszer 4 verziónál alapján jobb algoritmust.


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Minta 6: Segítségével Avro feltölteni az adatokat a Microsoft Azure HDInsight szolgáltatáshoz

A hatodik példa szemlélteti az Azure hdinsight szolgáltatáshoz szolgáltatás használata kapcsolatos néhány programozási technikákat. Ebben a példában a kódot tartalmazó minta az [Azure mintakódok](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) webhelyről letölthető.

A minta az alábbi műveleteket végzi el:

* Egy meglévő HDInsight-fürt csatlakozik.
* Több CSV-fájl serializes és feltöltések megjelenítése az eredmény az Azure Blob-tárolóhoz. (A CSV-fájlok kerülnek terjesztésre együtt a minta és az időszak 1970 2010 [Infochimps](http://www.infochimps.com/) bontva mellékletben árfolyam korábbi adatokból kivonat képviseli. A minta CSV-fájl adatokat olvas be, a rekordok **árfolyam** osztály példányai alakít át., és kattintson a tükröződés serializes. Készlet típusa meghatározása a Microsoft Avro Library kód generációs segédprogram keresztül JSON séma alapján létre.
* Új táblát hoz létre külső **készletek** a struktúra és a hivatkozásokat, hogy az adatokat az előző lépésben feltöltött nevezett.
* Lekérdezés végrehajtása a **készletek** táblázat fölött struktúra használatával.

Ezenkívül a minta hajt végre adattisztítási eljárás előtt és után fő műveleteket hajt végre. Során a karbantartás az összes kapcsolódó Azure Blob-adatok és mappák törlődik, és a struktúratáblával megszakad. A minta parancssorból a adattisztítási lépéseket is hívhatók.

A minta előfeltételei a következők:

* Az aktív Microsoft Azure-előfizetéssel és az előfizetés azonosítójával.
* Az előfizetés a megfelelő titkos kulccsal management tanúsítványt. A tanúsítvány az aktuális felhasználói személyes tárolója a számítógépen, a minta futtatásához használt kell telepíteni.
* Aktív HDInsight fürthöz.
* A HDInsight fürt az előző előfeltétel együtt a megfelelő elsődleges és másodlagos hívóbetű az Azure tároló fiók csatolva.

Minden információt a Előfeltételek kell megadni a minta konfigurációs fájl a minta futása előtt. Kétféleképpen lehet adunk:

* A minta legfelső szintű könyvtár a app.config fájl, és ezután létrehozhatja a minta
* A minta először összeállítása, és módosítsa az összeállítás Directoryban AvroHDISample.exe.config

Mindkét esetben minden Szerkesztés kell végezni a **<appSettings>** beállítások területen. Kövesse a megjegyzéseket a fájlban.
A minta végzi a parancssorból a következő parancs végrehajtása (ahol a minta .zip kiterjesztésű lett tekinti kibontott C:\AvroHDISample; Ha, használja a megfelelő fájl elérési útja):

    AvroHDISample run C:\AvroHDISample\Data

A fürt karbantartása, futtassa a következő parancsot:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
