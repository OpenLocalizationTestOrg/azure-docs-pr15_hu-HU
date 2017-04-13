<properties
    pageTitle="Mi történt a ASP.NET 5 projekteket (Visual Studio csatlakoztatott szolgáltatások) |} Microsoft Azure-tárolóhoz"
    description="Ismerteti, hogy mi történik, miután a csatlakozás Visual Studio segítségével a Visual Studio ASP.NET 5 projekt Azure tároló fiókhoz kapcsolt szolgáltatások"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Mi történt a ASP.NET 5 projekteket (Visual Studio Azure tároló csatlakoztatva services)?

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

Emellett a NuGet csomag **Microsoft.Framework.Configuration.Json** hozzá lett adva.

## <a name="connection-string-for-azure-storage-added"></a>Kapcsolati karakterlánc hozzáadott Azure tárolására
A projekt config.json fájlban a tároló kijelölt fiók kapcsolati karakterláncot, és a billentyűk elem jött létre.

További tudnivalókért lásd: az [ASP.NET 5](http://www.asp.net/vnext).
