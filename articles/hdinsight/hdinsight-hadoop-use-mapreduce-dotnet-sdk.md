<properties
    pageTitle="HDInsight .NET SDK használatával MapReduce feladatok elküldése |} Microsoft Azure"
    description="Megtudhatja, hogy miként küldhetnek MapReduce feladatokat Azure hdinsight szolgáltatáshoz hadoop HDInsight .NET SDK használatával."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK használatával MapReduce feladatok futtatása

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Megtudhatja, hogy miként küldhetnek HDInsight .NET SDK használatával MapReduce feladatokat. HDInsight fürt beépített üveg fájl az egyes MapReduce minták. A fájl üveg */example/jars/hadoop-mapreduce-examples.jar*.  A minták egyik *wordcount*. A C# konzol alkalmazás wordcount feladat elküldése fejleszt.  A feladat beolvassa a */example/data/gutenberg/davinci.txt* fájlt, és az eredmények */example/data/davinciwordcount*.  Ha meg szeretné futtassa újra az alkalmazást, akkor kell mappa karbantartása: a kimeneti.

> [AZURE.NOTE] Az ebben a cikkben leírt lépéseket kell elvégezni a Windows ügyfélprogramból. Használatával kapcsolatos tudnivalókat használatával egy Linux, az OS X vagy a Unix ügyfél struktúra, a tabulátorválasztó a témakör tetején látható.

##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **A Hadoop-fürt hdinsight szolgáltatásból lehetőségre**. Lásd: [használatba Linux-alapú Hadoop a hdinsight szolgáltatásból lehetőségre](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012 és 2013-ban és a Skype 2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK használatával MapReduce feladatok elküldése

A HDInsight .NET SDK biztosít a .NET-ügyfél tárak, így könnyebb HDInsight fürt a .NET rendszerhez készült. 

**Feladatok elküldése**

1. C# console-alkalmazás létrehozása a Visual Studio alkalmazásban.
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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- Fürt létrehozása és a struktúra feladat elküldése című témakörben talál [Azure hdinsight szolgáltatáshoz – első lépések](hdinsight-hadoop-linux-tutorial-get-started.md).
- HDInsight fürt létrehozására, lásd: [létrehozása Linux-alapú Hadoop fürt a hdinsight szolgáltatásból lehetőségre](hdinsight-hadoop-provision-linux-clusters.md).
- HDInsight fürt kezelésére, olvassa el a [kezelése a Hadoop fürt a HDInsight](hdinsight-administer-use-management-portal.md)című témakört.
- A HDInsight .NET SDK tanulási, [HDInsight .NET SDK – segédlet](https://msdn.microsoft.com/library/mt271028.aspx)című cikkben.
- A nem interaktív Azure hitelesíteni, lásd: a [hitelesítési nem interaktív .NET HDInsight-alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




