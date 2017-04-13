<properties
   pageTitle="A StorSimple 8100 eszköz telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként kicsomagolása csatlakoztatási állvány és kábelmodemek StorSimple 8100 eszköze, mielőtt telepítése és konfigurálása a szoftvert."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Kicsomagolása, állvány-csatlakoztatási, és kábelmodemek StorSimple 8100 eszköze

## <a name="overview"></a>– Áttekintés

A Microsoft Azure StorSimple 8100 egy egyetlen ház, állvány csatlakoztatott eszközt. Ebből az oktatóanyagból megtudhatja, hogyan kicsomagolása, állvány-csatlakoztatási, és a StorSimple 8100 kábel hardver beállítása és a StorSimple eszköz telepítése előtt.

## <a name="unpack-your-storsimple-8100-device"></a>Kicsomagolása StorSimple 8100 eszköze

Az alábbi lépésekkel világos, részletes útmutatást ad arról, hogy miként kicsomagolása a StorSimple 8100 tárolóeszközre. Ez az eszköz van teljesült egyetlen mezőben.

### <a name="prepare-to-unpack-your-device"></a>Az eszköz kicsomagolása való felkészülés

Mielőtt kicsomagolása az eszközön, akkor olvassa el az alábbi információkat.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png)![nehéz súly ikon](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Figyelmeztetés!**

1. Győződjön meg arról, hogy rendelkezik-e két személy érhető el, a ház vastagságának kezelése, ha a saját kezűleg is kezelése. A teljesen konfigurált ház is összehasonlítani legfeljebb 32 kg (70 dkg.).
1. A mező elhelyezése egy strukturálatlan, szintű felületen.

Ezután kövesse az alábbi lépéseket az eszköz kicsomagolása.

#### <a name="to-unpack-your-device"></a>Az eszköz kicsomagolása

1. Vizsgálja meg a mezőbe, és crushes, darabra, víz sérülések vagy bármely más egyértelmű sérülések összecsomagolása használatos. Ha a lista vagy a csomagolást szigorúan sérült, ne nyissa meg a mezőben. Adja meg, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) felmérheti, hogy az eszköz állapotban van.

