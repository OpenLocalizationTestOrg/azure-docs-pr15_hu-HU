<properties
   pageTitle="StorSimple eszköz vezérlők kezelése |} Microsoft Azure"
   description="Megtudhatja, hogy miként leállítása, indítsa újra, állítsa le vagy a StorSimple eszköz vezérlők alaphelyzetbe állítása."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>A StorSimple eszköz vezérlők kezelése

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban ismerteti a különböző műveleteket végezheti el a StorSimple eszköz vezérlők. A vezérlőkkel kapcsolatban az StorSimple eszköz olyan aktív-passzív konfigurációban felesleges (partner) vezérlők. Egy adott időben csak egy vezérlő aktív, és a lemez és a hálózat műveletek feldolgozása. A többi vezérlő egy passzív üzemmódban van. Ha nem sikerül az aktív vezérlő, a passzív vezérlő automatikusan aktívvá válik.

Ebben az oktatóanyagban használatával való kezeléséről az eszköz vezérlők lépésenkénti útmutatást tartalmaz a:

- A StorSimple kezelő szolgáltatás a **Karbantartás** lapjának **vezérlők** szakasza
- A Windows PowerShell-StorSimple.

Azt javasoljuk, hogy Ön kezeli-e az eszköz vezérlők StorSimple Manager Service. Ha a művelet csak a Windows PowerShell használatával StorSimple hajtható végre, az oktatóprogram ellenőrzi a őket megjegyezni.

Ebben az oktatóanyagban elolvasása, után lesz képes:

- Indítsa újra, vagy állítsa le a StorSimple eszköz vezérlő
- Állítsa le a StorSimple eszköz
- Az StorSimple eszköz gyári visszaállítása


## <a name="restart-or-shut-down-a-single-controller"></a>Indítsa újra, vagy csak egy vezérlő leállítása

Egy vezérlő újraindítás vagy a Leállítás nincs szükség a szokásos működésének részeként. Leállítás műveletek egyetlen eszköz-vezérlő gyakoriak csak abban az esetben, amelyben egy hibás eszköz hardver-összetevő helyettesítő van szükség. A vezérlő újraindítása is szükség lehet olyan helyzetben, amelyben gyakoroljanak hatást teljesítményre fölösleges memóriahasználat vagy egy hibás vezérlő. Is szüksége lehet egy vezérlő újraindítása után sikeres vezérlőt helyettesíti, ha ki szeretne engedélyezni, és ellenőrizze a lecserélve vezérlőt.

Indítsa újra az eszközt nem zavaró a csatlakoztatott kezdeményezők, feltéve, hogy a passzív vezérlő érhető el. Egy passzív vezérlő hiánya esetén érhető el, és be van kapcsolva a kikapcsolva, majd indítsa újra az aktív vezérlő vonhat a legrövidebb leállás és szolgáltatási zavarok.

> [AZURE.IMPORTANT]

> - **A futó vezérlő kell soha nem fizikailag távolítható el, eredményezne redundancia mérvű és az állásidőt fokozott kockázatot.**

> - Az alábbi eljárást csak a StorSimple fizikai eszközök vonatkozik. Információ arról, hogy miként indítsa el, le, és indítsa újra a virtuális eszközt [a virtuális eszköz használata](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device)című rész tartalmaz.

Indítsa újra, vagy állítsa le a egy egyetlen eszköz vezérlő StorSimple az Azure klasszikus portált a StorSimple kezelő szolgáltatás vagy a Windows PowerShell használatával

Az eszköz vezérlők kezelése az Azure klasszikus portálról, hajtsa végre az alábbi lépéseket.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Indítsa újra, vagy állítsa le a vezérlő klasszikus portálon

1. Nyissa meg azt **Eszközök > karbantartási**.

1. Nyissa meg a **Hardver állapotát** , és ellenőrizze, hogy az eszközön mindkét vezérlők állapotának **Kifogástalan**.

    ![Győződjön meg róla StorSimple eszköz vezérlők megfelelő](./media/storsimple-manage-device-controller/IC766017.png)

