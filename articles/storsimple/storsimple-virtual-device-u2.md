<properties 
   pageTitle="StorSimple virtuális eszköz frissítés 2 |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre, telepítése, és kezelheti a Microsoft Azure virtuális hálózatban StorSimple virtuális eszköz. (2 StorSimple frissítés vonatkozik)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Üzembe helyezéséhez és kezeléséhez az Azure virtuális StorSimple eszköz


##<a name="overview"></a>– Áttekintés
StorSimple 8000 sorozat virtuális eszköz olyan további lehetőséggel a Microsoft Azure StorSimple megoldás épített. A StorSimple virtuális eszközt a Microsoft Azure virtuális hálózatban virtuális gépen fut, és felhasználhatja azt biztonsági mentése és adatok másolása az állomástól. Ebből az oktatóanyagból megtudhatja, hogy miként üzembe helyezéséhez és kezeléséhez az Azure virtuális eszköz, és alkalmazandó szoftver frissítés 2-es verziójú futó virtuális eszközök és az alsó.


#### <a name="virtual-device-model-comparison"></a>Virtuális eszköz adatmodellek összehasonlítása

A virtuális StorSimple eszköz két modellek, a szabványos 8010 (korábbi nevén az 1 100) és (frissítés 2 bevezetett) 8020 támogatás érhető el. A két modellek összehasonlítása alatt található táblázatos.


