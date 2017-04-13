<properties 
   pageTitle="Az SQL-U feladatok hibakeresési |} Microsoft Azure" 
   description="Megtudhatja, hogyan szeretné hibakeresése U-SQL nyelvben sikertelen csúcsra Visual Studio segítségével. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>C# kód U – SQL-adatok tó Analytics feladatokhoz hibakeresése 

Megtudhatja, hogy miként szeretné hibakeresése felhasználói kód belül hibák miatt sikertelen az SQL-U feladatok Azure adatok tó Visual Studio eszközeivel. 

A Visual Studio eszközben töltse le a lefordított kód és a szükséges csúcsra adatok nyomon követése és sikertelen feladatok hibakeresési fürt teszi lehetővé.

Nagy adatok rendszerek általában nyújtanak bővítési modell keresztül nyelvek például Java, C#, Python stb. Számos rendszer hibakeresési információk, azt az alkalmazást az egyéni kódon hibakeresési nehezen korlátozott futtatókörnyezet szükséges. A legújabb Visual Studio eszközök egy szolgáltatást, úgynevezett "Csúcsra hibakeresési nem sikerült" megtalálható. Ezzel a szolgáltatással letöltheti a futási idejű adatok az Azure helyi munkaállomás, hogy nem sikerült egyéni C# kód használata az azonos futtatókörnyezet és a pontos bemeneti adatok a felhőből hibakeresése is.  A problémák javítása után újra futtathatja a módosított kód Azure-ban a csoportban.

Ez a funkció videó bemutatása [az egyéni kódjának Azure adatok tó Analytics hibakeresési](https://mix.office.com/watch/1bt17ibztohcb)című témakör tartalmaz.

>[AZURE.NOTE] Visual Studio lefagy vagy összeomlik, ha nem rendelkezik az alábbi két windows-frissítések: [a Microsoft Visual C++ 2015 terjeszthető változatát frissítés 2](https://www.microsoft.com/download/details.aspx?id=51682), [Egyetemes C futtatókörnyezet Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Előfeltételek
-   Telt el az [első lépések](data-lake-analytics-data-lake-tools-get-started.md) cikket.

## <a name="create-and-configure-debug-projects"></a>Hozzon létre, és állítsa be a hibakeresési projektek

Sikertelen feladat adatok tó Visual Studio eszközben megnyitásakor értesítés fog kapni. A hiba részletes információ a hiba fülre, és az ablak tetején lévő sárga értesítési sáv fog megjelenni. 

![Azure U-SQL nyelvben adatok tó Analytics hibakeresési visual studio letöltési csúcspont](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Töltse le a csomópontra, és hibakeresési megoldás létrehozása**

1.  Nyissa meg a Visual Studio sikertelen az SQL-U feladat.
2.  Kattintson a **Letöltés** töltse le a szükséges erőforrások és a bemeneti adatfolyamok gombra. Kattintson az **ismét** gombra, ha a letöltés sikertelen volt.
3.  Helyi hibakeresési projekt létrehozása a letöltés befejeződése után kattintson a **Megnyitás** gombra. Egy üres projekt **LocalVertexHost** nevű **VertexDebug** nevű új Visual Studio megoldást hoz létre.

(Script.usql.cs) mögött U – SQL-kódot használt a felhasználó által definiált operátorok, ha a felhasználó által definiált operátorok kóddal Osztályjegyzetfüzet tár C# projekt létrehozása, és az a projekt szerepeltetni a VertexDebug megoldás.

Ha DLL-összeállítások van regisztrálva, az adatok tó Analytics-adatbázishoz, hozzá kell adnia a VertexDebug megoldást a szerelvények forráskódot.
 
Ha létrehozott egy külön C# osztálytár a U – SQL-kódot és regisztrált DLL-összeállítások az adatok tó Analytics-adatbázishoz, hogy fel kell vennie a szerelvények C# forrásprojektben a VertexDebug megoldás.

Néhány ritkán használata a felhasználó által definiált operátorok (Script.usql.cs) fájlt a eredeti megoldásban mögött U-SQL nyelvben kódot. Ha használható legyen, kell a forráskódot tartalmazó C# tár létrehozása és módosítása az összeállítás nevét a fürt regisztrált egy. Az összeállítás neve, jelölje be a parancsfájl használ, amelyeken a fürt a fürt regisztrált elérheti. Erre a U-SQL nyelvben feladat megnyitása, és kattintson a feladat panel az "parancsfájl". 

**A megoldás konfigurálása**

1.  A megoldás Intézőből kattintson a jobb gombbal az imént létrehozott C# projekt, és válassza a **Tulajdonságok parancsot**.
2.  A kimenet elérési útja értéke LocalVertexHost project munka elérési útját. LocalVertexHost projekt munkakönyvtár elérési útja LocalVertexHost tulajdonságok keresztül is elérheti.
3.  A C# projekt összeállítása annak érdekében, hogy a LocalVertexHost projektbe munkakönyvtár .pdb fájlmentés, vagy másolhatja a .pdb fájlt kézzel ebbe a mappába.
4.  A **Kivétel beállításai**területen jelölje be a közös nyelvi futtatókörnyezet kivételek:

![Azure U-SQL nyelvben adatok tó Analytics hibakeresési visual studio beállítás](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>A feladat hibakeresése

Miután hibakeresési megoldást hozott létre a kívánt pozícióba a csúcsot letöltésével és van konfigurálva a környezet, indítsa el a U – SQL-kódot hibakeresése során.

1.  Megoldás Intézőből kattintson a jobb gombbal az imént létrehozott **LocalVertexHost** projekt **hibakeresési**mutasson, kattintson a **Start új példányát**. A LocalVertexHost az indítási projekt kell állítani. A következő üzenet figyelmen kívül hagyhatja, először jelenhet meg. A hibakeresési képernyő eléréséhez egy percig is eltarthat.
 
    ![Azure U-SQL nyelvben adatok tó Analytics hibakeresési visual studio figyelmeztetés](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Használja a Visual Studio alapú hibakeresési felület (megtekintés, változók, stb.) a probléma elhárításához. 
5.  Miután azonosította, hogy probléma, javítása a kódot, és ezután újraépítéséhez C# projekt vizsgálja, hogy újra lépést mindaddig, amíg az összes problémák megoldott előtt. Miután a hibakeresési sikeresen befejeződött, a kimeneti ablakban, a következő üzenet megjelenítése 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>A feladat újraküldése

Miután befejezte a U – SQL-kódot hibakeresési, újraküldése a sikertelen a feladatot.

1. Regisztráljon az ADLA adatbázis új dll-összeállítások.

    1.  A kiszolgáló Explorer/felhő Intézőből adatok tó Visual Studio eszközben bontsa ki az **adatbázisok** csomópontot 
    2.  Kattintson a jobb gombbal összeállítások külső.FÜGGV összeállítások. 
    3.  Regisztráljon az új dll-összeállítások a ADLA adatbázist.
 
2.  Vagy a C# kód másolása script.usql.cs – C# kód fájl mögött.
3.  Küldje el újra a feladatot.

##<a name="next-steps"></a>Következő lépések

- [Oktatóprogram: Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md)
- [Oktatóanyag: adatok tó eszközeivel for Visual Studio U – SQL-parancsfájlok kidolgozása](data-lake-analytics-data-lake-tools-get-started.md)
- [A felhasználó által definiált U-SQL nyelvben operátorok Azure adatok tó Analytics feladatokhoz kidolgozása](data-lake-analytics-u-sql-develop-user-defined-operators.md)

