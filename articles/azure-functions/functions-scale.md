<properties
   pageTitle="Hogyan méretezheti Azure függvények |} Microsoft Azure"
   description="Ismerje meg, hogyan Azure függvények méretezni a eseményvezérelt munkaterhelésekből az igényeinek megfelelően."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure függvények funkciók, esemény feldolgozása, webhooks, dinamikus számítási, kiszolgáló nélküli architektúra"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Hogyan méretezheti Azure függvények

## <a name="introduction"></a>– Bevezetés

Azure-funkciók az egyik előnye, hogy számítási erőforrások csak felhasznált szükség esetén. Ez azt jelenti, hogy fizet az üresjárati VMs vagy kell lefoglalása kapacitása mikor valószínűleg kell azt nem. Ehelyett a platform lefoglalja számítási power, amikor a kód fut, kezelheti a betöltés szükség szerint méretezze át, majd méretezze át nem fut a kódot.

Az eljárás ezt a funkciót a dinamikus szolgáltatáscsomagja.  

Ez a cikk áttekintést nyújt a dinamikus szolgáltatáscsomagja működése és a platform hogyan méretezze át a kód futtatásához igény.

Ha még nem ismeri a Azure függvények, akkor ellenőrizze, hogy az [Azure függvények áttekintése](functions-overview.md) cikk megértéséhez képességeit.

## <a name="configure-azure-functions"></a>Azure funkciók beállítása

Két fő beállítása kapcsolódnak méretezés:

* [Azure alkalmazás szolgáltatáscsomagja](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vagy dinamikus előfizetési csomagja
* A végrehajtás környezetben memóriaméret

A költség, a függvény a választott díjcsomagjától függően változik. Egy dinamikus szolgáltatás csomaggal költség feladata végrehajtás ideje, memóriaméret és végrehajtások számát. A díjak felmerülés, csak ha a kód ténylegesen fut.

Az alkalmazás szolgáltatáscsomagja a funkciók a meglévő VMs, amely az is előfordulhat, hogy a többi kód futtatásához használt tárolja. Miután fizet az alábbi VMs havonta, nem további ingyenesek végrehajtás függvények őket.

## <a name="choose-a-service-plan"></a>Szolgáltatás csomag választása

Függvények létrehozásakor kijelölhet egy dinamikus szolgáltatás vagy az [alkalmazás szolgáltatáscsomagja](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)a futtatásához.
Az alkalmazás szolgáltatáscsomagja a függvények egy dedikált virtuális web apps munka ma Basic, a normál vagy a prémium termékváltozatok hasonlóan fog futni.
Ez a dedikált virtuális az alkalmazások és a függvények rendelt és mindig elérhető e kód aktívan végrehajtja-e. Ez a beállítás jó meglévő, a kihasználtság VMs, amelyeken már egyéb programkódját esetén, vagy ha várhatóan folyamatosan vagy szinte folyamatosan függvények futtatásához. A virtuális különválasztja futtatókörnyezet és a memóriában méretét a költség. Sok hosszabb ideig futó funkciók közül az egy vagy több VMs rendszeren futó költségét a költség eredményt adja, korlátozhatja.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
