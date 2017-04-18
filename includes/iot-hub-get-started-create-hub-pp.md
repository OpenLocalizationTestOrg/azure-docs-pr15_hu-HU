## <a name="create-a-device-management-enabled-iot-hub"></a>Hozzon létre egy eszközt engedélyezve IoT központi-kezelés

Mivel a mobileszköz-kezelés IoT központi előzetes verzióban, egy eszköz engedélyezett felügyeleti IoT hubhoz létrehozásához szükséges. Általános elérhetősége IoT központi mobileszköz-kezelés elérésekor ebben az oktatóanyagban frissülnek. Az alábbi lépésekkel követnie az Azure portálon a feladat elvégzéséhez.

1.  Jelentkezzen be az [Azure-portálon].
2.  A Jumpbar, a kattintson az **Új**, majd kattintson **Az Internet a dolgokat**, és válassza az **Azure IoT központi**.

    ![][img-new-hub]

3.  A **Központi IoT** lap válassza ki az adatokat a IoT-központját.

    ![][img-configure-hub]

  -   A **név** mezőbe írja be egy nevet a IoT-központját. A **név** érvényes és esetén érhető el, zöld pipa jelenik meg a **név** mezőbe.
  -   Jelölje ki a **árak és arányának réteg**. Ebben az oktatóprogramban egy adott réteg nincs szükség.
  -   **Erőforráscsoport**hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt. További tudnivalókért lásd: [használata az erőforrás csoportok kezelése az Azure erőforrások].
  -   A jelölőnégyzet bejelölésével **Engedélyezheti mobileszköz-kezelés – előzetes verzió**.
  -   A **hely**válassza ki a helyet, a IoT központi tárolni. Mobileszköz-kezelés IoT központi érhető el csak kelet-Amerikai Egyesült Államok, Észak-Európa és kelet-ázsiai nyilvános előzetes verzióban.

    > [AZURE.NOTE]Ha nem jelöli a **Mobileszköz-kezelés engedélyezése**jelölőnégyzetet, a minták nem működnek.<br/>Jelölje be a **Mobileszköz-kezelés engedélyezése**, hozzon létre IoT központi csak kelet-Amerikai Egyesült Államok, Észak-Európa és kelet-ázsiai használható és nem szánt gyártási esetek előnézetét. Be- és kijelentkezés a eszköz engedélyezett felügyeleti hubok, eszközök nem tudja áttelepíteni.

4.  A IoT központi konfigurációs beállítások választotta, kattintson a **Létrehozás**gombra. Eltarthat néhány percig, amíg a IoT központi létrehozása az Azure. Az állapot ellenőrzése, figyelheti a **Startboard** be- és az **értesítések** panel végrehajtását.

    ![][img-monitor]

5.  A IoT központi létrehozás sikeresen, a lap számára a központi automatikusan megnyitja. Jegyezze le a **Hostname (állomásnév)**, és válassza a **megosztott access-házirendek**.

    ![][img-keys]

6.  Jelölje ki az **iothubowner** házirendet, majd másolja a vágólapra, és jegyezze fel a kapcsolati karakterláncot az a **iothubowner** lap. Másolja a vágólapra egy helyen érheti el később mert szüksége, ebben az oktatóanyagban befejezéséhez.

    > [AZURE.NOTE] Gyártási alkalmazási eseteit győződjön meg arról, tartózkodik a **iothubowner** hitelesítő adataival.

    ![][img-connection]

Ezzel létrehozott egy eszközt management IoT központi engedélyezve van. A kapcsolati karakterlánc ebben az oktatóanyagban befejezéséhez szükséges.

## <a name="create-a-device-identity"></a>Egy eszköz azonosító létrehozása

Ebben a részben [IoT központi Explorer] nevű csomópontot eszköz használata[ iot-hub-explorer] ebben az oktatóprogramban egy eszköz identitás létrehozásához.

1. A parancssori környezetben futtassa a következő:

    telepítés -g npmiothub-explorer@latest

2. Ezután a következő parancsot történő bejelentkezéshez a központi való helyettesítéséhez megjegyzése `{service connection string}` előzőleg másolt IoT központi kapcsolati karakterlánccal:

    iothub-explorer bejelentkezési "{szolgáltatás kapcsolati karakterlánc}"

3. Végül a nevű új eszköz megjelenést alakíthat `myDeviceId` paranccsal:

    iothub-explorer myDeviceId – kapcsolat-karakterlánc létrehozása

Az eszköz kapcsolati karakterláncot az eredményből jegyezze fel. A kapcsolati karakterlánc használatos eszközhöz alkalmazással csatlakozni a eszközként IoT-központját.

![][img-identity]

[IoT központi – első lépések] hivatkozni[ lnk-getstarted] az eszköz identitások programozás útján létrehozása lehetőséget.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure portál]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Erőforrás-csoportok használatával az Azure erőforrások kezelése]: ../articles/azure-portal/resource-group-portal.md
