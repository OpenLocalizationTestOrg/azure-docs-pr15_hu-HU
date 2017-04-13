<properties
    pageTitle="Hadoop Sqoop használata a HDInsight |} Microsoft Azure"
    description="Megtudhatja, hogy miként használhatja a HDInsight .NET SDK futtatása a Sqoop importálása és exportálása egy Hadoop fürthöz és Azure SQL-adatbázishoz között."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>A HDInsight Hadoop .NET SDK használatának Sqoop feladatok futtatása

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogy miként HDInsight .NET SDK segítségével Sqoop feladatok futtatja a HDInsight importálásához és exportálásához HDInsight fürt és Azure SQL-adatbázis vagy SQL Server-adatbázis között.

> [AZURE.NOTE] Ez a cikk lépéseinek kínál vagy a Windows-alapú vagy Linux-alapú HDInsight fürtre; Ezeket a lépéseket a Windows ügyfélről azonban csak működik. Ez a cikk tetején lévő tabulátorválasztó segítségével más módon.

###<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **A Hadoop-fürt hdinsight szolgáltatásból lehetőségre**. Lásd: [fürt létrehozása és SQL-adatbázishoz](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>.NET SDK használatával Sqoop futtatása

A HDInsight .NET SDK biztosít a .NET-ügyfél tárak, így könnyebb HDInsight fürt a .NET rendszerhez készült. Ebben a részben létrehoz egy C# konzol alkalmazás a hivesampletable exportálása a e oktatóanyagok a korábban létrehozott SQL-adatbázis táblát.

**A feladat Sqoop elküldése**

1. C# console-alkalmazás létrehozása a Visual Studióban.
2. A Visual Studio csomag Manager konzolról a következő parancsot Nuget importálhatja a csomagot.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. A Program.cs fájlban használja a következő kódot:

        using System.Collections.Generic;
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
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Nyomja le az **F5 billentyűparancs hatására** a program futtatásához. 

##<a name="limitations"></a>Korlátozások

* Tömeges export - a Linux-alapú HDInsight, az adatok exportálása a Microsoft SQL Server- vagy SQL Azure-adatbázis használt Sqoop összekötő jelenleg nem támogatja a tömeges szúr be.

* Kötegelés – a HDInsight Linux-alapú használata esetén a `-batch` beszúrása végrehajtásakor váltás, Sqoop több beszúrása a Beszúrás műveletek kötegelés helyett hajt végre.

##<a name="next-steps"></a>Következő lépések

Most már megtanulta azt is van Sqoop használatáról. További tudnivalókért lásd:

- [A HDInsight használata Oozie](hdinsight-use-oozie.md): használata Sqoop művelet Oozie-munkafolyamatokban.
- [Elemzés nézetbeli késési adataival HDInsight használatával](hdinsight-analyze-flight-delay-data.md): nézetbeli elemzéséhez használható struktúra adatok késleltetése, majd Sqoop adatainak exportálása Azure SQL-adatbázishoz.
- [Töltse fel az adatok hdinsight szolgáltatáshoz](hdinsight-upload-data.md): más módszerek a feltöltéshez HDInsight/Azure Blob-tárolóhoz találja.


