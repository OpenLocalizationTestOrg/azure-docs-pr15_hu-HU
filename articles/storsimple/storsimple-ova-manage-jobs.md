<properties 
   pageTitle="Megtekintheti és kezelheti a feladatokat StorSimple virtuális tömb |} Microsoft Azure"
   description="A StorSimple kezelő szolgáltatás feladatok lap és a legutóbbi és az aktuális feladatok követéséhez a StorSimple virtuális tömb használatához ismerteti."
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
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Feladatok megtekintése a StorSimple virtuális tömb a StorSimple kezelő szolgáltatás használatával

## <a name="overview"></a>– Áttekintés

A **feladatok** lap megtekintésére és kezelésére, feladatok, amelyeket a StorSimple kezelő szolgáltatás csatlakoztatott virtuális tömbök (más néven helyszíni virtuális eszközök) elindító egyetlen központi portál biztosít. Virtuális különféle eszközökön futó, elvégzett és a sikertelen feladatok megtekintése. Eredmények táblázatos formában jelennek meg. 

![Feladatok lap](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Gyorsan megtalálhatja a feladatokat, például: leszűrése mezők érdekeltek a:

- **Állapot** – lehet keresni az összes futó, kész vagy sikertelen feladatokhoz.
- A **feladó és címzett** – feladatok a dátum és idő tartomány alapján szűrhetők.
- **Típus** – a projekt típusa kell az összes, visszaállítási, feladatátvevő, töltse le a frissítések vagy frissítések telepítéséhez.
- **Eszközök** – a szolgáltatáshoz való kapcsolódás egy adott eszközön kezdeményezett feladatokat. A szűrt feladatok majd megjelennének az alábbi attribútumok alapján:

    - **Típus** – a projekt típusa kell az összes, visszaállítási, feladatátvevő, töltse le a frissítések vagy frissítések telepítéséhez.

    - **Állapot** – feladatok lehet összes fut, befejezve, vagy nem sikerült.

    - **Szervezet** – lehet, hogy a feladatok mennyiségi, a megosztás vagy a eszköz társítva. 

    - **Eszköz** – az eszköz a nevére, kattintson a feladat indított.

    - **A lépések** – az idő, hogy a projekt mikor lett elindítva.

    - **Folyamatban** – az aktív feladat százalékos végrehajtására. Egy befejezett projekt 100 %-os mindig ki kell lennie.

A feladatok listáját 30 másodpercenként frissül.

## <a name="view-job-details"></a>Feladat részletei

Hajtsa végre az alábbi lépéseket minden feladat részleteinek megtekintéséhez.

#### <a name="to-view-job-details"></a>Feladat részletei

1. A **feladatok** lapon a megfelelő szűrőkkel lekérdezés futtatásával érdekeltek a feladat megjelenítése Befejezett vagy futó feladatok is kereshet.

2. Jelölje ki a feladat feladatok táblázatos listája.

3. A lap alján kattintson a **Részletek**gombra.

4. A **részletes** párbeszédpanel állapotát, a és a statisztika tekinthet meg. Az alábbi ábrán egy példát a **Biztonsági mentési feladat részletei** párbeszédpanel.
 
    ![Feladat részletei lapon](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Ha a virtuális gép fel van függesztve a hipervizor a feladat hibák

Amikor a projekt előrehaladását a StorSimple virtuális tömb és az eszköz (virtuális számítógép hipervizor rendszerbeli) fel van függesztve, a nagyobb, mint 15 percet, a feladat sikertelen lesz. Ez a StorSimple virtuális tömb idő miatt folyamatban van nincs szinkronban a Microsoft Azure időt. Példa egy visszaállítási feladat sikertelen az alábbi képernyőképen látható.

![Feladat hiba visszaállítása](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

E hibák biztonsági mentése, visszaállítása, frissítése és feladatátvevő feladatok érvényesek lesznek. A virtuális gép a Hyper-V már kiépítve, ha a számítógép ahányat szinkronizálása idő a hipervizor. Ebben az esetben, ha, indítsa újra a feladatot. 

## <a name="next-steps"></a>Következő lépések

[A helyi webes felügyelheti a StorSimple virtuális tömb felhasználói felület használata](storsimple-ova-web-ui-admin.md).
