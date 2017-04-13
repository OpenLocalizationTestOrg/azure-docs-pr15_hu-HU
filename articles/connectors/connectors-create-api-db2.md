<properties
    pageTitle="Az DB2 összekötő hozzáadása az összefüggés-alkalmazások |} Microsoft Azure"
    description="DB2 összekötő REST API-paraméterekkel áttekintése"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Első lépések az DB2-összekötő
Microsoft összekötő DB2 logika alkalmazások erőforrások tárolt IBM DB2-adatbázishoz csatlakozik. Ez az összekötő TCP/IP hálózaton keresztül DB2-kiszolgáló távoli számítógépek kommunikáljon a Microsoft ügyfél tartalmazza. Ez felhő adatbázisok, például az IBM Bluemix dashDB vagy a Windows Azure virtualizációs, futó IBM DB2 és tartalmazza a helyszíni az átjáró a helyszíni használatával adatbázisokat. Lásd: a [támogatott lista](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2-platformokon és aktuális verziója (Ez a témakör).

>[AZURE.NOTE] Ez a cikk verziójának logika alkalmazások általános elérhetőség (kiadás) vonatkozik. 

Az DB2 összekötő támogatja a következő adatbázis-műveletek:

- Lista-adatbázistáblák
- Olvassa el a használatával jelölje ki egy sor
- Olvassa el a használatával jelölje ki az összes sor
- Egy sor használatával Beszúrás hozzáadása
- ALTER egy sor UPDATE segítségével
- TÖRLÉS használata egy sor eltávolítása

Ez a témakör bemutatja, hogyan szeretné használni az összekötő egy adatbázis-műveletek folyamat logika alkalmazást.

Logika-alkalmazásokkal kapcsolatos további információért olvassa el a [összefüggés-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md)című témakört.

## <a name="available-actions"></a>Elérhető műveletek
Az DB2 összekötő támogatja a következő összefüggés alkalmazás műveletek:

- GetTables
- GetRow
- GetRows hívás
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>A Táblák listában
Egy logikai alkalmazás bármely művelet létrehozása a Microsoft Azure-portálon keresztül elvégzett több lépésben tevődik.

Az érték alkalmazásból vehet művelet lista egy DB2-adatbázisból származó táblázatot. A művelet arra utasítja az összekötő DB2-séma kimutatás, például: feldolgozása `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét**, például `Db2getTables`, **előfizetés**, **erőforráscsoport**, **helyét**és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** .
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje ki a **gyakoriság** legördülő listában jelölje ki a **nap**, és megadhatja az **intervallum** írja be a **7**.  
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban írja be a `db2` a **További műveletek keresése** mezőbe, és válassza a **DB2 - tábla (előzetes verzió)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  A **DB2 - tábla** konfigurációs ablaktáblában válassza a **keresztül, a helyszíni adatok átjáró**engedélyezése **jelölőnégyzetet** . Figyelje meg, hogy a helyszíni felhőből módosítsa a beállításokat.
    - Írja be az értéket a **kiszolgáló**címét vagy az alias kettőspont portszámot formájában. Írja be például a `ibmserver01:50000`.
    - Írja be az értéket az **adatbázist**. Írja be például a `nwind`.
    - Válassza a **hitelesítést**. Válassza ki például **egyszerű**.
    - Írja be a **felhasználónevét**értéket. Írja be például a `db2admin`.
    - Írja be a **jelszót**értéket. Írja be például a `Password1`.
    - Válassza az **átjárót**. Jelölje be például a **datagateway01**.
7. Válassza a **Create**, és válassza a **Mentés**gombra. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Jelölje ki a **Db2getTables** lap között **összes futtatja** az **Összefoglalás**területen (a legutóbbi futtatás) első felsorolt elemet.
9.  Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_tables**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell a táblák listáját.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>A kapcsolatok létrehozása
Ez az összekötő támogatja a kapcsolatok üzemeltetett adatbázisokat a helyszíni és a felhőben, használja az alábbi kapcsolat tulajdonságai parancsra. 

A tulajdonság | Leírás
--- | ---
kiszolgáló | Szükséges. Egy karakterlánc, amely egy TCP/IP címet vagy egy aliast követett IPv4 vagy a IPv6-formátumban (pontosvesszővel elválasztott) szerint TCP/IP portszámot elfogadja. 
adatbázis | Szükséges. Fogadja el a karakterlánc, amely egy DRDA relációs adatbázis neve (RDBNAM). A z/OS DB2 elfogadja 16 bájtos karakterlánc (adatbázis-IBM DB2 z/OS hely nevezik). DB2-i5/OS fogadja el a 18 bájtos karakterláncra (az IBM DB2-adatbázis terület i relációs adatbázisban). A LUW DB2 fogadja el a 8 bájtos karakterláncra.
hitelesítés | Nem kötelező. Fogadja el a cikk listaérték, Basic vagy a Windows (kerberos). 
felhasználónév | Szükséges. Fogadja el a szöveges értékként. A z/OS DB2 fogadja el a 8 bájtos karakterláncra. DB2 az i fogadja el a 10 bájtos karakterlánc. Linux vagy UNIX DB2-8 bájtos karakterlánc elfogadja. A Windows DB2 fogadja el a 30 bájtos karakterlánc.
jelszó | Szükséges. Fogadja el a szöveges értékként.
átjáró | Szükséges. Egy lista elem értéket, amely az definiált logika alkalmazások tároló csoporton belül helyszíni átjáró elfogadja.  

## <a name="create-the-on-premises-gateway-connection"></a>A helyszíni átjáró kapcsolat létrehozása
Ez az összekötő érheti el egy, a helyszíni átjáró a helyszíni DB2-adatbázishoz. Átjáró témakörökben olvashat. 

1. Az **átjárók** konfigurációs ablaktáblában válassza a **keresztül átjáró**engedélyezése **jelölőnégyzetet** . Figyelje meg, hogy a helyszíni felhőből módosítsa a beállításokat.
2. Írja be az értéket a **kiszolgáló**címét vagy az alias kettőspont portszámot formájában. Írja be például a `ibmserver01:50000`.
3. Írja be az értéket az **adatbázist**. Írja be például a `nwind`.
4. Válassza a **hitelesítést**. Válassza ki például **egyszerű**.
5. Írja be a **felhasználónevét**értéket. Írja be például a `db2admin`.
6. Írja be a **jelszót**értéket. Írja be például a `Password1`.
7. Válassza az **átjárót**. Jelölje be például a **datagateway01**.
8. Válassza a **Create** továbbra is. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Felhőbeli kapcsolat létrehozása
Ez az összekötő felhő DB2-adatbázis érheti el. 

1. Az **átjárók** konfigurációs ablakban a Kilépés a **jelölőnégyzet** (azokét) **keresztül átjáró**letiltja. 
2. Írja be az értéket a **kapcsolat neve**. Írja be például a `hisdemo2`.
3. Írja be az értéket **DB2-kiszolgáló neve**, cím vagy alias kettőspont portszámot formájában. Írja be például a `hisdemo2.cloudapp.net:50000`.
3. Írja be az értéket a **DB2-adatbázis neve**. Írja be például a `nwind`.
4. Írja be a **felhasználónevét**értéket. Írja be például a `db2admin`.
5. Írja be a **jelszót**értéket. Írja be például a `Password1`.
6. Válassza a **Create** továbbra is. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Jelölje ki a minden sorok beolvasása
Megadhatja, hogy egy logikai alkalmazás művelet-ból DB2 táblázat összes sorát. Az összekötő, például: feldolgozása egy DB2 SELECT utasítás arra utasítja, ez `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét**, például `Db2getRows`, **előfizetés**, **erőforráscsoport**, **helyét**és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** .
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje be a **gyakorisági** legördülő listában jelölje ki a **nap**, és válassza az **intervallum** írja be a **7**. 
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban írja be a `db2` a **További műveletek keresése** mezőbe, és válassza a **DB2 - az első sor (előzetes verzió)**.
6. Jelölje ki az **első sor (előzetes verzió)** művelet **kapcsolat módosítása**.
7. A **kapcsolatok** konfigurálása ablakban jelölje ki az **Új létrehozása**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. Az **átjárók** konfigurációs ablakban a Kilépés a **jelölőnégyzet** (azokét) **keresztül átjáró**letiltja.
    - Írja be az értéket a **kapcsolat neve**. Írja be például a `HISDEMO2`.
    - Írja be az értéket **DB2-kiszolgáló neve**, cím vagy alias kettőspont portszámot formájában. Írja be például a `HISDEMO2.cloudapp.net:50000`.
    - Írja be az értéket a **DB2-adatbázis neve**. Írja be például a `NWIND`.
    - Írja be a **felhasználónevét**értéket. Írja be például a `db2admin`.
    - Írja be a **jelszót**értéket. Írja be például a `Password1`.
9. Válassza a **Create** továbbra is.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. A **táblázat neve** listában jelölje ki a **lefelé mutató nyílra**, és válassza a **területre**.
11. Tetszés szerint jelölje be a **Speciális beállítások megjelenítése** lekérdezési beállítások megadásához.
12. Válassza a **Mentés**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Jelölje ki a **Db2getRows** lap között **összes futtatja** az **Összefoglalás**területen (a legutóbbi futtatás) első felsorolt elemet.
14. Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_rows**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell a sorok listáját.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>Egy sor használatával Beszúrás hozzáadása
Megadhatja, hogy egy logikai app művelet hozzáadása egy sor DB2 táblázatban. Ez a művelet arra utasítja az összekötő DB2 Beszúrás utasítás – például feldolgozása `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét**, például `Db2insertRow`, **előfizetés**, **erőforráscsoport**, **helyét**és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** .
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje be a **gyakoriság** legördülő listában jelölje ki a **nap**, és válassza az **intervallum** írja be a **7**. 
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban írja be a **db2** **kereshet a további műveletek** szerkesztése listában, és válassza a **DB2 - sor beszúrása (előzetes verzió)**.
6. Jelölje ki az **első sor (előzetes verzió)** művelet **kapcsolat módosítása**. 
7. A **kapcsolatok** konfigurálása ablakban jelöljön ki egy kapcsolatot. Jelölje be például a **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. A **táblázat neve** listában jelölje ki a **lefelé mutató nyílra**, és válassza a **területre**.
9. Adja meg az összes szükséges oszlopok (lásd: a piros csillag) értékeit. Írja be például a `99999` **AREAID**, írja be a `Area 99999`, és írja be `102` **REGIONID**számára. 
10. Válassza a **Mentés**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Jelölje ki a **Db2insertRow** lap között **összes futtatja** az **Összefoglalás**területen (a legutóbbi futtatás) első felsorolt elemet.
12. Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_rows**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell az új sor.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Egy sor kijelölése használatával beolvasása
Megadhatja, hogy egy logikai alkalmazás művelet-ból egy sor DB2 táblázatban. Ez a művelet arra utasítja az összekötő egy DB2 hol VÁLASSZA a kimutatás, például: feldolgozása `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét** (például "**Db2getRow**"), **előfizetés**, **erőforráscsoport**, **helyét**, és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** . 
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje be a **gyakorisági** legördülő listában jelölje ki a **nap**, és válassza az **intervallum** írja be a **7**. 
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban írja be a **db2** **kereshet a további műveletek** szerkesztése listában, és válassza a **DB2 - az első sor (előzetes verzió)**.
6. Jelölje ki az **első sor (előzetes verzió)** művelet **kapcsolat módosítása**. 
7. A **kapcsolatok** konfigurációk ablakában jelölje ki egy meglévő kapcsolat. Jelölje be például a **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. A **táblázat neve** listában jelölje ki a **lefelé mutató nyílra**, és válassza a **területre**.
9. Adja meg az összes szükséges oszlopok (lásd: a piros csillag) értékeit. Írja be például a `99999` **AREAID**számára. 
10. Tetszés szerint jelölje be a **Speciális beállítások megjelenítése** lekérdezési beállítások megadásához.
11. Válassza a **Mentés**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Jelölje ki a **Db2getRow** lap között **összes futtatja** az **Összefoglalás**területen (a legutóbbi futtatás) első felsorolt elemet.
13. Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_rows**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell a sor.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>UPDATE segítségével egy sor módosítása
Megadhatja, hogy egy logikai alkalmazás művelet egy sor az DB2 táblázat módosítása. Ez a művelet arra utasítja az összekötő egy DB2 UPDATE utasítás, például: feldolgozása `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét**, például `Db2updateRow`, **előfizetés**, **erőforráscsoport**, **helyét**és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** .
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje be a **gyakoriság** legördülő listában jelölje ki a **nap**, és válassza az **intervallum** írja be a **7**. 
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban írja be a **db2** **kereshet a további műveletek** szerkesztése listában, és válassza a **DB2 - frissítés sor (előzetes verzió)**.
6. Jelölje ki az **első sor (előzetes verzió)** művelet **kapcsolat módosítása**. 
7. A **kapcsolatok** konfigurációk ablaktáblában válassza a jelölje ki egy meglévő kapcsolat. Jelölje be például a **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. A **táblázat neve** listában jelölje ki a **lefelé mutató nyílra**, és válassza a **területre**.
9. Adja meg az összes szükséges oszlopok (lásd: a piros csillag) értékeit. Írja be például a `99999` **AREAID**, írja be a `Updated 99999`, és írja be `102` **REGIONID**számára. 
10. Válassza a **Mentés**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Jelölje ki a **Db2updateRow** lap között **összes futtatja** az **Összefoglalás**területen (a legutóbbi futtatás) első felsorolt elemet.
12. Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_rows**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell az új sor.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>TÖRLÉS használata egy sor eltávolítása
Megadhatja, hogy egy logika alkalmazás a DB2 táblázatokból eltávolíthat egy sor műveletet. Ez a művelet arra utasítja az összekötő DB2 törlése utasítás – például feldolgozása `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Logika alkalmazás létrehozása
1.  Válassza az **Azure indítsa el a falat**, **+** (pluszjel) **webes + Mobile**, majd a **Logika alkalmazás**.
2.  Adja meg a **nevét**, például `Db2deleteRow`, **előfizetés**, **erőforráscsoport**, **helyét**és **Alkalmazás szolgáltatás megtervezése**. Jelölje be a **PIN-kód irányítópult**, és válassza a **Create**.

### <a name="add-a-trigger-and-action"></a>Adja hozzá a kiváltó ok mező és a művelet
1.  A **Logika alkalmazások Designer**jelölje ki a **sablonok** listában **Üres LogicApp** . 
2.  **Eseményindítók** , jelölje be **az ismétlődés**. 
3.  Az **Ismétlődés** eseményindító válassza a **Szerkesztés**, jelölje be a **gyakorisági** legördülő listában jelölje ki a **nap**, és válassza az **intervallum** írja be a **7**. 
4.  Jelölje be a **+ új lépést** , és válassza a **Hozzáadás művelet**.
5.  A **Műveletek** csoportban jelölje ki a **db2** szerkesztése **További műveletek keresése** listában, és válassza a **DB2 - (előzetes verzió) sorának törlése**.
6. Jelölje ki az **első sor (előzetes verzió)** művelet **kapcsolat módosítása**. 
7. A **kapcsolatok** konfigurációk ablakában jelölje ki egy meglévő kapcsolat. Jelölje be például a **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. A **táblázat neve** listában jelölje ki a **lefelé mutató nyílra**, és válassza a **területre**.
9. Adja meg az összes szükséges oszlopok (lásd: a piros csillag) értékeit. Írja be például a `99999` **AREAID**számára. 
10. Válassza a **Mentés**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Jelölje ki a **Db2deleteRow** lap **összefoglaló**csoportban az **összes fut** listán belül első felsorolt elemet (legutóbbi futtatás).
12. Jelölje ki a **logika alkalmazás futtatásához** a lap, **Részletek futtatása**. A **művelet** listában jelölje ki a **Get_rows**. Lásd: az érték **állapot**, kell lennie, amely **sikerült**. Jelölje be a **bemeneti adatok alapján hivatkozás** megtekintéséhez a bemeneti adatok alapján. Jelölje ki a **Kimenet hivatkozásra**, és a kimeneti értékeket; megtekintése amely tartalmaznia kell a törölt sort.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Műszaki információk

## <a name="actions"></a>Műveletek
Művelet egy olyan művelet, a munkafolyamat egy logikai alkalmazásban definiált által végzett. Az DB2-adatbázis összekötő tartalmazza a következő műveleteket. 

|Művelet|Leírás|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Egyetlen sor egy DB2 táblából|
|[GetRows hívás](connectors-create-api-db2.md#get-rows)|Sorok egy DB2 táblából|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Új sor beszúrása DB2 táblába|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Sorok törlése a DB2 táblából|
|[GetTables](connectors-create-api-db2.md#get-tables)|Táblázatok átveszi DB2-adatbázishoz|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Frissíti egy meglévő sor DB2 táblázatban|

### <a name="action-details"></a>Művelet részletei

Ebben a részben lásd: az egyes műveletek, például minden olyan kötelező és választható bemeneti tulajdonságainak és bármely megfelelő eredményt ad, az összekötő társított pontos részleteket.

#### <a name="get-row"></a>Első sor 
Egyetlen sor átveszi DB2 táblához.  

| Tulajdonság neve| Megjelenítendő név |Leírás|
| ---|---|---|
|táblázat * | Táblázat neve |DB2 tábla neve|
|azonosító * | Sorazonosító |A sor beolvasásához egyedi azonosító|

Csillag (*) azt jelzi, hogy a tulajdonság szükség.

##### <a name="output-details"></a>Kimeneti részletei
Elem

| Tulajdonság neve | Adattípus |
|---|---|
|ItemInternalId|karakterlánc|


#### <a name="get-rows"></a>Első sor 
Sorok átveszi DB2 táblához.  

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|táblázat *|Táblázat neve|DB2 tábla neve|
|$skip|Kihagyott elemek száma|Kihagyása bejegyzéseinek száma (alapértelmezett = 0)|
|$top|Get maximális száma|Beolvasásához bejegyzések maximális száma (alapértelmezett = 256)|
|$filter|Szűrő lekérdezések|Az ODATA szűrése lekérdezés korlátozása bejegyzéseinek száma|
|$orderby|Rendezési szempont|Az ODATA orderBy lekérdezés bejegyzések sorrendjének megadása|

Csillag (*) azt jelzi, hogy a tulajdonság szükség.

##### <a name="output-details"></a>Kimeneti részletei
ItemsList

| Tulajdonság neve | Adattípus |
|---|---|
|érték|tömb|


#### <a name="insert-row"></a>Sor beszúrása 
Új sor beszúrása DB2 táblába.  

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|táblázat *|Táblázat neve|DB2 tábla neve|
|elem *|Sor|Sor beszúrása a megadott táblázat DB2|

Csillag (*) azt jelzi, hogy a tulajdonság szükség.

##### <a name="output-details"></a>Kimeneti részletei
Elem

| Tulajdonság neve | Adattípus |
|---|---|
|ItemInternalId|karakterlánc|


#### <a name="delete-row"></a>Sor törlése 
Sorok törlése DB2 táblából.  

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|táblázat *|Táblázat neve|DB2 tábla neve|
|azonosító *|Sorazonosító|Egyedi azonosítója, a sor törlése|

Csillag (*) azt jelzi, hogy a tulajdonság szükség.

##### <a name="output-details"></a>Kimeneti részletei
Nincs lehetőség.

#### <a name="get-tables"></a>Tábla 
Táblázatok átveszi DB2-adatbázishoz.  

Nincsenek a hívás paraméterek. 

##### <a name="output-details"></a>Kimeneti részletei 
TablesList

| Tulajdonság neve | Adattípus |
|---|---|
|érték|tömb|

#### <a name="update-row"></a>Sor frissítése 
Frissíti egy meglévő sor DB2 táblázatban.  

|Tulajdonság neve| Megjelenítendő név|Leírás|
| ---|---|---|
|táblázat *|Táblázat neve|DB2 tábla neve|
|azonosító *|Sorazonosító|Egyedi azonosítója, a sor frissítése|
|elem *|Sor|Frissített értékeket tartalmazó sor|

Csillag (*) azt jelzi, hogy a tulajdonság szükség.

##### <a name="output-details"></a>Kimeneti részletei  
Elem

| Tulajdonság neve | Adattípus |
|---|---|
|ItemInternalId|karakterlánc|


### <a name="http-responses"></a>HTTP-válaszok

Ha a hívások átirányítása a különböző műveletek elvégzése adott válaszok jelenhetnek meg. Az alábbi táblázat ismerteti a válaszokat és azok leírását:  

|név|Leírás|
|---|---|
|200|oké|
|202|Elfogadott|
|400|Hibás kérés|
|401|Ezzel az illetéktelen|
|403|Tiltott|
|404|Nem található|
|500|Belső kiszolgálóhiba. Ismeretlen hiba|
|alapértelmezett|A művelet sikertelen volt.|


## <a name="supported-db2-platforms-and-versions"></a>Támogatott platformok DB2 és a verziókkal
Ez az összekötő támogatja a következő IBM DB2-platformon és verzióiban, valamint az IBM DB2 kompatibilis termékek (pl. IBM Bluemix dashDB), amelyek támogatják a normális eloszlású relációs adatbázis architektúra (DRDA) SQL Access Manager (SQLAM) 10 és 11-es verziójú:

- IBM DB2-z/OS 11.1
- IBM DB2-z/OS 10.1
- IBM DB2-i 7.3.
- IBM DB2-i 7.2.
- IBM DB2-i 7.1.
- IBM DB2-LUW 11
- IBM DB2-LUW 10.5


## <a name="next-steps"></a>Következő lépések

[Egy logikai-alkalmazás létrehozása](../app-service-logic/app-service-logic-create-a-logic-app.md). Ismerkedjen meg az [API-khoz listában](apis-list.md)a összefüggés-alkalmazások más elérhető összekötők.

