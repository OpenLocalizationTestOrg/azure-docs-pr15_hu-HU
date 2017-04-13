<properties
   pageTitle="Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure klasszikus Portal)"
   description="Ismerkedés az SQL adatbázis dinamikus adatok maszkolás az Azure klasszikus portálon"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>

# <a name="get-started-with-sql-database-dynamic-data-masking-azure-classic-portal"></a>Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure klasszikus Portal)

> [AZURE.SELECTOR]
- [Dinamikus adatok maszkolás - Azure portál](sql-database-dynamic-data-masking-get-started.md)

## <a name="overview"></a>– Áttekintés

SQL adatbázis dinamikus adatok maszkolás korlátozza a bizalmas adatokat kapta maszkolás azt nem jogosultsággal rendelkező felhasználók által. Dinamikus adatok maszkolás az Azure SQL-adatbázis V12 verziója támogatott.

Dinamikus adatok maszkolás megelőzhető, ezzel az illetéktelen hozzáférést bizalmas adatokhoz, mivel az ügyfelek jelölhet ki mennyi jelenítse meg az alkalmazási réteg minimális hatással bizalmas adatokat. Egy csoportházirend-alapú biztonsági szolgáltatás, amely a bizalmas adatokat egy lekérdezés eredményhalmazában elrejti a kijelölt adatbázismezők, fölé közben az adatbázis adatai nem változik.

Például egy, a telefonos ügyfélszolgálat munkatársával előfordulhat, hogy azonosítsa a hívók úgy, hogy társadalombiztosítási szám vagy a hitelkártya száma több számjegyből, de ezek adatelemeket kell nem teljesen kitenni a munkatársával. Maszkolás szabály határozható meg, hogy minden maszkok, de az utolsó négy számjeggyel, tetszőleges társadalombiztosítási szám vagy a hitelkártya száma az eredményben a lekérdezés állította. Másik példa a megfelelő adatokat maszk definiálható személyes azonosításra (adat) adatok védelme érdekében, hogy a fejlesztők gyártási környezetekben hibaelhárítási célból megfelelőségi rendelkezések megsértése nélkül is lekérdezhetők.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL adatbázis dinamikus adatok maszkolás alapjai

Dinamikus adatok a naplózás- és biztonsági lapján, az adatbázis az Azure klasszikus portálon házirend maszkolás beállítása.


> [AZURE.NOTE] Dinamikus adatok az Azure-portálon maszkolás beállítani, olvassa el a [– első lépések SQL adatbázis dinamikus adatok maszkolás (Azure Portal)](sql-database-dynamic-data-masking-get-started.md)című témakört.


### <a name="dynamic-data-masking-permissions"></a>Dinamikus adatok maszkolás engedélyek

Dinamikus adatok maszkolás az Azure-adatbázis rendszergazdája, a kiszolgáló felügyeleti vagy a biztonsági őr szerepkörök beállíthatók.

### <a name="dynamic-data-masking-policy"></a>Dinamikus adatok maszkolás házirend

* **SQL-felhasználók ne módosíthatna maszkolás** - felhasználók SQL-vagy AAD azonosítási adatok maszkolás kap a SQL-lekérdezés eredményei között. Figyelje meg, hogy rendszergazdai jogosultságokkal rendelkező felhasználók mindig kimarad az maszkolás gombra, és az eredeti adatok bármely maszk nélkül.

* **Szabályok maszkolás** – a kijelölt mezők kell elrejtése és a használandó maszkolás függvény meghatározó szabályhalmaz. A kijelölt mezők az adatbázis sémája nevét, a táblázat neve és az oszlopnév definiálható.

* **Függvények maszkolás** - egy sor olyan módszereket, amelyek az adatok másik felhasználási területei információk megjelenítése.

