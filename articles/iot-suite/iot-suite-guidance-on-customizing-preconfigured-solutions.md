<properties
    pageTitle="Megoldások testreszabása előre |} Microsoft Azure"
    description="Az előre beállított Azure IoT csomagja megoldások testreszabásával ismerteti."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Előre beállított megoldás testreszabása

Az előre definiált megoldások az Azure IoT csomagot ellátni a szolgáltatások belül a közös munka egy végpont megoldás előadásához programcsomag mutatja be. A kezdőponttól vannak számos helyről, ahol kiterjesztése és a különböző forgatókönyvekben megoldás testreszabása. Az alábbi szakaszok ismertetik az alábbi általános testreszabási pontok.

## <a name="finding-the-source-code"></a>Keresés a Forráskód

Az előre definiált megoldások forráskódot GitHub, a következő tárházakban található érhető el:

- Távoli figyelemmel kísérése: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- [Https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance) prediktív karbantartása:

Az előre definiált megoldások forráskódot kapja meg bemutatják, a mintázatok és eljárások az Azure IoT Suite használata IoT megoldás végpontok közötti működésének végrehajtásához használja. Létre és helyezhetnek üzembe a GitHub tárházakban található megoldásokkal részletes tájékoztatást talál.

## <a name="changing-the-preconfigured-rules"></a>Az előre beállított szabályok módosítása

