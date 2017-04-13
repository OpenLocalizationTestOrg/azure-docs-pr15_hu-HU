<properties
   pageTitle="StorSimple virtuális tömb - VMware rendelkezést terjesztése"
   description="Második oktatóprogram StorSimple virtuális tömb telepítési sorozat magában foglalja a kiépítési VMware virtuális eszközt."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Virtuális tömb StorSimple telepítése – egy virtuális tömbben VMware kiépítése

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>– Áttekintés 
Ebben az oktatóanyagban kiépítési StorSimple virtuális tömbök (más néven StorSimple helyszíni virtuális eszközök vagy StorSimple virtuális eszközök) futó március 2016 általános elérhetőség (kiadás) Megjelenés vonatkozik. Ebből az oktatóanyagból megtudhatja, hogy miként kiépítése, és csatlakoztassa a StorSimple virtuális tömb host operációs rendszert futtató VMware ESXi 5.5 és a fenti. Ez a cikk az Azure klasszikus portál, valamint a Microsoft Azure kormányzati Cloud StorSimple virtuális tömbök telepítését vonatkozik.

Ha kiépítése rendszergazdai jogosultságokkal kell, és kapcsolódás virtuális eszköz. A kiépítési és a kezdeti beállítás befejezéséhez körülbelül 10 percig is eltarthat.


## <a name="provisioning-prerequisites"></a>A kiépítési vonatkozó követelmények

Itt találja az Előfeltételek hozhatók létre virtuális eszköz host operációs rendszert futtató VMware ESXi 5.5 és felett.

### <a name="for-the-storsimple-manager-service"></a>A StorSimple kezelő szolgáltatás

Mielőtt elkezdené, győződjön meg arról, hogy:

-   [Felkészülés a portál StorSimple virtuális tömb](storsimple-ova-deploy1-portal-prep.md)összes lépését befejezése.

