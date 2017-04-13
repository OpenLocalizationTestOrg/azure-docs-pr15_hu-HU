<properties
   pageTitle="A Microsoft Azure alkalmazás szolgáltatás az DB2 összekötő használatával |} Microsoft Azure"
   description="Az DB2 összekötő használatáról logika alkalmazás eseményindítók és a műveletek"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="gplarsen"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="plarsen"/>

# <a name="db2-connector"></a>DB2-összekötő
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

Microsoft-összekötő DB2-alkalmazások Azure alkalmazás szolgáltatáson keresztül csatlakozhat az IBM DB2-adatbázishoz tárolt erőforrások API alkalmazás. Összekötő tartalmaz egy Microsoft Client TCP/IP hálózati kapcsolaton keresztül csatlakozhat DB2-kiszolgáló távoli számítógépek, beleértve az Azure Service Bus továbbítási segítségével a helyszíni DB2-kiszolgálókat Azure hibrid kapcsolatok összekötő támogatja a következő adatbázis-műveletek:

- Olvassa el a sorok kiválasztása
- A szavazás olvasható sorok használó VÁLASSZA a kijelölés követ
- Egy sor vagy használatával Beszúrás több (tömeges) sor hozzáadása
- Egy sor vagy a frissítés használatával több (tömeges) sor módosításához
- Távolítsa el az egy sorból vagy több (tömeges) a sorok törlése
- Megváltoztathatja a sorokat, jelölje be a frissítés ahol a KURZOR követ EGÉRMUTATÓ használatával olvasása
- Olvasás, frissítés ahol a KURZOR követ, VÁLASSZA a EGÉRMUTATÓ használatával sorok eltávolítása
- Eljárás futtassa a bemeneti és kimeneti paraméterekkel, a visszatérési érték, resultset, hívás használatával
- Egyéni parancsok és a kijelölés, Beszúrás, frissítés, törlés használata összetett műveletek

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
Összekötő támogatja a következő összefüggés alkalmazás eseményindítók és a műveletek:

Eseményindítók | Műveletek
--- | ---
<ul><li>A szavazás adatok</li></ul> | <ul><li>Tömeges beszúrása</li><li>Beszúrás</li><li>Tömeges frissítése</li><li>Frissítés</li><li>Hívás</li><li>Tömeges törlése</li><li>Törlése</li><li>Válassza a</li><li>Feltételes frissítése</li><li>Küldés EntitySet</li><li>Feltételes törlése</li><li>Egy személy kijelölése</li><li>Törlése</li><li>A EntitySet Upsert</li><li>Egyéni parancsok</li><li>Összetett műveletek</li></ul>


## <a name="create-the-db2-connector"></a>Az DB2 összekötő létrehozása
Logika-alkalmazásból vagy a Microsoft Azure piactéren lévő összekötő például adhatja meg az alábbi példában:  

1. Válassza az Azure startboard **piactérről**.
2. A **minden** lap, a **db2** írja be a **Keresés mindenhol** mezőbe, és kattintson az enter billentyűt.
3. A Keresés ablaktábla minden eredmény, és válassza a **DB2 összekötő**.
4. Válassza az összekötő leírás DB2 lap **létrehozása**.
5. Írja be a nevét (például "Db2ConnectorNewOrders"), alkalmazás szolgáltatás tervezése és más tulajdonságait DB2 összekötő csomag lap.
6. Jelölje ki a **Package settings**, és adja meg a következő csomag beállításokat:  

    név | Szükséges |  Leírás
--- | --- | ---
ConnectionString | igen | DB2 ügyfél kapcsolati karakterláncot (például "hálózati cím = kiszolgálónév; Hálózati Port = 50000; Felhasználói azonosító = felhasználónév; Jelszó = jelszó; kezdeti katalógus = minta; A webhelycsoport csomag NWIND; = séma alapértelmezett = NWIND ").
Táblák | igen | Tábla, nézet és a alias neve OData műveletek és példákkal (például "*NEWORDERS*") swagger dokumentáció létrehozásához szükséges vesszővel elválasztott listája.
Eljárások | igen | Vesszővel elválasztott listája eljárás és függvényt (például "SPORDERID").
Helyi | nem | Telepítse a helyszíni Azure Service Bus továbbítási használatával.
ServiceBusConnectionString | nem | Azure Service Bus továbbítási kapcsolati karakterláncot.
PollToCheckData | nem | VÁLASSZA a számlakivonat logika alkalmazás eseményindító használata (például "SELECT száma (\*) a NEWORDERS WHERE SZÁLLÍTÁSIDÁTUM IS NULL").
PollToReadData | nem | SELECT utasítás logika alkalmazás eseményindító használata (például "VÁLASSZA a \* a hol található a SZÁLLÍTÁSIDÁTUM a NULL a frissítés NEWORDERS").
PollToAlterData | nem | A frissítés, vagy a DELETE utasítás logika alkalmazás eseményindító használata (például "frissítés NEWORDERS beállítása SZÁLLÍTÁSIDÁTUM = az aktuális dátum hol aktuális a &lt;KURZOR&gt;").

