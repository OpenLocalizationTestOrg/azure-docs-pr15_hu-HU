<properties
   pageTitle="A Microsoft Azure alkalmazás szolgáltatás az Informix összekötő használatával |} Microsoft Azure"
   description="Az Informix összekötő használatáról logika alkalmazás eseményindítók és a műveletek"
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

# <a name="informix-connector"></a>Informix összekötő
>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások 2014-es – 12-01-séma verziót vonatkozik.

A Microsoft összekötő Informix alkalmazások Azure alkalmazás szolgáltatáson keresztül csatlakozhat az IBM Informix adatbázisban tárolt erőforrások API alkalmazás. Összekötő tartalmaz egy Microsoft Client számítógépekhez való csatlakozáshoz távoli Informix server Azure hibrid kapcsolat a helyszíni Informix kiszolgálókkal az Azure Service Bus továbbítási segítségével többek között a TCP/IP hálózati kapcsolaton keresztül. Összekötő támogatja a következő adatbázis-műveletek:

- Olvassa el a sorok kiválasztása
- A szavazás olvasható sorok használó VÁLASSZA a kijelölés követ
- Egy sor vagy használatával Beszúrás több (tömeges) sor hozzáadása
- Egy sor vagy a frissítés használatával több (tömeges) sor módosításához
- Távolítsa el az egy sorból vagy több (tömeges) a sorok törlése
- Megváltoztathatja a sorokat, jelölje be a frissítés ahol a KURZOR követ EGÉRMUTATÓ használatával olvasási
- Olvasás, frissítés ahol a KURZOR követ, VÁLASSZA a EGÉRMUTATÓ használatával sorok eltávolítása
- Eljárás futtassa a bemeneti és kimeneti paraméterekkel, a visszatérési érték, resultset, hívás használatával
- Egyéni parancsok és a kijelölés, Beszúrás, frissítés, törlés használata összetett műveletek

## <a name="triggers-and-actions"></a>Eseményindítók és műveletek
Összekötő támogatja a következő összefüggés alkalmazás eseményindítók és a műveletek:

Eseményindítók | Műveletek
--- | ---
<ul><li>A szavazás adatok</li></ul> | <ul><li>Tömeges beszúrása</li><li>Beszúrás</li><li>Tömeges frissítése</li><li>Frissítés</li><li>Hívás</li><li>Tömeges törlése</li><li>Törlése</li><li>Válassza a</li><li>Feltételes frissítése</li><li>Küldés EntitySet</li><li>Feltételes törlése</li><li>Egy személy kijelölése</li><li>Törlése</li><li>A EntitySet Upsert</li><li>Egyéni parancsok</li><li>Összetett műveletek</li></ul>


## <a name="create-the-informix-connector"></a>Az Informix összekötő létrehozása
Meghatározhatja összefüggés-alkalmazásból vagy a Microsoft Azure piactéren lévő összekötő például az alábbi példában:  

1. Válassza az Azure startboard **piactérről**.
2. A **minden** lap, a **informix** írja be a **Keresés mindenhol** mezőbe, és kattintson az enter billentyűt.
3. A Keresés ablaktábla minden eredmény, és válassza a **Informix összekötő**.
4. A leírás lap Informix összekötő válassza a **létrehozása**lehetőséget.
5. Írja be a nevét (például "InformixConnectorNewOrders"), alkalmazás szolgáltatás tervezése és más tulajdonságait Informix összekötő csomag lap.
6. Jelölje ki a **Package settings**, és adja meg a következő csomag beállításokat.

    név | Szükséges |  Leírás
