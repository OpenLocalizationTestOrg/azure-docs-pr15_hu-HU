<properties 
   pageTitle="A StorSimple biztonsági katalógus kezelése |} Microsoft Azure"
   description="Megtudhatja, hogyan listában, jelölje ki és törlése a biztonságimásolat-készletek kötet StorSimple kezelő szolgáltatás biztonságimásolat-katalógus lapot használva."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>A biztonságimásolat-katalógust kezelése a StorSimple kezelő szolgáltatás használatával

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás **Biztonsági** katalógusoldal jönnek létre, ha manuális vagy ütemezett biztonsági másolatok vesznek biztonsági beállítása jeleníti meg. Ezen a lapon egy biztonsági házirend vagy a mennyiség, jelölje be az összes biztonsági másolatot a lista vagy a biztonsági másolatok törlése, vagy biztonsági használata vagy a mennyiségi klónozhatja.

Ebből az oktatóanyagból megtudhatja, hogyan listában, jelölje ki és biztonsági másolat törlése. Megtudhatja, hogy miként eszköze visszaállítása biztonsági másolatból, keresse fel a [visszaállítása egy biztonsági beállítása az eszközt](storsimple-restore-from-backup-set.md). Megtudhatja, hogy miként kötet klónozhatja, lépjen [a StorSimple mennyiségi Adatfeliratsor](storsimple-clone-volume.md).

![Biztonsági másolat katalógus](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

A **Biztonsági másolat** katalógusoldal biztosít a biztonsági másolat szűkítése a lekérdezés kijelölés beállítása. A biztonságimásolat-készletek amely beolvassa, a következő paraméterek alapján szűrhető:

- **Eszköz** – az eszköz biztonsági másolat megadása készült.

- **Biztonsági másolat házirend vagy mennyiségi** – a biztonsági házirendeket és a biztonságimásolat-készlet társított mennyiségi.

- A **feladó és címzett** – a biztonságimásolat-készlet létrehozása a dátum és idő tartományban.

A szűrt biztonságimásolat-készletek majd megjelennének az alábbi attribútumok alapján:

- **Név** – a biztonsági másolat házirend vagy a biztonságimásolat-készlet társított mennyiségi nevére.

- **Méret** – a biztonsági másolat megadása a valós méret.

- **Létrehozás dátuma** – Ha létrehozásuk dátuma és időpontja. 

- **Típus** – biztonságimásolat-készletek pillanatfelvételek a felhő vagy helyi pillanatképek lehet. Helyi pillanatkép biztonsági másolatot a mennyiségi tárolható összes adatra helyileg az eszközön, mivel a biztonsági mentés a felhőbe tartózkodó mennyiségi adatok felhőben pillanatkép hivatkozik. Helyi pillanatképek elérése gyorsabb, mivel a felhőben egy adott időpontban érvényes adatok tűrőképessége van kiválasztva.

- **Elindító** – a biztonsági másolatok indítható automatikusan ütemezett vagy kézzel egy. Biztonsági másolatok ütemezése biztonsági házirend is használhatja. Másik lehetőségként a **biztonsági másolat készítése** lehetőséget is használhatja a kézi biztonsági másolatot készíthet.

## <a name="list-backup-sets-for-a-volume"></a>A mennyiségi beállítja a lista biztonsági mentése
 
Hajtsa végre a következő felsoroló kötet biztonsági másolatok.

#### <a name="to-list-backup-sets"></a>A lista biztonságimásolat-készletek

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre.

2. Az alábbi képlettel történik szűrése a lehetőségek közül:

    1. Jelölje ki a megfelelő eszköz.

    2. A legördülő listában válassza a megfelelő a biztonsági másolatok megtekintése a mennyiségi.

    3. Adja meg az időtartomány.

    4. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) lekérdezés végrehajtásához.
 
    A kijelölt kötet társított biztonsági másolatok jelenjen meg a biztonságimásolat-készletek listáját.

## <a name="select-a-backup-set"></a>Kattintson a biztonsági másolat beállítása

Jelölje ki a biztonságimásolat-mennyiségi vagy biztonsági házirendek készlettel az alábbi lépések elvégzésével.

#### <a name="to-select-a-backup-set"></a>Jelölje be a biztonsági másolat megadása

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre.

2. Az alábbi képlettel történik szűrése a lehetőségek közül:

    1. Jelölje ki a megfelelő eszköz.

    2. A legördülő listában válassza a mennyiségi vagy a biztonsági másolat házirendet a biztonsági másolat, válassza a használni kívánt.

    3. Adja meg az időtartomány.

    4. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) lekérdezés végrehajtásához.

    A biztonsági másolatok társított a kijelölt kötet, vagy biztonsági házirend jelenjen meg a biztonságimásolat-készletek listáját.

3. Jelölje ki, és bontsa ki a biztonságimásolat-készletet. A **visszaállítása** és **törlése** elem jelennek meg az oldal alján. Az alábbi műveletek egyikét a biztonságimásolat-készlet kijelölt végezhet.

## <a name="delete-a-backup-set"></a>Biztonsági másolat törlése

Ha nem szeretné megőrizni az adatokat, társítva biztonsági törlése Hajtsa végre az alábbi lépéseket a biztonságimásolat-készlet törléséhez.

#### <a name="to-delete-a-backup-set"></a>A biztonságimásolat-készlet törlése

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági mentés katalógusának fülre**.

2. Az alábbi képlettel történik szűrése a lehetőségek közül:

    1. Jelölje ki a megfelelő eszköz.

    2. A legördülő listában válassza a mennyiségi vagy a biztonsági másolat házirendet a biztonsági másolat, válassza a használni kívánt.

    3. Adja meg az időtartomány.

    4. Kattintson az ellenőrzés ikon ![Ellenőrzés ikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) lekérdezés végrehajtásához.

    A biztonsági másolatok társított a kijelölt kötet, vagy biztonsági házirend jelenjen meg a biztonságimásolat-készletek listáját.

3. Jelölje ki, és bontsa ki a biztonságimásolat-készletet. A **visszaállítása** és **törlése** elem jelennek meg az oldal alján. Kattintson a **Törlés**gombra.

4. Kap értesítést jelenít meg, amikor a törlés folyamatát, és amikor sikeresen befejeződött. Miután végzett a törlés, frissítse az ezen a lapon a lekérdezést. A törölt biztonságimásolat-készlet már nem jelenik meg a biztonságimásolat-készletek listájában.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [a biztonságimásolat-katalógus visszaállítása egy biztonsági csoportjából az eszköz](storsimple-restore-from-backup-set.md)használatáról.

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
