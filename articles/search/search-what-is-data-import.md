<properties
    pageTitle="Adatok fel az Azure keresési |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás"
    description="Megtudhatja, hogy miként tölthet fel adatokat az Azure keresési index."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Azure keresési feltölteni az adatokat
> [AZURE.SELECTOR]
- [– Áttekintés](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [TÖBBI](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Az adatok az Azure keresési levő feltöltése
Kétféleképpen az Azure keresési index az adatok kitöltéséhez. Az első lehetőség van manuálisan terjesztése az adatok be az index az Azure keresési [REST API -t](search-import-data-rest-api.md) vagy a [.NET SDK](search-import-data-dotnet.md). A második lehetőséget, ha [egy támogatott adatforrás mutasson](search-indexer-overview.md) a Azure keresési index, és mondja el a adatokat automatikusan olvashat be a a keresési szolgáltatás Azure keresési.

Ez az útmutató csak az adatok feltöltése (Ez csak támogatott a [REST API -val](search-import-data-rest-api.md) és a [.NET SDK](search-import-data-dotnet.md)) leküldéses mintája használatáról utasításokat terjed ki, de Ön is talál további információt az alábbi ki modell.

### <a name="push-data-to-an-index"></a>Index leküldéses adatok

Ezt a megközelítést utal, az adatok programozás útján küldene Azure keresési való kereséshez elérhetővé szeretné tenni. Nagyon kicsi késés követelményeknek megfelelő alkalmazások (például ha szinkronizálja a dinamikus készlet adatbázisok műveletek keres szükséges), a leküldéses modell nincs.

Használhatja a [REST API -t](https://msdn.microsoft.com/library/azure/dn798930.aspx) vagy a [.NET SDK](search-import-data-dotnet.md) index leküldéses az adatokat. Jelenleg nem támogatott eszköz a portálon keresztüli, hogy adatokat.

Ezt a megközelítést oka rugalmasabb, mint a leküldéses modell dokumentumokat is feltölthet, egyenként vagy kötegekben (legfeljebb 1000 köteg vagy 16 MB-os, amelyik korlát véget előbb). A leküldéses modell is lehetővé teszi a dokumentumok feltöltése a Azure keresési függetlenül attól, ahol az adatok.

### <a name="pull-data-into-an-index"></a>Adatokat olvashat be index

A leküldéses modell támogatott adatforrás bejárja, és automatikusan feltölti az adatokat az Azure keresési index meg. Indexelő nyomon követésével módosítása és törlése a meglévő dokumentumok felismerve új dokumentumok mellett, távolítsa el a szükséges a aktívan kezelése az adatokat a index.

Azure keresés ezt a funkciót alkalmazása *Indexelő*, jelenleg elérhető [Blob-tárolóhoz (előzetes verzió)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL-adatbázishoz, és az SQL Server Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)keresztül.

Az indexelő funkciókat van téve az [Azure-portálon](search-import-data-portal.md) is ahogy a [REST API -t](https://msdn.microsoft.com/library/azure/dn946891.aspx).
