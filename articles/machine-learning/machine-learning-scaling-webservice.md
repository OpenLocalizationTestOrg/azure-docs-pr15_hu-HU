<properties
   pageTitle="Webszolgáltatás méretezés |} Microsoft Azure"
   description="Megtudhatja, hogy miként méretezheti webszolgáltatás feldolgozási növekvő, és fel új végpontok."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure gépi tanulási, web services, operationalization, beosztását, végpontot, feldolgozási"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Webszolgáltatás méretezése

>[AZURE.NOTE] Ez a témakör ismerteti a klasszikus gépi tanulási webszolgáltatás alkalmazandó technikákat alkalmaz. 

Alapértelmezés szerint minden közzétett webes szolgáltatás 20 egyidejű kérelmek támogatására van beállítva, és a magas, mint 200 egyidejű kérelmek lehet. Az Azure klasszikus portál oly módon, hogy állítsa ezt az értéket, miközben Azure gépi tanulási automatikusan optimalizálja a beállítását, hogy a legjobb teljesítmény elérése érdekében a webszolgáltatás, és a portál nem veszi figyelembe. 

Hívja fel az API 200 a Max párhuzamos hívások értéknél nagyobb terhelést a terv támogatja, ha több végpontok webes ugyanezt a kell létrehozni. Akkor is majd véletlenszerűen elosztása a betöltés az összeset.

## <a name="add-new-endpoints-for-same-web-service"></a>Új végpontok azonos webszolgáltatás hozzáadása

A webszolgáltatás méretezését egy közös feladatot. Oka annak, ha át kívánja méretezni nem támogatja a legfeljebb 200 egyidejű kérelmek, több végpontok elérhetősége növelése vagy a webszolgáltatás külön végpontjait a szükséges. A méretezés növelheti a azonos webszolgáltatás [Azure klasszikus portál](https://manage.windowsazure.com/) vagy az [Azure gépi tanulási webszolgáltatás](https://services.azureml.net/) portálon keresztül további végpontok hozzáadásával.

Új végpontok hozzáadásával kapcsolatos további tudnivalókért olvassa el a [Végpontok létrehozása](machine-learning-create-endpoint.md)című témakört.

Ne feledje, hogy a magas verzió-ellenőrzési száma használatával hátrányos, ha nem az API ennek megfelelően nagy sebességének hívott lehet. Látható kapcsolata időről-időre időkorlátok és/vagy kiugrásainak megfelelő a a késés Ha viszonylag kevés terhelést elhelyezése egy nagy terhelés konfigurált API-t.

A szinkronizált API-k jellemzően azokról a helyzetekről, amikor egy kis késés van szükség. Itt késés azt jelenti, hogy az idő az API-t egy kérelem befejezéséhez szükséges időt, és nem számlája bármelyik hálózati késések. Tegyük fel, hogy van egy olyan 50-ms válaszidejű API-t. Teljesen használhatnak, a szabályozási szinthez magas kapacitással és a Max párhuzamos hívások = 20, kell fordulnia, ez az API 20 * 1000 / 50 = 400 időtúllépés másodpercenként. Ez további bővítése, a Max párhuzamos hívások 200 indíthat hívást másodpercenként, feltéve, hogy egy 50-ms késés az API 4000 időpontokat.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
