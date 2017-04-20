> [AZURE.SELECTOR]
- [C-on Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
- [C a Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
- [C-on mbed](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
- [NODE.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)

## <a name="scenario-overview"></a>Eset-áttekintés

Ebben az esetben létrehozhat egy eszköz, amely a következő távmérő küld a távoli [előre beállított megoldás]ellenőrzése[lnk-what-are-preconfig-solutions]:

- Külső hőmérséklet
- Belső hőmérséklete
- Nedvesség

Az egyszerűség kedvéért az eszközön a kódot generál példaértékek, de javasoljuk, hogy a minta valós érzékelők csatlakozik az eszközhöz, és elküldi a valós távmérő kiterjesztése.

Cikkben található lépések végrehajtásához szüksége van egy aktív Azure fiókra. Ha nem rendelkezik fiókkal, mindössze néhány perc is létrehozhat egy ingyenes próba fiók. Részletekért lásd a [Azure ingyenes próbaverzió][lnk-free-trial].

## <a name="before-you-start"></a>Mielőtt elkezdené

Kód írása a az eszközt, mielőtt a távoli felügyeleti előre beállított megoldás kiépítése, és majd az új egyéni eszközt, hogy a megoldás kiépítése.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Megoldás a távoli figyelés előre rendelkezés

Ez az oktatóanyag hoz létre az eszköz adatokat küld a [távoli figyelés] példánya[ lnk-remote-monitoring] előre megoldás. Ha még nem már kiépítése Azure fiókjában megoldás a távoli figyelés előre, kövesse az alábbi lépéseket:

1. Kattintson a <https://www.azureiotsuite.com/> lap **+** egy új megoldás létrehozásához.

2. Az új megoldás létrehozásához a **távoli figyelés** panelen kattintson a **Kijelölés** gombra.

3. A **felügyeleti megoldás létrehozása távoli** lapon írjon be egy tetszés szerinti **megoldás neve** , jelölje ki a telepíteni kívánt **terület** és válassza ki a használni kívánt Azure előfizetés. Kattintson a **Létrehozás megoldás**.

4. Várjon, amíg a kiépítési folyamat befejeződik.

> [AZURE.WARNING] Az előre konfigurált megoldások számlázható Azure szolgáltatások használhatók. Ügyeljen arra, hogy az előre beállított megoldás eltávolítása az előfizetésből, amikor elkészült vele, minden szükségtelen terhek elkerülése érdekében. Látogasson el a <https://www.azureiotsuite.com/> oldalra az előfizetésből teljesen eltávolítja egy előre konfigurált megoldás.

A távoli felügyeleti megoldás a kiépítési folyamat befejeződésekor kattintson **elindítani** a megoldás irányítópult megnyitásához a böngészőben.

![][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Az eszköz a távoli felügyeleti megoldás kiépítése

> [AZURE.NOTE] Ha az eszköz már a megoldás a fedezettel rendelkezik, ezt a lépést kihagyhatja. Tudja, hogy az eszköz hitelesítő adatok az ügyfélalkalmazás létrehozásakor kell.

Egy eszköznek az előre beállított oldat csatlakozni azt előbb azonosítania kell magát IoT hubhoz érvényes hitelesítő adatokkal. Az eszköz hitelesítő beolvasható a megoldás irányítópult. Az eszköz hitelesítő adatok bele a későbbi részében a tankönyv ügyfélalkalmazás. 

Új eszköz hozzáadása a távoli felügyeleti megoldás, hajtsa végre az alábbi lépéseket a megoldás irányítópult:

1.  Kattintson az irányítópult bal alsó sarkában az **eszköz hozzáadása**.

    ![][1]

2.  **Egyéni eszközök** lapján kattintson a **Hozzáadás gombra új**.

    ![][2]

3.  Válassza ki az **én saját azonosító megadása**, például a **mydevice**eszköz azonosító beírásához, ellenőrizze a megadott néven már nem használja, és kattintson a **Create** kiépítése az eszköz **Azonosító ellenőrzése** gombra.

    ![][3]

5. Jegyezze fel az eszköz hitelesítő adatok (Eszközazonosító, IoT Hub állomásnév és eszköz kulcs), az ügyfél alkalmazás szüksége van rájuk, kapcsolódni a távoli felügyeleti megoldás. Majd kattintson a **kész**gombra.

    ![][4]

6. Ellenőrizze, hogy az eszköz az eszközök részben jeleníti meg. Az eszköz állapota **függőben** , mindaddig, amíg az eszköz kapcsolatot hoz létre a távoli felügyeleti megoldás.

    ![][5]

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png
[5]: ./media/iot-suite-selector-connecting/suite5.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/