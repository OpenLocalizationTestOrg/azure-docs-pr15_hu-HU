<properties 
   pageTitle="Access-vezérlő rekordok kezelése a StorSimple virtuális tömb |} Microsoft Azure"
   description="Ismerteti, hogyan szeretné kezelni az access vezérlő rekordokat (ACRs) határozza meg, mely állomások csatlakoztathatja virtuális StorSimple tömb a kötet."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Access-vezérlő rekordok kezelése a StorSimple virtuális tömb a StorSimple kezelő szolgáltatás használatával 

## <a name="overview"></a>– Áttekintés

Az Access vezérlő rekordok (ACRs) lehetővé teszik, hogy adja meg, mely a hosts StorSimple virtuális tömbképletek (más néven a StorSimple helyszíni virtuális eszköz) a kötet csatlakozhat. ACRs vannak beállítva, hogy egy adott mennyiségi, és a iSCSI minősített neve (IQN-nevekre vonatkozóan) a hosts tartalmaz. Amikor állomás kötet csatlakozni próbál létrehozni, az eszköz ellenőrzi az, hogy a IQN jelölőnégyzetét mennyiségi társított ACR, és ha egyező, majd a kapcsolat folyamatban van. Az access vezérlő ( **rekordok** ) szakaszában kattintson **a lap** az access vezérlőt tartalmazó rekordokat a megfelelő IQN-nevekre vonatkozóan állomások jeleníti meg.

Ebből az oktatóanyagból megtudhatja, a következő gyakori ACR kapcsolódó műveletek:

- A IQN beszerzése
- Az access vezérlő rekord létrehozása 
- Az access vezérlő rekord szerkesztése 
- Az access vezérlő rekord törlése 

> [AZURE.IMPORTANT] 
> 
> - Ha egy ACR hozzárendelése a kötet, gondoskodik, hogy a kötet sem egyidejűleg elérhető több-a csoportosított host mivel ez lehet sérült a mennyiségi. 
> - Egy ACR törlése a kötet, ügyeljen az, hogy a megfelelő állomás nem éri el a hangerőt, mert a törlés eredményezhet egy írási és olvasási zavarok.

## <a name="get-the-iqn"></a>A IQN beszerzése

Hajtsa végre az alábbi lépéseket a Windows Server 2012-kiszolgáló állomás Windows IQN eléréséhez.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Egy ACR hozzáadása

ACRs, kövesse az StorSimple kezelő szolgáltatás **beállítása** lapon. Általában egy ACR fog társítása egy mennyiségi.

A hangerő-ACR társítása tudni lépjen a [kötet hozzáadása](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Ha egy ACR hozzárendelése a kötet, gondoskodik, hogy a kötet sem egyidejűleg elérhető több-a csoportosított host mivel ez lehet sérült a mennyiségi.
 
A következő lépésekkel hozzáadása egy ACR.

#### <a name="to-add-an-acr"></a>Egy ACR hozzáadása

1. A szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatás, és válassza a **beállítás** lapon.

    ![konfiguráció lapon](./media/storsimple-ova-manage-acrs/acr1.png)

2. Az **Access vezérlő rekordok**csoportjában a táblázatos listaelem adnia a ACR **nevét** .

3. **ISCSI kezdeményező neve**csoportban adja meg a Windows-szolgáltató IQN nevét. 

4. Kattintson a **Mentés** elemre a lap alján az újonnan létrehozott ACR mentéséhez. A következő megerősítő üzenetet fog látni.

    ![megerősítést kérő üzenet](./media/storsimple-ova-manage-acrs/acr2.png)

5. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-manage-acrs/check-icon.png). A táblázatos nevére a partnerlistában a kiegészítés megfelelően frissül.

## <a name="edit-an-acr"></a>Egy ACR szerkesztése

Használatával az **beállítása** lapon az Azure klasszikus portálon ACRs szerkesztése. 

> [AZURE.NOTE] Csak a jelenleg nem használt ACRs kell módosítani. Egy olyan kötet, amely jelenleg használatban van társítva ACR szerkesztéséhez, érdemes először a mennyiségi offline állapotba.

Hajtsa végre az alábbi lépések végrehajtásával szerkesztheti egy ACR.

#### <a name="to-edit-an-acr"></a>Egy ACR szerkesztése

1. A szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatás, és válassza a **beállítás** lapon.

2. Az access vezérlő rekordok táblázatos listája, mutasson a módosítani kívánt ACR.

3. Adja meg egy új nevet, illetve IQN a ACR.

4. Kattintson a **Mentés** elemre a lap alján menteni a módosított ACR. A megerősítést kérő üzenet jelenik meg. 

5. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-manage-acrs/check-icon.png). A táblázatos nevére a partnerlistában a módosítás megfelelően frissül.

## <a name="delete-an-access-control-record"></a>Az access vezérlő rekord törlése

Használatával az **beállítása** lapon az Azure klasszikus portálon ACRs törlése. 

> [AZURE.NOTE] 
> 
> - Csak a jelenleg nem használja ACRs törölnie kell. Ha törölni szeretne egy olyan kötet, amely jelenleg használatban van társítva ACR, meg kell először a mennyiségi offline állapotba.
> - Egy ACR törlése a kötet, ügyeljen az, hogy a megfelelő állomás nem éri el a hangerőt, mert a törlés eredményezhet egy írási és olvasási zavarok.

A következő lépésekkel egy access-vezérlő rekord.

#### <a name="to-delete-an-access-control-record"></a>Az access vezérlő rekord törlése

1. A szolgáltatás érkezési lapon jelölje ki azt a szolgáltatást, kattintson duplán a szolgáltatás, és válassza a **beállítás** lapon.

2. Az access-vezérlő rekordok (ACRs) táblázatos listája, mutasson a törölni kívánt ACR.

3. A Törlés (**x**) ikonra a választott ACR a rendkívüli jobb oldali oszlopban fog megjelenni. Kattintson az **x** ikonra a ACR törlése. A következő megerősítő üzenetet fog látni.

    ![megerősítést kérő üzenet](./media/storsimple-ova-manage-acrs/acr3.png)

5. Kattintson az ellenőrzés ikon ![Jelölje be a ikon](./media/storsimple-ova-manage-acrs/check-icon.png). A táblázatos nevére a partnerlistában a törlés megfelelően frissül.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [kötet felvételével és konfigurálásával a ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
