<properties
   pageTitle="Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure Portal)"
   description="Ismerkedés az SQL adatbázis dinamikus adatok maszkolás az Azure-portálon"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Első lépések az SQL adatbázis dinamikus adatok maszkolás (Azure Portal)

> [AZURE.SELECTOR]
- [Dinamikus adatok maszkolás - Azure klasszikus portál](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>– Áttekintés

SQL adatbázis dinamikus adatok maszkolás korlátozza a bizalmas adatokat kapta maszkolás azt nem jogosultsággal rendelkező felhasználók által. Dinamikus adatok maszkolás az Azure SQL-adatbázis V12 verziója támogatott.

Dinamikus adatok maszkolás megelőzhető, ezzel az illetéktelen hozzáférést bizalmas adatokhoz, mivel az ügyfelek jelölhet ki mennyi jelenítse meg az alkalmazási réteg minimális hatással bizalmas adatokat. Egy csoportházirend-alapú biztonsági szolgáltatás, amely a bizalmas adatokat egy lekérdezés eredményhalmazában elrejti a kijelölt adatbázismezők, fölé közben az adatbázis adatai nem változik.

Például egy, a telefonos ügyfélszolgálat munkatársával előfordulhat, hogy azonosítsa a hívók úgy, hogy társadalombiztosítási szám vagy a hitelkártya száma több számjegyből, de ezek adatelemeket kell nem teljesen kitenni a munkatársával. Maszkolás szabály határozható meg, hogy minden maszkok, de az utolsó négy számjeggyel, tetszőleges társadalombiztosítási szám vagy a hitelkártya száma az eredményben a lekérdezés állította. Másik példa a megfelelő adatokat maszk definiálható személyes azonosításra (adat) adatok védelme érdekében, hogy a fejlesztők gyártási környezetekben hibaelhárítási célból megfelelőségi rendelkezések megsértése nélkül is lekérdezhetők.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL adatbázis dinamikus adatok maszkolás alapjai

Dinamikus adatok az Azure-portálon házirend maszkolás az SQL-adatbázis konfigurációs lap és a beállítások lap a dinamikus adatok maszkolás művelet kiválasztásával beállítása.


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
| **Véletlen szám** | **Maszkolás módszer véletlen számot állít elő, amely** a kijelölt határai és a tényleges adattípusok megfelelően. Ha a kijelölt határai egyenlő, a maszkolás függvény állandó szám lesz.<br/><br/>![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Egyéni szöveg** | **Az első és utolsó karaktere teszi lehetővé, amely módszer maszkolás** , és hozzáadja a saját, belső margóinak karakterlánc közepén. Ha az eredeti karakterlánc rövidebb, mint az elérhető előtag és utótag, csak a kitöltés karakterlánc lesz. <br/>előtag [tartalomelrendezésre] utótag<br/><br/>![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>A maszk ajánlott mezők

A DDM javaslatok motor mezőként esetleg bizalmas, amely lehet a jó szolgálatot tesznek az maszkolás megjelöli a bizonyos mezőket az adatbázisból. A portál dinamikus adatok maszkolás fel az ajánlott oszlopokat az adatbázis fogja látni. Az összes kell tennie, kattintson a **Maszk hozzáadása** egy vagy több oszlop, majd **Mentse** ahhoz, hogy ezeket a mezőket maszkot alkalmazni.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dinamikus adatok maszkolás az adatbázis az Azure-portálon beállítása

1. Indítsa el az Azure-portált a [https://portal.azure.com](https://portal.azure.com).

2. Nyissa meg a beállítások lap a maszk bizalmas adatokat tartalmazó adatbázist.

3. Kattintson a **Dinamikus adatok maszkolás** csempére, amely elindítja az **Adatok dinamikus maszkolás** konfigurációs lap.

    * Azt is megteheti görgessen le a **Műveletek** csoportban, és kattintson az **Adatok dinamikus maszkolás**.

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. A **Dinamikus adatok maszkolás** konfigurációs lap, amely a javaslatok motor menügombon van maszkolás adatbázis oszloppal jelenhet meg. Annak érdekében, hogy elfogadja a javaslatok, egyszerűen kattintson a **Maszk hozzáadása** egy vagy több oszlop és a maszk hoz létre az alapértelmezett típus oszlop alapján. Módosíthatja a maszkolás függvény a maszkolás szabály kattintva, majd a Szerkesztés a maszkolás mező formázása az alapértelmezettől eltérő lehetőség. Ne felejtse el menteni szeretné a beállításait a **Mentés** gombra.

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Maszkot szeretne adni egy bármely oszlop az adatbázisban, az **Adatok dinamikus maszkolás** konfigurációs a lap tetején kattintson a **Maszk hozzáadása** a konfigurációs **Maszkolás szabály hozzáadása** lap megnyitása

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Jelölje ki a **séma**, **tábla** - és **oszlop** meghatározása a kijelölt mezőt, amely fog kell elrejtése.

7. Válassza ki a **Mező formázása maszkolás** kategóriák maszkolás bizalmas adatokat a listáról.

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Az adatok, frissítse az maszkolás házirend maszkolás dinamikus adatok a szabályok a szabály lap maszkolás kattintson a **Mentés** gombra.

9. Írja be az SQL-felhasználókat vagy AAD identitások maszkolás kell zárni, így a maszkolás bizalmas adatokhoz való hozzáférése van. Meg kell felhasználók pontosvesszővel elválasztott listája. Ne feledje, hogy rendszergazdai jogosultságokkal rendelkező felhasználók mindig a maszk eredeti adatokhoz való hozzáférés.

    ![Navigációs ablak](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Szeretné tenni, hogy az alkalmazási réteg megjeleníthető a kiemelt jogosultságokkal rendelkezik alkalmazás felhasználóinak bizalmas adatokat, az SQL-felhasználó hozzáadása vagy AAD identitás az alkalmazás segítségével az adatbázist a lekérdezés. Ajánlott, hogy a listát a bizalmas adatokat információk megjelenítése céljából jogosultsággal rendelkező felhasználók minimális számú tartalmaz.

10. Az adatok maszkolás konfigurációs lap az új vagy frissített maszkolás házirend mentéséhez kattintson a **Mentés** gombra.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Az adatbázis Powershell-parancsmagok használata maszkolás dinamikus adatok beállítása

Lásd: [az SQL Azure-adatbázis parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dinamikus adatok maszkolás az adatbázis REST API segítségével beállítása

Lásd: a [Műveletek az Azure SQL-adatbázisait](https://msdn.microsoft.com/library/dn505719.aspx).
