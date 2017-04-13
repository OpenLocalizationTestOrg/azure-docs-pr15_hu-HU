<properties
   pageTitle="StorSimple virtuális tömb – a Hyper-V rendelkezést terjesztése"
   description="Második oktatóprogram StorSimple virtuális tömb környezetben magában foglalja a Hyper-v virtuális eszköz kiépítési."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Virtuális tömb StorSimple telepítése – egy virtuális tömböt a Hyper-V kiépítése

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>– Áttekintés

Ebben az oktatóanyagban kiépítési a Microsoft Azure StorSimple virtuális tömbök (más néven StorSimple helyszíni virtuális eszközök vagy StorSimple virtuális eszközök) futó március 2016 általános elérhetőség (kiadás) Megjelenés vonatkozik. Ebben az oktatóprogramban egy StorSimple virtuális tömböt host operációs rendszert futtató a Hyper-V Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 rendszeren kiépítését ismerteti. Ez a cikk az Azure klasszikus portál, valamint a Microsoft Azure kormányzati Cloud StorSimple virtuális tömbök telepítését vonatkozik.

Ha kiépítése rendszergazdai jogosultságokkal kell, és virtuális eszköz beállítása. A kiépítési és a kezdeti beállítás befejezéséhez körülbelül 10 percig is eltarthat.


## <a name="provisioning-prerequisites"></a>A kiépítési vonatkozó követelmények

Itt találja az Előfeltételek hozhatók létre virtuális eszköz host operációs rendszert futtató a Hyper-V Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 rendszeren.

### <a name="for-the-storsimple-manager-service"></a>A StorSimple kezelő szolgáltatás

Mielőtt elkezdené, győződjön meg arról, hogy:

-   [Felkészülés a portál StorSimple virtuális tömb](storsimple-ova-deploy1-portal-prep.md)összes lépését befejezése.

-   A virtuális eszköz kép Hyper-v az Azure portáljáról letöltött. További tudnivalókért lásd: [a 3: Töltse le a virtuális eszköz képet](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] A szoftver fut a StorSimple virtuális tömb csak használható együtt a Storsimple kezelő szolgáltatás.

### <a name="for-the-storsimple-virtual-device"></a>A StorSimple virtuális eszköz

Mielőtt beállítaná a virtuális eszköz, győződjön meg arról, hogy:

-   Van hozzáférése host operációs rendszert futtató a Hyper-V Windows Server 2008 R2 vagy újabb, amely egy kiépítése használt eszköz.

-   A host rendszer nem tudja az alábbi forrásokban hozhatók létre virtuális eszköze jelöl ki:

    -   Legalább 4 magmintákat.

    -   Legalább 8 GB RAM.

    -   Egy hálózati kapcsolat.

    -   500 GB virtuális lemez rendszeradatokat.

### <a name="for-the-network-in-the-datacenter"></a>A hálózati a adatközponthoz

Mielőtt elkezdené, tekintse át a hálózati követelmények StorSimple virtuális eszköz telepítése, és állítsa be megfelelően a adatközponthoz hálózathoz. További tudnivalókért olvassa el a [StorSimple virtuális tömb hálózati követelmények](storsimple-ova-system-requirements.md#networking-requirements)című témakört.

## <a name="step-by-step-provisioning"></a>Lépésenkénti kiépítése

Építse, és a virtuális eszköz csatlakozik, meg kell hajtsa végre az alábbi lépéseket:

1.  Győződjön meg arról, hogy a host rendszer kellő erőforrások virtuális eszközön minimális követelmények teljesítése érdekében.

2.  Építse be a hipervizor virtuális eszköz.

3.  Indítsa el a virtuális eszközt, és elérheti az IP-címet.

Az alábbi szakaszok ezeket a lépéseket mindegyik magyarázza.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Lépés: 1: Győződjön meg arról, hogy a host rendszer megfelel-e a virtuális eszközön minimális követelmények

Hozzon létre egy virtuális eszközt, szüksége lesz:

-   A Windows Server 2012 R2, Windows Server 2012-ben vagy Windows Server 2008 R2 SP1 rendszeren telepített a Hyper-V szerepkört.

-   A host Microsoft a Hyper-V elemre a Microsoft Windows ügyfél csatlakozik.

Győződjön meg róla, hogy a mögöttes hardver (host rendszer), amelyen a virtuális eszköz hoz létre tudja az alábbi forrásokban jelöl ki a virtuális eszköz:

- Legalább 4 magmintákat.
- Legalább 8 GB RAM.
- Egy hálózati kapcsolat.
- 500 GB virtuális lemez rendszeradatokat.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Lépés: 2: Kiépítése hipervizor a virtuális eszköz

A következő lépésekkel kiépítése a hipervizor egy eszközt.

#### <a name="to-provision-a-virtual-device"></a>A virtuális eszköz kiépítése

1.  A helyi meghajtón a virtuális eszköz kép másolása a Windows Server állomáson. Ez a kép (virtuális vagy VHDX), amely letöltötte az Azure portálon keresztül. Jegyezze fel fogja használni, akkor ez az eljárás későbbi, ahová másolása a képet.

2.  Nyissa meg **A Kiszolgálókezelő**. A jobb felső sarokban kattintson az **eszközök** , és jelölje be a **Hyper-V kezelője**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Ha a Windows Server 2008 R2 rendszerben nyissa meg a Hyper-V kezelője. Kattintson a Kiszolgálókezelő **szerepkörök > a Hyper-V > Hyper-V kezelője**.

1.  A **Hyper-V kezelője**a hatókör ablaktáblán kattintson a jobb gombbal kattintva nyissa meg a helyi menü a rendszer csomópontot, és kattintson az **Új** > **virtuális gépen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Az **első lépések** az új virtuális gép varázsló lapján kattintson a **Tovább**gombra.

1.  Az **Adja meg nevét és helyét** lapon adja meg egy **nevet** a virtuális eszköz. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  **Generációs megadása** lapon válassza az eszköz kép típusát, és kattintson a **Tovább**gombra. A lapon nem jelenik meg, a Windows Server 2008 R2 használata.

    * Válassza a **generációs 2** , ha a Windows Server 2012 vagy újabb .vhdx képként letöltött.
    * Válassza a **generációs 1** , ha a Windows Server 2008 R2 vagy újabb .vhd képként letöltötte.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  A **memória hozzárendelése** lapon adja meg az **indítási memória** legalább **8192 MB**, nem dinamikus memóriafoglalási engedélyezése, és kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  A **Hálózatkezelés beállítása** lapon adja meg a virtuális kapcsoló, amelyhez csatlakozik az internethez, és kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  **Kapcsolódás virtuális merevlemez** lapon válassza **egy meglévő virtuális merevlemez használata**, adja meg a virtuális eszköz kép (.vhdx vagy .vhd) helyét, és kattintson a **Tovább gombra**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Tekintse át az **Összegzés** , és kattintson a **Befejezés** hozhat létre virtuális gépen. De nem Ugrás a következő még, – van szüksége néhány Processzormagok és egy másik meghajtóra. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  A minimális követelményeknek, 4 magmintákat szüksége lesz. A host rendszerhez, a **Hyper-V kezelő** ablakban listájában szereplő **virtuális gépeken futó**, jobb oldali ablaktáblában kijelölt virtuális processzorok, felvételére szolgáló keresse meg az imént létrehozott virtuális gépet. Jelölje ki, és kattintson a jobb gombbal a számítógép nevét, és válassza a **Beállítások**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Kattintson a **Beállítások** lapon, a bal oldali ablaktáblában a **processzor**. Állítsa be **a virtuális processzorok száma** a jobb oldali ablaktáblában 4-es (vagy több). Kattintson a **alkalmazása**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  A minimális követelményeknek is kell hozzáadnia 500 GB virtuális adatok lemezen. A **Beállítások** lapon:

    1.  A bal oldali ablaktáblában válassza a **SCSI-vezérlő**.
    2.  A jobb oldali ablaktáblán jelölje ki a **Merevlemez-meghajtó,** és kattintson a **Hozzáadás**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  A **merevlemez-meghajtó** lapon jelölje ki a **virtuális merevlemez** lehetőséget, és kattintson az **Új**gombra. Ekkor az **Új virtuális merevlemez-varázsló**elindul.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Az új virtuális merevlemez-varázsló **első lépések** lapon kattintson a **Tovább**gombra.

1.  **Válassza a lemez formátum lapon**fogadja el az alapértelmezett beállítás a **VHDX** formátum. Kattintson a **Tovább**gombra. A képernyőn nem látható, ha futtatja a Windows Server 2012 R2 vagy Windows Server 2008 R2.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  **Válassza a lemez lapon**beállítása virtuális merevlemez-típus, **dinamikusan növekvő** (ajánlott). Ha úgy dönt, hogy a **rögzített méretű** lemezen, is működik, de előfordulhat, hogy túl hosszú ideig várjon. Azt javasoljuk, hogy ne használjon a **Differencing** lehetőséget. Kattintson a **Tovább**gombra. Ne feledje, hogy **dinamikusan növekvő** az alapértelmezett a Windows Server 2012 R2 és a Windows Server 2012-ben. A Windows Server 2008 R2 az alapértelmezett érték a **rögzített méretű**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  Az **Adja meg nevét és helyét** lapon adja meg a **nevét** , valamint a **helyet** (meg, akkor átválthat az egyik) adatok lemez. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  A **Lemez konfigurálása** lapon válassza az **Új üres virtuális merevlemez-meghajtó létrehozása** lehetőséget, és adja meg a mérete **500 GB** (vagy több). Amikor 500 GB minimálisan, mindig a nagyobb lemezen kiépítése meg. Megjegyzés: a kibontása vagy kisebb a lemez egyszer kiépítéstől nem. Kiépítése lemez méretét a további tudnivalókért tekintse át az méretező szakaszt, az [ajánlott eljárások a](storsimple-ova-best-practices.md#configuration-best-practices) dokumentumban. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Az **Összegzés** lapon tekintse át a virtuális adatok lemez részleteit, és ha teljesül, a lemez létrehozása a **Befejezés** gombra. A varázsló bezárul, és egy virtuális merevlemez hozzáadódik a számítógépen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Ad vissza, a **beállításokat** tartalmazó lapra. Kattintson **az OK gombra** a **Beállítások** lap bezárása és visszatérés a Hyper-V ablakot.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Lépés 3: Indítsa el a virtuális eszközt, és a IP el

Hajtsa végre az alábbi lépésekkel indítsa el a virtuális eszköz és csatlakozhat hozzá.

#### <a name="to-start-the-virtual-device"></a>A virtuális eszköz indítása

1.  Indítsa el a virtuális eszközt.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Az eszköz futása után válassza ki az eszközt, kattintson a jobb gombbal, és kattintson a **Csatlakozás**gombra.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Előfordulhat, hogy csak 5-10 perccel az eszköz, ahhoz, hogy készen. Állapotüzenet jelenik meg a projekt haladásának jelzése konzolt. Miután az eszköz készen áll, válassza az **Action**. Nyomja le a `Ctrl + Alt + Delete` a virtuális eszköz való bejelentkezéshez. Az alapértelmezett felhasználó *StorSimpleAdmin* pedig az alapértelmezett jelszó *Jelszo1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Biztonsági okokból a eszközt rendszergazdai jelszó lejár az első bejelentkezés. A rendszer kéri a jelszó módosítása.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Írja be egy jelszót, amely legalább 8 karaktereket tartalmaz. A jelszó teljesíteniük kell legalább 3 kívül az alábbi 4 követelmények: nagybetűsre, kisbetűsre, numerikus és különleges karaktereket. Írja be újra a jelszót újra. Ön értesítést kap, hogy a jelszava.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Miután sikeresen megváltozik a jelszót, a virtuális eszköz indítsa újra a is. Várja meg az eszköz indításához.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Az eszköz a Windows PowerShell-konzol együtt egy állapotsáv jelenik meg.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  6 – 8 csak lépésekre indításakor be nem DHCP környezetben. Ha DHCP környezetben, hagyja ki ezeket a lépéseket, és folytassa a 9. Az eszköz nem DHCP környezetben felfelé indul el, ha a következő képernyőn jelenik meg.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Most már a hálózat konfigurálása kell.

1.  Használja a `Get-HcsIpAddress` parancs a virtuális eszközén engedélyezve hálózati kapcsolatok listáját. Ha az eszköz engedélyezve van egy hálózati felhasználói felülete, van-e az alapértelmezett nevet a kapcsolat rendelt `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Használja a `Set-HcsIpAddress` parancsmagot a hálózat konfigurálása. Példa nézhet ki:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Miután a kezdeti telepítés befejeződött, és az eszköz indult felfelé, a eszköz szalagcím szövegének jelenik meg. Jegyezze le a IP-címe és URL-CÍMÉT a szalagcím szövegként jelenik meg az eszköz kezelése. Csatlakoztatása a webes a virtuális eszköz felhasználói felület, és töltse ki a helyi beállítása és a regisztrációs e IP-címet kell használni.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Nem kötelező) Csak akkor, ha az eszköz a kormányzati felhőben telepít, végezze el ezt a lépést. Az eszközön most fog engedélyezi az Amerikai Egyesült Államok Federal információk Processing Standard (FIPS) módját. A FIPS 140 standard a titkosítási algoritmust kényes adatok védelméhez amerikai szövetségi kormányzati számítógép rendszerek által jóváhagyott határozza meg.
    1. Ha engedélyezni szeretné a FIPS mód, futtassa a következő parancsmagot:

        `Enter-HcsFIPSMode`

    2. Miután engedélyezte a FIPS mód, hogy a titkosítási ellenőrzések csak akkor lépnek érvénybe, indítsa újra az eszközt.

        > [AZURE.NOTE] Engedélyezheti vagy FIPS üzemmód letiltása az eszközön. Váltakozó FIPS és a nem FIPS mód között az eszközön nem támogatott.

Ha az eszköz nem felel meg a minimális konfiguráció, transzparens (lásd alább) szöveg hiba jelenik meg. Szüksége lesz az eszköz beállítások módosíthatók, úgy is, hogy megfelelő források minimális követelmények teljesítése érdekében. Indítsa újra a, majd az eszköz csatlakozni. Olvassa el a minimális konfiguráció követelményeknek [lépés 1: Győződjön meg arról, hogy a host rendszer megfelel-e a virtuális eszközön minimális követelmények](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Ha a kezdeti konfigurálása a helyi web felhasználói felület használata közben, arcra bármilyen hiba, keresse meg a következő munkafolyamatok [kezelése a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)használata a helyi webes felhasználói felület.

-   Diagnosztikai vizsgálatok futtatása hibáinak elhárítása a [webhely felhasználói felület beállítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generate napló csomag és a naplófájlok megtekintése](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![videó ikon](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Videó érhető el**

A videóból megtudhatja, hogyan egy StorSimple virtuális tömböt a Hyper-V szolgáltatást úgy is létrehozhatnak figyelése.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Következő lépések

-   [Egy fájl kiszolgálójával a StorSimple virtuális tömb beállítása](storsimple-ova-deploy3-fs-setup.md)

-   [Egy iSCSI kiszolgálójával a StorSimple virtuális tömb beállítása](storsimple-ova-deploy3-iscsi-setup.md)
