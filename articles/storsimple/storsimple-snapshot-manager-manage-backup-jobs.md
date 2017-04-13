<properties 
   pageTitle="Biztonsági mentési feladatok StorSimple pillanatkép Manager |} Microsoft Azure"
   description="A StorSimple pillanatkép kezelő MMC beépülő modul használatával megtekintheti és ütemezett aktuálisan futó és kész biztonsági feladatok kezelése ismerteti."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>-Kezelővel StorSimple pillanatkép megtekintéséhez és biztonsági másolat feladatok kezelése

## <a name="overview"></a>– Áttekintés

A **feladatok** csomópontot, a **hatókör** ablaktáblán az **ütemezett**, **utolsó 24 órát**és a **operációs rendszert futtató** biztonsági interaktív vagy egy beállított házirend kezdeményezett látható. 

Ebből az oktatóanyagból megtudhatja, hogyan használhatja a **feladatok** csomópont ütemezett, a legutóbbi és a futó biztonsági feladatokra vonatkozó információk megjelenítéséhez. (Az **eredmény** ablaktáblában megjelenik a listája, feladatok és a megfelelő információkat.) Ezenkívül kattintson a jobb gombbal a listában szereplő feladat és környezetben, amely felsorolja a rendelkezésre álló műveletek menüjének megjelenítéséhez.

## <a name="view-scheduled-jobs"></a>Ütemezett feladatainak megtekintése

A következő eljárással biztonsági ütemezett feladatainak megtekintése.

#### <a name="to-view-scheduled-jobs"></a>Az ütemezett feladatainak megtekintése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és kattintson az **ütemezett**. A következő információkat az **eredmények** ablaktáblában jelenik meg:

    - **Név** – az ütemezett pillanatkép neve

    - **Futtassa a következő** – dátuma és időpontja, a következő ütemezett pillanatkép

    - **Utolsó futtatása** – a dátum- és a legutóbbi ütemezett pillanatkép

    >[AZURE.NOTE] Az egyszeri csak egy adott időpontban érvényes a **Következő futtatása** és az **Utolsó futtatása** azonos lesz. 
 
    ![Biztonsági mentési feladat ütemezése](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. További műveletek végrehajtása egy adott feladatra, kattintson a jobb gombbal a feladat nevére, az **eredmény** ablaktáblában, és válassza a menü elemei közül.

## <a name="view-recent-jobs"></a>A legutóbbi feladatok megtekintése

A következő eljárással biztonsági mentés és visszaállítás az elmúlt 24 óra végrehajtott feladatokat.

#### <a name="to-view-recent-jobs"></a>A legutóbbi feladatok megtekintése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és kattintson az **utolsó 24 óra**. Az **eredmények** ablaktáblája (a legfeljebb 64 feladatok) az elmúlt 24 óra biztonsági feladatok látható. A következő információkat az **eredmények** ablaktáblája, attól függően, hogy **a megadott választható** jelenik meg:

    - **Név** – az ütemezett pillanatkép nevét.
 
    - **Lépések** – a dátumot és időpontot, amikor a pillanatkép meg.

    - **Leállítva** – a dátumot és időpontot, amikor a pillanatkép befejeződött, vagy meg lett szakítva.

    - Times **eltelt** – a **lépések** és **Leállítva** között eltelt idő mennyiségét.

    - **Állapot** – a legutóbb befejezett projekt állapotát. **Sikeres** azt jelzi, hogy a biztonsági mentés sikeres hozták létre. **Nem sikerült** , az azt jelzi, hogy a feladat nem futtathatók sikeresen.

    - **Információk** : a hiba okát.

    - **Bájt feldolgozott (MB)** – lett feldolgozásban (MB) mennyiségi csoportból adatok mennyiségét. 

    ![Az elmúlt 24 óra futtatott feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. További műveletek végrehajtása egy adott feladatra, kattintson a jobb gombbal a feladat nevére, az **eredmény** ablaktáblában, és válassza a menü elemei közül.

    ![Feladat törlése](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Futó feladatok megtekintése

Az alábbi eljárással az éppen futó feladatok megtekintése.

#### <a name="to-view-currently-running-jobs"></a>Az éppen futó feladatok megtekintése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **feladatok** csomópontot, és **futtatása**. Attól függően, hogy **a választható, adja meg** az alábbi információkat az **eredmények** ablaktáblában jelenik meg: 

    - **Név** – az ütemezett pillanatkép nevét.

    - **Lépések** – a dátumot és időpontot, amikor a pillanatkép meg.

    - **Ellenőrzés** – az aktuális művelet a biztonsági másolat.

    - **Állapot** – százalékos készültségi.
    
    - **Eltelt** –, amely a biztonsági mentés kezdete óta eltelt idő mennyiségét. 

    - **Átlagos átvitt (MB)** – az összes bájt időtartamának feldolgozása (MB) által feldolgozott adatok arányát.

    - **Bájt feldolgozott (MB)** – bájtok feldolgozásban (MB) adatot.

    - **Bájt írt (MB)** – bájtok írt (MB) adatot. Azt is tartalmaz, az adatok, valamint a metaadatok, és így általában nagyobb, mint a bájt feldolgozása.

    ![Futó feladatok](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. További műveletek végrehajtása egy adott feladatra, kattintson a jobb gombbal a feladat nevére, az **eredmény** ablaktáblában, és válassza a menü elemei közül.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Ismerje meg, [StorSimple pillanatkép Manager kezelheti a biztonságimásolat-katalógus](storsimple-snapshot-manager-manage-backup-catalog.md)használatáról.















            


 