-   A virtuális eszköz kép VMware az az Azure portáljáról letöltött. További tudnivalókért lásd: [a 3: Töltse le a virtuális eszköz képet](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>A StorSimple virtuális eszköz 

Mielőtt beállítaná a virtuális eszköz, győződjön meg arról, hogy:

-   Van hozzáférése a Hyper-V szolgáltatást futtató host rendszert (2008 R2 vagy újabb) lehet, hogy egy kiépítése használt eszköz.

-   A host rendszer nem tudja az alábbi forrásokban hozhatók létre virtuális eszköze jelöl ki:

    -   Legalább 4 magmintákat.

    -   Legalább 8 GB RAM.

    -   Egy hálózati kapcsolat.

    -   500 GB virtuális lemez rendszeradatokat.

### <a name="for-the-network-in-datacenter"></a>A hálózat adatközpontban 

Mielőtt elkezdené, győződjön meg arról, hogy:

-   Felül a hálózati követelmények StorSimple virtuális eszköz telepítése, és a beállításokat az Adatközpont hálózaton követelményei szerint. További tudnivalókért olvassa el a [StorSimple virtuális tömb rendszerkövetelményei](storsimple-ova-system-requirements.md)című témakört.

## <a name="step-by-step-provisioning"></a>Lépésenkénti kiépítése 

Építse, és a virtuális eszköz csatlakozik, meg kell hajtsa végre az alábbi lépéseket:

1.  Győződjön meg arról, hogy a host rendszer kellő erőforrások virtuális eszközön minimális követelmények teljesítése érdekében.

2.  Építse be a hipervizor virtuális eszköz.

3.  Indítsa el a virtuális eszközt, és elérheti az IP-címet.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Lépés: 1: Győződjön meg róla, host rendszer megfelel minimális virtuális eszköz

Hozzon létre egy virtuális eszközt, szüksége lesz:

-   A host operációs rendszert futtató VMware ESXi Server 5.5 és feletti elérésére.

-   VMware vSphere ügyfél, a rendszer a ESXi host kezeléséhez.

    -   Legalább 4 magmintákat.

    -   Legalább 8 GB RAM.

    -   Egy hálózati kapcsolat az internethez adatforgalom képes hálózathoz csatlakoztatva. A minimális Internet-sávszélesség 5 MB az optimális használatához az eszköz engedélyezni kell lennie.

    -   500 GB virtuális lemezen az adatokat.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Lépés: 2: Kiépítése hipervizor a virtuális eszköz

Hajtsa végre az alábbi lépéseket a hipervizor a virtuális eszköz hozhatók létre.

1.  Másolja a virtuális eszköz képe, a rendszer. Ez a kép letöltötte az Azure klasszikus portálon keresztül. 
    1.  Győződjön meg arról, hogy ez legyen-e a legújabb képfájl letöltött. Ha korábban letöltötte a képet, töltse le ismét, hogy biztosan a legújabb képet a. A legújabb kép (egy helyett) két fájllal rendelkezik.
    2.  Jegyezze fel fogja használni, akkor ez az eljárás későbbi, ahová másolása a képet.

2.  Jelentkezzen be az ESXi webkiszolgálóra a vSphere ügyfélprogramban. Szüksége lesz egy virtuális gép létrehozása rendszergazdai jogosultságokkal kell rendelkeznie.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  A vSphere ügyfél, a bal oldali ablaktáblában a készlet szakaszában válassza a ESXi kiszolgáló.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  A VMDK ESXi kiszolgálóra először feltöltődnek. Nyissa meg a jobb oldali ablaktáblában a **beállítás** lapon. **Hardver**csoportban jelölje be a **tárhely**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  A jobb oldali ablaktábla **Datastores**jelölje ki a kívánt helyre kattintva töltse fel a VMDK adattárhoz. Az adattárhoz elég hely az operációs rendszer és az adatok lemezt kell rendelkeznie.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Kattintson a jobb gombbal, és válassza a **Tallózás adattárhoz**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  **Adattárhoz** böngészőablakban fog megjelenni.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Kattintson az eszköztár ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) ikonra kattintva hozzon létre egy új mappát. Adja meg a mappa nevét, és jegyezze le azt. (A legjobb ajánlott) virtuális gép létrehozásakor később fogja használni a a mappa nevét. Kattintson az **OK gombra**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  A bal oldali ablaktáblában a **Böngésző adattárhoz**megjelennek az új mappát.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Kattintson a Feltöltés ikonra ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) , és válassza a **Fájl feltöltése**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Érdemes most tallózással keresse meg és mutasson a letöltött VMDK fájlokat. Két fájl lesz. Jelölje ki a feltölteni kívánt fájlt.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Kattintson a **Megnyitás**gombra. A feltöltés a VMDK fájl a megadott adattárhoz elindítható lesz. Eltarthat néhány percig, amíg a feltölteni kívánt fájlt.


1.  A feltöltést követően megjelenik a létrehozott mappában lévő fájl. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Most már a második VMDK fájl feltöltése az azonos adattárhoz kell.

