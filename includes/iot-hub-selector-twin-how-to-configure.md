> [AZURE.SELECTOR]
- [NODE.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)

## <a name="introduction"></a>Bevezetés

[IoT Hub twins Ismerkedés]a[lnk-twin-tutorial], megtanult eszköz meta-adatok beállítása a megoldás hátsó végétől, *címkék*, jelentés eszköz feltételek egy eszköz alkalmazásból *jelentett tulajdonságok*segítségével, és ezeket az adatokat egy SQL-szerű nyelv lekérdezéséhez.

Ebben az oktatóanyagban megtanulhatja, hogyan használható az a kettős *kívánt tulajdonságok* *Tulajdonságok jelentett*, távoli konfigurálására eszköz apps együtt. Pontosabban Ez az oktatóanyag azt mutatja, hogyan twin által jelentett és kívánt tulajdonságok egy többlépcsős konfigurációs eszköz alkalmazás beállítás engedélyezése, és adja meg a művelet állapotát a megoldás háttér látják keresztül minden eszköz.

Magas szinten ez az oktatóanyag Eszközkezelés *kívánt állapotot mintát* követi. Ezt a mintát alapvető lényege, ahhoz, hogy a megoldás háttér adja meg a kívánt parancsok elküldése, a kezelt eszközök állapotát. Ez az eszköz felelős a legegyszerűbben elérni a kívánt állapotot (nagyon fontos, ahol eszközre feltételek azonnal adott parancsok végrehajtására hatással IoT esetekben) létrehozó helyezi közben folyamatosan jelentése a háttér az aktuális állapotát és a potenciális hibaállapotokat. A kívánt állapotot minta lehetővé teszi, hogy a háttér ahhoz, hogy a konfigurációs folyamat állapotának teljes láthatóságát keresztül minden eszköz megegyezik műszeres eszközök, nagy mennyiségű kezelésére.
Bővebb információt a szerepkör a kívánt állapotot minta található Eszközkezelés [áttekintése az Azure IoT Hub-eszköz kezelése]a[lnk-dm-overview].

> [AZURE.NOTE] Ha több interaktív módon (a felhasználó által szabályozott alkalmazásból a ventilátor bekapcsolása) eszközök szabályozott esetekben fontolja meg a [felhő-eszköz]módszerekkel[lnk-methods].

Ebben az oktatóanyagban az alkalmazás háttérkiszolgálón módosításait a távmérő konfiguráció egy céleszköz és alapján, és amelyek az eszköz app követi egy többlépcsős folyamat alkalmazása a konfigurációs frissítésben (pl. a számítógép újraindítására szoftver modul), egy egyszerű késéssel szimulálja a tankönyv).

Háttér-tárolja a konfiguráció a két eszköz kívánt tulajdonságok a következő módon:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [AZURE.NOTE] Konfigurációk lehet összetett objektumok, mivel ezek általában egyedi azonosítók kapják (kivonatok vagy [GUID][lnk-guid]) egyszerűsítése érdekében ezek az összehasonlítások.

Az eszköz app jelentések jelentett tulajdonságait a kívánt tulajdonság **telemetryConfig** tükrözése a jelenlegi konfiguráció:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Fontos tudni, hogyan a jelzett **telemetryConfig** van egy további tulajdonság **állapotát**, a konfiguráció frissítési folyamat állapotának jelentéséhez használt.

Új kívánt konfiguráció fogadásakor az eszköz alkalmazás függőben lévő konfigurációs adatok módosításával jelentések:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Majd egy későbbi időpontban az eszköz alkalmazás jelentést küld a hiba a művelet sikeres a fenti tulajdonság frissítésével.
Megjegyzés: hogyan a háttér tudja, bármikor, a konfigurációs folyamat állapotának lekérdezése az eszközök között.

Ez az oktatóanyag bemutatja, hogyan kell:

- Hozzon létre egy szimulált eszköz konfigurációs frissítéseket kap a háttér és a jelentések több frissítés *jelentett* tulajdonságainak a konfiguráció frissítési folyamat.
- Hozzon létre egy háttér-alkalmazás, amely frissíti az eszköz kívánt beállításait, majd lekérdezi a konfiguráció frissítési folyamat.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier