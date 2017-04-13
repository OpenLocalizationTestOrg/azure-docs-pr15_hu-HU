<properties 
   pageTitle="A StorSimple biztonsági házirendek kezelése |} Microsoft Azure"
   description="Ebből a cikkből megtudhatja, hogyan használhatja a StorSimple kezelő szolgáltatás kézi biztonsági mentést, a biztonsági másolat ütemterveket és a biztonsági adatmegőrzési létrehozása és kezelése."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>Biztonsági házirendek kezelése a StorSimple kezelő szolgáltatás használatával

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan segítségével StorSimple kezelő szolgáltatás **Biztonsági házirendek** lapján biztonsági folyamatok és a StorSimple kötet biztonsági adatmegőrzési. Azt is megtudhatja, hogy miként befejezni egy kézi biztonsági mentést.

A **Biztonsági házirendek** lap biztonsági házirendek kezelése és ütemezése helyi és felhőbeli pillanatképek teszi lehetővé. (Biztonsági házirendek biztonsági ütemtervek és mennyiségének gyűjtemény biztonsági adatmegőrzés konfigurálása.) Biztonsági házirendek lehetővé teszi pillanatkép egy több kötet egyidejű. Ez azt jelenti, hogy a biztonsági másolatok egy biztonsági házirendje által létrehozott lesz összeomlik egységes másolatok. Ezen a lapon találhatók a biztonsági házirendeket, azok típusát, társított kötet, tartja meg az biztonsági másolatok számát és a ahhoz, hogy ezek a házirendek lehetőséget.

A **Biztonsági házirendek** lapon is lehetővé teszi a meglévő biztonsági házirendek egyet vagy többet az alábbi mezők szűrése:

- **Házirend neve** – a társított a házirend nevére. A különböző típusú házirendek a következők:

   - Ütemezett házirendek, a felhasználó által létrehozott explicit módon.
   - Az automatikus házirendek, amelyek jönnek létre, ha az alapértelmezett biztonsági másolatot a mennyiségi beállítás engedélyezése a kötet létrehozása idején. Ezek a házirendek elnevezése VolumeName_Default, ahol mennyiségi név utal állítható be a felhasználó az Azure klasszikus portálon StorSimple hangerejének neve megegyezik. Az automatikus házirendek 22:30 eszköz időben kezdődő napi felhő pillanatképek eredményez.
   - Az importált házirendek, a StorSimple pillanatkép Manager eredetileg létrehozott. Ezek van az importált a házirendek az StorSimple pillanatkép Manager állomás leíró címke.

- **Kötet** – a házirend társított kötet. A biztonsági másolat házirend társított összes kötet vannak csoportosítva biztonsági másolatok létrehozásakor.

- **Utolsó sikeres biztonsági másolatból** – a dátum- és a legutóbbi sikeres biztonsági mentés ezzel a házirenddel megtett.

- **Következő biztonsági másolat** – dátuma és időpontja, a következő ütemezett biztonsági másolat, amely a házirend kezdeményezheti.

- **Ütemezés** – a biztonsági másolat házirend társított ütemezések számát.

A gyakran használt műveletek ezen a lapon elvégezhető a következők:

- Biztonsági másolat házirend hozzáadása 
- Hozzáadásához vagy módosításához az ütemezés 
- Biztonsági másolat házirend törlése 
- A kézi biztonsági másolat készítése 
- Hozzon létre egy egyéni biztonsági házirendet több kötet és az ütemtervekről 

## <a name="add-a-backup-policy"></a>Biztonsági másolat házirend hozzáadása

A biztonsági másolatok automatikusan ütemezése biztonsági házirend hozzáadása. A következő lépésekkel StorSimple mobileszközére biztonsági házirend hozzáadása az Azure klasszikus portálon. Miután felvette a házirendet, megadhatja az ütemezés (lásd: [hozzáadása vagy módosítása az ütemezett](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![A videó érhető el](./media/storsimple-manage-backup-policies/Video_icon.png) **Videó érhető el**

Nézze meg ezt a videót, mely szemlélteti, hogyan hozhatja létre a helyi, és a felhő biztonsági házirend, kattintson [ide](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Hozzáadásához vagy módosításához az ütemezés

Felvehet és módosítása meglévő biztonsági házirend StorSimple eszközén csatolva van ütemezett. A következő lépésekkel hozzáadásához vagy módosításához az ütemezés az Azure klasszikus portálon.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Biztonsági másolat házirend törlése

A következő lépésekkel StorSimple eszközén biztonsági házirend törlése az Azure klasszikus portálon.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>A kézi biztonsági másolat készítése

Az igény szerinti (kézi) biztonsági másolat létrehozása egyetlen kötet az Azure klasszikus portálon, hajtsa végre az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Hozzon létre egy egyéni biztonsági házirendet több kötet és az ütemtervekről

Az Azure klasszikus portál több kötet és az ütemtervekről tartalmazó egyéni biztonsági házirendek létrehozásához kövesse az alábbi lépéseket.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Következő lépések

További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
