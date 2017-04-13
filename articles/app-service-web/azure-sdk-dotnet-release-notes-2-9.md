<properties 
   pageTitle="A .NET rendszerhez 2.9 Azure SDK – kibocsátási megjegyzések" 
   description="A .NET rendszerhez 2.9 Azure SDK – kibocsátási megjegyzések" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-29-release-notes"></a>A .NET rendszerhez 2.9 Azure SDK – kibocsátási megjegyzések

##<a name="overview"></a>– Áttekintés

A dokumentum tartalmaz a kibocsátási megjegyzések az Azure SDK .NET 2.9 kiadását. 

Ebben a kiadásban frissítéseiről részletes tudnivalókért lásd az [Azure SDK 2.9 bejelentése közzé](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/).

## <a name="azure-sdk-29-for-visual-studio-2015-update-2-and-visual-studio-15-preview"></a>Azure SDK 2.9 for Visual Studio 2015 Update 2-es vagy Visual Studio "15" megtekintése
 
A frissítés a következő hibajavítás tartalmazza:

- VIGYE az API-ügyfél generációs kapcsolatos problémát, amelyben az "Ismeretlen típusú" karakterlánc módon jelennek meg a kód-gen mappa nevét, illetve a nevét a névtér kihagyott be a létrehozott kódot.
- Ütemezett WebJobs, amelyben a hitelesítési adatait sikertelen volt az ütemező folyamat kiépítési átadandó kapcsolatos problémát.

A frissítés tartalmazza a következő új szolgáltatást:

- A "Szolgáltatások" lap alkalmazás szolgáltatás kiépítési párbeszédpanel a másodlagos alkalmazás szolgáltatások támogatása. 

##<a name="azure-data-lake-tools-for-visual-studio-2015-update-2"></a>Azure adatok tó Tools for Visual Studio 2015 frissítése 2
 
A frissítés többek között a következők:

- **Azure tó Adateszközök** for Visual Studio .NET kiadását az Azure SDK most egyesítve lesz. Az eszköz automatikusan települ, ha telepíti az Azure SDK csomagjában talál. 

    Az eszköz frissül gyakran, válassza [az alábbi](http://aka.ms/datalaketool) a frissítések letöltéséhez.

- **Server Explorer** most funkcióval az összes megtekintése és létrehozása az egyes U-SQL nyelvben metaadatok szervezetek. További tudnivalókért lásd: a [blogban](https://azure.microsoft.com/documentation/services/data-lake-analytics/) .


##<a name="hdinsight-tools"></a>HDInsight eszközök 

**HDInsight Tools** for Visual Studio támogatja HDInsight verzió 3.3, például megjelenítő Tez diagramok és más nyelvi kijavítja.


##<a name="azure-resource-manager"></a>Azure erőforrás-kezelő 

Ebben a kiadásban ARM sablonok [KeyVault](../resource-manager-keyvault-parameter.md) támogatása felveszi.

##<a name="see-also"></a>Lásd még:

[Azure SDK 2.9 bejelentése bejegyzésben](https://azure.microsoft.com/blog/announcing-visual-studio-azure-tools-and-sdk-2-9/)
