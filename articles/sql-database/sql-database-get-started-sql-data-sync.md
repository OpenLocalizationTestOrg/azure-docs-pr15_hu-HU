<properties
    pageTitle="SQL-adatbázisok adatszinkronizálás – első lépések"
    description="Ebben az oktatóanyagban segít, hogy az első lépések az SQL Azure adatszinkronizálás (előzetes verzió)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Első lépések az SQL Azure-adatok szinkronizálása (előzetes verzió)
Ebből az oktatóanyagból megismerheti a kapcsolatos alapismeretekről: Azure SQL-adatok szinkronizálása az Azure klasszikus portálon.

Ebben az oktatóanyagban azt feltételezi, hogy a minimális előzetes élmény és az SQL Server Azure SQL-adatbázis. Ebben az oktatóanyagban teljesen beállítva, és a beállított ütemezés szinkronizálása hibrid (SQL Server és SQL-adatbázis-példányok) szinkronizálási csoport létrehozását.

> [AZURE.NOTE] A teljes műszaki dokumentáció Azure SQL adatok szinkronizálás során, az MSDN webhelyen található korábban beállított egy PDF formátumban érhető el. Töltse le [az alábbi](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Lépés: 1: Az Azure SQL-adatbázis csatlakoztatása

1. Jelentkezzen be a [Klasszikus portálon](http://manage.windowsazure.com).

2. A bal oldali ablaktáblában kattintson az **SQL-ADATBÁZISAIT** .

3. Kattintson a **szinkronizálás** az oldal alján. Kattintson a Szinkronizálás gombra, ha egy lista látható a is hozzáadhat - tartalomelem **Új szinkronizálás csoportban** , és az **Új szinkronizálási ügynök**jelenik meg.

4. Az új SQL adatok szinkronizálása Agent varázsló indítása, kattintson az **Új szinkronizálási ügynök**.

5. Ha még nem helyezett előtt, **kattintson a letöltés itt**ügynökszoftvert.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Lépés: 2: Ügyfél ügynök hozzáadása
Ez a lépés nem kötelező, csak akkor, ha szeretné, hogy egy helyszíni SQL Server-adatbázisban a szinkronizálás csoportban található fogja. Ugrás lépés: 4, ha a szinkronizálás csak az SQL-adatbázis-példányok tartalmaz.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>2a lépés: a szükséges szoftver telepítése
Győződjön meg arról, hogy a következő telepítve a számítógépen, amelyen telepíti az ügyfél ügynök.

- **.NET-keretrendszer 4.0-s**

 Telepítse a .NET-keretrendszer 4.0 [Itt](http://go.microsoft.com/fwlink/?linkid=205836).

- **A Microsoft SQL Server 2008 R2 SP1 rendszer CLR típusok (x86)**

 Telepítse a Microsoft SQL Server 2008 R2 SP1 rendszer CLR típusok (x86) [Itt](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **A Microsoft SQL Server 2008 R2 SP1 megosztott Management objektumok (x86)**

 Telepítse a Microsoft SQL Server 2008 R2 SP1 megosztott objektumok (x86) [Itt](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Lépés a 2b: új ügyfél ügynök telepítése

Kövesse a [ügyfél ügynököt (SQL-adatok szinkronizálása)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) a agent telepítéséhez.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Lépés: 2c: az új SQL adatok szinkronizálása Agent varázsló befejezése

1.  Az új SQL adatok szinkronizálása Agent varázsló vissza.
2.  Adja meg a agent leíró nevét.
3.  A legördülő listából válassza ki a **régió** (adatközpont) a ügynök tárolni.
4.  A legördülő listából válassza ki az **ELŐFIZETÉS** tárolni a ügynök.
5.  Kattintson a jobbra mutató nyílra.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Lépés 3: SQL Server-adatbázis regisztrálása az ügyfél ügynök

Az ügyfél ügynök telepítése után Regisztráljon minden helyszíni SQL Server-adatbázishoz, amely egy szinkronizálási csoport ügynök szerepeltetni kíván.
Adatbázis regisztrációhoz ügynök kövesse a képernyőn megjelenő utasításokat a [regisztráció egy ügyfél ügynök az SQL Server-adatbázishoz](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Lépés: 4: Szinkronizálás csoport létrehozása


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>4a lépés: az új szinkronizálási csoport varázsló elindítása

1.  Vissza a [Klasszikus portálon](http://manage.windowsazure.com).
2.  Kattintson az **SQL-ADATBÁZISAIT**.
3.  **SZINKRONIZÁLÁSI hozzáadása** a lap alján kattintson, és új szinkronizálás csoportban válassza a fiók.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Lépés: 4: az alapvető beállítások megadása


1.  Írja be egy jellemző nevet a szinkronizálás csoportban.
2.  A legördülő listából válassza ki a **régió** (adatközpont) szeretné üzemeltetni a szinkronizálás csoportban.
3. Kattintson a jobbra mutató nyílra.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Lépés: 4c: a szinkronizálás központi meghatározása

1. A legördülő listából válassza ki az SQL-adatbázis-példányt, a szinkronizálási csoport központi szolgáló.
2. Írja be a hitelesítő adatok SQL-adatbázis példány – **Központi FELHASZNÁLÓNEVÉT** és a **Központi JELSZAVÁT**.
3. Várja meg az SQL-adatok szinkronizálása kattintva erősítse meg a FELHASZNÁLÓNEVÉT és JELSZAVÁT. Ekkor megjelenik a jelszó jobbra jelenik meg, amikor a hitelesítő adatok megerősítést zöld pipa jelzi.
4. A legördülő listából válassza ki az **ÜTKÖZÉSEK feloldása** házirend.

 A **Központi Wins** - bármilyen módosítást, a központi adatbázis írása a hivatkozás adatbázisokhoz írt, ugyanazon felülírása módosítások hivatkozás adatbázis-rekord. Funkcionális Ez azt jelenti, hogy a központi írt első módosítása a többi adatbázisokhoz tagszámítógépekre.


 **Ügyfél Wins** - változások a központi írt írja felül hivatkozás adatbázisokat a módosításokat. Funkcionális Ez azt jelenti, hogy az utolsó módosítás írni a központi egyik tartani, és az egyéb adatbázisok propagálja.

5.  Kattintson a jobbra mutató nyílra.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Lépés: 4d: hivatkozás adatbázis hozzáadása


Ismételje meg ezt a lépést a szinkronizálási csoportba felvenni kívánt minden további adatbázis.

1. A legördülő listából válassza ki az adatbázis hozzáadása.

    A legördülő menü az adatbázisok mindkét ügynök és SQL-adatbázis-példányok regisztrálta SQL Server-adatbázisok tartalmazzák.
2.  Írja be hitelesítő adatait az adatbázis - **FELHASZNÁLÓNEVÉVEL** és **JELSZAVÁVAL**.
3.  A legördülő listából válassza ki a **SZINKRONIZÁLÁSI IRÁNYBA** ebben az adatbázisban.

    **Kétirányú** - változások a hivatkozás adatbázisban a központi adatbázisba kerülnek, és módosításokat a központi adatbázisba kerülnek a hivatkozás-adatbázishoz.

    **Szinkronizálás a központból** – az adatbázis megkapja a frissítéseket a központból. Módosítások nem küldi a hubon keresztül csatlakozott.

    **A központi szinkronizálás** – az adatbázis küld frissítéseket a hubon keresztül csatlakozott. A központi változásai nem kerülnek az adatbázishoz.

4.  A szinkronizálás csoportban szerkesztésének befejezéséhez kattintson a jobb alsó sarkában látható a varázsló. Várja meg a SQL-adatok szinkronizálás kattintva erősítse meg a hitelesítő adatokat. Egy zöld pipa jelzi, hogy a hitelesítő adatok megerősítést.

5.  Kattintson a pipára másodszori. Ez visszatér a **szinkronizálás** lap SQL-adatbázisait. A szinkronizálás csoportban most már szerepel a többi szinkronizálási csoportja vagy ügynökök.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>5 lépés: Az adatok szinkronizálása meghatározása

Azure SQL-adatok szinkronizálása jelölje ki a táblázatokhoz és oszlopokhoz a szinkronizálandó teszi lehetővé. Ha is szűrni kívánt oszlop, amely csak egyedi értékeket tartalmazó sorok (például: életkor > = 65) szinkronizálni, az SQL-adatok szinkronizálása portálon használja az Azure, és jelölje ki a dokumentációt a, a táblázatok, oszlopok és sorok szinkronizálás meghatározása az adatok szinkronizálása.

1.  Vissza a [Klasszikus portálon](http://manage.windowsazure.com).
2.  Kattintson az **SQL-ADATBÁZISAIT**.
3.  Kattintson a **szinkronizálás** fülre.
4.  Kattintson a szinkronizálás csoportban a nevére.
5.  Kattintson a **Szinkronizálás szabályok** fülre.
6.  Jelölje ki az adatbázist szeretne megadni a szinkronizálás csoportban sémában.
7.  Kattintson a jobbra mutató nyílra.
8.  **SÉMA a frissítés**gombra.
9.  Mindegyik az adatbázis táblájához szeretné hozzáadni a szinkronizálás az oszlopok kijelölése
    - Nem támogatott adattípusú oszlopok nem jelölhető ki.
    - Ha nem a táblázatok oszlopait van kijelölve, a táblázat nem szerepel a szinkronizálás csoportban.
    - Kiválasztása/kijelölés megszüntetése a táblákat, kattintson a képernyő alján jelölje ki.
10. Kattintson a **MENTÉS**gombra, és várja meg a szinkronizálás csoportban kiépítési befejezéséhez.
11. Szeretne visszatérni az adatok szinkronizálása céloldal, kattintson a vissza nyílra (fölött a szinkronizálási csoport neve) a képernyő bal felső sarokban.

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Lépés a 6: A szinkronizálás csoport konfigurálása

Mindig szinkronizálhatja szinkronizálási csoport SZINKRONIZÁLÁSA az adatok szinkronizálása céloldal alján gombra kattintva.
Ütemezett szinkronizálás, állítsa be a szinkronizálás csoportban.

1.  Vissza a [Klasszikus portálon](http://manage.windowsazure.com).
2.  Kattintson az **SQL-ADATBÁZISAIT**.
3.  Kattintson a **szinkronizálás** fülre.
4.  Kattintson a szinkronizálás csoportban a nevére.
5.  Kattintson a **beállítás** fülre.
6.  **AZ AUTOMATIKUS SZINKRONIZÁLÁSA**
    - Adja meg a készlet gyakoriság szinkronizálása a szinkronizálás csoportban, kattintson a **Tovább**gombra. Továbbra is szinkronizálhat igény szerint a Szinkronizálás gombra kattintva.
    - Kattintson a **ki** konfigurálása a szinkronizálási csoport csak akkor, amikor kattintson a Szinkronizálás gombra.
7.  **SZINKRONIZÁLÁSI GYAKORISÁGA**
    - AUTOMATIKUS szinkronizálás be van KAPCSOLVA, ha szinkronizálási gyakoriságának megadásához. A gyakoriság 1 hónap és az 5 perc között kell lennie.
8.  Kattintson a **MENTÉS**gombra.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Gratulálok. Hozott létre, amely tartalmaz egy SQL-adatbázis-példányt, mind az SQL Server-adatbázis szinkronizálása csoport.

## <a name="next-steps"></a>Következő lépések
SQL-adatbázis és SQL-adatok szinkronizálása lásd: további információt:

* [A teljes SQL-adatok szinkronizálása műszaki dokumentáció letöltése](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL-adatbázis – áttekintés](sql-database-technical-overview.md)
* [Adatbázis életciklus-kezelése](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
