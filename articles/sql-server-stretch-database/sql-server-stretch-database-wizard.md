<properties
    pageTitle="Első lépések a engedélyezése adatbázis Nyújtás varázsló futtatásával |} Microsoft Azure"
    description="Megtudhatja, hogy miként Nyújtás adatbázis adatbázis konfigurálása varázsló Nyújtás a engedélyezése adatbázis futtatásával."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Első lépések a engedélyezése adatbázis futtatásával Nyújtás varázsló

Adatbázis konfigurálása Nyújtás adatbázis, futtassa a engedélyezése adatbázis Nyújtás varázsló.  Ez a témakör leírja, hogy meg kell adnia az adatok és a választási lehetőségek, hogy módosítania kell a varázslóban.

Kitöltés adatbázissal kapcsolatos további tudnivalókért lásd: a [Nyújtás adatbázis](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Később Ha letiltja Nyújtás adatbázis, ne feledje, hogy a kitöltés adatbázis letiltása, az adatbázis és tábla nem törli a távoli objektum. Ha törölni szeretné a távoli tábla vagy a távoli adatbázis, akkor engedje el az Azure adatkezelési portál használatával. A távoli objektumok továbbra is Azure költségeket merülnek fel, amíg nem törli őket kézzel. 

## <a name="launch-the-wizard"></a>A varázsló elindítása

1.  Az SQL Server Management Studio eszközben, az objektum Explorerben jelölje ki az adatbázist, amelyen a Nyújtás engedélyezni szeretné.

2.  Jobb\-gombra, és jelölje ki a **tevékenységeket**, és válassza a **Nyújtás**, és válassza a **engedélyezése** a varázsló elindításához.

## <a name="Intro"></a>– Bevezetés
Tekintse át a varázsló, és a vonatkozó követelmények célját.

A fontos Előfeltételek közé tartoznak az alábbiak:

-   Akkor rendszergazdának kell lennie az adatbázis-beállítások módosítása.
-   Ha a Microsoft Azure-előfizetéssel kell rendelkeznie.
-   Az SQL Server engedélyezni szeretné a kommunikációt a távoli Azure-kiszolgálóval rendelkezik.

![A kitöltés adatbázis varázsló bevezető lapja][StretchWizardImage1]

## <a name="Tables"></a>Jelölje ki a táblázatokban
Válassza a Nyújtás engedélyezése kívánt táblákat.

Sok sort tartalmazó táblázatok a rendezett lista tetején jelenik meg. A varázsló megjeleníti a táblalistából, mielőtt megvizsgálja őket, amely jelenleg nem támogatott Nyújtás adatbázis adattípusokhoz.

![Válassza a Nyújtás adatbázis varázsló táblák lapján][StretchWizardImage2]

|Oszlop|Leírás|
|----------|---------------|
|(nem cím)|Ebben az oszlopban a kijelölt tábla Nyújtás engedélyezése jelölőnégyzet.|
|**név**|Adja meg annak az oszlopnak a nevére a táblázatban.|
|(nem cím)|Az oszlopban szereplő szimbólumok jelenthetnek figyelmeztetés számolótábláknak\'t megakadályozása a a kijelölt tábla engedélyezése Nyújtás. Azt is jelenthetnek egy megoldhatja a problémát, hogy megakadályozza, hogy a kijelölt tábla engedélyezése Nyújtás \- például mert a táblázat egy nem támogatott adattípust használja. A szimbólum elemleírás további információk megjelenítéséhez mutasson. További információt a olvassa el a [Nyújtás adatbázis vonatkozó korlátozások](sql-server-stretch-database-limitations.md)című témakört.|
|**Kiterjesztett**|Azt jelzi, hogy a táblázat már engedélyezett-e Nyújtás.|
|**Áttelepítése**|Áttelepítheti a teljes táblázat (a**Teljes táblázatot**), vagy egy olyan létező oszlopra a táblázat a adhatja meg a szűrő. Ha azt szeretné, jelölje be a sorok áttelepítendő különböző szűrő függvény használata, futtassa a ALTER TABLE utasítás adja meg a szűrés után a kilépéshez. További információ a filter függvény című témakörben [Jelölje be a sorok szűrése függvény segítségével az áttelepítendő](sql-server-stretch-database-predicate-function.md). A függvény használatáról további információt a lásd: [Nyújtás adatbázis engedélyezése a táblázat](sql-server-stretch-database-enable-table.md) vagy [ALTER TABLE (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Sorok**|A sorok számát adja meg a táblázatban.|
|**Méret (KB)**|Adja meg a táblázat mérete a kilobájtban.|

## <a name="Filter"></a>Tetszés szerint adja meg a sor szűrő

Ha azt szeretné, jelölje ki az áttelepítendő sorok szűrése függvény megadására, az alábbiakra lapon **Jelölje be a táblákat** .

1.  A listájában **Válassza ki a kívánt egymástól a táblákat** kattintson a **Teljes táblázatot** a táblázat sorában. A **sorok Nyújtás kiválasztása** párbeszédpanel.

    ![A filter függvény meghatározása][StretchWizardImage2a]

2.  A **sorok Nyújtás kiválasztása** párbeszédpanelen válassza a **Sorok kiválasztása**.

3.  A **Név mezőben**adja meg a filter függvény nevét.

4.  A **Where** záradékot válasszon egy olyan oszlopot, a táblázat, válasszon egy operátort, és adjon meg egy értéket.

5. **Jelölje be** a függvény tesztelése gombra. Ha a függvény visszatérési értéke a táblából - Ez azt jelenti, hogy beolvashatók a feltételnek - megfelelő sorok áttelepítése a vizsgálat a **sikeres**jelentést.

    >   [AZURE.NOTE] A mezőben lévő értéket, amely megjeleníti a Szűrés lekérdezési csak olvasható. A mezőben lévő értéket a lekérdezés nem szerkeszthetők.

6.  Kattintson a kész, a **választó táblák** lapra való visszatéréshez.

A filter függvény csak akkor, ha a varázsló végzett az SQL Server jön létre. Egészen addig bármikor visszatérhet a módosítása, és nevezze át a filter függvény **Jelölje be a táblák** lapra.

![Jelölje ki a táblák lap a filter függvény definiálása után][StretchWizardImage2b]

Ha szeretne egy másik típusú filter függvény használatával jelölje ki az áttelepítendő sorokat, az alábbi lehetőség közül.  

-   Lépjen ki a varázsló, és futtassa az ALTER TABLE utasítás Nyújtás ahhoz a táblához, és adja meg a szűrő függvényt. További információt a című témakörben talál [Nyújtás adatbázis engedélyezése a tábla](sql-server-stretch-database-enable-table.md).  

-   Futtassa a ALTER TABLE utasítás után a kilépéshez szűrő függvényt adhatja meg. A szükséges lépéseket olvassa el a [adja hozzá a varázsló futtatása után a filter függvény](sql-server-stretch-database-predicate-function.md#addafterwiz)című témakört.

## <a name="Configure"></a>Azure telepítésének konfigurálásához

1.  Jelentkezzen be a Microsoft Azure egy Microsoft-fiókjával.

    ![Jelentkezzen be az Azure - adatbázis Nyújtás varázsló][StretchWizardImage3]

2.  Jelölje ki a meglévő Azure előfizetés Nyújtás adatbázishoz.

3.  Jelöljön ki egy Azure területet.
    -   Ha létrehoz egy új kiszolgálót, a kiszolgáló a régió jön létre.  
    -   A kijelölt tartományban lévő Ha meglévő kiszolgálók, a varázsló őket megjeleníti, ha úgy dönt, hogy a **meglévő kiszolgáló**.

    A késleltetés csökkentése érdekében, válassza az Azure terület, amelyben az SQL Server található. [Azure régiók](https://azure.microsoft.com/regions/)talál további tudnivalókat a régiók.

4.  Adja meg, hogy szeretné-e egy meglévő kiszolgálót használ, vagy hozzon létre egy új Azure kiszolgálót.

    Az Active Directory az SQL Server Azure Active Directory rendelkező identitásszolgáltatóval összevont, ha a távoli Azure kiszolgáló kommunikáció tetszés szerint használhatja az SQL Server szövetséges szolgáltatásfiók. További tudnivalókat a beállítás követelményei [Adatbázis MEGVÁLTOZTATHATJA az beállítások beállítása (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/bb522682.aspx)című cikkben olvashat.

    -   **Hozzon létre új kiszolgálót**

        1.  Hozzon létre egy bejelentkezési és a jelszót a kiszolgáló rendszergazdája.

        2.  Ha szükséges az SQL Server szövetséges szolgáltatásfiók használva kommunikálhat a Azure távoli kiszolgáló.

        ![Hozzon létre új Azure server - adatbázis Nyújtás varázsló][StretchWizardImage4]

    -   **Meglévő kiszolgáló**

        1.  Jelölje ki a meglévő Azure kiszolgálót.

        2.  Válassza ki a hitelesítési módot.

            -   Ha bejelöli az **SQL Server-hitelesítés**, adja meg a rendszergazda bejelentkezési és a jelszavát.

            -   Jelölje ki az **Active Directory beépített hitelesítése** szövetséges szolgáltatásfiók az SQL Server használatával kommunikál a távoli Azure kiszolgáló. A kijelölt server Azure Active Directory nem integrálva van, ha ez a beállítás nem látható.

        ![Jelölje ki a meglévő Azure server - adatbázis Nyújtás varázsló][StretchWizardImage5]

## <a name="Credentials"></a>Hitelesítő adatok biztonságos
Egy adatbázis fő kulcsot a Nyújtás adatbázis használ a távoli adatbázishoz való csatlakozáshoz használt hitelesítő adatok biztonságos kell rendelkeznie.  

Ha már létezik egy adatbázis fő billentyűt, írja be jelszavát azt.  

![A kitöltés adatbázis varázsló biztonságos hitelesítő adatok lapján][StretchWizardImage6b]

Ha az adatbázis nincs diaminta meglévő kulcs, hozzon létre egy adatbázis fő kulcsot erős jelszó megadása  

![A kitöltés adatbázis varázsló biztonságos hitelesítő adatok lapján][StretchWizardImage6]

További tudnivalókat az adatbázis fő billentyűt lásd: [DIAMINTA kulcs létrehozása (a Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) és a [fő adatbázist kulcs létrehozása](https://msdn.microsoft.com/library/aa337551.aspx). További tudnivalók a hitelesítő adatok, amelyek a varázsló létrehozza című témakörben [Létrehozása adatbázis hatóköre hitelesítő adatok (Transact-SQL nyelvben)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Jelölje ki az IP-cím
Az alhálózathoz IP-címtartományokat (ajánlott) vagy a nyilvános IP-címét az SQL Server használatával tűzfal szabály létrehozása a Azure, amely lehetővé teszi az SQL Server kommunikáció a távoli Azure-kiszolgálóval.

Az IP-cím vagy a címet, amely akkor ezen a lapon adja meg, hogy az Azure kiszolgálót úgy, hogy a bejövő adatokat, a lekérdezések és az SQL Server átadni a Azure tűzfalon keresztül által kezdeményezett adatkezelési műveletek. A varázsló nem módosítható az SQL Server tűzfal beállításainak.

![Jelölje ki az Nyújtás adatbázis varázsló IP-cím lapja][StretchWizardImage7]

## <a name="Summary"></a>Összefoglalás
Tekintse át a megadott értékek és a beállításokat, a varázsló, és a becsült költségeket Azure kiválasztott. Válassza a Nyújtás ahhoz, hogy a **befejezési** .

![A kitöltés adatbázis varázsló összefoglalási lapja][StretchWizardImage8]

## <a name="Results"></a>Eredmény
Tekintse át az eredményeket.

Lásd: a Lync-adatok áttelepítése állapotát, [Monitor és problémáinak megoldása adatok áttelepítése (Nyújtás adatbázis)](sql-server-stretch-database-monitor.md).

![Az adatbázis Nyújtás varázsló eredmények lapja][StretchWizardImage9]

## <a name="KnownIssues"></a>A varázsló hibaelhárítása
**Az adatbázis Nyújtás varázsló sikertelen volt.**
Ha Nyújtás adatbázis még nincs engedélyezve a kiszolgáló szintjén, és futtassa a varázsló nélkül a rendszer rendszergazdai engedélyekkel, amelyek lehetővé teszik, a varázsló hibát jelez. Kérje meg a rendszergazdát, nyújtás adatbázis engedélyezéséhez kattintson a helyi kiszolgálói példány, és futtassa újra a varázslót. További információt a című témakörben talál [kapcsolatban előzetesen szükséges: Nyújtás adatbázist ahhoz, hogy a kiszolgálón jogosultsági](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Következő lépések
További táblák engedélyezése Nyújtás adatbázis. Adatok áttelepítése figyelheti és kezelheti a Nyújtás\-adatbázisok és táblák engedélyezve van.

-   [Tábla engedélyezése Nyújtás adatbázis](sql-server-stretch-database-enable-table.md) ahhoz, hogy további táblákat.

-   [Monitor és problémáinak megoldása adatok áttelepítése](sql-server-stretch-database-monitor.md) adatok áttelepítése állapotának megtekintéséhez.

-   [Mutasson az egérrel, és folytassa a Nyújtás adatbázis](sql-server-stretch-database-pause.md)

-   [Kezelése és nyújtás adatbázis – problémamegoldás](sql-server-stretch-database-manage.md)

-   [Biztonsági másolat Nyújtás engedélyező adatbázisok](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Lásd még:

[Adatbázis Nyújtás adatbázis engedélyezése](sql-server-stretch-database-enable-database.md)

[Táblázat Nyújtás adatbázis engedélyezése](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