1.  Vissza a vSphere ügyfele ablak. Kijelölt ESXi kiszolgálóval kattintson a jobb gombbal, és válassza az **Új virtuális gép**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  Ekkor megjelenik egy **Új virtuális gép létrehozása** ablak. Az **beállítása** lapon jelölje ki az **egyéni** lehetőséget. Kattintson a **Tovább**gombra.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  A **nevét és helyét** lapon adja meg a virtuális gép nevét. Ez a név meg kell egyeznie a mappa nevét (javasolt a legjobb megoldás) lépés 8 korábban megadott.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  A **tároló** lapon jelölje be a adattárhoz kiépítése a virtuális használni kívánt.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  A **Virtuális gép verzió** lapon válassza az **virtuális gép verzió: 8**. Figyelje meg, hogy 8-11 verziók összes támogatott.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  **A vendégként való bekapcsolódáshoz operációs rendszer** lapján jelölje be a **Vendég operációs rendszer** mint a **Windows**. **Verzió**a legördülő listából válassza ki azt **a Microsoft Windows Server 2012 (64 bites)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  A **processzor** lapon módosítsa a **virtuális sockets számát** és a **virtuális szoftvercsatorna per magmintákat számát** , hogy a **magmintákat száma** 4 (vagy több). Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  A **memória** lapon adja meg a 8 GB (vagy több) RAM. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  A **hálózat** lapon adja meg a hálózati kapcsolatok száma. A minimális követelmények egy olyan hálózati kapcsolaton.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  A **SCSI vezérlő** lapon fogadja el az alapértelmezett **LSI logika vezérlő**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  A **Válassza ki a lemezen** lapon válassza a **meglévő virtuális lemez használatát**. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  A **Meglévő lemez kiválasztása** lapon a **Lemez fájl elérési útját**, kattintson a **Tallózás gombra**. Ekkor megnyílik a **Tallózás Datastores** párbeszédpanel megjelenítéséhez. Nyissa meg azt a helyet, ahol az a VMDK feltöltött. Most már látni fogja az adattárhoz csak egy fájlt, a két fájl, először feltöltött egyesített. Jelölje ki a fájlt, és kattintson az **OK gombra**. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  A **Speciális beállítások** lapon fogadja el az alapértelmezett, és kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  **Készen áll a teljes** lapon tekintse át az új virtuális gép társított összes beállítást. Jelölje be a **Befejezés előtt virtuális gép beállításainak szerkesztése**. Kattintson a **továbbra is**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  A **virtuális gépeken futó** tulajdonságlapján a **hardver** lapon keresse meg a hardver. Jelölje be az **új merevlemez-meghajtó**. Kattintson a **hozzáadása**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Ekkor megjelenik a **Hardver hozzáadása** ablakban. **Eszköz típusa** lapján a **Válassza ki a hozzáadni kívánt eszköz típusa**csoportban jelölje ki a **merevlemezen** , és kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  A **Válassza ki a lemezen** lapon válassza az **Új virtuális lemez létrehozása**. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  A **lemezen létrehozása** lapon módosítsa a **Lemez mérete** 500 GB (vagy több). A **Lemez kiépítési**jelölje be a **Vékony rendelkezni**. Kattintson a **Tovább**gombra.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  A **Speciális beállítások** lapon fogadja el az alapértelmezett.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  **Készen áll a teljes** lapon tekintse át a merevlemez-beállításokat. Kattintson a **Befejezés gombra**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Most már visszatér virtuális gép tulajdonságok lapon. Új merevlemez-meghajtó bekerül a virtuális gépet. Kattintson a **Befejezés gombra**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  A virtuális gép, a jobb oldali ablaktáblában kijelölt, és nyissa meg az **Adatlap** fülre. Tekintse át a virtuális gép beállításai.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

A virtuális gép most már kiépítve. A következő lépés ezen a gépen power és IP-címe.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Lépés 3: Indítsa el a virtuális eszközt, és a IP el

Hajtsa végre az alábbi lépésekkel indítsa el a virtuális eszköz és csatlakozhat hozzá.

#### <a name="to-start-the-virtual-device"></a>A virtuális eszköz indítása

