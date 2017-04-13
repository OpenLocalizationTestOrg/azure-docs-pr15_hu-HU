<properties 
    pageTitle="Azure keresés profilok használatáról a pontozási |} Microsoft Azure |} A felhőben tárolt keresési szolgáltatás" 
    description="Optimalizálhatja a keresési rangsorolást profilok pontozási Azure keresés, a Microsoft Azure szolgáltatott felhő keresési szolgáltatásának keresztül." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Azure keresési pontozási profilok használatáról

Pontozási profilok újdonsága Microsoft Azure keresési, testre a keresési eredmények, számítás befolyásoló elemek rangsorolásának módja a keresési eredmények listájában. Érdemes elképzelnie profilok pontozási a modell relevancia, így azáltal előre meghatározott feltételeknek megfelelő elemek. Tegyük fel, hogy az alkalmazása egy online szállások foglalás nélkül kapnak helyet. Azáltal a `location` mezőben, a keresést, amely tartalmazza, hogy van-e Seattle kifejezés Seattle elemek magasabb értékek eredményez, mint a `location` mezőben. Ne feledje, hogy meg is egynél több pontozási profil vagy a senki értéket, ha az alapértelmezett pontozási elegendő az alkalmazás.

Segít a pontozási profilok kísérletezni, letölthet egy minta alkalmazás pontozási profilok rangsorolt találatok sorrendjének módosítása. A minta alkalmazás, amely egy konzol – esetleg nem nagyon reálisan a való életben alkalmazások fejlesztése – hasznos, de mindazonáltal egy tanulási eszközt. 

A minta alkalmazás bemutatja a kitalált adatokat nevű pontozási viselkedése a `musicstoreindex`. A minta alkalmazás egyszerűség megkönnyíti a pontozási profilok és a lekérdezések, és a program végrehajtása esetén olvassa el a sorrend azonnali hatással.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Előfeltételek

A minta alkalmazás írott C# Visual Studio 2013-ban. Ha még nem rendelkezik a Visual Studio másolatát, próbálja meg az ingyenes [Visual Studio 2013 Express edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) .

Az Azure-előfizetéssel és az oktatóprogram elvégzéséhez Azure keresőszolgáltatás lesz szüksége. [Hozzon létre egy keresési szolgáltatás a portálon](search-create-service-portal.md) talál segítséget a szolgáltatás beállítása.

