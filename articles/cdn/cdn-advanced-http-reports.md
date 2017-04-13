<properties
    pageTitle="HTTP-jelentések speciális Azure CDN |} Microsoft Azure"
    description="Speciális HTTP-jelentések a Microsoft Azure CDN. Ezeket a jelentéseket CDN tevékenység részletes tájékoztatást nyújt."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>A Microsoft Azure CDN speciális HTTP-jelentések

## <a name="overview"></a>– Áttekintés

A dokumentum bemutatja a speciális HTTP jelentése a Microsoft Azure CDN. Ezeket a jelentéseket CDN tevékenység részletes tájékoztatást nyújt.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Speciális HTTP-jelentések elérése

1. Az a CDN a profil lap a **kezelés** gombra.

    ![CDN-profil lap kezelése gomb](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    Ekkor megnyílik a CDN kezelőportálja segítségével.

2. Az **elemzés** lapon mutasson, majd mutasson a **HTTP-jelentések speciális** előugró.  Kattintson a **nagy Platform HTTP**.

    ![CDN - jelentések Speciális menü kezelőportálja](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Jelentés beállítások is megjelennek.

## <a name="geography-reports-map-based"></a>Földrajzi hely jelentések (Térképalapú)

Nincsenek öt jelentés, amely jelzi azon részeire lépkedhet, amelyből a tartalom kért térkép előnyeit. Ezeket a jelentéseket nemzetközi térkép, Egyesült Államok térkép, Kanada térkép, Európa térkép és Ázsia Csendes-óceáni térkép.

Minden térképalapú jelentés az orderby_expression földrajzi személyek (tehát országok államok és tartományok), amely az adott régióban származik találatok százalékos megfelelően. Ezenkívül segít a helyek, amelyből a tartalom kért ábrázolása térképen kapja. Fontos tudni az egyes régiókra aszerint, hogy az adott régióban tapasztalt igény szerinti mennyiségét színkódolást. Bárka árnyékolt terület alsó igény szerint a tartalom, adja meg, miközben sötétebb régiók jelezheti magasabb szintű igény szerint a tartalom számára.

Részletes forgalmat és sávszélesség az egyes régiókra vonatkozó információt közvetlenül a térkép alá. Így megtekintheti a találatok, a találatok százaléka, az adatok mennyiségét száma átvitt (gigabájt), és az egyes régiókra vonatkozó átvitt adatok százalékát. Az egyes ezeket a mértékek megtekintése a leírását. Végül (tehát ország, megye vagy tartomány) terület mutat, amikor nevét és a találatok, hogy a tartományban lévő e jelenik meg, az elemleírás.

Egy rövid leírást az egyes: térképalapú földrajzi jelentés alatt megadva.

Jelentés neve | Leírás
------------|------------
Nemzetközi térkép | Ez a jelentés megtekintése a világszerte igény szerint a CDN-tartalom számára teszi lehetővé. Az egyes országok színkódos jelzi, hogy az adott régióban származik találatok százalékos nemzetközi térképen.
Amerikai Egyesült Államok térkép | Ez a jelentés megtekintése a igény szerint a CDN-tartalom számára az Amerikai Egyesült Államokban teszi lehetővé. Egyes állapotok jelölése más színnel, ez jelzi, hogy az adott régióban származik találatok százalékos térképen.
Kanada térkép | Ez a jelentés megtekintése a CDN-tartalom számára az igény szerinti Kanadában teszi lehetővé. Minden tartomány jelölése más színnel, ez jelzi, hogy az adott régióban származik találatok százalékos térképen.
Európa térkép | Ez a jelentés megtekintése a CDN-tartalom számára az igény szerinti Európában teszi lehetővé. Az egyes országok jelölése más színnel, ez jelzi, hogy az adott régióban származik találatok százalékos térképen.
Ázsia Csendes-óceáni térkép | Ez a jelentés megtekintése a CDN-tartalom számára az igény szerinti Ázsiában teszi lehetővé. Az egyes országok jelölése más színnel, ez jelzi, hogy az adott régióban származik találatok százalékos térképen.

## <a name="geography-reports-bar-charts"></a>Földrajzi hely jelentések (sávdiagramok)

Két további jelentés áll, amely szerint a Földrajz statisztikai adatokat felső városokat és a felső országok. Ezeket a jelentéseket rangsorolása városok és országok, a kurzor aszerint, hogy az adott régióban származik találatok száma. Esetén az ilyen típusú jelentés létrehozása, sávdiagram megjelöli a felső 10 városok vagy országok által kért tartalom egy bizonyos platformot fölé. A sávdiagram lehetővé teszi, hogy gyorsan felmérheti, hogy a tartalom iránti kérelmek a legmagasabb szám generálásához régiók.

A diagram (y) bal oldalán lévő azt jelzi, hogy az adott régióban történt, hány találatok. Közvetlenül a diagram (az x tengely) alatt találja címke az, hogy a felső 10 régióban.

### <a name="using-the-bar-charts"></a>A sávdiagramok használatával

* Egy sáv mutat, ha a neve és a teljes oldalszám, hogy a tartományban lévő-e a találatok jelennek meg, az elemleírás.
* A felső városok jelentés az elemleírásban azonosítja a nevét, állam/tartomány és ország rövidítést város.
* Ha nem kell meghatározni a város vagy egy kérelmet származási régió (tehát megye), majd azt jelzi, hogy azok ismeretlen. Ha az ország ismeretlen, majd a két kérdőjel (azaz????), jelennek meg.
* Egy jelentésben mértékek "Európa" vagy "Ázsiai és csendes-óceáni régió." Ezeket az elemeket a rendszer nem szeretett volna adja meg azokat a régiókat minden IP-címének statisztikai adatokat. Ehelyett csak vonatkoznak összehívások helyezkednek el Europe vagy az ázsiai és csendes-óceáni helyett egy adott város vagy ország IP-címtartományokat származik.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs találat, találatok százaléka, adatok mennyiségét száma átvitt (gigabájt), és a felső 250 régió átvitt adatok százalékos kifejezésre keresve. Az egyes ezeket a mértékek megtekintése a leírását.

Egy rövid leírást a két alábbi jelentéstípusok megadva.

Jelentés neve | Leírás
------------|------------
Felső várost | Ez a jelentés régió rangsorolja városok szerint elindító találatok száma.
Felső országok | Ez a jelentés régió rangsorolja országokból származó találatok száma szerint.

## <a name="daily-summary"></a>Napi összegzése

A napi összefoglaló jelentés megtekintése a találatok és száma naponta egy adott platformon átvitt adatok teszi lehetővé. Az információk gyors discern CDN tevékenység mintázatok használható. Például ez a jelentés segíthetnek észleli melyik napon tapasztalt nagyobb vagy kisebb, mint a várható forgalmat.

Esetén az ilyen típusú jelentés létrehozása, sávdiagram nyújt a platform-specifikus igény szerint a tapasztaltabb mennyiségét vizuális feltüntetése naponta hatálya alá a jelentés időszakban. Ezt fogja ehhez a jelentés minden nap megjelenítése a sáv. Például az időtartamot kijelölése neve "Múlt héten" hét sávokkal sávdiagram hoz létre. Mindegyik sáv jelzi az adott napon a tapasztaltabb találatok száma.

A diagram (y) bal oldalán lévő azt jelzi, hogy hány találatok történt a megadott dátumot. Közvetlenül a diagram (az x tengely) alatt megtalálja azt a dátumot jelzi, hogy címkét (formátum: YYYY-MM-DD) minden nap, a jelentésben szereplő.

> [AZURE.TIP] Egy sáv mutat, ha, hogy mikor történt dátum találatok száma megegyezik egy elemleírás jelenik meg.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs találatok száma és az átvitt (gigabájt) adatok mennyiségét kifejezésre keresve minden nap, a jelentés tárgyát.

## <a name="by-hour"></a>Óra

A által óra jelentés lehetővé teszi a találatok és egy adott platformon átvitt óránkénti kombinálásával teljes számának megtekintése. Az információk gyors discern CDN tevékenység mintázatok használható. Ha például a jelentés segíthetnek észleli azokat az időszakokat a nap során fellépő nagyobb vagy kisebb, mint a várható forgalmat.

Sávdiagram esetén az ilyen típusú jelentés létrehozása, egy platform-specifikus igény szerint a jelentés tárgyát időszakban óránkénti kombinálásával tapasztalt mennyiségét vizuális feltüntetése nyújt. Azt fogja ehhez egy sáv megjelenítése minden olyan órában jelentés tárgyát. Például a 24 órás kijelölése időszak hoz létre húsz négy sávokkal sávdiagram. Mindegyik sáv jelzi, hogy órában tapasztalt találatok száma.

Bal oldalán a diagram (y) azt jelzi, hogy hány találatok az egy megadott órában történt. Közvetlenül a diagram (az x tengely) alatt megtalálja, amely jelzi a dátum/idő címke (formátum: YYYY-MM-DD hh: mm) minden olyan órában a jelentésben szereplő. 24 órás formátumban idő jelenteni, és van megadva, az Egyezményes/GMT időzóna használatával.

> [AZURE.TIP] Egy sáv mutat, ha adott órát előfordult találatok száma szerint elemleírás jelenik meg.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs találatok száma és (a GB-os) átvitt adatok mennyisége kifejezésre keresve minden olyan órában jelentés tárgyát.

## <a name="by-file"></a>Fájl

A fájl által jelentés megtekintése az igény szerinti és a forgalmat a legtöbb kért eszközök egy adott platformon keresztül felmerülő összegét teszi lehetővé. Esetén az ilyen típusú jelentés létrehozása, sávdiagram jön létre, a felső 10 leginkább igényelt eszközök a megadott időszakban.

> [AZURE.NOTE] Ez a jelentés alkalmazásában él CNAME URL-ek a egyenértékű CDN URL-címeit alakulnak. Egy tárgyi eszköz CDN vagy a CNAME kérhetik azt használt URL-cím él függetlenül társított találatok száma a pontos tally lehetővé teszi.

A diagram (y) bal oldalán lévő azt, hogy az egyes eszközök kérelmek a megadott időszakban. Közvetlenül a diagram (az x tengely) alatt találja, amely jelzi a fájl nevét a felső 10 kért eszközök minden egyes címke.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs megtalálja az alábbi adatokat az egyes a felső 250 kért eszközök: relatív elérési utat, a teljes oldalszám találatok, a találatok százaléka, adatok mennyiségét átvitt (gigabájt), és az átvitt adatok százalékát.

## <a name="by-file-detail"></a>Fájl részletei

A által fájl-részletek jelentés megtekintése az igény szerinti és a forgalom az egy adott tárgyi eszköz egy adott platformon keresztül felmerülő összegét teszi lehetővé. Ez a jelentés nagyon tetején van a részletek a fájl lehetőséget. Ez a beállítás a legtöbb kért eszközök listája a kijelölt platformon ismertetése Annak érdekében, hogy a fájl részletek szerinti jelentés készítése, szüksége lesz válassza ki a kívánt eszközt a részletek a fájl lehetőséget. Amely után sávdiagram fog jelezheti a napi igény szerint, a megadott időszakban létrehozott.

Bal oldalán a diagram (y) azt jelzi, hogy egy tárgyi eszköz egy adott napon tapasztalt kérések száma. Közvetlenül a diagram (az x tengely) alatt megtalálja azt a dátumot jelzi, hogy címke (formátum: YYYY-MM-DD) mely CDN-igény szerint az eszköz történt.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs találatok száma és az átvitt (gigabájt) adatok mennyiségét kifejezésre keresve minden nap, a jelentés tárgyát.

## <a name="by-file-type"></a>Fájltípus

A fájl típusa szerint jelentés megtekintése az igény szerinti és a fájltípus felmerült forgalom összegét teszi lehetővé. Gyűrű diagram esetén az ilyen típusú jelentés létrehozása, a felső 10 fájltípusok által generált találatok százalékos jelzi.

> [AZURE.TIP] A gyűrű diagram szelet mutat, ha az Internet médiatípus az, hogy a fájltípus estén elemleírás.

A gyűrű diagram létrehozásához használt adatok alatti megtekintheti. Nincs látni fogja a fájl nevét kiterjesztéssel/Internet médiatípus, a találatok száma, a találatok, százalékos (a GB-os) átvitt adatok mennyisége, és a felső 250 fájltípusok minden egyes átvitt adatok százalékát.

## <a name="by-directory"></a>Címtár

A címtár által jelentés megtekintése az igény szerinti és a tartalom egy bizonyos címtárból egy adott platformon keresztül felmerülő forgalom összegét teszi lehetővé. Sávdiagram esetén az ilyen típusú jelentés létrehozása, jelzi a felső 10 könyvtárak tartalmának által generált találatok száma.

### <a name="using-the-bar-chart"></a>A sávdiagram használatával

* Mutasson egy sáv relatív könyvtár elérési útját a megfelelő megtekintéséhez.
* A könyvtár almappába tárolt tartalmat nem számolja igény szerint címtár kiszámításakor. A számítás kizárólag a tényleges címtárban tárolt tartalom létrehozott kérelmek számát támaszkodik.
* Ez a jelentés alkalmazásában él CNAME URL-ek a egyenértékű CDN URL-címeit alakulnak. Egy tárgyi eszköz CDN vagy a CNAME kérhetik azt használt URL-cím él függetlenül társított összes statisztikai pontos tally lehetővé teszi.

A diagram (y) bal oldalán lévő azt jelzi, hogy az a 10 legjobb könyvtárak tárolt tartalmat kérelem száma. A diagram egyes sávján könyvtárában jelöli. A color-coding rendszer segítségével egyezik egy sávot egy mappába a felső 250 teljes könyvtárak szakaszban.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs megtalálja a következő információkat a felső 250 könyvtárak mindegyikére: relatív elérési utat, a teljes oldalszám találatok, a találatok százaléka, adatok mennyiségét átvitt (gigabájt), és az átvitt adatok százalékát.

## <a name="by-browser"></a>Böngészőben

A webböngésző által jelentés lehetővé teszi, hogy milyen böngészők használt kérése a tartalom megtekintése. Kördiagram esetén az ilyen típusú jelentés létrehozása, a felső 10 böngészők kezelnie kérések százalékos jelzi.

### <a name="using-the-pie-chart"></a>A diagram segítségével

* Mutasson a kördiagramon a böngészőben és a verzió megtekintése szelet.
* Ez a jelentés alkalmazásában a minden egyedi verziójának kombináció másik böngésző számít.
* A "Egyéb" nevű szeletet azt jelzi, hogy más böngészők és a verziókkal által kérések százalékos.

A diagram létrehozásához használt adatok alatti megtekintheti. Nincs a böngésző típus-/ verziószámmal, a találatok száma és a találatok százalékos kifejezésre keresve az egyes a felső 250 böngészők.

## <a name="by-referrer"></a>Referrer szerint

A által Referrer jelentés lehetővé teszi, hogy a legjobb ajánlók tartalom megjelenítése a kijelölt platformon. Egy referrer azt jelzi, hogy a hostname (állomásnév), amelyhez egy kérelmet jött létre. Sávdiagram esetén az ilyen típusú jelentés létrehozása, igény szerint (tehát találatok) generálja a 10 legjobb ajánlók mennyiségét jelzi.

Bal oldalán a diagram (y) azt jelzi, hogy az eszköz hibát észlelt a az egyes referrer kérések száma. A diagram minden sáv egy referrer jelöli. A color-coding rendszer segítségével egyezik egy sávot egy referrer a felső 250 Referrer szakaszban.

A sáv diagram létrehozásához használt adatok alatti megtekintheti. Nincs URL-CÍMÉT, találatok száma és a találatok a 250 a legjobb ajánlók készített kifejezésre keresve.

## <a name="by-download"></a>Letölthető

A le a jelentés lehetővé teszi, hogy a legtöbb kért tartalom letöltési mintázatok elemezheti. A kimutatás tetején, hogy összehasonlítja kísérelte meg a bejegyzett letöltött felső 10 kért eszközök letöltését sávdiagram tartalmazza. Minden sáv használható színkódos aszerint, hogy-e egy kísérlet letöltési (kék) vagy egy befejezett letöltési (zöld).

> [AZURE.NOTE] Ez a jelentés alkalmazásában él CNAME URL-ek a egyenértékű CDN URL-címeit alakulnak. Egy tárgyi eszköz CDN vagy a CNAME kérhetik azt használt URL-cím él függetlenül társított összes statisztikai pontos tally lehetővé teszi.

A bal oldalán a diagram (y) azt jelzi, hogy a fájl nevét a felső 10 kért eszközök minden egyes. Közvetlenül a diagram (az x tengely) alatt próbált/befejeződött letöltések teljes számát jelölő címkék találja.

Közvetlenül alatt a sávdiagram, az alábbi információk megjelennek a felső 250 kért eszközök: (beleértve a fájl neve) relatív elérési út, száma letöltött hányszor, hányszor volt a kért és kérelmeket, amely a teljes letöltés megszakításához százalékos.

> [AZURE.TIP] Az CDN-HTTP-ügyfél (azaz a böngészőben) nem arról tájékoztatja mikor tárgyi eszköz teljesen letöltődött. Eredményt adja, felkínálunk számítja ki, hogy egy eszköz teljesen letöltődött állapotkód és bájt-tartománya szerint kérések. A legfontosabb dolog azt keresni ezt a számítást, ha, hogy 200 OK állapotkód a kérelem eredménye. Ha igen, majd megnézi az bájt-tartomány kérések annak érdekében, hogy a teljes eszköz terjed ki. Végezetül azt hasonlítsa össze a mérete a kért eszköz átvitt adatok mennyiségét. Ha az átvitt adatok egyenlő vagy nagyobb, mint a fájl méretét és a bájt-tartomány kérések megfelelőek eszköz, majd a találatot lesznek megszámlálva teljes letöltésként.
>
>Ez a jelentés interpretive jellegét, miatt, érdemes szem előtt tartani a következő pontok, amelyek megváltoztathatják egységesebb és a jelentés pontosságát.
>
>* Forgalmat nem lehet pontosan előre jelzett, amikor a felhasználó-ügynökök korábbi verziókban eltérően működnek. Ez lehet eredményt bejegyzett letöltési 100 %-nál nagyobb.
>* Elemeket, amelyeket a HTTP fokozatos letöltése kihasználhatja előfordulhat, hogy nem kell pontosan jelöli a jelentés. Ennek oka a videoklipet a különböző pozícióba kérő felhasználók.

## <a name="by-404-errors"></a>404-es hibák szerint

A által 404-es hibák jelentés lehetővé teszi, hogy azonosítsa a tartalomtípust, amely a legtöbb 404-es nem található állapot kódok száma hoz létre. A jelentés tetején a 10 legjobb eszközök, amelynek 404-es nem található állapotkód visszaadott sávdiagram tartalmazza. A sávdiagram száma egy 404-es nem található állapotkódja azokat az eszközöket eredményező kéréseivel kérések hasonlítja össze. Minden sáv használható színkódos. A sárga sávra jelzik, hogy a kérelem eredményeként 404-es nem található állapotkód szolgál. Egy piros sáv használják, hogy az eszköz kérelem száma.

> [AZURE.NOTE] Ez a jelentés alkalmazásában vegye figyelembe az alábbiakat:
>
>* Találat tartozó állapotkód függetlenül tárgyi eszköz kérésének jelöli.
>* Él CNAME URL-ek a egyenértékű CDN URL-címeit alakulnak. Egy tárgyi eszköz CDN vagy a CNAME kérhetik azt használt URL-cím él függetlenül társított összes statisztikai pontos tally lehetővé teszi.

A diagram (y) bal oldalán lévő azt jelzi, hogy a fájl nevét a felső 10 kért eszközök 404-es nem található állapotkód eredményező minden egyes. Közvetlenül a diagram (az x tengely) alatt kérések száma és 404-es nem található állapotkód eredményező kérések számát jelölő címkék találja.

Közvetlenül alatt a sávdiagram, az alábbi információk megjelennek a felső 250 kért eszközök: (beleértve a fájl neve) relatív elérési utat, 404-es nem található állapotkód eredményező kérések száma, száma, hogy az eszköz felkérték hányszor és 404-es nem található állapotkód eredményező kérések százalékát.

## <a name="see-also"></a>Lásd még:
* [Azure CDN – áttekintés](cdn-overview.md)
* [A Microsoft Azure CDN valós idejű stat](cdn-real-time-stats.md)
* [A szabályok motor használata alapértelmezett HTTP működés felülbírálása](cdn-rules-engine.md)
* [Szegély-teljesítmény elemzése](cdn-edge-performance.md)
