<properties 
   pageTitle="Egy StorSimple mennyiségi visszaállítása biztonsági másolatból |} Microsoft Azure"
   description="Megtudhatja, hogyan StorSimple kötet visszaállítása egy biztonsági csoportjából a StorSimple kezelő szolgáltatás biztonsági katalógusoldal használatával."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Egy StorSimple mennyiségi visszaállítása egy biztonsági beállítása

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>– Áttekintés

A **Biztonsági másolat** katalógusoldal jönnek létre, ha a kézi és automatikus biztonsági mentés vesznek biztonsági beállítása jeleníti meg. Ezen a lapon egy biztonsági házirend vagy a mennyiség, jelölje be az összes biztonsági másolatot a lista vagy a biztonsági másolatok törlése, vagy biztonsági használata vagy a mennyiségi klónozhatja.

 ![Biztonsági másolat katalógusoldal](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Ebből az oktatóanyagból megtudhatja, hogyan a **Biztonsági másolat** katalógusoldal használata az eszközön kötet visszaállítása egy biztonsági csoportjából.

## <a name="how-to-use-the-backup-catalog"></a>A biztonsági másolat katalógus használata 

A **Biztonsági másolat** katalógusoldal tartalmaz, amely segít a biztonsági másolat szűkítése a lekérdezés kijelölés beállítása. A biztonságimásolat-készletek visszaadott a következő paraméterek alapján szűrhető:

- **Eszköz** – az eszköz biztonsági másolat megadása készült.
- **Biztonsági másolat házirend** vagy **mennyiségi** – a biztonsági házirendeket és a biztonságimásolat-készlet társított mennyiségi.
- **És **hogy** –** a biztonságimásolat-készlet létrehozása a dátum és idő tartományban.

A szűrt biztonságimásolat-készletek majd megjelennének az alábbi attribútumok alapján:

- **Név** – a biztonsági másolat házirend vagy a biztonságimásolat-készlet társított mennyiségi nevére.
- **Méret** – a biztonsági másolat megadása a valós méret.
- **A létrehozás dátuma** – Ha létrehozásuk dátuma és időpontja. 
- **Típus** – biztonságimásolat-készletek pillanatfelvételek a felhő vagy helyi pillanatképek lehet. Helyi pillanatkép biztonsági másolatot a mennyiségi tárolható összes adatra helyileg az eszközön, mivel a biztonsági mentés a felhőbe tartózkodó mennyiségi adatok felhőben pillanatkép hivatkozik. Helyi pillanatképek elérése gyorsabb, mivel a felhőben egy adott időpontban érvényes adatok tűrőképessége van kiválasztva.
- **Által kezdeményezett** – a biztonsági másolatok indítható automatikusan szerint ütemezett vagy kézzel egy. (Is használhatja a biztonsági másolat házirend ütemezése biztonsági mentést. Másik lehetőségként használhatja a **biztonsági másolat készítése** lehetőséget egy interaktív biztonsági másolat készítése.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>A StorSimple mennyiségi visszaállítása biztonsági másolatból

A **Biztonsági másolat** katalógusoldal szeretné visszaállítani a StorSimple mennyiségi egy adott biztonsági másolat is használhatja. 

> [AZURE.WARNING] Visszaállítása biztonsági másolatból lecseréli a meglévő kötet biztonsági másolatból. Ez az adatvesztés bármely után a biztonsági mentés felvételének írt jelenhet meg.

Kezdeményez egy visszaállítása kötet, mielőtt győződjön meg arról, hogy a mennyiségi offline. A hangerő offline állapotba az állomáson első szüksége lesz, és kattintson az eszközt. Kövesse a [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline). Hajtsa végre az alábbi lépéseket a mennyiségi visszaállítása egy biztonsági csoportjából.

### <a name="to-restore-from-a-backup-set"></a>Vissza a biztonságimásolat-készlet

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre.

    ![Biztonsági másolat katalógus](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Jelölje ki a biztonsági másolatot, állítsa az alábbiak szerint:
  1. Jelölje ki a megfelelő eszköz.
  2. A legördülő listában válassza a mennyiségi vagy a biztonsági másolat házirendet a biztonsági másolat, válassza a használni kívánt.
  3. Adja meg az időtartomány.
  4. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) lekérdezés végrehajtásához.
 
    A biztonsági másolatok társított a kijelölt kötet, vagy biztonsági házirend jelenjen meg a biztonságimásolat-készletek listáját.

3. Bontsa ki a biztonságimásolat-készletet társított kötet megtekintéséhez. Ezek a kötet kell venni a host és az eszköz a kapcsolat nélküli előtt vissza tudja állítani őket. Kövesse a [kötet offline állapotba](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Győződjön meg arról, hogy Ön által végrehajtott az állomáson offline kötet első, mielőtt offline kötet az eszközön. Ha nem a kötet offline az állomáson, esetleg vezethet adatsérülés.

4. Jelölje ki a biztonságimásolat-készletet. Kattintson a lap alján a **visszaállítása** gombra.

6. Megerősítését kéri. 

    ![Megerősítés lapon](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Ellenőrizze a visszaállítás információkat, és kattintson az ellenőrzés ikon ![ikon ellenőrzése](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Meg szeretné tekinteni, a **feladatok** lap elérése visszaállítási feladat kezdeményez. 

8. A visszaállítás befejeződése után ellenőrizze, hogy a kötet tartalmának helyett jelennek meg a biztonsági másolat kötet.

![A videó érhető el](./media/storsimple-restore-from-backup-set/Video_icon.png) **Videó érhető el**

Nézze meg ezt a videót, mely szemlélteti, hogyan használja az adatfeliratsor, és visszaállíthatja a törölt fájlok StorSimple szolgáltatásai, kattintson [ide](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan [kezelheti a StorSimple kötet](storsimple-manage-volumes.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