1.  Indítsa el a virtuális eszközt. A vSphere Konfigurációkezelő, a bal oldali ablaktáblában válassza ki az eszközét, és kattintson a jobb gombbal kattintva jelenítse meg a helyi menüben. Jelölje ki a **kiemelt** , és válassza a **Bekapcsolás**. Ez a kell power a virtuális gépen. Az alsó **Legutóbbi feladatok** ablaktáblában az vSphere ügyfél állapota tekinthet meg.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  A beállítási teendők fog néhány percet igénybe vehet. Ha az eszköz fut, nyissa meg a **konzol** fülre. Küldje el a Ctrl + Alt + Delete jelentkezzen be az eszközt. Azt is megteheti a kurzort a konzol ablakban, és nyomja le a Ctrl + Alt + Insert. Az alapértelmezett felhasználó *StorSimpleAdmin* pedig az alapértelmezett jelszó *Jelszo1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Biztonsági okokból a eszközt rendszergazdai jelszó lejár az első bejelentkezés. A rendszer kéri a jelszó módosítása.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Írja be egy jelszót, amely legalább 8 karaktereket tartalmaz. A jelszónak ezeknek a követelményeknek 3 "Házon kívül" 4: nagybetűsre, kisbetűsre, numerikus és különleges karaktereket. Írja be újra a jelszót újra. Ön értesítést kap, hogy a jelszava.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Miután sikeresen megváltozik a jelszót, a virtuális eszköz előfordulhat, hogy újra a számítógépet. Várja meg az újraindítás befejezéséhez. Az eszköz a Windows PowerShell-konzol együtt egy állapotsáv jelennek meg.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  6 – 8 csak lépésekre indításakor be nem DHCP környezetben. Ha DHCP környezetben, hagyja ki ezeket a lépéseket, és folytassa a 9. Az eszköz nem DHCP környezetben felfelé indul el, ha a következő képernyőn jelenik meg. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Most már a hálózat konfigurálása kell.

1.  Használja a `Get-HcsIpAddress` parancs a virtuális eszközén engedélyezve hálózati kapcsolatok listáját. Ha az eszköz engedélyezve van egy hálózati felhasználói felülete, van-e az alapértelmezett nevet a kapcsolat rendelt `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Használja a `Set-HcsIpAddress` parancsmagot a hálózat konfigurálása. Példa nézhet ki:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Miután a kezdeti telepítés befejeződött, és az eszköz indult felfelé, a eszköz szalagcím szövegének jelenik meg. Jegyezze le a IP-címe és URL-CÍMÉT a szalagcím szövegként jelenik meg az eszköz kezelése. Csatlakoztatása a webes a virtuális eszköz felhasználói felület, és töltse ki a helyi beállítása és a regisztrációs e IP-címet kell használni.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Nem kötelező) Csak akkor, ha az eszköz a kormányzati felhőben telepít, végezze el ezt a lépést. Az eszközön most fog engedélyezi az Amerikai Egyesült Államok Federal információk Processing Standard (FIPS) módját. A FIPS 140 standard a titkosítási algoritmust kényes adatok védelméhez amerikai szövetségi kormányzati számítógép rendszerek által jóváhagyott határozza meg.
    1. Ha engedélyezni szeretné a FIPS mód, futtassa a következő parancsmagot:
        
        `Enter-HcsFIPSMode`

    2. Miután engedélyezte a FIPS mód, hogy a titkosítási ellenőrzések csak akkor lépnek érvénybe, indítsa újra az eszközt.

        > [AZURE.NOTE] Engedélyezheti vagy FIPS üzemmód letiltása az eszközön. Váltakozó FIPS és a nem FIPS mód között az eszközön nem támogatott.


Ha az eszköz nem felel meg a minimális konfiguráció, transzparens (lásd alább) szöveg hiba jelenik meg. Szüksége lesz az eszköz beállítások módosíthatók, úgy is, hogy megfelelő források minimális követelmények teljesítése érdekében. Indítsa újra a, majd az eszköz csatlakozni. Olvassa el a minimális konfiguráció követelményeknek [lépés 1: Győződjön meg arról, hogy a host rendszer megfelel-e a virtuális eszközön minimális követelmények](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Ha a kezdeti konfigurálása a helyi web felhasználói felület használata közben, arcra bármilyen hiba, keresse meg a következő munkafolyamatok [kezelése a StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)használata a helyi webes felhasználói felület.

-   Diagnosztikai vizsgálatok futtatása hibáinak elhárítása a [webhely felhasználói felület beállítása](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors).

-   [Generate napló csomag és a naplófájlok megtekintése](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Következő lépések

-   [Egy fájl kiszolgálójával a StorSimple virtuális tömb beállítása](storsimple-ova-deploy3-fs-setup.md)

-   [Egy iSCSI kiszolgálójával a StorSimple virtuális tömb beállítása](storsimple-ova-deploy3-iscsi-setup.md)