--- | --- | ---
ConnectionString | igen | Informix ügyfél kapcsolati karakterláncot (például "hálózati cím = kiszolgálónév; Hálózati Port = 9089; Felhasználói azonosító = felhasználónév; Jelszó = jelszó; kezdeti katalógus nwind; = alapértelmezett séma = informix ").
Táblák | igen | Tábla, nézet és a alias neve OData műveletek és példákkal (például "NEWORDERS") swagger dokumentáció létrehozásához szükséges vesszővel elválasztott listája.
Eljárások | igen | Vesszővel elválasztott listája eljárás és függvényt (például "SPORDERID").
Helyi | nem | Telepítse a helyszíni Azure Service Bus továbbítási használatával.
ServiceBusConnectionString | nem | Azure Service Bus továbbítási kapcsolati karakterláncot.
PollToCheckData | nem | VÁLASSZA a számlakivonat logika alkalmazás eseményindító használata (például "SELECT száma (\*) a NEWORDERS WHERE SZÁLLÍTÁSIDÁTUM IS NULL").
PollToReadData | nem | SELECT utasítás logika alkalmazás eseményindító használata (például "VÁLASSZA a \* a hol található a SZÁLLÍTÁSIDÁTUM a NULL a frissítés NEWORDERS").
PollToAlterData | nem | A frissítés, vagy a DELETE utasítás logika alkalmazás eseményindító használata (például "frissítés NEWORDERS beállítása SZÁLLÍTÁSIDÁTUM = az aktuális dátum hol aktuális a &lt;KURZOR&gt;").

7. Kattintson **az OK gombra**, és válassza a **Hozzon létre**.
8. Amikor elkészült, a Package Settings keresse a következőhöz hasonló:  
![][1]


## <a name="logic-app-with-informix-connector-action-to-add-data"></a>Logika alkalmazás Informix összekötő művelettel adatok hozzáadása ##
Megadhatja, hogy egy logikai alkalmazás művelet adatok hozzáadása egy API beszúrási vagy a bejegyzés segítségével entitás OData művelet Informix táblához. Például beszúrhat egy új vevői rendelés rekordot feldolgozásával egy BESZÚRÁSA SQL-utasításnak definiált azonosító oszlopot tartalmazó táblázat azonosító értékét, vagy a logika alkalmazásba érintett sorok visszaadó (ORDID a végleges TÁBLAVÁLASZTÁS (BESZÚRÁSA be NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) értékek (?,?,?,?,?,?))).

> [AZURE.TIP] Informix kapcsolat "*Küldés EntitySet*" azonosító oszlop értékét adja eredményül, és a "*API beszúrási*" adja vissza az érintett sorok

1. Válassza az Azure startboard **+** (pluszjel) **webes + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "NewOrdersInformix"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panelen válassza ki az **Ismétlődés**, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **Informix összekötő**, bontsa ki a műveletek listából választhatja ki a **beszúrni NEWORDER**.
7. Bontsa ki a paraméterek listában adja meg a következő értékeket:  

    név | Érték
--- | --- 
CUSTID | 10042
SHIPID | 10000
SHIPNAME | Lusta K Kountry áruházból 
SHIPADDR | 12 zenekar teraszos
SHIPCITY | Walla Walla 
SHIPREG | POSTAFIÓK
SHIPZIP | 99362 

8. Jelölje ki a **be van jelölve** a művelet beállítások ikonra, majd **Mentse**mentése.
9. A beállítások a következőképpen kell kinéznie:  
![][3]  
10. **Műveletek** **fut, az összes** listában jelölje ki az első felsorolt elemet (legutóbbi futtatás). 
11. Jelölje ki a **logika alkalmazás futtatásához** a lap, a **művelet** elem **informixconnectorneworders**.
12. A **logikai alkalmazás művelet** lap jelölje ki a **BEMENETBEN HIVATKOZÁSRA**. Informix összekötő folyamat egy paraméteres utasítást használja a bemeneti adatok alapján.
13. A **logikai alkalmazás művelet** lap jelölje ki a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A bemeneti adatok alapján a következőképpen kell kinéznie:  
![][4]

#### <a name="what-you-need-to-know"></a>Mit kell tudnia

