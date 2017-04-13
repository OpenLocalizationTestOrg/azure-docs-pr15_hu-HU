<properties 
   pageTitle="Egy StorSimple mennyiségi visszaállítása biztonsági másolatból |} Microsoft Azure"
   description="Megtudhatja, hogyan StorSimple kötet visszaállítása egy biztonsági csoportjából a StorSimple kezelő szolgáltatás biztonsági katalógusoldal használatával."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Egy StorSimple mennyiségi visszaállítása egy biztonsági csoportjából (frissítés 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>– Áttekintés

A **Biztonsági másolat** katalógusoldal jönnek létre, ha a kézi és automatikus biztonsági mentés vesznek biztonsági beállítása jeleníti meg. Ezen a lapon egy biztonsági házirend vagy a mennyiség, jelölje be az összes biztonsági másolatot a lista vagy a biztonsági másolatok törlése, vagy biztonsági használata vagy a mennyiségi klónozhatja.

 ![Biztonsági másolat katalógusoldal](./media/storsimple-restore-from-backup-set-u2/restore.png)

Ebből az oktatóanyagból megtudhatja, hogyan szeretné visszaállítani az eszköz biztonsági meg a **Biztonsági másolat** katalógusoldal használandó.

A helyi mennyiségi vagy felhőalapú biztonsági másolat vissza tudja állítani. Bármelyik lehetőséget választja a visszaállítási művelet azonnal, miközben az adatok letöltése a háttérben felvette a mennyiségi online. 

A visszaállítási művelet kezdeményez, mielőtt tartsa szem előtt a következőket:

- **Végre kell hajtania a mennyiségi offline** – a mennyiségi offline állapotba az állomás és az eszközt, mielőtt kezdeményezhet a visszaállítási művelet. Bár a visszaállítási művelet automatikusan elérhetővé teszi a mennyiségi online az eszközön, kézzel kell hozása online az eszközön az állomáson. A mennyiségi online is átvihet az állomáson, amint a hangereje online, az eszközön. (Nem kell várnia, amíg a visszaállítási művelet befejeződött.) Lépjen a bemutatókból, [a mennyiségi offline](storsimple-manage-volumes-u2.md#take-a-volume-offline)állapotba.

- **Visszaállítás után kötet típusa** – törölt kötet szolgáltatáscsomagot függően a pillanatkép; Ez azt jelenti, hogy kötet, hogy azok helyileg kiemelt szolgáltatáscsomagot helyileg rögzített kötet, és a kötet, amelyeket többszintű szolgáltatáscsomagot többszintű kötet.

    Meglévő kötet a mennyiségi aktuális használatát típusú felülírja a típusa, amely a pillanatkép található. Például a mennyiségi meg visszaállítani a kötet típusa többszintű volt, és kötet típusa letöltése helyi meghajtóra kiemelt (miatt egy átalakítási műveletet végrehajtott) megtett pillanatkép, ha majd hangerejének visszaáll mint helyileg rögzített kötet. Hasonlóképpen ha egy meglévő helyileg rögzített mennyiségi volt bontva, és ezt követően visszaállíthatja azt egy régebbi pillanatkép venni, amikor a mennyiségi volt kisebb a visszaállított mennyiségi őrzik meg aktuális kibontott méretét.

    Nem alakíthatók kötet helyileg rögzített kötet többszintű kötet vagy egy többszintű mennyiségi helyileg rögzített kötet közben a mennyiségi visszaállítják. Várja meg, amíg a visszaállítási művelet befejeződött, és ezután alakíthatja a mennyiségi valamilyen más típust. A mennyiségi konvertálásával kapcsolatos további információért lépjen [mennyiségi típusának módosítása](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **A mennyiségi mérete jelennek meg a visszaállított mennyiségi** – a helyi meghajtóra rögzített mennyiségi (mert helyileg rögzített kötet teljesen kiépített) törölt visszaállítása: fontos tényező az. Győződjön meg arról, hogy van elegendő hely, mielőtt megkísérelne visszaállítása egy korábban törölt helyileg rögzített mennyiségi. 

- **Nem lehet bontsa ki a kötet közben van visszaállítják** – várja meg, amíg a visszaállítási művelet befejeződött, mielőtt megkísérelne bontsa ki a mennyiségi. A mennyiségi kibontása tudni lépjen [a mennyiség módosítása](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Végezheti el a biztonsági másolatot, miközben egy helyi mennyiségi visszaállítandó** – képekkel nyissa meg [a biztonsági házirendek kezelése StorSimple Manager szolgáltatással](storsimple-manage-backup-policies.md).

- **A visszaállítási művelet lemondhatja** – Ha megszakítja a visszaállítási feladat, majd a mennyiségi fog állítható vissza az államot, a visszaállítási művelet kezdeményezett előtti. Lépjen a bemutatókból, a [megszakításhoz feladatot](storsimple-manage-jobs-u2.md#cancel-a-job).

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

A **Biztonsági másolat** katalógusoldal szeretné visszaállítani a StorSimple mennyiségi egy adott biztonsági másolat is használhatja. Felhívjuk a figyelmét arra, akkor jó helyen jár, visszaállítása a mennyiségi visszaáll az állapotába, amelyben a, ha a biztonsági mentés felvételének hangerejének. A biztonsági mentés után felvett adatokat fog veszni.

> [AZURE.WARNING] Visszaállítása biztonsági másolatból lecseréli a meglévő kötet biztonsági másolatból. Ez az adatvesztés bármely után a biztonsági mentés felvételének írt jelenhet meg.

### <a name="to-restore-your-volume"></a>A hangerő visszaállítása

1. A StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre.

    ![Biztonsági másolat katalógus](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Jelölje ki a biztonsági másolatot, állítsa az alábbiak szerint:
  1. Jelölje ki a megfelelő eszköz.
  2. A legördülő listában válassza a mennyiségi vagy a biztonsági másolat házirendet a biztonsági másolat, válassza a használni kívánt.
  3. Adja meg az időtartomány.
  4. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) lekérdezés végrehajtásához.
 
    A biztonsági másolatok társított a kijelölt kötet, vagy biztonsági házirend jelenjen meg a biztonságimásolat-készletek listáját.

3. Bontsa ki a biztonságimásolat-készletet társított kötet megtekintéséhez. Ezek a kötet kell venni a host és az eszköz a kapcsolat nélküli előtt vissza tudja állítani őket. A **Mennyiségi tárolók** lapon a kötet érhet el és kövesse a lépéseket [a mennyiségi offline állapotba](storsimple-manage-volumes-u2.md#take-a-volume-offline) offline állapotba.

    > [AZURE.IMPORTANT] Győződjön meg arról, hogy Ön által végrehajtott az állomáson offline kötet első, mielőtt offline kötet az eszközön. Ha nem a kötet offline az állomáson, esetleg vezethet adatsérülés.

4. Vissza szeretne térni a **Biztonságimásolat-katalógus** fülre, és válassza a biztonságimásolat-készletet.

5. Kattintson a lap alján a **visszaállítása** gombra.

6. Megerősítését kéri. Tekintse át a visszaállítás információkat, és jelölje be a megerősítést kérő jelölőnégyzetet.

    ![Megerősítés lapon](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Kattintson az ellenőrzés ikon ![ikon ellenőrzése](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Meg szeretné tekinteni, a **feladatok** lap elérése visszaállítási feladat kezdeményez. 

8. A visszaállítás befejeződése után ellenőrizze, hogy a kötet tartalmának helyett jelennek meg a biztonsági másolat kötet.

![A videó érhető el](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Videó érhető el**

Nézze meg ezt a videót, mely szemlélteti, hogyan használja az adatfeliratsor, és visszaállíthatja a törölt fájlok StorSimple szolgáltatásai, kattintson [ide](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Ha nem sikerül egy a visszaállítása

Fog értesítést kapni, ha a visszaállítási művelet sikertelen valamilyen okból. Ez akkor fordulhat elő, ha frissítse a biztonsági másolat listában, és győződjön meg arról, hogy a biztonsági másolat is érvényes. Ha a biztonsági mentés érvényes, és meg visszaállítani a felhőből, majd csatlakozási problémákat tapasztal okozhatják a problémát. 

A visszaállítási művelet elvégzéséhez a mennyiségi offline állapotba az állomáson, és próbálkozzon újra a visszaállítási művelet. Figyelje meg, hogy a kötet adatait a visszaállítás során végrehajtott módosításai elvesznek.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan [kezelheti a StorSimple kötet](storsimple-manage-volumes-u2.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
