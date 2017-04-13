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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>A mennyiségi klónozhatja a StorSimple kezelő szolgáltatás használatával

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás **Biztonsági** katalógusoldal jönnek létre, ha a kézi és automatikus biztonsági mentés vesznek biztonsági beállítása jeleníti meg. Ezen a lapon egy biztonsági házirend vagy a mennyiség, jelölje be az összes biztonsági másolatot a lista vagy a biztonsági másolatok törlése, vagy biztonsági használata vagy a mennyiségi klónozhatja.

![Biztonsági másolat katalógusoldal](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Ebben az oktatóanyagban ismerteti, hogy miként használhatja az egyes kötet klónozhatja beállítása biztonsági. Azt is bemutatja *tranziens* és *állandó* klónok közötti különbség. 

## <a name="create-a-clone-of-a-volume"></a>Hozzon létre egy kötet átirattal

Ugyanarra az eszközre, egy másik eszközre vagy akár egy virtuális gép átirattal helyi vagy felhőalapú pillanatkép használatával hozhat létre.

#### <a name="to-clone-a-volume"></a>A mennyiségi klónozhatja

1. StorSimple kezelő szolgáltatás lapon kattintson a **biztonsági másolat katalógus** fülre, és válassza a biztonságimásolat-készletet.

2. Bontsa ki a biztonságimásolat-készletet társított kötet megtekintéséhez. Kattintson a gombra, és jelölje ki a mennyiségi a biztonságimásolat-készletet.

     ![A mennyiségi másolása](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Kattintson a **Adatfeliratsor** a kijelölt kötet klónozhatja megkezdéséhez.

4. Csoportban **Adja meg nevét és helyét**a mennyiségi Adatfeliratsor varázsló:

  1. Azonosítsa a célként megadott eszköz. Ez a, a helyet, ahol az adatfeliratsor létrejön. Válassza a ugyanarra az eszközre, vagy adjon meg egy másik eszközt. Ha úgy dönt, egyéb felhő szolgáltatók társított kötet (nem Azure), a legördülő listában a cél eszköz csak akkor látható fizikai eszközök. Egyéb felhő szolgáltatók virtuális eszközön társított kötet nem klónozhatja.

        >  [AZURE.NOTE] Győződjön meg róla, hogy az adatfeliratsor szükséges kapacitásának kisebb, mint a célként megadott eszköz a rendelkezésre álló kapacitás.
  2. Adja meg a adatfeliratsor egyedi mennyiségi nevét. A név 3 és 127 karakter közé kell tartalmaznia.
  3. Kattintson a nyíl ikonra ![nyíl-ikon](./media/storsimple-clone-volume/HCS_ArrowIcon.png) folytassa a következő lapra.

5. Csoportban **Adja meg hosts, használhatja a mennyiségi**:

  1. Adja meg az adatfeliratsor hozzáférési vezérlő rekord (ACR). Felvehet egy új ACR és a meglévő listából választhatja ki.
  2. Kattintson az ellenőrzés ikon ![négyzet-ikon](./media/storsimple-clone-volume/HCS_CheckIcon.png)a művelet végrehajtásához.

6. A adatfeliratsor feladat kezdődik, és értesítést kap a adatfeliratsor sikeresen létrehozásakor. Kattintson a **Feladat megtekintése** Lync- **feladatok** lapon a adatfeliratsor feladatot.

7. Miután az adatfeliratsor feladat befejezése:

  1. Lépjen az **eszközök** lapra, és kattintson a **Hangerő tárolók** fülre. 
  2. Jelölje be a mennyiségi tároló a forrás kötet, amely akkor klónozva társított. A mennyiségek listában az imént létrehozott adatfeliratsor láthatók.

>[AZURE.NOTE] Figyelés és alapértelmezett biztonsági másolatot a rendszer automatikusan letiltotta klónozott kötet.

Az így létrehozott átirattal tranziens átirattal. Adatfeliratsor típusok kapcsolatos további tudnivalókért [tranziens összehasonlítása állandó klónok](#transient-vs.-permanent-clones)témakörben talál.

Ez az adatfeliratsor ettől kezdve a normál kötet, és minden olyan művelet, hogy a kötet lehetőség van az adatfeliratsor az elérhető lesz. Szüksége lesz a mennyiségi bármely biztonsági másolatok konfigurálása.

## <a name="transient-vs-permanent-clones"></a>Tranziens állandó klónok összehasonlítása

Ideiglenes (tranziens) és állandó klónok jönnek létre csak akkor is klónozhatja, egy másik eszközt alkalmazásba című témakör tartalmaz. Átmásolhatja egy adott mennyiségi egy biztonsági másolatból egy másik eszközt szeretne beállítani. Az így létrehozott átirattal *tranziens* átirattal. Az ideiglenes (tranziens) adatfeliratsor lesz az eredeti kötet mutató hivatkozásokat és fogja használni a kötet helyileg írása közben olvasható. 

Után a elvégzi tranziens átirattal felhő pillanatképét, akkor az eredményül kapott adatfeliratsor *állandó* átirattal lesz. Az állandó adatfeliratsor független, és nem rendelkezik az eredeti kötet, amely azt át lett klónozva mutató hivatkozásokat.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Az ideiglenes (tranziens) és állandó klónok felhasználási területei

Az alábbi szakaszok ismertetik, például helyzetek, amelyben tranziens és állandó klónok is használható.

### <a name="item-level-recovery-with-a-transient-clone"></a>Az ideiglenes (tranziens) átirattal elemszintű helyreállítás

Egy éves egy Microsoft PowerPoint-bemutató formátumú fájlok helyreállítása szüksége. A rendszergazda segítségét azonosítja a biztonsági másolat az adott időkereten, szűrőt, és a hangerő. A rendszergazda majd klónokat a hangerőt, megtalálta a keresett fájlt, és itt meg. Ebben az esetben tranziens átirattal használják. 
 
![A videó érhető el](./media/storsimple-clone-volume/Video_icon.png) **Videó érhető el**

Nézze meg ezt a videót, mely szemlélteti, hogyan használja az adatfeliratsor, és visszaállíthatja a törölt fájlok StorSimple szolgáltatásai, kattintson [ide](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Az éles üzemi környezetben, az állandó átirattal tesztelése

A tesztelés hiba az éles üzemi környezetben ellenőrizni szeretné. Úgy hozhat létre a kötet átirattal az éles üzemi környezetben pillanatkép készítése a adatfeliratsor a felhőben. A klónozott mennyiségi ettől független. Ebben az esetben az állandó átirattal használják.

## <a name="next-steps"></a>Következő lépések
- Ismerje meg, visszaállítása [egy biztonsági csoportjából StorSimple mennyiségig](storsimple-restore-from-backup-set.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.

 
