<properties 
    pageTitle="Rugalmas Adatbáziseszközök – első lépések" 
    description="Egyszerű magyarázat rugalmas adatbázis eszközök funkció Azure SQL-adatbázis, például egyszerű példa alkalmazás futtatásához." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Rugalmas Adatbáziseszközök – első lépések

A dokumentum megismerkedhet a fejlesztők számára a minta app futtatásával. A minta létrehoz egy egyszerű sharded alkalmazást, és tallózása rugalmas Adatbáziseszközök főbb szolgáltatásait. A példa bemutatja a [Rugalmas adatbázis ügyfél tár](sql-database-elastic-database-client-library.md) függvények

A tár telepítéséhez lépjen [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Figyelje meg, hogy a tárban telepítve van-e az alábbiakban ismertetett minta alkalmazásban.

## <a name="prerequisites"></a>Előfeltételek

1. Visual Studio 2012 vagy nagyobb a és C# szükség. Töltse le a [Visual Studio tölthető le](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)egy ingyenes verzióban.
2. Nuget 2.7 vagy újabb verziójában. A legújabb verzióra című témakörben kaphat [NuGet telepítése](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Töltse le és futtassa a minta alkalmazás

A **Azure SQL rugalmas adatbázis – első lépések** minta bemutatja az Azure SQL rugalmas adatbázis eszközökkel sharded alkalmazások fejlesztésének tapasztalnak a legfontosabb szempontjait. Fő használati eset [shard megfeleltetése kezelése](sql-database-elastic-scale-shard-map-management.md), a [függő útválasztási adatok](sql-database-elastic-scale-data-dependent-routing.md) és a [több elem shard lekérdezése](sql-database-elastic-scale-multishard-querying.md)koncentrál. Töltse le, és futtassa a minta, kövesse az alábbi lépéseket: 

1. Nyissa meg a Visual Studio, és válassza a **Fájl -> Új projekt ->**.
2. A párbeszédpanelen kattintson az **Online**elemre.

    ![Új projekt > Online][2]
3. Kattintson a **Visual C#** **minták**csoportban.

    ![Kattintson a vizuális C#][3]
4. A Keresés mezőbe írja be a **rugalmas db** a minta kereséséhez. A cím **Rugalmas DB eszközök az Azure SQL - az első lépések** megjelenik.

    ![Keresés mező][1]
 
5. Jelölje ki a minta, válassza ki a nevét, és az új projekt helyét, és nyomja meg az **OK gombra** a projekt létrehozása.
6. Nyissa meg a megoldást a minta projekthez a **app.config** fájlt, és kövesse a fájlt a az Azure SQL-adatbázis-kiszolgáló neve és a bejelentkezési adatokat (felhasználónév és jelszó).
7. Építse fel, és futtassa az alkalmazást. Amikor a rendszer, hagyjon Visual Studio visszaállítani a NuGet csomagok a megoldás. Ez a rugalmas adatbázis ügyfél tár legújabb verziójának letölthető NuGet.
8. Játszhat le, ha többet szeretne tudni az ügyfél tár funkciók más-más beállításokat. Megjegyzés: a lépéseket, az alkalmazás megnyitja a konzolhoz kimeneti, és nyugodtan feltárása a kódot a színfalak mögött.

    ![folyamatban][4]

Gratulálunk – sikeresen beépített és az első az Azure SQL-adatbázis használata rugalmas Adatbáziseszközök sharded alkalmazásnak a futtatására. Rövid nézze meg a Visual Studio vagy SQL Server Management Studio az Azure-adatbázis kiszolgálóhoz való csatlakozás által létrehozott a minta shards. Megfigyelheti, hogy új minta shard adatbázisok és egy shard térkép manager-adatbázist, amely a minta hozott létre.

> [AZURE.IMPORTANT] Javasoljuk, hogy mindig a legújabb verzióját használja Management Studio maradjon szinkronizálja a frissítéseket és a Microsoft Azure SQL-adatbázis. Az [SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>A kód minta kulcsfontosságú

1. **Shards kezelése és Shard térképek**: A kód azt szemlélteti, hogyan dolgozhat shards, tartományok, és a hozzárendelések fájl **ShardMapManagerSample.cs**. Itt ebben a témakörben további információt talál: [Shard térkép kezelése](http://go.microsoft.com/?linkid=9862595).  
2. **Függő útválasztási adatok**: a megfelelő shard tranzakciók útválasztás **DataDependentRoutingSample.cs**látható. További részletekért olvassa el [a függő útválasztási adatok](http://go.microsoft.com/?linkid=9862596). 
3. **Lekérdezés több Shards keresztül**: a fájl **MultiShardQuerySample.cs**eset lekérdezés shards keresztül. További információra kíváncsi olvassa el a [Több elem Shard lekérdezése](http://go.microsoft.com/?linkid=9862597)című témakört.
4. **Üres shards hozzáadása**: új, üres shards közelítéses hozzáadása végzi a fájlban **AddNewShardsSample.cs**kódot. Ez a témakör részleteit tartoznak: [Shard térkép kezelése](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Más skála rugalmas műveletek

1. **Egy meglévő shard felosztása**: A videofunkcióinak shards felosztása **felosztása és egyesítése eszköz**megadva. Itt az eszközről további információt találhat: [Áttekintés eszköz felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Meglévő shards egyesítése**: Shard összevonása is megtörténik a **felosztása és egyesítése eszköz**használatával. További tudnivalókért tekintse át: [Áttekintés eszköz felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Költség

A rugalmas Adatbáziseszközök ingyenes. Rugalmas Adatbáziseszközök nem ír elő további díjak fölött a az Azure használati költséget. 

A minta alkalmazás például hoz létre az új adatbázisok. A költség attól függ, az Azure SQL-adatbázis adatbázis edition úgy dönt, és az Azure az alkalmazás használatát.

Árak információkat talál [SQL-adatbázis árak részleteket](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Következő lépések
A rugalmas Adatbáziseszközök kapcsolatos további tudnivalókért lásd:

* [Rugalmas adatbázis eszközök dokumentáció térkép](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Mintakódok: 
    -    [Rugalmas DB az Azure SQL - az első lépések](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Az Azure SQL - entitás keretrendszer integrálása rugalmas DB](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [A parancsprogram-központban shard rugalmasság](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [skála rugalmas bejelentése](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Csatorna 9: [skála rugalmas áttekintő videó](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Vitafórum-fórum: [Azure SQL-adatbázis-fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Teljesítmény mérésére: [Teljesítmény számláló shard térkép Manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
