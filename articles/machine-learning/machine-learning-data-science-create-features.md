<properties
    pageTitle="Ez a funkció a Cortana Analytics folyamat műszaki |} Microsoft Azure" 
    description="Ismerteti, szolgáltatás műszaki alkalmazásában, és gépi tanulási adatok fejlesztési folyamatának szerepkörében talál példákat."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Ez a funkció a Cortana Analytics folyamat műszaki 

Ez a témakör ismerteti a szolgáltatás műszaki alkalmazásában, és gépi tanulási adatok fejlesztési folyamatának szerepkörében talál példákat. A példák bemutatják, ez az eljárás használatával Azure gépi tanulási Studio alakzatformátumtól. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Ez a **menüben** , amelyek bemutatják, hogyan hozhat létre az adatok szolgáltatásai különböző környezetekben témakörökre mutató hivatkozásokat tartalmaz. Ezt a műveletet a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)a lépés nem.

Ez a funkció mérnöki kísérletek algoritmusok tanulmányi lehetőségek segítik a tanulási folyamatot nyers adatokból létrehozásával előrejelzési teljesítményének növeléséhez. A műszaki, és a szolgáltatások listáját az a keret határolja TDSP egy része a [Mi az a csapat adatok tudományos folyamat?](data-science-process-overview.md) Szolgáltatás mérnöki és a kijelölést a TDSP **fejlesztése szolgáltatások** lépésénél részei. 

* **a szolgáltatás műszaki**: ezt a folyamatot megpróbálja megfelelő Továbbiak készítése az adatok a meglévő nyers funkciók és tanulási algoritmus előrejelzési teljesítményének növeléséhez.

* **a szolgáltatás kiválasztása**: ezt a folyamatot csökkentheti a dimensionality a oktatás probléma valaki megpróbálja kijelöli a eredeti adatfunkciók fő része.

Szokásos **szolgáltatás műszaki** első érvényesül további funkciókkal létrehozásához, és kattintson a **szolgáltatás kiválasztása** lépés nem számít, ismétlődnek vagy nagymértékben korrelációs szolgáltatások kiküszöbölése érdekében történik.

Gépi tanulási használt oktatóanyag adatok gyakran bővíthetők kivonással funkciói gyűjtött nyers adatokból. Egy olyan visszafejtett szolgáltatás elsajátítása sorolják be a képek kézzel írt karakterek környezetében példája a helyes összeállítás terjesztési bit nyers adatokból egy kicsit sűrűség térkép létrehozása. A térkép segíthetnek, keresse meg dolgozhat hatékonyabban egyszerűen a nyers eloszlás közvetlenül a szegélyek a karaktereket.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Szolgáltatások létrehozása az adatokból – Ez a funkció műszaki

Az oktatóanyag adatok, amelyek tartozik egy szolgáltatások (változók és az oszlopokban tárolt mezők) tevődik össze (rekordok vagy sorok tárolt észrevételek), példák mátrixok áll. A kísérleti Tervező megadott szolgáltatások szabadságfokának, a mintázatok, az adatok várható. Bár a nyers adatokból mezők számos közvetlenül beépíthetők a kijelölt szolgáltatáskészlet láthatja el ismeretekkel a modellben használva, akkor gyakran a helyzet, hogy a Továbbiak (visszafejtett) kell-e alakítható ki a található egy továbbfejlesztett oktatás adatkészlet létrehozásához nyers adatokból.

Milyen típusú szolgáltatások létre kell hozni az adatkészlet tökéletesíthetik, amikor egy a modellben képzési? Továbbfejlesztett képzés visszafejtett szolgáltatások információkat jobban különbséget tesz a mintázatok, az adatok. További információkat, amely nem egyértelmű rögzített vagy egyszerűen látható eredeti vagy meglévő szolgáltatás megadása az új szolgáltatások megakadályozási. De ez a folyamat elindít egy elem. Hang- és hatékonyabban döntéseket gyakran szükség bizonyos tartomány szakértelmét.

Azure gépi tanulási indításakor legegyszerűbben bonyolultnak ezt a folyamatot, konkrétan a megadott a Studio minták használatával. Két példa a Itt jelennek meg:

