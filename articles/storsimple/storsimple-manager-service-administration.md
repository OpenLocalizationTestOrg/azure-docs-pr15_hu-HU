<properties 
   pageTitle="StorSimple kezelő szolgáltatás felügyeleti |} Microsoft Azure"
   description="Megtudhatja, hogy miként használatával való kezeléséről az StorSimple eszköz a StorSimple kezelő szolgáltatás az Azure klasszikus portálon."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>A StorSimple kezelő szolgáltatás használatával felügyelheti a StorSimple eszköz

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti a StorSimple-kezelő szolgáltatás felület, például hogy miként csatlakozhat azt a rendelkezésre álló különböző beállításokat és a felhasználói felület keresztül elvégezhető adott munkafolyamatok mutató. Az útmutató olyan alkalmazható mindkettő; a StorSimple fizikai és a virtuális eszközt.

Után a cikket elolvasva megtanulhatja, hogy:

- Csatlakozás StorSimple kezelő szolgáltatás
- Nyissa meg a StorSimple Manager felhasználói felület
- Service StorSimple Manager StorSimple eszköze felügyelete


## <a name="connect-to-storsimple-manager-service"></a>Csatlakozás StorSimple kezelő szolgáltatás

A StorSimple kezelő szolgáltatás fut, a Microsoft Azure-ban, és több StorSimple eszköz csatlakozik. Ezekre az eszközökre kezelése a böngészőben futó központi Microsoft Azure klasszikus portál segítségével. Csatlakozás a StorSimple kezelő szolgáltatás, tegye a következőket.

#### <a name="to-connect-to-the-service"></a>A szolgáltatáshoz való kapcsolódáshoz

