<properties 
   pageTitle="Be- és kikapcsolása az StorSimple eszköz |} Microsoft Azure"
   description="Megtudhatja, hogyan kapcsolja be egy új StorSimple eszközt, egy eszközt leállítása vagy elvesznek a power be- és kikapcsolhatja a futó eszközt."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Az StorSimple eszköz be- és kikapcsolása 

## <a name="overview"></a>– Áttekintés

Leáll a Microsoft Azure StorSimple eszköz esetén nem szükséges szokásos működésének részeként. Azonban előfordulhat kapcsolhatja be egy új vagy egy eszköz, amelyen már le kell állítani. Általánosságban elmondható a Leállítás esetekben, amelyben sikertelen hardver cseréje, fizikailag időegység áthelyezése vagy kell vennie egy eszközt szolgáltatás ki van szükség. Ebben az oktatóanyagban bekapcsolása, és leállítja a különböző forgatókönyvekben StorSimple eszköze szükséges eljárását ismerteti.

Az alábbi táblázat felsorolja a különböző forgatókönyvekben bekapcsolása és StorSimple eszköze leállítása, és a megfelelő eljárásban mutató hivatkozásokat tartalmaz.

|Eset|Referenciaanyagokat|
|:-------|:---------------|
|Új eszköz bekapcsolása|[Új eszköz bekapcsolása](#turn-on-a-new-device)<ul><li>[Új eszköz, és csak az elsődleges ház](#new-device-with-primary-enclosure-only)</li><li>[Új eszköz EBOD ház](#new-device-with-ebod-enclosure)</li></ul>|
|Leállítás után egy eszközt bekapcsolása|[Leállítás után egy eszközt bekapcsolása](#turn-on-a-device-after-shutdown)<ul><li>[Csak az elsődleges ház eszköz](#device-with-primary-enclosure-only)</li><li>[Eszköz EBOD ház](#device-with-ebod-enclosure)</li></ul>|
|Egy eszközt bekapcsolása után egy áramszünet|[Egy eszközt bekapcsolása után egy áramszünet](#turn-on-a-device-after-a-power-loss)<ul><li>[Csak az elsődleges ház eszköz](#8100)</li><li>[Eszköz EBOD ház](#8600)</li></ul>|
|Egy eszközt bekapcsolása után az elsődleges ház és EBOD kapcsolat elvész.|[Egy eszközt bekapcsolása után az elsődleges és EBOD ház kapcsolat elvész.](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Állítsa le a eszköz futtatása|[Kapcsolja ki a eszköz futtatása](#turn-off-a-running-device)<ul><li>[Csak az elsődleges ház eszköz](#8100a)</li><li>[Eszköz EBOD ház](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Új eszköz bekapcsolása

A StorSimple eszközön az első alkalommal bekapcsolásának lépései attól függően, hogy az eszköz egy 8100 vagy egy 8600 modellt. A 8100 tartalmaz egy egyetlen elsődleges ház, mivel a 8600 egy elsődleges ház és EBOD tokba kettős-ház eszközt. A lépések részletes mindkét modellek az alábbi szakaszok tartoznak.

- [Új eszköz, és csak az elsődleges ház](#new-device-with-primary-enclosure-only)

- [Új eszköz EBOD ház](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Új eszköz, és csak az elsődleges ház

A StorSimple 8100 modell egy egyetlen ház eszközt. Az eszköz a felesleges Power és hűtési modulok (PCMs) tartalmazza. Mindkét PCMs kell telepítenie és más power források magas rendelkezésre állásának csatlakozik.

A következő lépésekkel az eszközt a power kábelmodemek.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]A teljes eszköz beállítása és kábelek utasításokat lépjen [a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md). Győződjön meg arról, hogy pontosan hajtsa végre az utasításokat.

### <a name="new-device-with-ebod-enclosure"></a>Új eszköz EBOD ház

A StorSimple 8600 modell egy elsődleges ház és EBOD tokba is tartalmaz. Ehhez a egységekkel a soros csatolt SCSI (Társítások) kapcsolódási és power közös csatlakoztatva.

Ez az eszköz beállítása első alkalommal, amikor Társítások kábelek először hajtsa végre a lépéseket, és végezze el a power kábelek.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]A teljes eszköz beállítása és kábelek utasításokat lépjen [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md). Győződjön meg arról, hogy pontosan hajtsa végre az utasításokat.

## <a name="turn-on-a-device-after-shutdown"></a>Leállítás után egy eszközt bekapcsolása

Lépései StorSimple eszközön után le lett állítva eltérőek attól függően, hogy az eszköz egy 8100 vagy egy 8600 modellt. A 8100 tartalmaz egy egyetlen elsődleges ház, mivel a 8600 egy elsődleges ház és EBOD tokba kettős-ház eszközt.

- [Csak az elsődleges ház eszköz](#device-with-primary-enclosure-only)

- [Eszköz EBOD ház](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Csak az elsődleges ház eszköz

A Leállítás, után a következő eljárással be egy elsődleges ház és nincs EBOD ház StorSimple eszközön.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Az elsődleges ház eszköz bekapcsolása

1. Győződjön meg arról, hogy a power – Váltás a mindkét Power és hűtési modulokat (PCMs) ki állásba. Ha a kapcsolót ki állásba nem szerepelnek, fordítsa meg őket ki állásba, és megvárja, amíg a LED válassza a.

2. A power kapcsolók a bekapcsolva állásba mindkét PCMs átállításával kapcsolja be az eszközt. Az eszköz be kell kapcsolnia.

3. Ellenőrizze az alábbiakat, ellenőrizze, hogy az eszköz teljesen meg:

    1. Az OK LED mindkét PCM modulokat a pedig zölddel kiemelve.

    2. Az állapot LED mindkét vezérlők Egyszínű zöld.

    3. A kék LED vezérlők egyik villogó, amely jelzi, hogy a vezérlő aktív.

    Ha e feltételek valamelyike nem teljesül, majd az eszköz egyáltalán nem megfelelő. Adja meg, [lépjen kapcsolatba a Microsoft-támogatást](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Eszköz EBOD ház

A Leállítás, után a következő eljárással be egy elsődleges ház és EBOD tokba StorSimple eszközön. Minden egyes lépés végrehajtása sorrendben pontosan leírt módon. Az adatok elvesztését ezt a hibát okozhat.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Az elsődleges és EBOD tokba eszköz bekapcsolása

1. Győződjön meg arról, hogy a EBOD ház csatlakoztatva van-e az elsődleges ház. További tudnivalókért olvassa el a [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md)című témakört.

2. Győződjön meg arról, hogy a Power és hűtési modulok (PCMs) a EBOD és az elsődleges letölthetés ki állásba. Ha a kapcsolót ki állásba nem szerepelnek, fordítsa meg őket ki állásba, és megvárja, amíg a LED válassza a.

3. Kapcsolja be a EBOD ház első power kapcsolót a bekapcsolva állásba mindkét PCMs átállításával. A PCM LED zöld kell lennie. A kiválasztott egy zöld EBOD vezérlő LED azt jelzi, hogy a EBOD ház meg.

4. Az elsődleges ház kapcsolni a power kapcsolók tükrözése a mindkét PCMs bekapcsolva állásba. A most már a teljes rendszerben.

5. Győződjön meg róla, hogy a Társítások LED zöld, amely biztosítja, hogy a EBOD ház és az elsődleges ház közötti kapcsolat jó.

## <a name="turn-on-a-device-after-a-power-loss"></a>Egy eszközt bekapcsolása után egy áramszünet

Egy áramszünet vagy a munka megszakítását is leállítása egy StorSimple eszközt. A áramszünet akkor fordulhat elő, a power éppen a Kellékek egyik vagy mindkét power éppen a Kellékek. A lépések eltérőek attól függően, hogy az eszköz egy 8100 vagy 8600 modellt. A 8100 tartalmaz egy egyetlen elsődleges ház, mivel a 8600 egy elsődleges ház és EBOD tokba kettős-ház eszközt. Ez a szakasz ismerteti a helyreállítás minden forgatókönyvet.

- [Csak az elsődleges ház eszköz](#8100)

- [Eszköz EBOD ház](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Csak az elsődleges ház eszköz<a name="8100">

A rendszer az egyik saját power éppen a Kellékek áramszünet esetén továbbra is rendes működését. Azonban ahhoz, hogy az eszköz magas elérhetőségét, visszaállíthatja power a power ellátási minél korábban beállítást.

Ha egy áramszünet vagy a mindkét power éppen a Kellékek áramszünet, a rendszer leáll tetszetős és ellenőrzött módon. Amikor helyreáll a teljesítményt, a a rendszer automatikusan bekapcsolása.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Eszköz EBOD ház<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Adja meg egy power Power veszteség

A rendszer az egyik saját power éppen a Kellékek az elsődleges ház vagy a EBOD ház áramszünet esetén továbbra is rendes működését. Azonban ahhoz, hogy az eszköz magas elérhetőségét, állítsa vissza power power nyújtására minél korábban beállítást.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>A két power éppen a Kellékek elsődleges és EBOD letölthetés áramszünet

Ha egy áramszünet vagy a mindkét power éppen a Kellékek áramszünet, a EBOD ház azonnal leáll, és az elsődleges ház tetszetős és ellenőrzött módon leáll. Amikor helyreáll a power, a készülék automatikusan elindul.

Ha a power ki kell kapcsolni saját kezűleg, majd a következő lépésekkel power visszaállításához a rendszer.

1. Kapcsolja be a EBOD ház.

2. Miután a EBOD ház be van kapcsolva, az elsődleges ház bekapcsolása.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>A Power veszteség mindkét power éppen a Kellékek a EBOD ház

A kábelek beállításakor győződjön meg róla, hogy a EBOD soha nem kapcsolódik önállóan egy külön PDU. Ha a EBOD és elsődleges ház nem sikerül egy időben, a rendszer fog visszaállítani.

Ha csak a EBOD ház mindkét power éppen a Kellékek sikertelen, a rendszer nem fogja automatikusan visszaállítani. A következő lépésekkel kapcsolja be a rendszer, és visszaállítja a megfelelő állapotot:

1. Ha az elsődleges ház be van kapcsolva, akkor kapcsolja ki a kiemelt és hűtési modulok (PCMs) is.

2. Várjon néhány percig, állítsa le a rendszer.

3. Kapcsolja be a EBOD ház.

4. Miután a EBOD ház be van kapcsolva, az elsődleges ház bekapcsolása.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Egy eszközt bekapcsolása után az elsődleges és EBOD ház kapcsolat elvész.

Ha megszakad a kapcsolat a készenléti vezérlő és a megfelelő EBOD vezérlő között, az eszköz is működő. A rendszer az aktív vezérlő és a megfelelő EBOD vezérlő közötti kapcsolat elvész, ha feladatátvevő mi történjen, és az eszköz továbbra is működnek a szokásos módon.

Ha mind a soros csatolt SCSI (Társítások) kábelek törlődik, vagy a EBOD ház és az elsődleges ház közötti kapcsolat daraboltak van, az eszköz nem fog működni. Ezen a ponton hajtsa végre az alábbi lépéseket.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Az eszköz bekapcsolása után kapcsolat elvész

1. Hozzáférés a telefon vissza az eszköz.

2. A Társítások a EBOD ház és az elsődleges ház közötti kapcsolat megszakad, ha a minden Társítások sávcímke LED a EBOD ház ki lesz.

3. Állítsa le a Power és hűtési modulok (PCMs) is a EBOD ház és az elsődleges ház.

4. Várja meg, amíg az összes LED-mindkét a letölthetés hátterében kikapcsolása.

5. Szúrja be a Társítások kábelek, és győződjön meg arról, hogy van-e a EBOD ház és az elsődleges ház közötti jó kapcsolat.

6. Kapcsolja be a EBOD ház első mindkét PCM kapcsolók átállításával bekapcsolva állásba.

7. Győződjön meg arról, hogy a EBOD ház annak ellenőrzése, hogy a zöld LED tovább szerint be van kapcsolva.

8. Kapcsolja be az elsődleges ház.

9. Győződjön meg arról, hogy az elsődleges ház annak ellenőrzése, hogy a vezérlő zöld LED tovább szerint be van kapcsolva.

10. Ellenőrizze, hogy az elsődleges ház EBOD ház kapcsolatot jó, jelölje be, hogy a Társítások sáv LED (négy EBOD vezérlő /)-e minden be.

>[AZURE.IMPORTANT] Ha a Társítások kábelek hibás vagy a EBOD ház és az elsődleges ház közötti kapcsolat nem jó, ha bekapcsolja a rendszer, a program elküldi helyreállítási módba. Tájékoztassa a [lépjen kapcsolatba a Microsoft terméktámogatási](storsimple-contact-microsoft-support.md) , ha ez történik.

## <a name="turn-off-a-running-device"></a>Kapcsolja ki a eszköz futtatása

Állítsa le, ha van egy hibás összetevője, hogy ki kell cserélni vagy helyezi át, szolgáltatás, ki vett futó StorSimple eszköz szükséges. A lépések eltérnek attól függően, hogy az StorSimple eszköz egy 8100 vagy egy 8600 modellt. A 8100 tartalmaz egy egyetlen elsődleges ház, mivel a 8600 egy elsődleges ház és EBOD tokba kettős-ház eszközt. Ez a szakasz részletesen a lépéseket követve állítsa le egy futó eszközt.

- [Az elsődleges ház eszköz](#8100a)

- [Eszköz EBOD ház](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Az elsődleges ház eszköz<a name="8100a"> 

Az eszköz leállítása tetszetős és ellenőrzött módon, is elvégezheti az Azure klasszikus portálon keresztül, vagy a Windows PowerShell-StorSimple. 

>[AZURE.IMPORTANT] Leállítása nem futó eszköz az eszköz hátulján power gomb segítségével.
>
>Kilépés az eszközt, előtt ellenőrizze, hogy az eszköz minden összetevő megfelelő. Az Azure klasszikus portálon nyissa meg az **eszközök** > **karbantartási** > **Hardveres állapotát**, és győződjön meg arról, hogy minden összetevő állapota zöld. Ez csak a megfelelő rendszerhez igaz. Ha van Leállítás gombot a rendszer választva egy hibás összetevő cseréje, megjelenik egy sikertelen (piros) vagy a csökkent **Hardveres állapotát**a megfelelő összetevő állapota (sárga).

A Windows PowerShell StorSimple vagy az Azure klasszikus portál nyitja meg, miután kövesse a [Leállítás StorSimple eszközt](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Eszköz EBOD ház<a name="8600a">

>[AZURE.IMPORTANT] Az elsődleges ház és a EBOD ház leállítása előtt biztosítása érdekében, hogy az eszköz minden összetevő megfelelő. Az Azure klasszikus portálon nyissa meg az **eszközök** > **karbantartási** > **Hardveres állapotát**, és győződjön meg róla, hogy minden összetevő megfelelő.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Egy ház EBOD futó eszközt leállítása

1. Kövesse a lépéseket minden [StorSimple eszköz leállítása](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) az az elsődleges ház szerepel.

2. Után az elsődleges ház leállítása, állítsa le a a EBOD átállításával ki Power és hűtési modul (PCM) is kapcsolót.

3. Ha ellenőrizni szeretné, hogy leállt a EBOD, ellenőrizze, hogy a EBOD ház hátulján összes fény kikapcsolása.

>[AZURE.NOTE] A Társítások kábelek a EBOD ház csatlakozhat az elsődleges ház használt nem el kell távolítani addig, amíg a rendszer leállítása után.

## <a name="next-steps"></a>Következő lépések

[Kapcsolatfelvétel a Microsoft támogatási szolgálattal](storsimple-contact-microsoft-support.md) Ha problémát bekapcsolása vagy leállítása egy StorSimple eszközt.