| Maszkolás függvény | Logika maszkolás |
|----------|---------------|
| **Alapértelmezett**  |**A kijelölt mezők adattípusainak megfelelően teljes maszkolás**<br/><br/>• Használata XXXX vagy kevesebb Xs, ha a mező a mérete kisebb, mint a 4 karakter karakterlánc adattípusokhoz (nchar, ntext, nvarchar).<br/>• Numerikus típusú vonatkozó (bigint, bit, tizedes, int, pénz, numerikus, smallint, megjelenítésekor a rendszer, tinyint, lebegtetés, valós) használata a nulla értéket.<br/>• 1900-01-01-as használja a dátum/idő adattípusok (dátum, datetime2, datetime, datetimeoffset, smalldatetime, idő).<br/>• SQL-változat, az aktuális típus az alapértelmezett értéket használja.<br/>• XML a dokumentum <masked/> használják.<br/>• Üres értéket használja a speciális adattípusokhoz (időbélyeg tábla, Hierarchiaazonosító, globálisan egyedi azonosítója, bináris, a kép, a varbinary térbeli típusok).
| **Hitelkártya** |**Módszer, amelyeket elérhetővé teszi a a kijelölt mezők utolsó négyjegyű maszkolás** , és hozzáadja a állandó karakterlánc előtaggal hitelkártyát formájában.<br/><br/>XXXX-XXXX-XXXX-1234|
| **Társadalombiztosítási számot.** |**Módszer, amelyeket elérhetővé teszi a a kijelölt mezők utolsó négyjegyű maszkolás** , és hozzáadja a állandó karakterlánc előtaggal-amerikai társadalombiztosítási szám formájában.<br/><br/>XXX-XX-1234 |
| **E-mailben** | **Az első betűje közzététele és felülírja a tartományt XXX.com módszer maszkolás** egy e-mail cím formájában állandó karakterlánc előtag használata.<br/><br/>aXX@XXXX.com |
| **Véletlen szám** | **Maszkolás módszer véletlen számot állít elő, amely** a kijelölt határai és a tényleges adattípusok megfelelően. Ha a kijelölt határai egyenlő, a maszkolás függvény állandó szám lesz.<br/><br/>![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **Egyéni szöveg** | **Az első és utolsó karaktere teszi lehetővé, amely módszer maszkolás** , és hozzáadja a saját, belső margóinak karakterlánc közepén. Ha az eredeti karakterlánc rövidebb, mint az elérhető előtag és utótag, csak a kitöltés karakterlánc lesz.<br/>előtag [tartalomelrendezésre] utótag<br/><br/>![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-classic-portal"></a>Dinamikus adatok maszkolás az adatbázis az Azure klasszikus portálon beállítása

1. Indítsa el az Azure klasszikus portált a [https://manage.windowsazure.com](https://manage.windowsazure.com).

2. Kattintson a maszk kívánt adatbázist, és kattintson a **NAPLÓZÁS és a biztonság** fülre.

3. **Dinamikus adatok maszkolás**, csoportban kattintson a **engedélyezve** ahhoz, hogy a szolgáltatás maszkolás dinamikus adatok.  

4. Írja be az SQL-felhasználókat vagy AAD identitások maszkolás kell zárni, így a maszkolás bizalmas adatokhoz való hozzáférése van. Meg kell felhasználók pontosvesszővel elválasztott listája. Ne feledje, hogy rendszergazdai jogosultságokkal rendelkező felhasználók mindig a maszk eredeti adatokhoz való hozzáférés.

    >[AZURE.TIP] Szeretné tenni, hogy az alkalmazási réteg megjeleníthető a kiemelt jogosultságokkal rendelkezik alkalmazás felhasználóinak bizalmas adatokat, az SQL-felhasználó hozzáadása vagy AAD identitás az alkalmazás segítségével az adatbázist a lekérdezés. Ajánlott, hogy a listát a bizalmas adatokat információk megjelenítése céljából jogosultsággal rendelkező felhasználók minimális számú tartalmaz.

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. A menüsávon a lap alján kattintson a **MASZK hozzáadása** kattintva nyissa meg a maszkolás szabály beállítások ablakban.

6. Jelölje ki a **séma**, a **táblázat** és az **oszlop** fog kell elrejtése a kijelölt mezők meghatározása a legördülő listákból.

7. A bizalmas adatokat maszkolás kategóriák listájából válassza ki a **MASZKOLÁS FÜGGVÉNY** .

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png)

8. Kattintson az **OK gombra** az adatok maszkolás frissítse az maszkolás házirend maszkolás dinamikus adatok a szabályok a szabály megadására szolgáló ablakba.

9. Az új vagy frissített maszkolás házirend mentése a **MENTÉS** gombra.


## <a name="set-up-dynamic-data-masking-for-your-database-using-transact-sql-statements"></a>Az adatbázis használata a Transact-SQL-utasítások maszkolás dinamikus adatok beállítása

[Dinamikus adatok maszkolás](https://msdn.microsoft.com/library/mt130841.aspx)olvashat.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Az adatbázis Powershell-parancsmagok használata maszkolás dinamikus adatok beállítása

Lásd: [az SQL Azure-adatbázis parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dinamikus adatok maszkolás az adatbázis REST API segítségével beállítása

Lásd: a [Műveletek az Azure SQL-adatbázisait](https://msdn.microsoft.com/library/dn505719.aspx).