7. Kattintson **az OK gombra**, és válassza a **Hozzon létre**.
8. Amikor elkészült, a Package Settings keresse a következőhöz hasonló:  
![][1]


## <a name="logic-app-with-db2-connector-action-to-add-data"></a>Logika alkalmazás DB2 összekötő művelettel adatok hozzáadása ##
Egy logikai alkalmazás művelet adatok felvétele egy API beszúrási vagy a bejegyzés segítségével entitás OData művelet DB2 táblázat határozhatja meg. Például beszúrhat egy új vevői rendelés rekord egy BESZÚRÁSA SQL-utasításnak definiált azonosító oszlopot tartalmazó táblázat feldolgozásával visszaadó azonosító értékét, vagy a sorok befolyásolja a logika alkalmazásba (ORDID a végleges TÁBLAVÁLASZTÁS (NWIND történő BESZÚRÁSA. NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) ÉRTÉKEK (?,?,?,?,?,?))).

> [AZURE.TIP] DB2 kapcsolat "*Küldés EntitySet*" azonosító oszlop értékét adja eredményül, és a "*API beszúrási*" adja vissza az érintett sorok

1. Válassza az Azure startboard **+** (pluszjel) **Web + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "NewOrdersDb2"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panelen válassza ki az **Ismétlődés**, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **DB2 összekötő**, bontsa ki a műveletek listából választhatja ki a **NEWORDER beszúrni**.
7. Bontsa ki a paraméterek listában adja meg a következő értékeket:  

    név | Érték
--- | --- 
CUSTID | 10042
SHIPNAME | Lusta K Kountry áruházból 
SHIPADDR | 12 zenekar teraszos
SHIPCITY | Walla Walla 
SHIPREG | POSTAFIÓK
SHIPZIP | 99362 

8. Jelölje ki a **be van jelölve** a művelet beállítások ikonra, majd **Mentse**mentése.
9. A beállítások a következőképpen kell kinéznie:  
![][3]

10. **Műveletek** **fut, az összes** listában jelölje ki a első felsorolt elemet (legutóbbi futtatás). 
11. Jelölje ki a **logika alkalmazás futtatásához** a lap, a **művelet** elem **db2connectorneworders**.
12. A **logikai alkalmazás művelet** lap jelölje ki az **BEMENETBEN HIVATKOZÁSRA**. DB2 összekötő folyamat egy paraméteres utasítást használja a bemeneti adatok alapján.
13. A **logikai alkalmazás művelet** lap jelölje ki a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A bemeneti adatok alapján a következőképpen kell kinéznie:  
![][4]

#### <a name="what-you-need-to-know"></a>Mit kell tudnia

- Összekötő csonkolja DB2 táblázatnevek, amikor alkotó logika alkalmazás művelet nevét. Ha például a művelet **NEWORDERS beillesztése** **NEWORDER szúrhatók**be lesz csonkítva.
- Miután mentette a logika alkalmazás **Eseményindítók és a műveletek**, logika alkalmazás végrehajtja a műveletet. Lehetnek olyan késleltetést számú másodpercig (például a 3-5 másodperc) előtt logika alkalmazás végrehajtja a műveletet. Másik lehetőségként rákattinthat **Futtatása** a művelet feldolgozása.
- DB2 összekötő EntitySet amelynek tulajdonságait, például hogy a tagok a DB2 egy oszlopnak felel meg az alapértelmezett érték vagy tagjai generált (pl. azonosító) oszlop határozza meg. Logika alkalmazás a EntitySet tag azonosító neve jelölésére DB2 oszlopokat, hogy az értékek melletti piros csillag jeleníti meg. A ORDID tag, amely megfelel DB2 azonosító oszlop nem kell megad egy értéket. Az egyéb választható tagok (elemek, ORDDATE, REQDATE, SHIPID, FUVARDÍJ, SHIPCTRY), amelyek megfelelnek az alapértelmezett értékű DB2 oszlopok értékek is megadhat. 
- DB2 összekötő adja vissza logika alkalmazásba a válasz a EntitySet a értékekre kiterjedő identitás oszlopokat, amely a DRDA SQLDARD (SQL-adatok területen válasz adatok) származik előkészített BESZÚRÁSA SQL utasítást a bejegyzés pontra. DB2-kiszolgáló nem a beszúrt értékeket vissza az alapértelmezett értékeket.  