| Eszköz modell          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **A maximális kapacitás**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure virtuális**              | Standard_A3 (4 magmintákat, 7 GB memóriát)                                                                      | Standard_DS3 (4 magmintákat, 14 GB memóriát)                                                                                                                          |
| **Verziók közötti kompatibilitás** | Verziójú előre frissítése 2 vagy újabb verzió                                             | Frissítés futtatása a 2-es vagy újabb verziók                                                                                                  |
| **Régió elérhetősége**   | Az összes Azure területek                                                         | Azure régiók, amelyek támogatják a prémium tárhely<br></br>Régiók listáját lásd: a [támogatott régiók 8020](#supported-regions-for-8020) |
| **Tárolási típusa**          | Azure szabványos tárolót használ helyi lemez<br></br> Megtudhatja, hogyan hozhat [létre egy szokásos tárterület-fiókkal]() | Azure prémium tárterületet használ a helyi lemezre<sup>2</sup> <br></br>Megtudhatja, hogyan hozhat [létre egy prémium tárterület-fiókkal](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Terhelést útmutató**     | Elem szintű lekérés a biztonsági mentés fájlok                                              | A felhő fejlesztők és forgatókönyvek, kis késés, nagyobb teljesítmény munkaterhelésekből tesztelése <br></br>Másodlagos eszköz vészhelyreállítás                                                                                            |
 
<sup>1</sup> *az 1 100 korábbi neve*.

<sup>2</sup> a 8010 és a 8020 *Azure szabványos tárhely a felhőben réteg használata. A helyi réteg belül az eszközt csak létezik a különbség a*.

#### <a name="supported-regions-for-8020"></a>A támogatott régiók 8020

A jelenleg támogatott 8020 prémium tároló régiók alatt megjelennének. Ez a lista folyamatosan frissíthető, mert további régióban prémium tároló elérhetővé válik. 

| S nem.                                                  | Jelenleg támogatott régiók |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | A központi Amerikai Egyesült Államok                     |
| 2                                                       |  Kelet-Amerikai Egyesült Államok                       |
| 3                                                       |  Kelet-amerikai 2                     |
| 4                                                       | Nyugati Amerikai Egyesült Államok                        |
| 5                                                       | Észak-Európa                   |
| 6                                                       | Nyugati Európa                    |
| 7                                                       | Délkelet-ázsiai                 |
| 8                                                       | Japán keleti                     |
| 9                                                       | Japán nyugati                     |
| 10                                                      | Ausztrália keleti                 |
| 11                                                      | Ausztrália Délkelet *           |
| 12                                                      | Kelet-ázsiai *                     |
| 13-mal                                                      | Dél-központi USA -beli *              |

* Az alábbi geos nemrégiben elindult prémium tároló.

Ez a cikk ismerteti a lépésenkénti folyamata az Azure virtuális StorSimple eszköz telepítése. Ez a cikk elolvasása, után lesz:

- Ismerje meg, hogy miben különbözik a virtuális eszköz a fizikai eszközről.

- Tudja létrehozása és konfigurálása a virtuális eszközt.

- Csatlakozás a virtuális eszközt.

- Megtudhatja, hogy miként működik a virtuális eszközzel.

Ebben az oktatóanyagban összes StorSimple virtuális eszköz futtatása a frissítés, 2 vagy újabb vonatkozik. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Hogyan különböznek egymástól a virtuális eszköz a fizikai eszközök

StorSimple virtuális eszköz a Microsoft Azure virtuális gép egyetlen csomóponton futó StorSimple csak szoftver változatát. A virtuális eszköz katasztrófa visszaállítási helyzetek, amelyben a fizikai eszközök nem érhető el, és használata az elemszintű lekérés biztonsági másolatok, vészhelyreállítás a helyszíni és felhőbeli fejlesztők és a próba esetek a megfelelő támogatja.

#### <a name="differences-from-the-physical-device"></a>A fizikai eszközök közötti különbségeket

A következő táblázat mutatja a StorSimple virtuális eszköz és a StorSimple fizikai eszközök közötti néhány alapvető különbség.

|                             | Fizikai eszközök                                          | Virtuális eszköz                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Hely**                    | A adatközponthoz található.                               | Futtatja az Azure-ban.                                                                            |
| **Hálózati kapcsolatok**          | Hat hálózati kapcsolatok van: adatok 0-5 adatokat.                  | Még csak egy hálózati kapcsolat: adatok 0.                                        |
| **Regisztráció**                | A Registered konfigurációs lépés során.                | Regisztráció az egy külön feladatot.                                                          |
| **Szolgáltatás adatok titkosítási kulcs** | Újragenerálása fizikai az eszközre, és kattintson az új kulcs frissítse a virtuális eszközt.           | Nem lehet újra létrehozni a virtuális eszközről. |


## <a name="prerequisites-for-the-virtual-device"></a>A virtuális eszköz előfeltételei

Az alábbi szakaszok ismertetik a konfigurációs Előfeltételek StorSimple virtuális mobileszközére. Előtt virtuális eszköz telepítésével, tekintse át a [biztonsági kérdései virtuális eszközt használ](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure vonatkozó követelmények

Mielőtt kiépítése a virtuális eszközt, akkor kell az alábbi Előkészületek az Azure környezetben:

- A virtuális eszköz, [állítsa be az Azure virtuális hálózaton](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Ha prémium tárterületet használ, a virtuális hálózat, amely támogatja a prémium tároló Azure területen kell létrehoznia. További információ a [8020, amely jelenleg támogatott régiók](#supported-regions-for-8020).
- Célszerű az alapértelmezett DNS-kiszolgáló helyett a saját DNS-kiszolgáló neve megadása Azure által biztosított használatára. Ha a DNS-kiszolgáló neve nem érvényes, vagy ha nem tudja oldani a IP-címek megfelelően a DNS-kiszolgáló, a virtuális eszköz létrehozása sikertelen lesz.
- Webhely pont- és a webhelyek – olyan nem kötelező, de nem szükséges. Ha kívánja, beállíthatja a speciális esetek ezeket a beállításokat. 
- A virtuális hálózaton, amelyekkel a virtuális eszköz által elérhetővé tett kötet [Azure virtuális gépeken futó](../virtual-machines/virtual-machines-linux-about.md) (kiszolgáló állomás) hozhat létre. Ezeket a kiszolgálókat az alábbi feltételeknek kell megfelelnie:                            
    - Windows vagy Linux VMs iSCSI kezdeményező szoftver telepítve lenniük.
    - A virtuális eszközként virtuális ugyanabba a hálózatba futnia.
    - Tudja a virtuális eszközt a belső IP-címét a virtuális eszköz iSCSI céljának csatlakozni.

- Ellenőrizze, hogy iSCSI és a felhő forgalom támogatása beállította a virtuális hálózaton.


#### <a name="storsimple-requirements"></a>StorSimple vonatkozó követelmények

A virtuális eszköz létrehozása előtt végezze el az alábbi frissítéseket a Azure StorSimple szolgáltatásban:


- [Access vezérlőelem-rekordok](storsimple-manage-acrs.md) hozzáadása a VMs, amely a virtuális eszköz kiszolgálók fognak.

- [Tárterület-fiók](storsimple-manage-storage-accounts.md#add-a-storage-account) használata ugyanabban a virtuális eszközként régióban. Tárterület-fiókok különböző régiókban vonhat gyenge teljesítményt. A virtuális eszközzel egy normál vagy prémium tároló fiókot is használhatja. További információt a [szabványos tárterület-fiók]() vagy [prémium tárterület-fiók](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) létrehozása

- Használjon másik tárhely a virtuális eszköz létrehozása szeretne használni az adatokhoz. Gyenge teljesítményt vonhat tároló ugyanazzal a fiókkal.

Győződjön meg arról, hogy az alábbi információk első lépések:

- Azure klasszikus portál fiókjába access hitelesítő adatokkal.

- A szolgáltatás adatok titkosítási kulcs fizikai eszközéről másolata.


## <a name="create-and-configure-the-virtual-device"></a>Létrehozása és konfigurálása a virtuális eszköz

Végrehajtása előtt győződjön meg arról, hogy teljesülnek-e a [virtuális eszköz Előfeltételek](#prerequisites-for-the-virtual-device)ezeket a műveleteket. 

Miután létrehozott egy virtuális hálózatot, StorSimple kezelő szolgáltatás beállítva, és a fizikai StorSimple eszköz regisztrált a szolgáltatással, használatával az alábbi lépéseket létrehozása és konfigurálása a StorSimple virtuális eszköz. 

### <a name="step-1-create-a-virtual-device"></a>Lépés: 1: Hozzon létre egy virtuális eszköz

A következő lépésekkel hozhat létre a StorSimple virtuális eszközt.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Ha a virtuális eszköz létrehozása sikertelen ebben a lépésben, akkor valószínűleg nincs kapcsolat az internethez. További információért lépjen [Internet kapcsolódási hibák elhárítása](#troubleshoot-internet-connectivity-errors) amikor creatig egy virtuális eszközt.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Lépés: 2: Konfigurálása és regisztrálhatja a virtuális eszköz

Az eljárás megkezdése előtt győződjön meg arról, hogy az adatok titkosítókulcs másolatának. A szolgáltatás adatok titkosítókulcs hozta létre úgy állította be, hogy az első StorSimple eszköz, és mentse a biztonságos helyen utasították. Ha nem rendelkezik a szolgáltatás adatok titkosítási kulcs egy példányát, kapcsolatba kell lépnie a Microsoft Support segítségét.

A következő lépésekkel beállítása és a StorSimple virtuális eszköz regisztrálni.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Lépés 3: (Nem kötelező) módosítása a konfigurációs beállításai

Az alábbi szakasz ismerteti a konfigurációs beállításai, szükséges a StorSimple virtuális eszköz, ha a használni kívánt CHAP, StorSimple pillanatkép Manager vagy az eszköz rendszergazdáját jelszó módosítása.

#### <a name="configure-the-chap-initiator"></a>A CHAP kezdeményező konfigurálása

A paraméter tartalmaz, amely a virtuális eszköz (cél) vár a próbálnak hozzáférni a kötet kezdeményezők (-kiszolgálók) a hitelesítő adatait. A kezdeményezők CHAP felhasználónév és a hitelesítés során az eszközre azonosítása maguk CHAP jelszó biztosít. A lépések részletes leírását lépjen [Az eszköz beállítása CHAP](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>A CHAP cél konfigurálása

A paraméter tartalmaz, amely a virtuális eszköz használja, amikor egy CHAP engedélyező kezdeményező kér kölcsönös- vagy kétirányú hitelesítés hitelesítő adatait. A virtuális eszköz segítségével fordított CHAP felhasználónevet és jelszót fordított CHAP azonosítja magát a kezdeményező hitelesítési folyamat során. Ne feledje, hogy CHAP cél beállítások globális beállításokat. Ezek a fájlok érvényesek, amikor a csatlakoztatott virtuális tárolóeszközre összes kötet CHAP hitelesítés fogja használni. A lépések részletes leírását lépjen [Az eszköz beállítása CHAP](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>A megadott StorSimple pillanatkép Manager jelszó beállítása

StorSimple pillanatkép Managert a Windows-szolgáltató a találhatók, és a rendszergazdák kezelése helyi formájában StorSimple eszköze biztonsági másolatait, és a felhő pillanatképek.

>[AZURE.NOTE] A virtuális eszköz a Windows-szolgáltató esetében az Azure virtuális gépen.

A tartalomkeresési egy eszközt a StorSimple pillanatkép Manager, a rendszer kéri, a StorSimple eszköz IP-cím és a jelszót a tárolóeszközre hitelesítést végezni. A lépések részletes leírását lépjen [konfigurálása StorSimple pillanatkép Manager jelszót](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Az eszköz rendszergazdai jelszó módosítása 

A Windows PowerShell-felület használatakor a virtuális eszköz eléréséhez szükséges eszközt rendszergazdai jelszó megadását. Az adatok biztonsága érdekében a jelszó módosítása, a virtuális eszköz használata előtt szükség. A lépések részletes leírását lépjen [konfigurálása eszközt rendszergazdai jelszót](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Csatlakozás távolról a virtuális eszköz
A Windows PowerShell felületén keresztül virtuális eszközére távelérési alapértelmezés szerint nincs engedélyezve. Virtuális eszközre Távfelügyelet először engedélyezése, majd kattintson az ügyfél, a virtuális eszköz eléréséhez használt engedélyezze szüksége.

A két lépésből álló folyamat távoli részletes alatt.

### <a name="step-1-configure-remote-management"></a>Lépés: 1: Távfelügyelet beállítása

A következő lépésekkel állítsa be a Távfelügyelet StorSimple virtuális mobileszközére.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Lépés: 2: Távolról is elérheti a virtuális eszköz

Miután engedélyezte a StorSimple eszköz beállítása lapon távfelügyelet, használhatja a Windows PowerShell távelérési a virtuális eszköz csatlakoztatása egy másik virtuális számítógépéről kezdése ugyanazon a virtuális hálózaton; Ha például az állomástól konfigurált és iSCSI kapcsolódásra használt virtuális lehet csatlakozni. A legtöbb környezetekben lesz már megnyitott egy nyilvános végpontot, a szolgáltató elérni a virtuális eszköz használható virtuális eléréséhez.

>[AZURE.WARNING] **A nagyobb biztonság érdekében kifejezetten ajánljuk, hogy HTTPS használni, a végpontok való csatlakozáskor, és törölje a végpontok, miután befejezte a távoli PowerShell-munkamenetet.**

Célszerű követnie az [StorSimple eszköze távolról kapcsolódás](storsimple-remote-connect.md) virtuális mobileszközére távelérési beállítása lépéseit.

## <a name="connect-directly-to-the-virtual-device"></a>Csatlakoztassa közvetlenül a virtuális eszköz

Is csatlakozhat közvetlenül a virtuális eszközt. Ha egy másik számítógépről vagy a Microsoft Azure környezetben a virtuális hálózaton kívüli közvetlenül a virtuális eszközt a csatlakoztatni kívánt további végpontok létrehozásához, ahogy az alábbi eljárás szükséges. 

A következő lépésekkel hozzon létre egy nyilvános végpontot virtuális eszközre.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Azt javasoljuk, hogy csatlakozni virtuális hálózaton belül egy másik virtuális számítógépéről, mert ez az eljárás kis méretűre állítása a virtuális hálózaton nyilvános végpontok számát. Ezt a módszert, ha egyszerűen csatlakoztatása a virtuális gép távoli asztali kapcsolat keresztül, és más Windows ügyfél helyi hálózaton módon beállította, hogy virtuális gép való használatra. Nem kell a nyilvános portszámot hozzáfűző, mivel a port már adattartományon.

## <a name="work-with-the-storsimple-virtual-device"></a>A virtuális StorSimple-eszköz használata

Most, hogy létrehozott és beállított a StorSimple virtuális eszköz, készen elkezdhet dolgozni rajta. Dolgozhat mennyiségi tárolók kötet és biztonsági házirendek virtuális eszközön ugyanúgy, mint a fizikai StorSimple eszközön az egyetlen különbség, hogy szeretne-e győződjön meg arról, hogy az eszköz listából válassza ki a virtuális eszközt. Keresse meg [a StorSimple kezelő szolgáltatás kezelése a virtuális eszköz használata](storsimple-manager-service-administration.md) a virtuális eszközökre készült különböző felügyeleti feladatok lépésenkénti leírásaira kíváncsi.

Az alábbi szakaszok ismertetik néhány fog megjelenni, ha a virtuális eszköz használata különbséget.

### <a name="maintain-a-storsimple-virtual-device"></a>Naplók karbantartása: a virtuális StorSimple eszköz

Csak szoftver eszköz kerül, a virtuális eszköz karbantartási található minimális a fizikai eszközök karbantartása képest. Az alábbi lehetőség közül választhat:

- **Szoftverfrissítések** – lehet az, hogy a szoftver legutolsó frissítés dátumát, bármelyik együtt állapotüzenetek frissítése. A lap alján az **ellenőrzés frissítések** gomb segítségével egy manuális ellenőrzést hajt végre, ha szeretne új frissítések keresése. Ha frissítések áll rendelkezésre, kattintson a telepítendő **Frissítések telepítése** gombra. Mivel a virtuális eszközön csak egyetlen felülettel, ez azt jelenti, hogy fennakadás egy kis szolgáltatás frissítéseket alkalmazásakor. A virtuális eszköz leáll, és indítsa újra a (ha szükséges), amely rendelkezik jelent frissítéseinek. Lépésenkénti útmutatót lépjen a [frissítse az eszközt](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- A **csomag támogatja** – hoz létre, és töltse fel a támogatási csomagot a Microsoft Support a virtuális eszközzel kapcsolatos problémák megoldása érdekében. Lépésenkénti lépjen [létrehozása és kezelése egy támogatás csomagot](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Tárterület-fiókok egy virtuális eszközhöz

Tárterület-fiókok használata StorSimple kezelő szolgáltatás, a virtuális eszköz és a fizikai eszközök segítségével jön létre. A tárhely fiókok létrehozásakor azt javasoljuk, hogy használja az egy régió azonosítója a felhasználóbarát név érdekében győződjön meg arról, hogy a régió minden összetevő rendszer következetesek. A virtuális eszköz fontos, hogy minden összetevő teljesítményét érintő hibák elkerülése ugyanazon régióban legyen.

Lépésenkénti útmutatót lépjen a [tárterület-fiók felvétele](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Virtuális StorSimple eszköz inaktiválása

Virtuális eszköz inaktiválásával törli a virtuális és az erőforrások jön létre, ha azt kiépített. Miután a virtuális eszköz nyitásához azt nem lehet visszaállítani az eredeti állapotába. Csak a virtuális eszköz inaktiválta, hogy leállítása vagy törlése az ügyfelek és a hosts, amely függ.

A virtuális eszköz inaktiválásával eredménye a következő műveleteket:

- A virtuális eszköz el van távolítva.

- Az operációs rendszer lemez és adatok lemezt a virtuális eszköz által létrehozott törlődnek.

- A szolgáltatott szolgáltatás és a kiépítési során létre virtuális hálózati megőrződnek. Ha nem használ őket, törölni kell őket kézzel.

- A virtuális eszköz által létrehozott felhőalapú pillanatképek megőrződnek.

Lépésenkénti, kattintson a [Inaktiválás, és törölje a StorSimple eszköz](storsimple-deactivate-and-delete-device.md).

Amint a virtuális eszközt, inaktívvá StorSimple kezelő szolgáltatás lapon látható, törölheti a virtuális eszköz eszköz listából az **eszközök** lapon.


### <a name="start-stop-and-restart-a-virtual-device"></a>Kezdés, le, és indítsa újra a virtuális eszköz
Fizikai eszközök StorSimple eltérően nem nincs power be- és kikapcsolása gombra kattintva StorSimple virtuális eszközön leküldéses hatvány. Azonban lehetnek alkalommal hol kell indítsa újra a virtuális eszközt. Ha például egyes frissítések előfordulhat, hogy a virtuális újra kell indítani a frissítési folyamat befejezéséhez. Kezdje meg legkönnyebben, leállítása, és indítsa újra a virtuális eszköz, a virtuális gépeken futó kezelése konzolról.

Ha megnézi az Management Console, a virtuális Eszközállapot oka az **operációs rendszert futtató** bekapcsolják alapértelmezés szerint létrehozása után. Indítsa el, le, és indítsa újra a virtuális gép bármikor.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Gyári visszaállítása

Ha úgy dönt, hogy, csak ha elölről szeretné kezdeni a virtuális eszközzel, akkor egyszerűen inaktiválhatja és törölheti azt, és ezután hozzon létre egy újat. Amikor a fizikai eszközök új, mint a új virtuális eszköz nem tudja a frissítéseiről alkalmazást. Ezért ellenőrizze, hogy a frissítések keresése hivatkozásra, mielőtt használni kezdi.


## <a name="fail-over-to-the-virtual-device"></a>A virtuális eszköz átadni

Vészhelyreállítás (DR), amely a virtuális StorSimple eszköz készült esetek egyike. Ebben az esetben a fizikai StorSimple eszköz vagy a teljes adatközponthoz előfordulhat, hogy nem érhető el. Virtuális eszköz segítségével Szerencsére visszaállításhoz egy másik helyre. Során DR a mennyiségi tárolók az adatforrás eszközről tulajdonosának módosítása, és a virtuális eszköz átkerülnek. DR előfeltételei:, hogy a virtuális eszköz létrehozott és beállított, a mennyiségi tárolóban lévő összes kötet nyílt offline és a hangerő tároló tartozik egy felhőalapú pillanatkép.

>[AZURE.NOTE] 
>
> - Ha használja a virtuális eszköz a másodlagos eszközként DR, ne feledje, hogy a 8010 30 TB-nyi szabványos tárolási és 8020 rendelkezik 64 TB-nyi prémium tároló. Lehet, hogy újabb kapacitás 8020 virtuális eszköz további DR példa a legmegfelelőbb.
> - Nem feladatátvevő vagy adatfeliratsor frissítés 2 rendszert futtató egy eszközt a frissítés előtti 1 szoftver fut, a eszközről. Is azonban nem sikerül egy eszköz futtatása a frissítés 1 (1.1-es vagy 1.2-es) frissítés 2 rendszert futtató eszköz fölé

Lépésenkénti útmutatót lépjen [a virtuális eszköz áttérni](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Állítsa le, vagy törölje a virtuális eszköz

Ha korábban beállította, és egy virtuális eszköz most de meg szeretné szüntetni merülnek fel felhasználásának számítási díjai StorSimple használt, után leállíthatja a virtuális eszközt. A virtuális eszköz leállítása az operációs rendszer vagy tárolóban lévő adatok lemez nem törli. Azt leállítása díjak a előfizetés merülnek fel, de továbbra is az operációs rendszer és az adatok lemez tároló díjai.

Ha törli, vagy állítsa le a virtuális eszközt, meg kíván jeleníteni **Offline** , az eszközök lapon a StorSimple kezelő szolgáltatás. Megadhatja, inaktiválása vagy törlése az eszközön, ha is szeretné törölni a virtuális eszköz által létrehozott biztonsági másolatok. További tudnivalókért lásd: az [Inaktiválás és törlés StorSimple eszköz](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Internetes kapcsolódási hibák elhárítása 

A virtuális eszköz kibocsátása során, ha nincs kapcsolat az internethez, a létrehozási lépés sikertelen lesz. Kapcsolatos problémák megoldásához, ha a hiba miatt internetkapcsolat, az Azure klasszikus portálon végezze el az alábbi lépéseket:

1. A Windows server 2012 virtuális gép létrehozása az Azure-ban. A virtuális számítógéphez a ugyanazzal a tárterület-fiókkal, VNet és a virtuális eszköz által használt alhálózat kell használni. Ha már van egy meglévő Windows kiszolgáló állomás azonos tárterület-fiókjához, Vnet és alhálózat Azure-ban, is vele az internetkapcsolat hibáinak elhárítása.
2. Távoli jelentkezzen be az előző lépésben létrehozott virtuális gépen. 
3. A virtuális gép belül parancs ablak megnyitása (Win + r billentyűkombinációt, és írja be `cmd`).
4. A következő parancs futtatásával a parancssorba.

    `nslookup windows.net`

5. Ha `nslookup` nem sikerül, majd internetes kapcsolat hiba megakadályozza, hogy a virtuális eszköz regisztrálása a StorSimple kezelő szolgáltatás. 
6. Végezze el a szükséges módosításokat annak érdekében, hogy a virtuális eszköz Azure webhelyeken, például "windows.net" elérheti a virtuális hálózathoz.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [a StorSimple kezelő szolgáltatás kezelése a virtuális eszköz](storsimple-manager-service-administration.md)használatáról.
 
- Dátumtáblázatok ismertetése visszaállítása [egy biztonsági csoportjából StorSimple mennyiségig](storsimple-restore-from-backup-set.md). 

