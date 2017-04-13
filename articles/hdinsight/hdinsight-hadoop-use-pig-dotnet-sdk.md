<properties
   pageTitle="A HDInsight .NET Hadoop malac használata |} Microsoft Azure"
   description="Megtudhatja, hogy miként malac feladatok a HDInsight hadoop elküldése a .NET SDK Hadoop használatával."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>A .NET SDK használata a HDInsight Hadoop malac feladatok futtatása

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

A dokumentum tartalmaz egy példa egy hadoop HDInsight fürt malac feladatok elküldése a .NET SDK Hadoop használatával.

A HDInsight .NET SDK .NET ügyfél tárakat, amely megkönnyíti a .NET HDInsight fürt végezhető biztosít. Malac MapReduce műveletek létrehozása adatok átalakítások sorozata modellezése teszi lehetővé. Megtanulhatja, hogyan HDInsight fürthöz malac feladat elküldése egy egyszerű C# alkalmazás segítségével.

## <a name="prerequisites"></a>Előfeltételek

A jelen cikkben ismertetett lépések elvégzéséhez az alábbi lesz szüksége.

* Az Azure hdinsight szolgáltatáshoz (Hadoop a HDInsight) fürt (vagy a Windows vagy Linux-alapú).
* Visual Studio 2012 vagy 2013-ban vagy a Skype 2015.

## <a name="create-the-application"></a>Az alkalmazás létrehozása

A HDInsight .NET SDK biztosít a .NET-ügyfél tárak, így könnyebb HDInsight fürt a .NET rendszerhez készült. 


1. Nyissa meg a Visual Studio 2012 vagy 2013-ban
2. A **fájl** menüben válassza az **Új** , és válassza a **Projekt**.
3. Az új projekt írja be vagy válassza ki az alábbi értékeket.

    <table>
    <tr>
    <th>A tulajdonság</th>
    <th>Érték</th>
    </tr>
    <tr>
    <th>Kategória</th>
    <th>Sablonok és Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Sablon</th>
    <th>Konzol alkalmazás</th>
    </tr>
    <tr>
    <th>név</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Kattintson az **OK gombra** a projekt létrehozása.
5. Kattintson az **eszközök** menü Jelölje be a **Tár csomag Manager** vagy az **Nuget csomagot**, és válassza a **Csomag kezelője konzol**.
6. A következő parancsot a konzolban a .NET SDK csomagok telepítése.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Megoldás Intézőből kattintson duplán a **Program.cs** a megnyitáshoz. A meglévő kódot lecserélése a következőre.

        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Nyomja le az **F5 billentyűparancs hatására** az alkalmazás indításához.
8. Az **ENTER** billentyűt lenyomva lépjen ki az alkalmazást.

## <a name="summary"></a>Összefoglalás

Amint látható, a .NET SDK Hadoop teszi malac feladatok HDInsight fürthöz elküldése .NET-alkalmazások létrehozása, és figyelemmel követheti a a feladat állapota.

## <a name="next-steps"></a>Következő lépések

Ha általános információkra malac a hdinsight szolgáltatásból lehetőségre.

* [A HDInsight Hadoop malac használata](hdinsight-use-pig.md)

Az egyéb lehetőségeiről a HDInsight Hadoop dolgozhat.

* [A HDInsight Hadoop struktúra használata](hdinsight-use-hive.md)

* [A Hadoop a HDInsight használata MapReduce](hdinsight-use-mapreduce.md) [előzetes verzió – portal]: https://portal.azure.com/
