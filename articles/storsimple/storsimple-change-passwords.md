<properties 
   pageTitle="Változtassa meg a StorSimple jelszavait |} Microsoft Azure" 
   description="Megtudhatja, hogy miként használhatja a StorSimple kezelő szolgáltatás StorSimple pillanatkép kezelő és eszköz rendszergazdai jelszavának módosítása." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>A StorSimple kezelő szolgáltatás használatával StorSimple jelszavának módosítása

## <a name="overview"></a>– Áttekintés 

Az Azure klasszikus portál **beállítása** lapján módosíthatja egy StorSimple kezelő szolgáltatás által kezelt StorSimple eszközön eszköz paramétert tartalmazza. Ebből az oktatóanyagból megtudhatja, hogyan használhatja **a lap** az eszköz rendszergazdáját vagy StorSimple pillanatkép Manager jelszó módosítása.

## <a name="change-the-device-administrator-password"></a>Az eszköz rendszergazdai jelszó módosítása

A Windows PowerShell-felület használatakor az StorSimple eszköz eléréséhez szükségesek írjon be egy eszközt rendszergazdai jelszót. Ha az első StorSimple eszköz regisztrált egy szolgáltatást, az alapértelmezett a kapcsolat jelszava *Jelszo1*. Az adatok biztonsága érdekében szükségesek módosíthatja a jelszavát a regisztrációs folyamat végén. A jelszó módosítása nélkül nem lépjen ki a regisztrációs folyamat. További tudnivalókért lásd: [a 3: konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

A jelszót, amelyet először beállítása óta a Windows PowerShell-felületen regisztráció során módosíthatja az Azure klasszikus portálon keresztül. A következő lépésekkel módosíthatja a eszköz rendszergazdai jelszavát.

#### <a name="to-change-the-device-administrator-password"></a>Az eszköz rendszergazdai jelszó módosítása

1. A klasszikus portálon kattintson az **eszközök** > **konfigurálása** az eszközön.

2. Görgessen lefelé az **Eszköz rendszergazdáját jelszó** szakaszig. Adja meg a 8-ban 15 karaktereket tartalmazó rendszergazdai jelszót. A jelszó 3-as vagy több nagybetűsre, kisbetűsre, numerikus és speciális karakterek számának együttes kell lennie.

3. Erősítse meg a jelszót.

4. A lap alján a **Mentés** gombra.

Az eszköz rendszergazdai jelszó most frissítve lesznek. A módosított jelszó elérése a Windows PowerShell-felület is használhatja.

## <a name="change-the-storsimple-snapshot-manager-password"></a>StorSimple pillanatkép Manager jelszavának módosítása

StorSimple pillanatkép Managert a Windows-szolgáltató a találhatók, és a rendszergazdák kezelése helyi formájában StorSimple eszköze biztonsági másolatait, és a felhő pillanatképek.

A tartalomkeresési eszköz StorSimple pillanatkép Manager, a rendszer kéri, az eszköz IP-cím és a jelszót a tárolóeszközre hitelesítést végezni. Erre a jelszóra van konfigurálva a Windows PowerShell-kapcsolaton keresztül. További tudnivalókért lásd: [a 3: konfigurálása és a Windows Powershellen keresztül az eszköz regisztrálhat StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

A jelszót, amelyet először beállítása óta a Windows PowerShell-felületen regisztráció során módosíthatja a klasszikus portálon keresztül. A következő lépésekkel módosíthatja a StorSimple pillanatkép Manager jelszavát.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>StorSimple pillanatkép Manager jelszavának megváltoztatása

1. A klasszikus portálon kattintson az **eszközök** > **konfigurálása** az eszközön.

2. Görgessen le a **StorSimple pillanatkép Manager** szakasz. Írja be a jelszót, 14 vagy 15 karakter. Győződjön meg arról, hogy a jelszó 3-as vagy több nagybetűsre, kisbetűsre, numerikus és speciális karakterek kombinációját tartalmazza.

3. Erősítse meg a jelszót.

4. A lap alján a **Mentés** gombra.

Most már frissíteni kell a StorSimple pillanatkép Manager jelszavát.
 

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a [StorSimple biztonsági](storsimple-security.md).

- További tudnivalók [a eszköz konfigurációs módosításával](storsimple-modify-device-config.md).

- További tudnivalók [felügyelheti StorSimple eszköze az StorSimple kezelő szolgáltatás használatával](storsimple-manager-service-administration.md).
