<properties
    pageTitle="Egy gépi tanulási oktatás kísérlet átalakítása egy prediktív kísérlet |} Microsoft Azure"
    description="Hogyan használható a cserélendő analytics modellt, egy prediktív kísérlet, amelyek másként webszolgáltatás telepíthető képzése egy gépi tanulási oktatás kísérlet."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="garye"/>

# <a name="convert-a-machine-learning-training-experiment-to-a-predictive-experiment"></a>A cserélendő kísérlet egy gépi tanulási oktatás kísérlet átalakítása

Azure gépi tanulási lehetővé teszi, hogy a összeállítása, tesztelése és az előrejelzési analytics-megoldások telepítése.

Ha már létrehozott és a egy *oktatás kísérletezhet* betanítása a cserélendő analytics modell többször is, és készen áll új adatok pontszám használatával, kell előkészítése és a kísérlet az pontozási egyszerűbb. Majd telepítheti a *cserélendő kísérlet* Azure webes szolgáltatásként, hogy a felhasználók adatokat küld a modell és a modell előrejelzések fogadhatnak.

A cserélendő kísérlet való alakításával, amelyről a képzett modell készen áll a webes szolgáltatásként telepíthető. A webszolgáltatás felhasználók bemeneti adatokat küld a modell, és a modell vissza az előrejelzési eredmények küld. Igen, akkor átalakíthatja a cserélendő kísérlet kívánt Felhívjuk a figyelmét arra, hogyan várt a mások által használt modell.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

A folyamat egy oktatás kísérlet konvertálása egy prediktív kísérlet három lépésből áll:

1.  Mentse a gépi tanulási modellt, amely a korábban a képzett, majd cserélje le a gépi tanulási algoritmus és [Vonaton modell] [ train-model] modulok és a mentett képzett modellt.
2.  Csak a szükségesek az pontozási modulokat a kísérlet vágása. Egy oktatás kísérlet számos modulok szükség – oktatás, de nem szükségesek, amikor elkészült a modell képzett és pontozási használni.
3.  Adja meg, ahol a kísérlet az fogad a webes szolgáltatás felhasználó adatait, és a függvény milyen adatokat ad vissza.

## <a name="set-up-web-service-button"></a>Webszolgáltatás beállítása gomb

A kísérlet (**Futtatás** gomb a képernyő alján a kísérlet vászon) futtatása után a **Webszolgáltatás beállítása** gombra (jelölje ki a **Cserélendő webszolgáltatás** beállítás) meg elvégez egy prediktív kísérlet a oktatás kísérlet konvertálása három lépését:

1.  Azt menti a képzett modell (a bal oldalon a kísérlet vászon), a modul paletta **Képzett modellek** részében modul felülírja a gépi tanulási algoritmus és [Vonaton modell] [ train-model] modulok és a mentett képzett modellt.
2.  Eltávolítja a modulokat, amelyek egyértelműen nincs szükség. Ebben a példában, a [Szétválasztott adatok]ideértendők[split], 2nd [Pontszámhoz modell][score-model], és a [Modell felmérése] [ evaluate-model] modulokat.
3.  Azt létrehozza Web service beviteli kimeneti modulok és őket a kísérlet az alapértelmezett mappákban.

Ha például a következő kísérlet betanítja két szintű csillapítja döntési fa-modellben népszámlálás mintaadatok:

![Oktatás kísérlet][figure1]

A kísérlet a modulokat alapvetően négy különböző függvények hajtsa végre:

![A modul függvények][figure2]

E oktatás kísérletből egy prediktív kísérlet alakításakor ezeket a modulokat részét már nincs rá szükség, vagy egy másik célra rendelkeznek:

- **Adatok** – Ez a példa adatkészlet adatainak pontozási során nem használatos – a felhasználó a webszolgáltatás az adatok kiértékelhető fogja szolgáltatni. A metaadat-alapú e adatkészlet adattípusokat, például a azonban a képzett modellt használja. Így kell az adatkészlet tartani, hogy a metaadatok ezáltal a cserélendő kísérlet.

- Ezeket a modulokat **prep** – attól függően, hogy az adatokat a pontozási, küldött előfordulhat, hogy, vagy nem feltétlenül szükséges, a bejövő adatok feldolgozása.

    Például ez a példa a minta adatkészlet lehetnek a hiányzó értékeket, és oszlopokat, amelyeket nem szükséges a modell képzése tartalmazza. Így [Tiszta hiányzó adatok] [ clean-missing-data] modul tartalmazta üzletet a hiányzó értékeket, és egy [Oszlop kijelölése az adatkészlet] [ select-columns] modul tartalmazta további oszlopok kizárása az adatfolyam. Ha tudja, hogy az adatok esetében a webszolgáltatás keresztül pontozási küldött nem lesz a hiányzó értékeket, majd a [Hiányzó adatok karbantartása] eltávolíthatja[ clean-missing-data] modulra. Az [Oszlopok kiválasztása az adatkészlet] mivel azonban[ select-columns] modul segítségével definiálhatja pontszáma alatt áll, a modul van szüksége.

- **Vonaton** - után a modell sikeresen képzett, menti azt egy egyetlen képzett modell modulra. Majd cserélje ezeket az egyes modulokat a mentett képzett modell.

