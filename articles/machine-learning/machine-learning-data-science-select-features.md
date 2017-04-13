<properties
    pageTitle="Ez a funkció kijelölési a adatok tudományos folyamatban |} Microsoft Azure" 
    description="Szolgáltatásainak kiválasztása célját, és gépi tanulási adatok fejlesztési folyamatának szerepe talál példákat."
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


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>A csoportwebhely adatok tudományos folyamat (TDSP) szolgáltatásainak kiválasztása

Ez a cikk ismerteti, szolgáltatásainak kiválasztása alkalmazásában, és gépi tanulási adatok fejlesztési folyamatának szerepkörében talál példákat. Ezek a példák Azure gépi tanulási Studio alakzatformátumtól. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Ez a témakör ismerteti, szolgáltatásainak kiválasztása célját, és gépi tanulási adatok fejlesztési folyamatának szerepkörében talál példákat. Ezek a példák Azure gépi tanulási Studio alakzatformátumtól. 

A műszaki, és a szolgáltatások listáját az a keret határolja TDSP egy része a [Mi az a csapat adatok tudományos folyamat?](data-science-process-overview.md). Szolgáltatás mérnöki és a kijelölést a TDSP **fejlesztése szolgáltatások** lépésénél részei.

* **a szolgáltatás műszaki**: ezt a folyamatot megpróbálja megfelelő Továbbiak készítése az adatok a meglévő nyers funkciók és a tanulási módszerével prediktív power növelése.

* **a szolgáltatás kiválasztása**: ezt a folyamatot csökkentheti a dimensionality a oktatás probléma valaki megpróbálja kijelöli a eredeti adatfunkciók fő része.

Szokásos **szolgáltatás műszaki** első érvényesül további funkciókkal létrehozásához, és kattintson a **szolgáltatás kiválasztása** lépés nem számít, ismétlődnek vagy nagymértékben korrelációs szolgáltatások kiküszöbölése érdekében történik.


## <a name="filtering-features-from-your-data---feature-selection"></a>Szűrési funkciók az adatokból – szolgáltatás kiválasztása 

A szolgáltatás érték egy folyamat, amely gyakran alkalmazza a oktatás adatkészleteket, például osztályozás és a regressziós feladatok prediktív modellezési tevékenységek szerkezetének. A cél, jelölje ki a funkciók csak egy részhalmazát az eredeti adatkészlet méreteit csökkentő legnagyobb mennyiségű varianciája, az adatok ábrázolására minimális számú funkciók használatával. A szolgáltatások részhalmazát ezt követően a fogja tartalmazni a modell betanítása csak funkcióját tartalmazza. Szolgáltatásainak kiválasztása két fő célt szolgálja.

* Első lépésként szolgáltatásainak kiválasztása gyakran nő besorolás pontosság nincs jelentősége, felesleges kiküszöbölése vagy erősen összefüggésben funkciók.
* Második így csökken funkciói hatékonyabbá teszi a modell képzési folyamat számát. Ez különösen fontos, amelyek költséges támogatási vektoros gépek például láthatja el ismeretekkel a tanulók számára.

Bár szolgáltatásainak kiválasztása válassza ki azt az adatkészlet a modell képzése használt szolgáltatásainak számának csökkentése, azt nem általában hivatkozik "dimensionality csökkentése" kifejezés. Funkció kijelölési módszereket figyelmen kívül hagyásával őket bontsa ki az adatokat az eredeti lehetőségei egy részét.  Dimensionality csökkentése módszerek alkalmazhat visszafejtett funkciók, amelyek képesek átalakítása az eredeti funkciók, és így módosíthatja őket. Dimensionality csökkentése módszerek többek között az egyszerű összetevő-elemzés, kanonikus korrelációs elemzés és egyes érték dekompozíciós.

