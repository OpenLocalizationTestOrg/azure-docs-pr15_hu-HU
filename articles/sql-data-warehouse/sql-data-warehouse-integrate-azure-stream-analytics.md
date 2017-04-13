<properties
   pageTitle="Azure Értékáram-elemzés használata SQL adatraktár |} Microsoft Azure"
   description="Tippek az Azure Értékáram-elemzés és az Azure SQL-adatraktár megoldások fejlesztésével."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Azure Értékáram-elemzés használata SQL adatraktár

Azure Értékáram-elemzés olyan teljes körű felügyelt szolgáltatás, alacsony-késés, könnyen hozzáférhető, méretezhető összetett esemény feldolgozása kezeléséről a felhőben adatfolyam fölé. Alapvető olvasásával [Azure Értékáram-elemzés – bevezetés][]című témakörben. Értékáram-elemzés egy végpont megoldás létrehozása az [Azure Értékáram-elemzés használatának első lépései][] oktatóprogram követve majd talál.

Ebben a cikkben megtanulhatja, hogyan szeretné alkalmazni az Azure SQL-adatraktár adatbázis-kimeneti gyűjtő a gőzzel Analytics feladatokat.

## <a name="prerequisites"></a>Előfeltételek

Először futtassa a következő műveletek elvégzéséhez az [Azure Értékáram-elemzés használatának első lépései][] oktatóprogram során.  

1. Hozzon létre egy esemény központi beviteli
2. Konfigurálhatja és megkezdheti az esemény-készítő alkalmazás
3. A Értékáram-elemzés feladatok rendelkezés
4. Adja meg a projekt beviteli és a lekérdezés

Ezután az Azure SQL-adatraktár-adatbázis létrehozása

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Adja meg a projekt kimenet: Azure SQL-adatraktár adatbázis

### <a name="step-1"></a>Lépés: 1

A Értékáram-elemzés feladatok a **kimeneti** a lap tetején kattintson, és válassza a **Kimeneti hozzáadása**gombra.

### <a name="step-2"></a>Lépés: 2

Jelölje ki az SQL-adatbázishoz, és kattintson a Tovább gombra.

![][add-output]

### <a name="step-3"></a>3 lépés
Írja be az alábbi értékeket, a következő lapon:

- *Kimeneti Alias*: írja be a feladat kimeneti rövid nevét.
- *Előfizetés*:
    - Ha az adatraktár SQL-adatbázis, a megjelenítő Értékáram-elemzés feladatot azonos előfizetésben, jelölje be az SQL-adatbázis használata aktuális előfizetése.
    - Ha az adatbázis egy másik előfizetést, jelölje be a SQL-adatbázis használata másik előfizetésből.
- *Adatbázis*: Adja meg a céladatbázist nevét.
- *Kiszolgáló neve*: Adja meg az imént megadott adatbázis-kiszolgáló megadásáról. Ez keresése az Azure klasszikus Portal segítségével.

![][server-name]

- *Felhasználónév*: Adja meg a felhasználónevet, az adatbázis írási engedélyekkel rendelkező fiókkal.
- *Jelszó*: az adott felhasználói fiók adja meg a jelszót.
- *Táblázat*: Adja meg a cél táblához nevét az adatbázisban.

![][add-database]

### <a name="step-4"></a>Lépés: 4

Kattintson a feladat kimeneti hozzáadni, és ellenőrizze, hogy Értékáram-elemzés is tud csatlakozni az adatbázis a ellenőrzése gombra.

![][test-connection]

Az adatbázishoz a kapcsolat létrejött, amikor a képernyő alján a portál értesítés jelenik meg. A képernyő alján az adatbázishoz a kapcsolat tesztelése kapcsolat tesztelése gombra.

## <a name="next-steps"></a>Következő lépések

Integráció áttekintést [SQL adatraktár integrációs áttekintése][]című témakörben találhat.

Fejlesztési vonatkozó további tippek [SQL adatraktár fejlesztési áttekintése][]című témakörben találhat.

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Azure Értékáram-elemzés – bevezetés]: ../stream-analytics/stream-analytics-introduction.md
[Értékáram-elemzés Azure használatának első lépései]: ../stream-analytics/stream-analytics-get-started.md
[SQL-adatraktár fejlesztése – áttekintés]:  ./sql-data-warehouse-overview-develop.md
[SQL-adatraktár integrációs – áttekintés]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
