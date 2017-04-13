<properties
    pageTitle="Mi történt a felhőalapú szolgáltatás project? | Microsoft Azure |} Visual Studio kapcsolt szolgáltatások"
    description="Mi történik a cloud services projekt, miután a csatlakozás a Visual Studio segítségével Azure tároló fiókhoz csatlakoztatott szolgáltatások leírása"
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Mi történt a cloud services project (Visual Studio Azure tároló csatlakoztatva szolgáltatás)?

## <a name="references-added"></a>Hivatkozások hozzáadása

Az Azure tároló NuGet csomag a Visual Studio projekthez való hozzáadásának.  
A csomag hozzáadja a következő .NET hivatkozások:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Kapcsolati karakterlánc hozzáadott Azure tárolására
A kijelölt tárterület-fiók csatlakozási karakterlánc és kulcs létrehozott elemeket. A következő fájlok végzett módosítások:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
