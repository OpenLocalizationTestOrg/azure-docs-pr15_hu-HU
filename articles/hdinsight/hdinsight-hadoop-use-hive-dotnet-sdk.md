<properties
    pageTitle="HDInsight .NET SDK struktúra lekérdezések futtatása |} Microsoft Azure"
    description="Megtudhatja, hogy miként küldhetnek Hadoop feladatokat Azure hdinsight szolgáltatáshoz hadoop HDInsight .NET SDK használatával."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>HDInsight .NET SDK struktúra lekérdezések futtatása

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Megtudhatja, hogy miként struktúra lekérdezések HDInsight .NET SDK nyújt.

> [AZURE.NOTE] Az ebben a cikkben leírt lépéseket kell elvégezni a Windows ügyfélprogramból. Használatával kapcsolatos tudnivalókat használatával egy Linux, az OS X vagy a Unix ügyfél struktúra, a tabulátorválasztó a témakör tetején látható.

##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **A Hadoop-fürt hdinsight szolgáltatásból lehetőségre**. Lásd: [használatba Linux-alapú Hadoop a hdinsight szolgáltatásból lehetőségre](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012 és 2013-ban és a Skype 2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Lekérdezések HDInsight .NET SDK struktúra elküldése

A HDInsight .NET SDK biztosít a .NET-ügyfél tárak, így könnyebb HDInsight fürt a .NET rendszerhez készült. 

**Feladatok elküldése**

1. C# console-alkalmazás létrehozása a Visual Studióban.
2. A Nuget csomag Manager konzolról futtassa a következő parancsot.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Használja a következő kódot:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Nyomja le az **F5 billentyűparancs hatására** az alkalmazás futtatásához.


## <a name="next-steps"></a>Következő lépések

Ebben a cikkben többféle módon is létre HDInsight fürt megtanulta azt van. További tudnivalókért lásd: az alábbi cikkekben:

* [Első lépések az Azure hdinsight szolgáltatáshoz][hdinsight-get-started]
* [Hozzon létre Hadoop fürt hdinsight szolgáltatáshoz][hdinsight-provision]
* [A HDInsight Hadoop fürt kezelése az Azure portál használatával](hdinsight-administer-use-management-portal.md)
* [.NET-SDK HDInsight-hivatkozás](https://msdn.microsoft.com/library/mt271028.aspx)
* [Malac használata hdinsight szolgáltatáshoz](hdinsight-use-pig.md)
* [HDInsight Sqoop használata](hdinsight-use-sqoop-mac-linux.md)
* [Interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


