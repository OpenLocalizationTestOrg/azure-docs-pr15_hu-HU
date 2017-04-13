<properties
     pageTitle="Az Azure portal segítségével hozzon létre egy IoT központi |} Microsoft Azure"
     description="Megtudhatja, hogy miként hozhat létre és kezelhet Azure IoT hubok az Azure portálon keresztül"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Hozzon létre egy IoT hubhoz az Azure portál használatával

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>– Bevezetés

Ez a cikk ismerteti az Azure-portálon keresheti meg a IoT központi szolgáltatást, és hogyan hozhat létre és kezelhet IoT hubok.

## <a name="where-to-find-iot-hubs"></a>Hol találhatók a IoT hubok

Vannak olyan hol találhatók a IoT hubok különböző helyeken.

1. **+ Új**: **Azure IoT központi** IoT szolgáltatás, és kategóriában **Az Internet a dolgokat**, a **+ Új**, egyéb szolgáltatásokhoz hasonló találhatók.

2. IoT hubok is érhetők el a piactér **Internetes a dolog, amit**a fő szolgáltatásként.

## <a name="create-an-iot-hub"></a>Hozzon létre egy IoT központi

Egy IoT központban az alábbi módszerekkel hozhatók létre:

- A felvétel a következő képernyőn látható lap vezet létrehozása egy IoT hubhoz keresztül a **+ Új** lehetőséget. A lépéseket a IoT központi létrehozásához, ez a módszer és a piactér révén megegyeznek.

- A piactér keresztül egy IoT hubhoz létrehozása: **létrehozása** gombra kattintva megnyílik egy lap, amely megegyezik az előző lap az **+ Új** használat. A következő szakaszokban a több lépései egy IoT központi létrehozása.

### <a name="choose-the-name-of-the-iot-hub"></a>Válassza ki a nevét az IoT-központban

Hozzon létre egy IoT hubhoz, adjon nevet az-központban. Ez a név hubokon mindenütt egyedinek kell lennie. Nincs a párhuzamos hubok engedélyezett a háttér, azt ajánljuk, hogy a központi lehető egyedileg neve.

### <a name="choose-the-pricing-tier"></a>Válassza ki a árak réteg

Négy rétegek közül választhat: **szabad**, a **normál 1** és a **szabványos 2**és a **Szokásos S3**. A szabad réteg lehetővé teszi, hogy csak 500 eszközök IoT-hubon keresztül csatlakozott, valamint legfeljebb 8000 üzenet naponta kell csatlakoztatni.

**Szabványos S1**: IoT hubok S1 edition eszközök viszonylag kis mennyiségű adatot egy eszköz létrehozása nagy számú IoT megoldásainak lett tervezve. Minden egyes a S1 edition mértékegység lehetővé teszi, hogy legfeljebb 400 000 üzenetek napi összes csatlakoztatott eszközökön keresztül.

**Szabványos S2**: IoT központi S2 edition készült IoT megoldások, amelyben eszközök létrehozása nagy mennyiségű adatot. Minden egyes a S2 edition mértékegység lehetővé teszi, hogy minden csatlakoztatott eszközök közötti napi 6 millió üzeneteket.

**Szabványos S3**: IoT központi S3 edition készült IoT megoldások létrehozása nagy mennyiségű adatot. Minden egyes a S3 edition mértékegység lehetővé teszi, hogy minden csatlakoztatott eszközök közötti napi legfeljebb 300 millió üzeneteket.

![][4]

> [AZURE.NOTE] IoT központi csak egy ingyenes központi Azure előfizetésenként teszi lehetővé.

### <a name="iot-hub-units"></a>IoT központi mennyiség

Egy IoT központi egység üzenetek naponta egy bizonyos számát tartalmazza. A központi támogatott üzenetek száma kötetszámmal mennyiség szorozva az adott réteg naponta üzenetek számát. Például ha azt szeretné, hogy a bejövő üzenetek 700,000 adatok támogatása IoT központi, kiválaszthatja, két S1 réteg egység.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Erőforráscsoport és a felhő eszköz

