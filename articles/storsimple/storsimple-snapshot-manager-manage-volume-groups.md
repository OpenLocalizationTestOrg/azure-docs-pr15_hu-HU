<properties 
   pageTitle="StorSimple pillanatkép Manager mennyiségi csoportok |} Microsoft Azure"
   description="A hangerő-csoportok létrehozása és kezelése a StorSimple pillanatkép Manager beépülő modult használatát ismerteti."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>-Kezelővel StorSimple pillanatkép mennyiségi csoportok létrehozása és kezelése

## <a name="overview"></a>– Áttekintés

Használhatja a **Mennyiségi csoportok** csomópontot a **hatókör** ablaktáblán a mennyiségi csoportokhoz rendelése kötet, mennyiségi csoportra vonatkozó adatok megtekintése, biztonsági másolatok ütemezése és mennyiségi csoportok szerkesztése. 

Mennyiségi csoportok készletek kapcsolódó mennyiségének biztosítható, hogy a biztonsági mentés alkalmazás egységes is. További tudnivalókért lásd: a [kötet és a hangerő-csoportok](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) és [integráció a Windows kötet árnyék szolgáltatás](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * A mennyiség csoportban az összes kötet egyetlen felhő szolgáltató kell származnia.
> 
> * Ha mennyiségi csoportok állítja be, ne keverje fürt megosztott mennyiség (CSVs) és a CSVs ugyanabban a mennyiség csoportban. StorSimple pillanatkép Manager nem támogatja a azonos pillanatkép vegyesen CSVs és nem CSVs.
 
![Mennyiségi csoportok csomópont](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Ábra 1: StorSimple pillanatkép Manager mennyiségi csoportok csomópont** 

Ebből az oktatóanyagból megtudhatja, hogy StorSimple pillanatkép kezelője használatának:

- A hangerő-csoportok adatainak megtekintése 
- Mennyiségi csoport létrehozása
- Készítsen biztonsági másolatot mennyiségi csoport
- Mennyiségi csoport szerkesztése
- Mennyiségi csoport törlése

Minden műveletet is elérhetők a **Műveletek** ablaktáblában.
 
## <a name="view-volume-groups"></a>Mennyiségi csoportok megtekintése

A **Mennyiségi csoportok** csomópont gombra kattint, ha az **eredményeket** tartalmazó ablaktáblában minden mennyiségi csoport, attól függően, hogy a oszlopos kijelöléseknél, akkor az alábbi információkat jeleníti meg. (Az **eredmény** ablaktáblában szereplő oszlopok állítható be. Kattintson a jobb gombbal a **kötet** csomópontot, **megtekintése**és **Oszlopok hozzáadása/eltávolítása**jelölje be.)

Eredmények oszlop | Leírás 
:--------------|:------------ 
név           | A **név** oszlopban a mennyiségi csoport nevét tartalmazza.
Alkalmazás    | Az **alkalmazások** oszlop VSS szövegírói telepítve és a Windows host futó száma látható.
Kijelölt       | A **kijelölt** oszlopban szereplő kötet száma a mennyiségi csoport láthatók. Nulla (0) azt jelzi, hogy a kötet mennyiségi csoportjának társított alkalmazások nem.
Importált       | A **Imported** oszlop importált kötet száma látható. Ha értéke **Igaz**, ez az oszlop azt jelzi, hogy mennyiségi csoport importálta az Azure klasszikus portálról StorSimple pillanatkép Manager nem jött létre.
 
>[AZURE.NOTE] StorSimple pillanatkép Manager mennyiségi csoportok is megjelennek a **Biztonsági másolat házirendek** lapon az Azure klasszikus portálon.
 
## <a name="create-a-volume-group"></a>Mennyiségi csoport létrehozása

Az alábbi eljárással mennyiségi csoport létrehozásához.

#### <a name="to-create-a-volume-group"></a>Mennyiségi csoport létrehozása

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán kattintson a jobb gombbal a **Mennyiségi csoportok**elemet, és kattintson a **Hangerő csoport létrehozása**gombra. 

    ![Mennyiségi csoport létrehozása](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Ekkor megjelenik a **mennyiségi csoport létrehozása** párbeszédpanel. 

    ![Hozzon létre egy mennyiségi csoport párbeszédpanel](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Írja be az alábbi adatokat: 

    1. A **név** mezőbe írja be egy egyedi nevet az új mennyiségi csoport. 

    2. Az **alkalmazások** legördülő listában válassza a kötet, a mennyiségi csoportba fel társított alkalmazásokat. 

        Az **alkalmazások** mezőben listák csak a használó StorSimple kötet, és rendelkezik VSS szövegírói alkalmazások engedélyezve őket. Egy íróprogramban engedélyezve van, csak ha az író tudatában összes kötet StorSimple kötet. Ha a alkalmazások mező üres, nem használó Azure StorSimple kötet, és támogatták VSS szövegírói alkalmazások vannak telepítve. (Jelenleg Azure StorSimple támogatja a Microsoft Exchange és az SQL Server.) VSS szövegírói kapcsolatos további tudnivalókért olvassa el a [Windows kötet árnyék szolgáltatás integrációja](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service)című témakört.

        Ha kijelöl egy alkalmazást, a vele társított összes kötet automatikusan kijelölt. Viszont ha kijelöl egy adott alkalmazással társított kötet, az alkalmazás automatikusan listában van kiválasztva az **alkalmazást** . 

    3. A **kötet** legördülő listában válassza a mennyiségi csoport hozzáadása StorSimple kötet. 

      - Felvehet egy vagy több partíciót őket. (Több partíciót kötet lehet dinamikus lemez vagy egyszerű lemez több partíciót.) Több partíciót tartalmazó kötet egységként kell kezelni. Ezért vesz fel a partíciók közül csak mennyiségi csoportot, ha a többi partíciók automatikusan megjelennek a mennyiségi csoport egyszerre. Miután egy több partíciót mennyiségi mennyiségi csoportnak, több partíciót hangerejének továbbra is egységként kell kezelni.

      - Nem rendelhet bármely kötet őket üres mennyiségi csoportokat hozhat létre. 

      - Ne keverje fürt megosztott mennyiség (CSVs) és a CSVs ugyanabban a mennyiség csoportban. StorSimple pillanatkép Manager nem támogatja a azonos pillanatkép vegyesen CSV kötet, és nem CSV-kötet. 

4. Kattintson **az OK** gombra a mennyiségi csoport mentéséhez.

## <a name="back-up-a-volume-group"></a>Készítsen biztonsági másolatot mennyiségi csoport

A következő eljárással biztonsági másolatot készíteni a mennyiségi csoportot.

#### <a name="to-back-up-a-volume-group"></a>Biztonsági másolat készítése a mennyiségi csoport

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **Mennyiségi csoportok** csomópontot, kattintson a jobb gombbal a mennyiségi csoport nevét, és kattintson a **Készíthet biztonsági mentése**parancsra. 

    ![Azonnali biztonsági mentése mennyiségi csoport](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. **Készíthet biztonsági másolat** párbeszédpanelen válassza a **Helyi pillanatkép** vagy **Felhőalapú pillanatkép**, és kattintson a **Létrehozás**gombra. 

    ![Biztonsági másolat párbeszédpanelen készítése](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Ellenőrizze, hogy fut-e a biztonsági mentés, bontsa ki a **feladatok** csomópontot, és kattintson a **operációs rendszert futtató**. A biztonsági mentés szerepelnie kell.

5. A kész pillanatkép megtekintéséhez bontsa ki a **Biztonsági másolat katalógus** csomópontot, bontsa ki a mennyiségi csoport nevét, és válassza a **Helyi pillanatfelvétel** vagy **Felhőalapú pillanatkép**. Ha sikeresen befejeződött a biztonsági mentés lesz látható. 

## <a name="edit-a-volume-group"></a>Mennyiségi csoport szerkesztése

A következő eljárással mennyiségi csoport szerkesztése.

#### <a name="to-edit-a-volume-group"></a>Mennyiségi csoport szerkesztése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához.

2. A **hatókör** ablaktáblán bontsa ki a **Mennyiségi csoportok** csomópontot, kattintson a jobb gombbal a mennyiségi csoport nevét, és kattintson a **Szerkesztés**gombra. 

3. Ekkor megjelenik a **mennyiségi csoport létrehozása **párbeszédpanel. Módosíthatja a **nevét**, az **alkalmazások**és a **kötet** bejegyzéseket. 

4. Kattintson az **OK gombra** a módosítások mentéséhez.

## <a name="delete-a-volume-group"></a>Mennyiségi csoport törlése

A következő eljárással mennyiségi csoport törlése. 

>[AZURE.WARNING] A is törli a mennyiségi csoporthoz társított összes biztonsági másolatot.

#### <a name="to-delete-a-volume-group"></a>Mennyiségi csoport törlése

1. Kattintson az asztali ikonra StorSimple pillanatkép kezelő indításához. 

2. A **hatókör** ablaktáblán bontsa ki a **Mennyiségi csoportok** csomópontot, kattintson a jobb gombbal a mennyiségi csoport nevét, és kattintson a **Törlés**parancsra. 

3. A **Mennyiségi csoport törlése** párbeszédpanel jelenik meg. Írja be a **Megerősítés** a szövegmezőbe, és kattintson **az OK**gombra. 

    A törölt mennyiségi csoport eltűnik, az **eredmény** ablaktáblában a listából, és a biztonsági másolat katalógusról törli a mennyiségi csoporthoz társított összes biztonsági másolatot.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [StorSimple pillanatkép Manager a StorSimple megoldás való](storsimple-snapshot-manager-admin.md)használatáról.
- Megtudhatja, hogyan [-kezelővel StorSimple pillanatfelvétel létrehozásának és kezelésének biztonsági házirendek](storsimple-snapshot-manager-manage-backup-policies.md).
