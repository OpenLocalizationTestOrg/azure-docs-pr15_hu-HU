<properties
    pageTitle="A gépi tanulási modell Újraépítés |} Microsoft Azure"
    description="Megtudhatja, hogy miként Újraépítés a modellt, és frissítse a webszolgáltatás szeretné használni az újonnan képzett modell Azure gépi tanulási."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>A gépi tanulási modell Újraépítés

Gépi tanulási modellek Azure gépi tanulási operationalization folyamatának részeként a modell képzett, és menti. Majd létrehozhat annak segítségével predicative webszolgáltatás. A webszolgáltatás majd felhasználhatók, a webhelyek, az irányítópultok és mobilalkalmazások. 

Gépi tanulási segítségével létrehozott modellek sem általában statikus. Elérhetővé válik az új adatok, vagy ha az ügyfél, a API saját adatot tartalmaz a modell kell retrained kell. 

Átképzés fordulhat elő, gyakran. A programozott átképzés API szolgáltatással programozás útján is a átképzés API-k használata a modell Újraépítés és frissítse a webszolgáltatás az újonnan képzett modell. 

A dokumentum ismerteti a átképzési folyamat, és megtudhatja, hogy miként használhatja a átképzés API-k.

## <a name="why-retrain-defining-the-problem"></a>Miért Újraépítés: a probléma definiálása  

A gépi tanulási képzési folyamat részeként modell képzett adatok egy készlet segítségével. Gépi tanulási segítségével létrehozott modellek sem általában statikus. Elérhetővé válik az új adatok, vagy ha az ügyfél, a API saját adatot tartalmaz a modell kell retrained kell.

Ez a helyzet programozott API-val, vagy a fogyasztói az API-k létrehozása, amelyek egyszeri vagy normál alapján Újraépítés is, a saját adatok használata modell ügyfél kényelmesen nyújt. Azok majd felmérése átképzés eredményét, és frissítse az újonnan képzett modell használata a webes szolgáltatáshoz tartozó API.

>[AZURE.NOTE] Ha egy meglévő oktatás kísérlet és az új webhely szolgáltatás, érdemes teljesített kapcsolat-újraépítési tanulmányozza a következő részben leírt forgatókönyv követő helyett egy meglévő prediktív webszolgáltatás.

## <a name="end-to-end-workflow"></a>Végpont munkafolyamat 

A folyamat hozzáadásához kövesse az alábbi összetevőket: A oktatás kísérlet és a prediktív kísérlet közzétett egy webszolgáltatásból. Ahhoz, hogy egy képzett modell átképzés, a oktatás kísérlet webes szolgáltatásként a kimenet képzett modell tehető közzé. Ezzel az API-val való hozzáférést a modell átképzés. 

Új és a klasszikus Web services alkalmazni az alábbi lépéseket:

A kezdeti prediktív webszolgáltatás létrehozása:

* Hozzon létre egy oktatás kísérlet
* Hozzon létre egy prediktív webes kísérlet
* Prediktív webszolgáltatás terjesztése

A webszolgáltatás Újraépítés:

* Oktatás kísérlet működéséhez átképzés frissítése
* A átképzési webszolgáltatás terjesztése
* A köteg végrehajtás szolgáltatáskód segítségével Újraépítés a modell

Az előző lépések útmutató olvassa el a [gépi tanulási Újraépítés modellek programozás útján](machine-learning-retrain-models-programmatically.md)című témakört.

Ha telepítette klasszikus webszolgáltatás:

* Hozzon létre egy új végpontot az előrejelzési webes szolgáltatása
* A javítás URL-cím és a kódot.
* A javítás URL-cím használatával az új végpontot mutasson a retrained modell 

Az előző lépések útmutató olvassa el a [Klasszikus webszolgáltatás Újraépítés](machine-learning-retrain-a-classic-web-service.md)című témakört.

Ha nehézségei klasszikus webszolgáltatás átképzés lép, lásd: [az Azure gépi tanulási klasszikus webszolgáltatás átképzésének hibaelhárítás](machine-learning-troubleshooting-retraining-models.md).

Ha telepítette az új webhely szolgáltatás:

* Jelentkezzen be az erőforrás-kezelő Azure-fiókjába
* A webes szolgáltatás definíció beszerzése
* A Web Service Definition JSON exportálása
* A hivatkozás frissítése a `ilearner` blob a a JSON-ban
* A JSON importálása a webes definiált szolgáltatás
* Frissítse a webszolgáltatás új webes szolgáltatás meghatározása

Az előző lépések útmutató olvassa el a [gépi tanulási kezelése a PowerShell-parancsmagok használata új webszolgáltatás Újraépítés](machine-learning-retrain-new-web-service-using-powershell.md)című témakört.

A folyamat beállítása klasszikus webszolgáltatás mind a következő lépésekből áll:

![Átképzési folyamat áttekintése][1]

Állíthat be egy új webszolgáltatás mind folyamata a következő lépésekből áll:

![Átképzési folyamat áttekintése][7]

## <a name="other-resources"></a>Egyéb források

- [Azure Data Factory modellek átképzés és Azure gépi tanulási frissítése](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Sok gépi tanulási modellek és készítése webes szolgáltatás végpontok egy kísérlet PowerShell használatával](machine-learning-create-models-and-endpoints-with-powershell.md)
- A [AML átképzés az adatmodellek használatával API -khoz](https://www.youtube.com/watch?v=wwjglA8xllg) videóból megtudhatja, hogyan készült Azure gépi tanulási gépi tanulási modellek Újraépítés a átképzés API-hoz és a PowerShell használatával.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

