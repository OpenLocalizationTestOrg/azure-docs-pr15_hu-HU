> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
- [A Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)

Ez a forgatókönyv a [szimulált eszköz felhő feltöltése minta] szemlélteti, hogyan lehet használni az [Azure IoT átjáró SDK] [ lnk-sdk] IoT Hub a szimulált eszközök eszköz felhő távmérő küldhet.

A forgatókönyv leírja:

1. **Architektúra**: a szimulált eszköz felhő feltöltése minta építészeti fontos információkat.

2. **Build és futtatja**: fordításához és futtatásához a minta szükséges lépéseket.

## <a name="architecture"></a>Architektúra

A szimulált eszköz felhő feltöltése minta szemlélteti, hogyan lehet az SDK segítségével hozzon létre egy szimulált eszközök távmérő küld egy IoT hub átjárónál. A szimulált eszközök nem tudnak kapcsolódni közvetlenül IoT Hub mert:

- Az eszközök ne használjon érthető IoT Hub kommunikációs protokoll.
- Az eszközök nincsenek elég okos ne felejtse el a IoT Hub által hozzájuk rendelt azonosító.

Az átjáró megoldja ezeket a problémákat a szimulált eszközök a következők szerint:

- Az átjáró a szimulált eszközök által használt protokoll eszköz felhő távmérő kap az eszközöket, és azokat az üzeneteket továbbítja az elosztó által értelmezhető protokollon keresztül IoT Hub értelmezése.
- Az átjáró IoT Hub identitások tárolja a szimulált eszközök nevében és proxyként működik, ha a szimulált eszközök IoT Hub üzeneteket küldeni.

A következő ábrán a mintát, beleértve a gateway modul fő összetevői:

![][1]


> [AZURE.NOTE] A modulok nem haladhatnak üzenetek egymással. A modulok egy belső broker, amely kézbesíti az üzeneteket az alábbi ábrán látható módon egy előfizetés mechanizmus használatával más modulok üzenetek közzététele. További tudnivalók: [Ismerkedés a IoT átjáró SDK][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Jegyzőkönyv a lenyelés modul

Ez a modul az adatokat szolgáltató eszközök, az átjárón keresztül, és a felhő kiindulópontját. A mintában a modul négy feladatokat hajtja végre:

1.  Szimulált hőmérsékleti adatokat hoz létre.
    
    Megjegyzés: Ha valódi eszközök, a modul volna adatainak beolvasása e fizikai eszközöket.

2.  A szimulált hőmérsékleti adatokat helyezi az üzenet tartalmát.

3.  A tulajdonság hamis MAC-címet ad a szimulált hőmérsékleti adatokat tartalmazó üzenetet.

4.  Azt elérhetővé az üzenet a következő modul a lánc.

> [AZURE.NOTE] A modul nevű **protokoll X lenyelés** a diagramban nevezik **eszköz szimulált** a forráskód.

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt; - &gt; IoT Hub azonosító modul

Ez a modul egy tulajdonságot, amely tartalmazza a MAC-cím, a szimulált eszközön a lenyelés protokoll modul által felvett üzeneteket keres. A modul megkeresi a tulajdonságot, ha egy másik tulajdonságot hozzáadja az üzenet IoT Hub eszközt kulccsal, és majd elérhetővé teszi az üzenet a következő modul a lánc. Ez a módja a minta társítja IoT Hub eszközt identitások szimulált eszközök. A fejlesztő állítja be kézzel a modul konfiguráció részeként a MAC-címek és a IoT Hub identitások közötti leképezéseket. 

> [AZURE.NOTE]  Ez a minta MAC-címet használja, mint egy egyedi eszköz azonosító, és azt korrelációban IoT Hub eszközt azonosító. Egy másik egyedi azonosítót használó saját modul is írhat. Előfordulhat például, egyedi sorozatszámot vagy egy egyedi eszköz neve, amely segítségével IoT Hub eszközt kilétét beágyazott távmérő adatokat.

### <a name="iot-hub-communication-module"></a>IoT Hub kommunikációs modul

Ez a modul üzenetek tart egy IoT hubbal a korábbi modul által hozzárendelt eszköz azonosságát, és elküldi az üzenet tartalmának IoT Hub HTTP protokollon keresztül. HTTP IoT Hub által érthető a három protokollok egyike.

IoT Hub kapcsolatot az egyes szimulált eszközök, hanem ez a modul egyetlen HTTP kapcsolaton keresztül nyitja meg az átjáró a IoT hub és a szimulált eszközök kapcsolódásának multiplexes adott kapcsolaton keresztül. Ez lehetővé teszi, hogy számos további eszközök csatlakoztatása, szimulált vagy más módon nem lenne lehetséges, ha azt megnyitni egy egyedi kapcsolatot minden eszközhöz, mint egy átjáró.

![][2]


<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Szimulált eszköz felhő feltöltése minta]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/sample_simulated_device_cloud_upload.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md