- Összekötő csonkolja Informix táblázatnevek, amikor alkotó logika alkalmazás művelet nevét. Ha például a művelet **NEWORDERS beillesztése** **NEWORDER szúrhatók**be lesz csonkítva.
- Miután mentette a logika alkalmazás **Eseményindítók és a műveletek**, logika alkalmazás végrehajtja a műveletet. Lehetnek olyan késleltetést számú másodpercig (például a 3-5 másodperc) előtt logika alkalmazás végrehajtja a műveletet. Másik lehetőségként rákattinthat **Futtatása** a művelet feldolgozása.
- Informix összekötő EntitySet amelynek tulajdonságait, például hogy a tagok egy Informix egy oszlopnak felel meg az alapértelmezett érték vagy tagjai generált (pl. azonosító) oszlop határozza meg. Logika alkalmazás a EntitySet tag azonosító neve jelölésére Informix oszlopokat, hogy az értékek melletti piros csillag jeleníti meg. A ORDID tag, amely megfelel Informix azonosító oszlop nem kell megad egy értéket. Az egyéb választható tagok (elemek, ORDDATE, REQDATE, SHIPID, FUVARDÍJ, SHIPCTRY), amelyek megfelelnek az alapértelmezett értékű Informix oszlopok értékek is megadhat. 
- Informix összekötő adja vissza logika alkalmazásba a válasz a EntitySet a értékekre kiterjedő identitás oszlopokat, amely a DRDA SQLDARD (SQL-adatok területen válasz adatok) származik előkészített BESZÚRÁSA SQL utasítást a bejegyzés pontra. Informix kiszolgáló adnak a beszúrt értékek, oszlopok esetén az alapértelmezett értékeket.  


## <a name="logic-app-with-informix-connector-action-to-add-bulk-data"></a>Logika alkalmazás Informix összekötő művelettel tömeges adatok hozzáadása ##
Egy logikai alkalmazás művelet adatok felvétele egy Informix táblázat API tömeges beszúrása művelet használatával adhatja meg. Például beszúrhat új vevői rendelés rekordokat, az azonosító oszlop célokkal definiált táblákon sor értékek tömbjét használata SQL BESZÚRÁSA utasítás feldolgozásával Visszatérés a logika alkalmazásba érintett sorok (végleges táblázat az ORDID jelölje be (Beszúrás be NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) értékek (?,?,?,?,?,?))).

