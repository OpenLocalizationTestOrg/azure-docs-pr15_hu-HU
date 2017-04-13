<properties
    pageTitle="Webes szolgáltatás végpontok létrehozása a gépi tanulási |} Microsoft Azure"
    description="Azure gépi tanulási Web service végpont létrehozása"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Végpont létrehozása

>[AZURE.NOTE] Ez a témakör ismerteti a klasszikus webszolgáltatás alkalmazandó technikákat alkalmaz.

Webszolgáltatások, Ön által forgalmazott előre oda az ügyfelekre létrehozásakor meg kell adnia a képzett modellek minden továbbra is a kísérlet, amely a webszolgáltatás készült csatolt ügyfélnek. Ezenkívül a kísérlet valamilyen frissítés kell vonatkozni szelektív zárólap a testre szabott funkciók felülírása nélkül.

Ehhez Azure gépi tanulási létrehozását teszi lehetővé a telepített webszolgáltatás több végpontokat. A webszolgáltatás az egyes végpont független, amelyekkel foglalkozott, szabályozott, és felügyelt. Minden végpontra egy egyedi URL-CÍMEK és a hitelesítési kulcsot, amely az ügyfelekkel oszthat.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Webszolgáltatás hozzáadása végpontok

Háromféleképpen végpont hozzáadása egy webszolgáltatásból.

* Programozás útján
* Az Azure gépi tanulási webszolgáltatásokhoz portálon keresztül
* Bár az Azure klasszikus portálra

Amikor létrejött a végpontot, keresztül szinkronizált API-k, köteg API-hoz, ezért használja fel, és az excel-munkafüzetek. Végpontok – Ez a felhasználói felület beszúrásán is használhatja a végpontot Management API-k programozás útján hozzáadása a végpontok.

 >[AZURE.NOTE] Ha további végpontok hozzáadta a webes szolgáltatás, az alapértelmezett végpont nem törölhetők.

## <a name="adding-an-endpoint-programmatically"></a>Zárólap programozás útján hozzáadása

Zárólap fűzheti hozzá a webszolgáltatás programozás útján a minta [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) kód használatával.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Az Azure gépi tanulási webszolgáltatásokhoz portálon végpont hozzáadása

1. Gépi tanulási Studio a bal oldali oszlopban kattintson a webes szolgáltatások.
2. A webes szolgáltatás irányítópult alján kattintson a **kezelés végpontok**. Az Azure gépi tanulási Web Services portál esetében a webszolgáltatás az végpontok lapon nyílik meg.
3. Kattintson az **Új**gombra.
4. Írja be a nevét és leírását az új végpontot. Végpont nevét kell 24 karakter vagy annál kisebb hossza és kisbetűk ábécét vagy a számok készült. Jelölje ki a naplózási szintet, és hogy engedélyezve van-e a mintaadatok. A naplózás további tudnivalókért olvassa el a [gépi tanulási webszolgáltatásokhoz naplózás engedélyezése](machine-learning-web-services-logging.md)című témakört.

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Az Azure klasszikus portálon végpont hozzáadása


1. Jelentkezzen be az [Azure klasszikus portálra](http://manage.windowsazure.com), és **Gépi tanulási** kattintson a bal oldali oszlopban. Kattintson a munkaterületre, amely tartalmazza a webszolgáltatás, amely érdekli.

    ![Nyissa meg azt a munkaterület](./media/machine-learning-create-endpoint/figure-1.png)

2. Kattintson a **webes szolgáltatások**.

    ![Nyissa meg azt a webes szolgáltatások](./media/machine-learning-create-endpoint/figure-2.png)

3. Kattintson a webszolgáltatás, amely érdekli elérhető végpontok listájának megjelenítéséhez.

    ![Nyissa meg azt a végpontot](./media/machine-learning-create-endpoint/figure-3.png)

4. A lap alján kattintson a **Végpont hozzáadása**gombra. Írja be a nevét és leírását, győződjön meg róla, létezik a webszolgáltatás az azonos nevű nincsenek végpontok. Hagyja az alapértelmezett értékére a szabályozási szintet, kivéve, ha speciális igényei vannak. Szabályozásának kapcsolatos további információért olvassa el a [Méretezés API-végpontok](machine-learning-scaling-webservice.md)című témakört.

    ![Végpont létrehozása](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Következő lépések

[Hogyan Azure gépi tanulási közzétett webszolgáltatás használhatnak](machine-learning-consume-web-services.md).
