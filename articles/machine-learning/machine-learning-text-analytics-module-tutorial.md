<properties
    pageTitle="Szöveg analytics modellek létrehozása az Azure gépi tanulási Studio |} Microsoft Azure"
    description="Azure gépi tanulási Studio modulok használata szöveg előfeldolgozása, N – g vagy szolgáltatás kivonatolás analytics modellek szöveg létrehozása"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Azure gépi tanulási Studio szöveg analytics modellek létrehozása

Azure gépi tanulási összeállítása és szöveg analytics modellek üzemeltető is használhatja. Ezek a modellek segítséget, például dokumentum besorolás vagy sentiment analysis kapcsolatos problémák megoldása.

A szöveg analytics-kísérlet ahogyan általában:

 1. A minden és szöveg adatkészlet előfeldolgozása
 2. Bontsa ki a numerikus szolgáltatás módszereket előre feldolgozott szövegre
 3. Vonaton besorolás vagy a regressziós modell
 4. Pontszám és a modell érvényesítése
 5. Gyártási rendszerbe állítják a modellt

Ebben az oktatóanyagban, ebből a videóból megismerheti ezeket a lépéseket sentiment analysis-modellben Amazon könyv véleményezések adatkészlet végigvezetjük (lásd: a tanulmány "adatainak, Bollywood, Blenders és gémhez-vezérlőelem: tartomány kiigazítás Sentiment osztályozási" János Blitzer, megjelölés Dredze és Fernando Pereira; A társítási számítási nyelvi elemek feldolgozása (vezérlés), a 2007-es.) Ez az adatkészlet, ahol a Véleményezés statisztikákat? (1-2 vagy 4-5) és szabadkézi szöveget. A cél, a Véleményezés pontszám előrejelzésére: alacsony (1 - 2), illetve magastól (4, 5).

A Cortana üzletiintelligencia-gyűjtemény ebben az oktatóanyagban kísérletek találja meg:

[Előrejelzésére könyv véleményezések] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Előrejelzésére könyv véleményezések - prediktív kísérlet] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Lépés: 1: Tiszta és szöveg adatkészlet előfeldolgozása

