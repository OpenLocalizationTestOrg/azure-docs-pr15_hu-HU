## <a name="view-device-telemetry-in-the-dashboard"></a>Az irányítópult nézetben eszköz távmérő

Az irányítópult a távoli felügyeleti megoldás lehetővé teszi a távmérő, amelyek az eszközök IoT Hub küldik meg.

1. A böngésző vissza a távoli felügyeleti megoldás irányítópult, a bal oldali panelen keresse meg az **eszközök listájában**kattintson az **eszközök** elemre.

2. Az **eszközök listája**megjelenik, hogy az eszköz állapota most **futó**.

    ![][18]

3. Kattintson az **Irányítópult** térjen vissza az irányítópult, a **nézet eszköz** legördülő a távmérő megtekintéséhez jelölje ki az eszközt. A minta alkalmazásából távmérő 50 egység belső hőmérsékletét, 55 egységek külső hőmérséklet és páratartalom 50 egység. Vegye figyelembe, hogy alapértelmezés szerint az irányítópult csak a hőmérséklet és páratartalom értékeket mutatja.

    ![][img-telemetry]

## <a name="send-a-command-to-your-device"></a>Parancsot küld az eszköz

Az irányítópult a távoli felügyeleti megoldás lehetővé teszi az eszközök IoT hubon keresztül parancsokat küldhet. Például a távoli felügyeleti megoldás elküldheti az eszköz belső hőmérséklet beállítása parancsot.

1. A távoli felügyeleti megoldás irányítópult kattintson a bal oldali panelen keresse meg az **eszközök listájában** **eszközök** .

2. Az eszközt az **Eszközök listában**kattintson az **Eszközazonosító** .

3. Az **eszköz részletek** panelen kattintson a **parancsok**.

    ![][13]

4. A **parancs** legördülő válassza ki a **SetTemperature**, és majd írjon be egy új hőmérséklet értéket **hőmérséklet** . Kattintson a **Küldés parancs** a parancsot küld az eszköznek.

    ![][14]

    > [AZURE.NOTE] A parancselőzmények kezdetben **lévőként**parancs állapotát mutatja. Ha az eszköz elismeri a parancsot, az állapot **sikeres**változik.

5. Az irányítópult ellenőrizze, hogy az eszköz most küld 75 új hőmérsékleti érték.

## <a name="next-steps"></a>A következő lépések

A cikk [testreszabása megoldások előre] [ lnk-customize] ismerteti néhány bővítheti ezt a mintát. Lehetséges bővítmények segítségével valós érzékelők és további parancsok végrehajtása.

Az [engedélyeket a azureiotsuite.com webhelyen]többet is megtudhatnak[lnk-permissions].

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