* Egy felügyelt kísérlet célérték tudjuk, ahol a regressziós példa [előrejelzési kerékpárhoz bérleti díjak száma](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)
* A szöveg adatbányászati példa sorbesorolásra [Kivonatolás funkció](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) használatával

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Példa 1: Időbeli szolgáltatások hozzáadása a regressziós modell ###

Használata a kísérlet "igény szerint az előrejelzés a kerékpárok" Azure gépi tanulási Studio a regressziós feladat szolgáltatások vissza szemléltetik. Az e kísérlet célja, hogy az igény szerinti Bikes, ez azt jelenti, hogy hány kerékpárhoz bérleti díjak adott hónap/nap/órán belül előre. Az adatkészlet "kerékpárhoz bérleti UCI adatkészlet" a bemeneti adatokat használják. Ez az adatkészlet megtartja az Amerikai Egyesült Államokban Washington Adatközpont kerékpárhoz bérleti hálózat tőke Bikeshare vállalattól tényleges adatok alapján. Az adatkészlet jelöli kerékpárhoz bérleti díjak száma a 2011-ben és az év 2012 években nap adott órán belül, és 17379 sorok és oszlopok 17 tartalmazza. A nyers szolgáltatás időjárási feltételek (hőmérsékleti/páratartalom/szélsebesség) és a nap (munkaszüneti nap/hét napjának) típusú tartalmazza. A mező előrejelzésére "számláló", a száma, amelyek a kerékpárhoz bérleti díjak belül egy megadott órában jelöli, és amelyek csak jelszóval módosítható tartományok 977 1 tartományok.

Az oktatóanyag adatok, négy regressziós modellek kialakításának használ, ugyanazt az algoritmust, de négy különböző oktatás adatkészleteket hatékony szolgáltatások megépítése a célt. A négy adatkészleteket az azonos nyers bemeneti adatok megjelenítése, de a szolgáltatások növekvő számú beállítása. Ezek a funkciók négy kategóriába sorolhatók:

1. A = időjárási + ünnepi + hét.napja + szolgáltatások Hétvége a becsült nap
2. B =, amely az egyes az előző 12 órás voltak bérelt kerékpárok száma
3. C =, amely minden azonos órára előző 12 napjait voltak bérelt kerékpárok száma
4. D =, amely az egyes az azonos órát és azonos napjára az előző 12 hétben voltak bérelt kerékpárok száma

Már létezik az eredeti nyers adatokból, szolgáltatás beállítása A mellett a többi három különböző szolgáltatások a tervezési folyamat funkció révén jönnek létre. Szolgáltatás a Bikes nagyon legutóbbi igény szerint adja meg a B rögzített tartalmak. A szolgáltatás értéken C rögzített tartalmak az igény szerinti Bikes egy adott óra. A szolgáltatás Bikes D rögzített tartalmak igény szerint állítsa adott órát és a hét napjának adott. A négy oktatás adatkészleteket minden szolgáltatás beállítása A, A + B, A + B + C billentyűkombinációt, és tartalmazza A + B + C + D rendre.

Az Azure gépi tanulási kísérletet a e négy oktatás adatkészleteket kialakított az előre feldolgozott beviteli adatkészlet a négy ágak keresztül. Egy [végrehajtás R parancsfájl](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) modulban, amelyben egy sor olyan származtatott szolgáltatások (a szolgáltatás beállítása B, C és a D) a bal oldali, a legtöbb ilyen ág ág tartalmazza, kivéve a kurzor összeállítás és az importált adatkészlet fűzött. Az alábbi ábra bemutatja az R parancsfájl, a második bal ág szolgáltatáskészlet B létrehozásához használt.

