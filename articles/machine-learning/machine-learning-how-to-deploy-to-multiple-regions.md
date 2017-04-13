<properties
    pageTitle="Több területre webszolgáltatás telepítéséről |} Microsoft Azure"
    description="A régióban (Másolás) más új webes szolgáltatás üzembe lépéseket."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Több területre webszolgáltatás telepítéséről

Az új Azure webszolgáltatások könnyen telepíthető webszolgáltatás több területre előfizetések vagy a munkaterületek őt nem teszi lehetővé. 

Adott, ezért meg kell adnia egy számlázási csomagra, amelyben a webszolgáltatás telepíti területenként régió árak.

## <a name="to-create-a-plan-in-another-region"></a>Egy másik terület terv létrehozása

1. Jelentkezzen be a [Microsoft Azure gépi tanulási a webes szolgáltatások](https://services.azureml.net/).
2. Kattintson a **Csomagváltás** menü lehetőségre.
3. A tervek a nézet lap fölé, kattintson az **Új**gombra.
4. Az **előfizetés** legördülő listából válassza azt az előfizetést, az új csomag elhelyezkednek.
5. A **régió** legördülő listából válassza ki a régió, az új csomagban. A kijelölt terület az megjelenik az oldal **Megtervezése beállítások** szakaszban.
6. Az **Erőforráscsoport** legördülő listából válassza ki a terv erőforráscsoport. További tudnivalókat az erőforrás csoportok cikkekből [kezelése Azure portálon keresztül](../azure-portal/resource-group-portal.md).
7. Az **Előfizetés nevétől** írja be az előfizetésére.
8. A **Terv beállítások**csoportban kattintson az új csomagban a számlázási szintet.
9. Kattintson a **létrehozása**gombra.


## <a name="deploying-the-web-service-to-another-region"></a>A webes szolgáltatás üzembe helyezése a másik területére

1. Kattintson a **Web Services** menü lehetőségre.
2. Jelölje ki a webszolgáltatás telepít az új területére.
3. Kattintson a **Másolás**parancsra.
4. A **Webhely szolgáltatás neve**mezőbe írja be egy új nevet a webszolgáltatás.
5. A **webszolgáltatás leírását**írja be a webszolgáltatás leírását.
6. Az **előfizetés** legördülő listából válassza azt az előfizetést, az új webszolgáltatás elhelyezkednek.
7. Az **Erőforráscsoport** legördülő listából válassza ki a webszolgáltatás erőforráscsoport. További tudnivalókat az erőforrás csoportok cikkekből [kezelése Azure portálon keresztül](../azure-portal/resource-group-portal.md).
8. A **régió** legördülő listából válassza ki a régió, amelyben a webszolgáltatás telepítéséhez.
9. A **tárterület-fiókot** tartalmazó legördülő listára jelölje ki egy tárterület-fiókkal, amelyben a webszolgáltatás tárolni szeretné.
10. Az **Ár terv** legördülő listából válasszon egy csomagot a 8 lépésben kiválasztott régióban.
11. Kattintson a **Másolás**parancsra.

