<properties
   pageTitle="Visszaállítása biztonsági másolatból a StorSimple virtuális tömb"
   description="További tudnivalók a StorSimple virtuális tömb biztonsági másolatának visszaállítása."
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
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Visszaállítása biztonsági másolatból a StorSimple virtuális tömb

## <a name="overview"></a>– Áttekintés 

Ez a cikk a Microsoft Azure StorSimple virtuális tömbképletek (más néven StorSimple helyszíni virtuális eszköz vagy StorSimple virtuális eszköz) futó március 2016 általános elérhetőség (kiadás) megjelenés vagy újabb verziójára vonatkozik. Ez a cikk ismerteti részletesen szeretné visszaállítani a megosztás vagy a StorSimple virtuális tömb a kötet biztonsági csoportja. A cikk is részletezi az elemszintű helyreállítási működése a StorSimple virtuális tömb, amely fájlkiszolgálóra van beállítva.


## <a name="restore-shares-from-a-backup-set"></a>Megosztás visszaállítása egy biztonsági beállítása


**Mielőtt megpróbálja megosztások, győződjön meg arról, hogy rendelkezik-e elegendő hely a művelet befejezéséhez az eszközön.** Visszaállítása biztonsági másolatból, az [Azure klasszikus portálon](https://manage.windowsazure.com/)végezze el az alábbi lépéseket.

#### <a name="to-restore-a-share"></a>A megosztás visszaállítása

1.  Tallózással keresse meg a **biztonsági másolat katalógus**. Szűrheti a megfelelő eszköz és a biztonsági másolatok keresése időtartomány. Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-restore/image1.png) végrehajtja a lekérdezést.


1.  A biztonságimásolat-készletek jelenik meg a listában kattintson, és válassza az adott biztonsági. Bontsa ki a biztonsági mentés kattintva megtekintheti a különböző megosztások alatt. Kattintson a gombra, és jelölje be a visszaállítani kívánt megosztása.

2.  A lap alján kattintson az **Új visszaállítása**gombra.

3.  Ez a **visszaállítás új megosztási** varázslóban kezdeményez. **Adja meg nevét és helyét** lapon:


    1.  Ellenőrizze az adatforrás eszköz nevét. Ez az eszköz, amely tartalmazza a visszaállítani kívánt megosztás kell. Az eszköz Kijelölés gomb szürkén jelenik meg. Egy másik forrásból Eszközválasztás szüksége lesz a varázsló bezárásához és a biztonsági mentés újra meg újra.

    2.  Adja meg a megosztás nevét. A megosztás neve 3 – 127 karaktereket tartalmazhat.

    3.  Tekintse át a méretét, írja be, és a visszaállítani kívánt megosztás társítandó jogosultságok. Windows Intéző használatával megosztás tulajdonságainak módosítása a helyreállítás végeztével tudja.

    4.  Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  A visszaállítási feladat befejeződése után a visszaállítás indul el, és egy másik értesítés jelenik meg. Visszaállítás állapotának nyomon követéséhez, kattintson a **feladat megtekintése**gombra. Ezzel eljut a **feladatok** lap.

2.  A visszaállítási feladat állapotának nyomon követheti. Ha a visszaállítás 100 %-os készültségi, lépjen vissza a **megosztás** lapon az eszközön.

3.  Megtekintheti az új visszaállított megosztás most megosztásának listáját az eszközön. Figyelje meg, hogy a visszaállítás befejeződött, a megosztás azonos típusú. A többszintű megosztás visszahelyezi, többszintű és a helyi meghajtóra rögzített helyileg a kiemelt megosztás a megosztás.

Most az eszköz konfigurációjának befejeződött, és biztonsági mentése és visszaállítása a megosztás módjáról tanultak. 


## <a name="restore-volumes-from-a-backup-set"></a>Kötet visszaállítása egy biztonsági beállítása


Visszaállítása biztonsági másolatból, az Azure klasszikus portálon végezze el az alábbi lépéseket. A visszaállítás az adott virtuális eszközön; új kötet a biztonsági mentés visszaállítása nem állítható vissza egy másik eszközt.

#### <a name="to-restore-a-volume"></a>A hangerő visszaállítása

1.  Tallózással keresse meg a **biztonsági másolat katalógus**. Szűrheti a megfelelő eszköz és a biztonsági másolatok keresése időtartomány. Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-restore/image1.png) végrehajtja a lekérdezést.

2.  A biztonságimásolat-készletek jelenik meg a listában kattintson, és válassza az adott biztonsági. Bontsa ki a biztonsági mentés kattintva megtekintheti a különböző kötet alatt. Jelölje ki a visszaállítani kívánt kötet. 

5.  A lap alján kattintson az **Új visszaállítása**gombra. Az **új mennyiségi visszaállítás** varázsló elindul.

1.  **Adja meg nevét és helyét** lapon:


    1.  Ellenőrizze az adatforrás eszköz nevét. Ez az eszköz, amely tartalmazza a visszaállítani kívánt mennyiségi kell. Az eszköz kijelölés nem érhető el. A különböző forrásból Eszközválasztás szüksége lesz a varázsló bezárásához és a biztonsági mentés újra meg újra.

    2.  Mennyiségi nevezze el a mennyiségi új visszaállítják. A mennyiségi neve 3 – 127 karaktereket tartalmazhat.

    3.  Kattintson a nyíl ikonra.

        ![](./media/storsimple-ova-restore/image12.png)

1.  **A mennyiségi használatára képes megadása hosts** lapján jelölje be a megfelelő ACRs a legördülő listából.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-restore/image1.png). Ez kezdeményez egy visszaállítási feladat, és ekkor megjelenik a következő üzenet, amely a feladat folyamatban van.