Módosíthatja egy IoT központi partíciók számát. Alapértelmezett partíciót vannak beállítva, hogy a 4. megadhatja a különféle partíciót azonban a legördülő listából.

Az erőforrás-csoportok nem szükséges külön hozzon létre egy üres erőforráscsoport. Ha létrehoz egy erőforrás, megadhatja, vagy hozzon létre egy új erőforráscsoport erőforrás meglévő csoport használata.

![][5]

### <a name="choose-subscriptions"></a>Válassza az előfizetések

Azure IoT központi automatikusan, amelyhez a fiók kapcsolódik Azure-előfizetések listáját jeleníti meg. Adott Azure előfizetés társíthatja az IoT-központban az alábbi lehetőségek közül választhat.

### <a name="choose-the-location"></a>Válassza ki azt a helyet.

A hely lehetőséget biztosít a azon részeire lépkedhet, amelyben a rendszer felajánlja a IoT központi listáját. IoT központi érhető el az alábbi helyeken bevezetését tervezi: Ausztrália keleti, Ausztrália Délkelet, Ázsia keleti, ázsiai, Délkelet, Észak-Európa, Európa nyugati, japán keleti, japán nyugati US keleti, US nyugati.

### <a name="create-the-iot-hub"></a>A IoT központi létrehozása

Amikor elkészült az előző lépéseket minden, a IoT központi készen áll a hozható létre. Kattintson a **Létrehozás** megkezdéséhez háttéradatbázis létrehozása a IoT központi az adott beállítást, és üzembe megadott helyre.

Eltarthat néhány percig, amíg a IoT elosztót hozható létre, mert a háttéradatbázist telepítéshez végrehajtandó a megfelelő helyre Servers időt vesz igénybe.

## <a name="change-the-settings-of-the-iot-hub"></a>A IoT központi beállításainak módosítása

A központi IoT lap a létrehozása után módosíthatja egy meglévő IoT hubhoz beállításait.

![][8]

**A megosztott Access-házirendek**: ezek a házirendek az engedélyeinek definiálása eszközök és szolgáltatások IoT központi csatlakozni. Ezek a házirendek **az általános**csoportban kattintson a **Megosztott Access-házirendek** érheti el. Ez a lap a, módosíthatja a meglévő házirendeket, vagy új házirend hozzáadása.

### <a name="create-a-policy"></a>Házirend létrehozása

- Kattintson a **Hozzáadás gombra** kattintva nyissa meg a lap. Itt adhatja meg az új szabály nevét és az engedélyeket szeretne társítani ezt a házirendet, az alábbi ábrán látható módon.

    Vannak olyan több jogosultságot a megosztott házirendek társítható. Az első két házirendek, **olvassa el a beállításjegyzék** és a **beállításjegyzék írni**, további támogatási és az írási jogosultsággal, és az eszköz identitás tárolására és a identitás beállításjegyzék. A beállítás a írási automatikusan úgy dönt, az olvasási lehetőséget.

    A **Service csatlakozás** házirend engedélyt elérheti a felhőalapú melletti végpontok, például a fogyasztói csoport szolgáltatások IoT-hubhoz csatlakozik. Az **eszköz kapcsolódni** házirend hozzárendeli engedélyeit a IoT központi eszköz melletti végpontjait a üzenetek küldéséhez és fogadásához.

- Kattintson a **Létrehozás** hozzáadása az újonnan létrehozott házirendet, amely a meglévő listához.

![][10]

## <a name="messaging"></a>Csevegés

Kattintson az **üzenetkezelés** az éppen módosított IoT-központban tulajdonságainak üzenetküldés listájának megjelenítéséhez. Tulajdonságok, módosíthatja, illetve másolása két fő típusa van: **felhő eszközt,** és a **felhőbe eszköz**.