1. Válassza az Azure startboard **+** (pluszjel) **webes + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "NewOrdersBulkInformix"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panelen válassza ki az **Ismétlődés**, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **Informix összekötő**, bontsa ki a műveletek listából választhatja ki a **Tömeges beillesztése új**.
7. Adja meg a **sorok** érték tömbként. Például másolja és illessze be a következőt:  

    ```
    [{"custid":10081,"shipid":10000,"shipname":"Trail's Head Gourmet Provisioners","shipaddr":"722 DaVinci Blvd.","shipcity":"Kirkland","shipreg":"WA","shipzip":"98034"},{"custid":10088,"shipid":10000,"shipname":"White Clover Markets","shipaddr":"305 14th Ave. S. Suite 3B","shipcity":"Seattle","shipreg":"WA","shipzip":"98128","shipctry":"USA"}]
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

- Összekötő csonkolja Informix táblázatnevek, amikor alkotó logika alkalmazás művelet nevét. Például a **Tömeges beillesztése NEWORDERS** művelet **tömeges új szúrhatók**be lesz csonkítva.
- Lehet, hogy Informix adatbázis tábla-és nagybetűk megkülönböztetése. A tömeges beszúrása művelet tömb oszlopnevek Előfordulhat például, kisbetű ("custid") helyett nagybetűs ("CUSTID") kell megadni.
- Által kimarad azonosító oszlopok (például ORDID), nullable oszlopok (például a SZÁLLÍTÁSIDÁTUM) és az oszlopok alapértelmezett értékű (pl. ORDDATE, REQDATE, SHIPID, FUVARDÍJ, SHIPCTRY), Informix adatbázis értékek hoz létre.
- "Ma" és a "holnap" megadásával Informix összekötő hoz létre a "Az aktuális dátum" és "Aktuális dátum + 1 nap" funkciók (például REQDATE). 


## <a name="logic-app-with-informix-connector-trigger-to-read-alter-or-delete-data"></a>Informix összekötő eseményindító olvasása, módosítása vagy törlését a logika alkalmazás ##
Egy logikai alkalmazás eseményindító lekérdezik, és olvassa el az adatok egy Informix táblából API szavazás adatok összetett művelet használatával adhatja meg. Például a következőképpen olvasható egy vagy több új ügyfél sorrendben rekordokat, az rekordot ad vissza, a logika alkalmazásba. A Informix kapcsolat csomag/alkalmazás beállításai a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | <no value specified>


Logika alkalmazás eseményindító lekérdezik, olvassa el, és az API szavazás adatok összetett művelet segítségével Informix-táblázatok adatainak módosítása is adhat meg. Például a következőképpen olvasható egy vagy több új vevői rendelés rekordok, frissítse a sor értékeivel, a logika alkalmazásba a kijelölt (frissítés) a előtt rekordot ad vissza. Informix kapcsolatbeállításokat-csomag/alkalmazás a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | FRISSÍTÉS NEWORDERS beállítása SZÁLLÍTÁSIDÁTUM = az aktuális dátum hol az aktuális &lt;KURZOR&gt;


További lekérdezik, olvassa el és adat törlése egy Informix táblázatot összetett API szavazás adatok művelet logika alkalmazás eseményindító adhatja meg. Például a következőképpen olvasható egy vagy több új vevői rendelés rekordok, a logika alkalmazásba a kijelölt (előtt törlése) rekordot ad vissza, a sorok törlése. Informix kapcsolatbeállításokat-csomag/alkalmazás a következőképpen kell kinéznie:

    App beállítása | Érték
--- | --- | ---
PollToCheckData | SELECT száma (\*) a NEWORDERS, ahol a SZÁLLÍTÁSIDÁTUM értéke NULL
PollToReadData | VÁLASSZA a \* a NEWORDERS, ahol SZÁLLÍTÁSIDÁTUM értéke NULL frissítés
PollToAlterData | TÖRLÉS NEWORDERS hol az aktuális &lt;KURZOR&gt;

Ebben a példában logika alkalmazás fog lekérdezik, olvassa el, frissítése és ismételt olvassa el a Informix táblázatban lévő adatok.

1. Válassza az Azure startboard **+** (pluszjel) **webes + Mobile**, majd a **logika alkalmazást**.
2. Írja be a nevét (például "ShipOrdersInformix"), a alkalmazás szolgáltatás tervezése és az egyéb tulajdonságait, és válassza a **Létrehozás**gombra.
3. Az Azure startboard kijelölése **a logika alkalmazás az imént létrehozott, beállítások ikonra**, majd **Eseményindítók és műveletek**
4. Eseményindítók és a Műveletek lap válassza a **nulláról** sablonok összefüggés-alkalmazásból.
5. Az API-alkalmazások panel **Informix összekötő**jelölje ki, a gyakoriság és intervallum, majd a **pipa**beállítása.
6. Az API-alkalmazások panelen válassza ki a **Informix összekötő**, bontsa ki a műveletek listából választhatja ki, **Jelölje be a NEWORDERS**.
7. Jelölje ki a **be van jelölve** a művelet beállítások ikonra, majd **Mentse**mentése. A beállítások a következőképpen kell kinéznie:  
![][10]
8. A **Eseményindítók és a műveletek** lap bezárásához kattintson a gombra, és kattintson a **Beállítások** lap bezárásához.
9. **Műveletek** **fut, az összes** listájában kattintson az első felsorolt elem (legutóbbi futtatás).
10. Kattintson a **Futtatás logika alkalmazás** lap **a teendő** .
11. Kattintson a **logikai alkalmazás művelet** lap, a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
![][11]


## <a name="logic-app-with-informix-connector-action-to-remove-data"></a>Logika alkalmazás Informix összekötő művelettel adatok eltávolítása ##
Egy logikai alkalmazás művelet adatok eltávolítása egy bejegyzés vagy egy API törlése használatával entitás OData művelet Informix táblázat határozhatja meg. Például beszúrhat egy új vevői rendelés rekordot feldolgozásával egy BESZÚRÁSA SQL-utasításnak definiált azonosító oszlopot tartalmazó táblázat azonosító értékét, vagy a logika alkalmazásba érintett sorok visszaadó (ORDID a végleges TÁBLAVÁLASZTÁS (BESZÚRÁSA be NEWORDERS (CUSTID, SHIPNAME, SHIPADDR, SHIPCITY, SHIPREG, SHIPZIP) értékek (?,?,?,?,?,?))).

## <a name="create-logic-app-using-informix-connector-to-remove-data"></a>Adatok eltávolítása a Informix összekötő használatával összefüggés-alkalmazás létrehozása ##
Hozzon létre egy új logika alkalmazás belül a Microsoft Azure piactéren, és majd használni az Informix összekötő művelet eltávolítja a vevők rendeléseit. Például is használhatja a Informix összekötő feltételes törlési művelet folyamat egy törlése SQL utasítást (törlése a NEWORDERS hol ORDID > = 10000 számot).

1. Kattintson a Azure **Start** fal központi menüben **+** (pluszjel), kattintson a **webhely + Mobile**-e, és kattintson a **logika alkalmazást**. 
2. Írja be a **nevét**, például **RemoveOrdersInformix**az **alkalmazás logika létrehozása** lap.
3. Jelölje be, vagy egyéb beállításainak (például szolgáltatáscsomagja, erőforráscsoport) megadása.
4. A beállítások a következőképpen kell kinéznie. Kattintson a **létrehozása**:  
![][12]
5. Kattintson a **Beállítások** lap **Eseményindítók és a műveletek**.
6. **Eseményindítók és a műveletek** lap **logika alkalmazás sablonok** listájában kattintson a **Létrehozás létrehozása**gombra.
7. **Eseményindítók és a műveletek** lap az **API-alkalmazások** panelt, erőforrás csoportjában az **Ismétlődés**gombra.
8. A tervezési területen logika alkalmazást kattintson az **Ismétlődés** elemre **gyakoriság** és **intervallum**, például a **nap** és **1**, beállítása, kattintson a **be van jelölve** az ismétlődés elem beállítások mentéséhez.
9. **Eseményindítók és a műveletek** lap az **API-alkalmazások** panelen, az erőforrás csoporton belül kattintson **Informix összekötő**.
10. A tervezési területen logika alkalmazást az **összekötő Informix** művelet elemre, kattintson a műveletek listában bontsa ki a három pontra (**…**), és kattintson a **feltételes N törlése**.
11. Írja be a Informix összekötő teendő **ordid ge 10000** egy adott **kifejezés, amely azonosítja bejegyzéseinek egy részét**.
12. A **pipa** művelet beállításainak mentéséhez kattintson, és kattintson a **Mentés**gombra. A beállítások a következőképpen kell kinéznie:  
![][13]
13. A **Eseményindítók és a műveletek** lap bezárásához kattintson a gombra, és kattintson a **Beállítások** lap bezárásához.
14. **Műveletek** **fut, az összes** listájában kattintson az első felsorolt elem (legutóbbi futtatás).
15. Kattintson a **Futtatás logika alkalmazás** lap **a teendő** .
16. Kattintson a **logikai alkalmazás művelet** lap, a **Kimeneti ÉRTÉKEKET HIVATKOZÁSRA**. A kimeneti értékeket a következőképpen kell kinéznie:  
![][14]

**Megjegyzés:** Logika alkalmazás Tervező táblázatnevek csonkolja. Ha például a művelet **feltételes NEWORDERS törlése** **feltételes N**törlése lesz csonkítva.


> [AZURE.TIP] A következő SQL-utasításait használatával hozza létre a mintatáblázat és a tárolt eljárásokat. 

A következő SQL-DDL Informix kimutatások használatával NEWORDERS mintatáblázat hozhat létre:
 
    create table neworders (  
        ordid serial(10000) unique ,  
        custid int not null ,  
        empid int not null default 10000 ,  
        orddate date not null default today ,  
        reqdate date default today ,  
        shipdate date ,  
        shipid int not null default 10000 ,  
        freight decimal (9,2) not null default 0.00 ,  
        shipname char (40) not null ,  
        shipaddr char (60) not null ,  
        shipcity char (20) not null ,  
        shipreg char (15) not null ,  
        shipzip char (10) not null ,  
        shipctry char (15) not null default ''USA'' 
        )


A minta SPORDERID tárolt eljárás, az alábbi Informix DDL utasítás használatával hozhat létre:
 
    create procedure sporderid ( ord_id int)  
        returning int, int, int, date, date, date, int, decimal (9,2), char (40), char (60), char (20), char (15), char (10), char (15)  
        define xordid, xcustid, xempid, xshipid int;  
        define xorddate, xreqdate, xshipdate date;  
        define xfreight decimal (9,2);  
        define xshipname char (40);  
        define xshipaddr char (60);  
        define xshipcity char (20);  
        define xshipreg, xshipctry char (15);  
        define xshipzip char (10);  
        select ordid, custid, empid, orddate, reqdate, shipdate, shipid, freight, shipname, shipaddr, shipcity, shipreg, shipzip, shipctry  
            into xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry  
            from neworders where ordid = ord_id;  
        return xordid, xcustid, xempid, xorddate, xreqdate, xshipdate, xshipid, xfreight, xshipname, xshipaddr, xshipcity, xshipreg, xshipzip, xshipctry;  
    end procedure; 


## <a name="hybrid-configuration-optional"></a>Hibrid konfiguráció (nem kötelező)

> [AZURE.NOTE] Ez a lépés nem kötelező, csak akkor, ha a tűzfal mögött DB2 összekötő helyszíni használja.

Alkalmazás szolgáltatás a rendszerhez való csatlakozáshoz biztonságosan a helyszíni hibrid Configuration Manager használja. Ha összekötő használ egy helyszíni IBM DB2 kiszolgáló a Windows, a hibrid kapcsolatkezelő szükség.

Lásd: [a hibrid Kapcsolatkezelő használata](app-service-logic-hybrid-connection-manager.md).


## <a name="do-more-with-your-connector"></a>További műveletek az összekötő
Most, hogy az összekötő hoz létre, felveheti egy logikai alkalmazással üzleti munkafolyamat. Lásd: [logika alkalmazások melyek?](app-service-logic-what-are-logic-apps.md).

A REST API-k használata API-alkalmazások készítése. [Összekötők és az API-alkalmazások hivatkozás](http://go.microsoft.com/fwlink/p/?LinkId=529766)témakörben talál.

Tekintse át a teljesítmény statisztika és a vezérlő biztonsági az összekötő is. Lásd: [kezelése és figyelheti a beépített API-alkalmazások és összekötők](app-service-logic-monitor-your-connectors.md).


<!--Image references-->
[1]: ./media/app-service-logic-connector-informix/ApiApp_InformixConnector_Create.png
[2]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Create.png
[3]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_TriggersActions.png
[4]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersInformix_Outputs.png
[5]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Create.png
[6]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_TriggersActions.png
[7]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Inputs.png
[8]: ./media/app-service-logic-connector-informix/LogicApp_NewOrdersBulkInformix_Outputs.png
[9]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Create.png
[10]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_TriggersActions.png
[11]: ./media/app-service-logic-connector-informix/LogicApp_UpdateOrdersInformix_Outputs.png
[12]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Create.png
[13]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_TriggersActions.png
[14]: ./media/app-service-logic-connector-informix/LogicApp_RemoveOrdersInformix_Outputs.png