1. Nyissa meg azt a [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Használja a Microsoft-fiókja hitelesítő adatait, jelentkezzen be a Microsoft Azure klasszikus portálra (az ablak jobbra található).

1. Görgessen lefelé a bal oldali navigációs a StorSimple kezelő szolgáltatás eléréséhez.


## <a name="navigate-storsimple-manager-service-ui"></a>Nyissa meg a StorSimple kezelő szolgáltatás felhasználói felület

A navigációs hierarchiában a StorSimple kezelő szolgáltatás felhasználói felület az alábbi táblázatban látható.

- **StorSimple Manager** céloldal megnyitja a felhasználói felület szolgáltatói lapok alkalmazandó összes eszközön belül szolgáltatás.

- **Eszközök** lap megnyitja a eszközszintű – felhasználói felület lapok alkalmazható egy adott eszközt.

- **Mennyiségi tárolók** lap megnyitja egy eszközt társított összes kötet megjelenítő mennyiségi lapon.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple kezelő szolgáltatás navigációs hierarchia

|Kezdőlapja|Szolgáltatói lapok|Eszköz szintű lapok|Eszközszintű lapokon|
|---|---|---|---|
|StorSimple kezelő szolgáltatás|Szolgáltatás-irányítópult|Eszköz irányítópult||
||Eszközök →|Monitor|
||Biztonsági másolat katalógus|Mennyiségi containers→|Kötet|
||Állítsa be (szolgáltatás)|Biztonsági házirendek||
||Feladatok|(Eszköz) beállítása|
||Értesítések|Karbantartási|

![A videó érhető el](./media/storsimple-manager-service-administration/Video_icon.png) **Videó érhető el**

Nézni egy videót, amely végigvezeti a StorSimple kezelő szolgáltatás felhasználói felület, kattintson [ide](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>StorSimple eszköz használatával StorSimple kezelő szolgáltatás felügyelete

A következő táblázat mutatja az általános felügyeleti feladatok és a bonyolult munkafolyamatok, amelyek a felhasználói felület-StorSimple kezelő szolgáltatás belül lehet elvégezni. Az alábbi műveleteket a felhasználói felület lapok, amelyen kezdeményezett alapján vannak rendezve.

Az egyes munkafolyamat kapcsolatos további tudnivalókért kattintson a megfelelő lépéseket a táblázatban.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager munkafolyamatok

|Ha azt szeretné, hogy ehhez...|Nyissa meg a felhasználói felület lap...|Az alábbi eljárással.|
|---|---|---|
|A szolgáltatás hozzon létre</br>A szolgáltatás törlése</br>Szolgáltatás regisztrációs kulcs beszerzése</br>Szolgáltatás regisztrációs kulcs újragenerálása|StorSimple kezelő szolgáltatás|[Egy StorSimple kezelő szolgáltatás telepítése](storsimple-manage-service.md)
|A szolgáltatás adatok titkosítási kulcs módosítása</br>A művelet naplók megtekintése|Kezelő szolgáltatás → StorSimple irányítópult|[A StorSimple kezelő szolgáltatás irányítópultok használata](storsimple-service-dashboard.md)|
|Egy eszközt inaktiválása</br>Eszköz törlése|StorSimple kezelő szolgáltatás → eszközök|[Eszköz törlése és inaktiválása](storsimple-deactivate-and-delete-device.md)|
|Tudnivalók a katasztrófa helyreállítási és eszköz feladatátvevő</br>A fizikai eszközök áttérni</br>A virtuális eszköz áttérni</br>Üzleti folytonosságot vészhelyreállítás (BCDR)|StorSimple kezelő szolgáltatás → eszközök|[Feladatátvétel és katasztrófa helyreállításának StorSimple eszköze](storsimple-device-failover-disaster-recovery.md)|
|Lista biztonsági másolatok kötet</br>Kattintson a biztonsági másolat beállítása</br>Biztonsági másolat törlése|Kezelő szolgáltatás → StorSimple biztonsági másolat katalógus|[Biztonsági másolatok kezelése](storsimple-manage-backup-catalog.md)|
|A mennyiségi másolása|Kezelő szolgáltatás → StorSimple biztonsági másolat katalógus|[A mennyiségi másolása](storsimple-clone-volume.md)|
|Biztonsági másolat visszaállíthatja|Kezelő szolgáltatás → StorSimple biztonsági másolat katalógus|[Biztonsági másolat visszaállíthatja](storsimple-restore-from-backup-set.md)|
|Tárterület fiókokról</br>Tárterület-fiók felvétele</br>Tárterület fiók szerkesztése</br>Tárterület fiók törlése</br>Kulcs Elforgatás tárterület-fiókok|StorSimple kezelő szolgáltatás → konfigurálása|[Tárterület-fiókok kezelése](storsimple-manage-storage-accounts.md)|
|Sávszélesség-sablonok</br>A sávszélesség-sablon hozzáadása</br>A sávszélesség-sablon szerkesztése</br>A sávszélesség sablon törlése</br>A sávszélesség, alapértelmezett sablon használatához</br>Egy egész napos egy adott időben kezdődik sávszélesség-sablon létrehozása|StorSimple kezelő szolgáltatás → konfigurálása|[Sávszélesség-sablonok kezelése](storsimple-manage-bandwidth-templates.md)|
|Tudnivalók az access vezérlő rekordokról</br>Az access vezérlőelem-rekord létrehozása</br>Az access vezérlő rekord szerkesztése</br>Az access vezérlő rekord törlése|StorSimple kezelő szolgáltatás → konfigurálása|[Access-vezérlő rekordok kezelése](storsimple-manage-acrs.md)|
|Feladat részletei</br>Egy feladat megszakítása|StorSimple kezelő szolgáltatás → feladatok|[Feladatok kezelése](storsimple-manage-jobs.md)
|Értesítések fogadása</br>Értesítések kezelése</br>Értesítések áttekintése|StorSimple kezelő szolgáltatás → riasztásai|[Megtekintése és StorSimple értesítések kezelése](storsimple-manage-alerts.md)
|Csatlakoztatott kezdeményezők megtekintése</br>Keresse meg az eszköz hányadik</br>Keresse meg a cél IQN|StorSimple kezelő szolgáltatás → eszközök → irányítópult|[A StorSimple eszköz irányítópult használata](storsimple-device-dashboard.md)|
|Ellenőrző diagramok létrehozása|StorSimple kezelő szolgáltatás → eszközök → figyelése|[Lync-StorSimple eszköze](storsimple-monitor-device.md)|
|Mennyiségi tároló hozzáadása</br>Mennyiségi tároló módosítása</br>Mennyiségi tároló törlése|Kezelő szolgáltatás → StorSimple eszközök → mennyiségi tárolók|[Mennyiségi tárolók kezelése](storsimple-manage-volume-containers.md)|
|A hangerő hozzáadása</br>A mennyiség módosítása</br>A mennyiségi offline állapotba</br>Kötet törlése</br>A mennyiségi figyelése|Kezelő szolgáltatás → eszközök → mennyiségi tárolók → StorSimple kötet|[Kötet kezelése](storsimple-manage-volumes.md)|
|Eszköz beállításainak módosítása</br>Idő beállításainak módosítása</br>DNS.md beállításainak módosítása</br>Hálózati kapcsolatok konfigurálása|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[Eszköz konfigurációjának StorSimple mobileszközére módosítása](storsimple-modify-device-config.md)|
|Nézet web proxy beállításai|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[Webes proxybeállítások az eszközön](storsimple-configure-web-proxy.md)|
|Eszköz rendszergazdai jelszó módosítása</br>StorSimple pillanatkép Manager jelszó módosítása|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[StorSimple jelszó megváltoztatása](storsimple-change-passwords.md)|
|Állítsa be a távoli kezelése|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[Csatlakozás távoli StorSimple eszköze](storsimple-remote-connect.md)|
|Értesítési beállítások konfigurálása|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[Megtekintése és StorSimple értesítések kezelése](storsimple-manage-alerts.md)|
|Az StorSimple eszköz CHAP konfigurálása|StorSimple kezelő szolgáltatás → eszközök → konfigurálása|[Az StorSimple eszköz CHAP konfigurálása](storsimple-configure-chap.md)|
|Biztonsági másolat házirend hozzáadása</br>Hozzáadásához vagy módosításához az ütemezés</br>Biztonsági másolat házirend törlése</br>A kézi biztonsági másolat készítése</br>Hozzon létre egy egyéni biztonsági házirendet több kötet és az ütemtervekről|StorSimple kezelő szolgáltatás → eszközök → biztonsági házirendek|[Biztonsági házirendek kezelése](storsimple-manage-backup-policies.md)|
|Eszköz vezérlők leállítása</br>Indítsa újra az eszközt vezérlők</br>Eszköz vezérlők leállítása</br>Az eszköz gyári visszaállítása</br>(Csak a helyszíni eszköz fenti is)|StorSimple kezelő szolgáltatás → eszközök → karbantartása|[StorSimple eszköz vezérlő kezelése](storsimple-manage-device-controller.md)|
|Tudnivalók a StorSimple hardver-összetevő</br>Monitor hardver állapota</br>(Csak a helyszíni eszköz fenti is)|StorSimple kezelő szolgáltatás → eszközök → karbantartása|[Monitor hardver-összetevő](storsimple-monitor-hardware-status.md)|
|Támogatás csomag létrehozása|StorSimple kezelő szolgáltatás → eszközök → karbantartása|[Létrehozhatja és kezelheti a támogatási csomag](storsimple-create-manage-support-package.md)|
|Szoftverfrissítések telepítése|StorSimple kezelő szolgáltatás → eszközök → karbantartása|[Az eszköz frissítése](storsimple-update-device.md)|


##<a name="next-steps"></a>Következő lépések
Ha minden olyan problémák StorSimple eszköze napi működését, illetve a hardver-összetevő közül, olvassa el:

- [Egy műveleti eszköz hibáinak elhárítása](storsimple-troubleshoot-operational-device.md)
- [Figyelés jelző LED StorSimple használata](storsimple-monitoring-indicators.md)

Ha nem tudja megoldani a problémákat, és létre kell hoznia egy szolgáltatási kérelmet, keresse meg a [Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md).
