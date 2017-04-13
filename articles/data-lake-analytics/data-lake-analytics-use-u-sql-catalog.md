<properties
   pageTitle="Azure adatok tó Analytics U-SQL nyelvben katalógus bevezetésére |} Azure"
   description="Azure adatok tó Analytics U-SQL nyelvben katalógus bevezetése"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Az SQL-U katalógus használata

A U-SQL nyelvben katalógus adatok és a kód felépítését, így U-SQL nyelvben parancsfájlok megosztható szolgál. A katalógus lehetővé teszi, hogy az adatokkal az Azure adatok tó lehetséges legnagyobb teljesítményét.

Minden Azure adatok tó Analytics-fiók van társítva pontosan egy U-SQL nyelvben katalógus. A U-SQL nyelvben katalógus nem törölhetők. U-SQL nyelvben katalógusok jelenleg nem osztható meg tó adattár fiókok között.

Minden egyes U-SQL nyelvben katalógus **fő**nevű adatbázis tartalmazza. A fő adatbázist nem lehet törölni.  Minden egyes U-SQL nyelvben katalógus további további adatbázisok is tartalmazhat.

U – SQL-adatbázis tartalmazza:

- Összeállítások – megosztása .NET kód U – SQL-parancsfájlok között.
- Táblázat értékek függvények – megosztása U – SQL-parancsfájlok között U – SQL-kódot.
- Táblázatok – az adatok megosztása U – SQL-parancsfájlok között.
- A sémák - megosztása táblázat sémák U – SQL-parancsfájlok között.

## <a name="manage-catalogs"></a>Katalógusok kezelése
Minden Azure adatok tó Analytics-fiókhoz társított alapértelmezett Azure tó adattár fiókkal rendelkezik. Ehhez a fiókhoz tó adattár tó adattár alapértelmezett fiókként nevezik. Az SQL-U katalógus az alapértelmezett adattár tó fiók található/Új_katalógus mappájában vannak tárolva. Nem törli a található/Új_katalógus mappában lévő fájlok.

### <a name="use-azure-portal"></a>Azure portál használata

Lásd: az [adatok tó Analytics kezelése portál használatával](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog)


### <a name="use-data-lake-tools-for-visual-studio"></a>Visual Studio tó Adateszközök használata.

Adatok tó Tools for Visual Studio segítségével a katalógus kezelése.  A eszközökkel kapcsolatos további tudnivalókért olvassa el a [Használatával adatok tó Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)című témakört.

**A katalógus kezelése**

1. Nyissa meg a Visual Studio, és csatlakozzon az azure. A megjelenő utasításokat olvassa el a [Csatlakozás az Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Nyissa meg a **Kiszolgáló Explorer** : nyomja le a **CTRL + ALT + S billentyűkombinációt**.
2. A **Kiszolgáló Intéző**bontsa ki az **Azure**, bontsa ki az **Adatok tó Analytics**, bontsa ki az adatok tó Analytics-fiókját, bontsa ki az **adatbázisok**és majd bontsa ki a **fő**.



    - Új adatbázis hozzáadásához kattintson a jobb gombbal az **adatbázist**, és kattintson az **Adatbázis létrehozása**.
    - Egy új összeállítás hozzáadásához kattintson a jobb gombbal a **szerelvények**, és kattintson az **Összeállítás regisztrálni**.
    - Új séma hozzáadása, kattintson a jobb gombbal **a sémák**, és válassza a "létrehozása séma **.
    - Egy új táblázatot szeretne hozzáadni, kattintson a jobb gombbal a **táblázatok**, és válassza a "" létrehozása táblában **.
    - Hozzáadásáról egy új táblázatot értékelni függvény című témakörben olvashat [kidolgozása U-SQL nyelvben felhasználó által definiált operátorok az adatok tó Analytics feladatokat](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Tallózással keresse meg a Visual Studio U – SQL-katalógusok](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Lásd még:

- Első lépések
    - [Első lépések az adatok tó Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md)
    - [Első lépések az adatok tó Analytics Azure PowerShell használatával](data-lake-analytics-get-started-powershell.md)
    - [Első lépések az adatok tó Analytics Azure .NET SDK használatával](data-lake-analytics-get-started-net-sdk.md)
    - [Az SQL-U parancsfájlok tó Adateszközök for Visual Studio kidolgozása](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md)

- Az SQL-U és fejlesztés
    - [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md)
    - [Azure adatok tó Analytics feladatok U-SQL nyelvben ablak függvények használata](data-lake-analytics-use-window-functions.md)
    - [A felhasználó által definiált operátorok U-SQL nyelvben adatok tó Analytics feladatokhoz kidolgozása](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Kezelése
    - [Azure adatok tó Analytics Azure portálon kezelése](data-lake-analytics-manage-use-portal.md)
    - [Azure adatok tó Analytics Azure PowerShell használatával kezelése](data-lake-analytics-manage-use-powershell.md)
    - [Figyelésére és Azure portálon Azure adatok tó Analytics feladatok hibaelhárítása](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Végpont oktatóanyag
    - [Azure adatok tó Analytics interaktív oktatóanyagok használata](data-lake-analytics-use-interactive-tutorials.md)
    - [Azure adatok tó Analytics használatával webhely naplók elemzése](data-lake-analytics-analyze-weblogs.md)
