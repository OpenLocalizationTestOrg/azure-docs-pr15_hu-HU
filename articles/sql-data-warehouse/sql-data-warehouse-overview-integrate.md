<properties
   pageTitle="Az SQL adatraktár integrált megoldások |} Microsoft Azure"
   description="Az eszközök és a partnerek SQL adatraktár integrálása megoldásokkal. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Kihasználhatja az SQL adatraktár más szolgáltatások
Az alapvető funkcionalitást kívül SQL adatraktár lehetővé teszi a felhasználóknak az egyéb szolgáltatásokat az Azure mellette számos kihasználhatja.  Kifejezetten hogy jelenleg léptek mélyen integrálása az alábbi lépéseket:

+ A Power BI
+ Azure Data Factory
+ Azure gépi tanulási
+ Azure Értékáram-elemzés

Dolgozunk az való csatlakozáskor további szolgáltatások Azure ökológiai keresztül.

##<a name="power-bi"></a>A Power BI
A Power BI-integráció lehetővé teszi, a számítási power SQL adatraktár dinamikus jelentéskészítéssel, és a képi megjelenítések a Power BI használhatja. A Power BI-integráció jelenleg tartalmazza:

+ **Közvetlen csatlakozás**: egy speciális kapcsolatot a logikai leküldéses közzététele SQL adatraktár szemben.  Nagyobb skálával gyorsabb analysis ez biztosít.
+ **Nyissa meg a Power BI**: A "Megnyitás a Power BI" gomb átadja a példány adatait a Power BI, lehetővé tevő zökkenőmentesebb kapcsolat.

Lásd: [A Power BI](./sql-data-warehouse-integrate-power-bi.md) vagy további információt a [Power BI dokumentációt](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) .

##<a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory egy felügyelt platform összetett kivonat-terhelés folyamatok létrehozásához nyújt a felhasználóknak.  SQL adatok raktári funkciók integrálása az Azure Data Factory az alábbiakat tartalmazza:

+ **Tárolt eljárás**: téve a SQL adatraktár tárolt eljárások végrehajtását.
+ **Másolás**: az adatok áthelyezése SQL adatraktár ADF használatát.  Ez a művelet a csoportban a fedőlap ADF a szokásos adatok mozgását mechanizmusa vagy PolyBase is használhat. 

Lásd: [Azure Data Factory integrálás](./sql-data-warehouse-integrate-azure-data-factory.md) vagy az [Azure Data Factory dokumentációt](https://azure.microsoft.com/documentation/services/data-factory/) .

##<a name="azure-machine-learning"></a>Azure gépi tanulási
Azure gépi tanulási olyan teljes körű felügyelt analytics szolgáltatás, amely lehetővé teszi a felhasználóknak, hogy egy nagy eszközkészletet prediktív vizsgál bonyolult modellek létrehozása.  SQL adatraktár támogatott a forrás- és a más rendeltetési helyet az ezek a modellek, az alábbi funkciókat:

+ **Olvassa el az adatok:** Az adatmodellek meghajtó a méretezés az SQL-T használata SQL adatraktár szemben.
+ **Adatok írásához:** A módosítások véglegesítése bármely modellből vissza az SQL adatraktár.

Lásd: [Azure gépi tanulási integrálás](./sql-data-warehouse-integrate-azure-machine-learning.md) vagy az [Azure gépi tanulási dokumentációt](https://azure.microsoft.com/services/machine-learning/) .

##<a name="azure-stream-analytics"></a>Azure Értékáram-elemzés
Azure Értékáram-elemzés egy összetett, teljes körű felügyelt infrastruktúra feldolgozás és Azure-esemény központban létrehozott esemény adatok fogyasztása.  A folyamatos átvitelű adatok hatékony feldolgozott és relációs adatok mélyebb, speciális adatelemzési engedélyezése mellett tárolása SQL adatraktár integrációja tesz lehetővé.  

+ **Kimeneti feladat:** Értékáram-elemzés feladatok közvetlenül az SQL adatraktár kimeneti küldése

Lásd: [Azure Értékáram-elemzés integrálás](./sql-data-warehouse-integrate-azure-stream-analytics.md) vagy az [Azure Értékáram-elemzés dokumentációt](https://azure.microsoft.com/documentation/services/stream-analytics/) .

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
