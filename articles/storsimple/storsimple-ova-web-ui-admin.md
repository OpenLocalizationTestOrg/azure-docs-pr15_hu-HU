<properties 
   pageTitle="Felhasználói felület felügyeleti StorSimple virtuális tömb webes |} Microsoft Azure"
   description="A virtuális tömb StorSimple webes felhasználói felület keresztül egyszerű eszköz felügyeleti feladatokat ismerteti."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>A webhely felhasználói felület használatával felügyelheti a StorSimple virtuális tömb

![beállítási folyamatot.](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>– Áttekintés

Az oktatóanyag az Ez a cikk a Microsoft Azure StorSimple virtuális tömb (más néven a StorSimple helyszíni virtuális eszköz) március 2016 általános elérhetőség (kiadás) verzióját futtatja vonatkoznak. Ez a cikk ismerteti az egyes összetett munkafolyamatok és engedélykezelési feladatai, amelyek a StorSimple virtuális tömb hajtható végre. Kezelheti a StorSimple-kezelővel StorSimple virtuális tömb service (néven a felhasználói felület portál) felhasználói felület és a helyi webes felhasználói felület az eszközhöz. Ez a cikk a feladatokat, hogy a webhely felhasználói felület használatával végezhet koncentrál.

Ez a cikk az alábbi oktatóanyagok tartalmazza:

- A szolgáltatás adatok titkosítási kulcs beszerzése
- Webes felhasználói felület beállítása hibák elhárítása
- A napló csomag készítése
- Állítsa le, vagy az eszköz újraindítása

## <a name="get-the-service-data-encryption-key"></a>A szolgáltatás adatok titkosítási kulcs beszerzése

A szolgáltatás adatok titkosítási kulcs jön létre, amikor az első eszköz regisztrálása az StorSimple kezelő szolgáltatás. A kulcs van, majd a szolgáltatás regisztrációs használatával történő regisztráláshoz szükséges további eszközök a StorSimple kezelő szolgáltatás.

Ha elvesztette a szolgáltatás adatok titkosítókulcs és vissza kell rendelkeznie, hajtsa végre az alábbi lépéseket a felhasználói felület az eszköz helyi webhelyen regisztrált a szolgáltatásban.

#### <a name="to-get-the-service-data-encryption-key"></a>A szolgáltatás adatok titkosítási kulcs beszerzése

1. Csatlakozás helyi webes felhasználói felület. Nyissa meg a **konfigurációs** > **felhő beállításait**.
  

2. A lap alján kattintson a **szolgáltatás adatok titkosítási kulcs letöltése**gombra. Egy kulcsot fog megjelenni. Másolja a vágólapra, és mentse a következő kulcsot.
    
    ![Ismerkedés a szolgáltatás adatok titkosítási kulcs 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Webes felhasználói felület beállítása hibák elhárítása

Bizonyos esetekben az eszközt keresztül a helyi webes felhasználói felülete konfigurálásakor, előfordulhat, hogy fellépő esetleges hibák. Diagnosztizálása és ilyen hibák elhárítása a diagnosztika tesztek futtathatók.

#### <a name="to-run-the-diagnostic-tests"></a>A diagnosztikai vizsgálatok futtatása

1. A helyi webes felhasználói felület, válassza a **Hibaelhárítás** > **diagnosztikai tesztek**.

    ![1 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image29.png)

2. A lap alján kattintson a **Diagnosztikai vizsgálatok futtatása**gombra. Ez a teszteket a hálózaton, az eszköz, a webes proxy, minden lehetséges problémáinak diagnosztizálása kezdeményez idő vagy a felhőben beállításai. Ön értesítést kap, hogy fut-e vizsgálatok az eszközt.

3. Miután tesztek befejeződött, az eredmények jelennek meg. Az alábbi példa a diagnosztikai vizsgálat eredményeit jeleníti meg. Ne feledje, hogy a webes proxybeállítások voltak nincs beállítva az eszköz, így a web proxy próba nem futtathatók. A hálózati beállításokat, a DNS-kiszolgáló és az idő beállítása az összes többi vizsgálat sikeres volt.

    ![2 diagnosztika futtatása](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>A napló csomag készítése

A napló csomag tevődik a vonatkozó naplókat, amely segíthet a Microsoft Support az bármilyen eszközön problémák megoldásához. Ebben a kiadásban a napló csomag keresztül a helyi webes felhasználói felület hozhatók létre.

#### <a name="to-generate-the-log-package"></a>A napló csomag létrehozása

1. A helyi webes felhasználói felület, válassza a **Hibaelhárítás** > **rendszer naplók**.

    ![log csomag 1 készítése](./media/storsimple-ova-web-ui-admin/image31.png)

2. A lap alján kattintson a **log csomag létrehozása**. A rendszer naplók csomag jön létre. Ez eltarthat néhány percig.

    ![2 log csomag készítése](./media/storsimple-ova-web-ui-admin/image32.png)

    Kap értesítést jelenít meg, miután a csomag sikeresen jön létre, és az oldal automatikusan frissítve lesznek, hogy a csomag létrehozásának dátum és időpont.

    ![3 log csomag készítése](./media/storsimple-ova-web-ui-admin/image33.png)

3. Kattintson a **log csomag letöltése**. A rendszer egy ZIP-csomag tölti le.

    ![4 napló csomag készítése](./media/storsimple-ova-web-ui-admin/image34.png)

4. A letöltött napló csomag kibontása, és a rendszer naplófájlok megtekintése.

## <a name="shut-down-and-restart-your-device"></a>Állítsa le, és indítsa újra az eszközt

Állítsa le, vagy indítsa újra az virtuális eszközt, a helyi webes felhasználói felület használatával. Azt, mielőtt indítani, tegye a kötet vagy megosztás az állomásnév oszlopban, majd az eszköz a kapcsolat nélküli ajánlott. Ez lesz összezárása adatsérülésekkel lehetőségét. 

#### <a name="to-shut-down-your-virtual-device"></a>A virtuális eszköz leállítása

1. A helyi webes felhasználói felület, válassza a **Karbantartás** > **Power beállításait**.

2. A lap alján kattintson a **Leállítás**gombra.

    ![eszköz leállítás 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Figyelmeztetés jelenik meg arról, hogy az eszköz a Leállítás szakítja meg bármely IO folyamatban, amelyekre a legrövidebb leállás eredményezi. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![eszköz leállítási figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)

    Ön értesítést kap, hogy a Leállítás kezdeményeztek.

    ![lépések eszköz leállítása](./media/storsimple-ova-web-ui-admin/image38.png)

    Az eszköz most leáll. Ha meg szeretné elindítani az eszközt, szüksége lesz megtennie a Hyper-V kezelője keresztül.

#### <a name="to-restart-your-virtual-device"></a>A virtuális eszköz újraindítása

1. A helyi webes felhasználói felület, válassza a **Karbantartás** > **Power beállításait**.

2. A lap alján kattintson az **Újraindítás**gombra.

    ![eszköz újraindítása](./media/storsimple-ova-web-ui-admin/image36.png)

3. Figyelmeztetés jelenik meg arról, hogy az eszköz újraindítása szakítja meg bármely IOs folyamatban, amelyekre a legrövidebb leállás eredményezi. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Indítsa újra a figyelmeztetés](./media/storsimple-ova-web-ui-admin/image37.png)

    Ön értesítést kap, hogy az újraindítás kezdeményeztek.

    ![Indítsa újra a kezdeményezett](./media/storsimple-ova-web-ui-admin/image39.png)

    Miközben folyamatban van az újraindítást, a kapcsolatot a felhasználói felület való elvesznek. Az újraindítás figyelheti a felhasználói felület rendszeres frissítésével. Azt is megteheti hogy figyelheti az eszköz újraindítása állapotát a Hyper-V kezelője keresztül.

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a StorSimple kezelő szolgáltatás kezelése az eszköz](storsimple-manager-service-administration.md)használatáról.
