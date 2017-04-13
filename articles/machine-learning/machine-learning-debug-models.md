<properties 
    pageTitle="A modell az Azure gépi tanulási hibakeresési |} Microsoft Azure" 
    description="Megtudhatja, hogyan szeretné hibakeresése a modell az Azure gépi tanulási hogyan." 
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
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>A modell az Azure gépi tanulási hibakeresése

Ez a cikk ismerteti, hogyan szeretné hibakeresése a Microsoft Azure gépi tanulási modellekben. Pontosabban lefedi a lehetséges okok miért vagy az alábbi két hiba jelenik meg, előfordulhat, hogy előforduló modell fut:

* a [Modell vonaton] [ train-model] modul hibát okoz 
* a [Modell pontszámhoz] [ score-model] modul helytelen eredményt ad 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Vonaton modell modul hibát okoz

![image1](./media/machine-learning-debug-models/train_model-1.png)

A [Modell vonaton] [ train-model] modul vár 2 bemeneti adatok alapján:

1. Milyen típusú besorolás/regressziós modell Azure gépi tanulási által biztosított modellek gyűjteményből
2. Egy adott címke oszloppal oktatóanyag adatok. A címke oszlopban adja meg a változó előrejelzésére. A beépített oszlopok a többi szolgáltatások vannak számol.

Ez a modul hibát okoz, az alábbi esetekben:

1. A címke oszlop nem megfelelően van megadva, mert egynél több oszlop ki van jelölve, a címke vagy egy helytelen oszlopindex ki van jelölve. Például a a második helyzet szeretné alkalmazni, ha egy oszlopindex 30-beviteli adatkészlet, amelyeket csak 25 oszlopok kellett volt használva.

2. Az adatkészlet tartalmaz olyan funkciót oszlopokat. Például ha a bemeneti adatkészlet címke oszlopként be van jelölve, csak 1 oszlopban nincs lenne, amellyel össze a modellben nincsenek funkciók. Ebben az esetben a [Vonaton modell] [ train-model] modul hibaüzenetet fog küldjön.

3. A beviteli adatkészlet (szolgáltatások vagy címke) egy érték végtelenig lehetnek.


## <a name="score-model-module-does-not-produce-correct-results"></a>Pontszámhoz modell modul nem eredményt megfelelő

![image2](./media/machine-learning-debug-models/train_test-2.png)

Egy tipikus oktatás/tesztelése graph felügyelt tanulási, a [Szétválasztott adatok] [ split] modul az eredeti adatkészlet elosztja a két részre: a, amely a modell képzése és részét pontszám mennyire a képzett modell végez adatokat, hogy lefoglalt did nem láthatja el ismeretekkel a. A képzett modell használják majd pontszám a tesztadatokat, amely után az eredmények kiértékelt határozza meg a modell pontosságát.

A [Modell pontszámhoz] [ score-model] modul két bemeneti adatok alapján van szükség:

1. Egy képzett modell kimenet [Vonaton] modellből[ train-model] modul
2. A pontozási adatkészlet, amely a modell nem lett képzett a nem

Előfordulhat, hogy fordul elő, akkor is, ha a kísérlet létrejött, az [Eredmény modell] [ score-model] modul helytelen eredményt ad. Különböző okokból jelenhet meg, hogy ez:

1. Ha a megadott címke kockák, és a regressziós modell van képzett az adatok, egy nem megfelelő eredményt ad a [Pontszámhoz modell] lehetne-e előállítani[ score-model] modulra. Ennek az oka regressziós folyamatos válasz változó van szükség. Ebben az esetben kell alkalmasabb besorolás modell használni. 
2. Hasonlóképpen osztályozási modell egy adatkészlet rendelkező pont számok lebegő címke oszlopban lévő van képzett, ha azt is eredményt nemkívánatos. Ennek oka az, osztályozási csak lehetővé teszi, hogy értékek tartomány egy függvényt, és általában némileg kis csoporthalmaz osztályok különálló válasz változó van szükség.
3. Ha a pontozási adatkészlet nem tartalmaz a modell, az [Eredmény modell] képzése használt összes funkcióját[ score-model] hibát jelez.
4. A [Modell pontszámhoz] [ score-model] bármely kimeneti az pontozási adathalmazban hiányzó értéket vagy a szolgáltatások egy végtelen értékét tartalmazó sor megfelelő módon hozhatók létre.
5. A [Modell pontszámhoz] [ score-model] alakulhat ki azonos kimeneti értékeket a pontozási adatkészlet az összes sorát. Ez akkor fordulhat, például ha kísérel meg besorolás döntés erdők használata, ha a lehető legkevesebb minta / levél csomópont választja ki kell több, mint a rendelkezésre álló tanfolyamok példák számát.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