## <a name="logic-app-with-db2-connector-action-to-add-bulk-data"></a>Logika alkalmazás DB2 összekötő művelettel tömeges adatok hozzáadása ##
Egy logikai alkalmazás művelet adatok felvétele segítségével API tömeges beszúrása művelet DB2 táblázat határozhatja meg. Például beszúrhat új vevői rendelés rekordokat, az azonosító oszlop célokkal definiált táblákon sor értékek tömbjét használata SQL BESZÚRÁSA utasítás feldolgozásával Visszatérés a sorok befolyásolja a logika alkalmazásba (ORDID a végleges TÁBLAVÁLASZTÁS (NWIND történő BESZÚRÁSA. NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) ÉRTÉKEK (?,?,?,?,?,?))).

1. Válassza az Azure startboard **+** (pluszjel) **webes + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "NewOrdersBulkDb2"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panelen válassza ki az **Ismétlődés**, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **DB2 összekötő**, bontsa ki a műveletek listából választhatja ki a **Tömeges beillesztése új**.
7. Adja meg a **sorok** érték tömbként. Például másolja és illessze be a következőt:

    ```
    [{"CUSTID":10081,"SHIPNAME":"Trail's Head Gourmet Provisioners","SHIPADDR":"722 DaVinci Blvd.","SHIPCITY":"Kirkland","SHIPREG":"WA","SHIPZIP":"98034"},{"CUSTID":10088,"SHIPNAME":"White Clover Markets","SHIPADDR":"305 14th Ave. S. Suite 3B","SHIPCITY":"Seattle","SHIPREG":"WA","SHIPZIP":"98128","SHIPCTRY":"USA"}]
    ```

8. Jelölje ki a **be van jelölve** a művelet beállítások ikonra, majd **Mentse**mentése. A beállítások a következőképpen kell kinéznie:  
![][6]

9. **Műveletek** **fut, az összes** listájában kattintson az első felsorolt elem (legutóbbi futtatás).
10. Kattintson a **Futtatás logika alkalmazás** lap **a teendő** .
11. A **logikai alkalmazás művelet** lap kattintson a **Ráfordítások HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
[][7]
12. Kattintson a **logikai alkalmazás művelet** lap, a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
![][8]

#### <a name="what-you-need-to-know"></a>Mit kell tudnia

- Összekötő csonkolja DB2 táblázatnevek, amikor alkotó logika alkalmazás művelet nevét. Például a **Tömeges beillesztése NEWORDERS** művelet **tömeges új szúrhatók**be lesz csonkítva.
- Által kimarad azonosító oszlopok (például ORDID), nullable oszlopok (például a SZÁLLÍTÁSIDÁTUM) és az oszlopok alapértelmezett értékű (pl. ORDDATE, REQDATE, SHIPID, FUVARDÍJ, SHIPCTRY), a DB2-adatbázishoz értékek hoz létre.
- "Ma" és a "holnap" megadásával DB2 összekötő hoz létre a "Az aktuális dátum" és "Aktuális dátum + 1 nap" funkciók (például REQDATE). 


## <a name="logic-app-with-db2-connector-trigger-to-read-alter-or-delete-data"></a>Logika alkalmazás a DB2 összekötő eseményindító olvasása, módosítása és adatok törlése ##
Megadhatja, hogy egy logikai alkalmazás eseményindító lekérdezik, és olvassa el az adatok egy API szavazás adatok összetett művelettel DB2 táblából. Például a következőképpen olvasható egy vagy több új ügyfél sorrendben rekordokat, az rekordot ad vissza, a logika alkalmazásba. A DB2 kapcsolat csomag/alkalmazás beállításai a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | <no value specified>


Logika alkalmazás eseményindító lekérdezik, olvassa el és változtatja meg egy API szavazás adatok összetett művelettel DB2 táblázat adatainak is adhat meg. Például a következőképpen olvasható egy vagy több új vevői rendelés rekordok, frissítse a sor értékeivel, a logika alkalmazásba a kijelölt (frissítés) a előtt rekordot ad vissza. A DB2 kapcsolat csomag/alkalmazás beállításai a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | FRISSÍTÉS NEWORDERS beállítása SZÁLLÍTÁSIDÁTUM = az aktuális dátum hol az aktuális &lt;KURZOR&gt;


