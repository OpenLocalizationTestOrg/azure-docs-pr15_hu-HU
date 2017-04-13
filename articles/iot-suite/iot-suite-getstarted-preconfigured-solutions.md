<properties
    pageTitle="Első lépések az előre definiált megoldások |} Microsoft Azure"
    description="Kövesse az ebben az oktatóprogramban egy előre Azure IoT csomagja megoldást üzembe helyez című témakörből."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Oktatóprogram: Az előre definiált megoldások – első lépések

## <a name="introduction"></a>– Bevezetés

Azure IoT csomagja [előre definiált megoldások] [ lnk-preconfigured-solutions] egyesítheti több Azure IoT szolgáltatásokat, amelyek a közös IoT vállalati verzió felhasználási területei megvalósítása végpont megoldásokat. A, *távoli figyelés* előre megoldást csatlakozik, és figyeli a eszközén. A megoldást is használhatja, az adatfolyam készülékekről adatok elemzése és üzleti eredmények javítása azáltal, hogy a folyamatok, hogy az adatok adatfolyam automatikusan válaszolni.

Ebből az oktatóanyagból megtudhatja, hogy hogyan a távoli felügyeleti előre beállított megoldás kiépítése. Azt is végigvezeti a távoli felügyeleti megoldást főbb tulajdonságait. Ezek a funkciók közül számos együtt az előre beállított megoldást üzembe helyezése a megoldást irányítópult keresztül érheti el:

![Távoli figyelés előre megoldás irányítópult][img-dashboard]

Ebben az oktatóanyagban befejezéséhez szüksége van egy aktív Azure-előfizetést.

