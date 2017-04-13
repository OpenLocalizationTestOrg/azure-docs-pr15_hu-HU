<properties
   pageTitle="A StorSimple mennyiségi klónozhatja |} Microsoft Azure"
   description="Ismerteti a különböző adatfeliratsor típusát, és mikor érdemes használni, és megtudhatja, hogy miként használhatja az egyes kötet klónozhatja beállítása biztonsági."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>A mennyiségi (frissítés 2) másolása a StorSimple kezelő szolgáltatás használatával

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás **Biztonsági** katalógusoldal jönnek létre, ha a kézi és automatikus biztonsági mentés vesznek biztonsági beállítása jeleníti meg. Ezen a lapon egy biztonsági házirend vagy a mennyiség, jelölje be az összes biztonsági másolatot a lista vagy a biztonsági másolatok törlése, vagy biztonsági használata vagy a mennyiségi klónozhatja.

![Biztonsági másolat katalógusoldal](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Ebben az oktatóanyagban ismerteti, hogy miként használhatja az egyes kötet klónozhatja beállítása biztonsági. Azt is bemutatja *tranziens* és *állandó* klónok közötti különbség.

>[AZURE.NOTE] 
>
>A helyi meghajtóra rögzített mennyiségi fog klónozható mint többszintű kötet. Ha módosítania kell a helyi meghajtóra kiemelt klónozott kötet, konvertálhatja az adatfeliratsor egy helyi rögzített mennyiségi után az adatfeliratsor művelet sikeresen befejeződött. A többszintű mennyiségi helyileg rögzített kötet konvertálásával kapcsolatos információ megnyitásához [mennyiségi típusának módosítása](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Ha megpróbálja a klónozott kötet konvertálása közvetlenül azután, hogy klónozhatja (Ha még egy tranziens adatfeliratsor), a helyi meghajtóra kiemelt többszintű a konvertálás meghiúsul, az alábbi hibaüzenet:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Ez a hiba érkezik, csak akkor, ha meg van klónozhatja megjelenítheti egy másik eszközt. A kiemelt helyi meghajtóra, ha először alakítja a tranziens adatfeliratsor állandó átirattal kötet sikeresen alakíthatók. Az ideiglenes (tranziens) adatfeliratsor állandó átirattal alakításához pillanatkép egy felhőalapú azt.

## <a name="create-a-clone-of-a-volume"></a>Hozzon létre egy kötet átirattal

Létrehozhat egy adatfeliratsor ugyanarra az eszközre, egy másik eszközre, vagy akár egy virtuális gép helyi használatával vagy felhőalapú pillanatkép.

#### <a name="to-clone-a-volume"></a>A mennyiségi klónozhatja

1. StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre, és válassza a biztonságimásolat-készletet.

2. Bontsa ki a biztonságimásolat-készletet társított kötet megtekintéséhez. Kattintson a gombra, és jelölje ki a mennyiségi a biztonságimásolat-készletet.

     ![A mennyiségi másolása](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Kattintson a **Adatfeliratsor** a kijelölt kötet klónozhatja megkezdéséhez.

4. Csoportban **Adja meg nevét és helyét**a mennyiségi Adatfeliratsor varázsló:

  1. Azonosítsa a célként megadott eszköz. Ez a, a helyet, ahol az adatfeliratsor létrejön. Válassza a ugyanarra az eszközre, vagy adjon meg egy másik eszközt. Ha úgy dönt, egyéb felhő szolgáltatók társított kötet (nem Azure), a legördülő listában a cél eszköz csak akkor látható fizikai eszközök. Egyéb felhő szolgáltatók virtuális eszközön társított kötet nem klónozhatja.

        >[AZURE.NOTE] Győződjön meg róla, hogy az adatfeliratsor szükséges kapacitásának kisebb, mint a célként megadott eszköz a rendelkezésre álló kapacitás.

  2. Adja meg a adatfeliratsor egyedi mennyiségi nevét. A név 3 és 127 karakter közé kell tartalmaznia. 
    
        >[AZURE.NOTE] A **Mennyiségi mint Adatfeliratsor** mező **Tiered** lesz, még akkor is, ha vannak klónozhatja, akkor egy helyi rögzített mennyiségi. Ez a beállítás; nem módosítható. jó helyen jár módosítania kell a helyi meghajtóra, valamint kell kiemelt klónozott kötet, ha konvertálhatja az adatfeliratsor egy helyi rögzített mennyiségi az adatfeliratsor sikeres létrehozását követően. A többszintű mennyiségi helyileg rögzített kötet konvertálásával kapcsolatos információ megnyitásához [mennyiségi típusának módosítása](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![1 adatfeliratsor varázsló](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Kattintson a nyíl ikonra ![nyíl-ikon](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) folytassa a következő lapra.

5. Csoportban **Adja meg hosts, használhatja a mennyiségi**:

  1. Adja meg az adatfeliratsor hozzáférési vezérlő rekord (ACR). Felvehet egy új ACR és a meglévő listából választhatja ki.

        ![2 adatfeliratsor varázsló](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Kattintson az ellenőrzés ikon ![négyzet-ikon](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)a művelet végrehajtásához.

6. A adatfeliratsor feladat kezdődik, és értesítést kap a adatfeliratsor sikeresen létrehozásakor. Kattintson a **Feladat megtekintése** Lync- **feladatok** lapon a adatfeliratsor feladatot. A következő üzenet az adatfeliratsor feladat befejezésekor jelennek meg:

    ![Adatfeliratsor üzenet](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Miután az adatfeliratsor feladat befejezése:

  1. Lépjen az **eszközök** lapra, és kattintson a **Hangerő tárolók** fülre. 
  2. Jelölje be a mennyiségi tároló a forrás kötet, amely akkor klónozva társított. A mennyiségek listában az imént létrehozott adatfeliratsor láthatók.

>[AZURE.NOTE] Figyelés és alapértelmezett biztonsági másolatot a rendszer automatikusan letiltotta klónozott kötet.

Az így létrehozott átirattal tranziens átirattal. Adatfeliratsor típusok kapcsolatos további tudnivalókért [tranziens összehasonlítása állandó klónok](#transient-vs.-permanent-clones)témakörben talál.

Ez az adatfeliratsor ettől kezdve a normál kötet, és minden olyan művelet, hogy a kötet lehetőség van az adatfeliratsor az elérhető lesz. Szüksége lesz a mennyiségi bármely biztonsági másolatok konfigurálása.

## <a name="transient-vs-permanent-clones"></a>Tranziens állandó klónok összehasonlítása

Ideiglenes (tranziens) klónok jönnek létre csak akkor van klónozhatja másik eszközre. Átmásolhatja egy adott mennyiségi egy biztonsági másolatból beállítása a StorSimple kezelő kezelt másik eszközre. Az ideiglenes (tranziens) adatfeliratsor lesz hivatkozások az adatok az eredeti hangerejének és fogja használni a adatokat olvasása és írása helyileg cél az eszközre. 

Után a elvégzi tranziens átirattal felhő pillanatképét, akkor az eredményül kapott adatfeliratsor *állandó* átirattal lesz. A folyamat során jön létre az adatokat egy példányát a felhőben, és az adatok és az Azure késések (Ez az Azure-Azure másolatot a) méretének határozza meg a time to másolandó adatok. Ezt a folyamatot hét napig vehet igénybe. Az ideiglenes (tranziens) adatfeliratsor lesz állandó átirattal ily módon, és nem rendelkezik az eredeti mennyiségi adatok, amely azt át lett klónozva mutató hivatkozásokat. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Az ideiglenes (tranziens) és állandó klónok felhasználási területei

Az alábbi szakaszok ismertetik, például helyzetek, amelyben tranziens és állandó klónok is használható.

### <a name="item-level-recovery-with-a-transient-clone"></a>Az ideiglenes (tranziens) átirattal elemszintű helyreállítás

Egy éves egy Microsoft PowerPoint-bemutató formátumú fájlok helyreállítása szüksége. A rendszergazda segítségét azonosítja a biztonsági másolat az adott időkereten, szűrőt, és a hangerő. A rendszergazda majd klónokat a hangerőt, megtalálta a keresett fájlt, és itt meg. Ebben az esetben tranziens átirattal használják. 
 
![A videó érhető el](./media/storsimple-clone-volume-u2/Video_icon.png) **Videó érhető el**

Nézze meg ezt a videót, mely szemlélteti, hogyan használja az adatfeliratsor, és visszaállíthatja a törölt fájlok StorSimple szolgáltatásai, kattintson [ide](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Az éles üzemi környezetben, az állandó átirattal tesztelése

A tesztelés hiba az éles üzemi környezetben ellenőrizni szeretné. A kötet átirattal létrehozása az éles üzemi környezetben, és majd megteheti ezt adatfeliratsor hozhat létre egy független klónozott mennyiségi felhő pillanatképét. Ebben az esetben az állandó átirattal használják.  

## <a name="next-steps"></a>Következő lépések
- Ismerje meg, visszaállítása [egy biztonsági csoportjából StorSimple mennyiségig](storsimple-restore-from-backup-set-u2.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.

 
