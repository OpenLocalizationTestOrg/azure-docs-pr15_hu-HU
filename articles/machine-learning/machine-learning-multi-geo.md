<properties
   pageTitle="Több-Geo súgó dokumentáció |} Microsoft Azure"
   description="Megtudhatja, hogy miként munkaterület létrehozása és közzététele a webszolgáltatás különböző a déli központi Amerikai Egyesült Államok (SCUS) az Azure területen Azure régió."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Több-Geo súgó dokumentáció

Ez a cikk ismerteti, hogyan munkaterület létrehozása és közzététele webszolgáltatás más Azure régiókban.

## <a name="create-a-workspace"></a>Munkaterület létrehozása

1. Jelentkezzen be az Azure klasszikus portálra.

2.  Kattintson a **+ Új** > **DATA SERVICES** > **gépi TANULÁSI** > **gyors létrehozása**.  **A hely** csoportban jelölje be a másik területére, például **Délkelet-ázsiai**.
![Több-Geo súgó kép 1][1]
3. Jelölje ki a munkaterület, és válassza a **bejelentkezési kísérleteket Machine Learning Studio**.
![Több-Geo súgó kép 2][2]

4. Ekkor a munkaterület bármely más munkaterület hasonlóan felhasználhatja egy másik régióban. Váltás a munkaterületek között, keresse meg a képernyő jobb felső sarkában. Kattintson a legördülő menü, jelölje ki a tartományt, és válassza ki a munkaterületet. Minden adat helyi a munkaterület területre; például a webes szolgáltatások munkaterületről létrehozott összes lesz a munkaterületen található régióban.
![Több-Geo súgó kép 3][3]

## <a name="open-an-experiment-from-gallery"></a>Nyissa meg a kísérlet gyűjteményből

Egy kísérlet gyűjtemény megnyitása esetén válassza ki melyik a kísérlet a másolni kívánt terület.

![Több-Geo súgó kép 4][4a]

## <a name="web-service-management"></a>Webszolgáltatás-kezelése

Programozás útján kezelheti a webes szolgáltatások, például átképzés a területhez kötött cím használata:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Megjegyzés: szempontok

1.  Csak másolhatja kísérletek közé tartoznak az ugyanazon régió ily módon munkaterületek. Ha módosítani szeretné a kísérlet másolja át a munkaterületek különböző régiókban, használhatja a [PowerShell](http://aka.ms/amlps) -parancsmag [*Másolása-AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) elvégezheti, hogy. A kísérlet az gyűjtemény közzétenni a listán nem módban egy másik megoldás: Nyissa meg a másik régióból a munkaterületen.
2.  A terület sorkijelölőre egyszerre csak jelennek meg munkaterületek egy területhez tartozik. A jövőben lesz munkaterületek van hozzáférése az összes területek között egyszerre egy teljes listájának megjelenítéséhez.  
3.  Egy ingyenes munkaterület vagy a vendégként való hozzáférés (névtelen) munkaterület létrejön és a déli központi USA-beli üzemeltetett A jövőben lesz a választott régióban Free/vendégként való hozzáférés-munkaterületek létrehozására.  
4.  Délkelet-ázsiai-munkaterületről rendszerbe webszolgáltatásokhoz is kell elhelyezni Délkelet-ázsiai. A jövőben fogja tudni szeretné, hogy létrehozhatók kísérletek egy régió rugalmasan, és üzembe helyezése, webes szolgáltatás végpontok különböző területekre hozza létre.  

## <a name="more-information"></a>További információ

Kérdés feltevése az [Azure gépi tanulási fórumán](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
