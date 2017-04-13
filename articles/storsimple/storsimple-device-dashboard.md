<properties
   pageTitle="Használja a StorSimple-kezelő eszköz irányítópult |} Microsoft Azure"
   description="A StorSimple kezelő szolgáltatás eszköz irányítópult és megtekintheti a tárolási mértékek és a csatlakoztatott kezdeményezők, és keresse meg a sorszám és IQN használatához ismerteti."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>A StorSimple-kezelő eszköz irányítópultok használata

## <a name="overview"></a>– Áttekintés

A StorSimple-kezelő eszköz irányítópult áttekintést egy adott StorSimple eszközhöz alkalmazással szemben a szolgáltatás irányítópulton információkkal szolgál a Microsoft Azure StorSimple megoldásban eszközök összes adatát.

![Eszköz irányítópult lapja](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Az irányítópult a következő információkat tartalmazza:

- **A diagramterület** – megtekintheti, hogy a megfelelő tárolási mértékek, a diagram területen kattintson az irányítópult tetején. Ez a diagram esetében a teljes elsődleges (a hosts eszközére által írt adatok mennyiségét) és a teljes felhő tárolására az eszköz egy időszakra vetített állapotával felhasznált mértékek tekinthet meg.

     Ebben a környezetben *elsődleges tároló* az állomás írt adatok mennyiségét hivatkozik, és mennyiségi típus szerint bonthatók: *elsődleges többszintű tároló* magában foglalja mind a helyben tárolt adatok és a felhőbe; többszintű adatok csak az adatokat tárol *elsődleges helyileg kiemelt tároló* tartalmazza. *Felhőalapú tárhelyek*– azonban, akkor a felhőben tárolt adatok mennyiségét mérését. Ide tartoznak a többszintű adatok és a biztonsági másolatok. Figyelje meg, hogy a felhőben tárolt adatok deduplicated és tömöríteni, mivel elsődleges tárhely azt jelzi, az adatok deduplicated és tömörített használt tárterület mennyiségét. (A két szám ráta tömörítést arról megszerezni összehasonlíthatja). A mind az elsődleges és a felhőalapú tárhelyek – az összegek a nyomon követés gyakoriság konfigurálnia kell alapján. Például ha úgy dönt, hogy egy egy hét gyakoriság, majd a diagram fog adatok megjelenítése az előző hét naponta.

     A diagram állíthatja be az alábbi képlettel történik:

     - Felhőbeli tárhelyről elfogyasztott mennyiség megjelenítésére, az idő mennyiségét megjelenítéséhez jelölje be a **FELHŐBEN tárolt használja** lehetőséget. A teljes tárterület az állomás írt megjelenítéséhez jelölje be az **Elsődleges TÖBBSZINTŰ tárolt használja** , és **Elsődleges HELYILEG kiemelt tároló használt** beállítások. Az ábrán a két lehetőség ki van jelölve; Emiatt a diagram megjelenítő tároló felhő és a elsődleges tároló. Figyelje meg, hogy az **Elsődleges TÖBBSZINTŰ tároló használt** sor bármelyik frissítés 2 telepítése előtt használt elsődleges tároló képviseli.
     - A diagram jobb felső sarkában a legördülő menü használatával 1 hét napjait, 1 hónap, 3-as havi vagy éves 1 időszakasz egységét. Megjegyzés: a legfelső szintű diagram nem frissítik csak egyszer naponta, és ezért tükrözni fogja az előző napra összegek.

     További tudnivalókért lásd: [az StorSimple eszköz figyelheti a StorSimple Manager szolgáltatással](storsimple-monitor-device.md).

- **Használatát áttekintése** – **Áttekintés használatát** területen megtekintheti használt elsődleges tároló mennyiségét, a kiépített tárhely, a és a maximális tárolókapacitású mobileszközére. A rendelkezésre álló tárhely legnagyobb mennyiségű használatát számok összehasonlításával megjelenik egy pillantással be kell szereznie további tárterületet, de ha. Ne feledje, hogy az Áttekintés 15 percenként frissül, a különbség a frissítési gyakoriság, mert előfordulhat, hogy másik sorszáma látható mint mutatják a diagram területére a fenti naponta frissül, amely a. További tudnivalókért lásd: [az StorSimple eszköz figyelheti a StorSimple Manager szolgáltatással](storsimple-monitor-device.md).


- **Értesítések** – az **értesítések** területen az eszköz riasztásainak áttekintése tartalmazza. Értesítések súlyosságát szerint csoportosított, és a darab hiányzik a riasztások minden súlyosságát szintjén számát. A riasztási súlyosságát kattintva megnyithatja a hatókörű nézetének mutatja, hogy csak az adott súlyosságát szint eszköz típusa riasztások az értesítések fülre.

- **Feladatok** : A **feladatok** területen megtekintheti az így létrejövő legutóbbi feladat tevékenység. Ez lehet meggyőződhessen arról, hogy a rendszer meg a várt módon működik, vagy hagyhatja, hogy, amelyet fel kell vennie javításra. További információt a legutóbb befejezett feladatok megtekintéséhez kattintson a **feladat sikeresen az elmúlt 24 óra**gombra.

- A **fontos** terület jobb oldalán lévő az irányítópulton eszköz modell, sorozatszám, állapot, leírás és a kötet száma például hasznos információt nyújt.

Is feladatátvevő konfigurálása és az eszközhöz irányítópult csatlakoztatott kezdeményezők megtekintése.

Ezen a lapon elvégezhető a gyakori feladatok a következők:

- Csatlakoztatott kezdeményezők megtekintése

- Keresse meg az eszköz hányadik

- Keresse meg az eszköz cél IQN

## <a name="view-connected-initiators"></a>Csatlakoztatott kezdeményezők megtekintése

A **Nézet csatlakoztatott kezdeményezők** hivatkozást követve az eszköz irányítópult **fontos** területén kattintson az eszköz csatlakozó iSCSI kezdeményezők tekintheti meg. Ez az oldal ismerteti, hogy az eszközre sikerült csatlakoztatni a kezdeményezők táblázatos listája. Az egyes a kezdeményező láthatja:

- A iSCSI minősített a nevét (IQN) a a csatlakoztatott kezdeményező.

- Az access-vezérlő rekord (ACR), amely lehetővé teszi, hogy a csatlakoztatott kezdeményező nevét.

- A csatlakoztatott kezdeményező IP-címét.

- A hálózati kapcsolatokat, amely a kezdeményező tároló eszközén csatlakozik. Ezek terjedhet 0 adatokból adatok 5.

- Összes kötet, hogy a csatlakoztatott kezdeményező hozzáférhetnek az aktuális ACR konfiguráció megfelelően.

Ha lásd: a váratlan kezdeményezők ebben a listában, vagy nem jelennek a várt, olvassa el a ACR konfigurációs. 512 kezdeményezők legfeljebb csatlakozhat az eszközön.

## <a name="find-the-device-serial-number"></a>Keresse meg az eszköz hányadik

Eszköz időértékét szükség lehet a Microsoft többutas I/O (MPIO) az eszközön konfigurálásakor. A következő lépésekkel eszköz időértékét kereséséhez.

#### <a name="to-find-the-device-serial-number"></a>Az eszköz hányadik megkeresése

1. Nyissa meg az **eszközök** > **Irányítópult**.

2. Keresse meg az irányítópult jobb oldali ablaktáblában a **fontos** területen.

3. Görgessen le, és keresse meg a soros számot.

## <a name="find-the-device-target-iqn"></a>Keresse meg az eszköz cél IQN

Az eszköz cél IQN szükség lehet a kérdés Handshake Authentication Protocol (CHAP) konfigurálásakor StorSimple eszközén. Hajtsa végre az alábbi lépéseket a eszköz cél IQN kereséséhez.

### <a name="to-find-the-device-target-iqn"></a>Az eszköz cél IQN megkeresése

1. Nyissa meg az **eszközök** > **Irányítópult**.

1. Keresse meg az irányítópult jobb oldali ablaktáblában a **fontos** területen.

1. Görgessen le, és keresse meg a cél IQN.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [StorSimple kezelő szolgáltatás irányítópult](storsimple-service-dashboard.md).
- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
