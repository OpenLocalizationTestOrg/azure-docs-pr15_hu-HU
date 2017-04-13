<properties
    pageTitle="Ez a funkció mérnöki és Azure gépi tanulási kijelölés |} Microsoft Azure"
    description="Ismerteti, szolgáltatásainak kiválasztása és a szolgáltatás műszaki alkalmazásában, és gépi tanulási adatok-finomító folyamatának szerepe talál példákat."
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
    ms.date="09/12/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Ez a funkció mérnöki és Azure gépi tanulási kijelölés

Ez a témakör ismerteti a műszaki funkció és szolgáltatás kiválasztása a adatok-finomító folyamat gépi tanulási céljából. Azt mutatja be, hogy Mik ezek a folyamatok Azure gépi tanulási Studio által biztosított példák használatával járnak.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Gépi tanulási használt oktatóanyag adatok gyakran bővíthetők a kijelölés vagy a szolgáltatások kivonása gyűjtött nyers adatokból. Példa olyan visszafejtett szolgáltatás elsajátítása sorolják be a képek kézzel írt karakterek környezetében terjesztési bit nyers adatokból összeállítás bit-sűrűség térkép. A térkép segíthetnek, keresse meg a nyers eloszlás hatékonyabban a szegélyek a karaktereket.

Tervezett és a kijelölt szolgáltatások hatékonyságnövelő oktatás folyamat, próbálja meg az adatokat a főbb információk kibontásához. Is növeli e modellek power sorolják be pontosan a bemeneti adatok, és további abroncsnyomásmérők erős előrejelzésére kamat eredményeit. Szolgáltatás mérnöki és a kijelölés kombinálhatja is, hogy a tanulási több olyan végeredményt tractable. Így fejlesztése, és kattintson a szolgáltatások szükséges kalibrálhatja vagy láthatja el ismeretekkel a modell számának csökkentése szerint kezeli. Matematikai beszél, a ki van jelölve a modell képzése szolgáltatásai, amelyek bemutatják, a mintázatok, az adatok, és ezután előrejelzésére sikeres eredmény független változó minimális számú.

A műszaki, és a szolgáltatások listáját része egy nagyobb folyamat, és a szokásos négy lépésből áll:

* Adatok gyűjtése
* Adatok adatbővítésre
* Építési modell
* Utáni feldolgozása

Tervezés és a kijelölés alkotó gépi tanulási az adatok finomító lépése. Ez a folyamat három szempont a mostani célok szempontjából különböztethető meg:

* **Adatkezelési előtti**: ezt a folyamatot próbál győződjön meg arról, hogy a gyűjtött adatok világos és egységes. Feladatok például több adatkészletek, hiányzó adatok kezelése, kezelési ellentmondó adatok integrálása és adattípusok konvertálása tartalmazza.
* **Ez a funkció műszaki**: ezt a folyamatot megpróbálja megfelelő Továbbiak készítése az adatok a meglévő nyers funkciók és növelik a tanulási módszerével prediktív power.
* **A funkció kijelölési**: Ez az eljárás kijelöli az eredeti adatok szolgáltatások csökkentheti a dimensionality a oktatás probléma kulcs részhalmazát.