- **Felhőalapú eszközre** beállítások: Ez a beállítás van két subsettings: **Cloud eszköz TTL** (time-to-live) és az üzenetek **adatmegőrzési idő** . A IoT központi első létrehozásakor alapértelmezés szerint egy órával mindkét ezeket a beállításokat létre. Ha módosítani szeretné ezeket az értékeket, a csúszkákkal, vagy írja be az értékeket.

- **A felhőbe** beállításai: Ez a beállítás még több subsettings, amelyek nevű/oszthatók ki az IoT-központban hoz létre, és csak másolhatók más subsettings, amelyek a testre szabható. Ezekkel a beállításokkal jelennek meg a következő szakaszban.

**Partíciót**: Ez az érték van állítva, a IoT központi hoz létre, és módosíthatják a keresztül ezt a beállítást.

**Esemény központi-kompatibilis nevét és végpont**: Ha a IoT központi hoz létre, amikor egy esemény hubhoz létrejön belső való hozzáférést bizonyos körülmények között nem lehet szükség. A központi-kompatibilis esemény neve és a végpont nem szabható testre, de használja a **Másolás** gomb keresztül érhető el.

**Adatmegőrzési idő**: egy nap alapértelmezés szerint állítsa, de testre szabható más értéket a legördülő lista használatával. Ez az érték megegyezik nap eszköz a felhőbe, és nem az órák, az eszközre felhő hasonló beállítása.

**Fogyasztói csoportok**: fogyasztói csoportokkal sok egy hasonló egyéb üzenetben rendszerek adatok lekérése a Kapcsolódás más alkalmazások és szolgáltatások IoT központi eltérő módon használható. Alapértelmezett fogyasztói csoport minden IoT központi jön létre. Azonban hozzáadása vagy törlése a IoT hubok fogyasztói csoportok.

> [AZURE.NOTE] Az alapértelmezett fogyasztói csoport nem szerkeszthetők és nem törölhető.

![][11]

## <a name="pricing-and-scale"></a>Árak és méretezése

Az egy meglévő IoT hubhoz árak módosíthatók **árak** beállításainál, a következő kivételekkel:

- Az aktuális végrehajtása egy IoT hubhoz egy ingyenes Termékváltozat az nem módosítható rétegek a kifizetett termékváltozatok egyikére (vagy fordítva).
- Csak lehet egy ingyenes réteg IoT központi Azure-előfizetésben.

![][12]

Áttérés a egy újabb réteg (S2 vagy S3) alsóbb szint (S1 vagy S2) engedélyezett, csak ha az adott napra küldött üzenetek száma nem ütközik. Például ha napi üzenetek száma meghaladja a 400000, majd a réteg számára az IoT-központban módosítható. Jó helyen jár Ha módosítja a S1 réteg majd a központi folyamatban van az adott napra.

## <a name="delete-the-iot-hub"></a>A IoT központi törlése

Kattintson a **Tallózás gombra**, és a megfelelő hubhoz törlése lehetőséget, majd a törölni kívánt IoT-hubhoz megkeresheti. Kattintson a **Törlés** gombra, a központi törlése a központi neve alatt.

## <a name="next-steps"></a>Következő lépések

Kövesse ezeket a hivatkozásokat, ha többet szeretne tudni az Azure IoT központi kezelése:

- [Tömeges IoT eszközök kezelése][lnk-bulk]
- [Mértékek használata][lnk-metrics]
- [Műveletek figyelése][lnk-monitor]

További feltárása a IoT központi funkcióinak, olvassa el:

- [Fejlesztőeszközök útmutató][lnk-devguide]
- [Egy eszközt a IoT átjáró SDK csomagjában talál, amelyek][lnk-gateway]
- [A IoT megoldás az alapoktól kezdve biztonságos mentése][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md