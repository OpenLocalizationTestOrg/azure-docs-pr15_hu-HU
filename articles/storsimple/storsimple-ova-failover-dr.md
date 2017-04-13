<properties
   pageTitle="A StorSimple virtuális tömb katasztrófa helyreállítási és eszköz feladatátvétele"
   description="Tudjon meg többet átadni a StorSimple virtuális tömb."
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

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>A StorSimple virtuális tömb katasztrófa helyreállítási és eszköz feladatátvétele


## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti a vészhelyreállítás a Microsoft Azure StorSimple virtuális tömbképletek (más néven a StorSimple helyszíni virtuális eszköz) együtt katasztrófa esetén a virtuális egy másik eszközre átadni a részletes lépéseket. Feladatátvevő lehetővé teszi az adatok a *forrás* eszközről a adatközpontban áttelepítése ugyanabban vagy egy másik földrajzi helye, amely egy másik *cél* eszközre. Az eszköz feladatátvevő nem a teljes eszközre. Feladatátvételkor a forrás eszköz adatainak felhőbe módosításaira tulajdonjogát, amely a célként megadott eszköz.

Eszköz feladatátvétel a katasztrófa helyreállítási (DR) funkcióval orchestrated van, és az **eszközök** lapon kezdeményezni. Ezen az oldalon a StorSimple Manager szolgáltatáshoz való kapcsolódás StorSimple eszközök vannak. Az egyes eszközök a felhasználóbarát név, állapot, kiépített és maximális beosztását, típus és modell jelennek meg.

![](./media/storsimple-ova-failover-dr/image15.png)

