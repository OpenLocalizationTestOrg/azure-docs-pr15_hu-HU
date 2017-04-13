<properties 
   pageTitle="SQL-adatbázis Azure Advisor az Azure portálon |} Microsoft Azure" 
   description="Használhatja az Azure SQL-adatbázis Advisor az Azure-portálon tekintheti át és javaslatok a meglévő is gyorsításához aktuális lekérdezés SQL-adatbázisok végrehajtása." 
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
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL-adatbázis Advisor az Azure portál használatával

> [AZURE.SELECTOR]
- [SQL-adatbázis Advisor – áttekintés](sql-database-advisor.md)
- [Portál](sql-database-advisor-portal.md)

Használhatja az Azure SQL-adatbázis Advisor az Azure-portálon tekintheti át és javaslatok a meglévő is gyorsításához aktuális lekérdezés SQL-adatbázisok végrehajtása.

## <a name="viewing-recommendations"></a>Javaslatok megtekintése

A javaslatok lapon, ahol megtekintheti a felső javaslatok alapján potenciális hatásuk teljesítmény javítása érdekében. Megtekintheti a korábbi műveletek állapotát. Válasszon egy ajánlási vagy az állapot kapcsolatban további részletekre kíváncsi.

Megtekintése és javaslatok alkalmazni, rendelkeznie kell a megfelelő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) engedéllyel Azure-ban. **Olvasó**, **SQL-adatbázis közreműködői** engedélyek szükségesek nézet javaslatok és a **tulajdonos**, hajtsa végre az esetleges műveleteket; **SQL-adatbázis közreműködői** engedélyekre van szükség Hozzon létre, vagy húzza az indexek, és megszüntetése indexek létrehozása.

1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **További szolgáltatások** > **SQL-adatbázisait**, és jelölje ki azt az adatbázist.
5. Kattintson a **Teljesítmény ajánlási** a kijelölt adatbázis elérhető javaslatok megtekintése gombra.

> [AZURE.NOTE] Javaslatok-adatbázis eléréséhez kell lennie egy napjává használatát kapcsolatban, és szükség van néhány tevékenység. Is szükség van néhány egységes tevékenységet. Az SQL-adatbázis Advisor könnyebben optimalizálhatja az következetes lekérdezés mintákat, mint azt is, a tevékenység véletlen spotty felszakadásáig. Javaslatok nem érhetők el, ha a **Teljesítmény ajánlási** lap biztosítania kell mailbe arról, hogy miért üzenet.

![Javaslatok](./media/sql-database-advisor-portal/recommendations.png)

Íme egy példa "Séma a probléma megoldásához" ajánlása az Azure-portálon.

