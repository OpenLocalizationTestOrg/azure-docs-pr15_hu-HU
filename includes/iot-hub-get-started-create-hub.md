## <a name="create-an-iot-hub"></a>Hozzon létre egy IoT központi

Hozzon létre egy IoT hubhoz szimulált mobileszközére való csatlakozáshoz. Az alábbi lépésekkel követnie a feladat elvégzéséhez az Azure portal segítségével.

1. Jelentkezzen be az [Azure portál][lnk-portal].

2. Kattintson a Jumpbar, **Új** > **Internetes a dolog, amit** > **Azure IoT központi**.

    ![Azure portál Jumpbar][1]

3. A **központi IoT** lap válassza ki az adatokat a IoT-központját.

    ![IoT központi lap][2]

    * A **név** mezőbe írja be egy nevet a IoT-központját. A **név** érvényes és esetén érhető el, zöld pipa jelenik meg a **név** mezőbe.
    * Jelölje ki a [árak és arányának réteg][lnk-pricing]. Ebben az oktatóprogramban egy adott réteg nincs szükség. Ebben az oktatóanyagban használja a szabad F1 réteg.
    * **Erőforráscsoport**hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy meglévőt. További tudnivalókért lásd: [használata az erőforrás csoportok kezelése az Azure erőforrások][lnk-resource-groups].
    * A **hely**válassza ki a helyet, a IoT központi tárolni. Ebben az oktatóanyagban legközelebbi adatközpontjának közül.

4. A IoT központi konfigurációs beállítások választotta, kattintson a **Létrehozás**gombra.  Eltarthat néhány percig, amíg a IoT központi létrehozása az Azure. Az állapot ellenőrzése, figyelheti a Startboard be- és az értesítések panel végrehajtását.

    ![Új IoT központi állapota][3]

5. Ha a IoT központi létrehozása sikeresen befejeződött, a IoT-központját az Azure-portálon kattintva nyissa meg a lap az új IoT központi webhely az új csempéjére. Jegyezze le a **Hostname (állomásnév)**, és válassza a **megosztott access-házirendek**.

    ![Új IoT központi lap][4]

6. A **megosztott access-házirendek** lap a jelölje ki az **iothubowner** házirendet, majd másolja, és jegyezze fel a kapcsolati karakterláncot az a **iothubowner** lap. További tudnivalókért lásd: a [hozzáférés-vezérlés] [ lnk-access-control] a "Azure IoT központi Fejlesztőeszközök útmutatóban."

    ![Megosztott access-házirendek lap][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
