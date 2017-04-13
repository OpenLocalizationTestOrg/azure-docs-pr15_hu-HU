<properties 
   pageTitle="Módosítsa az StorSimple eszköz konfigurációjának |} Microsoft Azure" 
   description="Már telepítette StorSimple eszközt dolgozóját, hogy a a StorSimple kezelő szolgáltatás használatát ismerteti." 
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
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>A StorSimple kezelő szolgáltatás használata a StorSimple eszköz konfigurációjának módosítása

## <a name="overview"></a>– Áttekintés 

Az Azure klasszikus portál **beállítása** lapján módosíthatja egy StorSimple kezelő szolgáltatás által kezelt StorSimple eszközön eszköz paramétert tartalmazza. Ebből az oktatóanyagból megtudhatja, hogyan használhatja **a lap** a következő eszköz szintű feladatokat:

- Eszköz beállításainak módosítása 
- Idő beállításainak módosítása 
- DNS-beállítások módosítása 
- Hálózati kapcsolatok módosítása
- Cserélje ki vagy máshoz IP-címei

## <a name="modify-device-settings"></a>Eszköz beállításainak módosítása

A hangeszköz beállításai az eszközön, és az eszköz leírása rövid nevét tartalmazza.

Csatlakozik az StorSimple kezelő szolgáltatás StorSimple eszközt van hozzárendelve egy alapértelmezett nevet. Az alapértelmezett nevet általában az eszköz dátumértékét tükrözi. Például olyan alapértelmezett eszköz név, amely 15 karakter hosszú, 8600-SHX0991003G44HT, például azt jelzi, a következőket:

- **8600** – azt jelzi, hogy az eszköz modell.
- **SHX** – azt jelzi, hogy a gyártási webhelyet.
- **0991003** - azt jelzi, hogy egy adott termék.
- **G44HT**- az utolsó 5 számjegy növeli egyedi dátumértékeket létrehozásához. Ez nem lehet egy egymás után következő sor.

Az Azure klasszikus portal segítségével átnevezése eszköz, és rendelje hozzá a kiválasztott egy egyedi nevet. A felhasználóbarát név minden karaktert is tartalmaz, és legfeljebb 64 karakter hosszúak lehetnek.

Megadhatja az eszköz leírását is. Eszköz leírás általában azonosíthatja a tulajdonos és az eszköz fizikai helyét. A Leírás mező 256-nál kevesebb karakterből kell állnia.
 
## <a name="modify-time-settings"></a>Idő beállításainak módosítása

Az eszköz annak érdekében, hogy a felhőalapú tárolási szolgáltatónál hitelesítő idő szinkronizálnia kell. Az időzóna válassza a legördülő listából, és adjon meg egy vagy két hálózati idő Protocol (alapú) kiszolgálók. Az elsődleges NTP-kiszolgáló szükség, és van megadva, az eszköz beállítása a Windows PowerShell-StorSimple használatakor. Az alapértelmezett Windows Server **time.windows.com** a NTP kiszolgálójával is megadhat. Megtekintheti a elsődleges NTP beállítása az Azure klasszikus portálon keresztül, de a Windows PowerShell-kapcsolat használatával módosítja.

A másodlagos NTP beállítása nem kötelező. A klasszikus portal segítségével másodlagos NTP kiszolgáló konfigurálása. 

A NTP kiszolgáló konfigurálásakor győződjön meg arról, hogy a hálózat lehetővé teszi, hogy a NTP forgalmat a adatközponthoz átadhatja az internethez. Egy nyilvános NTP kiszolgáló megadása esetén győződjön meg arról, hogy a hálózati tűzfalak és egyéb biztonsági eszközök vannak beállítva és a külső hálózat az utazási NTP forgalmának engedélyezésére. Kétirányú NTP forgalom nem engedélyezett, ha egy belső NTP-kiszolgáló (egy Windows tartományvezérlőnek biztosít ezt a funkciót) kell használnia. Ha az eszköz nem tudja szinkronizálni a idő, azt nem lehet használatával kommunikál a felhőalapú tárolási szolgáltató.