További lekérdezik, olvassa el és adat törlése egy API szavazás adatok összetett művelettel DB2 táblázat logika alkalmazás eseményindító adhatja meg. Például a következőképpen olvasható egy vagy több új vevői rendelés rekordok, a logika alkalmazásba a kijelölt (előtt törlése) rekordot ad vissza, a sorok törlése. A DB2 kapcsolat csomag/alkalmazás beállításai a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | TÖRLÉS NEWORDERS hol az aktuális &lt;KURZOR&gt;

Ebben a példában logika alkalmazás fog lekérdezik, olvassa el, frissítése és ismételt olvassa el a DB2 táblázatban lévő adatok.

1. Válassza az Azure startboard **+** (pluszjel) **webes + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "ShipOrdersDb2"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panel **DB2 összekötő**jelölje ki, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **DB2 összekötő**, bontsa ki a műveletek listából választhatja ki, **Jelölje be a NEWORDERS**.
7. Jelölje ki a **be van jelölve** a művelet beállítások ikonra, majd **Mentse**mentése. A beállítások a következőképpen kell kinéznie:  
![][10]  
8. A **Eseményindítók és a műveletek** lap bezárásához kattintson a gombra, és kattintson a **Beállítások** lap bezárásához.
9. **Műveletek** **fut, az összes** listájában kattintson az első felsorolt elem (legutóbbi futtatás).
10. Kattintson a **Futtatás logika alkalmazás** lap **a teendő** .
11. Kattintson a **logikai alkalmazás művelet** lap, a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
![][11]


## <a name="logic-app-with-db2-connector-action-to-remove-data"></a>Logika alkalmazás DB2 összekötő művelettel adatok eltávolítása ##
Egy logikai alkalmazás művelet adatok eltávolítása a bejegyzés vagy egy API törlése használatával entitás OData művelet DB2 táblázat határozhatja meg. Például beszúrhat egy új vevői rendelés rekord egy BESZÚRÁSA SQL-utasításnak definiált azonosító oszlopot tartalmazó táblázat feldolgozásával visszaadó azonosító értékét, vagy a sorok befolyásolja a logika alkalmazásba (ORDID a végleges TÁBLAVÁLASZTÁS (NWIND történő BESZÚRÁSA. NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) ÉRTÉKEK (?,?,?,?,?,?))).

## <a name="create-logic-app-using-db2-connector-to-remove-data"></a>Adatok eltávolítása a DB2 összekötő használatával összefüggés-alkalmazás létrehozása ##
Hozzon létre egy új logika alkalmazás belül a Microsoft Azure piactéren, és majd használni a DB2 összekötő művelet eltávolítja a vevők rendeléseit. Például is használhatja a DB2 összekötő feltételes törlési művelet folyamat egy törlése SQL utasítást (törlése a NEWORDERS hol ORDID > = 10000 számot).

1. Kattintson a Azure **Start** fal központi menüben **+** (pluszjel), kattintson a **webhely + Mobile**-e, és kattintson a **logika alkalmazást**. 
2. Írja be a **nevét**, például **RemoveOrdersDb2**az **alkalmazás logika létrehozása** lap.
3. Jelölje be, vagy egyéb beállításainak (például szolgáltatáscsomagja, erőforráscsoport) megadása.
4. A beállítások a következőképpen kell kinéznie. Kattintson a **létrehozása**:  
![][12]  
5. Kattintson a **Beállítások** lap **Eseményindítók és a műveletek**.
6. **Eseményindítók és a műveletek** lap **logika alkalmazás sablonok** listájában kattintson a **Létrehozás létrehozása**gombra.
7. **Eseményindítók és a műveletek** lap az **API-alkalmazások** panelt, erőforrás csoportjában az **Ismétlődés**gombra.
8. A tervezési területen logika alkalmazást kattintson az **Ismétlődés** elemre **gyakoriság** és **intervallum**, például a **nap** és **1**, beállítása, kattintson a **be van jelölve** az ismétlődés elem beállítások mentéséhez.
9. **Eseményindítók és a műveletek** lap az **API-alkalmazások** panelen, az erőforrás csoporton belül kattintson **DB2 összekötő**.
10. A tervezési területen logika alkalmazást kattintson a **DB2 összekötő** teendő kattintson a három pontra (**…**) a műveletek Mappalista kibontásához, kattintson a **feltételes N törlése**.
11. Írja be a DB2 összekötő teendő **ORDID ge 10000** egy adott **kifejezés, amely azonosítja bejegyzéseinek egy részét**.
12. A **pipa** művelet beállításainak mentéséhez kattintson, és kattintson a **Mentés**gombra. A beállítások a következőképpen kell kinéznie:  
![][13]  
13. A **Eseményindítók és a műveletek** lap bezárásához kattintson a gombra, és kattintson a **Beállítások** lap bezárásához.
14. **Műveletek** **fut, az összes** listájában kattintson az első felsorolt elem (legutóbbi futtatás).
15. Kattintson a **Futtatás logika alkalmazás** lap **a teendő** .
16. Kattintson a **logikai alkalmazás művelet** lap, a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
![][14]