[AZURE. OLYAN [oktatóprogram elvégzéséhez az Azure-fiók szükséges:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>A minta alkalmazás letöltése

Nyissa meg a [Azure keresési pontozási profilok bemutató](https://azuresearchscoringprofiles.codeplex.com/) funkcióhoz a Codeplex webhelyen minta leírt az oktatóprogram letöltése.

A forráskód lapon kattintson a **letöltése** első zip-fájl a megoldás gombra. 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config szerkesztése

1. Miután kinyerte a fájlokat, nyissa meg azt a megoldást a konfigurációs fájl szerkesztése a Visual Studio.
1. A megoldás Intézőben duplán **app.config**. Ezt a fájlt, adja meg a szolgáltatás végpontjának és egy `api-key` hitelesíti a kérést. Ezeket az értékeket a klasszikus portálról szerezhet be.
1. Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
1. Nyissa meg a szolgáltatás irányítópult Azure keresés.
1. Kattintson a **Tulajdonságok** csempére az szolgáltatás URL-cím másolása
1. Kattintson a **billentyűk** csempére kattintva másolja a vágólapra a `api-key`.

Amikor befejezte az URL-cím hozzáadását és `api-key` app.config, az alkalmazás beállításai így néz ki:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Ismerje meg az alkalmazás

Szinte készen létre, és futtassa az alkalmazást, de mielőtt tesz, nézze meg a JSON-fájlok létrehozása és az index feltöltéséhez használt.

**Schema.JSON** az indexet, beleértve a pontozási profilokat, amelyek kiemelten megjelenítő Ez a bemutató határozza meg. Értesítés a séma definiálja összes a tárgymutató-kereshető mezők, például: például használt mező `margin`, a pontozási profil használható. Profil szintaxis pontozási [hozzáadása egy Azure keresési indexhez pontozási profil](http://msdn.microsoft.com/library/azure/dn798928.aspx)ismertetését.

**Adat1-3.json** az adatokat, 246 album műfajok néhány keresztül biztosít. Tényleges album és az előadó információkat, például kitalált mezők kombinációja adatai `price` és `margin` keresési műveletek mutatja be. Az adatfájlok felel meg az index és a Azure keresési szolgáltatás van feltöltve. Után az adatok feltöltött és indexelt, elküldheti, lekérdezéseket.

**Program.cs** hajtja végre a következő műveleteket:

- Konzol ablak megnyitása.

- Azure keresést, a szolgáltatás URL-cím használatával csatlakozik, és `api-key`.

- Törli a `musicstoreindex` Ha létezik.

- Létrehoz egy új `musicstoreindex` schema.json fájl használatával.

- Az index, az adatfájlok használatával kitölt.

- Lekérdezi az indexet, négy lekérdezésekkel. Figyelje meg, hogy a pontozási profilok lekérdezési paraméterként adható. Az összes a lekérdezések keresése egyazon kifejezést, a legjobb". Az első lekérdezés bemutatja, hogyan alapértelmezett pontozási. A hátralévő három lekérdezések pontozási profil használja.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Össze, és futtassa az alkalmazást

Kizárja a csatlakozási problémák vagy összeállítás hivatkozás problémákat, összeállítása és annak érdekében, hogy a munkát, először ki nincs problémák lépnek fel az alkalmazásnak a futtatására. Meg kell jelennie egy konzol alkalmazás, nyissa meg a háttérben. Minden négy lekérdezés végre sorrendben Szüneteltetés nélkül. A teljes program számos rendszeren végrehajtja a 15 másodperc alatt. Ha a New tartalmaz egy üzenet arról "teljes. Nyomja le az enter továbbra is", a program sikeres befejezését. 

Hasonlítsa össze a program futtatja, is be van jelölve másolás beillesztés, a lekérdezés eredményében a konzolról, és illessze be azokat egy Excel-fájlban. 

Az alábbi ábrán az első három lekérdezések egymás mellé származó találatok jelenjenek meg. A lekérdezések használni egy keresőkifejezést "legjobb" számos fényképalbum címek, amely megjelenik.

   ![][10]

Az első lekérdezés használja az alapértelmezett pontozási. Csak a fényképalbum címek megjelenik a keresőkifejezést, és a más feltétel nem van megadva, mivel a lemez címe "legjobb" rendelkező elemek visszaadott abban a sorrendben, amelyben a keresési szolgáltatás megtalálja őket. 

A második lekérdezés pontozási profil használ, de figyelje meg, hogy a profil volna nincs hatása. Megegyeznek az első lekérdezés eredménye. Ennek oka az, a pontozási profil boosts mező (műfaj), amely nem germane a lekérdezésre. A keresett kifejezést "legjobb" nem létezik a dokumentum bármely "műfaj" mező. A pontozási profilok bejelölésének nincs hatása, amikor eredménye megegyezik az alapértelmezett pontozási.  

A harmadik lekérdezés első igazolása pontozási profil hatással. A keresett kifejezést a továbbra is ajánlott, hogy ugyanazok a album dolgozunk, de, mert a pontozási profil nyújt további feltételeket, amely növekedhet "minősítés" és "utolsó frissített", néhány elem van hajtott magasabb a listában.

A következő ábrán a negyedik és záró lekérdezés, csillapítja "nyereség" alapján. A "nyereség" mező nem kereshető és a keresési eredmények között nem adhatók vissza. A "nyereség" értéket a számolótábla bemutatják a pont, amely nagyobb margókat elemek jelenik meg a Keresés eredménye listában magasabban segíti manuális hozzá lett adva. 

   ![][9]

Most, hogy meg van végzett kísérletezést pontozási profiljaival próbálja használni a különböző szintaxisát, a program módosítása pontozási profilok vagy magasabb szintű adatok. A következő szakaszban található hivatkozásokra további információt.

<a id="next-steps"></a>
## <a name="next-steps"></a>Következő lépések

További tudnivalók a profilok pontozási. További információ [hozzáadása az Azure keresési indexhez pontozási profil](http://msdn.microsoft.com/library/azure/dn798928.aspx) témakörben talál.

További tudnivalók a keresési képletszintaxisát és a lekérdezés paraméterei. Lásd: [Dokumentumok keresése (Azure keresési REST API -val)](http://msdn.microsoft.com/library/azure/dn798927.aspx) további információt.

Lépés a vissza és többet szeretne tudni az index létrehozása kell? [Ebből a videóból](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) alapvető megértéséhez.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 