<properties
  pageTitle="Azure IoT csomagja és összefüggés-alkalmazások |} Microsoft Azure"
  description="Hogyan kell az üzleti folyamat Azure IoT csomagja logika alkalmazások bekötése oktatóanyagot."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Oktatóprogram: Logika alkalmazás csatlakoztatása a Azure IoT csomagja távoli figyelés előre megoldás

A [Microsoft Azure IoT Suite] [ lnk-internetofthings] távoli előre beállított megoldás figyelés van – első lépések gyorsan egy végpontok közötti szolgáltatáskészlet, amely egy IoT megoldás exemplifies kiválóan használhatók. Ebben az oktatóanyagban végigvezeti logika alkalmazás hozzáadása a Microsoft Azure IoT Suite távoli figyelés előre beállított megoldás. Ezek a lépések bemutatják, hogyan készíthet a IoT megoldás még tovább üzleti folyamatok útján.

_Ha egy távoli figyelés előre megoldás kiépítése ismertető útmutató keres, olvassa el a [oktatóprogram: első lépések az előre beállított IoT megoldások][lnk-getstarted]._

Ebben az oktatóanyagban megkezdése előtt a következőket kell tennie:

- A távoli nyomon az Azure előfizetés előre beállított megoldás kiépítése.

- Hozzon létre egy SendGrid fiókot ahhoz, hogy az üzleti folyamat kiváltó e-mailt küldhet. Iratkozzon fel egy ingyenes próbaverzió fiókra, [SendGrid](https://sendgrid.com/) **próbálja meg az ingyenes**gombra kattintva. Ingyenes próbaverzió fiókjának regisztráció után kell [API-ja kulcs](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) létrehozása az e-mail küldése engedélyt, SendGrid. A API kulcs belül az oktatóprogram szüksége.

Feltételezve, hogy már kiépítve a távoli figyelés előre megoldást, nyissa meg azt, hogy a megoldás az [Azure portál]az erőforráscsoport[lnk-azureportal]. Az erőforráscsoport megoldás neveként nevével megegyező nevű úgy döntött, amikor a távoli felügyeleti megoldás kiépítése-e. Az erőforráscsoport megjelenik a megoldás, kivéve az Azure Active Directory-alkalmazás az Azure klasszikus portálon megtalálható összes kiépített Azure erőforrás. Az alábbi képernyőképen egy példa **erőforráscsoport** lap távoli felügyeleti előre beállított megoldás jeleníti meg:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

A kezdéshez a logika alkalmazás beállítása az előre beállított megoldás használatára.

## <a name="set-up-the-logic-app"></a>A logikai alkalmazás beállítása

1. Kattintson a __Hozzáadás__ az Azure-portálon az erőforrás csoport a lap tetején.

2. __Logika alkalmazás__keresése, jelölje ki azt, és kattintson a **Létrehozás**gombra.

3. Töltse ki a __nevét__ , és **előfizetést** és az **erőforráscsoport** meg a távoli felügyeleti megoldás kiépítése használt. Kattintson a __létrehozása__gombra.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. A telepítés befejezése után megjelenik a logika alkalmazás szerepel a listában az erőforráscsoport erőforrásként.

5. Kattintson a logika alkalmazás, nyissa meg azt a logika alkalmazás lap, és jelölje ki az **üres logika** alkalmazássablon **Logika alkalmazások Designer**megnyitásához.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Jelölje ki a __kérése__. Ez a művelet Itt adhatja meg, hogy egy adott JSON a bejövő HTTP-kérelem formátumú tartalom jogszabályok eseményindító.

7. A szervezet JSON kérése séma illessze be a következő:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] A logika alkalmazás menti, de a művelet hozzáadása először másolhatja a HTTP-bejegyzés URL-CÍMÉT.

8. Kattintson az __+ új lépést__ a kézi eseményindító alatt. Kattintson a **Hozzáadás művelet**gombra.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Keresse meg a **SendGrid - küldhet e-mailt** , és kattintson rá.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Írjon be egy nevet a kapcsolathoz, például **SendGridConnection**, írja be a **SendGrid API -ja kulcs** létrehozott SendGrid fiókját beállítva, és kattintson a **Létrehozás**gombra.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. Adja meg az **a** és **a** mezőt a saját e-mail címét. Adja hozzá a **távoli értesítés [DeviceId] figyelése** a **Tárgy** mezőbe. Az **E-mail törzsében** mezőben adja hozzá a **eszköz [DeviceId] [measurementName] [measuredValue] érték a jelentett.** **Az előző lépéseket adatok beszúrhat** szakaszában kattintson a **[DeviceId]**, **[measurementName]**és **[measuredValue]** hozzáadhat.

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. A felső menüben kattintson a __Mentés__ gombra.