Nyilvános alapú kiszolgálók listájának megtekintéséhez nyissa meg a [NTP kiszolgálók webhelyen](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Mi történik, ha az eszközre telepítve van egy eltérő időzóna szerint?

Ha az eszköz a különböző időzónájának telepíti, az eszköz időzóna változik. Tekintve, hogy a biztonsági házirendek az eszköz időzóna használja, a biztonsági házirendek automatikusan módosítja az új időzóna szerint. Felhasználói beavatkozás nélkül szükség.

## <a name="modify-dns-settings"></a>DNS-beállítások módosítása

Az eszköz használatával kommunikál a felhőalapú tárolási szolgáltató alkalommal, amikor a DNS-kiszolgáló használja. Magas elérhetőség akkor be kell állítania az elsődleges és másodlagos DNS-kiszolgálók is a kezdeti eszköz telepítése során. Az elsődleges DNS-kiszolgáló dolgozóját, szüksége lesz a Windows PowerShell-felület használata az StorSimple eszközön.

Ha módosítani szeretné a másodlagos DNS-kiszolgálót, az Azure klasszikus portálon is használhatja.



## <a name="modify-network-interfaces"></a>Hálózati kapcsolatok módosítása

Az eszköz hat eszköz hálózati kapcsolatok, négy, amelyek 1 GbE, és két, amelynek 10 GbE tartalmaz. Ezek a felületek vannak adatok 0-5 adatok felirata. ADATOK 0, az adatok 1, az adatok 4 és a adatok 5 1 GbE,, mivel adatok 2 és 3 adatok 10 GbE hálózati kapcsolatok.

Az egyes használt kapcsolatok **Hálózati kapcsolat beállításainak** konfigurálása Ahhoz, hogy a magas elérhetőségét, javasoljuk, hogy legalább két iSCSI felületek és két felhő engedélyezni felületek az eszközön. Azt javasoljuk, de nem szükséges, hogy még nem használt kapcsolatok tiltható.

Ha beállítja a hálózati kapcsolatokat, meg kell adnia egy virtuális IP (virtuális).

0 adata felhő engedélyezni alapértelmezés szerint. ADATOK 0 konfigurálásakor meg is szükséges két rögzített IP-címek konfigurálása, egy, az egyes vezérlők. A rögzített IP-címek közvetlenül elérhető eszköz vezérlők is használható, és akkor hasznos, ha az eszközre, illetve a vezérlők elhárítását elérésekor frissítések telepítése.

StorSimple 8000 sorozat frissítés 1, a 0 adatok útválasztási mérőszám van beállítva a legkisebb; ezért eszköze problémamentesen működik StorSimple 8000 sorozat frissítés 1, ha a felhőben-forgalmat továbbítja 0 adatai között. Jegyezze fel az adott StorSimple eszközén egynél több felhő engedélyezni hálózati kapcsolat esetén.

>[AZURE.NOTE] A rögzített IP-címek a vezérlő karbantartási a frissítéseket, hogy az eszköz segítségével. A rögzített IP-címei ezért továbbíthatók és csatlakozhat az internethez kell lennie.

Az egyes hálózati kapcsolatok a következő paraméterek jelennek meg:

- A **sebesség** – nem felhasználó konfigurálható paraméter. ADATOK 0, az adatok 1, adatok 4-es és 5 adatok állandóan 1 GbE, mivel adatok 2 és 3 adatok 10 GbE felületek.

     >[AZURE.NOTE] Sebességétől és a kétoldalas nyomtatás mindig automatikus-egyeztetése. Jumbo keretek nem támogatottak.
 
- A **kapcsolat állapotának** – felületet engedélyezhető vagy tiltható le. Ha engedélyezve van, az eszköz próbálja használni a felület. Azt javasoljuk, hogy csak a csatlakozik a hálózathoz és használt kapcsolatok engedélyezni. Tiltsa le a bármely felületek, amely nem használ.

- **A kapcsolat típusú** – Ez a paraméter lehetővé teszi a iSCSI-forgalmat a felhő tárterület forgalom elkülönítése. Ez a paraméter lehet az alábbiak egyikét:

    - **A felhő engedélyezni** – Ha engedélyezve van, az eszköz fogja használni a kapcsolat kommunikálni a felhőben.
    - **engedélyezett iSCSI** – Ha engedélyezve van, az eszköz fogja használni a kapcsolat a iSCSI host kommunikálni.

    Azt javasoljuk, hogy a iSCSI-forgalmat a felhő tárterület forgalom elkülönítése. Tartsa szem előtt, ha a szolgáltató belül a eszközként ugyanahhoz az alhálózathoz, nem kell; átjáró hozzárendelése jó helyen jár Ha egy másik alhálózat, mint az eszköz a szolgáltató, szüksége lesz átjáró hozzárendelése.

- **IP-cím** – Ez lehet IPv4 vagy IPv6-ot vagy mindkettőt. A IPv4 és az IPv6-cím családok az eszköz hálózati kapcsolatok támogatott. IPv4 használatakor, pont-decimális, adja meg a 32 bites IP-cím (*xxx.xxx.xxx.xxx*). Ha IPv6-ot használ, egyszerűen ellátás előtaggal 4 jegyű, és egy 128 bites cím automatikusan létrejönnek, hogy előtag alapján eszköz hálózati felületén.

- **Alhálózat** – Ez az alhálózathoz maszk hivatkozik, és van konfigurálva a Windows PowerShell felületén keresztül.

- **Átjáró** : Ez az alapértelmezett átjáró a kapcsolat által használt kell, amikor megkísérli kommunikálni, amely kívül esik az azonos IP-címterület (alhálózat) csomópontot. Az alapértelmezett átjáró kell lennie az azonos címterület (alhálózat), a felület IP-címet, a alhálózat maszk által meghatározott.

- **Rögzített IP-cím** – Ez a mező akkor érhető el, csak a beállíthatja, hogy az adatok 0 közben felület. A műveletek, például a frissítés vagy az eszköz hibaelhárítási előfordulhat, közvetlenül csatlakozhat az eszköz vezérlőhöz. A rögzített IP-címet az aktív, mind az eszközén passzív vezérlő eléréséhez használható.

Vezérlő 0 és 1 vezérlő is újrakonfigurálni az Azure klasszikus portálon keresztül.

>[AZURE.NOTE] 
>
>- Ahhoz, hogy a megfelelő művelet, ellenőrizze a kapcsolat sebessége és a kapcsoló, amely az egyes eszköz kapcsolatok csatlakozik a kétoldalas nyomtatás. Csomagváltás csatolófelületet vagy egyeztetni kell vagy Gigabit Ethernet konfigurálhatók (1000 MB), és kétirányú. Operációs lassabban sebesség vagy a váltakozó felületek eredményezi teljesítménnyel kapcsolatos problémákat.
>
>- Megszakadása és legrövidebb leállás minimalizálásához azt javasoljuk, hogy engedélyezze az egyes az eszköze iSCSI hálózati felhasználói felületén a csatlakozás a kapcsoló portokat portfast. Ezzel biztosíthatja, hogy a hálózati kapcsolat gyorsan hozható létre feladatátvételnél.
 
## <a name="swap-or-reassign-ips"></a>Cserélje ki vagy máshoz IP-címei

Jelenleg Ha bármelyik hálózati kapcsolat a vezérlőn rendel egy virtuális használatban lévő (ugyanarra az eszközre, vagy egy másik eszközre, a hálózaton), majd a vezérlő meghiúsul fölé. Ezért végre kell hajtani a megfelelő lépéseket, ha az eszköz hálózati kapcsolat meg vannak lecserélése VIP mivel létrehoz egy ismétlődő IP-helyzet.

A következő lépésekkel felcserélése vagy rendelése az összes hálózati kapcsolatok VIP:

#### <a name="to-reassign-ips"></a>Ha átadni IP-címei

1. Törölje a jelet a mindkét felületek IP-címe.

2. Miután az IP-címek nincs bejelölve, az új IP-címek hozzárendelése a megfelelő felületek.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [a StorSimple eszköz MPIO](storsimple-configure-mpio-windows-server.md)konfigurálása.

- További tudnivalók [a StorSimple kezelő szolgáltatás StorSimple eszköze való](storsimple-manager-service-administration.md)használatáról.
     
