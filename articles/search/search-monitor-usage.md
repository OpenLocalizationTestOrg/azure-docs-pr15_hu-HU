<properties 
   pageTitle="Figyelheti a használatát és az Azure keresési szolgáltatás statisztika |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás" 
   description="Azure keresés, a Microsoft Azure szolgáltatott felhő keresési szolgáltatásának nyomon követheti a erőforrás felhasználás és index méretét." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Használat figyelése és az Azure keresési szolgáltatás statisztika

Indexek és a dokumentum mérete értéknövekedésével követés segíthetnek ezzel kapcsolatban beérkező módosítsa a kapacitás előtt szerezze meg a felső határ létrehozta a szolgáltatáshoz. 

Erőforrás-kihasználtság követésére száma és a szolgáltatás statisztikáját könnyen megtekinthetők az [Azure-portálon](https://portal.azure.com), de is töltheti a programozás útján készítésekor egy egyéni szolgáltatás felügyeleti eszközt. Ez a cikk bemutatja a lépéseket, mindkét módszer.

Az új keresési forgalom analytics funkció az index szintre tevékenység az összefüggéseket is használhatja. Látogasson el a [Keresés forgalom elemzésének Azure keresési](search-traffic-analytics.md) kezdéshez.

##<a name="view-counts-and-metrics-in-the-portal"></a>Megszámolja és mérőszámok megtekintése a portálon 

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com). 

2. Nyissa meg a szolgáltatás irányítópult az Azure keresési szolgáltatás. Mozaikok a szolgáltatás a Kezdőlap lapon található, vagy tallózással is a szolgáltatást a Tallózás a JumpBar gombjára. Lásd: a [Create szolgáltatás](search-create-service-portal.md) részletes útmutatást.

A használata című szakaszában tartalmaz egy állapotjelzője, amely közli, hogy milyen rendelkezésre álló erőforrásokat részét van használatban.

  ![][1]

Visszahívása, hogy a megosztott szolgáltatások van-e több kópia legfeljebb, és minden egyes partition. Ezenkívül azt 10 000 dokumentumokat csak támogatása a teljes vagy 50 MB adatot, amelyik véget előbb.

##<a name="get-index-statistics-using-the-rest-api"></a>Ismerkedés a REST API index statisztika

Az Azure keresési REST API-val és a .NET SDK is szolgáltatás mértékek programozott hozzáférést biztosít.  Alkalmazás használatakor [Indexelő](https://msdn.microsoft.com/library/azure/dn946891.aspx) való betöltéséhez index Azure SQL-adatbázis vagy DocumentDB, egy további API van szüksége a számok megszerezni érhető el. 

  + [Index statisztika beszerzése](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Dokumentumok száma](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Ismerkedés az indexelési állapot](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Következő lépések

Tekintse át a [korlátai és kapacitásbeli](search-limits-quotas-capacity.md) partíciók, ha szüksége, ha meglévő kapacitás nem elegendő kópiák határozza meg. 

Látogasson el a [kezelése a keresési szolgáltatás a Microsoft Azure](search-manage.md) szolgáltatás adminisztrációja további információt.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
