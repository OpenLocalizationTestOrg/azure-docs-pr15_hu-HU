<properties
   pageTitle="Naplózás az Azure SQL-adatraktár |} Microsoft Azure"
   description="Első lépések az Azure SQL-adatraktár naplózás"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Az SQL Azure-adatraktár naplózás

> [AZURE.SELECTOR]
- [A naplózás](sql-data-warehouse-auditing-overview.md)
- [Veszélyt kimutatására](sql-data-warehouse-security-threat-detection.md)

SQL adatraktár naplózás lehetővé teszi az adatbázis-naplózandó események bejelentkezés az Azure tároló fiókja rekordra. A naplózás segítséget nyújtanak a szabályozási megfelelőségről karbantartása, adatbázis tevékenység megértését és eltérések és üzleti kérdéseket jelezheti, vagy biztonsági megsértése gyanús rendellenességeinek bepillantást. SQL adatraktár Képletvizsgálat is integrálódik a Microsoft Power BI Lehatolás jelentések készítéséhez és elemzéshez.

Ellenőrző eszközök engedélyezése és betartását megfelelőségi szabványokat megkönnyítése, de nem garantálja megfelelőségi. Azure megfelelőségi szabványokat támogató programokban kapcsolatos további tudnivalókért olvassa el a az <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure az Adatvédelmi központ</a>című témakört.

+ [Az adatbázisok naplózás alapjai]
+ [Naplózás az adatbázis beállítása]
+ [Elemzése a naplókat és a jelentések]

##<a id="subheading-1"></a>Azure SQL adatok raktári adatbázis Képletvizsgálat alapjai


Adatraktár SQL-adatbázis naplózás lehetővé teszi:

- **Megőrzése** a kijelölt eseményekről könyvvizsgálati. Megadhatja kategóriába naplózásra adatbázis-műveletek.
- **Jelentés** adatbázis tevékenységet. Előre definiált jelentés és egy irányítópult segítségével gyorsan első lépések a tevékenység és esemény jelentéskészítés.
- Jelentések **adatainak elemzése** . A gyanús eseményeket, szokatlan tevékenység és trendek talál.

A következő eseményre kategóriák naplózásának adhatja meg:

**Egyszerű SQL** és az **SQL paraméteres** , amelynek a összegyűjtött naplók osztályozott  

- **Hozzáférés az adatokhoz**
- **Séma módosítása (DDL)**
- **Adatmódosítás (DML)**
- **Partnerek, szerepkörök és engedélyek (Kapcsolattárat)**
- A **tárolt eljárás**, **Bejelentkezés** , és **tranzakciók kezelése**.

Esemény kategóriákhoz tartozó **sikeres** és **sikertelen** műveletek naplózási külön-külön megtörténik.

Látható tevékenységek és események ellenőrzött kapcsolatban további részleteket lásd: a <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Naplózási formátum naplózása (dokumentum fájl letöltése)</a>.

Azure tároló fiókja naplókat tárolja. A naplózási napló adatmegőrzési időszak határozhatja meg.

Naplózási házirend definiálható adott adatbázis vagy egy alapértelmezett kiszolgáló-házirend. Egy alapértelmezett naplózási kiszolgáló-házirend minden adatbázisban a kiszolgálón, amelyen nem rendelkezik egy adott elsőrendű adatbázis naplózási házirend definiált érvényesek lesznek.

A beállítás előtt felfelé ["régebbi ügyfél"](sql-data-warehouse-auditing-downlevel-clients.md)használatakor naplózási naplózási jelölőnégyzetet.


##<a id="subheading-2"></a>Naplózás az adatbázis beállítása

1. Indítsa el az <a href="https://portal.azure.com" target="_blank">Azure-portálon</a>.

2. Nyissa meg azt a adatraktár SQL-adatbázis konfigurációs lap naplózni kívánt SQL Server /. Kattintson a **Beállítások** gombra, legfelül és majd, a beállítás a lap, és válassza a **naplózás**.

    ![][1]

3. A naplózási konfigurációs lap, a kijelölés megszüntetése a **Naplózási beállításokat öröklik kiszolgáló** jelölőnégyzetet. Ez lehetővé teszi, hogy adja meg a beállításokat az adott adatbázis.

    ![][2]

4. Ezután naplózásának **a** gombra kattintva.

    ![][3]

