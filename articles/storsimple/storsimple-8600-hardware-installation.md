<properties
   pageTitle="A StorSimple 8600 eszköz telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként kicsomagolása csatlakoztatási állvány és kábelmodemek StorSimple 8600 eszköze, mielőtt telepítése és konfigurálása a szoftvert."
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
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Kicsomagolása, állvány-csatlakoztatási, és kábelmodemek StorSimple 8600 eszköze

## <a name="overview"></a>– Áttekintés
A Microsoft Azure StorSimple 8600 kettős ház eszköz, és egy elsődleges és EBOD tokba áll. Ebből az oktatóanyagból megtudhatja, hogyan kicsomagolása, állvány-csatlakoztatási, és a StorSimple 8600 kábel hardver előtt állítsa be a StorSimple szoftvert.

## <a name="unpack-your-storsimple-8600-device"></a>Kicsomagolása StorSimple 8600 eszköze

Az alábbi lépésekkel útmutatást világos, részletes útmutatást a StorSimple 8600 tárolóeszközre kicsomagolása. Ez az eszköz van teljesült egyet az elsődleges ház, illetve a EBOD ház egy másik két mezőbe. Ezeket a mezőket két egyetlen keret majd kerül.

### <a name="prepare-to-unpack-your-device"></a>Az eszköz kicsomagolása való felkészülés

Mielőtt kicsomagolása az eszközön, akkor olvassa el az alábbi információkat.


