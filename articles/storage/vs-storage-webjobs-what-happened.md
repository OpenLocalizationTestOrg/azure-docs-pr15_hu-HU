<properties
    pageTitle="Mi történt a WebJob projekteket (Visual Studio Azure tároló csatlakoztatva szolgáltatás)? | Microsoft Azure"
    description="Mi történt az Azure WebJob projekt, miután részletesen a tárterület a Visual Studio segítségével a csatlakoztatott szolgáltatások leírása"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Mi történt a WebJob projekteket (Visual Studio Azure tároló csatlakoztatva szolgáltatás)?

## <a name="references-added"></a>Hivatkozások hozzáadása

Az Azure tároló NuGet csomag lett hozzáadva, vagy a Visual Studio projekt frissül.  
A csomag hozzáadja a következő .NET hivatkozások:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Kapcsolati karakterlánc hozzáadott Azure tárolására
A projekt App.config fájlban a **AzureWebJobsStorage** és **AzureWebJobsDashboard** bejegyzések frissített a kijelölt tárterület-fiók csatlakozási karakterlánc és billentyűt.

További információ az [Azure WebJobs dokumentáció forrásokban](http://go.microsoft.com/fwlink/?linkid=390226)talál.