5. Naplózási a konfigurációs lap válassza **Tároló részletei** a naplózási naplók tároló lap megnyitásához. Jelölje ki a naplók mentési helyének Azure tárterület-fiók és az adatmegőrzési időszak. **Tipp:** Az előre beállított jelentések sablonok ki a legtöbbet a ugyanazt a tárterület-fiókot ellenőrzött adatbázisokra használatával.

    ![][4]

6. Kattintson az **OK** gombra, a tárhely részletek konfiguráció mentéséhez.


7. **ESEMÉNYNAPLÓZÁS szerint**csoportban kattintson az összes eseményének bejelentkezni **sikeres** és **sikertelen** , vagy válassza ki az egyes kategóriák.


8. Naplózás az adatbázis állítja be, ha szüksége lehet változtatja meg a kapcsolati karakterláncot az ügyfél biztosítja az adatok Képletvizsgálat megfelelően rögzíthető. Jelölje be a [Kiszolgáló FQDN módosítása a kapcsolati karakterlánc](sql-data-warehouse-auditing-downlevel-clients.md) témakör régebbi ügyfélkapcsolatok.

9. Kattintson az **OK gombra**.


##<a id="subheading-3">Elemzése a naplókat és a jelentések</a>

Naplókat azt választotta, a telepítés során Azure tároló fiók **SQLDBAuditLogs** előtaggal vannak összesíti a tár táblák gyűjteménye. <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure tároló Explorer</a>például eszköz használata a naplófájlok megtekintése.

Előre beállított irányítópult jelentéssablon segít a napló az adatok gyors elemzése egy <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">Excel-számolótábla letölthető</a> amelyekhez. A sablon használata a naplókat, szüksége van az Excel 2013-at vagy újabb és letöltheti a Power Query <a href="http://www.microsoft.com/download/details.aspx?id=39379">Itt</a>.

A sablon a kitalált mintaadatok rendelkezik, és beállíthatja a Power Query importálhatja a napló közvetlenül az Azure tárterület-fiókját.

Jelentéssablon használata a részletes útmutatás olvassa el a <a href="http://go.microsoft.com/fwlink/?LinkId=506731">Hogyan szeretné (dokumentum letöltése)</a>.

![][5]


##<a id="subheading-4">A gyártási használatát tanácsok</a>
Ebben a szakaszban a leírás a fenti képernyőképek hivatkozik. <a href="https://portal.azure.com" target="_blank">Azure portálon</a> vagy a <a href= "https://manage.windowsazure.com/" target="_bank">Klasszikus Azure klasszikus portálon</a> használható.


##<a id="subheading-5"></a>Tárterület kulcs újbóli létrehozása

A gyártási valószínűleg a tárhely billentyűk rendszeres időközönként frissítse. Amikor frissítése a billentyűk mentse újra a házirendet szeretne. A folyamat az alábbiak szerint:.


1. Lépjen a **Tárterület hívóbetű** *elsődleges* *másodlagos* és **mentése**(fenti szakasz naplózás beállítása) a konfigurációs naplózási lap.
![][4]
2. Nyissa meg a tárterület a konfigurációs lap és a **újragenerálása** az *Elsődleges hívóbetű*.

3. Lépjen vissza a naplózási konfigurációs lap, váltson a **Tárhely hívóbetű** *másodlagos* az *elsődleges* , és nyomja le az **MENTENI**.

4. Térjen vissza a felhasználói felület tárolási és **újragenerálása** a *Másodlagos hívóbetű* (a Felkészülés a következő kulcsok frissítési ciklus.

##<a id="subheading-6"></a>Automatizálási
Vannak olyan több PowerShell-parancsmagok az Azure SQL-adatbázis biztonsági naplózás beállítása is használhatja. A naplózási parancsmagok eléréséhez kell futtatnia PowerShell Azure erőforrás-kezelő módban.

> [AZURE.NOTE] Az [Erőforrás-kezelő Azure](https://msdn.microsoft.com/library/dn654592.aspx) modul jelenleg előzetes verzióban. Esetleg nem nyújt az azonos szolgáltatásairól, az Azure modul.

Erőforrás-kezelő Azure módban van, amikor futtatni `Get-Command *AzureSql*` a rendelkezésre álló parancsmagok listáját.


<!--Anchors-->
[Az adatbázisok naplózás alapjai]: #subheading-1
[Naplózás az adatbázis beállítása]: #subheading-2
[Elemzése a naplókat és a jelentések]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
