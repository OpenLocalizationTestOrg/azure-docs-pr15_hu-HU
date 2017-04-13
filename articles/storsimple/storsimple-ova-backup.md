<properties 
   pageTitle="Biztonsági másolat oktatóprogram StorSimple virtuális tömb |} Microsoft Azure"
   description="Biztonsági másolat készítése StorSimple virtuális tömb megosztások és kötet ismerteti."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Készítsen biztonsági másolatot a StorSimple virtuális tömb

## <a name="overview"></a>– Áttekintés 

Ebben az oktatóanyagban a Microsoft Azure StorSimple virtuális tömbképletek (más néven StorSimple helyszíni virtuális eszköz vagy StorSimple virtuális eszköz) futó március 2016 általános elérhetőség (kiadás) megjelenés vagy újabb verziók vonatkozik.

A StorSimple virtuális tömb egy olyan hibrid felhőalapú tárolási helyszíni virtuális eszköz fájlkiszolgálóra vagy egy iSCSI kiszolgálón beállított. Készítsen biztonsági másolatokat, biztonsági mentés visszaállítása és hajtsa végre az eszköz feladatátvevő, ha vészhelyreállítás van szükség. Egy fájl kiszolgálójával konfigurálásakor is elemszintű helyreállítási lehetővé teszi. Ebből az oktatóanyagból megtudhatja, hogy miként ütemezett és kézi biztonsági másolatait a StorSimple virtuális tömb létrehozása az Azure klasszikus portálon vagy a StorSimple webes felhasználói felület használatával.


## <a name="back-up-shares-and-volumes"></a>Megosztás és a kötet biztonsági mentése

Biztonsági másolatok pont és az idő védelmet nyújtanak helyreállítás: javítása és kisméretűvé alakítása és az kötet visszaállítása idejét. Biztonsági másolatot készíthet egy megosztás vagy a mennyiségi StorSimple mobileszközön kétféleképpen: **ütemezett** vagy a **kézi**. A módszer az alábbi szakaszok Budai tárgyalja.

> [AZURE.NOTE] Ebben a kiadásban másolatok hozza létre egy alapértelmezett házirendet, amely egy adott időben mindennap elindul, és készít biztonsági másolatot a megosztás vagy a kötet az eszközön. Még nem hozható létre egyéni házirendek ütemezett biztonsági másolatok adott időben.

## <a name="change-the-backup-schedule"></a>A biztonsági mentés ütemezésének módosítása

A StorSimple virtuális eszköz rendelkezik egy alapértelmezett biztonsági házirendet, amely egy adott időben nap (22:30) kezdődik és biztonsági másolatot készít a megosztás vagy a kötet az eszközön egyszer. Módosíthatja az idő, amelynél a biztonsági másolat elindul, de a gyakoriság és az adatmegőrzési (amely megőrzéséhez biztonsági másolatok számát adja meg) nem módosíthatók. A biztonsági másolatok készítésekor a teljes virtuális eszköz készülnek a biztonsági másolatok; Emiatt azt javasoljuk, ezek a biztonsági mentés ütemezése essen.

A következő lépésekkel módosíthatja az alapértelmezett biztonsági kezdetének az [Azure klasszikus portálon](https://manage.windowsazure.com/) .

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>A kezdési időpontot, az alapértelmezett biztonsági házirend módosítása

1. Nyissa meg az eszköz **beállítása** fülre.

2. A **biztonsági mentés** csoportban adja meg a kezdési időpontot a napi biztonsági másolat.

3. Kattintson a **Mentés**gombra.

### <a name="take-a-manual-backup"></a>A kézi biztonsági másolat készítése

Nemcsak a másolatok készíthet egy kézi (igény szerinti) biztonsági bármikor.

#### <a name="to-create-a-manual-on-demand-backup"></a>Kézi (igény szerinti) biztonsági másolat létrehozása

1. Nyissa meg a **megosztás** vagy a **kötet** fülre.

2. A lap alján kattintson az **összes biztonsági másolatot**. Ellenőrizze, hogy szeretné-e a biztonsági másolat letöltése készítése kéri. Kattintson az ellenőrzés ikon ![ikon ellenőrzése](./media/storsimple-ova-backup/image3.png) a biztonsági mentés folytatásához.

    ![biztonsági másolat megerősítése](./media/storsimple-ova-backup/image4.png)

    Ön értesítést kap, hogy egy biztonsági mentési feladat indítása folyamatban van.

    ![biztonsági másolat indítása](./media/storsimple-ova-backup/image5.png)

    Ön értesítést kap, hogy a feladat sikeresen hozták létre.

    ![biztonsági mentési feladat létrehozása](./media/storsimple-ova-backup/image7.png)

3. A projekt haladásának nyomon követése, kattintson a **Feladat megtekintése**gombra.

4. A biztonsági mentési feladat befejezése után kattintson a **biztonsági másolat katalógus** fülre. Megjelenik a bejegyzett biztonsági másolatot.

    ![Befejezett biztonsági mentése](./media/storsimple-ova-backup/image8.png)

5. A szűrési lehetőségek beállítása a megfelelő eszköz biztonsági házirend és időtartomány, és válassza az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-backup/image3.png).

    A biztonsági mentés megjelennek a biztonságimásolat-készletek be a megjelenő listában.

## <a name="view-existing-backups"></a>Meglévő biztonsági másolatok megtekintése

Az Azure klasszikus portálon a meglévő biztonsági másolatok megtekintéséhez kövesse az alábbi lépéseket.

#### <a name="to-view-existing-backups"></a>Meglévő biztonsági másolatok megtekintése

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre.

2. Jelölje ki a biztonsági másolatot, állítsa az alábbiak szerint:

    1. Jelölje ki az eszközt.

    2. A legördülő listában válassza a megosztás vagy a biztonsági másolat, válassza a használni kívánt mennyiségi.

    3. Adja meg az időtartomány.

    4. Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-backup/image3.png) lekérdezés végrehajtásához.

    A kijelölt megosztás társított a biztonsági mentés vagy a mennyiségi jelenjen meg a biztonságimásolat-készletek listáját.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Videó érhető el**

Nézze meg a videóból megtudhatja, hogyan oszthat, készítsen biztonsági másolatot a megosztás és állítsa vissza a StorSimple virtuális tömbben lévő adatokat.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a StorSimple virtuális tömb felügyelete](storsimple-ova-web-ui-admin.md).
