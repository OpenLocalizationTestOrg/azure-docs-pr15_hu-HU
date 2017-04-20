<properties
    pageTitle="Válassza ki a az Azure gépi tanulási algoritmusok optimalizálása paramétereket |}] A Microsoft Azure"
    description="Válassza ki az Azure gép tanuló algoritmus beállítása az optimális paraméter ismerteti."
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
    ms.author="bradsev" />


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Válassza ki a paramétereket a az Azure gépi tanulási algoritmusok optimalizálása

Ez a témakör ismerteti, hogyan válassza ki a jobb oldali hyperparameter az Azure gép tanuló algoritmus beállítása. A legtöbb gépi tanulás algoritmusokat kell paramétereinek beállítása. Ha egy modell betanításához kell értékeket adja meg azokat a paramétereket. A képzett modell hatékonyságát függ a választott modell-paraméterek. A paraméterek optimális beállítása folyamata néven ismert *modell kiválasztása*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Különböző módon modell kiválasztása. A gépi tanulás, kereszt-ellenőrzési modell kiválasztása a legszélesebb körben használt módszerek egyike, és az alapértelmezett modell kijelölési mechanizmus Azure gép elsajátításához. Azure gép tanulás támogatja az R és a Python, mert mindig valósíthat meg saját modell a kiválasztási mechanizmusokat R vagy Python segítségével.

A legjobb paraméterkészlet keresése során négy lépésből áll:

1.  **A paraméter hely megadása**: algoritmus, először el kell dönteni a pontos paraméterértékeket, érdemes.
2.  **Meghatározása a kereszt-ellenőrzési beállítások**: eldöntheti, hogyan válassza az adatkészlet kereszt-ellenőrzés többi hajtogatási vonalon is.
3.  **A mérőszám meghatározása**: milyen metrikus meghatározására a legjobb beállított paraméterek, például a pontosság használata mellett dönt, a legfelső szintű átlagos hiba, pontosság, visszahívás vagy f-score négyzet.
4.  **Vonat, értékeli, és hasonlítsa össze**: a paraméterértékeket minden egyedi kombinációjának kereszt-ellenőrzési által végzett és a hiba mérőszám definiálhat alapján. Értékelésének és összevetésének után válassza ki a legjobban teljesítményű modell.

Az alábbi kép mutatja, hogyan ez érhető el, a gép tanulás Azure mutatja be.

![Keresse meg a legjobb paraméterkészlet](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>A paraméter terület meghatározása
A paraméter beállítása a modell inicializálási lépésnél adhatja meg. Minden gépi tanulási algoritmusok paraméter oldalán oktatását üzemmóddal rendelkezik: *Egyetlen paraméter* és a *Paraméter*. Válassza ki a paraméter tartomány módban. Paraméter-tartomány módban minden paraméterhez több értékeket adhat meg. Vesszővel tagolt értékeket adhatja a szövegmezőben.

![Két osztály csillapítja döntési fa, egyetlen paramétert](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Felváltva határozhatja meg a maximum és minimum pontok a rács és **használata tartomány**létrehozása a létrehozandó pontok száma. Alapértelmezés szerint a lineáris skálán a paraméterértékeket jönnek létre. De a **Logaritmikus skála** be van jelölve, ha az értékek jönnek létre a Logaritmikus skála (azaz arány a szomszédos pontok, a különbség nem állandó). Egész Paraméterek meghatározhat egy kötőjellel. Például "1-10" azt jelenti, hogy minden egész szám között 1 és 10 (a határokat is beleértve) alkotják a paraméter beállítása. A vegyes üzemmód is támogatott. Például a paraméter beállítása "1-10, 20, 50" magában foglalja a egész számok 1-10, 20 és 50.

![Két osztály csillapítja döntési fa, paraméter-tartomány](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Kereszt-ellenőrzési éleket meghatározása
A [partíció és a minta] [ partition-and-sample] modul segítségével éleket véletlenszerűen hozzárendelése az adatokat. A következő minta konfigurációját a modul azt meghatározása öt éleket és véletlenszerűen hajtás számot rendel a minta példányok.

![Partíció és a minta](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>A mérőszám meghatározása
A [Hangolás modell Hyperparameters] [ tune-model-hyperparameters] modul támogatja az empirically a legjobb halmaza egy adott algoritmus és a dataset paramétereinek kiválasztása. Egyéb információk mellett meghatározására a legjobb paraméterkészletet a mérőszám vonatkozó képzési modell, a **Tulajdonságok** ablaktábla e modul tartalmazza. Két másik legördülő lista jelölőnégyzeteiből, osztályozása és a regressziós algoritmusok, illetve rendelkezik. Ha a szóban forgó algoritmus a besorolási algoritmus, a regressziós metrikus rendszer figyelmen kívül hagyja, és fordítva. Ebben a példában a mérőszám **pontosság**.   

![Sweep paraméterek](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>A vonat, értékelése és összehasonlítása  
Az azonos [Finomhangolása modell Hyperparameters] [ tune-model-hyperparameters] modul betanítja a modellek, amelyek megfelelnek a paraméterkészletet, különböző mérőszámok értékeli, és létrehozza a legjobban képzett modell alapján úgy dönt, a metrika. Ennek a modulnak két kötelező bemenet:

* A kellő tanuló
* A DataSet adatkészlet

A modul is rendelkezik egy választható bemenő adathalmaz. A DataSet adatkészlet kapcsolódnak a dataset kötelező bemeneti adatokat hajtás. Ha az adathalmazban nincs hozzárendelve minden hajtás információt, majd 10-szeres kereszt-ellenőrzési automatikusan végrehajtott alapértelmezés szerint. Ellenőrzési dataset biztosított választható dataset kikötő és a hajtás hozzárendelés nem történik meg, ha egy vonat-vizsgálati módot választja, és a vonat a modell minden egyes paraméter kombináció szolgál az első adathalmaz.

![A határozat csillapítja fa osztályozó](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

A modell az adathalmazban érvényesítési majd értékelik. A bal oldali kimeneti port a modul különböző mérőszámok, a paraméterértékek funkciók mutatja. A jobb kimeneti port biztosít a képzett modell, amely megfelel a legjobban teljesítményű modell szerint a kiválasztott mérőszám (**pontosság** ebben az esetben).  

![A dataset érvényesítése](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Megtekintheti a pontos paraméterek választja ki a megfelelő kimeneti port megjelenítésére. Ez a modell használható pontozási vizsgált meg, vagy az operationalized webszolgáltatás képzett modellként mentés után.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