Ez csak a témakör a szolgáltatás mérnöki és az adatok finomító folyamat funkció kijelölési szempontjait. Az adatok feldolgozása előre lépés a további tudnivalókért lásd: a [előfeldolgozása Azure gépi tanulási Studio adatok](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Szolgáltatások létrehozása az adatokból – Ez a funkció műszaki

Az oktatóanyag adatok, amelyek tartozik egy szolgáltatások (változók és az oszlopokban tárolt mezők) tevődik össze (rekordok vagy sorok tárolt észrevételek), példák mátrixok áll. A kísérleti Tervező megadott szolgáltatások szabadságfokának, a mintázatok, az adatok várható. Bár a nyers adatokból mezők számos közvetlenül beépíthetők a kijelölt szolgáltatáskészlet láthatja el ismeretekkel a modellben használva, további visszafejtett funkciói gyakran kell alakítható ki a található egy továbbfejlesztett oktatás adatkészlet létrehozásához nyers adatokból.

Milyen típusú szolgáltatások tökéletesíthetik az adatkészlet, amikor egy a modellben képzési létre kell hozni? Továbbfejlesztett képzés visszafejtett szolgáltatások információkat jobban különbséget tesz a mintázatok, az adatok. Az új szolgáltatások további információkat, amelyek egyértelműen rendszer nem rögzít várt, vagy egyszerűen látható az eredeti vagy meglévő szolgáltatáskészlet, de ez a folyamat elindít egy elem. Hang- és hatékonyabban döntéseket gyakran szükség bizonyos tartomány szakértelmét.

Azure gépi tanulási indításakor legegyszerűbben a folyamat konkrétan bonyolultnak gépi tanulási Studio leírt minták használatával. Két példa a Itt jelennek meg:

* Egy felügyelt kísérlet célérték tudjuk, ahol a regressziós példa ([előrejelzési kerékpárhoz bérleti díjak száma](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4))
* A szöveg-adatbányászati besorolás példa [Kivonatolás funkció] használatával[feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>1 – példa a regressziós modell időbeli funkcióinak felvétele ###

A regressziós feladat szolgáltatások vissza szemléltetik, használata a kísérlet "igény szerint az előrejelzés a kerékpárok" Azure gépi tanulási Studio. A kísérlet célja előrejelzésére az igény szerinti Bikes, ez azt jelenti, hogy a kerékpárhoz bérleti díjakat, napi és óra egy adott hónapba eső szám. Az adathalmaz **kerékpárhoz bérleti UCI adatkészlet** a bemeneti adatokat használják.

Az adatsort, amely megőrzi az Amerikai Egyesült Államokban Washington Adatközpont kerékpárhoz bérleti hálózat tőke Bikeshare vállalattól tényleges adatok alapján. Az adathalmaz jelöli kerékpárhoz bérleti díjak száma a nap, egy adott órán belül 2012 2011-ben, és 17379 sorok és oszlopok 17 tartalmaz. A nyers szolgáltatás beállítása (hőmérsékleti, páratartalom, szélsebesség) időjárási feltételek és a nap (munkaszüneti nap vagy hét.napja) típusú tartalmazza. A mező előrejelzésére **számláló**és az adott órán belül a kerékpárhoz bérleti díjak képviselő, és, amely csak jelszóval módosítható tartományok 1 977 szeretné a számát.

Az oktatóanyag adatok hatékony szolgáltatások összeállításához, négy regressziós modellek ugyanazt a algoritmust használatával, de a négy különböző oktatás adathalmazok készültek. Négy adatkészletek az azonos nyers bemeneti adatok megjelenítése, de a szolgáltatások növekvő számú beállítása. Ezek a funkciók négy kategóriába sorolhatók:

1. A = időjárási + ünnepi + hét.napja + szolgáltatások Hétvége a becsült nap
2. B =, amely az egyes az előző 12 órás voltak bérelt kerékpárok száma
3. C =, amely minden azonos órára előző 12 napjait voltak bérelt kerékpárok száma
4. D =, amely az egyes az azonos órát és azonos napjára az előző 12 hétben voltak bérelt kerékpárok száma

Amellett, hogy már létezik az eredeti nyers adatokból, szolgáltatás beállítása A szolgáltatások más három beállítása a tervezési folyamat funkció révén jönnek létre. Szolgáltatás a Bikes a legutóbbi igény szerint adja meg a B rögzített tartalmak. A szolgáltatás értéken C rögzített tartalmak az igény szerinti Bikes egy adott óra. A szolgáltatás Bikes D rögzített tartalmak igény szerint állítsa adott órát és a hét napjának adott. Minden egyes oktatás négy adatkészletek magában foglalja a szolgáltatás beállítása A, A + B, A + B + C billentyűkombinációt, és A + B + C + D rendre.

Az Azure gépi tanulási kísérletet a e négy oktatás adatkészletek kialakított e négy használja az előre feldolgozott beviteli adatokból. A bal szélső ág kivételével minden ilyen ág tartalmazza az [R parancsfájl végrehajtása] [ execute-r-script] , amelyben egy sor olyan szolgáltatásokat (szolgáltatás állítja be, B, C és a D) származtatott modul rendre összeállítás és az importált adatkészlet fűzött. Az alábbi ábra bemutatja az R parancsfájl, a második bal ág szolgáltatáskészlet B létrehozásához használt.

![Hozzon létre egy szolgáltatáskészlet](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Az alábbi táblázat összefoglalja az összehasonlítás négy modellek teljesítmény eredményeit. A legjobb eredményt funkciók jelennek meg A + B + C billentyűkombinációt. Figyelje meg, hogy a hiba ráta kevesebb, amikor az oktatóanyag adatok szereplő további szolgáltatás beállítása. Ez ellenőrzi, hogy a, B és C a szolgáltatás beállítása található további információt a regressziós tevékenység vélelem. A D szolgáltatáskészlet hozzáadása úgy tűnik, hogy minden további hiba mértékének csökkentése, adja meg.

![Teljesítmény eredmény összehasonlítása](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Példa 2: Szöveg adatbányászati szolgáltatások létrehozása  

A szöveg adatbányászati, például minősítést és sentiment dokumentumelemzés kapcsolódó feladatok szolgáltatás műszaki körben érvényes. Például ha dokumentumok osztályozásához számos kategóriába sorolhatók, egy tipikus feltételezi, hogy a szavak vagy kifejezések egy dokumentum kategóriába sorolt valószínűséggel kisebb másik dokumentum kategóriájában. Ez azt jelenti a szó vagy kifejezés eloszlás gyakoriságának el tudja látni a dokumentum különböző kategóriák szabadságfokának. A szöveg adatbányászati alkalmazások a tervezési folyamat funkció hozhat létre az adott szó vagy kifejezés gyakoriságok, mert adott hardvereszközöket szöveg tartalmát általában lesz a bemeneti adatok funkciók van szükség.

Ebben a feladatban eléréséhez nevű *szolgáltatás kivonatolás* technika érvényes tetszőleges text funkcióinak hatékony alakítani tárgymutatók használata. Helyett szöveg szolgáltatások (szavakat vagy kifejezéseket) társítása egy adott indexbe, ez módszer függvények funkciókhoz kivonat függvény alkalmazásával, és közvetlenül használatával kivonat értékeik tárgymutatók használata.

Az Azure gépi tanulási, van a [Szolgáltatás kivonatolás] [ feature-hashing] modul, amely a szó vagy kifejezés funkciókról hoz létre. Az alábbi ábrán látható példa a modul használatára. A beviteli adathalmazban két oszlopot: a címjegyzék értékelése az 1-től 5-öt és a tényleges tekintse át a tartalmat. Ez a [Funkció a kivonatolás] célját[ feature-hashing] modul az új funkciók, amelyek a megfelelő szavak vagy kifejezések belül az adott könyv Véleményezés előfordulás gyakoriságának megjelenítése beolvasásához. Ez a modul használatához kell hajtsa végre az alábbi lépéseket:

1. Jelölje be a beviteli (**Col2** ebben a példában) tartalmazó oszlop.
2. Állítsa *Hashing bitsize* 8, ami azt jelenti, 2 ^ 8 = 256 szolgáltatások jönnek létre. Szó vagy kifejezés szöveg majd kivonatolt való 256 tárgymutatók használata. A paraméter *Hashing bitsize* csak jelszóval módosítható tartományok 1 és 31. Ha a paraméter értéke nagyobb számot, a szavakat vagy kifejezéseket valószínűleg kisebb be ugyanazt az indexet kivonatolható.
3. Állítsa a paraméter *N-g* 2. Az előfordulás gyakorisága unigrams (az összes egyetlen szó funkció) és bigrams (egy minden két szavak szomszédos funkció) beolvassa a beírt szöveg. A paraméter *N-g* csak jelszóval módosítható tartományok 0-10 jelző bekerülnek az egyik funkciójáról egymás után következő szavak maximális száma.  

![Ez a funkció ujjlenyomat modul](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Az alábbi ábrán látható, hogy ezek a funkciók kinézni.

![Ez a funkció ujjlenyomat példa](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Szűrés funkciói az adatokból – szolgáltatás kiválasztása  ##

A következő oktatás adatkészletek például osztályozás és a regressziós feladatok prediktív modellezési tevékenységek szerkezetének leggyakrabban alkalmazott áll *szolgáltatásainak kiválasztása* . A cél, jelölje ki a funkciók csak egy részhalmazát adatkészlet, amely a méreteit csökkenti a legnagyobb mennyiségű varianciája, az adatok ábrázolására minimális számú funkciók használatával. A szolgáltatások részhalmazát a felvenni kívánt a modell betanítása a csak elemeket tartalmaz. Szolgáltatásainak kiválasztása két fő célt szolgálja:

* Szolgáltatásainak kiválasztása gyakran nő besorolás pontosság nincs jelentősége, felesleges kiküszöbölése, vagy nagymértékben összefüggésben funkciók.
* Szolgáltatásainak kiválasztása csökkenti a funkciókat, amely lehetővé teszi a modell képzési folyamat hatékonyabb számát. Ez különösen fontos, amelyek költséges támogatási vektoros gépek például láthatja el ismeretekkel a tanulók számára.

Szolgáltatásainak kiválasztása az adatkészlet a modell képzése használt szolgáltatásainak számának csökkentése érdekében kér, bár nem általában szerepel a kifejezés *dimensionality csökkentésére.* Funkció kijelölési módszereket figyelmen kívül hagyásával őket bontsa ki az adatokat az eredeti lehetőségei egy részét.  Dimensionality csökkentése módszerek alkalmazhat visszafejtett funkciók, amelyek képesek átalakítása az eredeti funkciók, és így módosíthatja őket. Dimensionality csökkentése módszerek többek között az egyszerű összetevő elemzés, kanonikus korrelációs elemzés és egyes érték dekompozíciós.

Egy széles körben alkalmazott kategória funkció kijelölési módszerek felügyelt környezetben szűrő-alapú szolgáltatásainak kiválasztása. Egyes szolgáltatások és a cél attribútum összefüggés kiértékelése, ezeket a módszereket alkalmazása egy pontszám hozzárendelése a szolgáltatások statisztikai mértéket. A funkciók majd rangsora által is használhatja a tartására vagy megszüntetése egy adott szolgáltatást küszöbérték meg eredmény. A statisztikai mértékek, használja az alábbi módszerek egyikét közé a Pearson-féle korrelációs, kölcsönös információk és a khi-négyzet-próba.

Azure gépi tanulási Studio szolgáltatásainak kiválasztása nyújt a modulokat. Az alábbi ábrán látható, ezeket a modulokat tartalmaznia kell [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] és [Fisher lineáris Discriminant elemzés][fisher-linear-discriminant-analysis].

![Példa a szolgáltatás kiválasztása](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Ha például használja a [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] korábban tagolt szöveges adatbányászati példa a modul. Tegyük fel, hogy szeretne-e egy sor olyan 256 szolgáltatások – a [Szolgáltatás kivonatolás] létrehozását követően össze a regressziós modell[ feature-hashing] modult, és, hogy a válasz változó **Oszlop1** és a könyv jelöli áttekintése értékelése az 5-ig terjedő 1. **Szolgáltatás pontozási módszer** beállítása a **Pearson-féle korrelációs**, a **cél oszlop** **Oszlop1**és **50** **kívánt szolgáltatások számát** . A modul [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] együtt a cél attribútum **Oszlop1**50 funkciókat tartalmazó adatkészlet majd eredményez. Az alábbi ábrán az e kísérletből és a bemeneti paramétereket.

![Példa a szolgáltatás kiválasztása](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Az alábbi ábrán az eredményül kapott adatkészletek. Egyes szolgáltatásokhoz a Pearson-féle korrelációs magát, és a cél attribútum **Oszlop1**között van elért hány a rendszer. A szolgáltatások, amelyeknek felső értékek maradnak.

![Szűrő-alapú funkció kijelölési adatkészletek](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Az alábbi ábrán látható a megfelelő értékek kijelölt funkciói.

![A kijelölt szolgáltatás statisztikákat?](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Ez a [funkció kijelölési szűrő-alapú] alkalmazásával[ filter-based-feature-selection] modulban 50 "Házon kívül" 256 szolgáltatások legyen kijelölve, mert a legtöbb változó **Oszlop1** alapján a pontozási módszer **Pearson-féle korrelációs**tárolóval összefüggésben szolgáltatások rendelkeznek.

## <a name="conclusion"></a>Elfogadásáról
Szolgáltatás mérnöki és szolgáltatásainak kiválasztása a gyakran végzett előkészítése az oktatóanyag adatok, egy gépi tanulási modell készítésekor, a két lépésből áll. A szokásos módon szolgáltatás műszaki további funkciókkal létrehozásához először alkalmazza, és kattintson a szolgáltatás kiválasztása lépés nem számít, ismétlődnek vagy nagymértékben korrelációs szolgáltatások kiküszöbölése érdekében történik.

Még nem mindig feltétlenül szolgáltatásainak műszaki vagy a szolgáltatás kiválasztása végrehajtásához. E szükség van, vagy összegyűjtése az adatokat, a algoritmus választhat és a kísérlet a cél függ.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