**Megjegyzés:** Logika alkalmazás Tervező táblázatnevek csonkolja. Ha például a művelet **feltételes NEWORDERS törlése** **feltételes N**törlése lesz csonkítva.


> [AZURE.TIP] A következő SQL-utasításait használatával hozza létre a mintatáblázat és a tárolt eljárásokat. 

A következő SQL-DDL DB2 kimutatások használatával NEWORDERS mintatáblázat hozhat létre:
 
    CREATE TABLE ORDERS (  
        ORDID INT NOT NULL GENERATED BY DEFAULT AS IDENTITY (START WITH 10000, INCREMENT BY 1) ,  
        CUSTID INT NOT NULL ,  
        EMPID INT NOT NULL DEFAULT 10000 ,  
        ORDDATE DATE NOT NULL DEFAULT CURRENT DATE ,  
        REQDATE DATE DEFAULT CURRENT DATE ,  
        SHIPDATE DATE ,  
        SHIPID INT NOT NULL DEFAULT 10000,  
        FREIGHT DECIMAL (9,2) NOT NULL DEFAULT 0.00 ,  
        SHIPNAME CHAR (40) NOT NULL ,  
        SHIPADDR CHAR (60) NOT NULL ,  
        SHIPCITY CHAR (20) NOT NULL ,  
        SHIPREG CHAR (15) NOT NULL ,  
        SHIPZIP CHAR (10) NOT NULL ,  
        SHIPCTRY CHAR (15) NOT NULL DEFAULT 'USA' ,  
        PRIMARY KEY(ORDID)  
        )  
 
    CREATE UNIQUE INDEX XORDID ON ORDERS (ORDID ASC)  



A minta SPOERID tárolt eljárás, az alábbi DB2 DDL utasítás használatával hozhat létre:
 
    CREATE OR REPLACE PROCEDURE NWIND.SPORDERID (IN ORDERID VARCHAR(128))  
        DYNAMIC RESULT SETS 1  
    P1: BEGIN  
        DECLARE CURSOR1 CURSOR WITH RETURN FOR  
            SELECT * FROM NWIND.NEWORDERS  
                WHERE ORDID = ORDERID;  
        OPEN CURSOR1;  
    END P1  
    ') 


## <a name="hybrid-configuration-optional"></a>Hibrid konfiguráció (nem kötelező)

> [AZURE.NOTE] Ez a lépés nem kötelező, csak akkor, ha a tűzfal mögött DB2 összekötő helyszíni használja.

Alkalmazás szolgáltatás a rendszerhez való csatlakozáshoz biztonságosan a helyszíni hibrid Configuration Manager használja. Ha összekötő használ egy helyszíni IBM DB2 kiszolgáló a Windows, a hibrid kapcsolatkezelő szükség.

Lásd: [a hibrid Kapcsolatkezelő használata](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy logikai alkalmazással üzleti munkafolyamat. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

A REST API-k használata API-alkalmazások készítése. [Összekötők és az API-alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766)témakörben talál.

Tekintse át a teljesítmény statisztika és a vezérlő biztonsági az összekötő is. Lásd: [kezelése, és figyelemmel követheti a beépített API alkalmazásait és összekötők](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-db2/ApiApp_Db2Connector_Create.png
[2]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Create.png
[3]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_TriggersActions.png
[4]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersDb2_Outputs.png
[5]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Create.png
[6]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_TriggersActions.png
[7]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Inputs.png
[8]: ./media/app-service-logic-connector-db2/LogicApp_NewOrdersBulkDb2_Outputs.png
[9]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Create.png
[10]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_TriggersActions.png
[11]: ./media/app-service-logic-connector-db2/LogicApp_UpdateOrdersDb2_Outputs.png
[12]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Create.png
[13]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_TriggersActions.png
[14]: ./media/app-service-logic-connector-db2/LogicApp_RemoveOrdersDb2_Outputs.png

