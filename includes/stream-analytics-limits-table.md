<properties 
   pageTitle="Értékáram-elemzés korlátozza a táblázat"
   description="Rendszer korlátai és ajánlott méretben Értékáram-elemzés összetevők és a kapcsolatokkal ismerteti."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Korlát azonosító | Határérték       | Megjegyzések |
|----------------- | ------------|--------- |
| Régió száma előfizetésenként Streaming egységek maximális száma | 50 | Túl az 50 előfizetéshez tartozó adatfolyam egységek növelése kérelmének [Microsoft ügyfélszolgálat](https://support.microsoft.com/en-us)megkeresésével tehetők. |
| A folyamatos átvitelű egységek maximális sebesség | 1MB / s * | Egy SZU maximális sebesség attól függ, hogy az alkalmazási példát. Tényleges átvitel lehet alsó és összetettsége lekérdezés és a szétválasztás attól függ, hogy. További részletek [skála Azure Értékáram-elemzés feladatok növelheti a teljesítményt](../articles/stream-analytics/stream-analytics-scale-jobs.md) című témakörben találhatók. |
| Feladatonkénti bemenetben maximális száma | 60 | Értékáram-elemzés Feladatonkénti 60 ráfordítások kemény korlátozva van. |
| Kimeneti értékeket Feladatonkénti maximális száma | 60 | Értékáram-elemzés Feladatonkénti 60 kimeneti értékeket a merevlemez korlátozva van. |
| Feladatonkénti függvények maximális száma | 60 | Értékáram-elemzés Feladatonkénti 60 függvények kemény korlátozva van. |
| Régió száma feladatok maximális száma | 1500 | Előfordulhat, hogy egyes előfizetések legfeljebb 1500 feladatok per földrajzi területhez tartozik. |