1. A **Karbantartás** lap alján kattintson a **Vezérlők kezelése**elemre.

    ![StorSimple eszköz vezérlők kezelése](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Ha nem látja **A vezérlők kezelése**, majd meg kell frissítések telepítése. További tudnivalókért olvassa el a [StorSimple eszköze frissítése](storsimple-update-device.md)című témakört.

1. A **Vezérlő beállításainak módosítása** párbeszédpanelen tegye a következőket:
    1. A **Vezérlő válassza a** legördülő listából válassza ki a használni kívánt vezérlő. A beállításokat, hogy vezérlő 0 és 1 vezérlő. Ezek a vezérlők is aktív vagy passzív azonosítja.

        >[AZURE.NOTE] Egy vezérlő nem lehet kezelni, ha még ki nem érhető el, és be van kapcsolva, és nem jelenik a legördülő listában.

    2. A **Művelet kiválasztása** legördülő listából válassza ki, **Indítsa újra a vezérlőt** , vagy **állítsa le a vezérlő**.

        ![Indítsa újra a StorSimple eszköz passzív vezérlő](./media/storsimple-manage-device-controller/IC766020.png)
    3. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-device-controller/IC740895.png).

Ez indítani, vagy állítsa le a vezérlő. Az alábbi táblázat összefoglalja a részleteit, hogy mi történik, attól függően, hogy a megadott beállítások van a **Vezérlő beállításainak módosítása** párbeszédpanelen.  