![Séma a probléma megoldásához](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Az alábbi négy kategóriákba teljesítményre gyakorolt hatásukat potenciális szerint rendezve jelennek meg javaslatok:

| Hatás | Leírás |
| :--- | :--- |
| Magas | Magas hatása javaslatok meg kell határoznia a legjelentősebb teljesítményre gyakorolt hatás. |
| Közepes | Közepes hatása javaslatok kell javíthatja a teljesítményt, azonban nem jelentősen. |
| Alacsony | Alacsony hatása javaslatok biztosítania kell jobb teljesítményt nélkül, de javítása nem lehet jelentős. 


### <a name="removing-recommendations-from-the-list"></a>Javaslatok eltávolítása a listából

Ha a javaslatok listáját a listából eltávolítani kívánt elemeket tartalmaz, az ajánlási elvetheti:

1. Jelölje ki a **javaslatok**listáját ajánlást.
2. **Elvetéséhez** kattintson a **Részletek** lap.


Ha azt szeretné, felveheti elvetett elemek vissza a **javaslatok** listában:

1. Kattintson a **javaslatok** a lap **Nézet figyelmen kívül**.
1. Jelölje ki a elvetett elemet a listában, és a részletek megtekintéséhez.
1. Másik lehetőségként kattintson a **Elvetése visszavonása** állítsa vissza az index **javaslatok**fő listába való.



## <a name="applying-recommendations"></a>Javaslatok alkalmazása

SQL-adatbázis Advisor lépve teljes hozzáféréssel hogyan javaslatok vannak engedélyezve, használja a következő három lehetőség közül: 

- Egyszerre csak egy adott javaslatok alkalmazása
- Javaslatok az alkalmazás automatikusan alkalmazza a tanácsadó engedélyezése (jelenleg csak az index javaslatok vonatkozik).
- Manuális megvalósítását ajánlást, futtassa az ajánlott az SQL-T parancsfájlt az adatbázist.

Jelölje ki bármelyik ajánlást, a részletek megtekintéséhez, és kattintson a **Nézet parancsfájl** , tekintse át az ajánlási létrehozásának módjától pontos részleteit.

Az adatbázis online marad, amíg a advisor alkalmazza az ajánlási – használata SQL-adatbázis Advisor soha nem veszi adatbázis offline.

### <a name="apply-an-individual-recommendation"></a>Az egyes ajánlási alkalmazása

Tekintse át, és fogadja el a javaslatok egy egyszerre.

1. Kattintson a **javaslatok** lap ajánlást.
2. A **Részletek** lap kattintson az **Alkalmaz**gombra.

    ![Javaslat alkalmazása](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Indexek automatikus kezelése engedélyezése

Beállíthatja, hogy a SQL-adatbázis Advisor javaslatok automatikusan végrehajtásához. Amint elérhetővé válnak a javaslatok a program automatikusan alkalmazza őket. Az összes index művelet a szolgáltatás által felügyelt, ha a teljesítményre gyakorolt hatás negatív az ajánlási visszaáll.

1. Kattintson a **javaslatok** lap **automatizálás**:

    ![Advisor beállításai](./media/sql-database-advisor-portal/settings.png)

2. A tanácsadó **létrehozása** vagy **leválasztása** indexek automatikus beállítása:

    ![Indexek ajánlott](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Kézi futtatása ajánlott az SQL-T

Jelölje ki bármelyik ajánlást, és kattintson a **Nézet parancsfájl**. Futtassa a parancsfájlt manuálisan alkalmazza az ajánlási az adatbázist.

*Az indexek, amely a kézi végrehajtás nem felügyelt és szolgáltatás teljesítményt érvényesített* , azt adja meg a teljesítmény nyereség és módosíthatja vagy törölheti őket, ha szükséges ellenőrzéséhez létrehozását követően figyelheti a indexeknek alakot. Létrehozásával kapcsolatban további tájékoztatást az indexek olvassa el a [CREATE INDEX (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms188783.aspx)című témakört.


### <a name="canceling-recommendations"></a>Javaslatok megszakítása

A **függőben lévő** **ellenőrzése**és **sikeres** állapotban ajánlások megszakítható. Javaslatok állapotban **Executing** , nem lehet törölni.

1. Ajánlást a területen jelölje be **Előzmények beállítása** a **javaslatok részletek** lap megnyitásához.
2. Kattintson a **Mégse gombra** a folyamat az ajánlási alkalmazása gombra.



## <a name="monitoring-operations"></a>Műveletek figyelése

Ajánlást alkalmazása nem kerül sor azonnal. A portál ajánlási műveletek állapotának kapcsolatos részleteket. Az index lehet a lehetséges állapotok a következők:

| Állapot | Leírás |
| :--- | :--- |
| Függőben lévő | Javaslat parancs érkezett, és van ütemeznie vonatkoznak. |
| Végrehajtása | Az ajánlási van alkalmazva. |
| A siker | Javaslat sikeres volt alkalmazott. |
| Hibaüzenet | Hiba történt az ajánlási alkalmazása a folyamat során. Ez lehet egy tranziens problémát, illetve esetleg a séma módosítása a táblázatra, és a parancsfájl már nem érvényes. |
| Visszaállítás | Az ajánlási alkalmaztak, de nem performant lett tekinteni, és automatikusan vissza. |
| Visszaállítás | Az ajánlási vissza lett. |

Kattintson a folyamat ajánlási a lista kapcsolatban további részletekre kíváncsi:

![Indexek ajánlott](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Ajánlást visszaállítása

Ha a tanácsadó a ajánlási alkalmazása (azaz a manuális nem futtatta a T-SQL nyelvben parancsfájl) azt automatikusan visszaáll, ha úgy találja, hogy a teljesítményre gyakorolt hatás negatív. Ha valamilyen okból egyszerűen vissza kíván ajánlást végezheti el a következőket:


1. Sikeresen alkalmazott ajánlást a területen jelölje be **Előzmények hangolása** .
2. Kattintson a **Visszaállítás** az **ajánlási részletek** lap a.

![Indexek ajánlott](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Index ajánlások teljesítményt figyelése

Javaslatok sikeresen végrehajtása után (jelenleg tárgymutató-műveletek és lekérdezések javaslatok csak paraméterezni), hogy kattint az ajánlási Részletek lap nyissa meg az [Összefüggéseket a lekérdezési teljesítmény](sql-database-query-performance.md) és a teljesítmény hatása a legnépszerűbb lekérdezések **Lekérdezést az összefüggéseket** .

![Monitor teljesítményre gyakorolt hatás](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Összefoglalás

SQL-adatbázis Advisor SQL-adatbázis teljesítmény javítására javaslatokat tesz. Mert az SQL-T parancsfájlok, egyéni és a teljesen automatikus (jelenleg csak index), valamint a advisor itt optimalizálása az adatbázist, és végül a lekérdezési teljesítmény javítására hasznos segítséget.



## <a name="next-steps"></a>Következő lépések

Figyelheti a javaslatok, és alkalmazza őket a teljesítmény finomíthatja továbbra is. Adatbázis-munkaterhelésekből, dinamikus és folyamatosan módosítása. SQL-adatbázis advisor figyelheti, és adja meg a javíthatja a teljesítményt az adatbázis ajánlások továbbra is. 

 - Lásd: [Az SQL-adatbázis Advisor](sql-database-advisor.md) SQL-adatbázis Advisor áttekintése.
 - Lásd: a [Lekérdezési teljesítmény Hírcsatornájában](sql-database-query-performance.md) , ha többet szeretne tudni a teljesítmény hatása a legnépszerűbb lekérdezések megtekintése.

## <a name="additional-resources"></a>További források

- [Lekérdezés áruházból](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEX LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms188783.aspx)
- [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)






