<properties 
   pageTitle="StorSimple Manager virtuális tömb felügyeleti |} Microsoft Azure"
   description="Megtudhatja, hogy miként kezelheti a StorSimple helyszíni virtuális tömb az Azure klasszikus portálon a StorSimple kezelő szolgáltatás használatával."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>A StorSimple kezelő szolgáltatás használatával felügyelheti a StorSimple virtuális tömb

![beállítási folyamatot.](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti a StorSimple-kezelő szolgáltatás felület, például hogy miként csatlakozhat, és a rendelkezésre álló különböző beállításokat, és hivatkozásokra kattintva az adott munkafolyamatok, a felhasználói felület keresztül elvégezhető. 

Ez a cikk elolvasása, után, tudni fogja, hogy miként:

- Csatlakozás a StorSimple kezelő szolgáltatás
- Nyissa meg a StorSimple Manager felhasználói felület
- A Service StorSimple Manager StorSimple virtuális tömb felügyelete

> [AZURE.NOTE] Elérhető a StorSimple 8000 sorozat eszköz adatkezelési beállítások megtekintéséhez nyissa meg [a StorSimple Manager szolgáltatással való StorSimple eszközére](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Csatlakozás a StorSimple kezelő szolgáltatás

A StorSimple kezelő szolgáltatás fut, a Microsoft Azure-ban, és több StorSimple virtuális tömbök kapcsolódik. Ezekre az eszközökre kezelése a böngészőben futó központi Microsoft Azure klasszikus portál segítségével. Csatlakozás a StorSimple kezelő szolgáltatás, tegye a következőket.

#### <a name="to-connect-to-the-service"></a>A szolgáltatáshoz való kapcsolódáshoz

1. Nyissa meg a [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Használja a Microsoft-fiókja hitelesítő adatait, jelentkezzen be a Microsoft Azure klasszikus portálra (az ablak jobbra található).

3. Görgessen lefelé a bal oldali navigációs a StorSimple kezelő szolgáltatás eléréséhez.

    ![görgessen a szolgáltatás](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Nyissa meg a StorSimple-kezelő szolgáltatás felhasználói felület

A navigációs hierarchiában a StorSimple kezelő szolgáltatás felhasználói felület az alábbi táblázatban látható.

- A **Kezelő StorSimple** céloldal megnyitja a felhasználói felület szolgáltatói lapok alkalmazandó összes virtuális tömbök szolgáltatás belül.

- Az **eszközök** lap megnyitja a eszközszintű – Felhasználóifelület-lapok alkalmazható egy adott virtuális tömböt.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple kezelő szolgáltatás navigációs hierarchia

|Kezdőlapja|Szolgáltatói lapok|Eszköz szintű lapok|
|---|---|---|
|StorSimple kezelő szolgáltatás|Irányítópult (szolgáltatás)|Irányítópult (eszköz)|
||Eszközök →|Monitor|
||Biztonsági másolat katalógus|Megosztás (fájlkiszolgálóra) vagy </br>Kötet (iSCSI server)|
||Állítsa be (szolgáltatás)|(Eszköz) beállítása|
||Feladatok|Karbantartási|
||Értesítések|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Használja a StorSimple kezelő szolgáltatás felügyeleti feladatok

A következő táblázat mutatja az általános felügyeleti feladatok és a bonyolult munkafolyamatok, amelyek a felhasználói felület-StorSimple kezelő szolgáltatás belül lehet elvégezni. Az alábbi műveleteket a felhasználói felület lapok, amelyen kezdeményezett alapján vannak rendezve.

Az egyes munkafolyamat kapcsolatos további tudnivalókért kattintson a megfelelő lépéseket a táblázatban.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager munkafolyamatok

|Ha azt szeretné, hogy ehhez...|Nyissa meg a felhasználói felület lap...|Ezzel az eljárással|
|---|---|---|
|A szolgáltatás hozzon létre</br>A szolgáltatás törlése</br>A szolgáltatás regisztrációs kulcs beszerzése</br>A szolgáltatás regisztrációs kulcs újragenerálása|StorSimple kezelő szolgáltatás|[A StorSimple kezelő szolgáltatás telepítése](storsimple-ova-manage-service.md)|
|A szolgáltatás adatok titkosítási kulcs módosítása</br>A műveletek naplók megtekintése|Kezelő szolgáltatás → StorSimple irányítópult|[A szolgáltatás StorSimple irányítópult használata](storsimple-ova-service-dashboard.md)|
|A virtuális tömb inaktiválása</br>A virtuális tömb törlése|StorSimple kezelő szolgáltatás → eszközök|[Inaktiválja, vagy egy virtuális tömböt törlése](storsimple-ova-deactivate-and-delete-device.md)|
|Katasztrófa helyreállítási és eszköz feladatátvevő</br>Feladatátvevő vonatkozó követelmények</br>A virtuális eszköz áttérni</br>Üzleti folytonosságot vészhelyreállítás (BCDR)</br>Hibák vészhelyreállítás során|StorSimple kezelő szolgáltatás → eszközök|[A StorSimple virtuális tömb katasztrófa helyreállítási és eszköz feladatátvétele](storsimple-ova-failover-dr.md)|
|Megosztás és a kötet biztonsági mentése</br>A kézi biztonsági másolat készítése</br>A biztonsági mentés ütemezésének módosítása</br>Meglévő biztonsági másolatok megtekintése|StorSimple kezelő szolgáltatás → biztonsági másolat katalógus|[Készítsen biztonsági másolatot a StorSimple virtuális tömb](storsimple-ova-backup.md)|
|Megosztás visszaállítása egy biztonsági beállítása</br>Kötet visszaállítása egy biztonsági beállítása</br>Elemszintű helyreállítási (csak a fájl kiszolgálói)|Kezelő szolgáltatás → StorSimple biztonsági másolat katalógus|[Visszaállítása biztonsági másolatból a StorSimple virtuális tömb](storsimple-ova-restore.md)|
|Tárterület fiókokról</br>Tárterület-fiók felvétele</br>Tárterület fiók szerkesztése</br>Tárterület fiók törlése|StorSimple kezelő szolgáltatás → konfigurálása|[A StorSimple virtuális tömb tárterület-fiókok kezelése](storsimple-ova-manage-storage-accounts.md)|
|Tudnivalók az access vezérlő rekordokról</br>Hozzáadása vagy módosítása az access vezérlő rekord </br>Az access vezérlő rekord törlése|StorSimple kezelő szolgáltatás → konfigurálása|[Access-vezérlő rekordok kezelése a StorSimple virtuális tömb](storsimple-ova-manage-acrs.md)|
|Feladat részletei|StorSimple kezelő szolgáltatás → feladatok| [Virtuális tömb StorSimple feladatok kezelése](storsimple-ova-manage-jobs.md)|
|Értesítési beállítások konfigurálása</br>Értesítések fogadása</br>Értesítések kezelése</br>Értesítések áttekintése|StorSimple kezelő szolgáltatás → riasztásai|[Megtekintése és értesítések kezelése a StorSimple virtuális tömb](storsimple-ova-manage-alerts.md)|
|Az eszköz rendszergazdai jelszó módosítása|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[A virtuális tömb StorSimple eszközt rendszergazdai jelszavának módosítása](storsimple-ova-change-device-admin-password.md)|
|Szoftverfrissítések telepítése|StorSimple kezelő szolgáltatás → eszközök → karbantartása|[A virtuális tömb frissítése](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] A [helyi webes felület](storsimple-ova-web-ui-admin.md) esetén az alábbi műveleteket kell használnia:
>
>- [A szolgáltatás adatok titkosítási kulcs beolvasása](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Támogatás csomag létrehozása](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Indítsa újra a virtuális tömb](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Következő lépések
A felhasználói felület webes információt és használatához lépjen [a StorSimple webes felügyelheti a StorSimple virtuális tömb felhasználói felület használata](storsimple-ova-web-ui-admin.md).
