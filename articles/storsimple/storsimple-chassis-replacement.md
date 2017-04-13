<properties 
   pageTitle="Cserélje le a váz StorSimple eszközön |} Microsoft Azure"
   description="Távolítsa el, és cserélje le a váz StorSimple elsődleges ház vagy EBOD ház ismerteti."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-chassis-on-your-storsimple-device"></a>A váz StorSimple eszközén cseréje

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan eltávolítása, és cserélje le egy sorozat StorSimple 8000 eszköz váz. A StorSimple 8100 modell (egy váz), egyetlen ház eszközről, mivel a 8600 kettős ház eszköz (két váz). Egy 8600 modell esetleg sikertelen lehet az eszközben két váz sorolhatók: az az elsődleges ház vagy a váz a EBOD ház-váz.

Bármelyik lehetőséget választja a csere váz van szállított Microsoft üres lesz. Nincs Power és hűtési modulok (PCMs), vezérlő modulokat, folytonos állam meghajtók (SSD), a merevlemez-meghajtók (HDDs) vagy a EBOD modulok fognak szerepelni.

>[AZURE.IMPORTANT] Előtt eltávolítása, és a váz cseréjével kapcsolatban, tekintse át a biztonsági [StorSimple hardver összetevő](storsimple-hardware-component-replacement.md)helyett.

## <a name="remove-the-chassis"></a>A váz eltávolítása

Hajtsa végre az alábbi lépések elvégzésével távolítsa el a váz StorSimple eszközén.

#### <a name="to-remove-a-chassis"></a>Ha el szeretne távolítani egy váz

1. Győződjön meg róla, hogy az StorSimple eszköz leállítása és megszakad a kapcsolata, az összes power forrásokból.

2. Távolítsa el a hálózati és Társítások kábelek, ha van ilyen.

3. Távolítsa el a állvány a beállítást.

4. Távolítsa el a meghajtók mindegyike, és jegyezze fel a helyek, amelyről el. További tudnivalókért olvassa el a [a lemezmeghajtó eltávolítása](storsimple-disk-drive-replacement.md#remove-the-disk-drive)című témakört.

5. A EBOD ház (Ha ez a sikertelen váz) távolítsa el a EBOD vezérlő modulokat. További tudnivalókért olvassa el a [egy EBOD vezérlő eltávolítása](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller)című témakört. 

    Kattintson az elsődleges ház (Ha ez a sikertelen váz) távolítsa el a vezérlők, és jegyezze fel a helyek, amelyről el. További tudnivalókért olvassa el a [egy vezérlő eltávolítása](storsimple-controller-replacement.md#remove-a-controller)című témakört.

## <a name="install-the-chassis"></a>A váz telepítése

A következő lépésekkel telepíteni a váz StorSimple eszközén.

#### <a name="to-install-a-chassis"></a>Egy váz telepítése

1. Csatlakoztassa a váz a állvány a. További tudnivalókért lásd: [Állvány-csatlakoztatási StorSimple 8100 eszköze](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) vagy [Állvány-csatlakoztatási StorSimple 8600 eszközére](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Után a váz csatlakoztatva van a állvány a, telepítse őket korábban telepített a helyzetben a vezérlő modulokat.

3. Telepítse a meghajtók az beosztások és az általuk korábban telepített a helyek.

    >[AZURE.NOTE] Azt javasoljuk, hogy először telepítse a SSD a helyek, és végül telepítse a HDDs.

2. A állvány-és telepített összetevőket csatlakoztatott eszközzel a megfelelő power források csatlakoztatni az eszközt, és kapcsolja be az eszközt. A részletekért olvassa [StorSimple 8100 eszköze](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [kábellel StorSimple 8600 eszközére](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).