![funkciók létrehozása](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

A teljesítmény eredmények négy modellek összehasonlítása az alábbi táblázatban soroljuk fel. A legjobb eredményt funkciók jelennek meg A + B + C billentyűkombinációt. Megjegyzés: a hiba ráta kevesebb mikor további szolgáltatáskészlet szerepelnek az oktatóanyag adatok. Ellenőrzi, hogy az a, B, C szolgáltatás megadása található további információt a regressziós tevékenység vélelem. De a D-szolgáltatás hozzáadása úgy tűnik, hogy minden további hiba mértékének csökkentése, adja meg.

![eredmény összehasonlítása](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Példa 2: Szöveg adatbányászati szolgáltatások létrehozása  

A szöveg adatbányászati, például minősítést és sentiment dokumentumelemzés kapcsolódó feladatok szolgáltatás műszaki körben érvényes. Például ha azt szeretné, hogy a dokumentumok osztályozásához számos kategóriába sorolhatók, egy tipikus feltételezésen is, hogy egy dokumentumot kategóriába sorolt word/mondatok valószínűséggel kevesebb egy másik dokumentumba kategóriájában. A másik szavakat a szavakat vagy kifejezéseket eloszlás gyakoriságának el tudja látni a dokumentum különböző kategóriák szabadságfokának. Szöveg adatbányászati alkalmazásokban mert adott hardvereszközöket szöveg tartalmát általában lesz a bemeneti adatok a tervezési folyamat funkció van szükség a szó vagy kifejezés gyakoriságok érintő funkciók létrehozásához.

Ebben a feladatban eléréséhez nevű **szolgáltatás kivonatolás** technika érvényes tetszőleges text funkcióinak hatékony alakítani tárgymutatók használata. Helyett szöveg szolgáltatások (szavakat vagy kifejezéseket) társítása egy adott indexbe, ez módszer függvények kivonat függvény alkalmazásával funkciókhoz, és közvetlenül a kivonat értékeik használják tárgymutatók használata.

Az Azure gépi tanulási, [Szolgáltatás kivonatolás](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul, amely hozza létre ezeket a szó vagy kifejezés szolgáltatások kényelmesen van. Következő ábrán látható példa a modul használatára. A beviteli adatkészlet két oszlopot tartalmaz: a címjegyzék értékelése az 1-től 5-ig, alaptartományban és a tényleges Véleményezés tartalmat. Ez a [Funkció a kivonatolás](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) modul célja történő kitöltenem, az új szolgáltatásait, hogy a megfelelő szavak előfordulás gyakoriságának megjelenítés / tájékoztatás(ok) belül az adott könyv áttekintése. Ez a modul használatára, kövesse az alábbi lépéseket szükség:

* Első lépésként válassza ki a bemeneti szöveget tartalmazó oszlopot (ebben a példában a "Col2" jelöli).
* Második, állítsa a "Hashing bitsize" 8, ami azt jelenti, hogy 2 ^ 8 = 256 szolgáltatások jön létre. A teljes szöveget a word/fázis fog kivonatolható való 256 tárgymutatók használata. A paraméter "Hashing bitsize" csak jelszóval módosítható tartományok 1 és 31. A szavak / tájékoztatás(ok) várhatóan kisebb kivonatolható be ugyanazt az indexet, ha beállítása, hogy legyen a nagyobb számot.
* Harmadik, a paraméter értéke "N-g" 2. Az előfordulás gyakorisága unigrams (az összes egyetlen szó funkció) és bigrams (egy minden két szavak szomszédos funkció) megszerzi a beírt szöveg. A paraméter "N-g" csak jelszóval módosítható tartományok 0-azt jelzi, egymás után következő szavak bekerülnek az egyik funkciójáról maximális száma 10.  

!["Ez a funkció Hashing" modul](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Az alábbi ábrán látható, mi a következő új szolgáltatás megjelenés hasonló.

!["Ez a funkció Hashing" példa](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Elfogadásáról

Tervezett és a kijelölt szolgáltatások hatékonyságnövelő a oktatás folyamat, amely megkísérli bontsa ki az adatokat a főbb információk. Is növeli e modellek power sorolják be pontosan a bemeneti adatok, és további abroncsnyomásmérők erős előrejelzésére kamat eredményeit. Szolgáltatás mérnöki és a kijelölés kombinálhatja is, hogy a tanulási több olyan végeredményt tractable. Így fejlesztése, és kattintson a szolgáltatások szükséges kalibrálhatja vagy láthatja el ismeretekkel a modell számának csökkentése szerint kezeli. Matematikai beszél, a ki van jelölve a modell képzése szolgáltatásai, amelyek bemutatják, a mintázatok, az adatok, és ezután előrejelzésére sikeres eredmény független változó minimális számú.

Ne feledje, hogy nem mindig feltétlenül szolgáltatásainak műszaki vagy a szolgáltatás kiválasztása végrehajtásához. E rá szükség, vagy sem, az adatokat, hogy rendelkezik, vagy összegyűjtése, akkor válassza ki a algoritmus és a kísérlet a cél függ.
 