- **Pontszámhoz** - ebben a példában a felosztott modul segítségével a adatfolyam osztása tesztadatokat és oktatási adatokat. A cserélendő kísérlet ez nem szükséges, és el kell távolítani. Hasonlóképpen a 2nd [Pontszámhoz modell] [ score-model] modul és a [Modell felmérése] [ evaluate-model] modul segítségével összehasonlítása a tesztadatokat származó találatok jelenjenek meg, ezért ezeket a modulokat még nincs szükség a cserélendő kísérlet a. A hátralévő [Pontszámhoz modell] [ score-model] modul, azonban van szükség a webes szolgáltatáson keresztül pontszámhoz eredményezhet.

A következő példában szereplőhöz megjelenésével **Webszolgáltatás beállítása**parancsra kattintást követően:

![Átalakított prediktív kísérlet][figure3]

Ez a kísérlet webszolgáltatás telepítendő előkészítése elegendő lehet. Azonban érdemes néhány további munkavégzés a kísérlet az adott.

### <a name="adjust-input-and-output-modules"></a>Módosítsa a bemeneti és kimeneti modulok

A tanfolyam kísérlet használt oktatóanyag adatok csoportja, és az adatok beolvasása egy űrlap, amely a gépi tanulási algoritmus szükséges az egyes feldolgozási majd művelet. Ha az adatokat a webes szolgáltatáson keresztül fogadásához várt a feldolgozása nem szükséges, a kísérlet az áthelyezheti a **webes szolgáltatás beviteli modul** egy másik csomópontot.

Alapértelmezés szerint például a **Webes szolgáltatás beállítása** helyezi a **Web service beviteli** modul tetején látható a adatfolyam, ahogy a fenti ábrán. Jó helyen jár a bemeneti adatok nem kell a feldolgozás, ha majd kézzel is elhelyezheti a **Web service beviteli** múltbeli adatfeldolgozás modulokat:

![A web service bemeneti áthelyezése][figure4]

A bemeneti adatok szolgáltatáson keresztül a webszolgáltatás most továbbítja közvetlenül a pontszámhoz modell modulba bármely előfeldolgozása nélkül.

Alapértelmezés szerint hasonlóképpen, **Webes szolgáltatás beállítása** a webes szolgáltatás kimeneti modul a adatfolyam alján helyezi. Ebben a példában a webszolgáltatás ad vissza a felhasználónak a kimenet [Pontszámhoz modell] [ score-model] modul, amely magában foglalja a teljes bemeneti adatok vektoros, valamint a pontozási eredménye.

Jó helyen jár, ha inkább való visszatéréshez valami más – például, csak a pontozási eredményeit, és nem bemeneti adatok –, majd a teljes vektoros beszúrhat egy [Oszlop kijelölése az adatkészlet] [ select-columns] pontozási eredmények kivételével az összes oszlopok kizárása modulra. Majd áthelyezése a **webes szolgáltatás kimeneti** modul a kimenet az [Oszlopok kiválasztása az adatkészlet] [ select-columns] modul:

![A web service kimenet áthelyezése][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a>Hozzáadása vagy eltávolítása a további adatfeldolgozás modulok

Ha tudja, pontozási során nem lehet szükség a kísérlet további modulokat, ezek eltávolíthatók. Ha például azt át lett helyezve a **Web service beviteli** modul pontjához után az adatkezelési modulokat, mert azt is távolítsa el a [Tiszta hiányzó adatok] [ clean-missing-data] a cserélendő kísérlet modul.

A cserélendő kísérlet most néz ki:

![További modul eltávolítása][figure6]

### <a name="add-optional-web-service-parameters"></a>Nem kötelező, webes szolgáltatás paraméterek

Egyes esetekben előfordulhat, hogy engedélyezni szeretné a felhasználó a webszolgáltatás modulok működésének módosításához, ha a érhető el a szolgáltatás. *Webes szolgáltatás paraméterei* ehhez teszi lehetővé.

Gyakori példa beállítja az [Adatok importálása] [ import-data] modul, hogy a felhasználó a telepített webszolgáltatás lehetőségeiről másik adatforrást a webszolgáltatás érhető el. És konfigurálását az [Adatok exportálása] [ export-data] modul, hogy egy másik helyre adható meg.

Webes szolgáltatás paramétereit határozza meg, és társítása őket egy vagy több modul paraméterei, és megadhatja, hogy kötelező és választható. A felhasználó a webszolgáltatás is majd adjon meg értéket paraméterértékek érhető el a szolgáltatás és a modul műveletek megfelelően módosul.

Webes szolgáltatás paraméterei kapcsolatos további tudnivalókért lásd a [Használatával Azure gépi tanulási Web Service paraméterek ] [ webserviceparameters].

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a>A cserélendő kísérlet üzembe webes szolgáltatásként

Most, hogy a cserélendő kísérlet elég készítve, mint az Azure webszolgáltatás telepítheti. A webszolgáltatással felhasználó adatait a modell küldhet, és a modell az előrejelzések ad eredményül.

A teljes telepítési folyamatot kapcsolatos további tudnivalókért olvassa el a [Deploy az Azure gépi tanulási webszolgáltatás] című témakört.[deploy]

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
