<properties 
   pageTitle="Biztonsági házirendek StorSimple pillanatkép Manager |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, és szabályozhatja az ütemezett biztonsági másolatok biztonsági házirendek kezelése a StorSimple pillanatkép kezelő MMC beépülő modullal."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>-Kezelővel StorSimple pillanatkép hozhat létre és biztonsági házirendek kezelése

## <a name="overview"></a>– Áttekintés

Biztonsági házirend létrehoz egy ütemtervet adatok biztonsági másolatának mennyiségi helyileg vagy a felhőben. Amikor létrehoz egy biztonsági házirendet, adatmegőrzési szabály is megadhatja. (Legfeljebb 64 elérhető információk segítségével megőrizheti.) Biztonsági házirendek kapcsolatos további tudnivalókért lásd: a [biztonsági másolat típusok](storsimple-what-is-snapshot-manager.md#backup-type) [StorSimple 8000 sorozat: hibrid felhő megoldást](storsimple-overview.md).

Ebből az oktatóanyagból megtudhatja, hogy miként:

- Biztonsági másolat házirend létrehozása 
- A biztonsági másolat házirend szerkesztése 
- Biztonsági másolat házirend törlése 

## <a name="create-a-backup-policy"></a>Biztonsági másolat házirend létrehozása

A következő eljárással hozzon létre egy új biztonsági házirendet.

#### <a name="to-create-a-backup-policy"></a>Biztonsági másolat házirend létrehozása

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **Biztonsági másolat házirendek**, és kattintson a **Biztonsági másolat házirend létrehozása**gombra.

    ![Biztonsági másolat házirend létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    A **házirend létrehozása** párbeszédpanel jelenik meg. 

    ![Házirend - az Általános lap létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Az **Általános** lapon hajtsa végre az alábbi adatokat:

   1. **A szöveg mezőbe** írja be a szabály nevét.

   2. A **mennyiségi csoport** mezőbe írja be a csoport nevét, a mennyiségi társított a házirend.

   3. Válassza a **helyi pillanatkép** vagy **felhőalapú pillanatkép**.

   4. Válassza ki, hogy egy adott időpontban érvényes, amely megőrzi az számát. Ha bejelöli **az összes**, 64 pillanatképek lesz megmarad (legnagyobb). 

4. Kattintson az **Ütemezés** fülre.

    ![Házirend - Ütemezés lap létrehozása](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Az **Ütemterv** lapon hajtsa végre az alábbi adatokat: 

   1. Kattintson a **engedélyezése** jelölőnégyzetet, a következő biztonsági mentés ütemezése.

   2. A **Beállítások**csoportban jelölje ki **egyszeri**, **napi**, **heti**vagy **havi**. 

   3. A **Start** szövegmezőbe kattintson a naptár ikonra, és adja meg a kezdési dátumot.

   4. A **Speciális beállítások**beállíthatja, hogy nem kötelező ismétlési ütemterveket és a záró dátumot.

   5. Kattintson az **OK gombra**.

Miután létrehozott egy biztonsági házirendet, az alábbi információkat az **eredmények** ablaktáblában jelenik meg:

- **Név** – a biztonsági másolat házirend nevére.

- **Típus** – helyi pillanatfelvétel vagy felhőalapú pillanatkép.

- **Mennyiségi csoport** – a házirend társított mennyiségi csoportot.

- **Adatmegőrzés** – tartja meg az; pillanatképek száma a maximális érték 64.

- **Létrehozva** – ezzel a házirenddel létrehozott a dátumot.

- **Engedélyezett** -e a házirend van érvényben: **Igaz** jelzi, hogy valójában; **Hamis** azt jelzi, hogy nincs hatással. 

## <a name="edit-a-backup-policy"></a>A biztonsági másolat házirend szerkesztése

A következő eljárással biztonsági meglévő házirend szerkesztéséhez.

#### <a name="to-edit-a-backup-policy"></a>A biztonsági másolat házirend szerkesztéséhez

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson a **Biztonsági másolat házirendek** csomópontot. A biztonsági házirendek az **eredmény** ablaktáblában jelennek meg. 

3. Kattintson a jobb gombbal a szerkeszteni kívánt házirendet, és kattintson a **Szerkesztés**gombra. 

    ![A biztonsági másolat házirend szerkesztése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Amikor megjelenik a **házirend létrehozása** ablak, adja meg a módosításokat, és kattintson **az OK**gombra. 

## <a name="delete-a-backup-policy"></a>Biztonsági másolat házirend törlése

A következő eljárással biztonsági házirend törlése.

#### <a name="to-delete-a-backup-policy"></a>Biztonsági másolat házirend törlése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson a **Biztonsági másolat házirendek** csomópontot. A biztonsági házirendek az **eredmény** ablaktáblában jelennek meg. 

3. Kattintson a jobb gombbal a biztonsági másolat házirendet, amely a törölni kívánt, és kattintson a **Törlés**gombra.

4. A megerősítést kérő üzenet megjelenésekor kattintson az **Igen**gombra.

    ![Biztonsági másolat házirend megerősítés törlése](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Megtudhatja, hogyan [-kezelővel StorSimple pillanatkép megtekintéséhez és biztonsági másolat feladatok kezelése](storsimple-snapshot-manager-manage-backup-jobs.md).
