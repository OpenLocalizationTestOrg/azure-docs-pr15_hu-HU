<properties
   pageTitle="Az SQL-összekötő használata összefüggés-alkalmazások |} Microsoft Azure alkalmazás szolgáltatás"
   description="Hogyan hozhat létre, és állítsa be az SQL-összekötő vagy API alkalmazást, és vele az Azure alkalmazás szolgáltatás összefüggés-alkalmazásban"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="sameerch"/>


# <a name="get-started-with-the-microsoft-sql-connector-and-add-it-to-your-logic-app"></a>Első lépések a Microsoft SQL-összekötő, és adja hozzá az összefüggés-alkalmazás
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik. A séma Azure SQL-2015-08-01-előzetes verziója kattintson [Az SQL Azure-API](../connectors/connectors-create-api-sqlazure.md).

Csatlakozás egy helyszíni SQL Server vagy az Azure SQL-adatbázis létrehozása és módosítása a információk vagy adatok. Összekötők logika alkalmazások beolvasásához, folyamat és adatok leküldéses "munkafolyamat" részeként használható. Az SQL-összekötő használatakor az munkafolyamatokban esetek számos érhet el. Ha például közül választhat:

- A webhely vagy mobilalkalmazás használata SQL-adatbázisban található az adattartomány jelenítik meg.
- Adatok beillesztése egy SQL-adatbázis táblájának tárolás. Például alkalmazotti rekordok adja meg, frissítse az értékesítési rendelések, és így tovább.
- Adatok beolvasása SQL, és az üzleti folyamatok használni. Például lehet felvenni az ügyfélről nyilvántartott és ügyfelek rekordjait SalesForce elhelyezni.

Az SQL-összekötő is adhat az üzleti munkafolyamatok és a folyamat adatok ezzel a munkafolyamattal összefüggés-alkalmazásból részeként. 

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
*Eseményindítók* , amelyek akkor aktiválódnak események. Ha például a megfelelő sorrendben frissítésekor, vagy ha új ügyfél kerül. A *művelet* eredménye az eseményindító. Ha például megrendelés frissítésekor értesítést küld az Üzletkötő. Másik lehetőségként új ügyfél hozzáadásakor üdvözlő e-mail küldése az új ügyfélnek.

Az eseményindító vagy egy logikai alkalmazás támogatja az adatok és az JSON és az XML formátumok a művelet az SQL-összekötő is használható. Hogy minden táblának a csomagban lévő beállítások (további tudnivalókat, amely a témakör későbbi) van JSON egy sor és az XML műveletsorozat.

Az SQL-összekötő a következő eseményindítók és elérhető műveletek foglalja magában:

Eseményindítók | Műveletek
--- | ---
A szavazás adatok | <ul><li>Táblázat beszúrása</li><li>Tartalomjegyzék frissítése</li><li>Jelölje be a táblázatból</li><li>Táblázat törlése</li><li>Hívja fel a tárolt eljárás</li></ul>

## <a name="create-the-sql-connector"></a>Az SQL-összekötő létrehozása

Összekötő összefüggés-alkalmazásból hozhatók létre, vagy közvetlenül a a Microsoft Azure piactéren lévő hozható létre. Összekötő létrehozása a piactérről:  

1. Válassza az Azure startboard **piactérről**.
2. "SQL-összekötő" keresni, jelölje ki azt, és válassza a **Create**.
3. Írja be a nevét, alkalmazás szolgáltatás tervezése és egyéb tulajdonságait.
4. Írja be a következő csomag beállításokat:

    név | Szükséges |  Leírás
