<properties
    pageTitle="Lépés: 4: Képzése és a prediktív analitikus modellek felmérése |} Microsoft Azure"
    description="Lépés: 4, az elektronikus oktatóanyagok elkészítése prediktív megoldás Útmutató: vonaton, pontszám, és az Azure gépi tanulási Studio több levő felmérése."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Áttekintése a következő lépés: 4: Képzése és a prediktív analitikus modellek felmérése

Ez a témakör az útmutató, [az Azure gépi tanulási prediktív analytics megoldást fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md) negyedik lépése


1.  [Gépi tanulási munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Töltse fel a meglévő adatok](machine-learning-walkthrough-2-upload-data.md)
3.  [Hozzon létre egy új kísérlet](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Képzése és a modellek felmérése**
5.  [A webszolgáltatás terjesztése](machine-learning-walkthrough-5-publish-web-service.md)
6.  [A webszolgáltatás elérésére](machine-learning-walkthrough-6-access-web-service.md)

----------

Gépi tanulási modellek létrehozása az Azure gépi tanulási Studio használatának előnyeiről egyik próbálja meg a modell egynél több típusú kísérlet egyesével, és összehasonlíthatók az eredmények. Kísérletezés a következő típusú segít, hogy a legjobb megoldás keresse meg a problémát.

A kísérlet azt az útmutató fejleszt azt fogja két különböző típusú modellek létrehozása, és hasonlítsa pontozási eredményeit döntse el használni a saját végleges kísérlet szeretnénk algoritmus.  

Vannak olyan különféle modellek azt is választhat. A rendelkezésre álló modellek megtekintéséhez bontsa ki a modul paletta **Gépi tanulási** csomópontot, és bontsa ki a **Modell inicializálni** és a csomópontok alatta. A kísérlet az alkalmazásában azt választja ki a támogatási vektoros gépi (SVM) a második szintű csillapítja döntési fák modulokat.    

> [AZURE.TIP] A Súgó eldöntése a legjobban gépi tanulási algoritmus leginkább megfelelő az adott probléma megoldása szeretné, elolvashatja, [hogyan választhatja ki a Microsoft Azure gépi tanulási algoritmusok](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>A modellek képzése
Először nézzük beállítása a csillapítja döntési fa modell:  

1.  Keresse meg a [Két szintű csillapítja döntési fa] [ two-class-boosted-decision-tree] a modul paletta modult, és húzza az alakzatot a vásznat.
2.  Keresse meg a [Modell vonaton] [ train-model] modul, húzza az alakzatot a vászonra, és csatlakoztassa a kimenet a csillapítja döntési fa modul a bal oldali beviteli porthoz ("kellő modell") a [Vonaton modell] [ train-model] modulra.
    
    A [Második szintű csillapítja döntési fa] [ two-class-boosted-decision-tree] modul előkészíti a általános modell, és [Láthatja el ismeretekkel a modell] [ train-model] képzés az adatokat használja a modell képzése. 
     
3.  Csatlakozás a bal oldali eredménye ("eredményt adatkészlet"), a bal oldali [R parancsfájl végrehajtása] [ execute-r-script] jobbra modul bemeneti port ("adatkészlet") a [Vonaton modell] [ train-model] modulra.

    > [AZURE.TIP] Két a ráfordítások és a kimeneti értékeket az [R parancsfájl végrehajtása] közül nem szükséges[ execute-r-script] e kísérletből, így azt meghagyása eredeti leválasztott modul. 

4.  Jelölje ki a [Modell vonaton] [ train-model] modulra. A **Tulajdonságok** ablaktáblában kattintson az **Indítás oszlopkijelölő**gombra, válassza **Diagramtípusokat** a legördülő listában a **Megjeleníthető oszlopok** lehetőséget, és adja meg a "Kockázat követel" a szöveg mezőbe. Jelölje ki a legördülő menü a **Kijelölt oszlopok** **Diagramtípusokat** . Jelölje be a "Követel kockázat" és a kiemelt nyíl gombra kattintva helyezze át a **Kijelölt oszlopokat**. 
5.  Kattintson a **Mentés**gombra.


Ez a kísérlet részét most néz ki:  

![Tanfolyamok a modell][1]

Ezután a SVM modell beállítani.  

Először is, SVM egy kis ismertetése. Csillapítja döntési fák jól bármilyen típusú szolgáltatások használata. Azonban a SVM modul lineáris besorolás hoz létre, mivel a modell létrehozott van a legjobb próba hiba ugyanazt a skálát esetén minden numerikus funkció. Az összes numerikus szolgáltatások ugyanazt a skálát alakításához "Tanh" átalakítás használjuk (az [Adatok normalizálása] [ normalize-data] modul.) A [0,1] tartományba a számok Ez alakítja. Karakterlánc szolgáltatások alakulnak a SVM modul kockák funkcióinak és bináris 0 és 1 funkcióinak, ezért nincs szükség kézi átalakítása karakterlánc funkciók. Is hogy nem szeretné, hogy a hitelkártya kockázatok (oszlop 21) oszlopot fogja átalakítani - numerikus, de azt esetén tanfolyamok a modellt, előre, így szükség önállóan hagyja értéke.  

A SVM modell beállításához tegye a következőket:

1.  Keresse meg a [Két szintű támogatási vektoros gépi] [ two-class-support-vector-machine] a modul paletta modult, és húzza az alakzatot a vászonra.
2.  Kattintson a jobb gombbal a [Vonaton modell] [ train-model] modul, válassza a **Másolás**, kattintson a jobb gombbal a vászonra és válassza a **Beillesztés parancsot**. A [Vonaton modell] azon példányára[ train-model] modul rendelkezik az eredeti ugyanazon oszlop beállítást.
3.  A kimenet a SVM modul csatlakoztatása a bal oldali beviteli port ("kellő modell") a második [Vonaton modell] [ train-model] modulra.
4.  Keresse meg az [Adatok normalizálása] [ normalize-data] modult, és húzza az alakzatot a vásznat.
5.  A bemeneti e modul csatlakoztatása a bal oldali eredménye a bal oldali [R parancsfájl végrehajtása] [ execute-r-script] modul (figyelje meg, hogy a modul a kimeneti port egynél több más modul lehet, hogy csatlakoztatva).
6.  Csatlakozás a bal oldali kimeneti port ("transzformált adatkészlet") az [Adatok normalizálása] [ normalize-data] jobbra modul bemeneti port ("adatkészlet") a második [Vonaton modell] [ train-model] modulra.
7.  A **Tulajdonságok** ablaktáblában az [Adatok normalizálása] [ normalize-data] modulban választó **Tanh** **átalakítása módszer** paraméter.
8.  Kattintson az **Indítás oszlopkijelölő**gombra, "Nincs oszlop" kiválasztása **Kezdődő**, jelölje be a **Felvétel** a az első legördülő menü, jelölje ki a második legördülő menü **az oszlopok** és jelölje ki a **numerikus** a harmadik legördülő. Az összes a számokat tartalmazó oszlop (és csak numerikus) transzformált jelöli ki.
9.  Kattintson a pluszjelre (+) a sor jobb – Ezzel létrehoz egy legördülő lista sorában. Jelölje be a **kizárandó** az első legördülő, **oszlopnevek** jelölje ki a második tartalmazó legördülő listára, és kattintson a szövegmezőbe, és válassza a "Kockázat követel" és az oszlopok listából. Azt adja meg, hogy a hitelkártya kockázat oszlop figyelmen kívül hagyja (szükséges művelet, mert ez az oszlop nem numerikus, és így ellenkező esetben transzformált).
10. Kattintson az **OK gombra**.  


Az [Adatok normalizálása] [ normalize-data] modul most már készen végre szeretne hajtani Tanh átalakítás a hitelkártya kockázat oszlop kivételével az összes számokat tartalmazó oszlop.  

Ez a kísérlet részét most hasonlóan kell kinéznie a következőket:  

![A második modell tanfolyamok][2]  

##<a name="score-and-evaluate-the-models"></a>Pontszám és a modellek felmérése

Használjuk, amely lett elválasztva a [Szétválasztott adatok] az tesztelés adatait[ split] modult a képzett modellek pontszám. Azt is hasonlítsa össze az eredmények megtekintéséhez, amelyeket létrehozott, jobb eredmények két modellek.  

1.  Keresse meg a [Modell pontszámhoz] [ score-model] modult, és húzza az alakzatot a vásznat.
2.  Csatlakozás a [Vonaton modell] [ train-model] modul, amely a [Két szintű csillapítja döntési fa] csatlakozik[ two-class-boosted-decision-tree] balra modul beviteli a [Modell pontszámhoz] port[ score-model] modulra.
3.  Csatlakozás a [Pontszámhoz modell] jobb beviteli port[ score-model] a megfelelő [R parancsfájl végrehajtása] a bal oldali kimenet modul[ execute-r-script] modulra.

    A [Modell pontszámhoz] [ score-model] modul is most készítése a tesztelés adatok hitelkártya adatait, futtassa a modell között, és hasonlítsa össze az előrejelzések a modell a tesztelés adatok tényleges hitelképesség kockázat oszlopot hoz létre.

4.  Másolás és beillesztés a [Pontszámhoz modell] [ score-model] modul második másolatot, vagy új modul húzza az alakzatot a vásznat.
5.  A bal oldali beviteli port e modul csatlakoztatása a SVM modell (Ez azt jelenti, hogy a kimeneti port [Vonaton modell] csatlakozás[ train-model] modul, amely a [Két szintű támogatási vektoros számítógép] csatlakoztatva van[ two-class-support-vector-machine] modul).
6.  A SVM modell felkínálunk végezze el a próba-adatok ugyanazt az átalakítást, mint azt az oktatóanyag adatok. Így másolja és illessze be az [Adatok normalizálása] [ normalize-data] második másolatot, és csatlakoztassa a megfelelő [R parancsfájl végrehajtása] a bal oldali kimenet modul[ execute-r-script] modulra.
7.  Csatlakozás a [Pontszámhoz modell] jobb beviteli port[ score-model] az [Adatok normalizálása] a bal oldali kimenet modul[ normalize-data] modulra.  

Egy [Felmérése modell] használjuk a két pontozási eredmény értékelni,[ evaluate-model] modulra.  

1.  Keresse meg a [Modell felmérése] [ evaluate-model] modult, és húzza az alakzatot a vásznat.
2.  A bal oldali beviteli port csatlakoztatása a kimeneti port [Pontszámhoz modell] [ score-model] a csillapítja döntési fa modell társított modulra.
3.  A megfelelő bemeneti port kapcsolódni a másik [Pontszámhoz modell] [ score-model] modulra.  

A kísérlet futtatásához kattintson a **Futtatás** gombra a vászonra alatt. Eltarthat néhány percig. Egy jeleníti meg, hogy fut-e, majd a zöld pipa jelzi a modul befejezéskor minden modulban frissítésjelző jelölő. Ha modulokat be van jelölve, a kísérlet futása befejeződött.

A kísérlet kell ekkor valami hasonló látható:  

![Mindkét modellek értékelése][3]

Ellenőrizze az eredményt, kattintson a kimeneti port a [Modell felmérése] [ evaluate-model] modult, és válassza a **Megjelenítés**.  

A [Kiértékelendő modell] [ evaluate-model] modul ívek és mértékek, amelyek lehetővé teszik összehasonlíthatók az eredmények két scored modellek hoz létre. Az eredmények címzett operátor jellemző (ROC) görbék, pontosság/visszahívási görbék, vagy felvonó tekinthet meg. További adatot láthatóvá tenni, fejetlenséget mátrix, a görbe (AUC), és más mértékek területen összesített értékeket tartalmazza. A küszöbértéket módosításához húzza a csúszkát jobbra vagy balra, és látni, hogyan hatással van a mértékek készlete.  

Jobb oldalán a diagramot kattintson a **Scored adatkészlet** vagy **Scored adatkészlet összehasonlítása** , jelölje ki a társított görbe és a kapcsolódó mérési módja miatt az alábbi megjelenítéséhez. A görbék jelmagyarázatban "Pontszáma az adatkészlet" felel meg a [Modell logikai értéket] a bal oldali beviteli port[ evaluate-model] modul - ebben az esetben ez az a csillapítja döntési fa modell. "Pontszáma az adatkészlet összehasonlítása" felel meg a megfelelő beviteli port - abban az esetben a SVM modell. Ezek a címkék kattintáskor a modell görbét ki van emelve, és a megfelelő mérési módja miatt megjelenítéséhez, az alábbi ábrán látható módon.  

![Az adatmodellek ROC görbék][4]

Ezek az értékek vizsgálatakor eldöntheti, mely a modell a kívánt eredményt adja, amit keres a legközelebb esik. Térjen vissza, és a kísérlet a különböző modellek értékek módosításával találta. 

> [AZURE.TIP] Minden futtatásakor a kísérlet egy rekordot, hogy közelítését futtatása előzmények legyen. Ezek a közelítés megtekintése és a vászonra alatti **Futtatása ELŐZMÉNYEINEK megtekintése** gombra kattintva visszatérhet bármelyiküket. Választhatja **Előzetes futtassa** a **Tulajdonságok** ablaktáblában való visszatéréshez az azt közvetlenül megelőző közelítését egy meg van nyitva.
> 
Bármely, a kísérlet közelítését másolatát a vászonra alatt a **MENTÉS Másként** gombra kattintva teheti meg. A kísérlet **összefoglalása** és a **Leírás** tulajdonságok használatával nyomon követése a kísérlet iterációk száma a korábban elvégzett.

>  További részletekért olvassa el [kezelése kísérletezhet Azure gépi tanulási Studio iterációk száma](machine-learning-manage-experiment-iterations.md).  


----------

**[A webszolgáltatás Deploy](machine-learning-walkthrough-5-publish-web-service.md) tovább:**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
