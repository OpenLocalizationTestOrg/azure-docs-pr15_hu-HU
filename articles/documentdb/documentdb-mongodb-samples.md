<properties 
    pageTitle="Példák MongoDB DocumentDB |} Microsoft Azure" 
    description="Példák DocumentDB's protokoll támogatása MongoDB található." 
    keywords="mongodb példák"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-protocol-support-for-mongodb-examples"></a>Példák MongoDB DocumentDB protokoll támogatása
Ezek a példák használatához a következőket kell tennie:

- [Létrehozás](documentdb-create-mongodb-account.md) Azure DocumentDB fiók MongoDB protokoll támogatása.
- Lekérdezi protocol támogatási MongoDB [kapcsolati karakterlánc](documentdb-connect-mongodb-account.md) információt DocumentDB fiókját.

## <a name="get-started-with-a-sample-aspnet-mvc-task-list-application"></a>Minta ASP.NET MVC tevékenység lista alkalmazások – első lépések

Is használhatja az [létrehozása az Azure virtuális gépen futó MongoDB csatlakozik, amely webalkalmazást](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) oktatóprogram minimális módosítással, gyorsan beállítása egy MongoDB alkalmazás (vagy a helyi meghajtóra vagy az Azure-webappokban közzétett), amely protocol támogatásával az MongoDB DocumentDB fiókhoz csatlakozik.  

1. Kövesse az oktatóprogram egy módosítással.  A Dal.cs kódot cserélje ki:
    
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;
        using MyTaskListApp.Models;
        using MongoDB.Driver;
        using MongoDB.Bson;
        using System.Configuration;
        using System.Security.Authentication;

        namespace MyTaskListApp
        {
            public class Dal : IDisposable
            {
                //private MongoServer mongoServer = null;
                private bool disposed = false;

                // To do: update the connection string with the DNS name
                // or IP address of your server. 
                //For example, "mongodb://testlinux.cloudapp.net
                private string connectionString = "mongodb://localhost:27017";
                private string userName = "<your user name>";
                private string host = "<your host>";
                private string password = "<your password>";

                // This sample uses a database named "Tasks" and a 
                //collection named "TasksList".  The database and collection 
                //will be automatically created if they don't already exist.
                private string dbName = "Tasks";
                private string collectionName = "TasksList";

                // Default constructor.        
                public Dal()
                {
                }

                // Gets all Task items from the MongoDB server.        
                public List<MyTask> GetAllTasks()
                {
                    try
                    {
                        var collection = GetTasksCollection();
                        return collection.Find(new BsonDocument()).ToList();
                    }
                    catch (MongoConnectionException)
                    {
                        return new List<MyTask>();
                    }
                }

                // Creates a Task and inserts it into the collection in MongoDB.
                public void CreateTask(MyTask task)
                {
                    var collection = GetTasksCollectionForEdit();
                    try
                    {
                        collection.InsertOne(task);
                    }
                    catch (MongoCommandException ex)
                    {
                        string msg = ex.Message;
                    }
                }
        
                private IMongoCollection<MyTask> GetTasksCollection()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };

                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }
        
                private IMongoCollection<MyTask> GetTasksCollectionForEdit()
                {
                    MongoClientSettings settings = new MongoClientSettings();
                    settings.Server = new MongoServerAddress(host, 10250);
                    settings.UseSsl = true;
                    settings.SslSettings = new SslSettings();
                    settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
        
                    MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
                    MongoIdentityEvidence evidence = new PasswordEvidence(password);
        
                    settings.Credentials = new List<MongoCredential>()
                    {
                        new MongoCredential("SCRAM-SHA-1", identity, evidence)
                    };
                    MongoClient client = new MongoClient(settings);
                    var database = client.GetDatabase(dbName);
                    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                    return todoTaskCollection;
                }

                # region IDisposable
        
                public void Dispose()
                {
                    this.Dispose(true);
                    GC.SuppressFinalize(this);
                }

                protected virtual void Dispose(bool disposing)
                {
                    if (!this.disposed)
                    {
                        if (disposing)
                        {
                        }
                    }

                    this.disposed = true;
                }

                # endregion
            }
        }

2.  A következő változók a Dal.cs fájl / a Fiókbeállítások módosítása:

        private string userName = "<your user name>";
        private string host = "<your host>";
        private string password = "<your password>";

3. Az alkalmazás használatához!

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan [használható MongoChef](documentdb-mongodb-mongochef.md) protokollal DocumentDB fiókkal támogatása MongoDB.

 
