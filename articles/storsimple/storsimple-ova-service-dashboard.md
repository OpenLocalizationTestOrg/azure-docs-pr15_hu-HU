<properties 
   pageTitle="StorSimple kezelő szolgáltatás irányítópult - virtuális tömb |} Microsoft Azure"
   description="A StorSimple kezelő szolgáltatás irányítópult és Lync-állapota az StorSimple virtuális tömb használandó ismerteti."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>A StorSimple virtuális tömb StorSimple kezelő szolgáltatás irányítópult használata

## <a name="overview"></a>– Áttekintés

A StorSimple kezelő szolgáltatás Irányítópultlap StorSimple virtuális tömb (más néven StorSimple helyszíni virtuális eszközök vagy a virtuális eszközök), amelyeket a StorSimple-kezelő szolgáltatás csatlakoztatott kiemeli azokat a rendszergazdának figyelmet igénylő összegzés nézet nyújt. Ebben az oktatóanyagban vezet be az irányítópult oldalát ismerteti, hogy az irányítópult-tartalmak és a függvény és az ezen a lapon elvégezhető feladatokat ismerteti.

![Szolgáltatás-irányítópult](./media/storsimple-ova-service-dashboard/dashboard1.png)

A StorSimple kezelő szolgáltatás irányítópult jeleníti meg az alábbi adatokat:

- A **diagram** területére, kattintson a lap tetején akkor a virtuális eszközök a vonatkozó mérőszámok láthatja. Megtekintheti az összes virtuális eszközökön keresztül használt elsődleges tárolására, valamint a felhőbeli tárhelyről egy időszakra vetített állapotával virtuális eszközök által használt. Relatív és abszolút használatát adja meg, és 1 hét napjait, 1 hónap, 3-as havi vagy éves 1 időosztás lásd: a vezérlők használata a diagram jobb felső sarkában. A frissítési szabályok használata ![frissítés-vezérlők](./media/storsimple-ova-service-dashboard/refresh-control.png) frissítheti a diagram.

- Az **Áttekintés használatát** az elsődleges tároló kiépítéstől és minden virtuális eszközökön keresztül érhető el a teljes tárterület viszonyított összes virtuális eszközök elfogyasztott jeleníti meg. **Provisioned** hivatkozik, amely készíteni, és használatra lefoglalt tároló mennyiségét **használt** hivatkozik megosztások vagy kötet, lehetőség van: a virtuális eszközök csatlakozó kezdeményezők használatát és **Max. kapacitás** jeleníti meg a maximális összes virtuális eszközök kapacitása.

- Az **értesítések** területen az aktív riasztások pillanatképét lehetővé teszi az virtuális eszköz, értesítés súlyosságát szerint csoportosítva. A súlyosságát szint gombra kattintva megnyílik a **riasztások** lapon, az adott értesítések megjelenítése korlátozódik. A **riasztások** lapon megtekintheti, hogy az értesítést, beleértve az esetleges javasolt műveleteket adatait egyéni figyelmeztető üzenet kattinthat. Ha a probléma megoldódott is törölheti az értesítésre.

- A **projektek** terület legutóbbi feladatok pillanatképét virtuális eszközök csatlakozik a szolgáltatás lehetővé teszi. Nincsenek nézze meg, amely jelenleg folyamatban lévő és azok sikeres és sikertelen az elmúlt 24 óra a feladatok használható hivatkozások. 

- A **fontos** terület jobb oldalán lévő a lapon a szolgáltatás állapota száma a szolgáltatást, a szolgáltatás helyét, és a szolgáltatás társított az előfizetés részletei csatlakoztatott virtuális eszközök például hasznos információt nyújt. A műveletek napló mutató hivatkozást is van. Kattintson a hivatkozásra egy befejezett StorSimple kezelő szolgáltatás műveletek listájának megjelenítéséhez. 

Használhatja a StorSimple kezelő szolgáltatás Irányítópultlap kezdeményezése az alábbi műveleteket:

- Ismerkedés a szolgáltatás regisztrációs billentyűt.
- A művelet naplók megtekintése.

## <a name="get-the-service-registration-key"></a>A szolgáltatás regisztrációs kulcs beszerzése

A szolgáltatás regisztrációs kulcs StorSimple virtuális eszköz regisztrálhatja a StorSimple Manager szolgáltatással használják, hogy az eszköz további adatkezelési műveletek az Azure klasszikus portálon jelenik meg. A kulcs az első virtuális eszközön létrejön, és a hátralévő virtuális eszközök megosztott. 

**Regisztráció kulcs** (az oldal alján) gombra kattintva megnyílik a **Szolgáltatás regisztrációs kulcs** párbeszédpanel, ahol válassza az aktuális szolgáltatás regisztrációs kulcs másolja a vágólapra, vagy a szolgáltatás regisztrációs kulcs újragenerálása.

![regisztráció billentyűt](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

A kulcs újragenerálása nem befolyásolja az előzőleg regisztrált virtuális eszközök: csak a virtuális eszközök regisztrált a szolgáltatással után a kulcs jön hatással van.

További információt a szolgáltatás regisztrációs kulcs első kattintson [a szolgáltatás regisztrációs kulcs kérése](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>A műveletek naplók megtekintése

A művelet naplók érhető el, a **quick** Glance az irányítópult művelet naplók hivatkozására kattintva tekinthet meg. Ekkor a kezelés Szolgáltatások lapot, ahol szűréséhez és a StorSimple kezelő szolgáltatás a naplók adott című témakör tartalmazza.

![Jelentkezzen be a műveletek](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a helyi webes felhasználói felület a StorSimple virtuális tömb való](storsimple-ova-web-ui-admin.md)használatáról.