2.  A visszaállítási feladat befejeződése után a visszaállítás indul el, és egy másik értesítés jelenik meg. Visszaállítás állapotának nyomon követéséhez, kattintson a **feladat megtekintése**gombra. Ezzel eljut a **feladatok** lap.

3.  A visszaállítási feladat állapotának nyomon követheti. Nyissa meg újra a **kötet** lapot az eszközön.

4.  Most már megtekintheti az új visszaállított kötet mennyiségének a listában az eszközön. Megjegyzés: a mennyiségi azonos típusú, hogy a visszaállítás elvégezte az eltávolítást. Többszintű kötet, többszintű visszahelyezi, és a helyi meghajtóra rögzített mennyiségi visszahelyezi mint helyileg rögzített kötet.

5.  Miután a mennyiségi megjelenik a lista mennyiségének online, használatra a mennyiségi érhető el.  A iSCSI kezdeményező állomáson iSCSI kezdeményező tulajdonságok ablak a cél listájának frissítése.  A visszaállított mennyiségi nevét tartalmazó új cél szerint "Inaktív" Állapot oszlopban a jelenjenek meg.

6.  Jelölje be a cél, és kattintson a **Csatlakozás**gombra.   Miután a kezdeményező csatlakozik a cél, állapotát módosítsa **csatlakoztatva**. 

7.  A **Merevlemez-kezelés** ablakban a csatlakoztatott kötet jelenik meg az alábbi ábrán látható módon. Kattintson a jobb gombbal az észlelt mennyiségi (kattintson a lemez neve), és kattintson az **Online**.

> [AZURE.IMPORTANT] A hangerőt, vagy a megosztás visszaállítása biztonsági másolatból meg beállítása, ha nem sikerül a visszaállítási feladat, a cél mennyiségi, vagy megosztás továbbra is hozható létre a portálon. Fontos, hogy a célhely kötet törlése vagy a portálon: Ez az elem jövőbeli kérdéseket minimalizálásához megosztása.

## <a name="item-level-recovery-ilr"></a>Elemszintű helyreállítási (ILR)

Ebben a kiadásban a elemszintű helyreállítás (ILR) fájl konfigurálva StorSimple virtuális eszközön vezet be. A szolgáltatás lehetővé teszi, hogy végezze el a fájlok és mappák részletes helyreállítási felhő másolatából StorSimple az eszközre a megosztások. Felhasználók a törölt fájlokat beolvasható önkiszolgáló modell segítségével biztonsági másolata.

Minden megosztás van a legújabb biztonsági másolatait tartalmazó *.backups* mappája. A felhasználó nyissa meg a kívánt biztonsági másolat, fontos fájlok és mappák másolja a biztonsági mentés és visszaállításukról. A fájlok visszaállítása biztonsági mentés így nem hívások rendszergazdák számára.

1.  A ILR végrehajtásakor megtekintheti a biztonsági másolatok Windows Intéző keresztül. Kattintson az adott megosztása tekintse meg a biztonsági másolat a kívánt parancsra. Ekkor megjelenik a biztonsági másolatok tároló megosztása alapján létre *.backups* mappa. Bontsa ki a *.backups* mappa megtekintése a biztonsági másolatok. A mappa majd jelennek meg a teljes biztonsági hierarchia robbantott nézetét. Ebben a nézetben hoz létre az igény szerinti és történik általában csak néhány másodpercig hozhat létre.

    Az utolsó 5 biztonsági másolatok így jelenik meg, és egy elemszintű helyreállításhoz használható. Az 5 biztonsági másolata tartalmazza az alapértelmezett ütemezett és a kézi biztonsági másolatait is.

    
    -   **Ütemezett biztonsági mentés** másként nevű &lt;eszköz neve&gt;DailySchedule-ÉÉÉÉHHNN-ÓÓPPMM-UTC.

    -   **Kézi biztonsági mentést** , az Active Directory-ideiglenes-ÉÉÉÉHHNN-ÓÓPPMM-UTC nevű.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Azonosítsa a biztonsági mentés, ahol az a törölt fájl legújabb verzióját. A mappa nevét tartalmazza az egyes a fenti esetek UTC időbélyeg, amelynél a mappa létrehozásának nem Bár a tényleges eszköz indításakor esetén a biztonsági mentés. A mappa időbélyeg segítségével keresse meg és a biztonsági másolatok meghatározása.

2.  Keresse meg azt a mappát, vagy a azonosította az előző lépésben, biztonsági másolatot a visszaállítani kívánt fájlt. Megjegyzés: a a fájlok vagy mappák engedélyeinek rendelkező csak akkor tud megtekinteni. Ha Ön nem hozzáférhet az egyes fájlokat vagy mappákat, szüksége lesz egy ki a Windows Intézővel a megosztási engedélyek módosítása és hozzáférést biztosít az adott fájl vagy mappa megosztása rendszergazdához kell fordulnia. Egy ajánlott javasoljuk, hogy a megosztás rendszergazdának kell helyett egy felhasználó egy felhasználói csoportot.

3.  A fájl vagy mappa másolja a StorSimple fájlkiszolgálón megfelelő megosztása.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Videó érhető el**

Nézze meg a videóból megtudhatja, hogyan oszthat, készítsen biztonsági másolatot a megosztás és állítsa vissza a StorSimple virtuális tömbben lévő adatokat.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Következő lépések

További tudnivalók [A felhasználói felület helyi webes StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)felügyelete.