Ez a cikk csak olyan StorSimple virtuális tömbök alkalmazható. Nem sikerül egy 8000 sorozat eszközön fölé, keresse fel a [feladatátvevő és a helyreállítás StorSimple eszköz](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Mi az vészhelyreállítás?

Az elsődleges eszközt egy visszaállításához összeomlást követő helyreállítás (DR) során nem működik. Ebben az esetben a áthelyezheti az elsődleges eszközt használ a *forrás* , és egy másik eszközre megadásával és a *cél*egy másik hibás eszközzel tartozó felhő adatokat. Ez az eljárás a *feladatátvevő*nevezik. Feladatátvételkor összes kötet vagy a forrás eszközről a megosztások tulajdonosának módosítása, és a célként megadott eszköz átkerülnek. Az adatok szűrése nem engedélyezett.

Egy teljes eszköz visszaállítása a hőtérkép – térképalapú tiering használ, és nyomon követése, DR modellezni. Hőtérkép hőtérképen nincs érték hozzárendelése az adatok alapján határozza meg írási és olvasási mintázatok. A hőtérkép feleltesse majd Rétegek a legalacsonyabb hőtérkép adatblokkra a felhőbe először a helyi réteg a nagy meleg (leggyakrabban használt) adatblokkra tartásával. A hőtérkép során egy DR, szolgál állítani, és az adatokat a felhőből rehydrate. Az eszköz beolvassa az összes a kötet és megosztás a legutóbbi utolsó biztonságimásolat-(meghatározott belső), és a visszaállítás végrehajtja a biztonsági másolat. A teljes DR folyamat az eszköz van orchestrated.


## <a name="prerequisites-for-device-failover"></a>Az eszköz feladatátvevő vonatkozó követelmények


### <a name="prerequisites"></a>Előfeltételek

Bármilyen eszközön feladatátvételi az alábbi előfeltételek teljesülnie kell:

- Az adatforrás eszköz kell **inaktívvá válnak** állapotban.

- A célként megadott eszköz kell az Azure klasszikus portálon **aktív** jelenik meg. Szüksége lesz a cél virtuális eszköz ugyanabban vagy egy újabb kapacitásának hozhatók létre. A helyi webes felhasználói felület majd beállítása és a virtuális eszköz sikerült regisztrálni kell használni.

    > [AZURE.IMPORTANT] Próbálja meg a szolgáltatáson keresztül regisztrált virtuális eszköz beállítása **teljes eszköz beállítása**gombra kattintva. Nincs eszközök konfigurálása a szolgáltatáson keresztül kell elvégezni.

- A forrás- és célwebhelyek eszközt kell lennie az azonos típusú. Csak végződhet fölé egy másik fájlkiszolgálóra fájlkiszolgálóra konfigurált virtuális eszközt. Ugyanez igaz-iSCSI kiszolgálóhoz.

- Fájlkiszolgálóra DR ajánlott csatlakozik a célként megadott eszköz ugyanahhoz a tartományhoz, mint amit a forrás, hogy automatikusan megtörténik a megosztási engedélyeket. Csak a célként megadott eszköz ugyanabban a tartományban való áttérés támogatja a jelenlegi kiadásába.

### <a name="other-considerations"></a>Egyéb szempontok

- Azt javasoljuk, hogy Ön által a kötet vagy a megosztás offline adatforrás eszközre.

- Ha tervezett feladatátvevő, azt javasoljuk, hogy meg az eszköz biztonsági másolatot készíthet, majd folytassa a feladatátvételi adatvesztés minimalizálásához. Ha feladatátvételhez, a legutóbbi biztonsági mentés használandó visszaállítja az eszköz.

- A rendelkezésre álló cél DR az-e eszközök és összehasonlítása az adatforrás eszköz ugyanazon vagy nagyobb kapacitása. A szolgáltatás kapcsolódik, de nem felelnek meg a feltételnek megfelelő terület eszközök nem érhető el a cél eszköz.

### <a name="dr-prechecks"></a>DR prechecks

A DR megkezdése előtt prechecks az eszközön hajt végre. Ezek az ellenőrzések biztosíthatja, hogy nincs hiba fordul elő, amikor DR megkezdése. A prechecks a következők:

- A tárterület-fiók ellenőrzése

- A felhőbeli kapcsolat Azure ellenőrzése

- Rendelkezésre álló hely a célként megadott eszköz ellenőrzése

- Annak ellenőrzése, ha egy iSCSI server adatforrás eszköz érvényes ACR nevek, IQN (legfeljebb 220 karakter hosszúságú) és CHAP jelszavát (12 és 16 karakter hosszúságú) társított kötet

Ha nem sikerül a fenti prechecks közül, nem folytatja a DR. Ezek a problémák megoldásához, és próbálkozzon újra a DR szüksége.

Miután sikeresen befejeződött a DR, az a felhő adatok az adatforrás eszközön tulajdonjogát átkerül a célként megadott eszköz. Az adatforrás eszköz már majd nem érhető el a portálon. Hozzáférés az összes kötet és az adatforrás eszközön zárolva van, és a célként megadott eszköz aktívvá válik.

> [AZURE.IMPORTANT]
> 
> Abban az esetben, ha az eszköz már nem érhető el, a virtuális gép, amely a host rendszeren, kiépítéstől továbbra is van felhasználás erőforrásokat. Miután sikeresen befejeződött a DR, a host rendszerekből törölheti a virtuális számítógéphez.

## <a name="fail-over-to-a-virtual-array"></a>A virtuális tömb átadni

Azt javasoljuk, hogy egy másik StorSimple virtuális tömböt kiépítve konfigurált keresztül a helyi webes felhasználói felület és a StorSimple-kezelő szolgáltatás, ez az eljárás futtatása előtt regisztrált.


> [AZURE.IMPORTANT]
> 
> - Nem engedélyezett átadni a StorSimple 8000 sorozat eszközről 1200 virtuális eszközre.
> - A eszközről Federal információk Processing Standard (FIPS) engedélyezett virtuális kormányzati portálon klasszikus portálon Azure virtuális eszközre telepítve végződhet fölé. Fordítva is igaz.

A következő lépésekkel visszaállítja az eszköz a célként megadott StorSimple virtuális eszköz.

1. Kötet/megosztások offline állapotba az állomáson. Olvassa el az operációs rendszer – útmutatást az állomáson a kötet megosztások offline állapotba. Ha nincs már offline, meg kell összes a kötet/megosztás offline állapotba az eszközön ismételt megjelenítéséhez **Eszközök > megosztás** (vagy **eszköz > kötet**). Válassza a megosztás kötet, majd kattintson a **offline állapotba** az oldal aljára. Amikor a program megerősítést kér, kattintson az **Igen**gombra. Ismételje meg ezt a folyamatot az összes a megosztás/kötet az eszközön.

2. Az **eszközök** lapon válassza ki a forrást eszközt feladatátvételi, és kattintson az **Inaktiválás**gombra. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Megerősítését kéri. Eszköz az Inaktiválás következő állandó áll, amely nem vonható vissza. Fog is emlékezteti a megosztás/kötet offline állapotba az állomáson.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Amint megerősíti az Inaktiválás fog elindulni. Miután az Inaktiválás sikeresen befejeződött, hogy értesítést kap.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Az **eszközök** lapon az Eszközállapot most változik **inaktívvá válnak**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Jelölje ki a inaktiválva eszközt, majd a lap alján kattintson a **feladatátvevő**.

6. A megerősítés feladatátvevő varázslóban fel a megnyíló tegye a következőket:

    1. Használható eszközök legördülő listából válassza ki a **cél eszköz.** Csak azokat az eszközöket, hogy van elegendő kapacitása jelenik meg a legördülő listából.

    2. Tekintse át a részleteket, például eszköz neve, teljes kapacitásának és a neveket a megosztás, amely fölé sikertelen lesz az adatforrás eszköz társított.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Jelölje be az **e elfogadja feladatátvevő végleges művelet és a feladatátvételi sikeresen befejeződött, miután az adatforrás eszköz is törli**.

8. Kattintson az ellenőrzés ikon ![](./media/storsimple-ova-failover-dr/image1.png).


9. A feladatátvevő feladat kezdődik, és értesítést kap. Kattintson a **feladat megtekintése** Lync-a feladatátvételt.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. A **feladatok** lapon a forrás-eszközökre készült létrehozott feladatátvevő feladat jelenik meg. Ezt a feladatot a DR prechecks hajt végre.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Után a DR prechecks sikeresek, a feladatátvevő feladat visszaállítási feladatok minden megosztás/kötet, amely az adatforrás eszközön létezik fog elindítanak.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. A feladatátvételi befejezése után nyissa meg az **eszközök** lapra.

    egy. Jelölje ki a StorSimple virtuális eszköz használata a célként megadott eszköz az feladatátvevő folyamatot.

    b. Válassza a **megosztás** lapon ( **kötet** , vagy ha iSCSI server). A régi eszközről összes megosztás (kötet) most szerepelnie kell.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**A videó érhető el**

A videó bemutatja, hogyan lehet nem sikerül StorSimple helyszíni virtuális eszköz virtuális egy másik készülékre fölé.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Üzleti folytonosságot vészhelyreállítás (BCDR)

Egy üzleti folytonosságot visszaállításához összeomlást követő helyreállítás (BCDR) során akkor fordul elő, ha a teljes Azure adatközponthoz nem működik. Ez hatással lehet a StorSimple kezelő szolgáltatás és az ahhoz tartozó StorSimple berendezések.

Ha regisztráltak, mielőtt a katasztrófa történt StorSimple eszközök, StorSimple ezekre az eszközökre lehet törölni. Után a katasztrófa hozza létre, és állítsa be ezeket az eszközöket.

## <a name="errors-during-dr"></a>Hibák DR során

**Felhőbeli kapcsolat üzemszünetek DR során**

A felhőbeli kapcsolat megszakadt DR indítása után, ha és az eszközhöz visszaállítása befejezése előtt a DR meghiúsul, és értesítést kap. A célként megadott eszköz DR használt majd van megjelölve *használhatatlanná.* Cél ugyanarra az eszközre a jövőbeli DRs majd nem használhatók.

**Nem kompatibilis a cél eszköz**

Ha rendelkezésre a Céllista eszközök nincs elegendő hely, a célból, hogy nincsenek-e a cél kompatibilis eszközök hiba jelenik meg.

**Precheck hibák**

Ha a prechecks egyike nem teljesül, majd megjelenik precheck hibák.

## <a name="next-steps"></a>Következő lépések

További tudnivalók [A felhasználói felület helyi webes StorSimple virtuális tömb](storsimple-ova-web-ui-admin.md)felügyelete.
