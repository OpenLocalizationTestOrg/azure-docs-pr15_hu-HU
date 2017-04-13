< tulajdonságai
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>A legújabb rugalmas adatbázis ügyfél könyvtár használata-alkalmazás frissítése

Új a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md) csak a Visual Studio NuGetPackage Manager felület NuGetand keresztül érhető el. Frissítések hibajavítás tartalmaz, és az ügyfél tár új funkciók támogatása.

**a legújabb verzió:** Nyissa meg a [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Az alkalmazás az új tárral újjáépítése, valamint módosíthatja a meglévő Shard térkép Manager metaadatainak tárolja az új funkciók támogatása a Azure SQL-adatbázisait.

Ezek a lépések elvégzéséhez sorrendben biztosítja, hogy az ügyfél tár régi verziói már nem szerepel a környezet metaadatok objektumok frissítésekor, ami azt jelenti, hogy a régebbi verziójú metaadat-objektumok nem jön létre frissítés után.   

## <a name="upgrade-steps"></a>Frissítési lépést

**1. a alkalmazások frissítése** A Visual Studióban töltse le és hivatkozás a ügyfél tár legújabb be az összes, amelyek a tár; fejlesztési projektjeit Ezután újraépítéséhez és terjesztése. 

 * A Visual Studio megoldás, válassza az **eszközök** --> **NuGet csomag Manager** -->  **NuGet csomagok kezelése megoldás**. 
 * (A visual Studio 2013) A bal oldali ablaktáblában válassza a **frissítések**, és válassza az **Azure SQL adatbázis rugalmas skála ügyfél tár** ablakban megjelenő csomag **frissítés** gombjára.
 * (A visual Studio 2015) Állítsa az mezőben **frissítés érhető el**. Jelölje ki a frissíteni csomagot, és kattintson a **frissítés** gombra.
    
 
 * Építse fel és telepítse. 

**2. a parancsfájlok frissíteni.** Ha kezelése shards, [Töltse le az új dokumentumtár-verziót](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) , és másolja a vágólapra a címtárban, amelyből parancsfájlok végrehajtása a **PowerShell** -parancsfájlokat szolgáltatást használ. 

**3. a felosztása és egyesítése szolgáltatásban frissíteni.** Ha a rugalmas adatbázis felosztása és egyesítése eszköz használatával átrendezése sharded adatokat, [Töltse le és telepítse az eszközt a legújabb verzióját](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). A szolgáltatás megtalálható a frissítési lépést részletes [Itt](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. a Shard térkép Manager-adatbázis frissítése**. A metaadatok támogatása a Shard térképek Azure SQL-adatbázis frissítése.  Kétféleképpen elvégezhető a PowerShell alrendszerrel vagy C#. Mindkét lehetőségek alatt.

***1 beállítás: A frissítés metaadat-alapú PowerShell használatával***

1. Töltse le a legújabb parancssori segédprogram NuGet az [Itt](http://nuget.org/nuget.exe) , és mentse egy mappába. 

2. Nyisson meg egy parancssort, nyissa meg azt a mappába, és adja ki a parancsot:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Nyissa meg az almappát, ahol az új ügyfél DLL-verziójának csak letöltötte, például:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Töltse le a rugalmas adatbázis ügyfélprogram-frissítési scriptlet a [Parancsprogram-központban](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), és mentse a abba a mappába, ahol az a DLL-szolgáltatásfájlját.

5. A mappából "PowerShell.\upgrade.ps1" futtassa a parancssorból, és kövesse a képernyőn megjelenő utasításokat.
 
***2 beállítás: A metaadatok C# frissítése***

Másik lehetőségként a, a ShardMapManager, függvény minden shards fölé, és a metaadat-alapú frissítés végrehajtása hívja fel a következő példának megfelelően módszerek [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) és [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) Visual Studio-alkalmazás létrehozása: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

A metaadat-frissítések technikákat alkalmazható többször kárt nélkül. Például ha már frissítése után a ügyfél régebbi verzióját véletlenül egy shard hoz létre, futtathatja frissítés újra minden shards annak érdekében, hogy a infrastruktúra egész jelen-e a legújabb metaadatok között. 

**Megjegyzés:**  Az ügyfél tár új verziója közzé a mai dátumig továbbra is dolgozhat a Shard térkép Manager metaadatok korábbi verzióival Azure SQL-adatbázis és fordítva.   Azonban bővítve kihasználhatja az új funkciók közül egyesek a legutóbbi ügyfélprogram, metaadatok kell frissíteni.   Figyelje meg, hogy metaadatokat frissítések nem befolyásolja bármelyik felhasználó- vagy az alkalmazás-specifikus adatok, csak objektumok a Shard térkép kezelő által létrehozott és használt.  És a további alkalmazások között a fentebb ismertetett frissítési folyamat működését. 

## <a name="elastic-database-client-version-history"></a>Rugalmas adatbázis ügyfél korábbi verziók 

Korábbi verziók kattintson a [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 