<properties
   pageTitle="Új webes szolgáltatás üzembe helyezése"
   description="A munkafolyamat-ARM üzembe helyezése a webszolgáltatás alapján"
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
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Új webes szolgáltatás telepítése

Microsoft Azure gépi tanulási most Itt [Azure erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) alapuló webszolgáltatásokhoz, lehetővé teszi az új terv beállítások számlázás és üzembe helyezése a webszolgáltatás több területre.

Az általános munkafolyamatot bevezetését tervezi a Microsoft Azure gépi tanulási webszolgáltatások használatával webszolgáltatás van:

* Hozzon létre egy prediktív kísérlet
* üzembe
* Állítsa be a nevét
* számlázási terv
* Tesztelje
* felhasználja azt.

Az alábbi ábra szemlélteti a munkafolyamatot.

![Webes szolgáltatás telepítési munkafolyamat][1]
 
## <a name="deploy-web-service-from-studio"></a>Webszolgáltatás Studio terjesztése 

Új webes szolgáltatás telepítése a kísérlet. Jelentkezzen be a gépi tanulási Studio, és hozzon létre egy új prediktív webszolgáltatásból. 

**Megjegyzés**: Ha már telepítette a klasszikus webes szolgáltatásként kísérlet nem beállítaná őket, egy új webszolgáltatásból.
 
Kattintson a **Futtatás** a kísérlet vászon alján, és válassza a **Webes szolgáltatás üzembe** és **Üzembe webszolgáltatás [Új]**. A telepítés lap a gépi tanulási webszolgáltatás vezető nyílik meg.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Gépi tanulási Web Service Manager kísérlet lap terjesztése
A kísérlet üzembe lapon adja meg a webszolgáltatás nevét.
Válasszon egy árak csomagot. Ha egy meglévő, árak csomagot használ, kiválaszthatja azt, egyéb esetben kell létrehoznia a szolgáltatás új ár csomagot. 

1.  Az **Ár terv** legördülő lista válassza ki a meglévő csomagjával vagy jelölje ki a **Válassza ki az új csomag** lehetőséget.
2.  Az **Előfizetés nevétől**írja be egy nevet, amely azonosítja a csomagot, a számlán.
3.  Jelölje ki azt a **Havi megtervezése rétegek**. Figyelje meg, hogy a terv rétegek alapértelmezés szerint a tervek az alapértelmezett régió és a webszolgáltatás telepítve van a régióba.

Kattintson a **központi telepítés** és a webes szolgáltatás megnyílik a quickstart útmutató lapot.

## <a name="quickstart-page"></a>Quickstart útmutató lap
A szolgáltatás quickstart útmutató lap kínál fel az access és az útmutatást a leggyakoribb műveleteinek elkészíti új webszolgáltatás létrehozása után. További lehetőségek könnyen hozzáférhető a **próba** lapon és a **felhasználandó** lapot.

## <a name="testing-your-web-service"></a>A webszolgáltatás tesztelése

Quickstart útmutató lapján kattintson próba webszolgáltatás a gyakori feladatokat.   

A webszolgáltatás megegyezik egy kérés-válasz szolgáltatás (Erőforrásrekordok) tesztelése:

* A menüsávon kattintson a **vizsgálat** .
* Kattintson a **Kérelem-válasz**gombra.
* Írja be a kísérlet a bemeneti oszlopok megfelelő értéket.
* Kattintson a vizsgálat **Kérés-válasz**gombra.

A lap jobb oldalán lévő megjeleníteni kívánt eredmények.

A köteg végrehajtás szolgáltatás (BES) webszolgáltatás teszteléséhez használandó CSV-fájl:

* A menüsávon kattintson a **vizsgálat** .
* Kattintson a **köteget**.
* A bemenet csoportban kattintson a Tallózás gombra, és keresse meg az adatfájl – minta.
* Kattintson a **vizsgálat**gombra.

A próba állapotának **Tesztelése köteg feladatok**alatt jelenik meg.

## <a name="consuming-your-web-service"></a>A webszolgáltatás fogyasztása

Amikor rendszerbe webes szolgáltatásként, az Azure gépi tanulási kísérletek a REST API-t platformokon és eszközök széles köre felhasználható adja meg. Ennek oka az egyszerű elfogadja a REST API-t, és válasza a JSON formázott üzenetek. Az Azure gépi tanulási portál kódot, hívja fel a webszolgáltatás az R, C# és Python használható.
 
Felhasználás lapon találja meg:

* Az API-ja kulcs és URI a webszolgáltatás az alkalmazások használata más.
* Az Excel és a webes alkalmazássablonok indítása a felhasználás megkezdéséhez.
* A C#, python és R első kód minta.

További tájékoztatást a webes szolgáltatások használata más megtudhatja, [hogy miként felhasználása az Azure gépi tanulási webszolgáltatás, amely egy kísérlet tanulási számítógépről lett telepítve](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Következő lépések

Webes szolgáltatások használata más kapcsolatos további tudnivalókért lásd:

[Hogyan kell az Azure gépi tanulási webszolgáltatás kísérlet tanulási számítógépről telepített felhasználása](machine-learning-consume-web-services.md)

[Azure gépi tanulási webszolgáltatások: Telepítési és felhasználási](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