> [AZURE.NOTE]  Nem rendelkeznek fiókkal, ha mindössze néhány perc is létrehozhat ingyenes próba-fiók. A részletekért olvassa [Azure ingyenes próbaverziót][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>A megoldás irányítópult megtekintése

A megoldás irányítópult lehetővé teszi a telepített megoldást kezelése. Például telemetriai megtekintése, hozzáadása eszközök, és szabályok beállítása.

1.  Amikor befejeződött a kiépítési, és a előre beállított megoldás a csempe jelzi **készen áll**, kattintson a **Indítsa el** a távoli felügyeleti portál megoldás megnyitása egy új lapon.

    ![Indítsa el az előre beállított megoldás][img-launch-solution]

2.  Alapértelmezés szerint a megoldást portál mutatja az *Irányítópult megoldás*. Egyéb nézetek, a bal oldali menü segítségével lehet kiválasztani

    ![Távoli figyelés előre megoldás irányítópult][img-dashboard]

Az irányítópult jeleníti meg az alábbi adatokat:

- A térkép jeleníti meg a megoldást csatlakoztatott minden eszközhöz helyét. Amikor először futtatja a megoldást, négy szimulált olyan eszköz van. Azure WebJobs megvalósítása szimulált eszközök, és a megoldás a Bing Maps API segítségével adatokat a térképen ábrázolni.
- A **Telemetriai előzmények** ablaktábla rajzolja páratartalom és hőmérsékleti telemetriai egy kijelölt eszközről a valós idejű és jeleníti meg a maximális, minimum és átlag páratartalom például összesített adatok közelében.
- A **Riasztási előzmények** ablaktábla a legutóbbi riasztás események telemetriai érték küszöbértéket túllépésekor jeleníti meg. A példák az előre beállított megoldás által létrehozott kívül saját riasztások határozhatja meg.

## <a name="view-the-device-list"></a>Az eszköz listájának megtekintése

Az eszközök listája az bejegyzett eszközök látható a megoldást. Megtekintése és eszköz metaadatainak szerkesztése, hozzáadása vagy eltávolítása az eszközök és elküldése parancsok az eszközök.

1.  Kattintson az **eszközök** kattintva jelenítse meg az *eszközök listájában* a megoldás a bal oldali menüben.

    ![Eszközök listája az irányítópulton][img-devicelist]

2.  Az eszközök listája látható, hogy nincsenek-e négy szimulált eszközök hozta létre a kiépítési folyamat.

3.  Kattintson az eszköz részleteinek megtekintéséhez eszköz listában egy eszközt.

    ![Eszköz részletei az irányítópulton][img-devicedetails]

Az **Eszköz részletei** panelen három szakaszt tartalmaz:

- A **Műveletek** a szakasz a műveleteket végezheti el az eszközön. Ha letiltja az eszközt, már nem engedélyezett telemetriai küldéséhez és fogadásához a parancsok. Ha letiltja egy eszközt, majd engedélyezheti, újra. Az eszköz kiváltó-riasztás, amikor egy telemetriai értéke meghaladja a küszöbértéket társított szabály is hozzáadhat. Parancsok is küldhet egy eszközt. Amikor először csatlakozik egy eszközt, azt közli a megoldást reagálni tud a parancsok.
- Az **Eszköz tulajdonságainak** szakasz eszköz metaadatokat. A metaadatok részét származik, magát az eszköz (például a gyártó), és néhány generálja a megoldást (például a létrehozott idő). Az eszköz metaadatok innen szerkesztheti.
- A **Hitelesítési kulcsokat** szakaszban az eszköz segítségével hitelesíteni a megoldást billentyűk listája.

## <a name="send-a-command-to-a-device"></a>Parancs küld egy eszköz

Eszköz jobb oldali ablaktáblában látható a parancsok, amely támogatja az egy adott eszköz és lehetővé teszi, hogy küldje el a parancsok eszközre. Amikor először indítja el egy eszközt, a parancsról támogat adatokat küld a megoldást.

1.  Kattintson a részletek ablaktáblán eszköz a kijelölt eszköz **parancsok** elemre.

    ![Eszköz parancsok az irányítópulton][img-devicecommands]

2.  Válassza ki a parancslista **PingDevice** .

3.  Kattintson a **Küldés parancsot**.

4.  A parancs a parancs előzmények állapotát tekintheti meg.

    ![Az irányítópult parancs állapota][img-pingcommand]

A megoldás nyomon követi a minden parancs, küldjön állapotát. Az eredmény az eredetileg **folyamatban**. Az eszköz jelentések azt teljesítette a parancsot, ha az eredményhalmaz **sikeres**.

## <a name="add-a-new-simulated-device"></a>Szimulált új eszköz hozzáadása

Amikor az előre beállított megoldást üzembe helyez, amikor megjelenik az eszköz listában négy példa eszközök automatikusan kiépítése. Ezekre az eszközökre *szimulált eszközök* fut az Azure WebJob. Szimulált eszközök megkönnyítik a az előre beállított megoldás nincs szükség telepíteni valós, fizikai eszközök kísérletezhet. Ha szeretné, hogy a megoldás egy valós eszközt csatlakozni, olvassa el a [Csatlakozás eszközét, hogy a távoli figyelése előre beállított megoldás] [ lnk-connect-rm] oktatóprogram.

A következő lépések bemutatják a megoldás egy szimulált eszköz hozzáadása:

1.  Lépjen vissza az eszköz listából.

2.  Válassza a **+ hozzáadása eszköz** eszköz hozzáadása bal alsó sarokban.

    ![Az előre beállított megoldás eszköz hozzáadása][img-adddevice]

3.  Kattintson a **Hozzáadás** gombra az **Eszköz szimulált** hivatkozásra.

    ![Új eszköz részletei beállítása az irányítópulton][img-addnew]
    
    Új szimulált eszköz létrehozásán is felveheti a fizikai eszközök Ha úgy dönt, hogy hozzon létre egy **Egyéni eszköz**. Fizikai eszközök kapcsolódás a megoldást további információért olvassa el a [Csatlakozás eszközét, hogy a távoli előre beállított megoldás figyelés IoT programcsomag][lnk-connect-rm].

4.  Jelölje ki, **én szeretném határozza meg a saját Eszközazonosítót**, és adjon meg egy eszköz egyedi azonosító neve, például **mydevice_01**.

5.  Kattintson a **létrehozása**gombra.

    ![Új eszköz mentése][img-definedevice]

6. A **szimulált eszköz hozzáadása**3 kattintson a **Kész gombra** kattintva térhet vissza az eszköz listából.

7. Az eszköz **futtatása** megtekintheti az eszköz listában.

    ![Új eszköz nézet eszközök listája][img-runningnew]

8. Az irányítópulton az új eszközről is megtekintheti a szimulált telemetriai:

    ![Nézet telemetriai új eszközről][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Az eszköz metaadatainak szerkesztése

Amikor először kapcsolódik egy eszközt a megoldást, a metaadatait küld a megoldást. Az eszköz metaadat-alapú megoldást irányítópult keresztül szerkesztésekor új a metaadatok értékét elküldése az eszközre, és a megoldás DocumentDB adatbázisban tárolja az új értékeket. További tudnivalókért lásd: az [eszköz identitás beállításjegyzék és DocumentDB][lnk-devicemetadata].

1.  Lépjen vissza az eszköz listából.

2.  **Eszközök listában**válassza az új eszközre, és kattintson a **Szerkesztés** **Eszköz tulajdonságainak**szerkesztése:

    ![Eszköz metaadatainak szerkesztése][img-editdevice]

3. Görgessen le, és a szélesség és hosszúság vales lehet módosítani. Kattintson a **eszköz beállításjegyzék módosításainak mentéséhez**.

    ![Eszköz metaadatainak szerkesztése][img-editdevice2]

4. Lépjen vissza az irányítópulton, helyét eszközről megváltozott a térképen:

    ![Eszköz metaadatainak szerkesztése][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Az új eszközre vonatkozó szabály hozzáadása

Nincsenek közvetlenül a felvétele után az új eszközre vonatkozó szabályok. Ebben a részben vesz fel, hogy elindítja-riasztás, ha a hőmérsékleti az új eszköz által jelzett meghaladja 47 fok szabályt. Mielőtt nekikezdene, figyelje meg, hogy az új eszközre az irányítópulton telemetriai előzményeit jeleníti meg az eszköz hőmérsékleti soha nem meghaladja 45 fok.

1.  Lépjen vissza az eszköz listából.

2.  **Eszközök listában**válassza az új eszközre, és válassza a **szabály hozzáadása** az eszköz szabály hozzáadásához.

3. Hozzon létre egy szabályt, amely a **hőmérsékleti** használja, mint az adatmező és **AlarmTemp** használja a kimenet, ha a hőmérsékleti meghaladja 47 fok:

    ![Eszköz szabály hozzáadása][img-adddevicerule]

4. Kattintson a **Mentés és a szabályok megtekintése** elemre a módosítások mentéséhez.

5.  Az új eszközre eszköz ablaktáblában kattintson a **parancsok** elemre.

    ![Eszköz szabály hozzáadása][img-adddevicerule2]

6.  Jelölje ki a parancs **ChangeSetPointTemp** , és 45 **SetPointTemp** értékűre. Kattintson a **Küldés parancsot**:

    ![Eszköz szabály hozzáadása][img-adddevicerule3]

7.  Lépjen vissza az megoldás irányítópulton. Egy rövid idő múlva jelenik a **Riasztás előzmények** ablaktáblában egy új bejegyzés a hőmérsékleti az új eszköz által jelzett meghaladja 47 fokos:

    ![Eszköz szabály hozzáadása][img-adddevicerule4]

8. Tekintse át, és az irányítópult **szabályok** lap összes szabály szerkesztése:

    ![Lista eszköz szabályok][img-rules]

9. Tekintse át, és a műveleteket, amelyek a **Műveletek** lapon az irányítópult szabály válaszul tehetők szerkesztése:

    ![Listaműveletek eszköz][img-actions]

> [AZURE.NOTE] Lehetséges, amely szabály válasz e-mail üzenethez vagy SMS küldése vagy a vállalati verzió rendszer keresztül egy [Logikai alkalmazás]integrálása műveletek megadása[lnk-logic-apps]. További tudnivalókért lásd: a [logika alkalmazás csatlakoztatása az Azure IoT csomagja távoli figyelés előre megoldás][lnk-logicapptutorial].

## <a name="other-features"></a>Egyéb funkciók

A megoldás portál kereshet használatának eszközök, például modell szám adott tulajdonságokkal:

![A keresési eszközök][img-search]

Egy eszközt letilthatja, és miután le van tiltva, eltávolíthatja azt:

![Letiltása és eltávolítása egy eszközt][img-disable]

## <a name="behind-the-scenes"></a>Bepillantás a színfalak mögé

Amikor egy előre konfigurált megoldást üzembe helyez, amikor a telepítési folyamatot több erőforrások az Azure előfizetés a kijelölt hoz létre. Ezek az erőforrások megtekintheti az Azure- [portálon][lnk-portal]. A telepítési folyamatot hoz létre **erőforráscsoport** a nevét, választhat az előre beállított megoldás alapján néven:

![Előre beállított megoldás az Azure-portálon][img-portal]

Megtekintheti a beállításokat az egyes erőforrások szeretne, jelölje ki a listában az erőforrások erőforráscsoport szerinti.

Az előre beállított megoldás forráskódot is megtekintheti. A távoli felügyeleti előre beállított megoldás forráskód szerepel-e az [azure-iot-távoli-ellenőrző] [ lnk-rmgithub] GitHub tárházba:

- A **DeviceAdministration** mappát az irányítópult forráskódot tartalmazza.
- A **simulatort használja** mappát a szimulált eszközön forráskódot tartalmazza.
- A **EventProcessor** mappát a háttéradatbázist folyamat, amely kezelje a bejövő telemetriai forráskódot tartalmazza.

Ha végzett, törölheti a [azureiotsuite.com] Azure előfizetésből az előre beállított megoldás[ lnk-azureiotsuite] webhelyet. A webhely lehetővé teszi az előre definiált megoldások létrehozása után azok kiépítve összes erőforrás könnyen törölheti.

> [AZURE.NOTE] Annak érdekében, hogy minden, az előre beállított megoldás kapcsolatos törlése, törölheti a [azureiotsuite.com] a[ lnk-azureiotsuite] webhelyen, és ne egyszerűen törölje az erőforráscsoport a portálon.

## <a name="next-steps"></a>Következő lépések

Most, hogy egy előre beállított munkaidő megoldás van telepítve, továbbra is az alábbi cikkekben olvasásával IoT csomagja első lépések:

- [Távoli figyelés előre megoldás útmutató][lnk-rm-walkthrough]
- [Csatlakoztassa az eszközt a távoli felügyeleti előre beállított megoldás][lnk-connect-rm]
- [A azureiotsuite.com webhely engedélyei][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
