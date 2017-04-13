<properties 
   pageTitle="Azure SQL-adatbázis teljesítmény betekintést |} Microsoft Azure" 
   description="Az SQL Azure-adatbázis nyújt segítséget azokat a területeket, amellyel aktuális lekérdezési teljesítmény teljesítmény eszközöket." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL-adatbázis teljesítmény betekintést

Azure SQL-adatbázis eszközeivel teljesítmény azonosítása és az adatbázisok teljesítményének javítása intelligens eszközbeállítási műveletek és javaslatok a megadásával. 

1. Tallózással keresse meg az adatbázis az [Azure-portálra](http://portal.azure.com) , és kattintson a **minden elérhető beállítás** > **Teljesítmény **  >  **áttekintése** a **Teljesítmény** lap megnyitásához. 


2. Nyissa meg az [SQL-adatbázis tanácsadó](#sql-database-advisor)a **javaslatok** elemre, majd kattintson a **lekérdezések** kattintva nyissa meg a [Lekérdezési teljesítmény betekintést](#query-performance-insight).

    ![Teljesítmény megtekintése](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Teljesítmény – áttekintés

**– Áttekintés** és a **Teljesítmény** hivatkozásra kattintva megnyílik a teljesítmény irányítópult az adatbázis. Ebben a nézetben a adatbázis teljesítményét foglalja össze, és teljesítményének javítása és hibaelhárítása nyújt segítséget. 

![Teljesítmény](./media/sql-database-performance/performance.png)

- A **javaslatok** csempére biztosít részletezzük, hogy az adatbázis javaslatok beállítása (felső 3 javaslatok jelennek meg esetén további). Ez a csempére megnyitja **Az SQL-adatbázis Advisor**. 
- A **tevékenység hangolása** csempét a folyamatban lévő és a kész, az adatbázis műveletek finombeállítása, biztosítva gyorsnézete beállítása a tevékenység előzményei az összefoglalását tartalmazza. Ez a mozaik gombra kattintva megjeleníti az adatbázis teljes eszközbeállítási előzményeinek megtekintése.
- Az **automatikus beállítása** csempe mutatja be automatikus javítása az adatbázis (mely eszközbeállítási műveletek vannak beállítva, hogy az adatbázis automatikusan alkalmazható). Ez a csempe kattintva megnyithatja az automatizálási konfigurációs párbeszédpanel.
- Az **adatbázis-lekérdezések** csempére az adatbázis (általános DTU használatát és a felső erőforrás használata más lekérdezések) a lekérdezési teljesítmény összefoglalása jeleníti meg. Ez a csempére megnyitja **Lekérdezési teljesítmény betekintést**.



## <a name="sql-database-advisor"></a>SQL-adatbázis Advisor


[SQL-adatbázis Advisor](sql-database-advisor.md) intelligens eszközbeállítási ajánlások az adatbázis teljesítmény javítása érdekében is biztosít. 

- Arra vonatkozó javaslatokat, amely indexek hozhat létre, vagy húzza (és a beállítást alkalmazni az index javaslatok automatikusan felhasználói beavatkozás és visszaállítása automatikusan, amelyek negatív hatással van a teljesítményre gyakorolt javaslatok nélkül).
- Ha az adatbázis megadott séma problémák javaslatok.
- Javaslatok a lekérdezések paraméteres lekérdezések előnyeivel.




## <a name="query-performance-insight"></a>Lekérdezési teljesítmény betekintést

[Lekérdezési teljesítmény betekintést](sql-database-query-performance.md) lehetővé teszi, hogy kevesebb időt vesz adatbázis teljesítményét hibaelhárítási, mert:

- Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést. 
- A felső Processzor használata más lekérdezéseket, amelyek esetleg a jobb teljesítmény elérése érdekében kell állítani. 
- Az azt jelenti, hogy részletesen megjelenítsen egy lekérdezés adatait. 


## <a name="additional-resources"></a>További források

- [Egyetlen adatbázisok Azure SQL-adatbázis teljesítmény útmutató](sql-database-performance-guidance.md)
- [Ha egy rugalmas adatbázis készlet használni?](sql-database-elastic-pool-guidance.md)