13. Kattintson a **kérelem** eseményindító, és másolja a vágólapra __Az URL-címet a Http-bejegyzés__ értékét. Ez az URL az oktatóprogram később szüksége.

> [AZURE.NOTE] Logika Apps lehetővé teszi a [különböző típusú művelet] futtatása[ lnk-logic-apps-actions] többek között a műveletek az Office 365-ben. 

## <a name="set-up-the-eventprocessor-web-job"></a>A webes EventProcessor feladat beállítása

Ez a szakasz kapcsolódni a előre beállított megoldás a létrehozott érték alkalmazásba. A feladat végrehajtásához az URL-cím indíthatja el a logika a műveletet, amely akkor indul el, ha az eszköz érzékelő érték eléri az alkalmazás hozzáadása.

1. A mely számjegy-ügyfél használata klónozhatja a legújabb a [azure-iot-távoli-ellenőrző github tárházba][lnk-rmgithub]. Példa:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. A Visual Studióban nyissa meg a tárat a helyi példányt az __RemoteMonitoring.sln__ .

3. Nyissa meg a __ActionRepository.cs__ fájlt a **infrastruktúra\\tárházba** mappát.

4. Frissítse a **actionIds** szótár a __Http Post az URL-címet__ a következőképpen jegyezni az összefüggés-alkalmazás:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Megoldás a módosítások mentéséhez, és kilépés a Visual Studio.

## <a name="deploy-from-the-command-line"></a>A parancssorból terjesztése

Ebben a szakaszban a távoli felügyeleti megoldás az Azure-ban futó verzió cseréje a frissített verziójának telepítése.

1. A [fejlesztők beállítási] követő[ lnk-devsetup] képernyőn megjelenő utasításokat követve állítsa be a telepítéshez környezetben.

2.  Helyi meghajtóra üzembe helyezéséhez kövesse a [helyi telepítési] [ lnk-localdeploy] utasításokat.

3.  A felhőbe telepíthető, és a meglévő cloud-telepítés frissítése, hajtsa végre az a [felhő telepítési] [ lnk-clouddeploy] utasításokat. Az eredeti telepítő nevét használja az üzembe nevet. Példa az eredeti telepítő hívták **demologicapp**, ha a következő parancs használatával:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Az összeállítás parancsfájl fut, amikor biztos abban, hogy a azonos Azure-fiók, a előfizetés, a régió, valamint az Active Directory-példány, használ, a kiépítéstől a megoldást.

## <a name="see-your-logic-app-in-action"></a>Lásd: a művelet logika alkalmazását

A távoli felügyeleti előre beállított megoldást két szabályok beállítása alapértelmezés szerint, amikor Ön a megoldás kiépítése tartalmaz. Mindkét szabályok **SampleDevice001** az eszközre a következők:

* Hőmérsékleti > 38.00
* Páratartalom > 48.00

A hőmérsékleti szabály elindítja a **Előléptetése riasztás** műveletet, és a páratartalom szabály elindítja a **SendMessage** műveletet. Feltételezve, hogy mindkét műveletek a **ActionRepository** osztály használt azonos URL-CÍMÉT, a logika alkalmazás elindítja a szabály. Mindkét szabályok SendGrid segítségével e-mail küldése **a címet a figyelmeztetés adataival** .

> [AZURE.NOTE] A logikai alkalmazás továbbra is fennáll, minden alkalommal teljesül a küszöbértékét indíthatja el. Szükségtelen e-mailek elkerülése érdekében vagy letilthatja a szabályok a megoldást portálon vagy letilthatja az logika alkalmazást az [Azure portál][lnk-azureportal].

Kap e-maileket, mellett is megjelenik a logika alkalmazás futtatásakor a portálon:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Következő lépések

Most, hogy egy logikai alkalmazást használta az előre beállított megoldás csatlakozhat az üzleti folyamatok, többet is megtudhat az előre definiált megoldások testreszabásának lehetőségei:

- [A távoli felügyeleti előre beállított megoldást dinamikus telemetriai használata][lnk-dynamic]
- [Távoli nyomon követése eszköz információk metaadatok előre megoldás][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
