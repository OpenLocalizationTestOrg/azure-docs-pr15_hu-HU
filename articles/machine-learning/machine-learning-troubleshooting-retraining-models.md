<properties
    pageTitle="Hibaelhárítás a Retraining az Azure gépi tanulási klasszikus webszolgáltatás |} Microsoft Azure"
    description="Azonosítása és javítása a gyakori problémák encounted, amikor az Azure gépi tanulási webszolgáltatás vannak mind a modellt."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Az Azure gépi tanulási klasszikus webszolgáltatás átképzésének hibaelhárítása

## <a name="retraining-overview"></a>Átképzési – áttekintés

Amikor rendszerbe állítják a cserélendő kísérlet pontozási webes szolgáltatásként egy statikus modell. Elérhetővé válik az új adatok, vagy ha az ügyfél, a API rendelkezik saját adataikat, a modell kell lennie retrained. 

A klasszikus webszolgáltatás átképzési folyamata a teljes áttekintése a következő témakörben [Újraépítés gépi tanulási modellek programozás útján](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Átképzési folyamatábra

Ha a webszolgáltatás Újraépítés kell hozzá kell adnia a néhány további darab:

* A tanfolyam kísérlet rendszerbe webszolgáltatás. A kísérlet a kimenet a **Vonaton modell** modul csatolt **Web Service kimeneti** modul kell rendelkeznie.  

    ![A web service kimenet csatolása a vonaton modell.][image1]

* A pontozási webszolgáltatás hozzáadott új végpontot.  A végpont programozás útján a minta kódot a Újraépítés gépi tanulási modellek hivatkozott programozás útján is hozzáadhat témakört, vagy az Azure klasszikus portálon keresztül.

A tanfolyam webszolgáltatás API súgó lapon a minta C# kód segítségével majd Újraépítés modell. Miután az eredmények kell értékelni, és elégedve őket, frissíti a webes szolgáltatás használata a felvett új végpont pontozási képzett modellt.

Az összes a csatolt helyen a fő lépés, végre kell hajtania a modell Újraépítés szeretne a következők:

1.  A tanfolyam webszolgáltatás hívása: A hívás nem szeretné a köteg végrehajtás szolgáltatás (BES), nem a kérése válasz szolgáltatás (Erőforrásrekordok). Az API súgó lapon a minta C# kód segítségével a híváshoz. 
2.  Az értékek megkeresése az *BaseLocation*, *RelativeLocation*és *SasBlobToken*: ezeket az értékeket ad vissza az eredményben a oktatás webszolgáltatás hívása. 
      ![a kimenet átképzési mintát és BaseLocation RelativeLocation és SasBlobToken értékeket tartalmazó.][image6]
3.  A pontozási webszolgáltatásból a hozzáadott végpont frissítse az új képzett modell: a gépi tanulási Újraépítés modellekben programozás útján, feltéve minta kód használatával frissítheti az újonnan képzett modell pontozási adatmodellhez oktatás webszolgáltatásból felvett új végpontot.

## <a name="common-obstacles"></a>Gyakori akadálya

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Ellenőrizze, hogy a javítás URL-cím helyes esetén

A javítás URL-címet használja az új pontozási végpontot, a pontozási webszolgáltatás hozzá társított azzal kell lennie. Számos módon szerezze be a javítás URL-címe:

**Kapcsoló 1: programozottan**

A javítás URL-cím helyes beszerzése:

1.  A minta [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) kódokat futtathatnak.
2.  AddEndpoint kimenetét *HelpLocation* érték található, és másolja a vágólapra az URL-címet.

    ![Az eredményben a addEndpoint minta HelpLocation.][image2]

3.  A böngészőben nyissa meg a lapot, ahol súgótémakörökre mutató hivatkozások esetében a webszolgáltatás illessze be az URL-címet.
4.  A **Frissítés erőforrás** hivatkozásra kattintva nyissa meg a javítás súgó lapot.

**2 beállítás: Az Azure klasszikus portál használata**

1.  Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2.  Nyissa meg a gépi tanulási fülre. 
     ![Gépi leaning fülre.][image4]
3.  Kattintson a munkaterület nevét, majd a **Webes szolgáltatások**.
4.  Kattintson a pontozási webszolgáltatás használata. (Nem módosíthatja az alapértelmezett nevet a webszolgáltatás, ha azt érjen véget az [pontozási kitevő.].)
5.  Kattintson a **végpont hozzáadása**gombra.
6.  Miután hozzáadták az végpontot, kattintson a végpontjának neve. Kattintson a **Frissítés erőforrás** a javítási Súgó lap megnyitásához.

>[AZURE.NOTE] A oktatás webszolgáltatás helyett a cserélendő webszolgáltatás hozzáadott a végpontot, ha kap a következő hiba a **Frissítés erőforrás** hivatkozásra: Sajnáljuk, de ez a funkció nem támogatott vagy nem érhető el ez a környezetben. Ez a szolgáltatás nem rendelkezik a nem frissíthető erőforrással. Hogy a hiba kijavítása folyamatban és javítása a munkafolyamat éppen dolgozik.

![Új végpont irányítópult.][image3]

A javítás Súgó lap tartalmazza a javítás URL-címet kell használnia, és itt példakódot segítségével hívja meg.

![Javítás URL-címe.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Ellenőrizze, hogy, hogy a helyes pontozási végpont frissít

* A javítás nem a oktatás webszolgáltatás: A javítás művelet kell elvégezni a pontozási webes szolgáltatásokhoz.
* A javítás nem a webszolgáltatás az alapértelmezett végpont: A javítás művelet kell elvégezni, kattintson az új pontozási webes szolgáltatás végpontjának felvett.

Ellenőrizheti, hogy melyik webszolgáltatás, a végpontok van kapcsolva az Azure klasszikus portálon megtalálhatók. 

>[AZURE.NOTE] Győződjön meg arról, hogy a cserélendő webszolgáltatás, nem a oktatás webszolgáltatás hozzáadni az végpontot. Ha egy képzés és a prediktív webszolgáltatás megfelelően telepítette, meg kell jelennie felsorolt két külön webszolgáltatásokhoz. A cserélendő webszolgáltatás "[prediktív kitevő.]" kell végződnie.

1.  Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2.  Nyissa meg a gépi tanulási fülre. 
     ![Gépi tanulási felhasználói felületének munkaterület.][image4]
3.  Jelölje ki a munkaterület.
4.  Kattintson a **webes szolgáltatások**.
5.  Jelölje ki a cserélendő webszolgáltatásból.
6.  Győződjön meg arról, hogy az új végpont hozzáadva a webszolgáltatás.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>A munkaterület, amely a webszolgáltatás az annak biztosítására, hogy a helyes tartományban lévő ellenőrzése

1.  Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com).
2.  Gépi tanulási válassza a menüből.
      ![Gépi tanulási régió felhasználói felület.][image4]
3.  Ellenőrizze a munkaterület helyét.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png