![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png)![nehéz súly ikon](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Figyelmeztetés!**

1. Győződjön meg arról, hogy az eszköz vastagságának kezelhető, ha saját kezűleg is kezelése két személy. A teljesen konfigurált ház is összehasonlítani legfeljebb 32 kg (70 dkg.).
1. A mező elhelyezése egy strukturálatlan, szintű felületen.

Ezután kövesse az alábbi lépéseket az eszköz kicsomagolása.

#### <a name="to-unpack-your-device"></a>Az eszköz kicsomagolása

1. Vizsgálja meg a mezőbe, és crushes, darabra, víz sérülések vagy bármely más egyértelmű sérülések összecsomagolása használatos. Ha a lista vagy a csomagolást szigorúan sérült, ne nyissa meg a mezőben. Adja meg, [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) felmérheti, hogy az eszköz állapotban van.

2. Nyissa meg a külső mezőbe, és hajtsa végre a megfelelő elsődleges és a EBOD letölthetés két doboz. Az elsődleges és a EBOD letölthetés most kicsomagolása. Az alábbi ábrán látható a letölthetés egyik kicsomagolt nézetét.

    ![A tárolóeszközre kicsomagolása](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **A tárolóeszközre kicsomagolt nézete**

     Címke | Leírás
     ----- | -------------
     1     | Csomagolást mezőben
     2     | Társítások kábelek (az accessories és a kábelek tálca)
     3     | Alsó hab
     4     | Eszköz
     5     | Felső hab
     6     | Kiegészítő mezőben

3. Kicsomagolás után győződjön meg arról, hogy a két doboz:

  - 1 elsődleges ház (a elsődleges ház és EBOD ház van két külön mezők)
  - 1 EBOD ház
  - 4 vezeték, a mezőkben 2
  - 2 Társítások kábelek (csatlakozhat az elsődleges ház EBOD ház)
  - 1 fordított Ethernet-kábelt
  - 2 a soros konzol kábelek
  - a soros hozzáférés 1 egymás után-USB-konverter
  - 4 QSFP-a-adaptert SFP + 10 GbE hálózati kapcsolatok való használatra
  - 2 állvány csatlakoztatási Kit (4 egymás sínek csatlakoztatása a hardver 2 egyes az elsődleges ház és EBOD ház), a mezőkben 1
  - Első lépések dokumentáció

    Ha nem kapott a fenti elemek közül [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md).  

A következő lépésként állvány-csatlakoztatása az eszközön.

## <a name="rack-mount-your-storsimple-8600-device"></a>Állvány-csatlakoztatási StorSimple 8600 eszköze

A következő lépésekkel a StorSimple 8600 tárolóeszközre telepítse az első és hátsó bejegyzések szabványos 19 hüvelykes állványon. Az eszközhöz mellékelt két letölthetés: egy elsődleges ház és EBOD tokba. Mindkét kell állvány-csatlakoztatható.

A telepítés, ahol a több lépések, amelyek az alábbi eljárások Budai tárgyalja.

> [AZURE.IMPORTANT]
StorSimple eszközök kell lennie a megfelelő működéshez állvány csatlakoztatva.



### <a name="site-preparation"></a>Webhely előkészítése

A letölthetés telepíteni kell egy szabványos 19 hüvelykes állványon, amely tartalmazza az első és hátsó bejegyzések is. Az alábbi eljárással állvány telepítési előkészítése.

#### <a name="to-prepare-the-site-for-rack-installation"></a>A webhely Felkészülés állvány telepítése

1. Győződjön meg arról, hogy az elsődleges és a EBOD letölthetés pihenő biztonságosan egy strukturálatlan, stabil és szintű munka felületén (vagy hasonló).

2. Ellenőrizze, hogy a webhelyet, ahol állíthat be kívánja rendelkezik-e egy független forrás vagy egy szünetmentes ellátási (UPS) a Power terjesztési egység (PDU) állvány szabványos AC power.

3. Győződjön meg róla, hogy egy 4U (2 X 2U) tárolóhely elérhető a állvány kitöltési a letölthetés csatlakoztatni.

![Figyelmeztető ikon](./media/storsimple-safety/IC740879.png)![nehéz súly ikon](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Figyelmeztetés!**

 Győződjön meg arról, hogy rendelkezik-e két személy kezelése a vonalvastagság, ha az eszköz beállítása saját kezűleg is kezelése érhető el. A teljesen konfigurált ház is összehasonlítani legfeljebb 32 kg (70 dkg.).

### <a name="rack-prerequisites"></a>Állvány vonatkozó követelmények

A letölthetés egy szabványos 19 hüvelykes állványon szekrény a telepítéshez készült:

- A bejegyzés való bejegyzés állvány 27.84 hüvelykes minimális mélység
- Az eszköz 32 kg maximális vastagságának
- Maximális nyomása az 5 Pascal (0,5 mm vízoszlop)

### <a name="rack-mounting-rail-kit"></a>Állvány-csatlakoztatása vasúti kit

Egy sor olyan sínek csatlakoztatása nyújtanak a 19 hüvelykes állvány kabinet való használatra. A sínek teszteltük maximális ház vonalvastagság kezelheti. Ezek a sínek lehetővé teszi terület belül a állvány veszteségmentes több letölthetés telepítése. Először telepítse a EBOD ház.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>A EBOD ház telepítése a sínek

2. Végezze el ezt a lépést, csak akkor, ha a belső sínek nincsenek telepítve az eszközön. A belső sínek általában gyári vannak telepítve. Ha sínek nincs telepítve van, majd telepítse a bal oldali-sín és jobb-sín diákat a ház váz oldalára. Azok csatolása hat metrikus csavarok használatával mindkét oldalán. Tájolással érdekében a vasúti diákat vannak megjelölve, **LH – elülső** és **RH – az első**, és az érdekében, hogy a címkét a ház hátrafelé kúpos.

    ![Diák vasúti csatolása ház váz](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **A partnerek, a ház vasúti diák csatolása**

    Címke | Leírás
    ----- | -----------
    1     | M 3 x 4-es gomb-vezetője csavarok
    2     | Diák váz

3. A bal oldali vasúti és a megfelelő vasúti összeállítások csatolása állvány kabinet függőleges tagok. A szögletes zárójelek **LH**, **RH**, és **Ez az oldal felfelé** végigvezeti Önt a megfelelő tájolás vannak megjelölve.

4. Keresse meg a az első és hátsó az vasúti összeállítás vasúti PIN-kódok. A sín igazítása állvány bejegyzések közé, és a PIN-kódok beillesztése az első és hátsó-állvány bejegyzés függőleges tag holes bővítése. Győződjön meg arról, hogy a vasúti összeállítás szint-e.

5. Biztonságos a vasúti összeállítás a állvány függőleges tagok a metrikus csavarok megadott két használatával. Az első és hátsó egyik egy csavaros használja.

6. Ismételje meg ezeket a lépéseket az vasúti összeállítás.

     ![Diák vasúti csatolása kabinet állvány](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **A állvány vasúti összeállítások csatolása**

     Címke | Leírás
     ----- | -----------
     1     | Csavaros lefogási
     2     | Négyzet-visszajelzést nem adó első állvány bejegyzés csavaros
     3     | Első vasúti hely PIN balra
     4     | Csavaros lefogási
     5     | Bal oldali hátsó vasúti hely PIN-kódok

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>A EBOD ház a állvány a csatlakoztatása

Az imént telepített állvány sínek használ, hajtsa végre az alábbi lépéseket a EBOD ház a állvány a csatlakoztatni.

#### <a name="to-mount-the-ebod-enclosure"></a>Csatlakoztassa a EBOD ház

1. Emelje fel a ház asszisztens, és a állvány sínek igazítása.

2. A ház körültekintően beillesztése a sínek, és ezután leküldéses azt teljesen be a állvány kabinet.

    ![A állvány eszköz beszúrása](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **A ház a állvány a csatlakoztatása**

3. Távolítsa el a bal és jobb oldali első nyomkarima CAPS LOCK a CAPS LOCK ingyenes adatok. A nyomkarima CAPS LOCK egyszerűen illeszkednek a karimával alakzatot.

4. A ház biztonságos a állvány az egyes nyomkarima bal és jobb keresztül egy megadott Phillips-vezetője csavaros telepítésével.

4. Telepítse a nyomkarima CAPS LOCK őket egy helyen és a igazítás őket tetszőleges helyére.

     ![Nyomkarima CAPS LOCK telepítése](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **A nyomkarima CAPS LOCK telepítése**

     Címke | Leírás
     ----- | -----------
     1     | Ház rögzítési csavaros


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Az elsődleges ház a állvány a csatlakoztatása

Miután végzett a EBOD ház csatlakoztatása, szüksége lesz az elsődleges ház ezeket a lépéseket követve csatlakoztatásához.

> [AZURE.NOTE]
>
> - Előfordulhat, hogy az az elsődleges ház és a EBOD ház között a állvány néhány üres oszlop.
> - A megadott 2m Társítások kábel használatával az elsődleges ház csatlakoztatása a EBOD ház.
> - Vannak olyan nincs a központi egység EBOD egységhez relatív helyzetének korlátozó. Az elsődleges ház ezért mentesíthetők a felső tárolóhely és az alábbi EBOD ház – (vagy fordítva).

A következő lépésként az eszközt a power, a hálózati és a soros access kábelmodemek.

## <a name="cable-your-storsimple-8600-device"></a>A StorSimple 8600 kábel eszköz

Az alábbi eljárások power, a hálózati és a soros kapcsolatok StorSimple 8600 eszköze kábelmodemek ismertetik.

### <a name="prerequisites"></a>Előfeltételek

Első lépések az eszköz kábelmodemek, szüksége lesz:

- Az elsődleges ház és a EBOD ház teljesen kibontotta
- 4 kábelek (2 egyes az elsődleges és a EBOD ház) az eszközhöz mellékelt
- Adja meg a EBOD ház csatlakozhat az elsődleges ház eszközzel 2 Társítások kábelek
- Az Access 2 Power terjesztési egységek (PDU) (ajánlott)
- Hálózati kábel
- Hiányzik a soros kábelek
- A megfelelő illesztőprogram (ha szükséges) a számítógépen telepítve van a soros-USB konverter
- 4 QSFP megadott-a-adaptert SFP + 10 GbE hálózati kapcsolatok való használatra
- [Hardveres támogatott StorSimple eszközén 10 GbE hálózati kapcsolat esetén](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>Társítások és Power kábelek

Az eszköz egy elsődleges ház és EBOD tokba is tartalmaz. Ehhez a egységekkel a soros csatolt SCSI (Társítások) kapcsolódási és power közös csatlakoztatva.

Ez az eszköz beállítása első alkalommal, amikor Társítások kábelek először hajtsa végre a lépéseket, és végezze el a power kábelek.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Hálózati kábelek

Az eszköz van egy aktív készenléti konfigurációban: az adott időpontban egy vezérlő modul aktív és feldolgozása az összes lemez és a hálózat művelet, miközben a többi vezérlő modul készenléti. Vezérlő hiba történik, ha a készenléti vezérlő azonnal aktiválja, és továbbra is a lemez és a hálózati műveleteket.

A felesleges vezérlő feladatátvételi támogatásához kell kábelmodemek eszköz hálózatához, ahogy az alábbi lépéseket.

#### <a name="to-cable-for-network-connection"></a>A hálózati kapcsolat kábel

1. Az eszköz rendelkezik az egyes vezérlőn hat hálózati kapcsolatok: négy 1 GB/s és két 10 GB/s Ethernet portok. Olvassa el az alábbi ábrán a hátlap az eszköz adatainak portjához azonosításához.

     ![Hátlap 8600 eszköz](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Vissza az eszközön, rajta az adatok portokat**

     Címke   | Leírás
     ------- | -----------
     0,1,4,5 |  1 GbE hálózati kapcsolatok
     2 3     | 10 GbE hálózati kapcsolatok
     6       | A soros portok



1. Lásd: az alábbi ábra a hálózati kábelek. (A minimális hálózati konfigurálásról látható kék folytonos vonal. Magas rendelkezésre állásának és a teljesítményt, további konfiguráció szükséges látható pontozott vonalak.)

![Az 4U eszköz, hálózati kábel](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Hálózati kábelek az eszközön**

Címke | Leírás
----- | -----------
A    | Az Internet-hozzáféréssel rendelkező LAN
B    | Vezérlő 0
C    | PCM 0
D    | 1 vezérlő
E    | PCM 1
F    | 0 EBOD vezérlő
G    | 1 EBOD vezérlő
H, E  | A Hosts (például fájlkiszolgálókhoz)
0-5  | Hálózati kapcsolatok
6    | Elsődleges ház
7    | EBOD ház

Az eszköz kábelek, amikor a minimális konfiguráció szükséges:


- Legalább két hálózati kapcsolatok csatlakozik egy, az felhő elérése és egy iSCSI az egyes vezérlőn. Az adatok 0 port automatikusan engedélyezve és beállítva az eszköz a soros konzol keresztül. 0 adatok mellett egy másik adatok port is el kell konfigurálni az Azure klasszikus portálon keresztül. Ebben az esetben a 0 adatainak összekapcsolása port az elsődleges LAN (Internet-hozzáféréssel rendelkező hálózati). Az adatok portokat SAN/iSCSI LAN (virtuális) szakaszában a hálózaton, attól függően, hogy a megfelelő szerepkört kapcsolhat össze.

- Minden egyes vezérlőn azonos felületek ugyanabba a hálózatba, ha a vezérlő feladatátvétel rendelkezésre állásának csatlakozik. Például ha úgy dönt, hogy a csatlakozási adatok 0 és adatok 3-vezérlőket, akkor még nem csatlakozott a megfelelő adatokat 0 és az adatok 3 a többi vezérlő.

Magas rendelkezésre állásának és a teljesítmény szem előtt tartani:


- Ha lehetséges, minden egyes vezérlő konfigurálhatja felhőelérés (1 GbE) a hálózati kapcsolat két és egy másik pár iSCSI (10 GbE ajánlott).

- Ha lehetséges, csatlakoztassa hálózati kapcsolatok minden vezérlő és a kapcsoló hiba ellen rendelkezésre állásának két különböző kapcsolókat. Az ábra a két 10 GbE hálózati felületek, adatok 2 és 3 adatok az egyes vezérlő két különböző kapcsolókat csatlakozik. További tudnivalókért olvassa el a **hálózati kapcsolatok** [StorSimple eszköze magas elérhetősége követelményei](storsimple-system-requirements.md#high-availability-requirements-for-storsimple)szerint.

>[AZURE.NOTE] Ha SFP + adó használata 10 GbE hálózati kapcsolaton, használja a megadott QSFP-SFP + adaptereken. További információért lépjen [támogatott hardver StorSimple eszközén 10 GbE hálózati kapcsolat esetén](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>A soros port kábelek

A következő lépésekkel a soros port kábelmodemek.

#### <a name="to-cable-for-serial-connection"></a>A soros kapcsolat kábel

1. Az eszköz minden vezérlő, amely azonosítja a csavarkulcs ikonra a soros port tartalmaz. Keresse meg a soros portok, tanulmányozza az ábra, amely az adatokat portok eszköze hátulján.

2. Azonosítsa az eszköz hátlap aktív vezérlőhöz. A villogó kék LED jelzi, hogy a vezérlő aktív.

3. A megadott soros kábel (ha szükséges, az USB-a soros konverter a hordozható), és csatlakoztassa a konzol vagy számítógép (az eszközre terminálemuláció) a soros porthoz az aktív vezérlő.

4. A soros-USB-illesztőprogramok (az eszközhöz kapott része) telepítése a számítógépen.

5. Állítsa be a soros kapcsolatot az alábbi képlettel történik:
   - 115 200 átviteli
   - 8 adatok bittel
   - 1 stop bittel
   - Nincs eltérés áll fenn
   - Folyamatvezérlés **nincs** értékre van állítva

6. Ellenőrizze, hogy működik-e a kapcsolatot konzolban, az Enter billentyű megnyomásával. A soros konzol menü jelenjenek meg.

> [AZURE.NOTE] **Lights-Out kezelése:** Ha az eszközre telepítve van egy távoli adatközpontban vagy korlátozott hozzáférésű számítógépen van, ellenőrizze, hogy a soros csatlakozás mindkét vezérlők mindig e Váltás a soros konzol vagy hasonló berendezések. Ebben a csoportban adhatja távvezérlési sávon, és a támogatási műveletek esetén a hálózati zavarok és a váratlan hibák.

Az eszköz power, a hálózati hozzáférés és a soros kapcsolat kábelek befejeződött. A következő lépésként állítsa be a szoftver az eszközön.

## <a name="next-steps"></a>Következő lépések

Most már készen áll [telepíthető](storsimple-deployment-walkthrough-u2.md), és állítsa be a helyszíni StorSimple eszközt.