2. A mező kicsomagolása. Az alábbi képen látható az StorSimple eszköz kicsomagolt nézetét.

     ![A tárolóeszközre kicsomagolása](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **A tárolóeszközre kicsomagolt nézete**

     Címke | Leírás
     ----- | -------------
     1     | Csomagolást mezőben
     2     | Alsó hab
     3     | Eszköz
     4     | Felső hab
     5     | Kiegészítő mezőben


3. Miután kicsomagolás a mezőbe, győződjön meg arról, hogy:

   - 1 egyetlen ház eszköz
   - 2 vezeték
   - 1 fordított Ethernet-kábelt
   - 2 a soros konzol kábelek
   - a soros hozzáférés 1 egymás után-USB-konverter
   - 1 védett T10 egy csavarhúzót
   - 4 QSFP-a-adaptert SFP + 10 GbE hálózati kapcsolatok való használatra
   - 1 állvány-csatlakoztatási kit (2 egymás sínek hardver csatlakoztatása)
   - Első lépések dokumentáció

    Ha nem kapott a fenti elemek közül [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md).

A következő lépésként állvány-csatlakoztatása az eszközön.

## <a name="rack-mount-your-storsimple-8100-device"></a>Állvány-csatlakoztatási StorSimple 8100 eszköze

A következő lépésekkel telepíteni a StorSimple 8100 tárolóeszközre, az első és hátsó bejegyzések szabványos 19 hüvelykes állvány. A StorSimple 8100 eszköz egy egyetlen elsődleges ház tartalmaz.

A telepítés, ahol a több lépések, amelyek az alábbi eljárások Budai tárgyalja.

> [AZURE.IMPORTANT]
StorSimple eszközök kell lennie a megfelelő működéshez állvány csatlakoztatva.

### <a name="prepare-the-site"></a>A webhely előkészítése

Az első és hátsó bejegyzések is tartalmazó normál 19 hüvelykes állványon telepíteni kell az eszközt. Az alábbi eljárással állvány telepítési előkészítése.

#### <a name="to-prepare-the-site-for-rack-installation"></a>A webhely Felkészülés állvány telepítése

1. Győződjön meg arról, hogy az eszközön biztonságos azon múlik, egy strukturálatlan, stabil és szintű munka felület (vagy hasonló).

2. Ellenőrizze, hogy a webhelyet, ahol állíthat be kívánja rendelkezik-e a szabványos AC power egy független forrás vagy egy állvány power terjesztési egységgel (PDU) egy szünetmentes ellátási (UPS).

3. Győződjön meg arról, hogy egy 2U tárolóhely érhető el a állvány kitöltési csatlakoztatni az eszközt.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png)![nehéz súly ikon](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Figyelmeztetés!**

Győződjön meg arról, hogy rendelkezik-e két személy kezelése a vonalvastagság, ha az eszköz beállítása saját kezűleg is kezelése érhető el. A teljesen konfigurált ház is összehasonlítani akár 32 kg (70 dkg.).

### <a name="rack-prerequisites"></a>Állvány vonatkozó követelmények

A 8100 ház egy szabványos 19 hüvelykes állványon szekrény a telepítéshez terveztük:

- A bejegyzés való bejegyzés állvány 27.84 hüvelyk minimális mélységét.
- Az eszköz 32 kg maximális vastagságának
- Maximális vissza nyomása 5 Pascal (0,5 mm vízoszlop).

### <a name="rack-mounting-rail-kit"></a>Állvány-csatlakoztatása vasúti kit

Egy sor olyan sínek csatlakoztatása hiányzik a 19 hüvelykes állvány kabinet való használatra. A sínek teszteltük maximális ház vonalvastagság kezelheti. Ezeket a sínek lehetővé teszi a állvány területére adatvesztés nélkül több letölthetés telepítése.


#### <a name="to-install-the-device-on-the-rails"></a>Az eszköz telepítése a sínek

2. Végezze el ezt a lépést, csak akkor, ha a belső sínek nincsenek telepítve az eszközön. A belső sínek általában gyári vannak telepítve. Ha sínek nincs telepítve van, majd telepítse a bal oldali-sín és jobb-sín diákat a ház váz oldalára. Azok csatolása hat metrikus csavarok használatával mindkét oldalán. Tájolással érdekében a vasúti diákat vannak megjelölve, **LH – elülső** és **RH – az első**, és az érdekében, hogy a címkét a ház hátrafelé kúpos.<br/>

    ![Diák vasúti csatolása ház váz](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **Attaching belső vasúti diákon, hogy a diákat, a ház**

       Címke | Leírás
       ----- | -----------
       1     | M 3 x 4-es gomb-vezetője csavarok
       2     | Diák váz

3. A külső bal vasúti és a külső jobb vasúti összeállítások csatolása állvány kabinet függőleges tagok. A szögletes zárójelek **LH**, **RH**, és **Ez az oldal felfelé** végigvezeti Önt a megfelelő tájolás vannak megjelölve.

4. Keresse meg a az első és hátsó az vasúti összeállítás vasúti PIN-kódok. A sín igazítása állvány bejegyzések közé, és a PIN-kódok beillesztése az első és hátsó állvány bejegyzés függőleges tag holes bővítése. Győződjön meg arról, hogy a vasúti összeállítás szint-e.

5. Használja a két a megadott metrikus csavarok biztonságossá tétele a vasúti összeállítás a állvány függőleges tagok. Az első és hátsó egyik egy csavaros használja.

6. Ismételje meg ezeket a lépéseket az vasúti összeállítás.<br/>

     ![Diák vasúti csatolása kabinet állvány](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Külső vasúti összeállítások csatolása a állvány**

     Címke | Leírás
     ----- | -----------
     1     | Csavaros lefogási
     2     | Négyzet-visszajelzést nem adó első állvány bejegyzés csavaros
     3     | Bal oldali vasúti első helyre PIN-kódok
     4     | Csavaros lefogási
     5     | Bal oldali vasúti hátsó hely PIN-kódok


### <a name="mounting-the-device-in-the-rack"></a>Az eszköz a állvány csatlakoztatása

Az imént telepített állvány sínek használ, a következő lépésekkel csatlakoztatni az eszközt a állvány.

#### <a name="to-mount-the-device"></a>Az eszköz csatlakoztatása

1. A asszisztens a ház emelje fel, és igazíthatja a állvány sínek.

2. Gondosan beillesztése az eszköz a sínek, és ezután leküldéses az eszköz teljesen be a állvány kabinet.<br/>

    ![A állvány eszköz beszúrása](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Az eszköz a állvány csatlakoztatása**


3. Távolítsa el a bal és jobb oldali első nyomkarima CAPS LOCK a CAPS LOCK ingyenes adatok. A nyomkarima CAPS LOCK egyszerűen illeszkednek a karimával alakzatot.

5. Biztonságos a ház a állvány az egyes nyomkarima bal és jobb keresztül egy megadott Phillips-vezetője csavaros telepítésével.

4. Telepítse a nyomkarima CAPS LOCK őket egy helyen és a igazítás őket helyen.<br/>

     ![Nyomkarima CAPS LOCK telepítése](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **A nyomkarima CAPS LOCK telepítése**

     Címke | Leírás
     ----- | -----------
     1     | Ház rögzítési csavaros

A következő lépésként az eszközt a power, a hálózati és a soros access kábelmodemek.

## <a name="cable-your-storsimple-8100-device"></a>A StorSimple 8100 kábel eszköz

Az alábbi eljárások power, a hálózati és a soros kapcsolatok StorSimple 8100 eszköze kábelmodemek ismertetik.

### <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené az eszköze kábelek, szüksége lesz:

- A tárolóeszközre teljesen kibontotta és állvány csatlakoztatva.

- az eszközhöz mellékelt 2 kábelek

- 2 Power terjesztési egység (ajánlott) való hozzáférést.

- Hálózati kábel

- Hiányzik a soros kábelek

- A megfelelő illesztőprogram (ha szükséges) a számítógépen telepítve van a soros USB konverter

- 4 QSFP megadott-a-adaptert SFP + 10 GbE hálózati kapcsolatok való használatra

- [Hardveres támogatott StorSimple eszközén 10 GbE hálózati kapcsolat esetén](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>A Power kábelek

Az eszköz a felesleges Power és hűtési modulok (PCMs) tartalmazza. Mindkét PCMs kell telepítenie és más power források magas rendelkezésre állásának csatlakozik.

A következő lépésekkel az eszközt a power kábelmodemek.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Hálózati kábelek

Az eszköz a beállítás egy aktív készenléti: az adott időpontban egy vezérlő modul aktív és feldolgozása az összes lemez és a hálózat művelet, miközben a többi vezérlő modul készenléti. Ha nem sikerül egy vezérlő, a készenléti vezérlő azonnal aktiválva van, és továbbra is a lemez és a hálózati műveletek.

A felesleges vezérlő feladatátvételi támogatásához kell kábelmodemek eszköz hálózatához, az alábbi lépésekkel leírt módon.

#### <a name="to-cable-for-network-connection"></a>A hálózati kapcsolat kábel

1. Az eszköz rendelkezik az egyes vezérlőn hat hálózati kapcsolatok: négy 1 GB/s, és két 10 GB/s Ethernet portok. Azonosítsa az eszköz a hátlap különböző adatok portjához.

    ![Hátlap 8100 eszköz](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Újra az eszközt, rajta az adatok portok**

     Címke   | Leírás
     ------- | -----------
     0,1,4,5 |  1 GbE hálózati kapcsolatok
     2 3     | 10 GbE hálózati kapcsolatok
     6       | A soros portok

2. Lásd: az alábbi ábra a hálózati kábelek. (A minimális hálózati konfigurálásról látható kék folytonos vonal. Magas rendelkezésre állásának és a teljesítmény működéséhez szükséges további beállítási látható pontozott vonalak.)


    ![Az 2U eszköz, hálózati kábel](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Hálózati kábelek az eszközön**


  	|Címke | Leírás |
  	|----- | ----------- |
  	| A    | Az Internet-hozzáféréssel rendelkező LAN |
  	| B    | Vezérlő 0 |
  	| C    | PCM 0 |
  	| D    | 1 vezérlő |
  	| E    | PCM 1 |
  	| F, G | Hosts |
  	| 0-5  | Hálózati kapcsolatok |



Az eszköz kábelek, amikor a minimális konfiguráció szükséges:


- Legalább két hálózati kapcsolatok csatlakozik egy, az felhő elérése és egy iSCSI az egyes vezérlőn. Az adatok 0 port automatikusan engedélyezve és beállítva az eszköz a soros konzol keresztül. 0 adatok mellett egy másik adatok port is el kell konfigurálni az Azure klasszikus portálon keresztül. Ebben az esetben a 0 adatainak összekapcsolása port az elsődleges LAN (Internet-hozzáféréssel rendelkező hálózati). Az adatok portokat SAN/iSCSI LAN (virtuális) szakaszában a hálózaton, attól függően, hogy a megfelelő szerepkört kapcsolhat össze.

- Minden egyes vezérlőn azonos felületek ugyanabba a hálózatba, ha a vezérlő feladatátvétel rendelkezésre állásának csatlakozik. Például ha úgy dönt, hogy a csatlakozási adatok 0 és adatok 3-vezérlőket, akkor még nem csatlakozott a megfelelő adatokat 0 és az adatok 3 a többi vezérlő.

Magas rendelkezésre állásának és a teljesítmény szem előtt tartani:


- Ha lehetséges, minden egyes vezérlő konfigurálhatja felhőelérés (1 GbE) a hálózati kapcsolat két és egy másik pár iSCSI (10 GbE ajánlott).

- Ha lehetséges, csatlakoztassa hálózati kapcsolatok minden vezérlő és a kapcsoló hiba ellen rendelkezésre állásának két különböző kapcsolókat. Az ábra a két 10 GbE hálózati felületek, adatok 2 és 3 adatok az egyes vezérlő két különböző kapcsolókat csatlakozik.

További tudnivalókért olvassa el a **hálózati kapcsolatok** [StorSimple eszköze magas elérhetősége követelményei](storsimple-system-requirements.md#high-availability-requirements-for-storsimple)szerint.

>[AZURE.NOTE] Ha a 10 GbE hálózati kapcsolaton SFP + adó használja, használja a megadott QSFP-SFP + adaptereken. További információért lépjen [támogatott hardver StorSimple eszközén 10 GbE hálózati kapcsolat esetén](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>A soros port kábelek

A következő lépésekkel a soros port kábelmodemek.

#### <a name="to-cable-for-serial-connection"></a>A soros kapcsolat kábel

1. Az eszköz minden vezérlő, amely azonosítja a csavarkulcs ikonra a soros port tartalmaz. Keresse meg a [hálózati kábelek](#network-cabling) szakaszban keresse meg az eszköz a hátlap a soros portokat az ábrán.

2. Azonosítsa az eszköz hátlap aktív vezérlőhöz. A villogó kék LED jelzi, hogy a vezérlő aktív.

3. A megadott soros kábelek (ha szükséges, az USB-a soros konverter a hordozható), és csatlakoztassa a konzol vagy számítógép (az eszközre terminálemuláció) a soros porthoz az aktív vezérlő.

4. A soros-USB-illesztőprogramok (az eszközhöz kapott része) telepítése a számítógépen.

5. Állítsa be a soros kapcsolatot az alábbi képlettel történik: 115 200 átviteli, 8 adatok bittel, 1 stop bittel, nincs eltérés áll fenn és folyamatvezérlés nincs értékre van állítva.

6. Ellenőrizze, hogy működik-e a kapcsolatot konzolban, az Enter billentyű megnyomásával. A soros konzol menü jelenjenek meg.

>[AZURE.NOTE] **Lights-Out kezelése**: Ha az eszközre telepítve van egy távoli adatközpontban vagy korlátozott hozzáférésű számítógépen van, ellenőrizze, hogy mindkét vezérlők a soros csatlakozás vannak mindig csatlakoztatva a soros konzol váltás vagy hasonló berendezések. Ha hálózati megszakadása, sem a váratlan hibák távvezérlési sávon, és a támogatási műveletek így.

Az eszköz power, a hálózati hozzáférés és a soros kapcsolatok esetén most már van csatlakoztatva. A következő lépésként konfigurálása a szoftvert, és az eszköz telepítése.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogy miként [telepíthető, és állítsa be a helyszíni StorSimple eszközt](storsimple-deployment-walkthrough.md).