Az első lépések a kísérlet a Véleményezés eredmények elosztjuk kockák alsó és felső időszakok, a problémát, mint két szintű besorolás megfogalmazásához be. [Szerkesztése metaadatok] (https://msdn.microsoft.com/library/azure/dn905986.aspx) használjuk és modulok [csoport kockák Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Címke létrehozása](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Ezután azt a szöveget, [előfeldolgozása szöveg] (https://msdn.microsoft.com/library/azure/mt762915.aspx) modulról tiszta. Tisztítására csökkenti a zajszint az adatkészlet, keresse meg a legfontosabb funkciók, és javíthatja a pontosságát, a végleges modell segítséget. Stopwords – gyakori szavakat, mint például a "a" vagy "a" - és számokat, speciális karakterek, ismétlődő karaktereket, e-mail címét és URL-címek eltávolítása. Azt is alakítsa a szöveget kisbetűssé, lemmatize a szavakat, és észleli, majd jelölt mondat határai "|||" szimbólum előtti feldolgozott szövegben.

![Szöveg előfeldolgozása](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Mi történik, ha használni kívánt stopwords az egyéni lista? A választható bemeneteként továbbíthatja. Is összefűzendő karakterláncokat cseréje egyéni C# szintaxis reguláris kifejezésekkel használatával, és eltávolítása a szavak rész a beszéd: nouns, műveletek vagy mellékneveket.

A előfeldolgozása befejeződése után azt vonaton ossza szét az adatokat, és tesztelje a beállítása.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Lépés: 2: Numerikus szolgáltatás módszereket kinyerése előre feldolgozott szöveg

Szöveg mellé modell összeállításához általában hogy alakú szöveg átalakítása numerikus szolgáltatás módszereket. Ebben a példában [kibontása N-nyelvtani lehetőségei a szövegből] használjuk (https://msdn.microsoft.com/library/azure/mt762916.aspx) az ilyen formátumú szöveg adatok átalakítása modult. Ez a modul elválasztó karakter tabulátorral tagolt szavainak oszlopot fogad, és kiszámít egy szótár szavak vagy N-g szavakat, amelyek az adatkészlet jelennek meg. Ezután megszámolja, hány alkalommal minden word- vagy N-nyelvtani, az egyes rekordok jelenik meg, és szolgáltatás felhasználásával hoz azoktól számolja meg. Ebben az oktatóanyagban azt méretezési N-nyelvtani 2, így a szolgáltatás módszereket tartalmazni, kattintással és a két egymást követő szó kombinációi.

![N-g kibontása](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Azt alkalmazni TF * N-nyelvtani súlyozást IDF (kifejezés gyakoriság inverz dokumentum gyakoriság) számolja meg. Ezt a megközelítést felveszi a szavakat, amelyek egyetlen olyan rekordot gyakran jelennek meg, de a teljes adatkészlet ritka mindenütt vastagságának. Egyéb lehetőségek bináris, TF, hozzáadni, majd graph mérés.

Ezek a funkciók a magas dimensionality gyakran van. Például a corpus 100 000 egyedi szavakat tartalmaz, ha a szolgáltatás tárhely szeretné, hogy a dimenziókat 100 000 vagy több N-g használatakor. A kinyerni N-nyelvtani szolgáltatások modul lehetővé teszi: beállításcsoport a dimensionality csökkentése érdekében. Megadhatja, és a kihagyni rövid vagy hosszú, vagy túl ritka vagy jelentős prediktív érték túl gyakori szavakat. Ebben az oktatóanyagban 5-nél kevesebb rekordokat, illetve a 80 %-nál több, a rekordok N g kizárhatja azt.

Is jelölje be a csak azokat a szolgáltatásokat, amelyek a legtöbb a szövegbevitel cél a korrelációs szolgáltatásainak kiválasztása is használhatja. Khi-négyzet szolgáltatásainak kiválasztása segítségével válassza ki 1000 funkciókat. A kijelölt szavak vagy N-g szószedet a megfelelő kimenet kivonat N-g modul kattintva tekintheti meg.

Egy alternatív megoldás kibontása N-nyelvtani funkcióival, mint a modul szolgáltatás kivonatolás is használhatja. Figyelje meg, ha a [szolgáltatás kivonatolás] (https://msdn.microsoft.com/library/azure/dn906018.aspx) nem rendelkezik beépített funkció kijelölési lehetőségeit vagy TF * IDF mérés.

## <a name="step-3-train-classification-or-regression-model"></a>Lépés 3: Besorolás vagy a regressziós modell képzése

A szöveg van alakultak numerikus szolgáltatás oszlopok. Az adatkészlet továbbra is tartalmazza az előző lépésben karakterlánc oszlopok, így azt segítségével oszlopok kiválasztása adathalmazban kizárás őket.

[Két szintű logisztikus regresszió] (https://msdn.microsoft.com/library/azure/dn905994.aspx) a cél előrejelzésére majd használjuk: sürgős vagy nem Véleményezés pontszámhoz. Ezen a ponton a szöveg analytics probléma van esett át egy normál besorolás a problémát. Az Azure gépi tanulási használható eszközök használatával javíthatja a modellt. Például kísérletezhet a különböző osztályozónak megtudhatja, hogy hogyan pontos eredményeket adnak, vagy használja a pontosságának növelése érdekében finombeállítása hyperparameter.

![Vonaton és pontszámhoz](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Lépés: 4: Pontszám, és a modell érvényesítése

Hogyan érvényesítése, a képzett modell? Hogy elleni vizsgálatot adathalmazban pontszám azt, és a pontosság felmérése. Azonban a modell megtanulta az N-g és azok súlyok át a képzési adatkészlet szószedet. Emiatt azt során kell használni, hogy a szótár és az adott súlyok szolgáltatások kinyerése tesztadatokat, nem pedig a új létrehozása a szószedet. Emiatt azt kibontása N-nyelvtani szolgáltatások modul felvétele a kísérlet a pontozási ág, a kimeneti szószedet beolvasása oktatás ág és szószedet mód beállításához csak olvasható. Azt is letiltása a szűrés N-g gyakoriság 1 példány, és a maximum 100 %-a minimális beállításával, és kapcsolja ki a szolgáltatás kiválasztása.

Tesztadatokat a szövegoszlop numerikus szolgáltatás oszlop van esett át, miután azt zárni a karakterlánc oszlopok oktatás ág például előző szakaszokban. Ezután használja pontszámhoz modell, hogy az előrejelzések és felmérése modell modul pontosságának ki szeretné számítani.

## <a name="step-5-deploy-the-model-to-production"></a>Lépés az 5: Termelési rendszerbe állítják a modellt

Termelési telepíthető szinte készen áll a a modellt. Rendszerbe, webes szolgáltatás, amikor megnyitja a szabadkézi karaktersorozat bemeneteként, és vissza előrejelzése "sürgős" vagy "nem". Az ismert N-nyelvtani tanuláskor fontos funkciókat és képzett logisztikus regresszió modell, hogy ezek a funkciók a előrejelzése a szöveg átalakítása használ. 

A cserélendő kísérlet beállítani, azt először menti a N-nyelvtani szószedet adatkészlet, és a képzett logisztikus regresszió modell át a képzési ág a kísérlet. Hogy mentse a kísérlet a "Mentés másként" prediktív kísérlet egy kísérlet graph létrehozásához. A szétválasztott adatok modult, és a oktatás ág azt eltávolítása a kísérlet. Azt csatlakozhat a korábban mentett N-nyelvtani szótár és modell N-nyelvtani szolgáltatások kibontása és pontszámhoz modell modulokat, illetve. Azt is eltávolíthatja a modell felmérése modulra.

Azt oszlopok kiválasztása beszúrása adatkészlet modulban előfeldolgozása szöveg modult a címke oszlop eltávolítása előtt, és kijelölésének megszüntetése pontszámhoz modulban "Adatkészlet hozzáfűző pontszámhoz oszlop" lehetőséget. Úgy, hogy a webszolgáltatás nem kéri a címke próbál előrejelzésére, és hatással van a bemeneti szolgáltatásait, a válasz nem a visszhang.

![Prediktív kísérlet](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Most már van egy kísérlet közzétett webes szolgáltatásként, nevű kérés-válasz vagy a köteg végrehajtás API-k használata.

## <a name="next-steps"></a>Következő lépések

Tudnivalók a szöveg analytics modulok [MSDN dokumentáció] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
