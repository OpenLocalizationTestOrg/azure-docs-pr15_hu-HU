<properties 
   pageTitle="Megtekintheti és kezelheti a feladatokat StorSimple |} Microsoft Azure"
   description="A StorSimple kezelő szolgáltatás feladatok lap és a legutóbbi, a jelenlegi és az ütemezett biztonsági feladatok követéséhez használatához ismerteti."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>A StorSimple kezelő szolgáltatás használatával megtekintheti, és StorSimple feladatok kezelése

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>– Áttekintés

A **feladatok** lapon a egyetlen központi portál nyújt a megtekintés és a StorSimple kezelő szolgáltatás csatlakoztatott eszközökön elindító feladatok kezelése. Ütemezett, futó, elvégzett és a sikertelen feladatok több eszközök tekinthetők meg. Eredmények táblázatos formában jelennek meg. 

![Feladatok lap](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Gyorsan megtalálhatja a feladatokat, például: leszűrése mezők érdekeltek a:

- **Állapot** – feladatok futtatható, ütemezett, nem sikerült, kész, sztornószámla vagy visszavont.

- **Típus** – feladatok ütemezett vagy az igény szerinti (**Biztonsági másolat készítése**) biztonsági másolatot, klónozhatja, egy eszköz visszaállítása vagy frissítési művelet eredményeként hozhatók létre.

- **Eszközök** – a szolgáltatáshoz való kapcsolódás eszközön kezdeményezett feladatokat.

- A **feladó és címzett** – feladatok a dátum és idő tartomány alapján szűrhetők.

A szűrt feladatok majd megjelennének az alábbi attribútumok alapján:

- **Típus** – biztonsági másolat, adatfeliratsor, visszaállítása, feladatátvevő, vagy frissítést.

- **Állapot** – fut, az ütemezett, nem sikerült, kész, sztornószámla vagy visszavont.

- **Szervezet** – a feladatok kapcsolható össze a kötet, egy biztonsági házirend vagy egy eszközt. A adatfeliratsor feladat társítva a kötet, mivel az ütemezett biztonságimásolat-feladat társítva a biztonsági másolat házirend. Egy eszköz feladatot egy vészhelyreállítás (DR) vagy a visszaállítási művelet eredményeként jön létre.

- **Eszköz** – az eszköz a nevére, kattintson a feladat indított.

- **A lépések** – az idő, hogy a projekt mikor lett elindítva.

- **Folyamatban** – az aktív feladat százalékos végrehajtására. Egy befejezett projekt 100 %-os mindig ki kell lennie.

A feladatok listáját 30 másodpercenként frissül.

Ezen a lapon a projekttel kapcsolatos alábbi műveleteket végezheti el:

- Feladat részletei

- Egy feladat megszakítása

## <a name="view-job-details"></a>Feladat részletei

Hajtsa végre az alábbi lépéseket minden feladat részleteinek megtekintéséhez.

#### <a name="to-view-job-details"></a>Feladat részletei

1. A **feladatok** lapon a megfelelő szűrőkkel lekérdezés futtatásával érdekeltek a feladat megjelenítése A kész, fut, vagy ha megszakad a feladatok is kereshet.

2. Válasszon ki egy feladatot.

3. A lap alján kattintson a **Részletek**gombra.

4. **Biztonsági mentési feladat részletei** párbeszédpanelen a állapot, részleteit, statisztika és adatok statisztikai tekinthet meg.

## <a name="cancel-a-job"></a>Egy feladat megszakítása

A következő lépésekkel mondhatja le egy futó feladatot.

### <a name="to-cancel-a-job"></a>A megszakításhoz feladatot

1. A **feladatok** lapon a megfelelő szűrőkkel lekérdezés futtatásával megszakításhoz kívánt futó feladat megjelenítése

1. Jelölje ki a feladatot.

1. A lap alján kattintson a **Mégse gombra**.

1. Amikor a program megerősítést kér, kattintson az **Igen**gombra.

A feladat most megszakad.

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogy miként [kezelheti a StorSimple biztonsági házirendek](storsimple-manage-backup-policies.md).

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