Egyebek mellett egy körben alkalmazott kategória funkció kijelölési módszerek felügyelt környezetben neve "alapuló szűrők szolgáltatásainak kiválasztása". Egyes szolgáltatások és a cél attribútum összefüggés kiértékelése, ezeket a módszereket alkalmazása egy pontszám hozzárendelése a szolgáltatások statisztikai mértéket. A funkciók majd rangsora által a pontszám, amelyek felhasználhatók megkönnyíti a tartására vagy megszüntetése egy adott szolgáltatást küszöbérték beállítását. A statisztikai mértékek, az alábbi módszerek egyikét használt többek között személy korrelációs, kölcsönös információk és a Szlov négyzetben vizsgálatot.

Azure gépi tanulási Studio vannak modulok szolgáltatásainak kiválasztása megadva. Az alábbi ábrán látható, ezeket a modulokat tartalmaznia kell [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] és [Fisher lineáris Discriminant elemzés][fisher-linear-discriminant-analysis].

![Példa a szolgáltatás kiválasztása](./media/machine-learning-data-science-select-features/feature-Selection.png)


Fontolja meg, például a használatát a [szűrő-alapú szolgáltatásainak kiválasztása] [ filter-based-feature-selection] modulra. Céljából kényelmesebbé továbbra is használja a fent ismertetett szöveges adatbányászati példa. Tegyük fel, hogy össze a regressziós modell 256 szolgáltatások halmazának keresztül [Szolgáltatás kivonatolás] létrehozása után szeretnénk[ feature-hashing] modult, és, hogy a válasz változó "Oszlop1" és a könyv jelöli tekintse át az 1-től 5-ig terjedő minősítések. "Ez a funkció pontozási módszer" megadásával "Pearson-féle korrelációs", "céloszlopban" kell "Oszlop1" és "Kívánt szolgáltatások száma" 50. Kattintson a [szűrő-alapú szolgáltatásainak kiválasztása] modul[ filter-based-feature-selection] hoznak létre együtt a cél attribútum "Oszlop1" 50 funkciókat tartalmazó adatkészletet. Az alábbi ábrán látható az a kísérlet és a bemeneti paramétereket csak leírt.

![Példa a szolgáltatás kiválasztása](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Az alábbi ábrán az eredményül kapott adatkészleteket. Egyes szolgáltatásokhoz a Pearson-féle korrelációs magát, és a cél attribútum "Oszlop1" között van elért hány a rendszer. A szolgáltatások, amelyeknek felső értékek maradnak.

![Példa a szolgáltatás kiválasztása](./media/machine-learning-data-science-select-features/feature-Selection2.png)

A kijelölt szolgáltatások a megfelelő értékek az alábbi ábrán látható.

![Példa a szolgáltatás kiválasztása](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Ez a [funkció kijelölési szűrő-alapú] alkalmazásával[ filter-based-feature-selection] modulban 50 "Házon kívül" 256 szolgáltatások legyen kijelölve, mert a legtöbb korrelációs szolgáltatások, amelyeknek a cél változó "Oszlop1", "Pearson-féle korrelációs" pontozási módszer alapján.

## <a name="conclusion"></a>Elfogadásáról
Szolgáltatás mérnöki és szolgáltatásainak kiválasztása két gyakran megtervezett, és a kijelölt szolgáltatások hatékonyságnövelő a oktatás folyamatot, amely a adataiban kísérletek ki kell olvasni a termékkulccsal kapcsolatos adatokat. Is növeli e modellek power sorolják be pontosan a bemeneti adatok, és további abroncsnyomásmérők erős előrejelzésére kamat eredményeit. Szolgáltatás mérnöki és a kijelölés kombinálhatja is, hogy a tanulási több olyan végeredményt tractable. Így fejlesztése, és kattintson a szolgáltatások szükséges kalibrálhatja vagy láthatja el ismeretekkel a modell számának csökkentése szerint kezeli. Matematikai beszél, a ki van jelölve a modell képzése szolgáltatásai, amelyek bemutatják, a mintázatok, az adatok, és ezután előrejelzésére sikeres eredmény független változó minimális számú.

Ne feledje, hogy nem mindig feltétlenül szolgáltatásainak műszaki vagy a szolgáltatás kiválasztása végrehajtásához. E rá szükség, vagy sem, az adatokat, hogy rendelkezik, vagy összegyűjtése, akkor válassza ki a algoritmus és a kísérlet a cél függ.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