A távoli felügyeleti megoldást három [Azure Értékáram-elemzés](https://azure.microsoft.com/services/stream-analytics/) feladatok végrehajtásához eszköz információkat, telemetriai és szabályok logika jelenik meg a megoldást tartalmazza.

A három adatfolyam analytics feladatok és szintaxisáról mélység a [távoli figyelés előre beállított megoldás útmutató](iot-suite-remote-monitoring-sample-walkthrough.md)ismerteti. 

Feladatok közvetlenül a logika alter szerkesztése, vagy meghatározott logika hozzáadása a igényektől. Értékáram-elemzés feladatok megtalálhatja az alábbi képlettel történik:
 
1. [Azure portal](https://portal.azure.com)megnyitásához.
2. Nyissa meg a IoT megoldásként azonos nevű az erőforráscsoport. 
3. Jelölje ki a módosítani kívánt Azure Értékáram-elemzés feladatot. 
4. A feladat leállítása parancsok megadása **leállítása**gombra kattintva. 
5. Szerkessze a bemeneti adatok alapján, a lekérdezés és a kimeneti értékeket.

    Egy egyszerű módosítás azt javasoljuk, hogy a **szabályok** feladathoz használni a lekérdezés módosítása a **"<"** helyett egy **">"**. Továbbra is megjeleníti a megoldást portál **">"** amikor szabály szerkesztése, de észre fogja venni miatt az alapul szolgáló feladat változása tükrözött viselkedését.

6. A projekt indítása

> [AZURE.NOTE] A távoli felügyeleti Irányítópult, a feladatok megváltoztatása okozhat a meghiúsító az irányítópulton meghatározott adatokat, függ.

## <a name="adding-your-own-rules"></a>A saját szabályok hozzáadása

Azon túl az előre beállított Azure Értékáram-elemzés feladatok, az Azure portal segítségével adja hozzá az új feladatok, vagy vegye fel az új lekérdezésekre meglévő feladatokat.

## <a name="customizing-devices"></a>Eszközök testreszabása

A leggyakoribb bővítmény tevékenységek közül az igényektől jellemző eszközökkel működik. Vannak eszközök a többféle módszer áll rendelkezésére. Ezeket a módszereket terjed ki a megfelelő igényektől szimulált eszköz módosítása és a [IoT eszköz SDK][] használatával a fizikai eszközök csatlakoztatása a megoldást.

Lépésenkénti útmutató eszközök hozzáadása a távoli felügyeleti előre beállított megoldást olvassa el a [Iot csomagja csatlakozó eszközök](iot-suite-connecting-devices.md) és a [Távoli felügyeleti C SDK minta](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) célja, hogy a távoli felügyeleti előre beállított megoldást használata című témakört.

### <a name="creating-your-own-simulated-device"></a>A saját létrehozása szimulált eszköz

Távoli felügyeleti megoldás forráskódot (a fentiekben) szerepel, egy .NET simulatort használja. A simulatort használja az egyik részeként a megoldás kiépítése és is módosítani kell a különböző metaadatokat, telemetriai küldése vagy válasz különböző parancsokat.

Az előre beállított simulatort használja a távoli felügyeleti előre beállított megoldásban hőmérséklet és páratartalom telemetriai bocsát alacsonyabb eszközt módosíthatja a simulatort használja a [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) projekt, amikor már ágazik el a a GitHub tárat.

### <a name="available-locations-for-simulated-devices"></a>Szimulált ügyfélalkalmazásból elérhető helyek

Alapértelmezés szerint a helyek Seattle/Redmond, Washington, az Amerikai Egyesült Államok szerepel. Módosíthatja a következő helyeken és [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Összeállítását, és a saját (tényleges) eszközzel

Az [Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) a szükséges tárak számos eszköztípusok (nyelvek és operációs rendszerek) csatlakozás IoT megoldásokban.

## <a name="modifying-dashboard-limits"></a>Módosítás irányítópult korlátai

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Irányítópult legördülő menü jelenik meg az eszközök száma

Az alapértelmezett érték 200. Módosíthatja a [DashboardController.cs]ennyi[lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Bing-térkép vezérlőben megjelenítendő PIN-kódok száma

Az alapértelmezett érték 200. Módosíthatja a [TelemetryApiController.cs]ennyi[lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Telemetriai graph időtartam

Az alapértelmezett érték 10 perc. Lehet módosítani a [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Alkalmazás szerepkörök manuális beállítása

Az alábbi lépésekkel **rendszergazdája** és a **csak olvasható** alkalmazás szerepkörök hozzáadása egy előre konfigurált megoldás. Figyelje meg, hogy a azureiotsuite.com webhelyről már kiépítve előre definiált megoldások tartalmazzák a **rendszergazda** , a **csak olvasható** szerepkörök.

A **csak olvasható** szerepkör tagjai láthatja az irányítópulton és az eszközök listájában, de nem engedélyezettek eszközök hozzáadása, módosítása eszköz attribútumok és parancsok küldése.  A **rendszergazdai** szerepkör teljes hozzáférése van az összes funkciója a megoldás.

1. Nyissa meg az [Azure klasszikus portál][lnk-classic-portal].

2. Jelölje ki **az Active Directory**.

3. Kattintson a nevére a AAD bérlő, a megoldás kiépítése használt.

4. Kattintson az **alkalmazások**elemre.

5. Kattintson a nevére, az alkalmazás, amely az előre beállított megoldás. Ha nem látja az alkalmazást a listában, válassza az **alkalmazások vállalaton tulajdonosa** **megjelenítése** legördülő listája, és kattintson a pipára.

6.  A lap alján kattintson a **Cikkét kezelése** , majd **Töltse le a cikkét**.

7. A helyi számítógép zónában .json fájl letölti ezt.  Nyissa meg szerkesztésre szövegszerkesztőben lehetőség a fájlt.

8. A harmadik sorban .json fájl találja:

  ```
  "appRoles" : [],
  ```
  Cserélje ki a következőket:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Mentse a frissített .json fájlt (a meglévő fájl is felülírása).

10.  Az Azure felügyeleti portálon a lap alján jelölje ki a **Cikkét kezelése** , majd **Feltöltése cikkét** az előző lépésben mentett .json fájl feltöltése lehetőséget.

11. Most már hozzáadta a **csak olvasható** és a **rendszergazdai** szerepkör az alkalmazás.

12. A felhasználó a címtárban szeretne hozzárendelni az alábbi szerepkörök egyikét, lásd: az [engedélyek a webhelyen azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Visszajelzés

Van a Testreszabás kívánt lásd: a dokumentumban az érintett? Szolgáltatás javaslatok hozzáadása [Felhasználói hangos](https://feedback.azure.com/forums/321918-azure-iot)vagy ez a cikk az alábbi megjegyzést. 

## <a name="next-steps"></a>Következő lépések

További információ a lehetőségekről az előre definiált megoldások testreszabása című témakörben talál:

- [Csatlakozás a Azure IoT csomagja távoli figyelés előre megoldás logika alkalmazás][lnk-logicapp]
- [A távoli felügyeleti előre beállított megoldást dinamikus telemetriai használata][lnk-dynamic]
- [Távoli nyomon követése eszköz információk metaadatok előre megoldás][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[SDK IoT eszköz]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