--- | --- | ---
Kiszolgálónév | igen | Írja be az SQL Server nevét. Ha például írja be az *SQL Server/sqlexpress* vagy *SQLserver.mydomain.com*.
Port | nem | Alapértelmezett érték 1433.
Felhasználónév | igen | Írja be a felhasználónév jelentkezhet be az SQL Server. Ha egy helyszíni SQL Server csatlakozni, írja be a SQL-hitelesítés hitelesítő adatait.
Jelszó | igen | Írja be a felhasználónevét és jelszavát.
Az adatbázis neve | igen | Adja meg az adatbázishoz csatlakozik. Ha például *ügyfelei* vagy *dbo/rendelések*adhat meg.
A helyszíni | igen | Alapértelmezett értéke hamis. Írja be a hamis, ha egy Azure SQL-adatbázisokhoz való csatlakozási. Adja meg a igaz, ha a Kapcsolódás egy helyszíni SQL Server.
Szolgáltatás Bus kapcsolat-karakterlánc | nem | Ha a helyszíni kapcsolódik, adja meg a szolgáltatás Bus továbbítási kapcsolati karakterláncot.<br/><br/>[A hibrid kezelővel](app-service-logic-hybrid-connection-manager.md)<br/>[Bus árak szolgáltatás](https://azure.microsoft.com/pricing/details/service-bus/)
Partner kiszolgáló neve | nem | Az elsődleges kiszolgáló nem érhető el, ha egy partner kiszolgáló alternatív vagy biztonsági kiszolgálójával adhat meg.
Táblák | nem | A lista az adatbázistáblák az összekötő frissíthető. Írja be például a *OrdersTable* vagy *EmployeeTable*. Ha nincs táblák kerülnek, minden olyan tábla is használható. Ez az összekötő használata egy művelet érvényes táblák és/vagy a tárolt eljárás szükséges.
Tárolt eljárás | nem | Adjon meg egy meglévő tárolt eljárás, amely az összekötő hívható. Írja be például a *sp_IsEmployeeEligible* vagy *sp_CalculateOrderDiscount*. Ez az összekötő használata egy művelet érvényes táblák és/vagy a tárolt eljárás szükséges.
Rendelkezésre álló lekérdezés | Az eseményindító támogatás | Határozza meg, hogy semmilyen adatot érhető el az SQL Server-adatbázis tábla lekérdezési SQL-utasítást. Ez a kell rendelkezésre álló adatok sorok számát számértéket ad vissza. Példa: COUNT(*) table_name jelölje ki.
A szavazás lekérdezés | Az eseményindító támogatás | A lekérdezik az SQL Server-adatbázis tábla SQL-utasítást. Tetszőleges számú egymástól pontosvesszővel elválasztva SQL-utasítások adhat meg. Ez az utasítás végrehajtása tranzakción keresztül, és csak lekötött, ha az adatok biztonságos tárolása a logika alkalmazásban. Példa: Válasszon *table_name; Törölje a table_name. <br/>**Note**<br/>Meg kell adnia a szavazás utasítás végtelen ciklust elkerülő törlésével, áthelyezése vagy annak érdekében, hogy újra kérdezte nincs-e le ugyanazokat az adatokat a kijelölt adatok frissítését.

5. Amikor elkészült, a Package Settings keresse a következőhöz hasonló:  
![][1]  

6. Jelölje ki a **létrehozása**. 


## <a name="use-the-connector-as-a-trigger"></a>Az összekötő használni a kiváltó ok mező
Nézzük meg egy egyszerű logikai alkalmazás, amely lekérdezi egy SQL-táblából származó adatokat, az adatokat felveszi a másik táblában, frissíti az adatokat.

Az SQL-összekötő használja az eseményindító, adja meg a **Rendelkezésre álló lekérdezés** és a **Szavazás lekérdezés** értékeket. **Rendelkezésre álló lekérdezés** végrehajtása az ütemtervet, adja meg, és azt határozza meg, ha semmilyen adatot érhető el. A lekérdezés csak egy skaláris számot ad eredményül, mivel beállítva, és gyakori végrehajtás optimalizálva.

**Szavazás lekérdezés** csak végre, ha az elérhető lekérdezés azt jelzi, hogy adatokat érhető el. Ez az utasítás végrehajtja a tranzakción belül, és csak lekötött, amikor a kibontott adatok tartósan tárolja az munkafolyamatokban. Fontos elkerülése érdekében a végtelen kiolvasó újra ugyanazokat az adatokat. A végrehajtás tranzakció alapú jellegét törlése és frissítheti az adatokat, annak érdekében, hogy gyűjtött adatok lekérik a későbbiekben nem használható.

> [AZURE.NOTE] Ez az utasítás által visszaadott séma azonosítja a rendelkezésre álló tulajdonságok az összekötő. Az összes oszlop neve kell legyen.

#### <a name="data-available-query-example"></a>Adatok elérhető lekérdezési példája

    SELECT COUNT(*) FROM [Order] WHERE OrderStatus = 'ProcessedForCollection'

#### <a name="poll-data-query-example"></a>A szavazás adatok lekérdezési példája

    SELECT *, GetData() as 'PollTime' FROM [Order]
        WHERE OrderStatus = 'ProcessedForCollection'
        ORDER BY Id DESC;
    UPDATE [Order] SET OrderStatus = 'ProcessedForFrontDesk'
        WHERE Id =
        (SELECT Id FROM [Order] WHERE OrderStatus = 'ProcessedForCollection' ORDER BY Id DESC)

### <a name="add-the-trigger"></a>Az eseményindító hozzáadása
1. Létrehozásakor, vagy egy összefüggés-alkalmazás szerkesztése, jelölje be az SQL-összekötő létrehozott az eseményindító. Ez megjeleníti a rendelkezésre álló eseményindítók: **Szavazás adatok (JSON)** és a **Szavazás adatok (XML)**:  
![][5]

2. Jelölje ki a **Szavazás adatok (JSON)** eseményindító, írja be a gyakoriságot, és kattintson a ✓:  
![][6]

3. Az eseményindító megjelenik a logika alkalmazásban konfigurált. A output(s) az eseményindító jelennek meg, és az esetleges további műveleteket bemenetben is használható:  
![][7]

## <a name="use-the-connector-as-an-action"></a>Az összekötő használata egy művelet
Az egyszerű logikai alkalmazás lekérdezi egy SQL-táblázat adatainak példa összeadja az adatokat a másik táblában, és frissíti az adatokat.

Az SQL-összekötő használandó műveletet, írja be a nevet, a táblázatok, illetve a tárolt eljárásokat az SQL-összekötő létrehozásakor a beírt:

1. Az eseményindító után (vagy válassza a "kézi futtatása a logika"), adja hozzá az SQL-összekötő létrehozta a gyűjteményből. A Beszúrás műveletek, például *Beszúrása be TempEmployeeDetails (JSON)*közül választhat:  
![][8]

2. Írja be a bemeneti értékek az szúrhatók be, és kattintson a ✓ a rekordot:  
![][9]

3. A gyűjteményből válassza ki az azonos SQL-összekötő hozott létre. Egy műveletet jelölje ki a frissítés műveletet ugyanabban a táblában, például a *Frissítés EmployeeDetails*:  
![][11]

4. Írja be a bemeneti értékek a frissítés művelet, és kattintson a ✓:  
![][12]

A logikai alkalmazás tesztelheti a tábla, lekérdezés alatt álló az új rekord hozzáadásával.

## <a name="what-you-can-and-cannot-do"></a>Végezhető, illetve nem végezhető műveletek

SQL-lekérdezés | Támogatott | Nem támogatott
--- | --- | ---
Ha záradék | <ul><li>Operátorok: És, vagy =, <>, <, <, >, > = és hasonló</li><li>Több sub feltételt kombinálásának "(" és")"</li><li>Karakterlánc-literálok, a Datetime (szögletes aposztrófok), számokat (csak tartalmazzon számokból)</li><li>Feltétlenül kell bináris kifejezés formátumban, például ((operand operator operand) és/vagy (operandus operátor az operandus)) *</li></ul> | <ul><li>Operátorok: Között, a</li><li>Az összes beépített függvények, például ADD(), MAX() NOW(), POWER() és így tovább</li><li>Matematikai operátorok, például *, -, +, és így tovább</li><li>Használata karakterláncok összefűzése +.</li><li>Az összes illesztések</li><li>ÜRES és nem Null</li><li>Olyan szám nem numerikus karakteres hexadecimális számot például</li></ul>
Mezők (a választó lekérdezés) | <ul><li>Vesszővel elválasztott oszlopnevek érvényes. Nincs táblázat neve prefixumokban (az összekötő működik a egyszerre több tábla) engedélyezett.</li><li>Nevek is escape, a "[" és "]"</li></ul> | <ul><li>Kulcsszavak, például a felső, DISTINCT, és így tovább</li><li>Aliasokat, például az utca + várost, Zip-AS cím</li><li>Az összes beépített függvények, például a ADD(), MAX() NOW(), POWER() és így tovább</li><li>Matematikai operátorok, például *, -, +, és így tovább</li><li>Használata karakterláncok összefűzése +</li></ul>

#### <a name="tips"></a>Tippek

- Speciális lekérdezések esetén ajánlott a tárolt eljárás és a végrehajtás tárolt eljárás API végrehajtás létrehozásához.
- Belső lekérdezések használata esetén a segítségükkel tárolt eljárások belül.
- Csatlakozás több feltétel, használhatja a "és" és "Vagy" operátorokat.

## <a name="hybrid-configuration-optional"></a>Hibrid konfiguráció (nem kötelező)

> [AZURE.NOTE] Ez a lépés nem kötelező, csak akkor, ha a helyszíni SQL Server használja a tűzfal mögött.

Alkalmazás szolgáltatás a rendszerhez való csatlakozáshoz biztonságosan a helyszíni hibrid Configuration Manager használja. Ha egy helyszíni SQL Server összekötő használ, akkor a hibrid kapcsolatkezelő szükség.

> [AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók Ismerkedés az Azure összefüggés-alkalmazásokhoz, nyissa meg [Logika alkalmazás próbálja meg](https://tryappservice.azure.com/?appservice=logic), hol azonnal létrehozhat egy rövid életű starter logika alkalmazást az App szolgáltatás. Nem kötelező, hitelkártyák Nincs nyilatkozatát.

Lásd: [a hibrid Kapcsolatkezelő használata](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy logikai alkalmazással üzleti munkafolyamat. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

[Összekötők és az API-alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766)megtekintése a Swagger REST API hivatkozást.

Tekintse át a teljesítmény statisztika és a vezérlő biztonsági az összekötő is. Lásd: [kezelése, és figyelemmel követheti a beépített API alkalmazásait és összekötők](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-sql/Create.png
[5]: ./media/app-service-logic-connector-sql/LogicApp1.png
[6]: ./media/app-service-logic-connector-sql/LogicApp2.png
[7]: ./media/app-service-logic-connector-sql/LogicApp3.png
[8]: ./media/app-service-logic-connector-sql/LogicApp4.png
[9]: ./media/app-service-logic-connector-sql/LogicApp5.png
[10]: ./media/app-service-logic-connector-sql/LogicApp6.png
[11]: ./media/app-service-logic-connector-sql/LogicApp7.png
[12]: ./media/app-service-logic-connector-sql/LogicApp8.png