|Kijelölés #|Ha úgy dönt, hogy...|Ez történik.|
|---|---|---|
|1.|Indítsa újra a passzív vezérlő.|A feladat hoz létre, hogy indítsa újra a vezérlő, és értesítést kap a feladat sikeres létrehozását követően. Ez a vezérlő újraindítás kezdeményez. Figyelheti az újraindítást elérésével **szolgáltatás > irányítópultok > művelet naplók megtekintése** , és kattintson a paraméterek a szolgáltatásra szerinti szűrés.|
|2.|Indítsa újra az aktív vezérlő.|A következő figyelmeztetést jelenít meg: "újra kell indítania az aktív vezérlő, ha az eszköz nem fognak fölé a passzív vezérlő. Meg szeretné folytatni?" </br>Ha úgy dönt, hogy ez a művelet folytatásához, a következő lépésekkel indítsa újra a passzív vezérlő használtakkal azonos lesz (lásd: a kijelölt 1).|
|3.|Állítsa le a passzív vezérlő.|A következő üzenet jelenik meg: "Leállítás befejeződése után szüksége lesz a power gomb nyomni kapcsolni a vezérlőn. Biztosan állítsa le a vezérlő szeretne?" </br>Ha úgy dönt, hogy ez a művelet folytatásához, a következő lépésekkel indítsa újra a passzív vezérlő használtakkal azonos lesz (lásd: a kijelölt 1).|
|4.|Az aktív vezérlő leállt.|A következő üzenet jelenik meg: "Leállítás befejeződése után szüksége lesz a power gomb nyomni kapcsolni a vezérlőn. Biztosan állítsa le a vezérlő szeretne?" </br>Ha úgy dönt, hogy ez a művelet folytatásához, a következő lépésekkel indítsa újra a passzív vezérlő használtakkal azonos lesz (lásd: a kijelölt 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Indítsa újra, vagy állítsa le a egy vezérlő, a Windows PowerShell-StorSimple
Hajtsa végre az alábbi lépések végrehajtásával leállítása vagy újraindítása csak egy vezérlő az Azure klasszikus portálról StorSimple eszközén.


1. Hozzáférés az eszköz a soros konzol vagy egy olyan távoli számítógépről telnet-munkamenet használatával. Csatlakozás vezérlő 0 vagy 1, hogy vezérlő [Használata gitt való csatlakozáshoz a soros eszköz-konzol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)utasításait követve.

1. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférést biztosít**.

1. A szalagcím üzenetben jegyezze le a vezérlőn (vezérlő 0 vagy vezérlő 1) kapcsolódik, és hogy az aktív vagy a passzív (készenléti) vezérlő szó.
    - Állítsa le egy adatkezelő a parancssorba írja be:

        `Stop-HcsController`

        A vezérlő, amely csatlakozik, leáll. Ha leállítja az aktív vezérlő, majd meghiúsul fölé a passzív vezérlő azt leállítása előtt.
    - Indítsa újra a vezérlő, a parancssorba írja be:

        `Restart-HcsController`

        Ez a vezérlő, amely csatlakozik újraindul. Ha újra kell indítania az aktív vezérlő, meghiúsul fölé a passzív vezérlő az újraindítás előtt.


## <a name="shut-down-a-storsimple-device"></a>Állítsa le a StorSimple eszköz

Ez a szakasz ismerteti, hogyan leállítása egy futó vagy egy hibás StorSimple eszköz olyan távoli számítógépről. Egy eszközt után mindkét eszköz vezérlők leállítása ki van kapcsolva. A eszköz leállítása befejeződött, ha az eszköz fizikailag helyezi át vagy szolgáltatás ki származik.

> [AZURE.IMPORTANT] Az eszköz leállítása előtt az eszköz összetevők állapotának ellenőrzése. Nyissa meg azt **Eszközök > karbantartási > hardver állapot** , és ellenőrizze, hogy minden összetevő LED állapotának zöld. Csak a megfelelő eszköz lesz egy zöld állapotot. Ha van Leállítás gombot az eszköz választva a hibás összetevő cseréje, látni fogja a hibás (piros) vagy a megfelelő komponens csökkentett (sárga) állapotának.

#### <a name="to-shut-down-a-storsimple-device"></a>Egy StorSimple eszközt leállítása

1. A eljárással [Indítsa újra, vagy állítsa le a vezérlő](#restart-or-shut-down-a-single-controller) azonosításához és leállíthatja a passzív vezérlő az eszközön. Hajthatja végre ezt a műveletet az Azure klasszikus portálon vagy a Windows PowerShellben a StorSimple.
2. Ismételje meg a fenti az aktív vezérlő leállítására.
3. Most már tekintse meg az eszköz vissza sík kell. A két vezérlő teljesen leállítása, miután kell a állapotát LED mindkét vezérlők villogó piros. Kapcsolja ki az eszközt teljesen adott időben van szüksége, ha tükrözése Power és hűtési modulok (PCMs) power kapcsolót ki állásba. Ez célszerű kikapcsolni az eszközt.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Az eszköz gyári alapértelmezett beállításainak visszaállítása

> [AZURE.IMPORTANT] Ha az eszköz gyári alapértelmezett beállításainak visszaállítása van szüksége, forduljon a Microsoft Support. Az alábbiakban ismertetett eljárás csak Microsoft Support együtt kell használni.

Ez az eljárás a Microsoft Azure StorSimple eszköz visszaállítása a Windows PowerShell használatának StorSimple gyári beállításait ismerteti.
Eszköz visszaállítása eltávolítja az összes adatai és beállításai alapértelmezés szerint a teljes fürt.

Végezze el a Microsoft Azure StorSimple eszköz gyári alapértelmezett beállításainak visszaállítása az alábbi lépéseket:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Az eszközön a Windows PowerShellben StorSimple alapértelmezett beállításainak visszaállítása

1. Az eszköz a soros konzolon a hozzáférést. Jelölje be a szalagcím üzenet annak érdekében, hogy az aktív vezérlő csatlakozik.

1. A soros konzol menüben válassza az 1, **Jelentkezzen be a teljes hozzáférést biztosít**.

1. A parancssorba írja be a következő parancsot a teljes fürt, és minden adat, a metaadat-alapú és a vezérlő beállítások eltávolítása visszaállítása:

    `Reset-HcsFactoryDefault`

    Ehelyett alaphelyzetbe egy adatkezelő, használja a [Alaphelyzetbe-HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) parancsmagnak a `-scope` paraméter.)

    A rendszer újraindul többször. Kap értesítést jelenít meg, ha az újraindítás sikeresen befejeződött. Attól függően, hogy a rendszer modell-8100 eszközökhöz és a 60-90 percig, amíg ez a folyamat befejezéséhez egy 8600 45 – 60 percig is eltarthat.

    > [AZURE.TIP]

    > - Ha használ frissítés 1.2-es vagy korábbi verziójában használni a `–SkipFirmwareVersionCheck` paramétere ugorja át a belső vezérlőprogram verziójának ellenőrzése (ellenkező megjelenik egy belső vezérlőprogram eltérése: gyári alaphelyzetbe a belső vezérlőprogram verziókban nem egyezik miatt nem tudja folytatni. ).

    > - A gyár alaphelyzetbe eljárás meghiúsulhat StorSimple eszközök, amely a kormányzati portálon frissítés 1 vagy 1.1 fut, és elvégeztetni (ha az új vezérlők előtti frissítés 1 szoftverrel teljesített) sikeres egy vagy két vezérlőt helyettesíti. Ez történik, ha a gyár alaphelyzetbe képe egy SHA1 fájlt, a vezérlő, amely nem létezik a frissítés előtti 1 szoftverek jelenlétét érvényesítése. Ha ez a gyár alaphelyzetbe hiba jelenik meg, lépjen kapcsolatba a Microsoft Support, amely segít a következő lépésekkel. Ez a probléma nem láthatók, helyettesítő vezérlőkkel frissítés 1 vagy újabb szoftver gyárilag teljesített.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Kérdések és válaszok eszköz vezérlők kezelésével kapcsolatban

Ebben a részben azt mezőtó a gyakori kérdésekre irányító StorSimple eszköz vezérlők kapcsolatban.

**KÉRDÉSEK.** Mi történik, ha mindkét eszközömön vezérlői, hogy megfelelő és van kapcsolva a és lehet indítsa újra, vagy állítsa le a aktív vezérlő?

**VÁLASZOK PARANCSRA.** Ha mind a vezérlők a készülékén, hogy megfelelő és van kapcsolva, akkor felkéri a program megerősítést kér. Végezheti el:

- **Indítsa újra az aktív vezérlő** – értesítést kap, hogy az aktív vezérlő újraindítását hatására az eszközre az passzív vezérlő átadni a. A vezérlő újraindul.

- **Az aktív vezérlő leállítása** – értesítést kap, hogy az aktív vezérlő leállítása legrövidebb leállás eredményez. Akkor is kell leküldéses a power gombra a vezérlő bekapcsolása az eszközön.

**KÉRDÉSEK.** Mi történik, ha az eszközeimre passzív vezérlő nem érhető el, és be van kapcsolva kikapcsolva és lehet indítsa újra, vagy állítsa le a aktív vezérlő?

**VÁLASZOK PARANCSRA.** Ha az eszközén passzív vezérlő ki van kapcsolva, vagy nem érhető el, és úgy dönt, hogy:

- **Indítsa újra az aktív vezérlő** – értesítést kap, hogy a művelet folytatása eredményezi egy ideiglenes a szolgáltatási zavarok és megerősítését kéri.

- **Az aktív vezérlő leállítása** – értesítést kap, hogy a művelet folytatása legrövidebb leállás eredményez, és, hogy szüksége lesz a power gomb leküldéses lévő egyik vagy mindkét vezérlők kapcsolja be az eszközt. Megerősítését kéri.

**KÉRDÉSEK.** Amikor jelent, a vezérlő újraindítás vagy a leállítás nem sikerül végrehajtási?

**VÁLASZOK PARANCSRA.** Újraindítását vagy egy vezérlő leállítása meghiúsulhat, ha:

- Egy eszköz frissítés folyamatban van.

- A vezérlő újraindítása már folyamatban van.

- A vezérlő leállítása már folyamatban van.

**KÉRDÉSEK.** Hogyan lehet, tudni, ha egy vezérlő indítani, vagy állítsa le?

**VÁLASZOK PARANCSRA.** A vezérlő-a karbantartás lap állapot ellenőrzése A vezérlő állapota jelzik, hogy egy vezérlő újraindul vagy leáll. Emellett a riasztások lap fog tartalmazni tájékoztató értesítés, ha a vezérlő újraindítása vagy a Leállítás. A vezérlő újraindítása és leállítási műveleteket is rögzítése a művelet naplókban. További információt a művelet naplók nyissa meg [a művelet naplók](storsimple-service-dashboard.md#view-the-operations-logs)megtekintéséhez.

**KÉRDÉSEK.** Van-e a műveletekkel együtt eredményeként vezérlő feladatátvevő bármely gyakorolt hatás?

**VÁLASZOK PARANCSRA.** A TCP-kapcsolatokat kezdeményezők és az aktív vezérlő között vezérlő feladatátvevő eredményeként alaphelyzetbe kell állítani, de hozni, amikor a passzív vezérlő azt feltételezi, hogy a művelet. Ez a művelet során lehetnek olyan I/O tevékenység kezdeményezők és az eszköz között az ideiglenes (legfeljebb 30 másodperc) szünet.

**KÉRDÉSEK.** Hogyan lépjen vissza az adatkezelő szolgáltatás leállítása és eltávolítása után?

**VÁLASZOK PARANCSRA.** Egy vezérlő visszaállításához service kell behelyezi a váz [cserélje ki a vezérlő modult StorSimple eszközén](storsimple-controller-replacement.md)leírt módon.

## <a name="next-steps"></a>Következő lépések

- Ha a StorSimple eszköz vezérlőkkel, amely nem tudja elhárítani az ebben az oktatóanyagban, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md)felsorolt eljárásokat használatával kapcsolatos problémák megoldásához.

- A StorSimple kezelő szolgáltatás használatával kapcsolatos további információért olvassa el az [használata a StorSimple kezelő szolgáltatás felügyelete StorSimple eszközére](storsimple-manager-service-administration.md).
