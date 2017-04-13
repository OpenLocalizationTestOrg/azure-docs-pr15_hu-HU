<properties 
    pageTitle="Gépi tanulási közzététele webszolgáltatás Azure Piactérhez |} Microsoft Azure" 
    description="Az Azure gépi tanulási webszolgáltatás közzététele az Azure Piactérhez" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Azure gépi tanulási webszolgáltatás közzététele az Azure piactérről 

A Microsoft Azure piactéren lehetővé teszi a Azure gépi tanulási webszolgáltatásokhoz közzé, azok a kifizetett vagy ingyenes szolgáltatások külső felhasználók által történő használatra. Ez a cikk áttekintést nyújt, amelyek a folyamat, amely hivatkozásokat tartalmaz az első lépések útmutató. Ez az eljárás használatával elérhetővé teheti a webes szolgáltatások más fejlesztők számára a saját alkalmazások felhasználni.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>A közzétételi folyamat áttekintése 

Az Azure gépi tanulási webszolgáltatás közzétételi Azure Piactérhez lépései a következők:

1. Hozzon létre és gépi tanulási kérés-válasz szolgáltatás (Erőforrásrekordok) közzététele
2. A szolgáltatás üzembe termelési, és szerezze be az API-ja kulcs és az OData-végpont adatait.
3. Az URL-címet a közzétett webes szolgáltatás használata való közzétételhez [Microsoft Azure piactéren (Datamarket)](https://publish.windowsazure.com/workspace/) 
4. Miután beküldött dokumentumot, az ajánlat felül, és a szükséges előzetesen jóváhagyni az ügyfelekkel előtt elkezdheti azt vásárlásával. A közzétételi folyamat vehet igénybe néhány munkanapnak. 

## <a name="walk-through"></a>Haladjon végig, és
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Lépés: 1: Létrehozása és gépi tanulási kérés-válasz szolgáltatás (Erőforrásrekordok) közzététele###
 Ha van nem tette meg, kérjük, nézze meg a [végigvezetik](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Lépés: 2: A szolgáltatás üzembe termelési, és az API-ja kulcs és az OData-végpont adatait beszerzése###
1. Az [Azure klasszikus portál](http://manage.windowsazure.com)jelölje ki a **Gépi TANULÁSI** beállítást a bal oldali navigációs sávon, és válassza ki a munkaterület. 

2. Lapon kattintson a **WEBSZOLGÁLTATÁSOKHOZ** , és jelölje ki a kívánt közzététele a piactér webszolgáltatás.

    ![Azure piactérről][workspace]

3. Válassza ki a végpontot, akinek szeretne felhasználni a piactér van. Ha bármilyen további végpontokhoz nem hozott létre, választhat az **alapértelmezett** végpontot.

4. Miután kattintott, a végpont, láthatja az **API -ja kulcs**fogja. A darab szüksége lesz a 3 később adatait, így ügyeljen belőle másolatot.

    ![Azure piactérről][apikey]

5. Kattintson a **KÉRÉS/válasz** módszer, ezen a ponton a piactér közzétételi köteg végrehajtás szolgáltatások nem támogatjuk. Amely eljut az API Súgó lap kérés/válasz mód.

6. Másolja az **OData-végpontjának címe**, ezt az információt a 3 később szüksége lesz.

    ![Azure piactérről][odata]




a szolgáltatás üzembe éles.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Lépés 3: Használja a közzétett webes szolgáltatás URL-CÍMÉT való közzétételhez Microsoft Azure piactéren (DataMarket)###

1.  Nyissa meg azt az [Azure piactér (Datamarket)](http://datamarket.azure.com/home) 
2.  Kattintson a **Közzététel** hivatkozásra a lap tetején. Ezzel eljut a [Microsoft Azure közzétételi portál](https://publish.windowsazure.com)
3.  Kattintson a egy publisher nyilvántartásba **közzétevők** című szakaszt.
4.  Ha létrehoz egy új ajánlatot, válassza a **Data Services**, majd kattintson a **Létrehozás új adatok szolgáltatás**. 
 
    ![Azure piactérről][image1]

    <br />


5.  A **tervek** tájékoztatást nyújt a ajánlja fel, beleértve a árak tervet. Döntse el, ha egy ingyenes vagy fizetett szolgáltatás tartalmazni fogja. Első fizetni, adja meg a fizetési információk, például a banki és adó.

6.  A **Marketing** az ajánlat, például a cím és leírás az ajánlatra információt nyújtanak.

7.  **Árak** csoportban állítsa be az árat adott országhoz vagy a tervek, vagy, hogy a rendszer "autoprice" a felajánlást.

8. **Adatszolgáltatás** kattintson a lap, **Webes szolgáltatás** az **Adatforráshoz**.

    ![Azure piactérről][image2]

9.  A webes szolgáltatás URL-CÍMEK és az API kulcs lekérése az Azure klasszikus portálon a fenti 2 című cikkben ismertetett módon.

10. A piactér-adatok szolgáltatás beállítása párbeszédpanelen illessze be az OData-végpontjának címe a **Szolgáltatás URL-CÍMRE** mezőbe.

11. A **hitelesítés**válassza a **fejléc** a **Hitelesítési módot**.

    - Írja be a **táblázatfejlécek nevét**a "engedély".
    - A **Fejléc értéke**(az idézőjelek nélkül) írja be a "Bearer" **a szóköz** kattintson, és illessze be a API billentyűt.
    - Jelölje be **a szolgáltatás OData** jelölőnégyzetet.
    - Tesztelje a kapcsolatot a **Kapcsolat tesztelése** gombra.

12. **A kategóriák**csoportban győződjön meg róla, **Gépi tanulási** ki van jelölve.

13. Amikor befejezte **Közzététel**, majd **az előkészítés leküldéses**beírása az ajánlat, metaadatait parancsára. Ezen a ponton, értesítést kapnak az esetleges további problémák kijavításához szükséges.

14. A nyitott problémákat kiegészítésének megbizonyosodott kattintson **a termelési leküldéses jóváhagyást**. A közzétételi folyamat vehet igénybe néhány munkanapnak. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
